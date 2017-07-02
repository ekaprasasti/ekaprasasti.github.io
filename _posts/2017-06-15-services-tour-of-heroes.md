---
layout: post
title:  "Tour of Heroes #4: Services"
image: ''
date:   2017-06-15 00:05:03
tags:
- javascript
- Angular
description: ''
categories:
- Programming
serie: javascript
---

Tutorial ini merupakan kelanjutan step by step [aplikasi Tour of Heroes](/angular-tour-of-heroes-tutorial) dari Angular.

Seiring aplikasi Tour of Heroes terus berkembang, kita akan menambahkan lebih banyak component yang memerlukan akses ke data hero.

Dari pada copy dan paste code yang sama terus menerus, kita akan membuat satu data service yang *reusable* dan di *inject* kedalam component yang membutuhkannya. Menggunakan service yang terpisah membuat component lebih ringkas dan fokus kepada view atau tampilan, dan membuat lebih mudah untuk unit-test component dengan *mock* service.

Karena data services selalu menggunakan *asynchronous*, kita akan mengakhiri tutorial pada bagian ini dengan *Promise-based version* pada data service.

Ketika bagian tutorial ini selesai, aplikasi akan terlihat seperti berikut [live example](https://angular.io/generated/live-examples/toh-pt4/eplnkr.html) / [download example](https://angular.io/generated/zips/toh-pt4/toh-pt4.zip).

## Membuat hero service

Saat ini, `AppComponent` mendefinisikan heroes untuk di tampilkan. Namun, mendefinisikan heroes bukan merupakan tugas dari component, dan kita tidak mudah membagikan list heroes dengan component dan view lain. Pada bagian ini, kita akan memindahkan data hero ke satu service yang menyediakan data dan berbagi service tersebut dengan semua component yang membutuhkan data heroes.

Buat sebuah file di dalam folder `app` yang bernama `hero.service.ts`.

> *Naming Convention* pada sebuah file service merupakan nama dari service dengan *lowercase* di ikuti dengan `.service`. Untuk nama service yang mempunyai multi-kata, gunakan *lower dash-case*. Contoh nama file utuk service `SepecialSuperHeroService` adalah `special-super-hero.service.ts`.

Buat class `HeroService` dan export.

#### src/app/hero.service.ts (starting point)
```javascript
import { Injectable } from '@angular/core';

@Injectable()
export class HeroService {
}
```

### Injectable services

Perhatikan kita meng-import function `Injetable` pada Angular dan mengaplikasikan function tersebut dalam decorator `@Injectable()`.

Decorator `@Injectable()` memberitahukan TypeScript untuk membuat metadata tentang sebuah service. Metadata yang menentukan bahwa Angular mungkin perlu *inject* dependency lain kedalam service.

Meskipun `HeroService` saat ini tidak memiliki dependency, menerapkan decorator `@Injectable()` dari awal memastikan konsistensi dan kemudahan di waktu mendatang.

### Mendapatkan data hero

Tambahkan method `getHeroes()` stub.

#### src/app/hero.service.ts (getHeroes stub)
```javascript
@Injectable()
export class HeroService {
  getHeroes(): void {} // stub
}
```

`HeroService` bisa mendapatkan data `Hero` dari mana saja - web service, local storage, atau source data buatan. Membuang akses data dari component artinya kita bisa merubah persepsi tentang *implementation anytime*, tanpa menyentuh components yang membutuhkan data tersebut.

### Pindahkan data hero

Cut array `Heroes` dari `app.component.ts` dan paste di folder baru dalam folder `app` yang di beri nama `mock-heroes.ts`. Sebagai tambahan, copy statement `import {Hero} ...` karena array heroes menggunakan class `Hero`.

#### src/app/mock-heroes.ts
```javascript
import { Hero } from './hero';
 
export const HEROES: Hero[] = [
  { id: 11, name: 'Mr. Nice' },
  { id: 12, name: 'Narco' },
  { id: 13, name: 'Bombasto' },
  { id: 14, name: 'Celeritas' },
  { id: 15, name: 'Magneta' },
  { id: 16, name: 'RubberMan' },
  { id: 17, name: 'Dynama' },
  { id: 18, name: 'Dr IQ' },
  { id: 19, name: 'Magma' },
  { id: 20, name: 'Tornado' }
];
```

Constanta `HEROES` di export sehingga ia bisa di import di luar ini. Di dalam `app.component.ts`, di mana kita meng-cut array `HEROES`, tambahkan sebuah *uninitialized* property `heroes`.

#### src/app/app.component.ts (heroes property)
```javascript
heroes: Hero[];
```

### Return data hero

Kembali ke `HeroService`, import data `HEROES` dan return dari method `getHeroes()`. `HeroService` akan terlihat seperti berikut.

#### src/app/app.component.ts (hero-service-import)
```javascript
import { Injectable } from '@angular/core';

import { Hero } from './hero';
import { HEROES } from './mock-heroes';

@Injectable()
export class HeroService {
  getHeroes(): Hero[] {
    return HEROES;
  }
}
```

### Import hero service

Kita siap menggunakan `HeroService` di component lain, di mulai dari `AppComponent`. Import `HeroService` sehingga kita bisa mereferensikannya di dalam code.

#### src/app/app.component.ts (hero-service-import)
```javascript
import { HeroService } from './hero.service';
```

### Jangan gunakan *new* pada *HeroService*

Bagaimana seharusnya `AppComponent` mendapatkan instance HeroService?

Kita bisa membuat instance baru pada `HeroService` dengan `new` seperti ini.

#### src/app/app.component.ts
``` javascript
heroService = new HeroService(); // don't do this
```

Namun opsi ini tidak ideal dengan beberapa alasan:

- Component harus tahu bagaimana membuat `HeroService`. Jika kita merubah constructor `HeroService`, kita haru menemukan dan memperbaharui setiap tempat kita membuat service. Membuat code di banyak tempat membuat rawan kesalahan dan menambah beban test.

- Kita membuat service setiap kali kita menggunakan `new`. Bagaimana jika service meng-caches heroes dan membagikan *cache* tersebut dengan yang lain? Kita tidak dapat melakukannya.

- Dengan `AppComponent` terkunci di dalam implementasi yang sepesifik dari `HeroService`, berganti implementasi untuk skenario yang berbeda, seperti operasi offline atau menggunakan *mocked* (data buatan) version untuk testing, akan sulit di lakukan.

### Inject *HeroService*

Alih-alih menggunakan baris baru, Anda akan menambahkan dua baris.

- Menambahkan constructor yang juga mendefinisikan private property.

- Menambahkan `providers` metadata pada component.

Tambahkan constructor di dalam class:

#### src/app/app.component.ts (constructor)
```javascript
constructor(private heroService: HeroService) { }
```

Constructor itu sendiri tidak melakukan apapun. Parameter secara simultan mendefiniskan private property `heroService` dan mengidentifikasi sebgai `HeroService` injection site.

Sekarang Angular mengetahui untuk supply sebuah instance dari `HeroService` ketika membuat `AppComponent`.

> Baca lebih jauh mengenai *dependency injection* dalam page [Dependency Injection](https://angular.io/guide/dependency-injection).

*Injector* belum tahu bagaimana cara membuat `HeroService`. Jika kita menjalankan code sekarang, Angular akan mengirimkan error.

```javascript
EXCEPTION: No provider for HeroService! (AppComponent -> HeroService)
```

Untuk mengajarkan injector bagaimana menggunakan `HeroService`, tambahkan property array `providers` di bawah component metadata di dalam `@Component`.

#### src/app/app.component.ts (providers)
```javascript
providers: [HeroService]
```

Array `providers` memberitahukan Angular untuk membuat fresh instance dari `HeroService` saat membuat `AppComponent`. `AppComponent`, serta child component, bisa menggunakan service tersebut untuk mendapatkan data hero.

### *getHeroes()* di dalam *AppComponent*

Service berada di dalam private variabel `heroService`.

Kamu bisa memanggil service dan mengambil data dalam satu baris.

#### src/app/app.component.ts (letakan di dalam class)
```javascript
this.heroes = this.heroService.getHeroes();
```

Kita tidak benar-benar membutuhkan method khusus untuk membungkus satu baris.

#### src/app/app.component.ts (getHeroes)
```javascript
getHeroes(): void {
  this.heroes = this.heroService.getHeroes();
}
```

### The *ngOnInit* lifesycle hook

`AppComponent` harus mengambil dan menampilkan data hero tanpa masalah.

Kita mungkin akan memanggil method `getHeroes()` di dalam constructor, tapi constructor tidak boleh mengandung logic yang rumit, terutama constructor yang memanggil server, seperti method akses data. Constructor adalah di tujukan untuk inisiasi sederhana, seperti menghubungkan parameter constructor ke properties.

Agar Angular dapat memanggil `getHeroes()`, kita bisa mengimplementasikan Angular *ngOnInit lifecycle hook*. Angular menawarkan interface untuk memanfaatkan saat-saat penting dalam component lifecycle: *at creation*, *after each change*, dan *eventual destruction*.

Setiap interface mempunyai satu method. Ketika component mengimplementasikan method, Angular memanggilnya pada waktu yang tepat.

> Pelajari lebih lanjut mengenai *lifecycle hook* pada page [Lifecycle Hooks](https://angular.io/guide/lifecycle-hooks)

Berikut adalah outline `OnInit` interface secara garis besar:

#### src/app/app.component.ts
```javascript
import { OnInit } from '@angular/core';

export class AppComponent implements OnInit {
  ngOnInit(): void {
  }
}
```

Tambahkan implementasi interface `OnInit` pada export statement:

```javascript
export class AppComponent implements OnInit {}
``` 

Tulis method `ngOnInit` dengan menginisiasi logic di dalamnya. Angular akan memanggil pada waktu yang tepat. Pada kasus ini, inisiasi dengan memanggil `getHeroes()`:

#### src/app/app.component.ts (ng-on-init)
```javascript
ngOnInit(): void {
  this.getHeroes();
}
```

Aplikasi harus berjalan sesuai ekspektasi, menampilkan list heroes dan detail hero ketika user mengklik nama hero.

## Async service & Promises

`HeroService` me-return list data dari heroes, `getHeroes()` merupakan synchronous.

#### src/app/app.component.ts
```javascript
this.heroes = this.heroService.getHeroes();
```

Dalam beberapa kasus, data hero datang dari remote server. Ketika menggunakan remote server, user tidak bisa menunggu untuk server melakukan respon. Selain itu, kita tidak bisa mem-block userinterface saat menunggu.

Untuk mengkoordinasikan view dengan respon, kita bisa menggunakan *Promises*, yang merupakan teknik asynchronous yang merubah signature method `getHeroes()`.

### Membuat Promise pada hero service

Promise esensinya adalah *callback* ketika sudah ada *result*/hasil. 

> Ini merupakan penjelasan yang simpel. Pelajari lebih lanjut mengenai ES2015 Promises dalam page [Promises for asynchronous programming](http://exploringjs.com/es6/ch_promises.html)

Update `HeroService` dengan mereturn Promise method `getHeroes()`

#### src/app/hero.service.ts (excerpt)
```javascript
getHeroes(): Promise<Hero[]> {
  return Promise.resolve(HEROES);
}
```

Kita masih memakain data buatan (mocking data). Kita mensimulasikan *behavior* *ultra-fast*, *zero-lantency* server, dengan mereturn Promise yang segera di selesaikan dengan data heroes sebagai hasilnya.

### Act on the Promise

Sebagai hasil dari perubahan `HeroService`, `this.heroes` saat ini menggunakan `Promise` daripada sebuah array dari heroes.

#### src/app/app.component.ts (getHeroes - old)
```javascript
getHeroes(): void {
  this.heroes = this.heroService.getHeroes();
}
```

Kita harus merubah implementasi untuk berlaku sebagai Promise ketika menyelesaikannya.

Lewatkan function callback sebagai argument kepada method Promise `then()`.

#### src/app/app.component.ts (getHeroes - revised)
```javascript
getHeroes(): void {
  this.heroService.getHeroes().then(heroes => this.heroes = heroes);
}
```

Callback mengatur property component `heroes` kepada array heroes dengan mereturn service. Aplikasi tetap berjalan, menampilkan list heroes, dan merespon seleksi nama dengan detail view.

## Review app structure

Setelah kita melakukan refactoring, struktur file dan folder saat ini menjadi seperti berikut

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
| | |
| | -hero.ts
| | |
| | -hero-detail.compponent.ts
| | |
| | -hero.service.ts
| | |
| | -mock-heroes.ts
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

Dan berikut adalah code lengkap yang kita diskusikan pada tutorial ini.

#### src/app/hero.service.ts
```javascript
import { Injectable } from '@angular/core';

import { Hero } from './hero';
import { HEROES } from './mock-heroes';

@Injectable()
export class HeroService {
  getHeroes(): Promise<Hero[]> {
    return Promise.resolve(HEROES);
  }
}
```

#### src/app/app.component.ts
```javascript
{% raw %}
import { Component, OnInit } from '@angular/core';
 
import { Hero } from './hero';
import { HeroService } from './hero.service';
 
@Component({
  selector: 'my-app',
  template: `
    <h1>{{title}}</h1>
    <h2>My Heroes</h2>
    <ul class="heroes">
      <li *ngFor="let hero of heroes"
        [class.selected]="hero === selectedHero"
        (click)="onSelect(hero)">
        <span class="badge">{{hero.id}}</span> {{hero.name}}
      </li>
    </ul>
    <hero-detail [hero]="selectedHero"></hero-detail>
  `,
  styles: [`
    .selected {
      background-color: #CFD8DC !important;
      color: white;
    }
    .heroes {
      margin: 0 0 2em 0;
      list-style-type: none;
      padding: 0;
      width: 15em;
    }
    .heroes li {
      cursor: pointer;
      position: relative;
      left: 0;
      background-color: #EEE;
      margin: .5em;
      padding: .3em 0;
      height: 1.6em;
      border-radius: 4px;
    }
    .heroes li.selected:hover {
      background-color: #BBD8DC !important;
      color: white;
    }
    .heroes li:hover {
      color: #607D8B;
      background-color: #DDD;
      left: .1em;
    }
    .heroes .text {
      position: relative;
      top: -3px;
    }
    .heroes .badge {
      display: inline-block;
      font-size: small;
      color: white;
      padding: 0.8em 0.7em 0 0.7em;
      background-color: #607D8B;
      line-height: 1em;
      position: relative;
      left: -1px;
      top: -4px;
      height: 1.8em;
      margin-right: .8em;
      border-radius: 4px 0 0 4px;
    }
  `],
  providers: [HeroService]
})
export class AppComponent implements OnInit {
  title = 'Tour of Heroes';
  heroes: Hero[];
  selectedHero: Hero;
 
  constructor(private heroService: HeroService) { }
 
  getHeroes(): void {
    this.heroService.getHeroes().then(heroes => this.heroes = heroes);
  }
 
  ngOnInit(): void {
    this.getHeroes();
  }
 
  onSelect(hero: Hero): void {
    this.selectedHero = hero;
  }
}
{% endraw %}
```

#### src/app/mock-heroes.ts
```javascript
import { Hero } from './hero';
 
export const HEROES: Hero[] = [
  { id: 11, name: 'Mr. Nice' },
  { id: 12, name: 'Narco' },
  { id: 13, name: 'Bombasto' },
  { id: 14, name: 'Celeritas' },
  { id: 15, name: 'Magneta' },
  { id: 16, name: 'RubberMan' },
  { id: 17, name: 'Dynama' },
  { id: 18, name: 'Dr IQ' },
  { id: 19, name: 'Magma' },
  { id: 20, name: 'Tornado' }
];
```

## Apa saja yang sudah kita pelajari?

- Kita membuat service class yang bisa di bagikan kepada banyak component.

- Kita menggunakan *lifecycle hook* ngOnInit untuk mengambil data hero ketika `AppComponent` di aktifkan.

- Kita mendefinisikan `HeroService` sebagai provider untuk `AppComponent`.

- Kita membuat data hero buatan (mock) dan meng-import nya ke dalam service.

- Kita mendesign service untuk me-return Promise dan component mengambil data melalui Promise.

## Lampiran: Take it slow

Untuk mensimulasikan slow connection, import `Hero` symbol dan tambahkan method `getHeroesSlowly()` kepada `HeroService`.

#### app/hero.service.ts (getHeroesSlowly)
```javascript
getHeroesSlowly(): Promise<Hero[]> {
  return new Promise(resolve => {
    // Simulate server latency with 2 second delay
    setTimeout(() => resolve(this.getHeroes()), 2000);
  });
}
```

Seperti getHeroes(), code diatas juga mereturn `Promise`. Tapi Promise di sini menunggu 2 detik sebelum menyelesaikan Promise dengan data heroes.

Kembail kedalam `AppComponent`, ganti `getHeroes()` dengan `getHeroesSlowly()`.

## Referensi

- https://angular.io/tutorial/toh-pt4
