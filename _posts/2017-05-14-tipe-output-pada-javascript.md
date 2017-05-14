---
layout: post
title:  'Tipe Output pada Javascript'
image: ''
date:   2017-05-14 00:05:03
tags:
- javascript
description: ''
categories:
- Programming
serie: javascript
---

![Keep calm and console log](/assets/img/tipe-output-javascript/keep-calm-and-console-log.jpg)

Pada artikel kali ini saya akan membahas berbagai macam tipe output yang ada pada javascript. Ouput dapat kita gunakan untuk menampilkan sesuatu pada browser atau pada server dengan NodeJS. Selain itu output juga berguna jika kita ingin mendebug atau melihat isi dari variabel atau fungsi-fungsi lain pada program.

Tipe output yang saya bahas di sini ada lima yaitu:

1. **innerHTML**, menuliskan output pada element HTML

2. **document.write**, menuliskan output bersamaan dengan output pada HTML

3. **window.alert()**, menampilkan output pada pop-up alert box javascript

4. **console.log()**, menampilkan output pada browser console

5. **node output**, menampilkan output pada terminal

## innerHTML

Untuk mengakses elemen pada HTML, javascript bisa menggunakan method `document.getElementById(id)`. Argumen `id` mendefinisikan atribut `id` pada elemen HTML. Property `innerHTML` akan merubah konten HTML pada atribut `id` yang dituju, perhatikan contoh berikut.

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Ouput innerHTML</title>
    </head>
    <body>
        <h1>How to output innerHTML?</h1>

        <p id="demo"></p>

        <script>
            document.getElementById('demo').innerHTML = 9 + 8;
        </script>
    </body>
</html>
```

Simpan kode diatas dengan nama file `inner-html.html` dan buka file tersebut dengan browser, lihat apa yang terjadi? tag `<p></p>` terisi 17. Karena tag `<p>` memiliki atribut `id` demo yang sesuai dengan target `document.getElementById('demo')` pada javascript, sehingga HTML menampilkan output hasil dari `9 + 8` dengan memanggil `innerHTML` pada method `document.getElementById('demo')`.

![innerHTML](/assets/img/tipe-output-javascript/inner-html.png)

## document.write()

Untuk tujuan testing, akan lebih mudah menggunakan `document.write()`. Karena ia tidak mengubah elemen apapun pada HTML. Mengeksekusi `document.write()` setelah HTML berhasil di load, maka mengakibatkan content di HTML terhapus pada browser. Perhatikan contoh penggunaan `document.write()` berikut.

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Output document.write()</title>
    </head>
    <body>
        <h1>How to output with document.write()?</h1>

        <script>
            document.write(3 + 4);
        </script>
    </body>
</html>
```

Simpan kode diatas dengan nama `document-write.html` dan buka file tersebut pada browser, yang terlihat adalah setelah tag `<h1>How to output with document.write()?</h1>` akan tercetak 7. Lihat gambar berikut ini.

![document.write()](/assets/img/tipe-output-javascript/document-write.png)

Lalu apa yang terjadi jika kita biarkan HTML di tampilkan terlebih dahulu lalu kita baru mengeksekusi javascript? Perhatikan penggunaan `document.write` yang kedua berikut ini.

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Output document.write()</title>
    </head>
    <body>
        <h1>How to output with document.write()?</h1>

        <button onclick="document.write(5 + 6)">show me</button>
    </body>
</html>
```

Kita jalankan kode di atas pada browser, lalu apa yang terjadi? ketika kita klik button `show me` tiba-tiba content pada HTML `<h1>How to output with document.write()?</h1>` dan button `show me` terhapus. Ini di sebabkan karena `document.write()` di eksekusi setelah content HTML di load.

![document.write()](/assets/img/tipe-output-javascript/document-write-2.png)

## window.alert()

Untuk menampilkan output kita juga bisa menggunakan method `window.alert()` pada javascript, yaitu method yang dapat memunculkan sebuah pop-up alert pada browser. Perhatikan contoh penggunaan `window.alert()` berikut ini.

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Output window.alert()</title>
    </head>
    <body>
        <h1>How to output with window.alert()?</h1>

        <script>
            window.alert(7 + 8);
        </script>
    </body>
</html>
```

Kita simpan kode diatas dengan nama file `window-alert.html`, lalu jalankan pada browser. Maka akan muncul sebuah pop-up dari javascript berisi output dari `7 + 8`.

![window.alert()](/assets/img/tipe-output-javascript/window-alert.png)

## console.log()

Untuk keperluan debugging saya biasa menggunakan method `console.log()` pada javascript, karena method ini tidak seperti method sebelumnya yang mengubah dan memanipulasi kode HTML, method ini manampilkan output pada feature console di browser. Perhatikan kode berikut ini.

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Output console.log()</title>
    </head>
    <body>
        <h1>How to output with console.log()?</h1>

        <script>
            console.log(3 + 2);
        </script>
    </body>
</html>
```

Kita simpan kode di atas dengan nama file `console-log.html`, kita buka file tersebut pada browser. Lalu kita buka feature console pada browser kita, jika pada browser chrome kita dapat membuka console dengan menu `View > Developer > JavaScript Console` atau dengan shortcut keyboard `option + command + J`. Jika kita lihat output dari `3 + 2` dapat terilhat pada console.

![console.log()](/assets/img/tipe-output-javascript/console-log.png)

Debug berfungsi untuk mencari kesalahan dan cacat dalam program, baik itu kesalahan implementasi, design, konfigurasi, sampai kesalahan logika.

## Node Output

Pada [tulisan saya sebelumnya](http://ekaprasasti.com/mengapa-harus-javascript/), sudah kita ketahui bahwa saat ini javascript tidak hanya dapat berjalan pada browser atau sisi client. Tetapi javascript juga dapat berjalan pada sisi server melalui terminal. Lalu bagaimana mengakses javascript dengan terminal? jawabannya adalah dengan NodeJS. Sebelum menjalankan NodeJS kita harus [menginstall NodeJS](https://nodejs.org/en/download/package-manager/) terlebih dahulu.

Buka editor anda, lalu ketikan output javascript `console.log()`, contohnya `console.log(6 + 3)` simpan dengan nama `node-output.js`. Lalu buka terminal atau command prompt anda, arahkan directory pada tempat anda menyimpan file `node-output.js` yang sudah kita simpan. Setalah itu jalankan perintah `node node-output.js`, enter dan lihat hasil outputnya.

![node output](/assets/img/tipe-output-javascript/terminal-node-output.png)

Ok sampai sini dulu pembahasan mengenai metode ouput pada javascript. Jika ada yang ingin di diskusikan silahkan tinggalkan komentar di bawah dan jangan lupa di share.

### Referensi

- [w3school](https://www.w3schools.com/js/js_output.asp)
