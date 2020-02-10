---
layout: post
title:  'ES6: Arti Tanda 3 Titik (Spread Syntax Notation)'
image: '/assets/img/spread-syntax-notation/es6.jpeg'
date:   2020-02-10 00:00:00
tags:
- Javascript
- ReactJS
description: 'tahukah kamu maksud dari syntax `…newState` ? Syntax inilah yang dalam ES6 di namakan dengan Spread Syntax Notation.'
categories:
- My Daily Bug
---

![es6](/assets/img/spread-syntax-notation/es6.jpeg)

Saya akan membahas salah satu fitur penulisan kode atau syntax pada javascript ES6. Paling banyak kita temui ketika belajar react menggunakan ES6 adalah model syntax ketika mengoveride nilai dari sebuah state menggunakan setState seperti ini:

```javascript
this.state = {};

const newState = {
  name: 'Eka Prasasti',
  role: 'Software Engineer',
}

this.setState({
  …newState,
  jobdesk: 'Feature development',
});

// sehingga nilai dari this.state menjadi:
// {name: ‘Eka Prasasti’, role: ’Software Engineer’, jobdesk: ‘Feature development`}
```

Nah tahukah kamu maksud dari syntax `…newState` ? Syntax inilah yang dalam ES6 di namakan dengan Spread Syntax Notation.

> Spread syntax allows an iterable such as an array expression or string to be expanded in places where zero or more arguments (for function calls) or elements (for array literals) are expected, or an object expression to be expanded in places where zero or more key-value pairs (for object literals) are expected. - [developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

Jadi tanda 3 titik di depan sebuah nilai dari variable adalah iterasi atau pengulangan yang di sematkan pada sebuah variable atau nilai baru. Contoh sederhananya adalah pada operasi array berikut ini.

```javascript
const myArray = [1, 2, 3]
console.log(myArray) // output: [1, 2, 3]

const newArray = […myarray, 4, 5, 6]
console.log(newArray) // output: [1, 2, 3, 4, 5, 6]
```

Bagaimana sudah lebih jelas? Kita kembali di state pada react di awal. Syntax `…newState` bisa kita artikan juga sebagai berikut:

```javascript
const newState = {
  name: 'Eka Prasasti',
  role: 'Software Engineer',
}

this.setState({
  …newState,
  jobdesk: 'Feature development',
});

// code setState di atas penulisannya sama seperti ini.
this.setState({
  name: 'Eka Prasast',
  role: 'Software Engineer',
  jobdesk: 'Feature development',
});
```

Dengan spread syntax penulisan kode menjadi lebih singkat dan efisien, penulisan kode teknik seperti ini juga banyak kita temua pada implementasi reducer di state management seperti redux. Kita perlu me override kembali global state dan di gabung dengan nilai yang baru sehingga nilai yang lama tidak hilang, sehingga jika kita punya banyak global state kita tidak perlu menuliskan kembali state tersebut dari awal dalam reducer. Penjelasan mengenai redux dan reducer akan kita bahas di artikel mendatang. 