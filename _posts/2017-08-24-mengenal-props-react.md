---
layout: post
title:  'Pengenalan Props pada React'
image: '/assets/img/props-react/props-react.jpg'
date:   2017-08-24 00:05:03
tags:
- javascript
- ReactJS
description: 'Pada artikel kali ini kita akan belajar mengenai props, untuk melanjutkan jurnal pembelajaran saya mengenai ReactJS'
categories:
- Programming
serie: javascript
---

![props react](/assets/img/props-react/props-react.jpg)

<p style="text-align: center;">React props illustration by <a href="https://css-tricks.com/wp-content/uploads/2016/01/image01.jpg">css-tricks.com</a></p>

Pada artikel kali ini kita akan belajar mengenai props, untuk melanjutkan jurnal pembelajaran saya mengenai ReactJS di [codecademy](https://www.codecademy.com/learn/react-101). Latihan kita kali ini adalah “Interaksi Component”. Kita akan belajar cara agar component dapat saling berinteraksi, component lain dapat menyampaikan informasi ke component lain.

Informasi yang di lewatkan (passed) dari satu component ke component lainya di sebut sebagai “props”.

## Mengakses Component's props

Setiap component mempunyai sesuatu yang di sebut dengan `props`. Sebuah `props` pada component adalah object. Yang menyimpan informasi tentang component tersebut.

Untuk mengakses object component `props`, kita menggunakan sebuah ekspresi yang di sebut `this.props`. Di bawah ini adalah contoh dari `this.props` yang di gunakan di dalam render function.

```javascript
render: function () {
    console.log(“Props object comin up!”);

    console.log(this.props);

    console.log(“That was my props object”);

    return <h1>Hello world</h1>;
}
```

Informasi mengenai hasil dari `console.log(this.props)` memang seperti *useless*, tetapi beberapa informasi tersebut ada yang sangat penting. Lebih jauh akan kita lihat pada pembahasan selanjutnya. 

## Melewatkan `props` ke Component

Kita bisa melewatkan informasi ke dalam component React dengan memberikan *attribute* kepada component tersebut:

```html
<MyComponent foo="" />
``` 

Katakanlah kita ingin melewatkan sebuah pesan kepada component "This is some top secret info.". Kita bisa melakukannya dengan cara seperti ini:

```html
<Example message="this is some top secret info." />
```

Untuk melewatkan informasi ke component kita perlu memberi nama dari informasi yang ingin kita lewatkan. Pada contoh di atas kita menamainya dengan `message`. Kita bisa menggunakan nama apa saja yang kita mau.

Jika kita ingin melewatkan informasi yang bukan string misalnya array, maka bungkus infromasi tersebut di antara kurung kurawal

```html
<Greeting myInfo={["top", "secret", "lol"]} />
```

Contoh diatas kita melewatkan beberapa informasi ke pada `<Greeting />`. Contoh lain nilai yang bukan string menggunakan kurung kurawal.

```html
<Greeting name="Frarthur" town="Flundon" age="{2} haunted="{false} />
```

## Merender Component props

Kita akan menampilkan informasi yang di lewatkan pada component. Untuk melakukan hal tersebut kita punya beberapa tahapan:

1. Temukan component class yang menerima informasi tersebut.

2. Include `this.props.nama-informasi` di dalam component class pada method render di statement return.

Lihat contoh berikut, mulai dari sekarang kita akan menggunakan sintaks ES6 pada react:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class Greeting extends React.Component {
  render() {
    return <h1>Hi there, {this.props.firstName}</h1>;
  }
}

ReactDOM.render(
  <Greeting firstName='Groberta' />, 
  document.getElementById('app')
);
```

Pada contoh diatas, kita bisa lihat sebuah informasi di lewatkan pada `<Greeting />` dengan nama `firstName`. Bagaimana caranya menampilkan `firstName` di munculkan pada browser? Dengan menggunakan `this.props.firstName` pada class `Greeting` di method render statement `return`.

## Melewatkan props dari Component ke Component

Hal berguna lain dari `props` adalah kita dapat melewatkan informasi dari suatu component ke component lain yang berbeda. 

Penjelasan mengenai grammer kata `prop` dan `props`:

Jika anda perhatikan kita menggunakan kata `prop` dan `props`. `props` adalah nama dari sebuah object yang menyimpan informasi yang telah di lewatkan. `this.props` mengacu pada object tersebut. Pada saat bersamaan, setiap potongan informasi yang di lewatkan disebut juga dengan `prop`.

Untuk melewatkan `props` dari component ke component yang berbeda perhatikan contoh berikut:

File `Greeting.js`.

```javascript
import React from 'react';

export class Greeting extends React.Component({
	render() {
		return <h1>Hi there, {this.props.name}!</h1>
	}
});
```

Pada contoh di atas kita menggunakan `this.props.name` pada `<Greeting />`, yang di ambil dari `<App />`. jangan lupa untuk meng-export class jika kita menggunakan component `Greeting` di component `App`. 

Berikut kode dari component instance `<App />`:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { Greeting } from './Greeting';

class App extends React.Component {
  render() {
    return (
      <div>
        <h1>
          Hullo and, "Welcome to The Newzz," "On Line!"
        </h1>
        <Greeting name="Ruby" />
        <article>
          Latest newzz:  where is my phone?
        </article>
      </div>
    );
  }
}

ReactDOM.render(
  <App />, 
  document.getElementById('app')
);
```

Ketika kita menggunakan component lain dalam hal ini component `Greeting` kita harus meng-import filenya terlebih dahulu dengan sintaks `import { Greeting } from './Greeting';`. Kita akan melewatkan informasi `name` pada component instance `<Greeting>`.

## Render UI yang berbeda berdasarkan props

Kita dapat melakukan lebih banyak lagi dengan `props` dari pada hanya menampilkannya. Kita juga dapat menggunakan `props` untuk membuat keputusan.

Perhatikan pada contoh berikut:

```javascript
import React from 'react';

export class Welcome extends React.Component {
  render() {
    if (this.props.name == 'Wolfgang Amadeus Mozart') {
      return (
      	<h2>
      	  hello sir it is truly great to meet you here on the web
      	</h2>
      );
    } else {
      return (
      	<h2>
      	  WELCOME "2" MY WEB SITE BABYYY!!!!!
      	</h2>
      );
    }
  }
}
```

Pada contoh kode di atas, kita menggunakan sebuah `props` dengan nama `name` untuk menentukan keputusan, jika `props` menerima value yang cocok maka akan mereturn `hello sir it is truly great to meet you here on the web` sebaliknya jika value tidak cocok maka akan mereturn `WELCOME "2" MY WEB SITE BABYYY!!!!!`.

Sekarang kita lihat class `Home` yang melewatkan informasi props tersebut:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { Welcome } from './Welcome';

class Home extends React.Component {
  render() {
    return <Welcome name='Ludwig van Beethoven' />;
  }
}

ReactDOM.render(
  <Home />, 
  document.getElementById('app')
);
```

Pada component `Home` kita melewatkan props yang tidak sesuai dengan kondisi, maka hasilnya adalah false dan tercetak pada browser `WELCOME "2" MY WEB SITE BABYYY!!!!!`.

## Meletakan Event Handler pada Component Class

Kita bisa, dan akan selalu, melewatkan function sebagai `props`. Terutama pada function event handler. Pada pembahasan berikutnya. kita akan menggunakan function event handler sebagai `prop`. Namun kita harus mendefinisikan sebuah event handler sebelum kita bisa menggunkannya di manapun.

Bagaimana kita mendefinisikan sebuah event handler di dalam React?

Kita mendefinisikan sebuah event handler sebagai instruksi di dalam property pada object, sama halnya seperti render function. Hampir semua function yang kita definisikan pada React akan di definisikan dengan cara ini, sebagai *instructions object properties*.

Perhatikan contoh berikut:

```javascript
import React from 'react';

class Example extends React.Component {
  handleEvent() {
    alert(`I am an event handler.
      If you see this message,
      then I have been called.`);
  }

  render() {
    return (
      <h1 onClick={this.handleEvent}>
        Hello world
      </h1>
    );
  }
}
```

Pada kode di atas pada baris 4 sampai 8 sebuah *event handler* di definisikan. Dan pada baris ke 12 di method render *event handler* tersebut digunakan.

## Melewatkan Event Handler sebagai prop

Anda bisa melewatkan method dengan cara yang sama seperti anda menyampaikan informasi lainnya. Perhatikan kode berikut bagaimana kita melewatkan *event handler* sebagai prop:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { Button } from './Button';

class Talker extends React.Component {
  talk() {
    let speech = '';
    for (let i = 0; i < 10000; i++) {
      speech += 'blah ';
    }
    alert(speech);
  }
  
  render() {
    return <Button talk={ this.talk } />;
  }
}

ReactDOM.render(
  <Talker />,
  document.getElementById('app')
);
```

Tujuan dari kode di atas adalah untuk melewatkan function `talk` dari `<Talker />` ke `<Button />`. Berikut adalah component `Button`:

```javascript
import React from 'react';

export class Button extends React.Component {
  render() {
    return (
      <button>
        Click me!
      </button>
    );
  }
}
```

## Menerima Event Handler sebagai prop

Kita telah melewatkan function dari `<Talker />` ke `<Button />`.

Jika user klik element `<button></button>`, lalu kita ingin melewatkan function `talk` untuk di panggil. Itu artinya kita harus menyematkan `talk` kepada `<button></button>` sebagai *event handler*.

Bagaimana kita melakukannya? Cara yang sama ketika kita menyematkan event handler ke JSX element, yaitu kita memberikan element JSX sebuah atribut spesial. Nama atribut bisa seperti `onClick` atau `onHover`. Nilai dari attribute adalah event handler yang kita ingin gunakan.

```javascript
import React from 'react';

export class Button extends React.Component {
  render() {
    return (
      <button onClick={ this.props.talk }>
        Click me!
      </button>
    );
  }
}
```

## handleEvent, onEvent, dan this.props.onEvent

Mari kita membicarakan mengenai *naming convention*. Ketika kita melewatkan sebuah event handler sebagai `prop` seperti yang sudah kita lakukan, terdapat 2 nama yang harus kita pilih. Kedua nama ada pada component class, nama pertama yang harus kita pilih adalah nama event handler itu sendiri.

Lihat `Talker.js` yang sudah kita buat, kita menamainya dengan `talk` pada function event handler. Nama ke dua yang kita pilih adalah nama dari `prop` yang kita akan gunakan untuk pass event handler. Ini sama seperti nama dalam atribut.

Untuk nama `prop`, kita juga memilih `talk`, lihat baris kode ini:

```javascript
return <Button talk={this.talk} />;
```

Kedua nama ini bisa apa saja sesuai yang kita mau. Namun, ada sebuah naming convention yang sering mereka ikuti. Kita tidak harus mengikuti convention ini tapi kita harus mengerti kapan harus menggunakannya.

Berikut bagaimana sebuah naming convention bekerja: Pertama, fikirkan tipe dari event yang kita inginkan. Dalam contoh diatas tipe eventnya adalah “click”.

Jika kita menginginkan sebuah event “click”, maka kita namakan event handler kita `handleClick`. Jika kita menginginkan sebuah event “keyPress”, maka kita bisa menamai event handler `handleKeyPress`:

```javascript
class MyClass extends React.Component {
  handleHover() {
    alert('I am an event handler.');
    alert('I will be called in response to "hover" events.');
  }
}
```

Perhatikan contoh berikut:

File `Talker.js`.

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import { Button } from './Button';

class Talker extends React.Component {
  handleClick() {
    let speech = '';
    for (let i = 0; i < 10000; i++) {
      speech += 'blah ';
    }
    alert(speech);
  }
  
  render() {
    return <Button onClick={this.handleClick}/>;
  }
}

ReactDOM.render(
  <Talker />,
  document.getElementById('app')
);
```

File `Button.js`.

```javascript
import React from 'react';

export class Button extends React.Component {
  render() {
    return (
      <button onClick={this.props.onClick}>
        Click me!
      </button>
    );
  }
}
```

## this.props.children

Setiap component `props` object, mempunyai properti bernama `children`. `this.props.children` akan mereturn semua yang ada di antara opening component dan closing JSX tags.

Sejauh ini, semua component yang kita lihat mempunya tag self-closing, seperti `<MyComponentClass />`. Mereka yang tidak mempunyai tag penutup, kita bisa menulis `<MyComponentClass></MyComponentClass>`, dan itu akan tetap berjalan.

`this.props.children` akan mereturn apapun antara  `<MyComponentClass>` dan `</MyComponentClass>`. Perhatikan contoh berikut ini.

```javascript
// Example 1
<BigButton>
  I am a child of BigButton.
</BigButton>


// Example 2
<BigButton>
  <LilButton />
</BigButton>


// Example 3
<BigButton />
```

Pada example 1, `this.props.children` sama juga dengan text, “I am a child of BigButton.”. Example 2, `this.props.children` sama dengan component `<LilButton />`. Example 3, `this.props.children` adalah undefined.

## defaultProps

```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class Button extends React.Component {
  render() {
    return (
      <button>
        {this.props.text}
      </button>
    );
  }
}

ReactDOM.render(
  <Button />, 
  document.getElementById('app')
);
```

Lihat `Button` component class pada contoh diatas. Pada baris ke 8, `Button` berharap menerima `prop` dengan nama `text`. Hasil dari menerima `prop` dengan nama `text` akan di tampilkan ke element `<button></button>`.

Bagaimana jika `text` tidak melewati apa2 pada `Button`? Jika tidak ada yang melewatkan `text` pada `Button`, maka `Button` tidak akan menampilkan apa2 (blank). Lebih baik jika `Button` bisa menampilkan pesan di dalamnya secara default.

Kita dapat memungkinkan ini dengan menuliskan sebuah function dengan nama `defaultProps`:

```javascript
class Example extends React.Component {
  render() {
    return <h1>{this.props.text}</h1>;
  }
}

Example.defaultProps;
```

Function `defaultProps` harus mereturn sebuah object. Di dalam object, tulis properti untuk semua default `props` yang kita inginkan:

```javascript
class Example extends React.Component {
  render() {
    return <h1>{this.props.text}</h1>;
  }
}

// Set defaultProps equal to an object:
Example.defaultProps = {};
```

Di dalam object tersebut, tulis properties untuk semua default `props` yang kita inginkan:

```javascript
class Example extends React.Component {
  render() {
    return <h1>{this.props.text}</h1>;
  }
}

Example.defaultProps = { text: 'yo' };
```

Jika sebuah `<Example />` tidak melewatkan text apapun, maka "yo" akan di tampilkan.

## Kesimpulan this.props

Kita sudah menyelesaikan belajar `props`!

- Melewatkan `prop` dengan memberikan attribute pada sebuah component instance.

- Akses `prop` via `this.props.prop-name`.

- Menampilkan `prop`.

- Menggunakan `prop` untuk pengambilan keputusan tentang apa yang akan di tampilkan.

- Mendefinisikan event handler pada sebuah component class.

- Melewatkan event handler sebagai sebuah `prop`.

- Menerima `prop` event handler dan menyematkan sebuah event listener.

- Penamaan event handle dan atribute pada event handler sesuai convention.

- `this.props.children`

- `getDefaultProps`
