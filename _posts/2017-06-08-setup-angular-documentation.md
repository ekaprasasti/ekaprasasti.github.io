---
layout: post
title:  "Setup Angular"
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

Artikel ini adalah pembelajaran saya mengenai Angular yang bersumber dari official dokumentasinya yang bisa di buka [di sini](https://angular.io/docs/ts/latest/guide/setup.html).

Ok, langsung saja setup di sini adalah dengan menggunakan cloning dari repository yang di sediakan angular untuk file basic, bukan dari *create* file di angular cli.

## Setup local development environment

Pada bagian ini kita akan belajar bagaimana membuat angular berjalan pada komputer lokal kita. Setting project baru di lokal komputer sangat lah mudah, coba lihat langkah-langkah berikut (jangan lupa untuk menginstall [node dan npm](https://angular.io/docs/ts/latest/guide/setup.html#install-prerequisites))

1. Buat project folder dengan nama `quickstart`, kita dapat me-*rename* nya nanti.

2. Clone atau download angular dari [repository github](https://github.com/angular/quickstart.git) ke dalam project yang sudah kita buat.

3. Install semua packages dengan npm.

4. Jalankan npm start untuk meruning aplikasi kita.

Berikut langkah-langkahnya jika kita jalankan pada terminal,

```bash
git clone https://github.com/angular/quickstart.git quickstart
cd quickstart
npm install
npm start
```

## Delete non-essential files (optional)

Kita bisa dengan mudah menghapus file-file yang berfokus pada testing dan maintenance repository (termasuk hal yang berhubungan dengan git seperti folder `.git` dan file `.gitigonoe`)

> Lakukan ini hanya pada awalnya saja untuk menghindari secara tidak sengaja menghapus test dan pengaturan git anda sendiri!

Buka terminal dan enter command berikut,

### OS/X (bash)
```bash
xargs rm -rf < non-essential-files.osx.txt
rm src/app/*.spec*.ts
rm non-essential-files.osx.txt
```

### Windows
```
for /f %i in (non-essential-files.txt) do del %i /F /S /Q
rd .git /s /q
rd e2e /s /q
```

## Apa yang kita Clone?

Folder berisi file yang membentuk fondasi yang solid untuk local development. Karena itu, sangat banyak file di dalam folder di *working directory* kita, yang sebagian besar bisa kita [pelajari nanti](https://angular.io/docs/ts/latest/guide/setup-systemjs-anatomy.html).

Fokus pada tiga file TypeScipt (`.ts`) di dalam folder `/src`

### src/app/app.component.ts

```javascript
import { Component } from '@angular/core';

@Component({
  selector: 'my-app',
  template: `<h1>Hello {{name}}</h1>`
})
export class AppComponent { name = 'Angular'; }
```

Mendefinisikan sebuah `AppComponent`, ini adalah **root** component pada sebuah aplikasi

### src/app/app.module.ts

```javascript
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent }  from './app.component';

@NgModule({
  imports:      [ BrowserModule ],
  declarations: [ AppComponent ],
  bootstrap:    [ AppComponent ]
})
export class AppModule { }
```

Mendefinisikan `AppModule`, sebuah [root module](https://angular.io/docs/ts/latest/guide/appmodule.html) memerintahkan Angular bagaimana mengumpulkan semua hal yang di butuhkan dalam sebuah aplikasi. Saat ini kita hanya mendeklarasikan `AppComponent`, setelah ini kita akan banyak mendeklarasikan component lainnya.

### src/main.ts

```javascript
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule }              from './app/app.module';

platformBrowserDynamic().bootstrapModule(AppModule);
```

Mengcompile aplikasi dengan [JIT compiler](https://angular.io/docs/ts/latest/glossary.html#!#jit) (Just-in-time compilation) dan [bootstrap](https://angular.io/docs/ts/latest/guide/appmodule.html#main) `AppModule` untuk menjalankannya pada browser. Kita akan belajar tentang pilihan alternatif compiling dan [deployment](https://angular.io/docs/ts/latest/guide/deployment.html) nanti.

## Referensi

- https://angular.io/docs/ts/latest/guide/setup.html
