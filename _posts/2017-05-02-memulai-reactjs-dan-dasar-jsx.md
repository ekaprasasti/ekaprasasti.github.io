---
layout: post
title:  'Memulai ReactJS & dasar JSX'
image: ''
date:   2017-05-02 00:05:03
tags:
- Javascript
- ReactJS
description: ''
categories:
- Programming
serie: Javascript
---

![ReactJS JSX](/assets/img/memulai-reactjs-dan-dasar-jsx/react-jsx.jpg)

Sebelumnya saya mau cerita dulu nih kenapa jadi kepengen belajar ReactJS. Awalnya untuk framework frontend saya menggunakan AngularJS versi 1, kira-kira sudah setahun ini saya pakai semenjak develop aplikasi web [erabelajar.com](http://erabelajar.com) yang pake teknologi MEAN Stack. Waktu itu sebenernya sudah ada Angular versi 2 tapi saat itu masih release beta (padahal sekarang udah versi 4 aja ya. hehehee), jadi dari pada belum stabil dan dokumentasinya berubah-ubah saya putuskan pake AngularJS versi 1.

## Kenapa ReactJS

Jadi gini beberapa waktu lalu saya sempet belajar Angular 2, ya karena versinya sudah stable pastinya dan saya memang sudah tau kalo Angular versi 1 dan 2 ini bagai bumi dan langit, jauh banget beda nya. Ya bisa di bilang walaupun udah bisa dan paham AngularJS 1 ya harus belajar dari ulang dan dari basic lagi kalo mau bisa Angular 2. Setelah belajar saya menemukan beberapa kendala, yang paling berasa adalah Angular 2 pake bahasa pemrograman Typescript, yaitu pengembangan dari Javascript buatan microsoft, dan typescript ini membutuhkan sebuah compiler yang menjadikan kode typescript berubah menjadi kode javascript agar terbaca oleh browser. Maka dari itu Angular 2 butuh yang namanya CLI. Ini yang membuat saya agak gimana gitu, ya kan jadi dobel belajarnya, Typescript juga Angular juga. Masalahnya Typescript itu menurut saya secara syntaks berbeda dari Javascript dan butuh pemahaman khusus mengenai syntaks nya.

Setelah cari sana sini, akhirnya saya menemukan library [ReactJS](https://facebook.github.io/react/) (sebenernya udah tau lama sih, hehe), yang memang kayanya lagi hype banget di Indonesia juga udah lumayan banyak yang pake, awalnya saya ikut group telegram React Native ID, yang kayanya lagi rame banget, terus saya nonton webinar dari codepolitan bahas masalah ReactJS sama curiculum directornya Hactive8, mas [Riza Fahmi](https://twitter.com/rizafahmi22). Sekilas memang mirip sama Angular 2 karena mindsetnya sama-sama component, tapi beda Angular 2 itu adalah framework sedangkan kalo ReactJS itu adalah library. Dan yang pasti ReactJS pakai bahasa pemrogrammanya pure Javascript.

Pertama kali belajar saya langsung nyoba di [codecademy.com](https://www.codecademy.com/learn/react-101), di situ memang saya biasanya belajar kalo mau nyoba-nyoba teknologi baru atau kalo ada framework dan bahasa pemrograman terbaru, karena simpel gak perlu buka dokumentasi official nya, lewat codecademy kita sudah bisa melihat gambaran besarnya. Nah artikel ini kira-kira bisa jadi seperti rangkuman hasil pembelajaran saya di codecademy maupun di tempat-tempat lain.

Pertama dari beberapa keunggulan dari ReactJS,

- ReactJS adalah library javascript yang sangat cepat. Aplikasi yang di buat menggunakan ReactJS dapat menangani perubahan tampilan yang kompleks dan sangat cepat, serta responsive.
- ReactJS menggunakan konsep modular. Untuk membuat aplikasi dengan skala besar, kita dapat menulis kode-kode dengan skala yang lebih kecil untuk di satukan menjadi aplikasi utuh, dan dapat di gunakan kembali (reusable).
- ReactJS sangat scalable, artinya ReactJS dapat menangani dengan sangat baik sebuah program dengan skala yang besar yang dapat menampilkan perubahan data yang sangat kompleks.
- ReactJS sangat fleksibel. Dengan belajar 1 libary saja kita dapat membuat aplikasi Web, Moblie, maupun Desktop.
- ReactJS merupakan library yang sangat populer. Seperti yang sudah saya katakan sebelumnya di Indonesia sendiri sudah lumayan banyak developer dan website-website serta aplikasi yang menggunakan ReactJS.

## Apa itu JSX

Jika kita lihat definisi pada wikipedia, bisa kita dapat definisi JSX adalah seperti ini

> React components are typically written in JSX, a JavaScript extension syntax allowing quoting of HTML and using HTML tag syntax to render subcomponents. This is a React-specific grammar extension to JavaScript like now defunct. HTML syntax is processed into JavaScript calls of the React framework. Developers may also write in pure JavaScript.

Nah coba sekarang kita bahas lebih lanjut langsung ke contoh kodenya, biar cepet faham. Apa kalian pernah lihat kode javascript seperti ini,

```javascript     
var h1 = <h1>Hello world</h1>;
```

Jika kita jalankan kode seperti ini di file javascript bisa di pastikan program akan langsung error. Kenapa demikian karena pada kode javascript tidak dapat di sisipkan kode HTML, kita tahu bahwa kode `<h1>Hello world</h1>` adalah kode html yang di simpan dalam variabel h1. Baris kode di atas merupakan baris kode JSX.

JSX merupakan sebuah sintaks extension pada javascript, untuk di gunakan pada React. Sintaks kode JSX merupakan kode HTML yang di sematkan pada kode javascript. JSX bukanlah sintaks javascript, sehingga browser tidak bisa membaca sintaks ini, di butuh kan sebuah JSX compiler yang di gunakan untuk menterjemahkan JSX kedalama bahasa regular javascript agar bisa terbaca oleh browser.

> JSX terlihat sama dengan kode HTML, satu-satunya yang membuat berbeda dengan kode HTML adalah bahwa JSX berada di dalam kode javascript

Element JSX dapat di perlakukan seperti ekspresi pada javascript, artinya elemen JSX dapat di simpan dalam variabel, function, object, array, dan lain-lain seperti pada umumnya javascript.

JSX dalam variabel

```javascript
var navBar = <nav>I am a nav bar</nav>;
```

JSX dalam object

```javascript
var Pistons2004 = {
	center: <li>Ben Wallace</li>,
	powerForward: <li>Rasheed Wallace</li>,
	smallForward: <li>Tayshaun Prince</li>,
	shootingGuard: <li>Richard Hamilton</li>,
	pointGuard: <li>Chauncey Billups</li>
};
```

JSX dapat mempunyai attribute sama seperti pada umumnya tag HTML

```javascript
var title = <h1 id="title">Introduction to React.js: Part I</h1>;
```

Sama seperti HTML, JSX juga dapat menyimpan nested element (element di dalam element)

```javascript
var myDiv = (
	<div>
		<h1>Hello world</h1>
	</div>
);
```

Jika JSX terdapat lebih dari 1 baris maka harus di beri tanda kurung, nested JSX juga harus memiliki tag terluar, bisa di contohkan dengan sintaks JSX yang salah seperti ini,

```javascript
var paragraphs = (
	<p>I am a paragraph.</p>
	<p>I, too, am a paragraph.</p>
);
```

Kode diatas akan error karena JSX tidak memiliki tag terluar, solusinya adalah selalu memberikan tag div di awal dan di akhir untuk membungkus sintaks JSX

```javascript
var paragraphs = (
	<div>
		<p>I am a paragraph.</p>
		<p>I, too, am a paragraph.</p>
	</div>
);
```

## Rendering JSX

Merender JSX artinya menampilkan output kode JSX ke browser. Dalam kasus ini kita menggunakan library ReactDOM, berikut adalah contoh sintaks nya

```javascript
var React = require('react');
var ReactDOM = require('react-dom');

ReactDOM.render(<h1>Hello world</h1>, document.getElementById('app'));
```

[ReactDOM](https://facebook.github.io/react/docs/react-dom.html) adalah nama dari library javascript yang terdapat beberapa method yang dapat berkomunikasi dengan DOM (Document Object Model). Dalam contoh ini kita menggunakan method ReactDOM.render, di mana parameter pertama adalah tag html atau JSX yang akan kita render dan yang kedua adalah di element apa pada file html kita akan merubahnya melalui DOM.

Sebuah JSX dapat di simpan ke sebuah variabel dalam javascript, sehingga kita bisa mensematkan sebuah variabel dalam parameter pertama method ReactDOM.render untuk menampilkan JSX pada isi variabel tersebut, perhatikan contoh berikut

```javascript
var React = require('react');
var ReactDOM = require('react-dom');

var myList = (
	<ul>
		<li>
			Eka
		</li>
		<li>
			Prasasti
		</li>
	</ul>
);

ReactDOM.render(myList, document.getElementById('app'));
```

## JSX lebih lanjut

### Atribute class

Penggunaan nama atribute *class* dalam JSX di bedakan menjadi *className*, ini di karenakan keyword *class* adalah reserved word dalam javascript, artinya kata ini di larang pemakaiannya dalam penggunaan nama variabel atau data dalam javascript. Contohnya bisa kita lihat dalam kode berikut ini,

```javascript
<h1 className="big">Hey</h1>
```

Ketika JSX telah di render, atribute *className* secara otomatis akan terender sebagai atribute *class*.

### Penggunaan self-closing tag

Element HTML yang paling sering di jumpai menggunakan dua tag, yaitu tag pembuka (`<div>`) dan tag penutup (`</div>`). Namun pada beberapa element seperti `<img>` dan `<input>`, tag seperti ini hanya menggunakan satu tag, tidak ada tag penutup atau biasa yang di sebut *self-closing tag*.

Walaupun dalam HTML kita tidak di wajib kan menutup tag seperti ini, ketika kita menuliskan tag HTML self-closing tag dalam JSX, tag wajib di beri tanda slash di penghujung tag.

```javascript
var profile = (
	<div>
		<h1>I AM JENKINS</h1>
		<img src="images/jenkins.png" />
		<article>
			I LIKE TO SIT
			<br />
			JENKINS IS MY NAME
			<br />
			THANKS HA LOT
		</article>
	</div>
);
```

### JSX expression

Kode JSX dapat kita sisipkan sebuah kode regular javascript, perhatikan contoh berikut

```javascript
var React = require('react');
var ReactDOM = require('react-dom');

ReactDOM.render(
	<h1>{2 + 3}</h1>,
	document.getElementById('app')
);
```

Jika kita melihat kode di atas, kira-kira akan tampil seperti apa ya di browser? Yups, akan tampil angka 5, kenapa begitu? karena `2 + 3` adalah sama dengan 5, dan kita menggunakan kurung kurawal `{}` agar JSX mengerti kode yang berada dalam kurung kurawal tersebut akan di berlakukan seperti kode javascript. Apa jadinya jika tidak memakai kurung kurawal? Yups, kalian bener lagi! yang tampil di browser kita adalah apa adanya pada JSX yaitu `2 + 3`.

### Akses variabel pada kode JSX

Ketika kita inject javascript pada kode JSX, maka javascript adalah kode yang juga berada dalam file tersebut. Ini artinya kita juga bisa menyisipkan sebuah variabel pada kode JSX. Perhatikan contoh berikut,

```javascript
// Deklarasikan sebuah variabel:
var name =  ‘Eka Prasasti’;

// Akses variable
// dari dalam JSX expression:
var greeting = <p>Hello, {name}!</p>;
```

Lihat apa yang akan tampil pada browser, ya akan tampil Hello, Eka Prasasti! Ini di karenakan variabel `name` sudah kita sisipkan data Eka Prasasti.

Sebuah variabel juga dapat di sisipkan pada nilai sebuah attribute, contoh

```javascript
// Gunakan variabel untuk memberi nilai atribute height dan width pada JSX berikut:

var sideLength = "200px”;

var panda = (
    <img
        src="images/panda.jpg"
        alt="panda"
        height={sideLength}
        width={sideLength} />
);  
```  

### Event listener pada JSX

Seperti pada kode HTML, element pada JSX juga dapat di sisipkan sebuah event.

```javascript
function myFunc () {
    alert('Make myFunc the pFunc... omg that was horrible i am so sorry’);
}

<img onClick={myFunc} />
```

Pada kode diatas, function myFunc() akan di eksekusi setiap kali tag html `<img>` di klik. Ada banyak event listener pada HTML seperti *onclick*, *onmouseover*, dll. Pada JSX event listener pada HTML di tulis dengan camelCase, contohnya *onClick* atau *onMouseOver*.

Kita dapat melihat semua list event listener pada JSX [di sini](http://facebook.github.io/react/docs/events.html#supported-events).

### Penggunaan if statement pada JSX

Statement If pada JSX tidak dapat di lakukan, sebagai gantinya kita dapat menggunakan if dan else di luar JSX pada file yang sama. Lihat contoh berikut ini,

```javascript
var React = require('react');
var ReactDOM = require('react-dom');

if (user.age >= drinkingAge) {
	var message = (
		<h1>Hey, check out this alcoholic beverage!</h1>
	);
}
else {
	var message = (
		<h1>Hey, check out these earrings I got at Claire's!</h1>
	);
}

ReactDOM.render(
	message,
	document.getElementById('app')
);
```

### Ternary operator

Juga sebagai solusi pengganti kondisional if dan else, JSX mendukung apa yang di sebut dengan *ternary operator* yang dapat mendeteksi kondisi *true* atau *false*.

```javascript
var headline = (
	<h1>
		{ age >= drinkingAge ? 'Buy Drink' : 'Do Teen Stuff' }
	</h1>
);
```

Pada contoh di atas apabila *age* lebih dari *drinkingAge* maka berkondisi *true* dan akan mengesekusi `<h1>Buy Drink</h1>`, sebaliknya jika kondisi bernilai *false* maka akan mengesekusi `<h1>Do Teen Stuff</h1>`.

### && operator

Operator kondisi pada JSX juga mengenal operator &&. Perhatikan contoh berikut ini,

```javascript
var tasty = (
	<ul>
		<li>Applesauce</li>
		{ !baby && <li>Pizza</li> }
		{ age > 15 && <li>Brussels Sprouts</li> }
		{ age > 20 && <li>Oysters</li> }
		{ age > 25 && <li>Grappa</li> }
	</ul>
);
```

### Menampilkan isi array dengan .map

Array method .map juga dapat di gunakan pada React untuk menampilkan atau melooping semua isi array kedalam browser.

```javascript
var strings = ['Home', 'Shop', 'About Me’];

var listItems = strings.map(function(string){
    return <li>{string}</li>;
});

<ul>{listItems}</ul>
```

Pada contoh di atas array pada variabel strings akan di looping menggunakan .map yang disimpan pada variabel listItems. Maka jika kita eksekusi listItems akan menampilkan semua isi dari array strings pada browser.

### Menulis React tanpa menggunakan JSX

Kita bisa menulis kode React tanpa menggunakan kode JSX sama sekali. Tetapi kebanyakan dari programmer menggunakan JSX, dan memang di sarankan menggunakan JSX. Semua kembali kepada anda ingin menggunakan yang mana.

Di bawah ini adalah sebuah kode JSX,

```javascript
var h1 = <h1>Hello world</h1>;
```

Kode di atas dapat di tulis tanpa menggunakan kode JSX seperti ini,

```javascript
var h1 = React.createElement(
	"h1",
	null,
	"Hello, world"
);
```

dengan method React.createElement() kita dapat mengubah kode menjadi sebuah HTML persis dengan apa yang di lakukan oleh JSX. Untuk dokumentasi lanjut mengenai method React.createElement() dapat di jumpai [di sini](http://facebook.github.io/react/docs/top-level-api.html#react.createelement).

Ok, semoga bermanfaat hasil pembelajaran saya mengenai ReactJS di codecademy. Untuk disikusi lebih lanjut boleh meninggalkan komentar di bawah ya.
