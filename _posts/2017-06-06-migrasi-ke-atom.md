---
layout: post
title:  'Migrasi ke Atom'
image: ''
date:   2017-06-06 00:05:03
tags:
- review
description: ''
categories:
- My Daily Posts
serie: review
---

![atom logo](/assets/img/migrasi-ke-atom/atom-logo.jpg)

Sudah hampir 2 tahun ini saya pakai editor yang namanya Sublime versi 3. Sebagai programmer sudah selayaknya setiap hari pasti buka yang namanya editor untuk ngoding di depan layar hitam yang berisi ratusan atau bahkan ribuan line code. Ceritanya kira-kira sebulan lalu saya lagi *surfing* di youtube dan ketemu sama [videonya](https://www.youtube.com/watch?v=ArRtpiU-Mhg&t=5s) mas hilman dari [sekolah koding](https://www.sekolahkoding.com/) yang lagi ngebahas tentang editor atom.

*First impression* saya melihat editor atom dari video tersebut adalah terminal yang sudah build in (bisa di jalankan dengan package) di dalam aplikasi. Memang kendala saya menggunakan sublime adalah saya harus membuka terminal sendiri ketika development aplikasi yang memang harus di akses menggunakan terminal dan itu memang memakan memori(walaupun gak banyak), terlebih karena working directory saya menggunakan permission jadi saya harus mengakses terminal menggunakan `sudo` untuk misalnya membuat folder, file, menggunakan git, dan lain-lain.

Mungkin juga memang sublime menyediakan build in terminal menggunakan package yang di download, saya sendiri kurang tau mengenai hal ini karena keterbatasan ilmu yang saya miliki mengenai sublime. Karena selama ini saya selalu menggunakan sublime dalam keadaan defaultnya. Tetapi ada hal yang paling mendasar dan memotivasi saya untuk migrasi ke editor atom, yaitu **BOSAN**.

Iya bosan adalah yang paling mendasar mengapa saya ingin migrasi ke editor lain. 2 tahun dengan tampilan yang sama membuat mata ini agak jenuh. Dan kadang kita membutuhkan suatu yang baru supaya kita mendapat penyegaran motivasi dan berlama-lama di depan layar hitam yang penuh dengan text ini.

## Besutan Github

Siapa yang tidak memakai github dalam dunia programming, salah satu tempat menyimpan repository code dan media sosial bagi programmer ini merupakan gagasan yang sangat fenomenal dari penemu linux yaitu Linus Torvalds. Dengan mengusung nama github editor ini sangat cepat tenar dan berkembang (mungkin hanya saya satu-satunya programmer yang baru tau text editor ini).

Atom ini di buat menggunakan teknologi web menggunakan [electron](https://electron.atom.io/), dan karena open source sehingga membuat editor ini sangat hackable. Terbukti dari banyaknya jumlah package 'seru' yang bisa kita install dan coba satu persatu.

![atom package](/assets/img/migrasi-ke-atom/atom-package.png)

<p style="text-align: center;"><a href="https://atom.io/packages">daftar package</a> yang ada di atom, saat ini sudah ada 6,282 package</p>

Dari official [website](https://atom.io/) nya hal pertama sebelum saya lihat-lihat dan scroll kebawah adalah [video](https://www.youtube.com/watch?v=Y7aEiVwBAdk) commercialnya yang unik dan bergaya retro (jadul), cukup membuat saya penasaran untuk download dan coba langsung. Setelah saya scroll dan mendapatkan informasi umum mengenai atom dari landing pagenya, gak pake fikir panjang saya langsung download softwarenya dan install.

## Ketak-ketik Ketak-ketik

Setelah saya install di laptop langsung saya buka project saya menggunakan editor ini. Dan langsung saya buat untuk bekerja (waktu itu kebetulan lagi ada dedline kerjaan), kesan pertama yang saya lihat adalah warnanya yang begitu light menurut saya menambah kesan elegan, walaupun mata ini kurang nyaman untuk pertama kali karena bisa di bilang sangat berbeda dari sublime.

Bagi pengguna sublime dan beralih ke atom, sangatlah mudah dan tidak membutuhkan waktu yang lama. Karena interfacenya tidak jauh berbeda yaitu kita bisa melihat tree directory di sebelah kiri dan editor di sebelah kanan. Lalu juga shortcut-shortcut yang di gunakan bisa di bilang hampir sama. Begitu juga dengan fitur text editingnya.

Karena keluaran dari github, di atom terdapat fitur yang menandakan sebuah file berada dalam stagging atau belum atau sudah di commit atau baru saja di commit yang di tandai dengan warna berbeda. Serta informasi kita sedang berada di branch apa.

![atom screenshot](/assets/img/migrasi-ke-atom/atom-ss.png)

<p style="text-align: center;">Tampilan atom ketika saya sedang menulis artikel ini</p>

## Package

Ada banyak package yang tersedia di atom. package pertama yang saya install ke dalam atom saya adalah tentu yang menjadi tujuan saya ketika memakai editor itu yakni Terminal. Di [video](https://www.youtube.com/watch?v=ArRtpiU-Mhg&t=5s) mas hilman, ia mencotohkan memasang package bernama `terminal-plus`. Saya menemukan kendala ketika memasang package ini yaitu tampilan terminal saya menjadi blank hanya ada cursor kelapkelip saja di terminal, setelah googling sana sini akhirnya saya menemukan alternative lain yaitu [platformio-ide-terminal](https://atom.io/packages/platformio-ide). Dan setelah saya install, *viola*! saya berhasil membuka terminal langsung dari atom.

Selain terminal dan package bawaan dari atomnya, saya juga menginstall beberapa package lagi yaitu [minimap](https://atom.io/packages/minimap), [file icons](https://atom.io/packages/file-icons), [emmet](https://atom.io/packages/file-icons). Untuk kegunaan masing-masing silahkan lihat di tautan yang saya berikan.

![atom package installed](/assets/img/migrasi-ke-atom/atom-package-installed.png)

<p style="text-align: center;">Package yang terinstall di atom saya</p>

Selain package kita juga bisa menginstall theme, ada banyak sekali theme yang terdapat di atom, kita bisa melihat-lihat dan mencoba satu-satu dengan menginstall di atom kita, mulai dari font text, warna, syntax, dll. Tetapi saya lebih suka untuk memakainya secara default dengan layar hitam.

Tetapi perlu di perhatikan, jangan terlalu banyak menginstall package di atom kita, karena akan menambah beban memori yang di gunakan atom sehingga membuat atom menjadi lambat atau bahkan not responding tiba-tiba. Maka dari itu bijaklah dalam menggunakan dan menginstall package.

## Lemot

Sudah sebulan saya memakai atom untuk keperluan project saya setiap hari, saya menemukan adanya kekurangan dengan editor ini yaitu, lemot. Sebenarnya kata `lemot` ini agak berlebihan, tetapi satu-satunya yang sering atau sangat sering muncul ketika saya membuat sebuah proses di atom seperti, membuka project baru, menyimpan project ketika project akan di tutup, sering muncul notif bahwa atom not responding dan kita di hadapkan pilihan wait atau close.

Sebenarnya tidak ada masalah jika kita memilih button wait karena atom setelah itu akan berjalan secara normal. Awas jangan salah pilih close, karena bisa jadi code yang kita ketikan akan menjadi hilang, walaupun atom punya fitur untuk membuka project yang terakhir kita kerjakan walaupun belum di sava atau di saat not responding. Tetapi untuk berjaga lebih baik kita 'ber-husnudzon' terhadap atom dan menekan button wait maka semua akan kembali normal.

Ok, sampai sini dulu ya pengalaman migrasi saya dari editor sublime ke atom. Jika teman-teman punya pengalaman serupa atau mau berbagi silahkan ketik di halaman komentar.
