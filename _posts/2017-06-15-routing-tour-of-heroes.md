---
layout: post
title:  "Tour of Heroes: Routing"
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

Pada tutorial lanjutan mengenai aplikasi Tour of Heroes ini kita akan menambahkan requirement baru pada aplikas.

- Menambahkan *Dashboard* view.

- Menambahkan kemampuan untuk navigasi antara *Heroes* dan *Dashboard* view.

- Ketika user click nama hero, navigasi ke view detail dari hero yang di pilih.

- Ketika user click link tautan di dalam email, buka detail view untuk hero tertentu.

Ketika selesai nanti, user dapat menavigasi aplikasinya seperti berikut.

![Navigate Tour Of Heroes](https://angular.io/generated/images/guide/toh/nav-diagram.png)

Untuk memenuhi requirement tersebut, kita akan menambahkan Angular route ke dalam aplikasi.

> Untuk informasi lebih lanjut mengenai router, baca halaman [Routing and Navigation](https://angular.io/guide/router)

Hasil dari tutorial ini dapat kita lihat di sini [live example](https://angular.io/generated/live-examples/toh-pt5/eplnkr.html) / [download example](https://angular.io/generated/zips/toh-pt5/toh-pt5.zip)

## Action plan

Berikut adalah plan dari tutorial ini.

- Buat `AppComponent` ke dalam shell aplikasi yang hanya menangani navigasi.

- Relokasi ulang konsentrasi *Heroes* dengan `AppComponent` kepada `HeoresComponent` yang terpisah.

- Tambahkan routing.

- Buat component baru `DashboardComponent`.

- Kaitkan *Dashboard* kedalam struktur navigaasi.

> Routing adalah nama lain dari navigasi. Router adalah mekanisme untuk navigasi dari view ke view.

## Splitting *AppComponent*

Aplikasi kita saat ini me load `AppComponent` dan menampilkan list heroes.

Aplikasi yang di revisi harus menampilkan shell dengan pilihan tampilan (Dashboard dan Heroes) dan kemudian memilih default ke salah satu dari mereka.

`AppComponent` harus hanya bisa menangani navigasi, jadi kita akan memindahkan tampilan Heroes keluar dari `AppComponent` ke dalam componentnya `HeroesComponent.

### *HeroesComponent*

`AppComponent` sudah di dedikasikan untuk `Heroes`. Daripada memindahkan kode dari `AppComponent`, ganti nama menjadi `HeroesComponent` dan buat `AppComponent` shell yang terpisah.

Ikuti langkah berikut:

- Rename file `app.component.ts` dengan `heroes.component.ts`

- Rename class `AppComponent` dengan `HeroesComponent` (*rename locally*, hanya pada file ini).

- Renama selector my-app dnegan `my-heroes`.

#### src/app/heroes.component.ts (showing renamings only)
```javascript
@Component({
	selector: 'my-heroes',
})
export class HeroesComponent implements OnInit {
	
}
```

### Membuat *AppComponent*

`AppComponent` yang baru adalah aplikasi shell. Ia akan mempunya beberapa navigasi link di atas dan menampilkan area di bawahnya.

Lakukan langkah berikut

- Buat file `src/app/app.component.ts`.

- Definisikan export class `AppComponent`.

- Tambahkan decorator `@Component` di atas class dengan selector `my-app`.

- Pindahkan hal berikut dari `HeroesComponent` ke `AppComponent`:
	* class `title` property.
	* `@Component` template `<h1>` element, yang berisi binding `title`.

- Tambahkan element `<my-heroes>` pada template app di bawah heading sehingga kita masih dapat melihat heroes.

- Tambahkan `HeroesComponent` pada array `declarations` dari `AppModule` sehingga Angular mengenali tag `<my-heroes>`.

- Tambahkan `HeroService` pada array `providers` dari `AppModule` karena kita akan membutuhkannya pada setiap view lain.

- Hapus `HeroService` dari `HeroesComponent` array `providers` sejak di promoted.

- Tambahkan dukungan statement `import` untuk `AppComponent`.

Hasilnya akan menjadi seperti ini

#### src/app/app.component.ts (v1)
```javascript
import { Component } from '@angular/core';
 
@Component({
  selector: 'my-app',
  template: `
    <h1>{{title}}</h1>
    <my-heroes></my-heroes>
  `
})
export class AppComponent {
  title = 'Tour of Heroes';
}
```

#### src/app/app.module.ts (v1)
```javascript
import { Component } from '@angular/core';
 
@Component({
  selector: 'my-app',
  template: `
    <h1>{{title}}</h1>
    <my-heroes></my-heroes>
  `
})
export class AppComponent {
  title = 'Tour of Heroes';
}
```

Jalankan aplikasi dan akan menampilkan heroes.

## Menambahkan routing

Angular router merupakan bagian external, optional Angular NgModule disebut `RouterModule`. Router merupakan kombinasi dari beberapa service yang di sediakan (`RouterModule`), beberapa directives (`RouterOutlet`, `RouterLink`, `RouterLinkActive`), dan konfiguras (`Routes`). Kita akan membuat konfigurasi terlebih dahulu.

### <base href>

Buka `index.html` dan pastikan disana terdapat element `<base href="...">` (atau sebuah script yang secara dynamic mengatur element) pada bagian `<head>`.

#### src/index.html (base-href)
```html
<head>
  <base href="/">
```

> Untuk informasi lebih lanjut, lihat sesi [Set the base href](https://angular.io/guide/router) pada halaman [Routing and Navigation](https://angular.io/guide/router)

### Konfigurasi routes

Buat file konfigurasi untuk app routes.

*Routes* memberitahukan router untuk view mana yang akan di tampilkan ketika user klik lick atau paste URL ke dalam address bar browser.

Definisikan route pertama kali sebagai route component heroes.

#### src/app/app.module.ts (heroes route)
```javascript
import { RouterModule }   from '@angular/router';

RouterModule.forRoot([
  {
    path: 'heroes',
    component: HeroesComponent
  }
])
```

`Routes` merupakan array dari *route definitions*.

Mendefinisikan route mampunyai bagian seperti ini:

- *Path*: Router mencocokan *path route* pada URL di dalam address bar browser (`heroes`).

- *Component*: Componentn yang akan router gunakan ketika menuju navigasi route (`HeroesComponent`).

> Baca lebih lanjut mengenai pendefinisian route dengan `Routes` pada halaman [Routing & Navigation](https://angular.io/guide/router).

### Available router

Import `RouteModule` dan tambahkan ke dalam import array `AppModule`.

#### src/app/app.module.ts (app routing)
```javascript
import { NgModule }       from '@angular/core';
import { BrowserModule }  from '@angular/platform-browser';
import { FormsModule }    from '@angular/forms';
import { RouterModule }   from '@angular/router';
 
import { AppComponent }        from './app.component';
import { HeroDetailComponent } from './hero-detail.component';
import { HeroesComponent }     from './heroes.component';
import { HeroService }         from './hero.service';
 
@NgModule({
  imports: [
    BrowserModule,
    FormsModule,
    RouterModule.forRoot([
      {
        path: 'heroes',
        component: HeroesComponent
      }
    ])
  ],
  declarations: [
    AppComponent,
    HeroDetailComponent,
    HeroesComponent
  ],
  providers: [
    HeroService
  ],
  bootstrap: [ AppComponent ]
})
export class AppModule {
}
```

> Method `forRoot()` di panggil karena configurasi router ada pada app root. Method `forRoot()` mensupplay Router service providers dan directives yang di butuhkan oleh routing.

### Router outlet

Jika kita paste path `/heroes`, ke dalam address bar browser di akhir URL, router harus mencocokan route `heroes` dan menampilkan `HeroesComponent`. Namun, kita harus memberitaukan router di mana untuk menampilkan component. Untuk melakukannya, kita akan menambahkan element `<router-outlet>` di akhri template. `RouterOutlet` adalah satu dari directive yang di sediakan oleh `RouterModule`. Router menampilkan setiap component tepat di bawah `<router-outlet>` saat user menavigasi melalui aplikasi.

### Router link

User tidak harus paste route URL pada address bar. Tapi user bisa menambahkan tag anchor pada template, ketika di klik, mentrigger navigasi ke `HeroesComponent`.

Revisi template menjadi seperti ini:

#### src/app/app.component.ts (template-v2)
```javaascript
template: `
   <h1>{{title}}</h1>
   <a routerLink="/heroes">Heroes</a>
   <router-outlet></router-outlet>
 `
```

Refresh browser. Browser akan menampilkan aplikasi dengan link title dan heroes, tapi bukan list heroes.

> Adress bar browser menunjukan `/`. Route path `HeroesComponent` adalah `/heroes`, bukan `/`. Sebentar lagi kita akan menambahkan route yang mencocokan path `/`.

Click link navigasi *Heroes*. Address bar terupdate ke `/heroes` dan list heroes akan di tampilkan. `AppComponent` sekarang terlihat seperti berikut.

#### src/app/app.component.ts (v2)
```javascript
import { Component } from '@angular/core';
 
@Component({
  selector: 'my-app',
  template: `
     <h1>{{title}}</h1>
     <a routerLink="/heroes">Heroes</a>
     <router-outlet></router-outlet>
   `
})
export class AppComponent {
  title = 'Tour of Heroes';
}
```

## Menambahkan dashboard

Routing akan lebih masuk akan dengan multiple view. Untuk menambahkan view, buat `DashboarComponent`, yang mana user juga dapat menavigasinya.

#### src/app/dashboard.component.ts (v1)
```javascript
import { Component } from '@angular/core';

@Component({
  selector: 'my-dashboard',
  template: '<h3>My Dashboard</h3>'
})
export class DashboardComponent { }
```

Kita akan membuat component ini lebih berguna nanti.

### Konfigurasi dashboard route

Untuk mengajarkan `app.module.ts` untuk menavigasi ke dashboard, import component dashboard dan tambahkan definisi route pada array `Routes`.

#### src/app/app.module.ts (Dashboard route)
```javascript
{
  path: 'dashboard',
  component: DashboardComponent
},
```

Juga import dan tambahkan `DashboardComponent` pada `declartions` `AppModule`.

#### src/app/app.module.ts (dashboard)
```javascript
declarations: [
  AppComponent,
  DashboardComponent,
  HeroDetailComponent,
  HeroesComponent
],
```

### Menambahkan redirect route

Saat ini browser berjalan dengan `/` pada address bar. Ketika aplikasi berjalan, seharusnya menunjukan dashboard dan menampilkan URL `/dashboard`  pada address bar browser.

Untuk melakukannya, gunakan redirect route. Tambahkan array route definitions:

#### src/app/app.module.ts (redirect)
```javascirpt
{
  path: '',
  redirectTo: '/dashboard',
  pathMatch: 'full'
},
```

> Pelajari lebih lanjut mengenai *redirects* pada sesi [Redirecting routes](https://angular.io/guide/router) pada halaman [Routing & Navigation](https://angular.io/guide/router).

### Tambahkan navigasi ke dalam template

Tambahkan link navigasi dashboard ke dalam template, di atas link *Heroes*.

#### src/app/app.component.ts (template-v3)
```javascript
template: `
   <h1>{{title}}</h1>
   <nav>
     <a routerLink="/dashboard">Dashboard</a>
     <a routerLink="/heroes">Heroes</a>
   </nav>
   <router-outlet></router-outlet>
 `
```

> Tag `<nav>` tidak berpengaruh apa-apa, tapi ia akan berguna nanti ketuka kita membuat style pada link.

Pada browser, pergi ke root aplikasi (`/`) dan reload. Aplikasi akan menampilkan dashboard dan kita bisa menavigasi antara dashboard dan heroes.

## Menambahkan heros ke dalam dashboard

Untuk membuat dashboard lebih menarik, kita akan menampilkan 4 heroes.

Ganti metadata `template` dengan property `templateUrl` yang merupakan point kepada file template baru.

#### src/app/dashboard.component.ts (metadata)
```javascript
@Component({
  selector: 'my-dashboard',
  templateUrl: './dashboard.component.html',
})
```

Buat file dan isikan dengan konten.

#### src/app/dashboard.component.html
```html
<h3>Top Heroes</h3>
<div class="grid grid-pad">
  <div *ngFor="let hero of heroes" class="col-1-4">
    <div class="module hero">
      <h4>{{hero.name}}</h4>
    </div>
  </div>
</div>
```

`*ngFor` di gunakan kembali untuk mengiterasi list heroes dan menampilkan nama. Tambahan element `<div>` akan berguna untuk styling nanti.

### Berbagi *HeroService*

Untuk mengisi array `heroes`, kita bisa menggunakan kembali `HeroService`.

Sebelumnya, kita hapus `HeroService` dari array `provides` pada `HeroesComponent` dan menambahkannya kepada array `provides` pada `AppModule`. Itu memindahkan pembuatan *singleton* instance `HeroService`, dapat di gunakan pada seluruh component di aplikasi. Angular meng-inject `HeroService` dan kita bisa menggunakannya di dalam `DashboardComponent`.

### Get heroes

Di dalam `dashboard.component.ts`, tambahkan statement `import` berikut.

#### src/app/dashboard.component.ts (imports)
```javascript
import { Component, OnInit } from '@angular/core';

import { Hero } from './hero';
import { HeroService } from './hero.service';
```

Sekarang buat class `DashboardComponent` seperti berikut.

#### src/app/dashboard.component.ts (class)
```javascript
export class DashboardComponent implements OnInit {

  heroes: Hero[] = [];

  constructor(private heroService: HeroService) { }

  ngOnInit(): void {
    this.heroService.getHeroes()
      .then(heroes => this.heroes = heroes.slice(1, 5));
  }
}
```

Logic yang di gunakan pada `HeroesComponent` adalah.

- Mendefinisikan array property `heroes`.

- Inject `HeroService` ke dalam constructor dan memasukannya ke dalam private field `heroService`.

- Panggil service untuk mendapatkan heroes di dalam Angular `ngOnInit()` lifecycle hook.

Di dalam dashboard kita menspesifikasikan 4 heroes dengan method `Array.slice`. Refresh browser dan lihat 4 nama hero di dalam dashboard yang baru.

## Navigasi detail hero

Sementara detail hero yang di pilih di tampilkan di bawah dari `HeroesComponent`, users harus bisa menavigasi kedalam `HeroDetailComponent` di dalam cara berikut.

- Dari dashboard ke hero yang di pilih.

- Dari list heroes ke hero yang di pilih.

- Dari link URL yang di pastekan ke dalam address bar browser.

### Routing detail hero

Kita bisa menambahkan route `HeroDetailComponent` di dalam `app.module.ts`, di mana route lain di konfiguraskikan.

Route baru ini tidak seperti biasa, di mana kita harus memberitahukan `HeroDetailComponent` mana hero yang akan di tampilkan. Kita tidak perlu memberi tahu `HeroesComponent` atau `DashboardComponent` apa pun.

Saat ini parent `HeroesComponent` mengatur property component menjadi object hero dengan binding seperti ini:

```html
<hero-detail [hero]="selectedHero"></hero-detail>
```

Tapi binding seperti ini tidak bekerja pada skenario routing.

### Parameterized route

Kita bisa menambahkan `id` hero pada URL. Ketikan routing ke hero yang mempunyai `id` 11, kita berharap melihat URL seperti berikut

```html
/detail/11
```

Part `/detail/` pada URL merupkan constant. Numerik `id` berubah dari hero ke hero.

### Konfigurasi route dengan parameter

Gunakan *route definition* berikut.

#### src/app/app.module.ts (hero detail)
```javascript
{
  path: 'detail/:id',
  component: HeroDetailComponent
},
```

`:id` adalah placeholder untuk `id` hero yang spesifik ketika navigasi ke `HeroDetailComponent`.

> Pastikan untuk import hero detail component sebelum membuat route ini.

Kita tidak menambahkan link `'Hero Detail'` pada template karena users tidak klik link navigasi untuk melihat detail hero, tetapi mereka meng-klik nama, yang di mana nama di tampilkan di dalam dashboard di dalam list heroes.

Kita tidak ingin menambahkan klik hero sampai `HeroDetailComponent` di revisi dan siap menavigasi.

## Revisi *HeroDetailComponent*

Berikut `HeroDetailComponent` saat ini:

#### src/app/hero-detail.component.ts (current)
```javascript
import { Component, Input } from '@angular/core';
import { Hero } from './hero';
 
@Component({
  selector: 'hero-detail',
  template: `
    <div *ngIf="hero">
      <h2>{{hero.name}} details!</h2>
      <div>
        <label>id: </label>{{hero.id}}
      </div>
      <div>
        <label>name: </label>
        <input [(ngModel)]="hero.name" placeholder="name"/>
      </div>
    </div>
  `
})
export class HeroDetailComponent {
  @Input() hero: Hero;
}
```

Template tidak berubah. Nama hero akan di tampilkan dengan cara yang sama. Perubahan utama di pengaruhi oleh bagaimana kita *get* nama hero.

Kita tidak lagi menerima hero di dalam binding property parent component. `HeroDetalComponent` baru harus mengambil peramter `id` dari *params Obeervable* di service `ActivatedRoute` dan menggunakan `HeroService` mengambul id hero.

#### src/app/hero-detail.component.ts
```javascript
// Keep the Input import for now, you'll remove it later:
import { Component, Input, OnInit } from '@angular/core';
import { ActivatedRoute, Params }   from '@angular/router';
import { Location }                 from '@angular/common';

import { HeroService } from './hero.service';
```

Inject `ActivatedRoute`, `HeroService`, dan `Location` service kedalam constructor, simpan nilainya dengan *private fields*.

#### src/app/hero-detail.component.ts (constructor)
```javascript
constructor(
  private heroService: HeroService,
  private route: ActivatedRoute,
  private location: Location
) {}
```

Import operator `switchMap` untuk di gunakan nanti pada parameter route `Observable`.

#### src/app/hero-detail.component.ts (switchMap import)
```javascript
import 'rxjs/add/operator/switchMap';
```

Beritahu class untuk mempresentasikan interface `OnInit`.

#### src/app/hero-detail.component.ts
```javascript
export class HeroDetailComponent implements OnInit {}
```

Di dalam lifecycle hook `ngOnInit()`, gunakan parameter Observable untuk ekstrak nilai parameter `id` dari service `ActivateRoute` untuk mengambil `id` hero.

#### src/app/hero-detail.component.ts
```javascript
ngOnInit(): void {
  this.route.params
    .switchMap((params: Params) => this.heroService.getHero(+params['id']))
    .subscribe(hero => this.hero = hero);
}
```

Operator `switchMap` memetakan `id` di dalam Observable route parameter kepada `Observable` yang baru, hasil dari method `HeroService.getHero()`.

Jika user menavigasi ulang ke dalam component ini, request `getHero` masih teteap berjalan, `switchMap` cancel request lama dan lalu memanggil lagi `HeroService`.

`id` hero merupakan nomor. Parameter route selalu string. Jadi nilai parameter route di convert ke dalam number dengan operator Javascript.

### Tambahkan *HeroService.getHero*

Di code sebelumnya, `HeroService` tidak mempunyai method `getHero`. Untuk menyelesaikan masalah ini, buka `HeroService` dan tambahkan method `getHero()` yang memfilter list heroes dari `getHeroes()` dengan `id`.

#### src/app/hero.service.ts (getHero)
```javascript
getHero(id: number): Promise<Hero> {
  return this.getHeroes()
             .then(heroes => heroes.find(hero => hero.id === id));
}
```

### Temukan cara kembali

User mempunyai beberapa cara untuk menavigasi ke `HeroDetilComponent`.

Untuk navigasi ke tempat lainnya, user bisa klik satu atau dua link di dalam `AppComponent` atau klik tombol back. Sekarang tambahkan option ke tiga, method `goBack()` yang menavigasi mundur satu step ke belakang di dalam browser history menggunakan service `Location` yang sebelum kita inject.

#### src/app/hero-detail.component.ts (goBack)
```javascript
goBack(): void {
  this.location.back();
}
```

> Kembali terlalu jauh bisa membawa user keluar dari aplikasi. Dalam aplikasi nyata, kita dapat mencegah masalah ini dengan *CanDeactivate* guard. Baca lebih lanjut pada halaman [CanDeactivate](https://angular.io/api/router/CanDeactivate).

Kita bisa menerapkan method ini dengan event binding pada button *Back* yang kita tambahkan pada component template.

```html
<button (click)="goBack()">Back</button>
```

Migrasi template ke pada file yang bernama `hero-detail.component.html`.

#### src/app/hero-detail.component.html
```html
<div *ngIf="hero">
  <h2>{{hero.name}} details!</h2>
  <div>
    <label>id: </label>{{hero.id}}</div>
  <div>
    <label>name: </label>
    <input [(ngModel)]="hero.name" placeholder="name" />
  </div>
  <button (click)="goBack()">Back</button>
</div>
```

Update metadata component dengan `templateUrl` dengan pointing pada file yang baru saja kita buat.

#### src/app/hero-detail.component.ts (metadata)
```javascript
@Component({
  selector: 'hero-detail',
  templateUrl: './hero-detail.component.html',
})
```

Refresh browser dan lihat hasilnya.

## Memilih dashboard hero

Ketika user memilih hero di dalam dashboard, aplikasi harus menavigasi pada `HeroDetailComponent` untuk view dan edit hero yang di pilih.

Meskipun dashboard heores di presentasikan sebagai button, mereka harus bersika seperti tag anchor. Ketika di hover di atas block hero, target URL harus m=tampil di dala status bar browser dan user harus  bisa men-copy link atau membuka detail hero pada tab baru.

Untuk melakukan effect ini, buka `dashboard.component.html` dan ganti pengulangan  `<div *ngFor...>` dengan tag `<a>`. 

#### src/app/dashboard.component.html (repeated <a> tag)
```html
<a *ngFor="let hero of heroes"  [routerLink]="['/detail', hero.id]"  class="col-1-4">
```

Perhatikan binding `[routerLink]`. Seperti yang sudah di jelaskan di bagian `Router Link` pada bagian ini, navigasi di template `AppComponent` memiliki link router yang fixed path dari tujuan routes "/dashboard" dan "/heroes".

Saat ini, kita binding pada expresi yang berisi *array link parameter*. Array mempunyai 2 element: tujuan route *path* dan *router parameter* di set pada value `id` hero.

Dua items array dengn *path* dan *:id* token di dalam parameterized hero detail route yang kita tambahkan pada `app.module.ts` sebelumnya:

#### src/app/app.module.ts (hero detail)
```javascript
{
  path: 'detail/:id',
  component: HeroDetailComponent
},
```

## Refactor routes pada *Routing Module*

Hampir 20 baris pada `AppModule` yang di khususkan untuk mengkonfigurasi 4 routes. Ide baik untuk merefactor konfigurasi routing ke dalam classnya masing-masing. Sekarang `RouterModule.forRoot()` menghasilkan Angular `ModuleWithProviders`, sebuah class untuk routing. Untuk informasi lebih jauh lihat sesi [Milestone #2: The Routing Module](https://angular.io/guide/router#routing-module) pada halaman [Routing & Navigation](https://angular.io/guide/router).

Buat file `app-routing.module.ts` sebagai persamaan dengan `app.module.ts`. Berikan content, ekstrak dari class `AppModule`.

#### src/app/app-routing.module.ts
```javascript
import { NgModule }             from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
 
import { DashboardComponent }   from './dashboard.component';
import { HeroesComponent }      from './heroes.component';
import { HeroDetailComponent }  from './hero-detail.component';
 
const routes: Routes = [
  { path: '', redirectTo: '/dashboard', pathMatch: 'full' },
  { path: 'dashboard',  component: DashboardComponent },
  { path: 'detail/:id', component: HeroDetailComponent },
  { path: 'heroes',     component: HeroesComponent }
];
 
@NgModule({
  imports: [ RouterModule.forRoot(routes) ],
  exports: [ RouterModule ]
})
export class AppRoutingModule {}
```

Point berikut adalah tipikal dari module routing:

- Module routing pull route ke dalam variable. Varibel ini menjelaskan pattern module routing jika kita export module di masa depan.

- Module routing menambahkan `RouterModule.forRoot(routes)` untuk mengimport.

- Module routing menambahkan `RouterModule` ke `exports` sehingga components di dalam pasangan modulenya untuk mengakses *Router declarables*, seperti `RouterLink` dan `RouterOutlet`.

- Tidak ada `declarations`. Pendeklarasian merupakan tanggung jawab module yang berkaitan.

- Jika kita memiliki *guard services*, module routing menambahkan module `providers`. (Tidak ada dalm contoh ini).

### Update *AppModule*

Hapus konfigurasi routing pada `AppModule` dan import `AppRoutingModule`. gunakan statement `import` dari ES2015 dan tambahkan pada list `NgModule.imports`.

Berikut adalah revisi `AppModule`, perbandingan atara refactor dan yang belum di refactor.

#### src/app/app.module.ts (after)
```javascript
import { NgModule }       from '@angular/core';
import { BrowserModule }  from '@angular/platform-browser';
import { FormsModule }    from '@angular/forms';
 
import { AppComponent }         from './app.component';
import { DashboardComponent }   from './dashboard.component';
import { HeroDetailComponent }  from './hero-detail.component';
import { HeroesComponent }      from './heroes.component';
import { HeroService }          from './hero.service';
 
import { AppRoutingModule }     from './app-routing.module';
 
@NgModule({
  imports: [
    BrowserModule,
    FormsModule,
    AppRoutingModule
  ],
  declarations: [
    AppComponent,
    DashboardComponent,
    HeroDetailComponent,
    HeroesComponent
  ],
  providers: [ HeroService ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }
```

#### src/app/app.module.ts (before)
```javascript
import { NgModule }       from '@angular/core';
import { BrowserModule }  from '@angular/platform-browser';
import { FormsModule }    from '@angular/forms';
import { RouterModule }   from '@angular/router';
 
import { AppComponent }        from './app.component';
import { HeroDetailComponent } from './hero-detail.component';
import { DashboardComponent }  from './dashboard.component';
import { HeroesComponent }     from './heroes.component';
import { HeroService }         from './hero.service';
 
@NgModule({
  imports: [
    BrowserModule,
    FormsModule,
    RouterModule.forRoot([
      {
        path: '',
        redirectTo: '/dashboard',
        pathMatch: 'full'
      },
      {
        path: 'dashboard',
        component: DashboardComponent
      },
      {
        path: 'detail/:id',
        component: HeroDetailComponent
      },
      {
        path: 'heroes',
        component: HeroesComponent
      }
    ])
  ],
  declarations: [
    AppComponent,
    DashboardComponent,
    HeroDetailComponent,
    HeroesComponent
  ],
  providers: [
    HeroService
  ],
  bootstrap: [ AppComponent ]
})
export class AppModule {
}
```

Revisi dan simpel `AppModule` fokus pada identifikasi *key pieces* dari aplikasi.

## Memilih hero dari *HeroesComponent*

Di dalam `HeroesComponent`, template saat ini memperlihatkan style "master/detail" dengan list heroes teratas dan detail dari hero yang dipilih.

#### src/app/heroes.component.ts (current template)
```javascript
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
```

Hapus `<h1` di paling atas.

Hapus baris terakhir dari template dengan tag `<hero-detail>`.

Kita tidak lagi menampilkan full `HeroDetailComponent` disini. Sebagai gantinya kita akan menampilakan detail hero di halamannya sendiri dan route ke sana seperti yang kita lakukan di dashboard.

Namun, ketika user memilih hero dari list, mereka tidak masuk ke halaman detail. Sebagi gantinya, kita akan melihat detail mini di halaman ini dan harus klik button untuk menavigasi ke halaman detail lengkap.

### Menambahkan mini detail

Tambahkan HTML berikut di bagian bawah template di mana `<hero-detail>` terlebih dahulu.

#### src/app/heroes.component.ts
```html
<div *ngIf="selectedHero">
  <h2>
    {{selectedHero.name | uppercase}} is my hero
  </h2>
  <button (click)="gotoDetail()">View Details</button>
</div>
```

Setelah klik hero, user seharusnya melihati seperti di ini di bawah list hero.

![detail mini](https://angular.io/generated/images/guide/toh/mini-hero-detail.png)

### Format dengan pipi uppercase

Nama hero di tampilkan dengan huruf besar, karena pipe `uppercase` di sertakan pada binding, setelah operator pipe (|).

```html
{{selectedHero.name | uppercase}} is my hero
```

Pipe merupakan cara yang baik untuk memformat string, currency, dates dan banyak data lain. Angular menyediakan beberapa pipe dan kita juga bisa menulis pipe kita sendiri.

> Baca lebih lanjut mengenai pipe di halaman [Pipes](https://angular.io/guide/pipes)

### Pindahkan content dari component file

Component file terlalu besar. Sulit untuk menemukan component logic dengan *noise* dari HTML dan CSS. Sebelum kita membuat perubahan, migrasi template dan style kepada filenya masing-masing.

Pertama pindahkan content template dari `heroes.component.ts` ke dalam file baru `heroes.component.html`. Jangan copy *backticks*. Kembali ke `heroes.component.ts`, selanjutnya pindahkan content style ke dalam file baru `heroes.component.css`.

Dua file tersebut akan terlihat seperti berikut.

#### src/app/heroes.component.html
```html
<h2>My Heroes</h2>
<ul class="heroes">
  <li *ngFor="let hero of heroes"
    [class.selected]="hero === selectedHero"
    (click)="onSelect(hero)">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>
<div *ngIf="selectedHero">
  <h2>
    {{selectedHero.name | uppercase}} is my hero
  </h2>
  <button (click)="gotoDetail()">View Details</button>
</div>
```

#### src/app/heroes.component.css
```css
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
.heroes li:hover {
  color: #607D8B;
  background-color: #DDD;
  left: .1em;
}
.heroes li.selected:hover {
  background-color: #BBD8DC !important;
  color: white;
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
button {
  font-family: Arial;
  background-color: #eee;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
  cursor: hand;
}
button:hover {
  background-color: #cfd8dc;
}
```

Sekarang, kemabali ke metadata component dari `heroes.component.ts`, hapus `template` dan `styles`, ganti masing-masing dengan `templateUrl` dan `styleUrls`. Set propertinya pada file yang baru saja kita buat.

#### src/app/heroes.component.ts (revised metadata)
```javascript
@Component({
  selector: 'my-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: [ './heroes.component.css' ]
})
```

> Property `styleUrls` merupakan sebuah array dari nama file (dengan path). Kita bisa list multiple style file dari lokasi yang berbeda jika kita membutuhkannya.

### Update class *HeroesComponent*

`HeroesComponent` menavigasi ke `HeroesDetailComponent` sebagai respon terhadap klik button. Event klik button terikat pada method `gotoDetail()` yang menavigasi secara imperatif dengan memberi tahu router mana yang harus pergi.

Pendekatan ini membutuhkan perubahan berikut pada class component.

1. Import `Router` dari Angular router library.

2. Inject `Router` ke dalam constructor, bersama dengan `HeroService`.

3. Implementasikan `gotoDetail()` dengan memamnggil method router `navigate()`.

#### src/app/heroes.component.ts (gotoDetail)
```javascript
gotoDetail(): void {
  this.router.navigate(['/detail', this.selectedHero.id]);
}
```

Catat bahwa kita mempassing 2 element *link parameters array* (sebuah path dan route parameter) kepada method router `navigate()`. Berikut adalah refisi dari class `HeroesComponent`.

#### src/app/heroes.component.ts (class)
```javascript
export class HeroesComponent implements OnInit {
  heroes: Hero[];
  selectedHero: Hero;
 
  constructor(
    private router: Router,
    private heroService: HeroService) { }
 
  getHeroes(): void {
    this.heroService.getHeroes().then(heroes => this.heroes = heroes);
  }
 
  ngOnInit(): void {
    this.getHeroes();
  }
 
  onSelect(hero: Hero): void {
    this.selectedHero = hero;
  }
 
  gotoDetail(): void {
    this.router.navigate(['/detail', this.selectedHero.id]);
  }
}
```

Refresh browser dan mulai meng-klik. User bisa menavigasi di sekitar aplikas, dari dashboard ke detail hero dan kembali, dari list heroes ke detail mini ke detail hero dan kembali lagi ke heroes.

Kita sudah menerapkan semua *navigational requirements* dengan baik pada halaman ini.

## Style aplikasi

Aplikasi sudah functional tapi butuh untuk di styling. Dashboard heroes harus di tampilkan di dalam persegi. Kita telah menerima sekitar 60 baris CSS untuk tujuan ini, termasuk beberapa media query untuk responsive design.

Seperti yang kita ketahui, menambahkan CSS ke metadata component `styles` akan mengaburkan logika component. Sebagai gantinya, edit CSS di dalam file `*.css` terpisah.

Tambahkan file `dashboard.component.css` ke folder `app` dan referensikan file tersebut di dalam component metadata property array `styleUrls` seperti berikut.

#### src/app/dashboard.component.ts (styleUrls)
```javascript
styleUrls: [ './dashboard.component.css' ]
```

### Menambahkan detail hero style

Tambahkan `hero-detail.component.css` ke folder `app` dan referensikan file tersebut di dalam array `styleUrls` seperti yang kita lakukan untuk `DashboardComponent`. Juga, di dalam `hero-detail.component.ts`, hilangkan property `hero` decorator `@input` dan importnya.

Berikut konten dari file component css.

#### src/app/hero-detail.component.css
```css
label {
  display: inline-block;
  width: 3em;
  margin: .5em 0;
  color: #607D8B;
  font-weight: bold;
}
input {
  height: 2em;
  font-size: 1em;
  padding-left: .4em;
}
button {
  margin-top: 20px;
  font-family: Arial;
  background-color: #eee;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer; cursor: hand;
}
button:hover {
  background-color: #cfd8dc;
}
button:disabled {
  background-color: #eee;
  color: #ccc;
  cursor: auto;
}
```

#### src/app/dashboard.component.css
```css
[class*='col-'] {
  float: left;
  padding-right: 20px;
  padding-bottom: 20px;
}
[class*='col-']:last-of-type {
  padding-right: 0;
}
a {
  text-decoration: none;
}
*, *:after, *:before {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
h3 {
  text-align: center; margin-bottom: 0;
}
h4 {
  position: relative;
}
.grid {
  margin: 0;
}
.col-1-4 {
  width: 25%;
}
.module {
  padding: 20px;
  text-align: center;
  color: #eee;
  max-height: 120px;
  min-width: 120px;
  background-color: #607D8B;
  border-radius: 2px;
}
.module:hover {
  background-color: #EEE;
  cursor: pointer;
  color: #607d8b;
}
.grid-pad {
  padding: 10px 0;
}
.grid-pad > [class*='col-']:last-of-type {
  padding-right: 20px;
}
@media (max-width: 600px) {
  .module {
    font-size: 10px;
    max-height: 75px; }
}
@media (max-width: 1024px) {
  .grid {
    margin: 0;
  }
  .module {
    min-width: 60px;
  }
}
```

### Style navigation link

CSS yang disediakan membuat link navigasi di `AppComponent` terlihat lebih seperti tombol yang dapat dipilih. 

Tambahkan file `app.component.css` pada folder `app` dengan konten berikut.

#### src/app/app.component.css (navigation styles)
```css
h1 {
  font-size: 1.2em;
  color: #999;
  margin-bottom: 0;
}
h2 {
  font-size: 2em;
  margin-top: 0;
  padding-top: 0;
}
nav a {
  padding: 5px 10px;
  text-decoration: none;
  margin-top: 10px;
  display: inline-block;
  background-color: #eee;
  border-radius: 4px;
}
nav a:visited, a:link {
  color: #607D8B;
}
nav a:hover {
  color: #039be5;
  background-color: #CFD8DC;
}
nav a.active {
  color: #039be5;
}
```

> Directive *routerLinkActive*
> 
> Angular router menyediakan `routerLinkActive` yang dapat kita gunakan untuk menambahkan class ke elemen navigasi HTML yang routenya sesuai dengan route yang aktiv. Yang harus kita lukukan adalah mendefinisikan style untuk itu.

#### src/app/app.component.ts (active router links)
```html
template: `
  <h1>{{title}}</h1>
  <nav>
    <a routerLink="/dashboard" routerLinkActive="active">Dashboard</a>
    <a routerLink="/heroes" routerLinkActive="active">Heroes</a>
  </nav>
  <router-outlet></router-outlet>
`,
```

#### src/app/app.component.ts
```javascript
styleUrls: ['./app.component.css'],
```

### Global aplikasi style

Ketika kita menambahkan style pada component, kita menahan semua yang di butuhkan component -HTML, CSS, code- bersama dalam tempat yang nyaman. Ini mudah untuk package semuanya dan menggunakan component tersebut di luar sana.

Designer menyediakan beberapa style dasar untuk di terapkan ke elemen di keseluruhan aplikasi. Ini sesuai dengan *full set of master styles* yang kita install sebelumnya sebelum setup. Berikut kutipannya:

#### src/styles.css (excerpt)
```css
/* Master Styles */
h1 {
  color: #369;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 250%;
}
h2, h3 {
  color: #444;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: lighter;
}
body {
  margin: 2em;
}
body, input[text], button {
  color: #888;
  font-family: Cambria, Georgia;
}
/* everywhere else */
* {
  font-family: Arial, Helvetica, sans-serif;
}
```

Buat file `styles.css`. Pastikan file tersebut berisi master style yang tersedia [di sini](https://raw.githubusercontent.com/angular/angular/master/aio/tools/examples/shared/boilerplate/src/styles.css). Juga edit index.html untuk mereferensikan stylesheet.

#### src/index.html (link ref)
```html
<link rel="stylesheet" href="styles.css">
```

Lihat aplikasi sekarang. Dashboard, heros, dan navigasi link sudah selesai di lakukan style.

![tour of heroes navigation](https://angular.io/generated/images/guide/toh/heroes-dashboard-1.png)

## Struktur code dan aplikasi

```
angular-tour-of-heroes
|
 -src
| |
| -app
| | |
| | -app.component.css
| | |
| | -app.compponent.ts
| | |
| | -app.module.ts
| | |
| | -app-routing.module.ts
| | |
| | -dashboard.component.css
| | |
| | -dashboard.component.html
| | |
| | -dashboard.component.ts
| | |
| | -hero.service.ts
| | |
| | -hero.ts
| | |
| | -hero-detail.component.css
| | |
| | -hero-detail.component.html
| | |
| | -hero-detail.component.ts
| | |
| | -heroes.component.css
| | |
| | -heroes.component.html
| | |
| | -heroes.component.ts
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

## Apa saja yang sudah kita pelajari?

- Kita menambahkan router Angular untuk menavigasi component yang berbeda.

- Kita belajar bagaimana membuat router links pada navigation menu items.

- Kita menggunakan parameter route link untuk menavigasi pada detail dari hero yang di pilih user.

- Kita berbagai `HeroService` di antara berbagi sempat.

- Kita memindahkan HTML dan CSS keluar file component dan memasukannya kedalam filenya masing-masing.

- Kita menambahkan pipe untuk memformat data menjadi `uppercased`.

## Referensi

- https://angular.io/tutorial/toh-pt5
