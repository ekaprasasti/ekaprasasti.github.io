---
layout: post
title:  'PHP: Find & Replace DOCX, convert ke PDF setalahnya'
image: '/assets/img/php-find-replace-docx-convert-pdf/certificate-john-doe.jpg'
date:   2019-05-08 00:05:03
tags:
- PHP
description: ''
categories:
- Programming
serie: PHP
---

![sertificate john doe](/assets/img/php-find-replace-docx-convert-pdf/certificate-john-doe.jpg)

Baru-baru ini saya dapat project dengan fitur cukup unik, saya ambil contoh kasus menggunakan sertifikat. Pada halaman webadmin terdapat fitur untuk mengupload file sertifikat yang di jadikan sebagai template yang mana di dalam file tersebut terdapat konten dengan variabel `[name]` yang secara dinamis dapat berganti-ganti namanya sesuai user yang mendownload sertifikat tersebut dengan output pdf.

Setelah riset, tanya sana sini saya mendapatkan beberapa library seperti phpword, mpdf, fpdf, dan beberapa yang lainnya. Pada awalnya admin menginginkan mengupload template sertifikat dengan format pdf, yang setelah saya riset ternyata cukup sulit untuk membuat fitur find & replace pada file pdf menggunakan php.

![google group forums](/assets/img/php-find-replace-docx-convert-pdf/google-group.png)

<p style="text-align: center;">Begitu kata salah satu forum <a href="https://groups.google.com/forum/#!msg/comp.lang.php/_r-W-SeRSDE/zpyj92cnolIJ" target="_blank">Search and replace text on an existing PDF - Google Groups</a></p>

## Pake Library MPDF (gagal)

Tapi tunggu dulu saya tetep mencari dan menggali karena penasaran masa iya sih gak bisa? dan saya menemukan sebuah library yang bernama [mdpf](https://github.com/mpdf/mpdf) yang menurut saya dari beberapa library yang saya temukan ia yang cukup jelas dan logic bagaimana cara penggunaannya dan mempunyai sebuah fungsi yang bernama [overwrite](https://mpdf.github.io/reference/mpdf-functions/overwrite.html), langsung saja saya terapkan. Saya melakukan percobaan dengan membuat template yang di buat melalui mpdf terlebih dahulu.

```
<?php
    $mpdf = new \Mpdf\Mpdf();
    $mpdf->WriteHTML('<h1>[name]</h1>');
    $mpdf->Output();
```

lalu saya coba find & replace menggunakan fungsi overwrite() dengan kode seperti di bawah ini,

```
<?php
    $mpdf = new \Mpdf\Mpdf();
    $mpdf->percentSubset = 0;
    $search = array(
        '[name]'
    ); // $search merupakan array, yang mana kita bisa replace banyak tag dalam satu konten.
    $replacement = array(
        'john doe'
    );
    $mpdf->OverWrite('/path/template/template.pdf', $search, $replacement, 'I', 'path/result/result.pdf');
```

Dan alhamdulillah berhasil, udah seneng aja tuh saya, langsung saya coba menggunakan template beneran yang di upload sama admin, dan jenjreeengg..!! saya menemukan masalah.

![mpdf error](/assets/img/php-find-replace-docx-convert-pdf/mpdf-error.png)

<p style="text-align: center;"><a href="https://github.com/mpdf/mpdf/issues/626" target="_blank">OverWrite: does not handle CRLF documents correctly</a></p>

intinya dari permasalahan mpdf adalah ia gak detect tag saya `[name]`, saya bingung kenapa padahal kalo create pdfnya dari mpdf bisa tapi kalo dari file lain gak bisa, ternyata karena mpdf gak bisa detect file pdf dengan CRLF di dalamnya, CRLF itu semacam breakline yang ada di dalam konten pdf karena kita convert file docx ke pdf menggunakan thirdparty. Dan dari developer mpdf juga sepertinya sampe saat ini belum ada solusi. Kalo temen-temen punya solusi untuk mpdf nya jangan ragu untuk git pull request ya. hehee.

## Dari file DOCX aja

Akhirnya saya nyerah manipulasi data dari pdf, dan saya langsung cari solusi dengan manipulasi data langsung dari docxnya aja. Alhamdulillah ada library php yang cukup mahsyur yang menghandle operasi2 file word menggunakan php dia adalah [phpword](https://github.com/PHPOffice/PHPWord). Dan saya juga negoisasi dengan admin untuk upload template dengan format docx saja, alhamdulillah di setujui. Heheee!

![doc to pdf](/assets/img/php-find-replace-docx-convert-pdf/doc-pdf.png)

<p style="text-align: center;"><a href="https://www.uhp-software.com/de/blog-artikel/10/how-to-edit-word-document-templates-with-php-and-convert-it-to-pdf" target="_blank">How to edit word document templates with PHP and convert it to PDF?</a></p>

Dan saya dapet blog yang keren banget, yang di blog ini semua masalah saya semua terpecahkan, alhamdulillah. Untuk tutorial lebih jelasnya temen-temen langsung menuju tkp link di atas aja ya, yang saya bahas di sini adalah sebagian besarnya aja.

Pertama kita buat fitur find & replace pada file format docx dulu,

```
<?php
    $template = new \PhpOffice\PhpWord\TemplateProcessor(‘path/template/template.docx’);
    $template->setValue('[name]', 'John Doe');
    $template->saveAs('path/result/result.docx');
```

Dan ketika di buka file result.docx tag `[name]` pun berubah menjadi John Doe. Yeaaayy, alhamdulillah..!!

Dan yang ke dua tahap convert file dari docx ke pdf, dan di blog tersebut disarankan yang menggunakan aplikasi terminal base seperti libreoffice atau unconv, karena kalo convert filenya menggunakan library php seperti mpdf lagi atau yang lainnya hasilnya bisa gak sama dengan file docx aslinya, apalagi kalo file docx nya ada gambar di dalemnya.

Saya memilih libreoffice yang menurut saya dokumentasinya cukup baik. Tinggal buat sebuah syntax shell yang mengeksekusi convert file yang kita mau.

```
<?php
    shell_exec('libreoffice --headless --convert-to pdf /path/to/document/file --outdir /desired/output/directory');
```

Tetapi karena saya pengguna osx untuk development local, cara panggil libreofficenya jadi sedikit berbeda.

```
<?php
    shell_exec('/Applications/LibreOffice.app/Contents/MacOS/soffice --headless --convert-to pdf /path/to/document/file --outdir /desired/output/directory');
```

Nah kira-kira segitu dulu sharing2 saya kali ini. Oh ya kalo temen-temen punya solusi dengan find & replace langsung dari template pdf menggunakan php bisa komen di bawah ya, atau kalo temen-temen punya solusi yang lebih efektif lagi dari cara di atas juga boleh komen. Terimakasih :)
