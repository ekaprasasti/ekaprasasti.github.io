---
layout: post
title: "Mengatur Posisi Dengan CSS" 
image: ''
date:   2017-09-23 00:05:03
tags:
- CSS
- Web Design 
description: ''
categories:
- Web Design
serie: CSS 
---

Saat ini saya ingin membahas mengenai *positioning* element HTML menggunakan CSS. Biasanya programmer yang sudah terbiasa menggunakan framework CSS seperti Bootstrap sering melupakan dasar dari pengaturan posisi, karena framework seperti itu mereka sudah menyediakan class yang tinggal kita tambahkan di *attribute* HTML, biasanya kita menggunakan Grid dan Column yang di tandai dengan misal `col-md-12` dan sebagainya.

Saya rasa kita perlu belajar lagi mengenai dasar-dasar CSS ini khususnya mengenai *positioning*. Agar basic kita mengenai CSS ini kuat dengan tidak hanya mengandalkan framework seperti Bootstrap saja. Pada artikel kali ini setidaknya ada 5 syntax CSS yang bisa di gunakan untuk mengatur *positioning*, yaitu position, overflow, float, inline-block, dan Align. Mari kita bahas satu persatu.

## Position

Properti *position* di gunakan untuk menentukan *positioning* yang di gunakan pada sebuah elemen. Setidaknya ada 5 nilai yang dapat di gunakan menggunakan properti ini yaitu, static, relative, fixed, absolute, dan sticky. Letak pada posisi sendiri di atur dengan menggunakan properti *top*, *bottom*, *left*, dan *right* yang tidak akan bekerja kecuali properti *position* di definisikan terlebih dahulu. 

### posisition: static;

Elemen pada HTML secara default menggunakan posisi static. Elemen dengan *position* static sama seperti posisi normal biasa, dan tidak berpengaruh kepada properti *top*, *bottom*, *left*, dan *right*. Perhatikan contoh berikut:

```html
<style>
div.static {
  position: static;
  border: 3px solid #543535;
}
</style>
<div class="static">
  Elemen ini menggunakan position: static;
</div>
```

Jika kode di atas kita jalankan menggunakan browser, maka akan terlihat seperti ini:

![position static css](/assets/img/posisi-dengan-css/static.png)

Jika kita beri properti *top*, *bottom*, *left*, dan *right* pada kode css, contohnya seperti ini:

```css
div.static {
  position: static;
  top: 10px;
	left: 30px;  
  border: 3px solid #543535;
}
```

Maka hasilnya akan sama saja karena properti tersebut tidak berpengaruh pada posisi static.

### posisition: relative;

Elemen dengan posisi relative dapat di posisikan dengan properti *top*, *bottom*, *left*, dan *right* yang disesuaikan dengan posisi normal. Perhatikan contoh berikut:

```html
<style>
div.static {
  position: relative;
  top: 10px;
  left: 30px;
  border: 3px solid #543535;
}
</style>
<div class="static">
  Elemen ini menggunakan position: static;
</div>
```

Jika kode diatas di jalankan pada browser maka hasilnya akan seperti ini:

![posisiton relative css](/assets/img/posisi-dengan-css/relative.png)

Hasilnya adalah elemen berada di antara jarak kiri sejauh 30px dari posisi normal dan atas sejauh 10px dari posisi normal.

### position: fixed;

Berbeda dengan relative, posisi fixed mengatur properti *top*, *bottom*, *left*, dan *right* yang di sesuaikan dengan area browser. Perhatikan contoh berikut:

```html
<style>
div.static {
  position: fixed;
  bottom: 0;
  right: 0;
  width: 300px;
  border: 3px solid #543535;
}
</style>
<div class="static">
  Elemen ini menggunakan position: fixed;
</div>
```

Jika kode diatas di jalankan pada browser maka akan terlihat seperti berikut:

![position fixed css](/assets/img/posisi-dengan-css/fixed.png)

Kita lihat hasilnya pada gambar di atas, bahwa elemen berada di pojok kanan bawah karena kita mengatur properti *bottom* dan *right* sebesar 0. Elemen akan tetap berada pada posisinya walaupun browser di scroll.

### position: absolute;

Elemen dengan posisi absolute akan menyesuaikan dengan posisi relative dari tag root terdekatnya. Namun jika elemen tidak mempunya tag root maka akan menyesuaikan dengan tag body. Perhatikan contoh berikut ini:

```html
<style>
div.relative {
    position: relative;
    width: 400px;
    height: 200px;
    border: 3px solid #543535;
} 

div.absolute {
    position: absolute;
    top: 80px;
    right: 0;
    width: 200px;
    height: 100px;
    border: 3px solid #543535;
}
</style>
<div class="relative">Elemen ini menggunakan position: relative;
  <div class="absolute">Elemen ini menggunakan position: absolute;</div>
</div>
```

Jika kode diatas di jalankan pada browser maka hasilnya akan seperti ini:

![position absolute css](/assets/img/posisi-dengan-css/absolute.png)

Pada contoh di atas kita bisa melihat bahwa posisi elemen dari class `absolute` di atur dengan properti `top` sejauh 80px dari elemen class `relative` dan properti `right` sejauh 0 dari elemen class `relative`.

### position: sticky;

Elemen dengan posisi sticky adalah posisi elemen yang di sesuaikan dengan scroll pada browser. Untuk browser seperti Internet Explorer dan Edge tidak mensuport sticky, sedangkan untuk browser Safari kita harus menambahkan prefix `-webkit-sticky`. Untuk menggunakan posisi sticky ini setidaknya kita harus mengatur salah satu dari properti *top*, *bottom*, *left* dan *right*. Perhatikan contoh berikut:

```html
<style>
div.sticky {
  position: -webkit-sticky;
  position: sticky;
  top: 0;
  padding: 5px;
  background-color: #cae8ca;
  border: 2px solid #543535;
}
</style>
<div class="sticky">Coba scroll saya!</div>

<div style="padding-bottom:2000px">
  <p>Perkecil ukuran jendela browser dan coba scroll. Lorem ipsum dolor sit amet, illum definitiones no quo, maluisset concludaturque et eum, altera fabulas ut quo. Atqui causae gloriatur ius te, id agam omnis evertitur eum. Affert laboramus repudiandae nec et. Inciderint efficiantur his ad. Eum no molestiae voluptatibus.</p>
  <p>Perkecil ukuran jendela browser dan coba scroll.. Lorem ipsum dolor sit amet, illum definitiones no quo, maluisset concludaturque et eum, altera fabulas ut quo. Atqui causae gloriatur ius te, id agam omnis evertitur eum. Affert laboramus repudiandae nec et. Inciderint efficiantur his ad. Eum no molestiae voluptatibus.</p>
</div>
```

Jika kode HTML di atas di jalankan pada browser maka hasilnya akan seperti ini:

![position sticky css](/assets/img/posisi-dengan-css/sticky.png)

Tentu untuk praktek scrolling dengan image di atas tidak bisa memvisualisasikan penggunaan scroll pada browser. Untuk mempraktekannya copy kode di atas dan pastekan pada text editor dan simpan dengan nama misalnya belajar.html lalu buka file tersebut dengan browser dan perkecil ukuran browser agar kita bisa men-scroll browser keatas dan kebawah. 

Bisa di lihat bahwa elemen dengan posisi sticky walaupun kita scroll keatas atau ke bawah elemen tetap menyesuaikan. Posisi ini sering kita pakai untuk membuat navigasi pada website.

### z-index

Sebagai tambahan `z-index` adalah sebuah properti pada css yang di gunakan untuk menentukan urutan elemen yang saling tumpang tindih atau istilahnya adalah *overlapping elemen*. Untuk lebih jelasnya perhatikan contoh berikut ini:

```html
<style>
img {
    position: absolute;
    left: 0px;
    top: 0px;
    z-index: -1;
}
</style>
<h1>Contoh z-index</h1>
<img src="http://io13-high-dpi.appspot.com/images/CSS3_Logo.svg" height="140">
<p>Perhatikan gambar akan berada di bawah tulisan</p>
```

Jika kode di atas di jalankan pada browser maka hasilnya akan menjadi seperti ini:

![z-index](/assets/img/posisi-dengan-css/z-index.png)

Dengan properti `z-index` dengan nilai `-1` pada elemen `img` maka kita dapat membuat image tersebut berada di belakang elemen lainnya.

## Float

Float biasa di gunakan untuk mengatur posisi text di sekitar image. Perhatikan contoh berikut:

```html
<style>
img {
    float: right;
}
</style>
<p>Properti float membuat image berada di sebelah kanan dan text yang membungkus image</p>

<p><img src="http://io13-high-dpi.appspot.com/images/CSS3_Logo.svg" height="170">
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Phasellus imperdiet, nulla et dictum interdum, nisi lorem egestas odio, vitae scelerisque enim ligula venenatis dolor. Maecenas nisl est, ultrices nec congue eget, auctor vitae massa. Fusce luctus vestibulum augue ut aliquet. Mauris ante ligula, facilisis sed ornare eu, lobortis in odio. Praesent convallis urna a lacus interdum ut hendrerit risus congue. Nunc sagittis dictum nisi, sed ullamcorper ipsum dignissim ac. In at libero sed nunc venenatis imperdiet sed ornare turpis. Donec vitae dui eget tellus gravida venenatis. Integer fringilla congue eros non fermentum. Sed dapibus pulvinar nibh tempor porta. Cras ac leo purus. Mauris quis diam velit.</p>
```

Jika kode di atas di buka pada browser maka hasilnya akan menjadi seperti ini:

![float](/assets/img/posisi-dengan-css/float.png)

### Memisahkan diri dengan Clear

Elemen yang berada di sekitar properti float akan menyesuaikan dengan elemen dengan properti tersebut. Untuk mengatasinya gunakan properti `clear` pada elemen yang ingin kita pisahkan dari properti float. Perhatikan contoh berikut:

```html
<style>
.div1 {
    float: left;
    width: 100px;
    height: 50px;
    margin: 10px;
    border: 3px solid #543535;
}

.div2 {
    border: 1px solid red;
}


.div3 {
    float: left;
    width: 100px;
    height: 50px;
    margin: 10px;
    border: 3px solid #543535;
}

.div4 {
    border: 1px solid red;
    clear: left;
}
</style>
<h2>Dengan clear</h2>
<div class="div1">div1</div>
<div class="div2">div2 - Perhatikan bahwa div 2 berada setelah div 1 yang mana div 1 mempunyai properti float left. Maka yang terjadi adalah text pada div 2 menyesuaikan float pada div 1.</div>

<h2>Menggunakan clear</h2>
<div class="div3">div3</div>
<div class="div4">div4 - memisahkan diri dengan properti float pada div 3 menggunakan properti clear.</div>
```

Jika kode di atas di jalankan maka hasilnya akan seperti berikut ini:

![clear](/assets/img/posisi-dengan-css/clear.png)

### Clearfix Overflow

Jika elemen image lebih tinggi dari pada elemen text, maka elemen gambar tersebut akan terpisah dari elemen text yang membungkusnya. Untuk mengatasi hal ini kita menggunakan properti yang di namakan overflow. Overflow adalah sebuah properti pada CSS yang di gunakan untuk menscroll suatu elemen jika ukuran jendela browser lebih kecil dari elemen yang membungkusnya. Perhatikan contoh berikut:

```html
<style>
div {
    border: 3px solid #4CAF50;
    padding: 5px;
}

.img1 {
    float: right;
}

.clearfix {
    overflow: auto;
}

.img2 {
    float: right;
}
</style>
<p>Pada contoh berikut, image lebih tinggi dari elemen text sehingga image di overflow di luar elemen text</p>

<div>
	<img class="img1" src="http://io13-high-dpi.appspot.com/images/CSS3_Logo.svg" height="170"> Lorem ipsum dolor sit amet, consectetur adipiscing elit. Phasellus imperdiet, nulla et dictum interdum...
</div>

<p style="clear:right">Tambahkan class clearfix dengan overflow: auto; pada elemen image untuk mengatasi hal ini</p>

<div class="clearfix">
	<img class="img2" src="http://io13-high-dpi.appspot.com/images/CSS3_Logo.svg" height="170"> Lorem ipsum dolor sit amet, consectetur adipiscing elit. Phasellus imperdiet, nulla et dictum interdum...
</div>
```

Jika kode di atas di jalankan pada browser maka hasilnya akan menjadi seperti berikut:

![float overflow](/assets/img/posisi-dengan-css/float-overflow.png)

Dari contoh di atas bisa di lihat bahwa dengan `overflow: auto;` kita bisa membungkus elemen gambar yang lebih panjang dengan text. Dengan properti overflow maslah yang kita alami sudah teratasi namun banyak developer yang menggunakan *pseudo-class* after sebagai alternative lain, contohnya bisa di lihat dibawah ini. Mengenai apa itu *pseudo-class* akan di bahas pada kesempatan berikutnya.

```
.clearfix::after {
    content: "";
    clear: both;
    display: table;
}
```

## Inline Block

Sebagai alternative dari float kita dapat menggunakan `display: inline-block` untuk membuat grid, perbedaannya jika kita menggunakan float dalam menggunakan grid kita harus menggunakan properti `clear`. Perhatikan kedua contoh kode berikut:

### Menggunakan Float

```html
<style>
.floating-box {
    float: left;
    width: 150px;
    height: 75px;
    margin: 10px;
    border: 3px solid #73AD21;  
}

.after-box {
    clear: left;
    border: 3px solid red;      
}
</style>
<h2>Menggunakan float</h2>

<div class="floating-box">Floating box</div>
<div class="floating-box">Floating box</div>
<div class="floating-box">Floating box</div>
<div class="floating-box">Floating box</div>
<div class="floating-box">Floating box</div>
<div class="floating-box">Floating box</div>
<div class="floating-box">Floating box</div>
<div class="floating-box">Floating box</div>

<div class="after-box">Box setelah floating box</div>
```

### Menggunakan inline-block

```html
<style>
.floating-box {
    display: inline-block;
    width: 150px;
    height: 75px;
    margin: 10px;
    border: 3px solid #73AD21;  
}

.after-box {
    border: 3px solid red; 
}
</style>
<h2>Menggunakan inline-block</h2>

<div class="floating-box">Floating box</div>
<div class="floating-box">Floating box</div>
<div class="floating-box">Floating box</div>
<div class="floating-box">Floating box</div>
<div class="floating-box">Floating box</div>
<div class="floating-box">Floating box</div>
<div class="floating-box">Floating box</div>
<div class="floating-box">Floating box</div>

<div class="after-box">Box setelah floating box</div>
```

Kedua tipe kode di atas jika kita jalankan pada browser maka hasilnya akan sama:

![inline-block](/assets/img/posisi-dengan-css/inline-block.png)

Pada penggunaan properti `float` kita harus menggunakan properti `clear` untuk memisahkan class `after-box` dari baris float class `floating-box`. Tetapi pada kode yang ke dua menggunakan `display: inline-block` kita tidak perlu menggunakan properti `clear`. 

Sepertinya sudah cukup sampai di sini pembahasan mengenai properti dasar apa saja untuk mengatur posisi dari suatu elemen. Sebenarnya masih banyak trik atau cara lain untuk mengatur posisi elemen HTML menggunakan CSS, seperti menggunakan margin auto, padding, text-align, display block, dan line-height, yang mungkin akan saya bahas di lain kesempatan. Sebagai referensi teman-teman bisa membaca dokumentasi dari [w3school.com](https://www.w3schools.com/css/default.asp). Jika ada yang ingin di tanyakan dan di diskusikan silahkan bertanya di kolom komentar ya.
