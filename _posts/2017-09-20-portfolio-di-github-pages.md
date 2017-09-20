---
layout: post
title:  "Portfolio di Github Pages"
image: '/assets/img/portfolio-github-pages/portfolio.png'
date:   2017-09-20 00:05:03
tags:
- Life
description: 'Dirasa portfolio menggunakan PDF kurang terlihat profesional walaupun cukup, saya berfikir untuk membuat website portfolio supaya kelihatan lebih profesional.'
categories:
- My Daily Posts
serie: Life
---

![portfolio ekaprasasti](/assets/img/portfolio-github-pages/portfolio.png)

Akhir-akhir ini saya ada target untuk memperbanyak project freelance, salah satu tujuannya adalah selain buat tambahan dapur ngebul hehe, juga untuk memperbanyak portfolio. Sebelumnya portfolio saya di buat menggunakan PDF sederhana lewat software Pages di osx. Kalo temen2 mau liat portfolio saya versi PDF bisa lihat [di sini](bit.ly/EkaPortfolio).

Dirasa portfolio menggunakan PDF kurang terlihat profesional walaupun cukup, saya berfikir untuk membuat website portfolio supaya kelihatan lebih profesional. Di mulai dengan cari2 platform online yang bisa membuat website portfolio gratis, googling dan yang muncul teratas adalah platform dari Adobe yaitu [myportfolio.com](https://myportfolio.com). Hal yang pertama kali dalam melihat sebuah website dan platform online di biasakan untuk melihat kegunaannya apa setelah itu langsung menuju halaman [price](https://www.myportfolio.com/pricing). Dan di sana tertulis bahwa platform ini sudah include dengan photoshop dan kebetulan saya lagi pakai photoshop yang trial 30 hari, dan dengan pedenya saya login pake account adobe dan langsung bikin portfolio lumayan lama kelarnya dan setelah saya publish langsung disuruh bayar. Hahaa, waduh udah lama bikinnya mau di publish malah di suruh pake photoshop yang berbayar. hehee.

Yah mau gak mau harus bikin sendiri deh. Saya bikin cukup dengan halaman statis, dan dengan [template bootstrap](https://startbootstrap.com/template-overviews/freelancer/) yang di edit, karena statis saya pilih Github Pages. Github Pages memungkinkan kita menaruh website statis kita dan langsung bisa di akses lewat domain `username.github.io`. Nah blog saya ini saya buat juga menggunakan jekyll yang di hosting di Github Pages dan saya sudah modifikasi domainnya menjadi [ekaprasasti.com](http://ekaprasasti.com), untuk cara gimana install jekyll di Github Pages mudah2an di artikel selanjutnya saya bisa tulis tutorialnya. 

> Github Pages memungkinkan kita menaruh website statis kita dan langsung bisa di akses lewat domain username.github.io.

Nah masalahnya adalah saya ingin nanti portfolio saya dapat di akses di halaman [ekaprasasti.com/portfolio](http://ekaprasasti.com/portfolio), jadi menggunakan subfolder dari github, nah sempat bingun tuh bagaimana cara. Usut punya usut akhirnya saya menemukan caranya, yaitu kita tinggal buat repository baru contoh kalo saya dengan nama [potfolio](https://github.com/ekaprasasti/portfolio) setelah itu kita buat branch baru bernama `gh-pages` dan di merge dengan branch master. Dan voila!! Website portfolio statis saya sekarang sudah bisa di akses. Jadi dengan branch `gh-pages` kita bisa mengakses website statis kita dengan url `username.github.io/nama-repository`.

Untuk teman2 yang mau lihat2 portfolio saya bisa menuju [ke sini](http://ekaprasasti.com/portfolio) dan pastinya portfolio di sana belum semua saya masukan dan pasti terus saya update. Untuk yang mau kerja bareng boleh juga (nyolong promo freelance). hehee.
