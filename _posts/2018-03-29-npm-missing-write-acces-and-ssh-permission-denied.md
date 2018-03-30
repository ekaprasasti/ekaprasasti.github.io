---
layout: post
title:  "NPM missing write access Elixir Vue 2 & SSH permission denied (public key)"
image: '/assets/img/npm-missing-write-access-and-permission-denied/missing-write-access.png'
date:   2018-03-29 00:05:03
tags:
- Life
description: 'Hari ini saya dapet bug yang menurut saya lumayan aneh, setidaknya ada 2 error yang sederhana (seharusnya juga gak perlu ada) tapi nyarinya lumayan menyita waktu saya siang ini. Oke satu per satu saya bahas.'
categories:
- My Daily Bug
serie: Life
---

Hari ini saya dapet bug yang menurut saya lumayan aneh, setidaknya ada 2 error yang sederhana (seharusnya juga gak perlu ada) tapi nyarinya lumayan menyita waktu saya siang ini. Oke satu per satu saya bahas.


## Problem write access di folder yang tidak ada keberadaannya

Yang pertama ketika saya mau menginstall library sweetalert di project laravel saya, gak tau kenapa tiba2 mucul error. `Missing write access to /path/node_modules/laravel-elixir-vue-2`. 

![Missing write access](/assets/img/npm-missing-write-access-and-permission-denied/missing-write-access.png)

Saya coba pake sudo untuk akses admin, gak bisa juga. Ternyata saya periksa di folder `node_modules` gak ada folder laravel-elixir-vue-2. Loh kok aneh ya! Saya coba pake npm install atau pun sudo npm install, hasilnya sama aja. Wah bahaya masak saya gak bisa akses npm! Sungguh aneh.

Setelah saya ingat-ingat lagi ternyata beberapa waktu lalu saya pernah menginstall library *laravel-elixir-vue2*, dan seingat saya pun sudah saya hapus dengan benar. Gak nyangka di kemudian harinya library yang gak jadi saya pake ini bikin saya pusing. Saya kepikiran hapus folder ini pake `rm` di terminal, agak aneh sih, ngapain gak ada folder nya kok tapi di hapus? yaudah lah apa salahnya saya coba. Daaaannn., jenjreng BERHASIL..!! There’s so weird part dude…!!

![Missing write access](/assets/img/npm-missing-write-access-and-permission-denied/missing-write-access.png)


## Digital Ocean permission denied (public key)

Okeh, lanjut daily error yang ke 2. Macbook saya abis di upgrade ssd, dan di install ulang jadi osx baru lagi (dan saya harus install ulang semua environment di laptop saya, dan gak lama saya baru tau kalo gak perlu install ulang baru dari awal sebenernya cukup cloning aja pake timemachine. Hmm!). Dan otomatis saya harus configurasiin lagi ssh key saya, untuk akses ke server.

Saya pakai droplet di DigitalOcean, saya sudah daftarkan ssh key saya yang baru di akun saya di digitalocean tapi anehnya ketika saya mau masuk ke server lewat ssh muncul error `permission denied (public key)`. Hmm, aneh bin nyata. Oke saya coba reboot server, dan tetap gak berhasil masuk. Saya coba masuk ke console yang di sediakan di digital ocean dan berhasil, saya periksa file `~/.ssh/authorized_keys` dan isinya ternyata beda dengan `id_rsa.pub` saya. Hmm..

Yasudah saya replace saja isinya, tinggal edit. Muncul masalah baru, console dari digitalocean gak ada fitur paste, ya masa saya harus ngetik ssh key saya manual? setelah googling dan paling mendekati referensi di link ini [ini](https://www.digitalocean.com/community/questions/copy-and-paste-into-console) ada yang menyarankan pakai curl, pakai putty, dll. Saya fikir cukup ribet, akhirnya saya pakai cara saya sendiri.

Dengan menaruh file authorized_keys di salah satu project saya lewat git, dan saya git pull di server setelah itu saya move ke folder `~/.ssh/` dan akhirnya Alhamdulillah saya bisa masuk server di terminal kembali dengan ssh.