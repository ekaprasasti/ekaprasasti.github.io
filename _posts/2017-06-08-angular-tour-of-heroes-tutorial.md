---
layout: post
title:  "Angular: Tour of Heroes tutorial"
image: ''
date:   2017-06-08 00:05:03
tags:
- Javascript
- Angular
description: ''
categories:
- Programming
serie: Javascript
---

Tutorial ini adalah pembelajaran saya mengenai Angular yang bersumber dari [dokumentasi official](https://angular.io/docs/ts/latest/tutorial/) Angular. Dan juga ceritanya sekaligus saya belajar bahasa Inggris, jadi sekalian saya translate dokumentasinya ke bahasa Indonesia.

> The Tour of Heroes tutorial takes you through the steps of creating an Angular application in TypeScript.

Tujuan dari tutorial ini adalah membuat sebuah aplikasi yang dapat menolong staff agensi mengatur *stable of heroes*. Aplikasi ini mencakup *core fundamentals* dari Angular. Kita akan membuat aplikasi dasar dengan banyak fitur yang dapat kita temukan pada aplikasi berbasis data yang lengkap (*data-driven app*): mengambil dan menampilkan data hero, mengedit detail hero yang di pilih, dan navigasi di antara tampilan yang berbeda pada data heroic.

## Apa saja fitur yang akan kita buat?

- Kita akan menggunakan built-in directive untuk memunculkan dan menghilangkan elements dan menampilkan data pada list hero.

- Kita membuat component untuk menampilkan detail hero dan memunculkan sebuah array dari *hero*.

- Kita menggunakan *one-way data binding* hanya untuk membaca data.

- Kita akan menambahkan edit field untuk meng-update sebuah model dengan *two-way data binding*.

- Kita akan *bind* method component pada event user, seperti *keystrokes* dan *clicks*.

- Kita dapat memungkinkan user untuk memilih hero dari master list dan mengeditnya di dalam details view.

- Kita bisa memformat data dengan *pipes*.

- Kita akan membuat sebuah *shared service* untuk mengumpulkan *hero*.

- Kita juga akan membuat routing untuk menavigasi *view* yang berbeda pada setiap component.

Kita cukup belajar inti dari Angular untuk memulai dan setelahnya kita akan dapat melakukan apapun yang kita butuhkan dengan Angular. Aplikasi yang akan kita buat juga dapat kita buka di sini [live example](https://angular.io/resources/live-examples/toh-6/ts/eplnkr.html) / [downloadable example](https://angular.io/resources/zips/toh-6/toh-6.zip).

## The End Game

Bagian ini adalah ide visual yang akan kita buat pada tutorial ini, di mulai dari "Dashboard" view dan hero yang paling heroik.

![Dashboard](https://angular.io/generated/images/guide/toh/heroes-dashboard-1.png)

Kita dapat meng-klik 2 link di atas dashboard yaitu "Dashboard" dan "Heroes" yang merupakan navigasi antara Dashboard view dan Hero view.

Di dalam Dashboard Jika kita klik hero "Magneta", router akan membuka "Hero Details" yang di mana kita dapat meng-edit nama hero.

![hero details](https://angular.io/generated/images/guide/toh/hero-details-1.png)

Klik button "Back" untuk kembali ke Dashboard yang merupakan view utama kita. Jika di klik "Hero", aplikasi akan menampilkan master *hero* list.

![Heroes](https://angular.io/generated/images/guide/toh/heroes-list-2.png)

Jika kita klik nama hero yang berbeda, membuat nama hero maka button di bagian paling bawah menyesuaikan. Kita dapat men-klik "View Details" untuk menuju detail yang dapat kita edit pada hero yang di pilih.

Perhatikan alur dan diagram navigasi yang akan kita buat

![diagram navigations](https://angular.io/generated/images/guide/toh/nav-diagram.png).

## App in Action

![gif action](https://angular.io/generated/images/guide/toh/toh-anim.gif)

Kita akan membuat aplikasi Tour of Heroes, step by step. Setiap langkah memotivasi kita membuat requirement yang mungkin akan banyak kita temui di setiap aplikasi. Klik di setiap step pada daftar berikut ini.

1. [The Hero Editor](/the-hero-editor-tour-of-heroes)

2. [Master Details](/master-detail-tour-of-heroes)

3. [Multiple Component](/multiple-component-tour-of-heroes)

4. [Services](/services-tour-of-heroes)

5. [Routing](/routing-tour-of-heroes)

6. [HTTP](/http-tour-of-heroes)

## Referensi

- https://angular.io/docs/ts/latest/tutorial/
