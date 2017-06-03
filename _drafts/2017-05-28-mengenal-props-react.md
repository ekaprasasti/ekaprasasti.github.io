---
layout: post
title:  'Pengenalan Props pada React'
image: ''
date:   2017-05-28 00:05:03
tags:
- javascript
- ReactJS
description: ''
categories:
- Programming
serie: javascript
---

Pada artikel kali ini kita akan belajar mengenai props, untuk melanjutkan jurnal pembelajaran saya mengenai ReactJS di [codecademy](https://www.codecademy.com/learn/react-101). Latihan kita kali ini adalah “Interaksi Component”. Kita akan belajar cara agar component dapat saling berinteraksi, component lain dapat menyampaikan informasi ke component lain.

Informasi yang di lewatkan (passed) dari satu component ke component lainya di sebut sebagai “props”.

## Mengakses Component's props

Setiap component mempunyai sesuatu yang di sebut dengan `props`. Sebuah `props` pada component adalah object. Yang menyimpan informasi tentang component tersebut.

Untuk mengakses component `props` object, kita menggunakan sebuah ekspresi yang di sebut `this.props`. Di bawah ini adalah contoh dari `this.props` yang di gunakan di dalam render function.

```javascript
render: function () {
    console.log(“Props object comin up!”);

    console.log(this.props);

    console.log(“That was my props object”);

    return <h1>Hello world</h1>;
}
```
