---
layout: post
title:  '[Lumen 5.8] Hasil return json datetime beda sama di database'
image: '/assets/img/lumen-hasil-return-json-datetime-beda-sama-di-database/debug.png'
date:   2019-08-22 00:05:03
tags:
- PHP
- Laravel
description: 'Hari ini saya lagi ngerjain project di lumen, semacam framework php yang namanya laravel tapi khusus buat api, gak ada view, gak ada library macem2, intinya versi lite dari laravel makanya file size nya lebih kecil dan di reduce sedemikian rupa yang tujuannya emang cuman buat api aja.'
categories:
- My Daily Bug
serie: PHP
---

Hari ini saya lagi ngerjain project di lumen, semacam framework php yang namanya laravel tapi khusus buat api, gak ada view, gak ada library macem2, intinya versi lite dari laravel makanya file size nya lebih kecil dan di reduce sedemikian rupa yang tujuannya emang cuman buat api aja.

Pake lumen kekurangannya kita gak bisa pake beberapa fitur yang biasanya ada di laravel, dalam konteks sekarang adalah config timezone yang biasa ada di config/app.php. Bisa aja sebenernya manual kita create folder config dan file app.php sendiri juga dengan beberapa config lagi, tapi yang paling penting emang langsung dari server kita set timezone supaya ketika kita save ke database field created_at langsung sesuai sama apa yang mau kita setting ke timezone Asia/Jakarta, dari yang defaultnya UTC.

Saya create data user, misal tersimpan di database dengan format dan value kayak gini `2019-08-22 15:21:51`. Tapi anehnya adalah ketika saya return ke json, datanya berubah jadi format TZ tapi gak formatnya aja, jamnya juga nambah sekitar 8 jam dari data di database.

```php
// ambil data created_at di database ceritanya.
$user = User::find(1);

return response()->json([‘created_at’ => $user->created_at], 200);

/*
hasilnya kayak gini
{
  “created_at”: "2019-08-22T08:21:51.000000Z” 
}
*/
```

Kalo di debug returnnya bener sih data dari database, misal jalanin `dd($user->created_at);`

![debug](/assets/img/lumen-hasil-return-json-datetime-beda-sama-di-database/debug.png)

cuman kq kalo di return ke json bisa beda ya, hmmm...

Bingung juga apa yang salah, nyari2 sampe gak tau tiba2 dapet ilham coba ah format TZ saya ganti dengan format `yyyy-mm-dd hh:mm:ss` biar sama kayak di database, carilah saya di google dengan keyword "eloquent created_at response format” nemu deh [stackoverflow ini](https://stackoverflow.com/questions/47977423/laravel-created-at-return-object-in-place-of-date-format-in-database)

Simple tinggal tambahin function `toDateTimeString()` aja di value datetime format TZ dan, jenjreng bener loh sesuai sama database.

```php
// ambil data created_at di database ceritanya.
$user = User::find(1);

return response()->json([
  'created_at' => $user->created_at->toDateTimeString()
], 200);

/*
hasilnya kayak gini
{
  "created_at": "2019-08-22 15:21:51"
}
*/
```

Sampe sekarang juga masih nyari2 sih penyebabnya apa, harusnya sih dan biasanya tinggal ambil aja returnnya pasti sama kayak di database, tapi ini musti di convert lagi pake `toDateTimeString()`, sampe sekarang masih nyari2 penyebabnya apa. Kalo ada temen reader yang tau penyebabnya langsung aja kita diskusi di kolom komentar ya..