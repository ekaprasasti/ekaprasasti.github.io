---
layout: post
title:  "Tour of Heroes #2: Master/Detail"
image: ''
date:   2017-06-10 00:05:03
tags:
- Javascript
- Angular
description: ''
categories:
- Programming
serie: Javascript
---

Tutorial kali ini, kita akan melanjutkan [tutorial aplikasi Tour of Heroes](/angular-tour-of-heroes-tutorial) untuk menampilkan list heroes, dan memungkinkan user untuk memilih hero dan menampilkan detail hero.

Akhir dari tutorial ini dapat kalian lihat di sini [live example](https://angular.io/resources/live-examples/toh-2/ts/eplnkr.html) / [downloadable example](https://angular.io/resources/zips/toh-2/toh-2.zip).

## Displaying heroes

Untuk menampilkan list dari heroes, kita harus menambahkan data heroes di view template.

### Create heroes

Buat 10 heroes dalam array.

#### src/app/app.component.ts (hero array)
```javascript
const HEROES: Hero[] = [
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

Array heroes merupakan tipe dari Hero, sebuah class yang telah kita definisikan di tutorial sebelumnya.

### Expose heroes

Buat public property di dalam class AppComponent yang meng-expose heroes agar dapat kita binding.

#### app.component.ts (hero array property)
```javascript
heroes = HEROES;
```

Type `heroes` tidak terdefinisi karena TypeScipt mengambilnya dari dari array `HEROES`.

> Data hero terpisah dari class implementasi karena nanti pada akhirnya nama hero akan di ambil dari data service.

### Menampilkan nama hero di dalam template

Untuk menampilkan nama heroes pada unordered list, masukan code HTML di bawah ini di bawah title dan di atas hero detail.

#### app.component.ts (heroes template)
```html
<h2>My Heroes</h2>
<ul class="heroes">
  <li>
    <!-- each hero goes here -->
  </li>
</ul>
```

Dari html ini kita bisa mengisi template dengan nama hero.

### List heroes dengan ngFor

Tujuan dari bagian ini adalah unuk *bind* array dari heroes di dalam component template, dan menampilkannya secara individually.

Modifikasi tag `<li>` dengan menambahkan *built-in* directive `*ngFor`.

#### app.component.ts (ngFor)
```html
<li *ngFor="let hero of heroes">
```

Baca lebih lanjut dengan `ngFor` dan input variabel template di sesi [menampilkan array dengan *ngFor](https://angular.io/docs/ts/latest/guide/displaying-data.html#ngFor), [Displaying Data](https://angular.io/docs/ts/latest/guide/displaying-data.html), sesi [ngFor](https://angular.io/docs/ts/latest/guide/template-syntax.html#ngFor), dan [Template Syntax](https://angular.io/docs/ts/latest/guide/template-syntax.html).

Didalam tag `<li>`, tambahkan content dengan menggunakan template `hero` variabel untuk menampilkan property hero.

#### app.component.ts (ngFor template)
```html
{% raw %}
<li *ngFor="let hero of heroes">
  <span class="badge">{{hero.id}}</span> {{hero.name}}
</li>
{% endraw %}
```

Ketika browser me-refresh, list heroes akan tampil.

### Style heroes

Untuk menambahkan style dari component kita, set `styles` property dalam `@Component` decorator dengan class CSS di bawah ini.

#### src/app/app.component.ts (styles)

```javascript
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
`]
```

Jangan lupa menggunakan notasi backtick untuk membuat multi-line string.

Menambah style membuat file menjadi panjang. Di tutorial selanjutnya kita akan memindahkan style ke file terpisah.

Ketika kita menambahkan style di dalam component, mereka akan terbatas hanya sebatas component tersebut. Style di atas hanya untuk AppComponent dan tidak berefek pada HTML di luar component tersebut.

Template untuk menampilkan heroes akan terlihat seperti berikut ini

#### src/app/app.component.ts (styled heroes)
```html
{% raw  %}
<h2>My Heroes</h2>
<ul class="heroes">
  <li *ngFor="let hero of heroes">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>
{% endraw %}
```

## Memilih hero

Aplikasi saat ini dapat menampilkan list hero dengan baik dengan single hero dalam details view. Tapi list dan detail view tidak saling terkoneksi. Ketika user memilih hero dari form list, hero yang di pilih harus menampilkan detail view. UI pattern ini di sebut dengan "master/detail". Di dalam kasus, *master* adalah heroes list dan *detail* adalah detail hero yang di pilih.

Selanjutnya kita akan mengkoneksikan master ke detail dengan component `selectedHero`, dengan event click.

### Menangani click event

Tambahkan event click binding pada tag `<li>` seperti ini.

#### app.component.ts (template excerpt)
```html
<li *ngFor="let hero of heroes" (click)="onSelect(hero)">
  ...
</li>
```

Ekspresi `onSelect(hero)` memanggil method pada `AppComponent` yaitu `onSelect()`, passing variabel template input `hero` sebagai argument.

Pelajari lebih lanjut mengenai event binding pada [User Input](https://angular.io/docs/ts/latest/guide/user-input.html) dan sesi [Event binding](https://angular.io/docs/ts/latest/guide/template-syntax.html#event-binding) pada page [Template Syntax](https://angular.io/docs/ts/latest/guide/template-syntax.html#event-binding).

### Tambahkan click handler untuk mengakses selected hero

Kita tidak perlu membuat property `hero` karena kita tidak menampilkan single hero, kita menampilkan list heroes. Tapi user dapat memilih salah satu heroes dengan mengkliknya. Jadi ganti property `hero` dengan property `selectedHero`.

#### src/app/app.component.ts (selectedHero)
```javascript
selectedHero: Hero;
```

Tambahkan method `onSelect()` yang mengatur property `selectedHero` kepada `hero` yang user click.

#### src/app/app.component.ts (onSelect)
```javascript
onSelect(hero: Hero): void {
  this.selectedHero = hero;
}
```

Bind property `selectedHero` yang baru dengan code di bawah ini.

#### app.component.ts (template excerpt)
```html
<h2>{{selectedHero.name}} details!</h2>
<div><label>id: </label>{{selectedHero.id}}</div>
<div>
    <label>name: </label>
    <input [(ngModel)]="selectedHero.name" placeholder="name"/>
</div>
```

### Sembunyikan empty detail dengan ngIF

Ketika aplikasi di load, `selectedHero` tidak terdifinisi. Hero yang di pilih di inisiasi ketika user mengklik nama hero. Angular tidak bisa menampilkan property `selectedHero` yang tidak terfinisi dan menampilkan error yang terlihat pada console browser.

> EXCEPTION: TypeError: Cannot read property 'name' of undefined in [null]

Tambahkan built-in directive `ngIf` dan set pada property `selectedHero` pada component.

#### src/app/app.component.ts (ngIf)
```html
{% raw %}
<div *ngIf="selectedHero">
  <h2>{{selectedHero.name}} details!</h2>
  <div><label>id: </label>{{selectedHero.id}}</div>
  <div>
    <label>name: </label>
    <input [(ngModel)]="selectedHero.name" placeholder="name"/>
  </div>
</div>
{% endraw %}
```

Sekarang aplikasi tidak lagi error dan list nama kembali muncul pada browser.

Ketika tidak ada hero yang di pilih, directive `ngIf` membuang detail hero HTML dari DOM.

Ketika user memilih hero, `selectedHero` menjadi terdefinisi dan `ngIf` mengambil content detail hero dan menampilkannya pada DOM.

Baca lebih lanjut mengenai `ngIf` dan `ngFor` dalam halaman [Structural Directives](https://angular.io/docs/ts/latest/guide/structural-directives.html) dan sesi [Built-in directive](https://angular.io/docs/ts/latest/guide/template-syntax.html#directives) dari halaman [Template Syntax](https://angular.io/docs/ts/latest/guide/template-syntax.html).

### Style selected hero

Dalam `styles` metadata yang sudah kita buat di atas, disana ada CSS class dengan nama `selected`. Untuk membuat hero yang di pilih lebih terlihat, kita akan memakai class `selected` pada tag `<li>` ketika user mengklik pada nama hero. Sebagai contoh, ketika user klik "Magneta", warna background akan menjadi seperti ini.

![style selected hero](https://angular.io/generated/images/guide/toh/heroes-list-selected.png).

Di dalam template, tambahkan `[class.selected]` binding dengan `<li>`.

#### app.component.ts (setting the CSS class)
```javascript
[class.selected]="hero === selectedHero"
```

Ketika ekspresi (`hero === selectedHero`) bernilai `true`, Angular menambahkan CSS class `selected`. Ketika ekspresi bernilai `false`, Angular menghilangkan class `selected`.

Final version dari tag `<li>` akan terlihat seperti ini.

#### app.component.ts (styling each hero)
```html
{% raw %}
<li *ngFor="let hero of heroes"
  [class.selected]="hero === selectedHero"
  (click)="onSelect(hero)">
  <span class="badge">{{hero.id}}</span> {{hero.name}}
</li>
{% endraw %}
```

Setelah "Magneta", list akan terlihat seperti ini.

![list hero](https://angular.io/generated/images/guide/toh/heroes-list-1.png)

Berikut adalah file `app.component.ts` yang lengkap.

#### src/app/app.component.ts
```javascript
{% raw %}
import { Component } from '@angular/core';
export class Hero {
  id: number;
  name: string;
}
const HEROES: Hero[] = [
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
    <div *ngIf="selectedHero">
      <h2>{{selectedHero.name}} details!</h2>
      <div><label>id: </label>{{selectedHero.id}}</div>
      <div>
        <label>name: </label>
        <input [(ngModel)]="selectedHero.name" placeholder="name"/>
      </div>
    </div>
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
  `]
})
export class AppComponent {
  title = 'Tour of Heroes';
  heroes = HEROES;
  selectedHero: Hero;
  onSelect(hero: Hero): void {
    this.selectedHero = hero;
  }
}
{% endrow %}
```

## Apa saja yang sudah kita pelajari?

- Aplikasi tour of heroes menampilkan selectable list heroes.

- Kita menambahkan kemampuan untuk memilih hero dan menampilkan detail hero.

- Kita belajar menggunakan built-in directive `ngIf` dan `ngFor` dalam component template.

Di tutorial selanjutnya, kita akan memisahkan aplikasi ke pada subcomponent dan membuat mereka dapat bekerja secara bersamaan.

## Referensi

- https://angular.io/tutorial/toh-pt2
