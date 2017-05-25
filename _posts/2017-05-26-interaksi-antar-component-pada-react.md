---
layout: post
title:  'Interaksi Antar Component pada React'
image: ''
date:   2017-05-26 00:05:03
tags:
- javascript
- ReactJS
description: ''
categories:
- Programming
serie: javascript
---

Melanjutkan artikel [sebelumnya](http://ekaprasasti.com/dasar-react-component/) mengenai dasar react component dan juga catatan pembelajaran saya di [codecademy](https://www.codecademy.com/learn/react-101), kali ini kita akan membahas bagaimana component dalam aplikasi react saling berinteraksi satu sama lain.

Aplikasi React dapat berisi puluhan bahkan ratusan component. Setiap component merupakan bagian dari kode kecil yang mungkin tidak berpengaruh terhadap sistem jika sendirian. Namun bila di kombinasikan dengan component lain, mereka dapat membentuk ekosistem informasi yang sangat kompleks dan fantastis.

Dengan kata lain, aplikasi react di buat menggunakan component, tapi yang membuat React spesial bukan dari component itu sendiri. Yang membuat react spesial adalah ketika dimana component-component tersebut berinteraksi.

Pada pembahasan kali ini kita akan membahas bagaimana sebuah component berinteraksi dengan component lain (components interacting).

## Component pada function render

Kode berikut adalah sebuah function `render` yang me-return sebuah HTML menggunakan elemen JSX:

```javascript
var Example = React.createClass({
    render: function () {
        return <h1>Hello world</h1>;
    }
});
```

Function `render` bisa juga me-return JSX pada function `render` lain menggunakan *component instances*. Perhatikan contoh kode berikut.

```javascript
var OMG = React.createClass({
    render: function () {
        return <h1>Whooaa!</h1>;
    }
});

var Crazy = React.createClass({
    render: function () {
        return <OMG />;
    }
});
```

Pada contoh di atas, variabel `Crazy` me-render function yang me-return instance dari component class `OMG`. Atau dengan kata lain yang lebih sederhana, `Crazy` me-render `<OMG />`.

## Component merender component lainnya

Pada pembahaasan sebelumnya kita sudah mempelajari bagamana sebuah component di render menggunakan `ReactDOM.render`. Ketika sebuah component merender component lainnya, yang terjadi adalah sama seperti `ReactDOM.render` merender sebuah component.

Perhatikan 2 file kode berikut,

File `ProfilePage.js`

```javascript
var React = require('react');
var ReactDOM = require('react-dom');


var ProfilePage = React.createClass({
    render: function () {
        return (
            <div>
                <NavBar />
                <h1>All About Me!</h1>
                <p>I like movies and blah blah blah blah blah</p>
                <img src="https://s3.amazonaws.com/codecademy-content/courses/React/react_photo-monkeyselfie.jpg" />
            </div>
        );
    }
});
```

File `NavBar.js`

```javascript
var React = require('react');

var NavBar = React.createClass({
  render: function () {
    var pages = ['home', 'blog', 'pics', 'bio', 'art', 'shop', 'about', 'contact'];
    var navLinks = pages.map(function(page){
      return (
        <a href={'/' + page}>
          {page}
        </a>
      );
    });

    return <nav>{navLinks}</nav>;
  }
});
```

Kita lihat pada file `ProfilePage.js` pada line 9, component class ProfilePage merender sebuah instance dari component class `<NavBar />` pada file `NavBar.js`.

## Require file

Setiap kita menggunakan React, setiap file javascript di dalam aplikasi secara default tidak dapat terlihat pada file javascript lainnya. Pada contoh diatas, file `ProfilePage.js` dan file `NavBar.js` tidak bisa melihat kode satu sama lain. Nah di sinilah permasalahannya.

Pada baris 9, kita memasukan instance `<NavBar />` pada component class, tetapi karena kita berada di file ProfilePage.js, javascript tidak dapat menampilkan `<NavBar />`. Jika kita ingin menggunakan sebuah variabel yang di declare pada file yang berbeda seperti NavBar.js, maka kita harus mengimport file yang kita inginkan.

Untuk mengimport file, kita bisa menggukan `require`, dan mempassing path file, lihat contoh di bawah ini.

```javascript
var NavBar = require('./NavBar.js');
```

Kita sudah menggunakan keyword `require` sebelumnya, tapi berbeda kali ini. Jika kita mem-passing `require` sebuah string yang di mulai dengan salah satu dot atau slash, maka `require` akan memperlakukannya sebagai sebuah path file. `require` akan mencari path file, dan akan mengimport jika menemukan file tersebut.

## Module.exports

Ok kita sudah belajar bagaimana caranya mengimport file lain di dalam kode kita. Tapi tentu kita tidak ingin mengimport keseluruhan file, kita hanya ingin mengimport component class `NavBar`, sehingga kita bisa merender <NavBar /> instance. Yang kita butuh kan adalah mengimport hanya bagian yang spesifik pada sebuah file.

Jawabannya adalah dengan menggunakan `module.exports`. `module.exports` berasal dari [Node.js module system](https://www.sitepoint.com/understanding-module-exports-exports-node-js/) sama seperti halnya dengan `require`. Artinya `module.exports` dan `require` akan di gunakan bersama-sama, dan kita pada dasarnya tidak pernah melihat satu tanpa yang lainnya.

```javascript
// Manifestos.js:
var faveManifestos = {
    futurist: 'http://bit.ly/1lKuB2I',
    SCUM: 'http://bit.ly/1xWjvYc',
    cyborg: 'http://bit.ly/16sbeoI'
};

module.exports = faveManifestos;
```

Pada file berbeda, gunakan `require` untuk mengimport file sebelumnya. `require` tidak hanya me-return keseluruhan file, ia hanya akan me-return nilai pada `module.exports`.

```javascript
// App.js:
// Link to the Futurist Manifesto:

console.log(require('./Manifestos').futurist);
```
