---
layout: post
title:  'Si "Tukang Catat" MorganJS'
image: ''
date:   2017-06-03 00:05:03
tags:
- javascript
- NodeJS
description: ''
categories:
- Programming
serie: javascript
---

Morgan adalah sebuah HTTP request logger middleware untuk NodeJS, yang berfungsi sebagai pencatatan setiap request ke server. Pencatatan ini di sebut dengan istilah Logger yang dapat di akses melalui terminal.

Untuk dokumentasi selengkapnya silahkan anda menuju [github morgan](https://github.com/expressjs/morgan).

### Install Morgan

Untuk menginstall morgan pada sebuah project, buka terminal lalu ketikan command berikut :

```bash
npm install morgan
```

Setelah morgan terinstall selanjutnya load modul morgan pada server file dengan kode berikut ini

```javascript
var morgan = require('morgan');
```

### Setup Middleware

Buat sebuah file dalam directory local anda dengan nama server.js

```bash
touch server.js
```

Selanjutnya kita akan install ExpressJs. Express merupakan framework bagi NodeJS. Ketikan perintah berikut pada terminal anda :

```bash
npm install express --save
```
Setelah expressjs dan morgan di install dalam directory menggunakan npm, saatnya kita mensetup middleware aplikasi kita.

![install modul](/assets/img/tukang-catat-morgan/install-modul.jpg)

Buka server.js dengan editor kesukaan anda. Setelah itu load module expressjs dan morgan dengan mengetikan command berikut :

```javascript
var express = require('express');
var morgan = require('morgan');

var app = express();
```

Buat sebuah middleware morgan dengan sebuah parameter dev, kita akan membahasnya ini nanti.

```javascript
app.use(morgan('dev'));
```

Dan kita tambahkan route untuk root aplikasi kita.

```javascript
app.get('/', function (req, res) {
  res.send('hello, world!')
});
```

Terakhir kita akan membuat server dengan express pada port 3000, tambahkan kode berikut ini

```javascript
// aplikasi berjalan pada port 3000
app.listen(3000);
// buat sebuah notifikasi aplikasi kita sudah dapat di jalankan
console.log("Server running on port 3000");
```

Sehingga code lengkapnya menjadi seperti ini

```javascript
var express = require('express');
var morgan = require('morgan');

var app = express();

app.use(morgan('dev'));

app.get('/', function (req, res) {
  res.send('hello, world!');
});

app.listen(3000);
console.log("Server running on port 3000");
```

Jalankan aplikasi kita dengan command berikut

```bash
node server.js
```

![node-running](/assets/img/tukang-catat-morgan/node-running.jpg)

Akan muncul notifikasi yang sudah kita buat "Server running on port 3000", setelah itu buka browser dan arahkan ke url http://localhost:3000

![browser-open](/assets/img/tukang-catat-morgan/buka-browser.jpg)

Akan muncul hello, world!, yang berasal dari kode berikut `res.send('hello, world!');`

Buka kembali terminal anda

![morgan-result](/assets/img/tukang-catat-morgan/morgan-result.jpg)

Lihat hasil dari setup middleware morgan, ia mencatat semua request dan response apa yang sudah kita lakukan. Terlihat kita mengakses root / dengan status 200 dengan 4.411ms. Juga mengakses favicon.ico, karena favicon.ico belum kita definisikan maka ia akan berstatus 404 atau not found.

### Parameter morgan(format, options)

Morgan logger middleware function di buat menggunakan 2 parameter, yaitu format dan options. Format merupakan string yang sudah di definisikan dan di kenali oleh morgan. Kode berikut merupakan contohnya

```javascript
// EXAMPLE: hanya menampilkan kode yang error
morgan('combined', {
  skip: function (req, res) { return res.statusCode < 400 }
})
```

#### Format

* Combined

Replace middleware `app.use(morgan('dev'));` dan gunakan middleware berikut pada server.js

```javascript
app.use(morgan('combined'));
```

Jalankan server, buka browser dengan port 3000, dan kembali ke terminal untuk melihat catatan dari morgan

![combined](/assets/img/tukang-catat-morgan/combined.jpg)

Seperti yang kita lihat morgan menampilkan catatan yang lebih lengkap dari sebelumnya Standard Apache combined log output, yang berisi dengan tanggal akses, hardware apa server di akses dan juga detail browsernya.

Format output log dari format combined bisa kita lihat seperti ini :

```
:remote-addr - :remote-user [:date[clf]] ":method :url HTTP/:http-version" :status :res[content-length] ":referrer" ":user-agent"
```

* Common

Gunakan middleware berikut pada server.js

```javascript
app.use(morgan('common'));  
```

Jalankan server, buka browser dengan port 3000, dan kembali ke terminal untuk melihat catatan dari morgan

![common](/assets/img/tukang-catat-morgan/common.jpg)

Hasilnya kita ketahui dengan format yang sama dengan combined hanya tidak di sertai informasi user agent.

Format output log dari format common bisa kita lihat seperti ini :

```
:remote-addr - :remote-user [:date[clf]] ":method :url HTTP/:http-version" :status :res[content-length]
```

* dev

Sebelumnya kita sudah menggunakan format dev ini, sekarang saya akan menjelaskannya secara lebih rinci.

Gunakan middleware berikut pada server.js

```javascript
app.use(morgan('dev'));
```

Jalankan server, buka browser dengan port 3000, dan kembali ke terminal untuk melihat catatan dari morgan

![morgan-result](/assets/img/tukang-catat-morgan/morgan-result.jpg)

Catatan di warnai hasil dari status response pada development. Token `:status` akan berwarna merah pada server yang error, kuning pada error di sisi client, biru langit untuk redirect kode, dan tanpa warna untuk kode yang lain.

Format output log dari format dev bisa kita lihat seperti ini

```bash
:method :url :status :response-time ms - :res[content-length]
```

* Short

Gunakan middleware berikut pada server.js

```javascript
app.use(morgan('short'));  
```

Jalankan server, buka browser dengan port 3000, dan kembali ke terminal untuk melihat catatan dari morgan

![short](/assets/img/tukang-catat-morgan/short.jpg)

Lebih sederhana dari combined, dengan menghilangkan waktu dan tanggal akses, tetapi dengan memberi response time.

Format output log dari format short bisa kita lihat seperti ini

```bash
:remote-addr :remote-user :method :url HTTP/:http-version :status :res[content-length] - :response-time ms
```

* Tiny

Gunakan middleware berikut pada server.js

```javascript
app.use(morgan('tiny'));  
```

Jalankan server, buka browser dengan port 3000, dan kembali ke terminal untuk melihat catatan dari morgan

![tiny](/assets/img/tukang-catat-morgan/tiny.jpg)

Lebih sederhana hanya tidak menggunakan warna.

Format output log dari format short bisa kita lihat seperti ini

```bash
:method :url :status :res[content-length] - :response-time ms
```

### Options

Morgan mengenali 3 paramter dalam mendefinisikan middleware nya.

* Immediate

Bari log di tulis atas request. Ini berarti bahwa permintaan akan dicatat bahkan jika server crash, namun data dari response (seperti kode response, content length, dll) tidak dapat login.

* Skip

Function skip di tentukan jika pencatatan di lewati (skip). Options ini di panggil menggunakan skip(req, res).

Contoh dari skip

```javascript
// contoh: hanya menampilkan kode yang error
morgan('combined', {  
  skip: function (req, res) { return res.statusCode < 400 }
})
```

![skip](/assets/img/tukang-catat-morgan/skip.jpg)

Dengan options skip, kita lihat morgan hanya menampilkan request dan response yg error saja atau dengan status 400

* Stream

Berguna jika kita ingin menyimpan log pada file yang telah kita tentukan.

Sebagai contoh kita akan membuat morgan menuliskan catatannya pada file `access.log`.

Ganti kode di server.js dengan kode berikut ini

```javascript
var express = require('express')
var fs = require('fs')
var morgan = require('morgan')
var path = require('path')

var app = express()

// create a write stream (in append mode)
var accessLogStream = fs.createWriteStream(path.join(__dirname, 'access.log'), {flags: 'a'})

// setup the logger
app.use(morgan('combined', {stream: accessLogStream}))

app.get('/', function (req, res) {
  res.send('hello, world!')
})

app.listen(3000);  
console.log("Server running on port 3000");
```

Kita jalankan server dan buka browser dengan port 3000. Saat ini di directory kita terdapat file baru bernama access.log, dan sudah berisi pencatatan dari apa yang sudah kita akses.

### Kesimpulan

Banyak manfaat jika kita menggunakan MorganJS ini, dalam development kita dapat memanfaatkannya untuk optimasi website dengan melihat access time pada terminal atau pun melihat request atau response mana yang error. Pada production kita bisa memanfaatkan untuk merekam IP, hardware, sistem operasi, dan waktu pengunjung yang melihat website kita.

Untuk selengkapnya silahkan pelajari dokumentasi dari [github morgan](https://github.com/expressjs/morgan).

Semua kode dalam artikel ini dapat di akses di [github erabelajar](http://)
