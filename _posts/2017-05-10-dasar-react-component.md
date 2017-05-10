---
layout: post
title:  'Dasar React Component'
image: ''
date:   2017-05-10 00:05:03
tags:
- javascript
- ReactJS
description: ''
categories:
- Programming
serie: javascript
---

![ReactJS Component](/assets/img/dasar-react-component/react-component.jpg)

<p style="text-align: center;">React component illustration by <a href="https://www.kirupa.com/react/images/c_c_c_c_c_c_144.png">kirupa.com</a></p>

Melanjutkan catatan saya [sebelumnya](http://ekaprasasti.com/memulai-reactjs-dan-dasar-jsx) mengenai pembelajaran ReactJS di [codecademy.com](https://www.codecademy.com/learn/react-101)
kali ini kita akan belajar mengenai dasar dari react component. Tidak hanya mengenai react component, tapi kita juga akan sedikit mereview apa yang sudah kita pelajari di tulisan saya [sebelumnya](http://ekaprasasti.com/memulai-reactjs-dan-dasar-jsx).

Sebelumnya apa sih react component itu? Jika kita membuat aplikasi dengan react kita pasti selalu menggunakan component. Component adalah potongan kode kecil yang dapat di gunakan kembali (reusable) yang bertujuan agar user interface terpisah menjadi bagian-bagian kecil dan di satukan dan di render menjadi sebuah kode HTML.

## Component Class

Ok sekarang kita akan langsung masuk ke contoh koding untuk membuat component class. Setiap component selalu di awali dengan component class, yang merupakan sebuah method yang dapat membuat component pada react. Untuk membuat component react kita menggunakan `React.createClass`. Perhatikan kode berikut.

```javascript
var React = require('react');
var ReactDOM = require('react-dom');

var MyComponentClass = React.createClass({
    render: function () {
        return <h1>Hello world</h1>;
    }
});

ReactDOM.render(
    <MyComponentClass />,
    document.getElementById('app')
);
```

Kode diatas adalah contoh sederhana dalam membuat component pada react dengan method `React.createClass` yang di simpan dalam sebuah variabel `MyComponentClass`. Tetapi sebelum kita membahas lebih jauh mengenai component class ini, untuk mereview tulisan [sebelumnya](http://ekaprasasti.com/memulai-reactjs-dan-dasar-jsx) saya juga akan menjelaskan struktur dan sintaks lain pada kode diatas.

### Import React

Pada baris kode pertama `var React = require('react');` akan mengembalikan sebuah object javascript yang di simpan dalam sebuah variabel yang dinamakan `React`, object ini berisi beberapa method yang dapat di gunakan pada project react kita.

```javascript
var React = require('react');
```

Pengembalian object ini disebut dengan React *library*. Pada tulisan saya [sebelumnya](http://ekaprasasti.com/memulai-reactjs-dan-dasar-jsx), kita sudah belajar bagaimana menggunakan React *library* seperti `React.createElement`. Yang memanggil elemen JSX untuk di compile. Untuk alasan ini kita harus me-*require* React *library* dan menyimpannya dalam sebuah variabel yang dinamakan `React`.

### Import ReactDOM

```javascript
var ReactDOM = require('react-dom');
```

Sama seperti `require('react')`, `require('react-dom')` juga mengembalikan sebuah object javascript. Namun keduanya berisi react method yang berbeda, `require('react-dom')` di khsuskan untuk berinteraksi dengan DOM, pada kode di atas kita menggunakan method `ReactDOM.render`.

### Render Component

```javascript
ReactDOM.render(
    <MyComponentClass />,
    document.getElementById('app')
);
```

Pada kode diatas kita merender component react yang sudah kita buat. Untuk membuat memanggil `render` function pada sebuah component, passing component tersebut kepada method `ReactDOM.render`. `ReactDOM.render` akan menampilkan `<h1>Hello world</h1>` yang di `return` pada component class ke sebuah virtual DOM, dan akan terlihat pada browser `Hello world`.

## Membuat Component Class

Untuk membuat component class, kita gunakan salah satu methods dalam *library* React, `React.createClass`. Method `React.createClass` adalah merupakan salah satu method dari object yang di definisikan dari `require(‘react’)` pada baris pertama.

```javascript
var MyComponentClass = React.createClass({
    render: function () {
        return <h1>Hello world</h1>;
    }
});
```

### Component Class Naming Convesion

Ketika kita membuat sebuah component class baru, kita harus menyimpannya pada sebuah variabel, sehingga kita dapat menggunakan kembali component class tersebut. Pada contoh kode di atas kita membuat component class yang di simpan pada variabel yang kita namai `MyComponentClass`.

```javascript
var MyComponentClass = React.createClass();
```
Nama variabel pada component class harus menggunakan huruf besar pada huruf di kata pertama. Ini merupakan *naming convention* pada react yang harus kita ikuti. Nama class harus di tulis dengan cara UpperCamelCase.

### Instruksi Component Class

`React.createClass` akan mengesekusi sebuah argument. Yang mana argument tersebut harus merupakan sebuah javascript object. Object ini akan berperan sebagai instruksi dan menjelaskan kepada component class bagaimana membuat react component. Pada contoh `React.createClass` akan mengambil nilai sebuah JSX. Berikut adalah object pada contoh kode di atas.

```javascript
{
   render: function () {
     return <h1>Hello world</h1>;
   }
}
```

#### Render Function

Sekarang saya akan menjelaskan instruksi apa yang sebenarnya di kirimkan oleh `React.createClass`. Pertama, instruksi ini harus di simpan sebagai object. Untuk contoh kita akan membuat sebuah variabel yang berisi object.

```javascript
var instruction = {};
```
Hanya ada satu property yang bisa kita masukan pada object tersebut. yaitu render function. Render function adalah sebuah properti yang dapat di namai `render` dan nilainya adalah sebuah function. Istilah “render function" dapat merujuk ke keseluruhan properti, atau hanya bagian fungsi.

```javascript
var instructions = {
    render: function() {}
}
```

Untuk dapat merender, sebuah function harus mempunyai sebuah `return` statement. Biasanya statement ini me-*return* ekspressi JSX.

```javascript
var instructions = {
    render: function() {
        return <h1>hello world<h1>;
    }
}
```

#### Mendifinisikan Instruksi pada sebuah Function

Menyimpan instruksi object ke dalam sebuah variabel seperti contoh di atas adalah cara yang baik untuk mengilustrasikan bagaimana ia bekerja, namun pada prakteknya itu sangat jarang terjadi. Biasanya kita langsung memasukan object di dalam method `React.createClass`.

### Membuat Component Instance

`MyComponentClass` sekarang sudah bekerja sebagai component class dan dapat di gunakan untuk membuat sebuah react component. Sebuah instance component pada react dibuat dengan hanya satu baris:

```javascript
<MyComponentClass />
```

Untuk membuat sebuah react component, kita dapat menuliskan dalam format elemen JSX. Elemen JSX bisa berupa HTML atau component instance. JSX menggunakan kapitalisasi untuk membedakan keduanya. Itulah sebabnya nama class component harus di awali dengan huruf besar. Dalam elemen JSX, huruf pertama yang di kapitalisasi itu artinya merupakan component instance bukan tag HTML.

## JSX pada React Component

Nah sekarang kita sudah bisa membuat sebuah component pada react. Component react biasanya me-*return* kode JSX, aturan penulisan kode JSX ini pun sama seperti yang sudah saya jelaskan pada tulisan [sebelumnya](http://ekaprasasti.com/memulai-reactjs-dan-dasar-jsx). Tapi ada beberapa hal penting yang juga mengikuti aturan penulisan pada component react.

### Operasi Logika pada Render Function

Function `render` harus mempunya sebuah `return` statement. Namun, tidak hanya itu kemampuannya. Function `render` juga dapat di jadikan tempat untuk menaruh kode kalkulasi sederhana ataupun sebuah proses logika. Lihat dua contoh kode berikut.

```javascript
var Random = React.createClass({
    render: function () {

        // First, some logic that must happen
        // before rendering:
        var n = Math.floor(Math.random()*10+1);

         // Next, a return statement
        // using that logic:
        return <h1>The number is {n}!</h1>;
    }
});
```

```javascript
var task;
if (!apocalypse) {
    task = "learn React.js"
} else {
    task = "run around"
}

return <h1>Today I am going to {task}!</h1>;
```

### Menggunakan Keyword this pada Component

Keyword `this` dapat di gunakan pada React. Kita akan sering melihat `this` di dalam objet yang di *pass* pada `React.createClass`. Perhatikan contoh berikut.

```javascript
var IceCreamGuy = React.createClass({
    food: 'ice cream',

    render: function () {
        return <h1>I like {this.food}.</h1>;
    }
});
```

### Event Listener di dalam Component

Function render biasanya juga terdapat *event listener* seperti ini:

```javascript
render: function () {
    return (
        <div onHover={myFunc}> </div>
    );
}
```

Contoh *event handler* diatas adalah sebuah function, pada contoh di atas function *event handler* dinamakan dengan `myFunc`. Dalam React kita mendefinisikan *event handlers* sebagai property value dari sebuah instruksi pada object. Lihat contoh di bawah ini.

```javascript
React.createClass({
    myFunc: function () {
        alert('Stop it. Stop hovering.');
    },

    render: function () {
        return (
            <div onHover={this.myFunc}></div>;
        );
    }
});
```

Pada contoh di atas object pada `React.createClass` mempunyai dua properti yaitu `myFunc` dan `render`. `myFunc` di gunakan sebagai event handler. `myFunc` akan di panggil setiap kali user hovers diatas `<div></div>` yang di render.

## Penutup

Ok, sampai sini dulu ya catatan pembelajaran saya mengenai ReactJS di [codecademy.com](https://www.codecademy.com/learn/react-101). Semoga juga bisa bermanfaat buat temen-temen programmer. Buat yang mau belajar bareng dan diskusi boleh langsung tinggalin komentarnya di bawah.
