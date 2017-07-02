---
layout: post
title:  "Tour of Heroes #1: The Hero Editor"
image: ''
date:   2017-06-08 00:05:03
tags:
- javascript
- Angular
description: ''
categories:
- Programming
serie: javascript
---

Tutorial ini adalah langkah pertama membuat aplikasi [Tour of Heroes](/angular-tour-of-heroes-tutorial). Ikuti langkah-langkah [setup](/setup-angular-documentation/) untuk membuat project baru dengan nama `angular-tour-of-heroes`. Struktur file akan terlihat seperti berikut.

```
angular-tour-of-heroes
|
 -src
| |
| -app
| | |
| | -app.compponent.ts
| | |
| | -app.module.ts
| |
| -main.ts
| |
| -index.html
| |
| -styles.css
| |
| -systemjs.config.js
| |
| -tsconfig.json
|
-node_modules
|
-package.json
```

## Transpiling dan Running app

Buka terminal lalu tulis dan enter command berikut ini:

```bash
npm start
```

Perintah ini akan menjalankan compiler TypeScipt di dalam "Watch mode", dan akan me-recompiling secara automatis ketika kode berubah. Perintah tersebut secara simultan me-launch aplikasi di dalam browser dan me-refresh browser ketika kode berubah.

Kita dapat tetap mengerjakan aplikasi tanpa pause untuk recompile atau refresh browser.

## Show the Heroes

Tambahkan 2 property di dalam `AppComponent`, sebuah property `title` untuk nama aplikasi dan property `hero` dengan nama "Windstorm".

#### app.component.ts (AppComponent class)
```javascript
export class AppComponent {
  title = 'Tour of Heroes';
  hero = 'Windstorm';
}
```

Sekarang update template di dalam `@Component` decorator dengan data binding.

#### app.component.ts (@Component)
```html
{% raw %}
template: `<h1>{{title}}</h1><h2>{{hero}} details!</h2>`
{% endraw %}
```

Refresh browser dan akan menampilkan `title` dan nama `hero`. Tanda double kurung kurawal merupakan Angular sintaks *interpolation binding*.

## Hero Object

Hero membutuhkan lebih banyak property. Convert `hero` dari string ke class. Buat class `hero` dengan property `id` dan `name`. Tambahkan property berikut di bagian atas aplikasi, di atas `@Component`.

#### src/app/app.component.ts (Hero class)
```javascript
export class Hero {
  id: number;
  name: string;
}
```

Di dalam class `AppComponent`, refactor component property `hero` menjadi `Hero` type, lalu inisiasi `id` dengan `1` dan `name` dengan `Windstorm`.

#### src/app/app.component.ts (hero property)
```javascript
hero: Hero = {
  id: 1,
  name: 'Windstorm'
};
```

Karena kita merubah hero dari string ke object, update binding di dalam template yang mengambil property `name` hero.

```html
{% raw %}
template: `<h1>{{title}}</h1><h2>{{hero.name}} details!</h2>`
{% endraw %}
```

## Menambah HTML dengan multi-line template strings

Untuk menampilkan semua property hero, tambahkan `<div>` untuk property `id` pada hero dan `<div>` untuk property `name` pada hero. Untuk menjaga template agar tetap terbaca, letakan `<div>` di setiap line code dengan menggunakan **backticks**.

#### app.component.ts (AppComponent's template)
```html
{% raw %}
template: `
  <h1>{{title}}</h1>
  <h2>{{hero.name}} details!</h2>
  <div><label>id: </label>{{hero.id}}</div>
  <div><label>name: </label>{{hero.name}}</div>
`
{% endraw %}
```

## Edit hero name

User harus dapat men-edit nama hero di dalam `<input>` textbox. Textbox harus dapat menampilkan keduanya, property `name` dan property *update* yang di ketikan user.

Kita membutuhkan *two-way binding* antara element `<input>` form dan property `hero.name`.

### Two-way binding

Refactor nama hero di dalam template sehingga terlihat seperti kode berikut.

```html
<div>
  <label>name: </label>
  <input [(ngModel)]="hero.name" placeholder="name">
</div>
```

`[(ngModel)]` merupakan syntax Angular untuk binding property `hero.name` di dalam textbox.

Aplikasi akan error, dan jika kita lihat dalam browser console, kita akan melihat Angular memberitahukan errornya "`ngModel` ... isn't a known property of `input`."

`NgModel` adalah directive valid dari Angular, dan itu tidak tersedia secara default. Ini termasuk dalam optional module `FormsModule`. Kita harus menambahkan module tersebut secara manual.

### import FormsModule

Buka file `app.module.ts` dan import `FormsModule`. `AppModule` sekarang menjadi terlihat seperti berikut.

```javascript
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule }   from '@angular/forms'; // <-- NgModel lives here
import { AppComponent }  from './app.component';
@NgModule({
  imports: [
    BrowserModule,
    FormsModule // <-- import the FormsModule before binding with [(ngModel)]
  ],
  declarations: [
    AppComponent
  ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }
```

> Read more about `FormsModule` and `ngModel` in the [Two-way data binding with ngModel](https://angular.io/docs/ts/latest/guide/forms.html#ngModel) section of the [Forms](https://angular.io/docs/ts/latest/guide/forms.html) guide and the Two-way binding with NgModel section of the [Template Syntax](https://angular.io/docs/ts/latest/guide/template-syntax.html) guide.

Ketika browser di refresh, app kembali berjalan dengan normal.

## Apa saja yang sudah kita pelajari?

- Menggunakan kurung kurawal ganda (one-way data binding) untuk menampilkan title dan property object `hero`.

- Menulis multi-line template menggunakan ES2015 literal template untuk membuat template mudah di baca.

- Kita menambahkan two-way data binding untuk `<input>` element menggunakan built-in directive `ngModel`.

- Directive ngModel membuat perubahan pada setiap binding lain pada `hero.name`.

Berikut adalah file `app.component.ts` yang lengkap.

```javascript
{% raw %}
import { Component } from '@angular/core';
export class Hero {
  id: number;
  name: string;
}
@Component({
  selector: 'my-app',
  template: `
    <h1>{{title}}</h1>
    <h2>{{hero.name}} details!</h2>
    <div><label>id: </label>{{hero.id}}</div>
    <div>
      <label>name: </label>
      <input [(ngModel)]="hero.name" placeholder="name">
    </div>
    `
})
export class AppComponent {
  title = 'Tour of Heroes';
  hero: Hero = {
    id: 1,
    name: 'Windstorm'
  };
}
{% endraw %}
```

## Referensi

- https://angular.io/docs/ts/latest/tutorial/toh-pt1.html
