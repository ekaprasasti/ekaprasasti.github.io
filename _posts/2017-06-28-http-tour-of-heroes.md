---
layout: post
title:  "Tour of Heroes: HTTP"
image: ''
date:   2017-06-28 00:05:03
tags:
- javascript
- Angular
description: ''
categories:
- Programming
serie: javascript
---

Kita memasuki bagian akhir dari rangkaian tutorial Angular aplikasi Tour of Heroes yaitu HTTP.

Pada halaman ini, kita akan membuat beberapa pengembangan yaitu

- Mengambil data hero dari server.

- Memberi akses user untuk menambahkan, mengedit, dan menghapus nama hero.

- Simpan perubahan pada server.

Kita akan mengajarkan aplikasi untuk membuat panggilan HTTP yang sesuai ke remote server web API.

Ketika tutorial ini selesai aplikasi akan terlihat seperti ini [live example](https://angular.io/generated/live-examples/toh-pt6/eplnkr.html) / [download example](https://angular.io/generated/zips/toh-pt6/toh-pt6.zip).

## HTTP Service

[`HttpModule`](https://angular.io/api/http/HttpModule) bukan merupakan core module dari Angular. [`HttpModule`](https://angular.io/api/http/HttpModule) merupkan pilihan web access untuk Angular. Ini ada sebagai modul add-on terpisah yang disebut `@angular/http` dan dikirim dalam file script terpisah sebagai bagian dari package npm Angular.

Kita sudah siap untuk import `@angular/http` karena `systemjs.config` mengkonfigurasikan *SystemJS* untuk meload library ketika kita membutuhkan.

## Register HTTP Service

Aplikasi ini akan bergantung pada Angular http service, yang bergantung pada services pendukung lainnya. `HttpModule` dari library `@angular/http` memuat provider untuk satu set lengkap HTTP service.

Untuk memuat akses service ini dari semua aplikasi, tambahkan `HttpModule` ke daftar import `AppModule`.

#### src/app/app.module.ts (v1)
```javascript
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule }   from '@angular/forms';
import { HttpModule }    from '@angular/http';
 
import { AppRoutingModule } from './app-routing.module';
 
import { AppComponent }         from './app.component';
import { DashboardComponent }   from './dashboard.component';
import { HeroesComponent }      from './heroes.component';
import { HeroDetailComponent }  from './hero-detail.component';
import { HeroService }          from './hero.service';
 
@NgModule({
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    AppRoutingModule
  ],
  declarations: [
    AppComponent,
    DashboardComponent,
    HeroDetailComponent,
    HeroesComponent,
  ],
  providers: [ HeroService ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }
```

Perhatikan bahwa kita juga menambahkan `HttpModule` sebagai bagian dari *imports* array di dalam NgModule `AppModule`.

## Mensimulasikan web API

Kami merekomendasikan meregister service aplikasi di dalam root *providers* `AppModule`.

Sampai kita mempunyai web server yang dapat menangani permintaan data hero, HTTP client akan mengambil dan menyimpan data dari *mock service* (service tiruan) API web dalam memori.

Update `src/app/app.module.ts` dengan versi berikut, yang menggunakan *mock service*.

#### src/app/app.module.ts (v2)
```javascript
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule }   from '@angular/forms';
import { HttpModule }    from '@angular/http';
 
import { AppRoutingModule } from './app-routing.module';
 
// Imports for loading & configuring the in-memory web api
import { InMemoryWebApiModule } from 'angular-in-memory-web-api';
import { InMemoryDataService }  from './in-memory-data.service';
 
import { AppComponent }         from './app.component';
import { DashboardComponent }   from './dashboard.component';
import { HeroesComponent }      from './heroes.component';
import { HeroDetailComponent }  from './hero-detail.component';
import { HeroService }          from './hero.service';
 
@NgModule({
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    InMemoryWebApiModule.forRoot(InMemoryDataService),
    AppRoutingModule
  ],
  declarations: [
    AppComponent,
    DashboardComponent,
    HeroDetailComponent,
    HeroesComponent,
  ],
  providers: [ HeroService ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }
```

Alih-alih membutuhkan server API yang sebenarnya, contoh ini mensimulasikan komunikasi dengan remote server dengan menambahkan [InMemoryWebApiModule](https://github.com/angular/in-memory-web-api) ke modul `imports`, menggantikan layanan backend XHR `Http` client dengan alternatif in-memory secara efektif.

```javascript
InMemoryWebApiModule.forRoot(InMemoryDataService),
```

Konfigurasi method `forRoot()` mengambil class `InMemoryDataService` dalam memori database. Tambahkan file `in-memory-data.service.ts` pada folder `app` dengan konten berikut.

#### src/app/in-memory-data.service.ts
```javascript
import { InMemoryDbService } from 'angular-in-memory-web-api';
export class InMemoryDataService implements InMemoryDbService {
  createDb() {
    const heroes = [
      { id: 0,  name: 'Zero' },
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
    return {heroes};
  }
}
```

File ini menggantikan `mock-heroes.ts`, yang sekarang aman untuk di delete. Menambahkan hero "Zero" untuk konfirmasi bahwa data service dapat menangani hero dengan `id==0`.

> API web in-memory hanya berguna pada tahap awal pengembangan dan untuk demonstrasi seperti Tour of Heroes ini. Jangan khawatir dengan detail substitusi backend ini; Kita dapat melewatkannya saat Kita memiliki server API web yang nyata.
> 
> Baca lebih lanjut mengenai in-memory API web dalam sesi [Appendix: Tour of Heroes in-memory web api](https://angular.io/guide/http#in-mem-web-api) dari halaman [Http Client](https://angular.io/guide/http#in-mem-web-api).

## Heroes dan HTTP

Dalam implementasi `HeroService` saat ini, sebuah Promise  di selesaikan dengan me-*return* mock heroes.

#### src/app/hero.service.ts (old getHeroes)
```javascript
getHeroes(): Promise<Hero[]> {
  return Promise.resolve(HEROES);
}
```

Hal ini di implementasikan dalam mengantisipasi pengambilan data heroes dengan HTTP client, yang mana merupakan operasi asynchronous.

Sekarang convert `getHeroes()` untuk menggunakan HTTP.

#### src/app/hero.service.ts (updated getHeroes and new class members)
```javascript
private heroesUrl = 'api/heroes';  // URL to web api
 
constructor(private http: Http) { }
 
getHeroes(): Promise<Hero[]> {
  return this.http.get(this.heroesUrl)
             .toPromise()
             .then(response => response.json().data as Hero[])
             .catch(this.handleError);
}
 
private handleError(error: any): Promise<any> {
  console.error('An error occurred', error); // for demo purposes only
  return Promise.reject(error.message || error);
}
```

Update statement import seperti berikut.

```javascript
import { Injectable }    from '@angular/core';
import { Headers, Http } from '@angular/http';

import 'rxjs/add/operator/toPromise';

import { Hero } from './hero';
```

Refresh browser. Data hero seharusnya sudah berhasil di load dari mock server.

### HTTP Promise

Angular `http.get` me-return RxJS `Observable`. *Observables* merupakan cara powerful untuk mengelola arus data asynchronous. Kita akan belajar Observables lebih lanjut nanti pada tutorial ini.

Untuk saat ini, kita meng-convert `Observable` ke `Promise` menggunakan operator `toPromise`.

```javascript
.toPromise()
```

Ada banyak operator seperti `toPromise` yan merupakan *extend* dari `Observable` dengan kemampuan yang bermanfaat. Untuk menggunakan kemampuan tersebut, kita harus menambahkan sendiri operatornya. Hal itu semudah mengimpornya dari library RxJS seperti berikut.

```javascript
import 'rxjs/add/operator/toPromise';
```

Kita akan menambahkan lebih banyak operator, dan belajar mengapa kita harus melakukakannya, nanti pada tutorial ini.

### Extract data di dalam callback *then*

Di dalam promise callback `then()`, kita memanggil method `json` dari HTTP `response` untuk meng-ekstrak data dengan response nya.

```javascript
.then(response => response.json().data as Hero[])
```

Respon JSON memiliki satu properti `data`, yang menyimpan array dari heroes yang di inginkan. Jadi kita mengambil array dan mengembalikannya sebagai nilai Promise yang terselesaikan.

> Perhatikan berntuk data yang di kembalikan oleh server. Contoh API web in-memory ini  mengembalikan sebuah objek dengan property data. API anda mungkin akan mengembalikan yang lain. Sesuaikan kode yang cocok dengan API web anda.

### Error Handling

Pada akhir dari `getHeroes()`, kita `catch` kegagalan server dan melewatkannya pada sebuah *error handler*.

```javascript
.catch(this.handleError);
```

Ini adalah langkah yang penting. Kita harus mengantisipari kegagalan HTTP, karena sering terjadi karena alasan di luar kendali kita.

```javascript
private handleError(error: any): Promise<any> {
  console.error('An error occurred', error); // for demo purposes only
  return Promise.reject(error.message || error);
}
```

Demo service ini mengirimkan kesalahan ke konsol; Dalam kehidupan nyata, kita akan menangani kesalahan dalam kode. Untuk demo, ini berhasil.

Kode tersebut juga menyertakan kesalahan kepada promise yang di *reject*, sehingga dapat menampilkan pesan kesalahan yang benar kepada user.

### Mengambil hero dengan id

Ketika `HeroDetailComponent` meminta `HeroService` untuk mengambil hero, `HeroService` saat ini mengambil semua hero dan memfilter yang memiliki id yang cocok. Itu bagus untuk simulasi, tapi akan sia-sia untuk meminta server nyata untuk semua heroes saat kita menginginkannya. Sebagian besai web API mendukung permintaan *get-by-id* dalam bentuk `api/hero/:id` (seperti `api/hero/11`).

Update method `HeroService.getHero()` untuk membuat request *get-by-id*.

#### src/app/hero.service.ts
```javascript
getHero(id: number): Promise<Hero> {
  const url = `${this.heroesUrl}/${id}`;
  return this.http.get(url)
    .toPromise()
    .then(response => response.json().data as Hero)
    .catch(this.handleError);
}
```

Request hampir sama dengan `getHeros()`. Id hero di dalam URL mengidentifikasikan hero yang mana pada server yang akan di update.

Selain itu, data di dalam response dianggap sebagai object tunggal hero dan bukan merupakan sebuah array.

## API *getHeroes* yang tidak berubah

Meskipun kita membuat perubahan internal yang signifikan untuk `getHeroes()` dan `getHero()`, perubahan tidak akan berubah. Kita masih mengembalikan Promise dari kedua method tersebut. kita tidak perlu memperbarui salah satu component yang memanggil mereka.

Sekarang saatnya untuk menambahkan kemampuan untuk *create* dan *delete* hero.

## Meng-update detail hero

Cobalah untuk meng-edit nama hero di dalam detail hero view. Saat kita mengetik, nama hero ter-update pada heading view. Tetapi ketika kita klik tombol Back, perubahan akan hilang.

Update sebelumnya tidak hilang. Lalu apa yang berubah? Saat aplikasi menggunakan mock heroes, perubahan di terapkan langsung pada obejek hero dalam daftar tunggal, *app-wide*, *shared list*. Sekarang setelah kita mengambil data dari server, jika kita ingin perubahan lebih lanjut, kita harus menuliskannya kembali ke server.

### Tambahkan kemampuan untuk menyimpan detail hero

Pada akhir dari detail template hero, tambahkan tombol save dengan *event binding* `click` yang memanggil method component baru bernama `save()`.

#### src/app/hero-detail.component.html (save)
```html
<button (click)="save()">Save</button>
```

Tambahkan method `save()` berikut, yang menyimpan perubahan nama hero menggunakan method service hero  `update()` dan menavigasi kembali ke halaman sebelumnya.

#### src/app/hero-detail.component.ts (save)
```javascript
save(): void {
  this.heroService.update(this.hero)
    .then(() => this.goBack());
}
```

### Menambahkan method servce hero *update()*

Keseluruhan method `update()` mirip dengan `getHeroes()`, namun menggunakan HTTP `put()` untuk mempertahankan perubahan pada *server-side*.

#### src/app/hero.service.ts (update)
```javascript
private headers = new Headers({'Content-Type': 'application/json'});

update(hero: Hero): Promise<Hero> {
  const url = `${this.heroesUrl}/${hero.id}`;
  return this.http
    .put(url, JSON.stringify(hero), {headers: this.headers})
    .toPromise()
    .then(() => hero)
    .catch(this.handleError);
}
```

Untuk mengidentifikasi hero yang mana pada server yang harus di update, `id` hero di *encoded* di dalam URL. Body `put()` yang merupakan JSON string encoding dari hero, di peroleh dengan memanggil `JSON.stringify`. Tipe *body content* (`application/json`) di identifikasikan pada header request.

Refresh browser, ubah nama hero, simpan perubahan, dan klik button Back pada browser. Perubahan sekarang telah tersimpan.

## Kemampuan untuk menambahkan hero

Untuk menambahkan hero, aplikasi membutuhkan nama hero. Kita bisa menggunakan element `input` yang di hubungkan pada tombol add.

Masukan kode berikut ke dalam component HTML heroes, setelah heading.

#### src/app/heroes.component.html (add)
```html
<div>
  <label>Hero name:</label> <input #heroName />
  <button (click)="add(heroName.value); heroName.value=''">
    Add
  </button>
</div>
```

Pada respon event click, panggil component click handler lalu bersihkan input field sehingga siap untuk di masukan nama lain.

#### src/app/heroes.component.ts (add)
```javascript
add(name: string): void {
  name = name.trim();
  if (!name) { return; }
  this.heroService.create(name)
    .then(hero => {
      this.heroes.push(hero);
      this.selectedHero = null;
    });
}
```

Ketika nama yang di berikan tidak kosong, handler akan mendelegasikan pembuatan nama hero ke dalam service hero, dan kemudian menambahkan hero baru ke dalam array.

Implementasikan method `create()` di dalam class `HeroService`.

#### src/app/hero.service.ts (create)
```javascript
create(name: string): Promise<Hero> {
  return this.http
    .post(this.heroesUrl, JSON.stringify({name: name}), {headers: this.headers})
    .toPromise()
    .then(res => res.json().data as Hero)
    .catch(this.handleError);
}
```

Refresh browser dan buat beberapa hero.

## Kemampuan menghapus hero

Setiap hero dalam view heroes memiliki tombol delete.

Tambahkan element button pada heroes component HTML, setelah nama hero di tampilkan pada perulangan element `<li>`.

```html
<button class="delete"
  (click)="delete(hero); $event.stopPropagation()">x</button>
```

Elemetn `<li>` sekarang menjadi seperti ini.

#### src/app/heroes.component.html (li-element)
```html
<li *ngFor="let hero of heroes" (click)="onSelect(hero)"
    [class.selected]="hero === selectedHero">
  <span class="badge">{{hero.id}}</span>
  <span>{{hero.name}}</span>
  <button class="delete"
    (click)="delete(hero); $event.stopPropagation()">x</button>
</li>
```
 
Selain memanggil methdo component `delete()`, kode click handler tombol delete menghentikan penyebaran click event. Kita tidka ingin click handler `<li>` di triggered karena hal tersebut akan menghapus hero yang di pilih user.

Logika dari handler `delete()` agak rumit.

#### src/app/heroes.component.ts (delete)
```javascript
delete(hero: Hero): void {
  this.heroService
      .delete(hero.id)
      .then(() => {
        this.heroes = this.heroes.filter(h => h !== hero);
        if (this.selectedHero === hero) { this.selectedHero = null; }
      });
}
```

Tentu saja kita mendelegasikan penghapusan hero pada service hero, namun component tersebut masih bertanggung jawab untuk memperbaharui tampilan. Ini akan menghapus hero yang dihapus dari daftar dan mengatur ulang hero yang dipilih, jika perlu.

Untuk menempatkan tombol hapus di bagian paling kanan dari entri hero, tambahkan CSS berikut.

#### src/app/heroes.component.css (additions)
```css
button.delete {
  float:right;
  margin-top: 2px;
  margin-right: .8em;
  background-color: gray !important;
  color:white;
}
```

### Method hero service *delete()*

Tambahkan method hero service `delete()`, yang menggunakan HTTP method `delete()` untuk menghapus hero dari server.

#### src/app/hero.service.ts (delete)
```javascript
delete(id: number): Promise<void> {
  const url = `${this.heroesUrl}/${id}`;
  return this.http.delete(url, {headers: this.headers})
    .toPromise()
    .then(() => null)
    .catch(this.handleError);
}
```

Refresh browser dan coba fungsional baru delete.

## Observables

Setiap method service `Http` mengembalikan sebuah `Observable` dari objek HTTP `Response`.

`HeroService` meng-convert `Observable` tersebut kedalam `Promise` dan mengembalikan promise kepada pemanggilnya. Sesi ini akan menunjukan kita bagaimana, kapan, dan mengapa me-*return* atau mengembalikan `Observable` secara direct.

### Background

Sebuah Observable merupakan *stream* dari event yang bisa kita proses menggunakan array seperti operator.

Core Angular mempunya dukungan basic terhadapa observable. Developer menambahkan dukungan dengan operator dan ekstensi dari [library RxJS](http://reactivex.io/rxjs). 

Ingatlah bahwa HeroService berhubungan dengan operator `toPromise` ke hasil Observable dari `http.get()`. Operator itu meng-*convert* Observable menjadi Promise dan kita melewati promise tersebut kepada pemanggilnya.

Mengkonversi Promise seringkali merupakan pilihan yang baik. Kita biasa meminta `http.get()` untuk mengambil suatu data. Bila kita sudah menerima data, kita berhasil.

Tapi request tidak selalu di lakukan satu kali. Kita dapat memulai dengan satu permintaan, membatalkannya, dan mengajukan request yang berbeda sebelum server merespon request yang pertama.

Urutan *request-cancel-new-request* sulit di terapkan dengan Promise, tapi mudah dengan menggunakan Observable.

### Menambahkan kemampuan pencarian nama

Kita akan menambahkan fitur pencarian pada aplikasi Tour of Heroes. Dengan user mengtikan nama ke dalam *search box*, kita akan membuat perimntaan HTTP berulang untuk heroes memfilter dari nama tersebut.

Dimulai dengan membuat `HeroSearchService` yang mengirim *search queries* kepada server web API.

#### src/app/hero-search.service.ts
```javascript
import { Injectable } from '@angular/core';
import { Http }       from '@angular/http';
 
import { Observable }     from 'rxjs/Observable';
import 'rxjs/add/operator/map';
 
import { Hero }           from './hero';
 
@Injectable()
export class HeroSearchService {
 
  constructor(private http: Http) {}
 
  search(term: string): Observable<Hero[]> {
    return this.http
               .get(`api/heroes/?name=${term}`)
               .map(response => response.json().data as Hero[]);
  }
}
```

Panggilan `http.get()` di dalam `HeroSearchService`  serupa dengan yang ada di `HeroService`, meskipun URL saat ini mempunyai *query string*.

Lebih penting lagi, kita tidak lagi memanggil `toPromise()`. Sebagai gantinya kita mengembalikan Observable dari `http.get()`, setelah menghubungkannya ke operator RxJS lainnya, `map()`, untuk mengekstrak hero dari response data. Operator RxJS membuat pemrosesan respon mudah dan mudah di baca. Lihat pembahasan pada tutorial ini tentang operator.

### HeroSearchComponent

Buat sebuah `HeroSearchComponent` yang memanggil `HeroSearchService` yang baru kita buat. 

Template component di buat sederhana, hanya text box dan list dari hasil pencarian yang sesuai.

#### src/app/hero-search.component.html
```html
<div id="search-component">
  <h4>Hero Search</h4>
  <input #searchBox id="search-box" (keyup)="search(searchBox.value)" />
  <div>
    <div *ngFor="let hero of heroes | async"
         (click)="gotoDetail(hero)" class="search-result" >
      {{hero.name}}
    </div>
  </div>
</div>
``` 

Juga tambahkan sebuah style dari component yang baru kita buat.

#### src/app/hero-search.component.css
```css
.search-result{
  border-bottom: 1px solid gray;
  border-left: 1px solid gray;
  border-right: 1px solid gray;
  width:195px;
  height: 16px;
  padding: 5px;
  background-color: white;
  cursor: pointer;
}
 
.search-result:hover {
  color: #eee;
  background-color: #607D8B;
}
 
#search-box{
  width: 200px;
  height: 20px;
}
```

Dengan user mengetikan nama di dalam *search box*, buah keyup event binding memanggil method `search()` component dengan nilai *search box* yang baru.

Seperti yang di harapkan, `*ngFor` melakukan pengulangan object hero dari property component `heroes`.

Tapi yang kita lihat nanti, property `heroes` saat ini merupakan sebuat *Observable* dari array hero, bukan hanya array hero. `*ngFor` tidak bisa melakukan apa-apa dengan `Observable` sampai kita membuat route melalu pipi `aync` (`AsyncPipe`). Pipe `ascync` mengikuti `Observable` dan membuat array dari heroes ke `*ngFor`.

Buat `HeroSearchComponent` class dan metadata.

#### src/app/hero-search.component.ts
```javascript
import { Component, OnInit } from '@angular/core';
import { Router }            from '@angular/router';
 
import { Observable }        from 'rxjs/Observable';
import { Subject }           from 'rxjs/Subject';
 
// Observable class extensions
import 'rxjs/add/observable/of';
 
// Observable operators
import 'rxjs/add/operator/catch';
import 'rxjs/add/operator/debounceTime';
import 'rxjs/add/operator/distinctUntilChanged';
 
import { HeroSearchService } from './hero-search.service';
import { Hero } from './hero';
 
@Component({
  selector: 'hero-search',
  templateUrl: './hero-search.component.html',
  styleUrls: [ './hero-search.component.css' ],
  providers: [HeroSearchService]
})
export class HeroSearchComponent implements OnInit {
  heroes: Observable<Hero[]>;
  private searchTerms = new Subject<string>();
 
  constructor(
    private heroSearchService: HeroSearchService,
    private router: Router) {}
 
  // Push a search term into the observable stream.
  search(term: string): void {
    this.searchTerms.next(term);
  }
 
  ngOnInit(): void {
    this.heroes = this.searchTerms
      .debounceTime(300)        // wait 300ms after each keystroke before considering the term
      .distinctUntilChanged()   // ignore if next search term is same as previous
      .switchMap(term => term   // switch to new observable each time the term changes
        // return the http search observable
        ? this.heroSearchService.search(term)
        // or the observable of empty heroes if there was no search term
        : Observable.of<Hero[]>([]))
      .catch(error => {
        // TODO: add real error handling
        console.log(error);
        return Observable.of<Hero[]>([]);
      });
  }
 
  gotoDetail(hero: Hero): void {
    let link = ['/detail', hero.id];
    this.router.navigate(link);
  }
}
```

#### Search Terms

Fokus pada `searchTerms`:

```javascript
private searchTerms = new Subject<string>();

// Push a search term into the observable stream.
search(term: string): void {
  this.searchTerms.next(term);
}
```

`Subject` merupakan *producer* dari observable event stream. `searchTerms` memproduksi string `Observable`, kriteria filter dari pencarian nama.

#### Inisiasi property *heroes* (*ngOnInit*)

`Subject` juga merupakan `Observable`. Kita dapat merubah stream dari array `Hero` dan memberikan harisnya ke property `heroes`.

```javascript
heroes: Observable<Hero[]>;
 
ngOnInit(): void {
  this.heroes = this.searchTerms
    .debounceTime(300)        // wait 300ms after each keystroke before considering the term
    .distinctUntilChanged()   // ignore if next search term is same as previous
    .switchMap(term => term   // switch to new observable each time the term changes
      // return the http search observable
      ? this.heroSearchService.search(term)
      // or the observable of empty heroes if there was no search term
      : Observable.of<Hero[]>([]))
    .catch(error => {
      // TODO: add real error handling
      console.log(error);
      return Observable.of<Hero[]>([]);
    });
}
```

Melewatkan setiap *keystroke* user secara langsung ke `HeroSearchService` akan membuat HTTP request yang berlebihan, menghemat sumber daya server dan membakar paket data jaringan seluler.

Sebagai gantinya, kita bisa mengelompokan operatro Observable yang mengurangi arus request ke string Observable. Kita akan melakukan lebih sedikit panggilan ke `HeroSearchService` dan masih mendapatkan hasil yang cepat. Berikut caranya.

- `debounceTime(300)` tunggu sampai arus event string baru berhenti selama 300 milidetik sebelum melewati sepanjang string terbaru. Kita tidka pernah membuat permintaan lebih sering daripada 300ms.

- `distinctUntilChanged` memastikan bahwa permintaan hanya dikirim jika teks filter berubah.

- `switchMap()` memanggil service pencarian untuk setiap search term yang membuatnya melalui `debounce` dan `distinctUntilChanged`. Ini membatalkan dan membuang search observable sebelumnya, mengembalikan hanya observable search service yang baru.

- `catch` meng-intercept observable yang gagal. Contoh sederhana mencetak kesalahan ke konsol, aplikasi akan teteap hidup. Kemudian untuk menghapus hasi pencarian, kita mengembalikan obeservable yang berisi array kosong.

### Import operator RxJS

Sebagian besar operator RxJS tidak termasuk dalam Angular dari dasar implementasi Observable. Implementasi pada dasarnya hanya mencakup apa yang di butuhkan Angular itu sendiri.

Ketika kita membutuhkan lebih banyak fitur RxJS, tambahkan `Observable` dengan meng-*import* library di mana mereka di definisikan. Berikut adalah semua import RxJS yang di butuhkan oleh component.

#### src/app/hero-search.component.ts (rxjs imports)
```javascript
import { Observable }        from 'rxjs/Observable';
import { Subject }           from 'rxjs/Subject';

// Observable class extensions
import 'rxjs/add/observable/of';

// Observable operators
import 'rxjs/add/operator/catch';
import 'rxjs/add/operator/debounceTime';
import 'rxjs/add/operator/distinctUntilChanged';
```

Syntax `import 'rxjs/add/...'` mungkin kurang familiar. Karena tidak ada daftar simbol di antara kurung kurawal `{...}`.

Kita tidak memerlukan simbol operator itu sendiri. Dalam setiap kasus, tindakan meng-*import* library akan me-*loads* dan *executes* script library yang di pilih, pada gilirannya, menambahkan operator ke class `Observable`.

### Menambahkan component search ke dashboard

Tambahkan element HTML search hero di bawah dari template `DashboardComponent`.

#### src/app/dashboard.component.html
```html
<h3>Top Heroes</h3>
<div class="grid grid-pad">
  <a *ngFor="let hero of heroes"  [routerLink]="['/detail', hero.id]"  class="col-1-4">
    <div class="module hero">
      <h4>{{hero.name}}</h4>
    </div>
  </a>
</div>
<hero-search></hero-search>
```

Akhirnya, import `HeroSearchComponent` dari `hero-search.component.ts` dan tambahkan kepada array `declarations`.

#### src/app/app.module.ts (search)
```javascript
declarations: [
  AppComponent,
  DashboardComponent,
  HeroDetailComponent,
  HeroesComponent,
  HeroSearchComponent
],
```

Jalankan aplikasi kembali. Di dalam Dashboard, masukan beberapa text pada *search box*. Jika kita memasukan karakter yang sesuai dengan nama hero yang ada, kita akan melihat sesuatu seperti ini.

![Hero search box](https://angular.io/generated/images/guide/toh/toh-hero-search.png)

## Struktur aplikasi dan kode

Review contoh source code pada [live example](https://angular.io/generated/live-examples/toh-pt6/eplnkr.html) / [download example](https://angular.io/generated/zips/toh-pt6/toh-pt6.zip) pada tutorial ini. Pastikan kita memiliki struktur aplikasi seperti berikut.

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
| | -hero-search.component.html (new)
| | |
| | -hero-search.component.css (new)
| | |
| | -hero-search.component.ts (new)
| | |
| | -hero-search.service.ts (new)
| | |
| | -heroes.component.css
| | |
| | -heroes.component.html
| | |
| | -heroes.component.ts
| | |
| | -in-memory-data.service.ts (new)
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

Kita berada di akhir perjalanan, dan kita telah menyelesaikan banyak hal.

- Menambahkan dependencies yang di butuhkan untuk menggunakan HTTP di dalam aplikasi.

- Merefactor `HeroService` untuk meload hero dari web API.

- Menambahkan `HeroService` untuk mendukung `post()`, `put()`, dan method `delete()`.

- Meng-update component untuk menambahkan fitur adding, editing, dan deleting hero.

- Mengkonfigurasi sebuah in-memory web API.

- Belajar bagaimana menggunakan Observable.

Berikut adalah file yang kita tambahkankan atau di ubah pada tutorial ini.

#### app.component.ts
```javascript
import { Component }          from '@angular/core';
 
@Component({
  selector: 'my-app',
  template: `
    <h1>{{title}}</h1>
    <nav>
      <a routerLink="/dashboard" routerLinkActive="active">Dashboard</a>
      <a routerLink="/heroes" routerLinkActive="active">Heroes</a>
    </nav>
    <router-outlet></router-outlet>
  `,
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'Tour of Heroes';
}
```

#### app.module.ts
```javascript
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule }   from '@angular/forms';
import { HttpModule }    from '@angular/http';
 
import { AppRoutingModule } from './app-routing.module';
 
// Imports for loading & configuring the in-memory web api
import { InMemoryWebApiModule } from 'angular-in-memory-web-api';
import { InMemoryDataService }  from './in-memory-data.service';
 
import { AppComponent }         from './app.component';
import { DashboardComponent }   from './dashboard.component';
import { HeroesComponent }      from './heroes.component';
import { HeroDetailComponent }  from './hero-detail.component';
import { HeroService }          from './hero.service';
import { HeroSearchComponent }  from './hero-search.component';
 
@NgModule({
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    InMemoryWebApiModule.forRoot(InMemoryDataService),
    AppRoutingModule
  ],
  declarations: [
    AppComponent,
    DashboardComponent,
    HeroDetailComponent,
    HeroesComponent,
    HeroSearchComponent
  ],
  providers: [ HeroService ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }
```

#### heroes.component.ts
```javascript
import { Component, OnInit } from '@angular/core';
import { Router }            from '@angular/router';
 
import { Hero }                from './hero';
import { HeroService }         from './hero.service';
 
@Component({
  selector: 'my-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: [ './heroes.component.css' ]
})
export class HeroesComponent implements OnInit {
  heroes: Hero[];
  selectedHero: Hero;
 
  constructor(
    private heroService: HeroService,
    private router: Router) { }
 
  getHeroes(): void {
    this.heroService
        .getHeroes()
        .then(heroes => this.heroes = heroes);
  }
 
  add(name: string): void {
    name = name.trim();
    if (!name) { return; }
    this.heroService.create(name)
      .then(hero => {
        this.heroes.push(hero);
        this.selectedHero = null;
      });
  }
 
  delete(hero: Hero): void {
    this.heroService
        .delete(hero.id)
        .then(() => {
          this.heroes = this.heroes.filter(h => h !== hero);
          if (this.selectedHero === hero) { this.selectedHero = null; }
        });
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

#### heroes.component.html
```html
<h2>My Heroes</h2>
<div>
  <label>Hero name:</label> <input #heroName />
  <button (click)="add(heroName.value); heroName.value=''">
    Add
  </button>
</div>
<ul class="heroes">
  <li *ngFor="let hero of heroes" (click)="onSelect(hero)"
      [class.selected]="hero === selectedHero">
    <span class="badge">{{hero.id}}</span>
    <span>{{hero.name}}</span>
    <button class="delete"
      (click)="delete(hero); $event.stopPropagation()">x</button>
  </li>
</ul>
<div *ngIf="selectedHero">
  <h2>
    {{selectedHero.name | uppercase}} is my hero
  </h2>
  <button (click)="gotoDetail()">View Details</button>
</div>
```

#### heroes.component.css
```css
selected {
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
button.delete {
  float:right;
  margin-top: 2px;
  margin-right: .8em;
  background-color: gray !important;
  color:white;
}
```

#### hero-detail.component.ts
```javascript
import 'rxjs/add/operator/switchMap';
import { Component, OnInit }      from '@angular/core';
import { ActivatedRoute, Params } from '@angular/router';
import { Location }               from '@angular/common';
 
import { Hero }        from './hero';
import { HeroService } from './hero.service';
 
@Component({
  selector: 'hero-detail',
  templateUrl: './hero-detail.component.html',
  styleUrls: [ './hero-detail.component.css' ]
})
export class HeroDetailComponent implements OnInit {
  hero: Hero;
 
  constructor(
    private heroService: HeroService,
    private route: ActivatedRoute,
    private location: Location
  ) {}
 
  ngOnInit(): void {
    this.route.params
      .switchMap((params: Params) => this.heroService.getHero(+params['id']))
      .subscribe(hero => this.hero = hero);
  }
 
  save(): void {
    this.heroService.update(this.hero)
      .then(() => this.goBack());
  }
 
  goBack(): void {
    this.location.back();
  }
}
```

#### hero-detail.component.html
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
  <button (click)="save()">Save</button>
</div>
```

#### hero.service.ts
```javascript
import { Injectable }    from '@angular/core';
import { Headers, Http } from '@angular/http';
 
import 'rxjs/add/operator/toPromise';
 
import { Hero } from './hero';
 
@Injectable()
export class HeroService {
 
  private headers = new Headers({'Content-Type': 'application/json'});
  private heroesUrl = 'api/heroes';  // URL to web api
 
  constructor(private http: Http) { }
 
  getHeroes(): Promise<Hero[]> {
    return this.http.get(this.heroesUrl)
               .toPromise()
               .then(response => response.json().data as Hero[])
               .catch(this.handleError);
  }
 
 
  getHero(id: number): Promise<Hero> {
    const url = `${this.heroesUrl}/${id}`;
    return this.http.get(url)
      .toPromise()
      .then(response => response.json().data as Hero)
      .catch(this.handleError);
  }
 
  delete(id: number): Promise<void> {
    const url = `${this.heroesUrl}/${id}`;
    return this.http.delete(url, {headers: this.headers})
      .toPromise()
      .then(() => null)
      .catch(this.handleError);
  }
 
  create(name: string): Promise<Hero> {
    return this.http
      .post(this.heroesUrl, JSON.stringify({name: name}), {headers: this.headers})
      .toPromise()
      .then(res => res.json().data as Hero)
      .catch(this.handleError);
  }
 
  update(hero: Hero): Promise<Hero> {
    const url = `${this.heroesUrl}/${hero.id}`;
    return this.http
      .put(url, JSON.stringify(hero), {headers: this.headers})
      .toPromise()
      .then(() => hero)
      .catch(this.handleError);
  }
 
  private handleError(error: any): Promise<any> {
    console.error('An error occurred', error); // for demo purposes only
    return Promise.reject(error.message || error);
  }
}
```

#### in-memory-data.service.ts
```javascript
import { InMemoryDbService } from 'angular-in-memory-web-api';
export class InMemoryDataService implements InMemoryDbService {
  createDb() {
    const heroes = [
      { id: 0,  name: 'Zero' },
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
    return {heroes};
  }
}
```

#### hero-search.service.ts
```javascript
import { Injectable } from '@angular/core';
import { Http }       from '@angular/http';
 
import { Observable }     from 'rxjs/Observable';
import 'rxjs/add/operator/map';
 
import { Hero }           from './hero';
 
@Injectable()
export class HeroSearchService {
 
  constructor(private http: Http) {}
 
  search(term: string): Observable<Hero[]> {
    return this.http
               .get(`api/heroes/?name=${term}`)
               .map(response => response.json().data as Hero[]);
  }
}
```

#### hero-search.component.ts
```javascript
import { Component, OnInit } from '@angular/core';
import { Router }            from '@angular/router';
 
import { Observable }        from 'rxjs/Observable';
import { Subject }           from 'rxjs/Subject';
 
// Observable class extensions
import 'rxjs/add/observable/of';
 
// Observable operators
import 'rxjs/add/operator/catch';
import 'rxjs/add/operator/debounceTime';
import 'rxjs/add/operator/distinctUntilChanged';
 
import { HeroSearchService } from './hero-search.service';
import { Hero } from './hero';
 
@Component({
  selector: 'hero-search',
  templateUrl: './hero-search.component.html',
  styleUrls: [ './hero-search.component.css' ],
  providers: [HeroSearchService]
})
export class HeroSearchComponent implements OnInit {
  heroes: Observable<Hero[]>;
  private searchTerms = new Subject<string>();
 
  constructor(
    private heroSearchService: HeroSearchService,
    private router: Router) {}
 
  // Push a search term into the observable stream.
  search(term: string): void {
    this.searchTerms.next(term);
  }
 
  ngOnInit(): void {
    this.heroes = this.searchTerms
      .debounceTime(300)        // wait 300ms after each keystroke before considering the term
      .distinctUntilChanged()   // ignore if next search term is same as previous
      .switchMap(term => term   // switch to new observable each time the term changes
        // return the http search observable
        ? this.heroSearchService.search(term)
        // or the observable of empty heroes if there was no search term
        : Observable.of<Hero[]>([]))
      .catch(error => {
        // TODO: add real error handling
        console.log(error);
        return Observable.of<Hero[]>([]);
      });
  }
 
  gotoDetail(hero: Hero): void {
    let link = ['/detail', hero.id];
    this.router.navigate(link);
  }
}
```

#### hero-search.component.html
```html
<div id="search-component">
  <h4>Hero Search</h4>
  <input #searchBox id="search-box" (keyup)="search(searchBox.value)" />
  <div>
    <div *ngFor="let hero of heroes | async"
         (click)="gotoDetail(hero)" class="search-result" >
      {{hero.name}}
    </div>
  </div>
</div>
```

#### hero-search.component.css
```css
.search-result{
  border-bottom: 1px solid gray;
  border-left: 1px solid gray;
  border-right: 1px solid gray;
  width:195px;
  height: 16px;
  padding: 5px;
  background-color: white;
  cursor: pointer;
}
 
.search-result:hover {
  color: #eee;
  background-color: #607D8B;
}
 
#search-box{
  width: 200px;
  height: 20px;
}
```

## Referensi

- https://angular.io/tutorial/toh-pt6

