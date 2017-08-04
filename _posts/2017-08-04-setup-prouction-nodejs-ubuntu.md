---
layout: post
title:  "Setup Production Applikasi NodeJS di Ubuntu Server"
image: '/assets/img/setup-production-nodejs/featured-1.jpg'
date:   2017-08-04 00:05:03
tags:
- Javascript
- NodeJS
- DevOps
description: 'Pada kesempatan kali ini kita akan belajar bagaimana caranya mendeploy aplikasi berbasis NodeJS di server VPS yang kita miliki.'
categories:
- Programming
serie: Javascript
---

![Setup Production Applikasi NodeJS di Ubuntu Server](/assets/img/setup-production-nodejs/featured-1.jpg)

Pada kesempatan kali ini kita akan belajar bagaimana caranya mendeploy aplikasi berbasis NodeJS di server VPS yang kita miliki.

Saat ini di negara tercinta kita Indonesia belum ada hosting yang menyediakan layanannya berbasis NodeJS, sehingga kita harus mensetup secara manual aplikasi kita di VPS (Virtual Private Server).

## Install NodeJS

NodeJS merupakan sebuah environment berbasis javascript yang berjalan di sisi server (server side). Kita semua tau javascript merupakan bahasa pemrograman yang berjalan di sisi client (client side), tetapi menggunakan NodeJS memungkinkan javascript di eksekusi pada sisi server.

* Pertama kita harus login vps kita melalui terminal, ketikan perintah berikut

```bash
ssh root@31.220.111.123
```

Ganti IP 31.220.111.123 dengan IP milik anda, lalu di minta password dan masukan password anda untuk dapat login ke vps anda.

* Update repository anda menggunakan perintah

```bash
apt-get update
```

* Install NodeJS, tambahkan source package nodejs menggunakan perintah berikut pada terminal

```bash
curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
```

Setelah itu jalankan perintah :

```bash
apt-get install nodejs
```

Setelah terinstall, untuk mengecek apakah nodejs sudah terinstall pada ubuntu server kita gunakan perintah :

```bash
node -v
```

![node-version](/assets/img/setup-production-nodejs/node-version.jpg)

Jika sudah terinstall akan muncul versi nodejs yang kita gunakan.

## Setup Node App

Kita akan mencoba aplikasi sederhana menggunakan nodejs. Buat sebuah file dengan nama server.js dan edit menggunakan editor nano dengan mengetikan perintah berikut

````bash
nano server.js
```

* lalu masukan source code berikut untuk membuat server dengan port 8080

````javascript
var http = require('http');
http.createServer(function(req, res) {
   res.writeHead(200, {'Contect-type': 'text/plain'});
   res.end('Hello World\n');
}).listen(8080, '31.220.111.123');
console.log('server running at http://31.220.111.123:8080/');
```

![server.js](/assets/img/setup-production-nodejs/server-js.jpg)

Ganti IP 31.220.111.123 dengan IP Address milik anda

Lalu ketik `ctrl + x` dan `y` untuk menyimpan code yang sudah kita ketikan.

* Jalankan server yang sudah kita buat menggunakan perintah berikut

```bash
node server.js
```

![test-node-app](/assets/img/setup-production-nodejs/test-node-1.jpg)

* Untuk menjalankan aplikasi kita dapat membuka browser dan mengetikan url `http://31.220.111.123:8080/` atau dengan perintah 

````/bash
curl http://31.220.111.123:8080
```

![browser-test](/assets/img/setup-production-nodejs/browser-test.jpg)

![curl-test](/assets/img/setup-production-nodejs/curl-test.jpg)


## Install & Konfigurasi PM2

PM2 merupakan Production Proccess Manager untuk nodejs. Dengan PM2 kita tidak perlu menjalankan manual server menggunakan `node server.js`, PM2 mampu membuatnya otomatis, juga jika terjadi perubahan pada VPS PM2 dapat merestart otomatis server kita.

* Install PM2 pada Ubuntu gunakan perintah berikut

```bash
sudo npm install pm2 -g
```

* Jalankan server menggunakan PM2 dengan mengetikan perintah

```bash
pm2 start server.js
```

* Setelah itu check menggunakan perintah

```bash
pm2 status
```

![pm2-status](/assets/img/setup-production-nodejs/pm2-status.jpg)

## Install Nginx

Nginx menurut mbah [wikipedia](https://id.wikipedia.org/wiki/Nginx) adalah server HTTP dan Proxy dengan kode sumber terbuka yang bisa juga berfungsi sebagai proxy IMAP/POP3.

* Install Nginx pada Ubuntu Server menggunakan perintah berikut 

```bash
sudo apt-get install nginx
```

* Buka file default pada sites-available nginx dengan editor nano 

```bash
sudo nano /etc/nginx/sites-available/default
```

lalu masukan kode berikut ini

```bash
server {
    listen 80;

    server_name erabelajar.com;

    location / {
        proxy_pass http://31.220.111.123:8080;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Ganti erabelajar.com dengan nama domain anda dan 31.220.111.123 dengan IP address anda.

Lalu ketik `ctrl + x` dan `y` untuk menyimpan code yang sudah kita ketikan.

* Restart Nginx dengan mengetikan perintah :

```bash
sudo service nginx restart
```

setelah itu kita akan mendapatkan notikasi bahwa konfigurasi kita sudah benar dan dapat berjalan

```bash
* Restarting nginx nginx
```

Langkah terkahir buka browser dengan mengetikan 
[erabelajar.com](http://erabelajar.com) atau ip address 31.220.111.123 tanpa lagi menggunakan port 8080.

## Kesimpulan
Pada pembahasan kita kali terdapat 5 langkah dan terbagi menjadi 2 yaitu App Server dan Web Server.

### App Server

* Install NodeJS runtime
* Setup Note App
* Install & Konfigurasi PM2

### Web Server

* Install Nginx
* Konfigurasi reverse proxy

Ok, selamat mencoba dan semoga ilmu yang saya bagi di sini bermanfaat. see ya!
