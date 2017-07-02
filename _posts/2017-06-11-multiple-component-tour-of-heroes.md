---
layout: post
title:  "Tour of Heroes #3: Multiple Component"
image: ''
date:   2017-06-11 00:05:03
tags:
- javascript
- Angular
description: ''
categories:
- Programming
serie: javascript
---

Tutorial kali ini kita akan membahas Refactor master/detail view kedalam component yang terpisah. Tutorial ini merupakan lanjutan dari tutorial Angular [Tour of Heroes](/angular-tour-of-heroes-tutorial). 

Saat ini `AppComponent` dapat melakukan apa saja. Selanjutnya akan banyak requirement dan fitur yang akan di implementasi. Kita tidak bisa terus menumpuk fitur dalam satu component, hal ini membuat sulit untuk di maintain.

Kita harus memecahnya kedalam sub-component, setiap component fokus pada tugas atau workflow yang spesifik. Ketika selesai, aplikasi akan terlihat seperti berikut [live example](https://angular.io/resources/live-examples/toh-3/ts/eplnkr.html) / [downloadable example](https://angular.io/resources/zips/toh-3/toh-3.zip).

## Component hero detail

Buat file dengan nama `hero-detail.component.ts` pada folder `app/`. File ini akan menyimpan component baru `HeroDetailComponent`.

Nama file dan component mengikuti standard yang di deskripsikan Angular [style guide](https://angular.io/docs/ts/latest/guide/style-guide.html#naming).

- Nama component class harus di tulis dengan *upper camel case* dan di akhiri dengan kata "Component". Component class hero detail adalah `HeroDetailComponent`.

- Nama file component harus di eja dengan *lower dash case*, setiap kata di sisipi dengan dash (strip), dan di akhiri dengan `.component.ts`. Class `HeroDetailComponent` ada pada file `hero-detail.component.ts`.

Mulai dengan `HeroDetailComponent` berikut.

#### app/hero-detail.component.ts (initial version)
```javascript
import { Component } from '@angular/core';

@Component({
  selector: 'hero-detail',
})
export class HeroDetailComponent {
}
```

Untuk mendefinisikan component, kita akan selalu meng-import `Component` symbol.

Selector `hero-detail`, akan sesuai dengan tag element pada template component parent. Di akhir tutorial ini, kita akan menambahkan `<hero-detail>` element pada template `AppComponent`.

Export selalu component class karena kita akan selalu meng-import nya di suatu tempat.

### Hero detail template

Untuk memindahkan detail view ke dalam `HeroDetailComponent`, cut content hero detail di bagian bawah template `AppComponent` dan paste kedalam template property baru di metadata `@Component`.

`HeroDetailComponent` mempunyai hero, bukan *selected hero*. Ganti kata "selectedHero", dengan kata, "hero", pada template. Jika sudah, template baru kita sekarang terlihat seperti ini.

#### src/app/hero-detail.component.ts (template)
```javascript
{% raw %}
@Component({
  selector: 'hero-detail',
  template: `
    <div *ngIf="hero">
      <h2>{{hero.name}} details!</h2>
      <div><label>id: </label>{{hero.id}}</div>
      <div>
        <label>name: </label>
        <input [(ngModel)]="hero.name" placeholder="name"/>
      </div>
    </div>
  `
})
{% endraw %}
```

### Tambahkan property hero

Tambahkan property pada class `HeroDetailComponent` seperti ini.

#### src/app/hero-detail.component.ts (hero property)
```javascript
hero: Hero;
```

Property `hero` di tulis sebagai instance dari `Hero`. Class `Hero` masih berada pada file `app.component.ts`. Sekarang ada 2 component yang membutuhkan reference pada class `Hero`. Agular [style guide](https://angular.io/docs/ts/latest/guide/style-guide.html#rule-of-one) merekomendasikan 1 class per file.

Pindahkan class `Hero` dari `app.component.ts` kepada file `hero.ts`.

#### src/app/hero.ts
```javascript
export class Hero {
  id: number;
  name: string;
}
```

Sekarang class `Hero` mempunyai file tersendiri, `AppComponent` dan `HeroDetailComponent` harus mengimportnya. Tambahkan statement `import` di bagian atas pada file `app.component.ts` dan `hero-detail.component.ts`

```javascript
import { Hero } from './hero';
```

### Property hero sebagai property input

Component utama `AppComponent` memberitahukan component turunannya `HeroDetailComponent`, hero yang mana yang akan di turunkan dengan *binding* `selectedHero` kepada property `hero` di `HeroDetailComponent`. Binding akan terlihat seperti ini:

```html
<hero-detail [hero]="selectedHero"></hero-detail>
```

Menempatkan tanda kurung di antara property `hero`, dan di tambah tanda sama dengan (=), hal ini membuat target kepada ekspresi binding. Kita harus mendeklarasikan target binding menjadi property input. Jika tidak, Angular akan mereject binding dan menampilkan error.

Pertama, ubah statement import `@angular/core` dengan menambah symbol `Input`.

#### src/app/hero-detail.component.ts (excerpt)

```javascript
import { Component, Input } from '@angular/core';
```

Lalu deklarasikan `hero` sebagai input property dengan menambahkan decorator `@Input` yang telah kita import sebelumnya

#### src/app/hero-detail.component.ts (excerpt)
```javascript
@Input() hero: Hero;
```

Baca lebih lanjut mengenai property input dalam halaman [Attribute Directives](https://angular.io/docs/ts/latest/guide/attribute-directives.html#why-input).

Itu saja. Property `hero` hanya berisi class `HeroDetailComponent`.

```javascript
export class HeroDetailComponent {
  @Input() hero: Hero;
}
```

Kode di atas adalah menerima object hero melalui property input dan bind property dengan template.

Berikut code `HeroDetailComponent` yang lengkap.

#### src/app/hero-detail.component.ts
```javascript
{% raw  %}
import { Component, Input } from '@angular/core';
import { Hero } from './hero';
@Component({
  selector: 'hero-detail',
  template: `
    <div *ngIf="hero">
      <h2>{{hero.name}} details!</h2>
      <div><label>id: </label>{{hero.id}}</div>
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
{% endraw %}
```

## Mendeklarasikan *HeroDetailComponent* dalam *AppModule*

Setiap component harus di deklarasikan pada satu (dan hanya boleh satu) Angular module.

Buka `app.module.ts` di editor dan import `HeroDetailComponent`.

#### src/app/app.module.ts
```javascript
import { HeroDetailComponent } from './hero-detail.component';
```

Tambahkan `HeroDetailComponent` pada module array `declarations`.

#### src/app/app.module.ts
```javascript
declarations: [
  AppComponent,
  HeroDetailComponent
]
```

Secara umum, array `declarations` berisi list component dari aplikasi, pipes, dan directive yang termasuk di dalam module. Component harus di deklarasikan di dalam module sebelum component lain menggunakannya. Module ini mendeklarasikan hanya dua component pada aplikasi, yakni `AppComponent` dan `HeroDetailComponent`.

Baca lebih lanjut mengenai Angular modules dalam guide [NgModules](https://angular.io/guide/ngmodule)

## Menambahkan component *HeroDetailComponent* ke dalam *AppComponent*

`AppComponent` masih sebagai master/detail view. Ini di gunakan untuk menampilkan detail hero. Sekarang kita akan menugaskannya kepada `HeroDetailComponent`.

Panggil ulang `hero-detail`, yang merupakan CSS `selector` pada `HeroDetailComponent`.

Tambahkan element `<hero-detail>` di bagian bawah template `AppComponent`, yang dimana view detail hero di gunakan.

#### app.component.ts (excerpt)
```javascript
<hero-detail [hero]="selectedHero"></hero-detail>
```

Sekarang setiap kali `selectedHero` berubah, `HeroDetailComponent` menampilkan hero yang baru.

Revisi dari template `AppComponent` harus terlihat seperti ini.

```javascript
{% raw %}
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
{% endraw %}
```

## Apa saja yang sudah kita rubah?

Sebelumnya, dimanapun user mengklik nama hero, detail hero akan muncul di bawah list hero. Tapi sekarang `HeroDetailView` yang akan menampilkan detail tersebut.

Refactor original `AppComponent` pada 2 component memiliki keuntungan, untuk saat ini dan yang akan datang:

1. Kita membuat simpel `AppComponent` dengan mengurangi kemampuan component tersebut.

2. Kita dapat mengembangkan `HeroDetailComponent` menjadi hero editor yang lebih kaya tanpa harus menyentuh `AppComponent`.

3. Kita dapat mengembangkan `AppComponent` tanpa harus menyentuh view detail view.

4. Kita dapat menggunakan kembali `HeroDetailComponent` dalam template lain kedepannya pada component.

### Review struktur app

Saat ini struktur folder dan file akan menjadi seperti ini.

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

- Kita membuat component yang reusable.

- Kita belajar bagaimana membuat component menyetujui input.

- Kita belajar mendeklarasikan directive yang di butuhkan aplikasi pada Angular module. Kita me-list directive di dalam `NgModule` decorator pada array `declarations`.

- Kita belajar bagaimana bind parent component dengan child component.

## Referensi

- https://angular.io/tutorial/toh-pt3
