---
layout: post
title:  'NPM Global "Missing write access" Error'
image: ''
date:   2021-01-25 00:00:00
tags:
- NPM
description: 'Hari ini ketika saya menjalankan npm install -g <package> di terminal mac, saya menemukan error'
categories:
- My Daily Bug
---

Hari ini ketika saya menjalankan `npm install -g <package>` di terminal mac, saya menemukan error:

```
Missing write access to /usr/local/lib/node_modules
```

Sehingga saya tidak bisa menginstall library global untuk menjalankan perintah pada command line seperti  `vue create hello-world` atau `react-native init AwesomeProject`.

Untuk mengatasi masalah init kita harus memiliki *write access* pada folder `/usr/local/lib/node_modules`  untuk menginstall library global, maka dari jalankan perintah ini:

```
sudo chown -R $USER /usr/local/lib/node_modules
```

Setelah itu jalankan kembali `npm install -g <package>` dan proses install pun berhasil.

Sekarang kita lihat apa yang sebenarnya sudah kita lakukan:

`sudo` ini artinya kita mengakses command sebagai `root` atau user super admin, yang mana karena kita tidak dapat mengakses folder ini sebagai user biasa untuk mendapatkan *permission* kita akan berlaku sebagai user `root` . Dan perintah ini akan meminta password user pada terminal.

`chown` adalah perintah yang kita gunakan untuk mengganti owner dari permission folder tersebut. Setelah itu kita set dengan `-R` , sehingga kita sebagai owner dapat mengakses seluruh file dan folder yang ada pada folder tersebut.

`$USER` adalah sebuah variable environment yang kita set  sebagai username yang aktiv pada terminal.

Dan terakhir di ikuti dengan `path` folder yang akan kita ganti akses permission nya, sesuai dengan pesan error dari npm.