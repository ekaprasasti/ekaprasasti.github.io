---
layout: post
title:  'Stateful dan Stateless'
image: '/assets/img/stateful-stateless-react/props-state.jpg'
date:   2017-08-25 00:05:03
tags:
- Javascript
- ReactJS
description: 'Kali ini kita akan belajar mengenai programming pattern dari react menggunakan state, yaitu stateful component dan stateless component.'
categories:
- Programming
serie: Javascript
---

![stateless stateful](/assets/img/stateful-stateless-react/props-state.jpg)

<p style="text-align: center;">Illustration by <a href="https://image.slidesharecdn.com/discoverreact-150603094016-lva1-app6891/95/discover-react-59-638.jpg?cb=1435652856">slideshare.com</a></p>

Kita tau kalau react itu segalanya mengenai component, dan untuk mengakses dan menyimpan alur data pada component kita menggunakan props dan state seperti yang sudah saya tulis di artikel [sebelumnya](/this-state-pada-react) mengenai pembelajaran saya di [codecademy](https://www.codecademy.com/learn/react-101). Masih dari codecademy kali ini kita akan belajar mengenai programming pattern dari react menggunakan state, yaitu stateful component dan stateless component.

Stateful component mendeskripsikan component yang mempunyai property state. Sebaliknya stateless component di istilahkan untuk component yang tidak mempunyai property state.

Programming pattern menggunakan stateful dan stateless component artinya, kita melewatkan atau mempassing state dari stateful component ke stateless component. Untuk mempraktekannya kita akan membuat 2 component yaitu stateful dan stateless component.

## Membuat Stateful Component Class

Buat sebuah class component yang bernama Parent yang kita gunakan sebagai stateful component. Component stateful membutuhkan method constructor.

File `Parent.js`:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { Child } from './Child';

class Parent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { name: 'Frarthur' };
  }
  
  render() {
    return <Child name="Frarthur" />;
  }
}

ReactDOM.render(
	<Parent />,
  document.getElementById('app')
);
```

Kode diatas kita menerima kode dari file `Child.js` sebagai stateless component dengan kode `import { Child } from './Child';` dan mengirimkan sebuah state dengan melewatkan props pada sebuah component instance `<Child name="Frathur" />`. Dan merender component child pada stateful component yaitu `<Parent />`.

## Membuat Stateless Component Class

Berikut kode dari stateless component `<Child />`.

File `Child.js`:

```javascript
import React from 'react';

export class Child extends React.Component {
  render() {
    return <h1>Hey, my name is {this.props.name}!</h1>;
  }
}
```

Kita lihat pada kode di atas kita mengakses state dari component stateless `<Parent />` dengan menggunakan props. Setelah itu kode pada method render di tampilkan pada component `<Parent />`. 

## Jangan Mengupdate Props

Sebelumya kita telah belajar bagaimana component merubah state dengan `this.setState()`. Lalu bagaimana dengan props? bisakah sebuah component merubah props? Jawabannya adalah tidak bisa!

Props dan state sama-sama menyimpan informasi dinamis, lalu di mana perbedaannya?

- Props menyimpan informasi dinamis yang bisa di rubah di component yang berbeda. 
- State juga menyimpan informasi dinamis tetapi kita bisa merubahnya di componentnya sendiri.
