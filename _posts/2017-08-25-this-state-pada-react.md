---
layout: post
title:  'Menggunakan this.state pada React'
image: '/assets/img/state-react/state-react.png'
date:   2017-08-25 00:05:03
tags:
- Javascript
- ReactJS
description: 'Pada artikel kali ini kita akan belajar mengenai props, untuk melanjutkan jurnal pembelajaran saya mengenai ReactJS sekarang kita akan berkenalan dengan namanya State. Tulisan ini merupakan kelanjutan jurnal catatan saya belajar di codecademy'
categories:
- Programming
serie: Javascript
---

![state react](/assets/img/state-react/state-react.png)

<p style="text-align: center;">React state illustration by <a href="http://chibicode.com/static/images/react-for-designers/react-style-3.png">chibicode.com</a></p>

Yuk kita belajar ReactJS lagi, setelah kemarin saya membahas [props pada ReactJS](/mengenal-props-react) sekarang kita akan berkenalan dengan namanya State. Tulisan ini merupakan kelanjutan jurnal catatan saya belajar di [codecademy](https://www.codecademy.com/learn/react-101).

Aplikasi React sering kali membutuhkan informasi yang dinamis agar bisa di render. Ada 2 cara component untuk menerapkan informasi yang dinamis, yaitu dengan `props` dan `state`. Antara `props` dan `state` setiap nilai yang di gunakan di dalam component harus selalu sama.  

Kemarin kita sudah panjang lebar membahas `props` saatnya sekarang kita belajar `state`. `props` dan `state` adalah semua yang kita butuhkan untuk mengatur ecosystem dalam interaksi dengan component React.

## Setting Initial State

Tidak seperti `props`, sebuah component `state` tidak di lewatkan di luar component. Sebuah component dapat menentukan `state` nya sendiri.

Agar sebuah component mempunyai `state`, kita akan memberikan sebuah *state property*. Property ini harus di deklarasikan di dalam method constructor, perhatikan contoh berikut:

```javascript
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = { mood: 'decent' };
  }

  render() {
    return <div></div>;
  }
}

<Example />
```

Apa itu method constructor? `super(props)`? Mari kita bahas satu persatu kode yang tidak familiar ini.

```javascript
constructor(props) {
  super(props);
  this.state = { mood: 'decent' };
}
``` 

`this.state` harus bernilai sebuah object, seperti contoh di atas. Object ini mempresentasikan initial "state" pada setiap component instance. Kita akan membahas ini lebih jauh sebentar lagi!

`constructor` dan `super` keduanya merupakan fitur dari ES6, bukan kode unik dari React. Penjelasan lengkap tentang fungsionalitas ini silahkan baca-baca [disini](https://hacks.mozilla.org/2015/07/es6-in-depth-classes/). Penting untuk di catat bahwa component React selalu mempunyai dan memanggil `super` di dalam `constructor` agar program berjalan dengan benar.

Pada baris terakhir kode, `<Example />` merupakan sebuah `state`, dan `state` tersebut sama dengan `{ mood: 'decent' }`.

Pastikan method `constructor` dan `render` tidak di pisahkan dengan koma! Method jangan di pisahkan dengan koma jika berada di dalam body class. Ini untuk menekankan fakta bahwa antara class dan object adalah berbeda.

## Mengakses Component state

Untuk mengakses component state, gunakan ekspresi `this.state.name-of-property`:

```javascript
class TodayImFeeling extends React.Component {
  constructor(props) {
    super(props);
    this.state = { mood: 'decent' };
  }

  render() {
    return (
      <h1>
        I'm feeling {this.state.mood}!
      </h1>
    );
  }
}
```

Contoh di atas component class membaca property di dalam `state` dari dalam function `render`. Seperti `this.props`, kita bisa menggunakan `this.state` dari semua property yang di definisikan di dalam body component class.

## Update state dengan `this.setstate`

Sebuah component dapat melakukan lebih dari sekedar membaca `state`nya sendiri. Sebuah component juga dapat mengubah `state`nya sendiri. Component merubah `state` dengan memanggil `this.setState`.

`this.setState` mengambil 2 argumen: sebuah object akan mengupdate sebuah component, dan sebuah callback. Kita pada dasarnya tidak membutuhkan callback.

```javascript
import React from 'react';

class Example extends React.Component {
  constructor(props) {
  	super(props);
    this.state = {
      mood:   'great',
      hungry: false
    };
  }

  render() {
    return <div></div>;
  }
}

<Example />
```

Pada kode diatas, `<Example />` mempunyai state sebagai berikut:

```javascript
{
  mood:   'great',
  hungry: false
}
```

Sekarang katakanlah kita akan memanggil `<Example />`:

```javascript
this.setState({ hungry: true });
```

Setelah di jalankan kode diatas, maka state `<Example />` akan menjadi seperti ini:

```javascript
{
  mood:   'great',
  hungry: true
}
```

`this.setState` mengambil sebuah object, dan menggabungkan object dengan component `state` tersebut.

## Memanggil `this.setState` dari function lain

Cara yang paling umum untuk memanggil `this.setState` adalah dengan memanggil custom function yang membungkus panggilan `this.setState`. Function `makeSomeFog` dibawah ini adalah contohnya:

```javascript
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = { weather: 'sunny' };
    this.makeSomeFog = this.makeSomeFog.bind(this);
  }

  makeSomeFog() {
    this.setState({
      weather: 'foggy'
    });
  }
}
```

Perhatikan bagaimana method `makeSomeFog()` berisi pemanggilan `this.setState()`. Dan dan mungkin anda memperhatikan kode berikut:

```javascript
this.makeSomeFog = this.makeSomeFog.bind(this);
```

Baris ini di butuhkan karena body `makeSomeFog()` berisi kata `this`. Kita akan membahasnya sebentar lagi.

Mari kita bahas bagaimana sebuah function membungkus `this.setState()` dengan latihan pada state `<Mood />` berikut ini:

File `Mood.js`:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class Mood extends React.Component {
  constructor(props) {
    super(props);
    this.state = { mood: 'good' };
    this.toggleMood = this.toggleMood.bind(this);
  }

  toggleMood() {
    const newMood = this.state.mood == 'good' ? 'bad' : 'good';
    this.setState({ mood: newMood });
  }

  render() {
    return (
      <div>
        <h1>I'm feeling {this.state.mood}!</h1>
        <button onClick={this.toggleMood}>
          Click Me
        </button>
      </div>
    );
  }
}

ReactDOM.render(<Mood />, document.getElementById('app'));
```

Berikut bagaimana state `<Mood />` di bentuk:

1. User mentriger event dengan mengklik `<button></button>`.

2. Event pada step pertama akan di eksekusi, pada hal ini menggunakan attribute `onClick` pada baris ke 20.

3. Ketika event terjadi, maka akan memanggil *event handler* function, dalam hal ini `this.setState` pada baris 20.

4. Di dalam body dari *event handler*, `this.setState()` di panggil. 

5. Sekarang state component berubah.

Instance `<Mood />` akan menampilkan "I'm feeling good!" pada button. Klik button maka akan berubah menjadi "I'm feeling bad!". Klik lagi maka akan kembali menjadi "I'm feeling good!" dan seterusnya.

Satu catatan penting, kita tidak bisa memanggil `this.setState()` dari dalam function render!

## `this.setState` automatisasi pemanggilan render

Lihat kode berikut pada file yang kita beri nama `Toggle.js` dan `Mood.js`:

File `Toggle.js`:

```javascript
import React from 'react';


const green = '#39D1B4';
const yellow = '#FFD712';

class Toggle extends React.Component {
  render() {
    return (
      <div>
        <h1>
          Change my color
        </h1>
      </div>
    );
  }
}
```

File `Mood.js`:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class Mood extends React.Component {
  constructor(props) {
    super(props);
    this.state = { mood: 'good' };
    this.toggleMood = this.toggleMood.bind(this);
  }

  toggleMood() {
    const newMood = this.state.mood == 'good' ? 'bad' : 'good';
    this.setState({ mood: newMood });
  }

  render() {
    return (
      <div>
        <h1>I'm feeling {this.state.mood}!</h1>
        <button onClick={this.toggleMood}>
          Click Me
        </button>
      </div>
    );
  }
}

ReactDOM.render(<Mood />, document.getElementById('app'));
```

Ketika user mengklik tag `<button></button>` , function `changeColor` di panggil. Lihat pada `changeColor`. `changeColor` memanggil `this.setState`, yang mana mengupdate `this.state.color`. Namun, bahkan jika `this.state.color` merubahnya dari `green` ke `kuning, itu saja tidak membuat warna screen berubah!

Warna screen tidak bisa di rubah sampai `Toggle` di renders. Di dalam render function, pada line berikut:

```javascript
{% raw %}
<div style={{background:this.state.color}}>
{% endraw %}
```

Hal itu merubah virtual DOM warna object kepada `this.state.color` baru, akhirnya menyebabkan perubahan pada screen.

## Kesimpulan Interaksi Component

Dalam tulisan kali ini kita belajar bagaimana menggunakan `import` dan `export` untuk mengakses satu file ke file yang lain. 

Sebuah aplikasi React secara dasar merupakan sebuah kumpulan component yang banyak, mensetting `state` dan melewatkan `props` dari satu ke yang lain. Sekarang dengan mengerti bagaimana menggunakan `props` dan `state`, kita mempunya tools penting yang sangat kita butuhkan dalam membuat aplikasi. 
