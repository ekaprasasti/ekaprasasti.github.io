---
layout: post
title:  'Input Year Mask Vue JS'
image: '/assets/img/mask-input-year-vue/the-mask.gif'
date:   2018-04-15 00:05:03
tags:
- Life
description: 'Di seri My Daily Bug kali ini saya mau ngebahas proses ketika saya membuat input tahun yang di mana di validasi dan di pastikan yang di input adalah format tahun saja tidak lebih dari 4 digit tipe number, seperti itu sederhananya. Di html kita bisa menemukan input dengan tipe date yaitu `tahun-bulan-tanggal` dan **monthyear** yaitu `bulan-tahun`, tetapi kita tidak menemukan di html tipe yang hanya tahun saja, jadi mau tidak mau kita harus membuat field input tersebut secara manual.'
categories:
- My Daily Bug
serie: Life
---

Di seri My Daily Bug kali ini saya mau ngebahas proses ketika saya membuat input tahun yang di mana di validasi dan di pastikan yang di input adalah format tahun saja tidak lebih dari 4 digit tipe number, seperti itu sederhananya. Di html kita bisa menemukan input dengan tipe date yaitu `tahun-bulan-tanggal` dan **monthyear** yaitu `bulan-tahun`, tetapi kita tidak menemukan di html tipe yang hanya tahun saja, jadi mau tidak mau kita harus membuat field input tersebut secara manual.

![the mask](/assets/img/mask-input-year-vue/the-mask.gif)

Setelah saya research saya menemukan satu library untuk mask input yang paling sering muncul di urutan teratas pencarian google dan sepertinya memang paling sering di pakai oleh developer lain, yaitu [Inputmask karya Robin Herbots](https://github.com/RobinHerbots/Inputmask). Saat ini saya menggunakan Vue, dan saya “sedang” menghindari pemakaian jQuery di dalam kodingan vue saya, alasannya sederhana supaya semua plain javascript dan meninggalkan jQuery library yang saat ini sudah “usang”. Sehingga saya memutuskan untuk tidak memakai library tersebut (walaupun akhirnya nanti saya baru ngeh kalo plugin ini ada juga untuk plain javascript).

Saya kembali melakukan pencarian apakah ada library atau  algoritma yang bisa saya pakai untuk mask input 4 digit untuk tahun ini. Saya menemukan kembali satu buah library yang mungkin bisa saya pakai, yaitu [imask js](https://unmanner.github.io/imaskjs/). Selain bisa memakai plain javascript, library ini juga mendukung frontend framework seperti react, angular, dan tentu saja vue js. Tidak fikir panjang langsung saya pakai yang versi vue nya.

Ada 2 tipe yang bisa kita pakai menggunakan library ini yaitu dengan component atau directive. Dengan contoh kode seperti ini,

**component**

```javascript
<template>
  <imask-input
    v-model="numberModel"
    :mask="Number"
    radix="."
    :unmask="true"
    @accept="onAccept"  // first argument will be `value` or `unmaskedValue` depending on prop above
    // ...and more mask props in a guide

    // other input props
    placeholder='Enter number here'
  />
</template>

<script>
  import {IMaskComponent} from 'vue-imask';

  export default {
    data () {
      return {
        numberModel: '',
        onAccept (value) {
          console.log(value);
        }
      }
    },
    components: {
      'imask-input': IMaskComponent
    }
  }
</script>
```

**directive**

```javascript
<template>
  <input
    :value="value"
    v-imask="mask"
    @accept="onAccept"
    @complete="onComplete">
</template>

<script>
  import {IMaskDirective} from 'vue-imask';

  export default {
    data () {
      return {
        value: '',
        mask: {
          mask: '{8}000000',
          lazy: false
        },
        onAccept (maskRef) {
          console.log('accept', maskRef.value);
        },
        onComplete (maskRef) {
          console.log('complete', maskRef.unmaskedValue);
        }
      }
    },
    directives: {
      imask: IMaskDirective
    }
  }
</script>
```

Saya coba menggunakan directive karena menurut saya lebih mudah dan saya juga sudah memisahkan component input tersebut sebetulnya di dalam project saya, jadi sepertinya jadi sangat tidak intuitif sekali jika di dalam component terdapat component library (walaupun sebenarnya tidak masalah).

Ok setelah saya implementasikan dan works! ada satu kendala, yaitu masalah id. Jadi begini input tersebut saya jadikan component dan di component parentnya yaitu di form yang menggunakan input tersebut, saya menggukan 2 input year, tetapi karena id itu harus unik jadi saya mempasing props id berbeda, dan cara itu pun juga tetap tidak bisa, jadi intinya dengan directive kita tidak bisa menggunakan 2 component input yang sama. 

Saya beralih ke penggunaan library dengan component, setelah jadi dan benar works juga bisa di gunakan multiple form, muncul lagi kendala yaitu dengan component dari `imask js` ini, kita tidak bisa menggunakan global vue object `$emit` yang berguna untuk mengirim data ke component parentnya yaitu form. hufh…!! Setalah saya mencari2 lagi dan debug sana sini akhrinya saya memutuskan untuk tidak memakai library ini. Hahaaaa.. 

Hmm, pencarian saya berlanjut. Saya menemukan kembali library yang bisa menggunakan plain javascript tetapi ketika saya menggunakan `document.getElementById` di created pada vue yang terbaca di console hanya satu. 

> untuk `document.getElementById` setelah beberapa lama akhirnya saya mengerti untuk menaruhnya tidak di create karena create itu lifecycle di vue yang mana di eksekusi ketika template belum di jalankan. Untuk penggantinya bisa kita gunakan mounted, untuk lebih jelasnya bisa baca tulisan saya di medium mengenai [Lifecycle Vue JS](https://medium.com/@ekaprasasti/mengenal-lifecycle-hooks-pada-vue-js-78cd2225a69)

Saya mencoba untuk membuat dengan javascript secara manual, ada beberapa solusi yang saya temukan di stackoverflow maupun di beberapa website. Tetapi saya akhirnya memutuskan untuk memakai logika saya sendiri (haha setelah menyerah dengan library buatan orang lain ujung2nya ya mikir sendiri).

Saya menggukan event input `@input` pada vue untuk mendeteksi input yang di ketik pada keyboard dan mempasing hasilnya pada sebuah function, di function tersebut saya validasi jika panjang inputnya lebih dari 4 maka sisanya akan di slice. Berikut contoh kodenya.

```javascript
<template>
  <div class="form-group">
    <label>{{ label }}</label>
    <input
      name="year"
      :disabled="disabled"
      v-model="year"
      @input="maxLengthCheck"
      class="form-control"
      :placeholder="placeholder">
  </div>
</template>

<script>
export default {
  name: 'InputYear',
  props: [
    'label',
    'placeholder'
  ],
  data () {
    return {
      year: ''
    }
  },
  watch: {
    dataModel (value) {
      this.year = value
    }
  },
  methods: {
    maxLengthCheck () {
      if (this.year.length >= 4) {
        this.year = this.year.slice(0, 4)
      }
    }
  }
}
</script>
```

Kode di atas adalah component asli yang saya buat di sana sudah ada campuran kode dari vuex, dan libarary validasi `vee-validate`. Yang perlu di perhatikan mungkin `@input` pada input field di template, yang mengakses method `maxLengthCheck`. Akhirnya berhasil, dan ternyata sangat sederhana. Hikmah yang bisa di ambil adalah jangan patah semangat untuk trial dan error!

## Good Resources

* [stackoverflow "Why is document.getElementById returning null"](https://stackoverflow.com/questions/2632137/why-is-document-getelementbyid-returning-null)

* [stackoverflow "Vue limit characters in text area input truncate filter"](https://stackoverflow.com/questions/46289311/vue-limit-characters-in-text-area-input-truncate-filter)

* [jsfiddle](http://jsfiddle.net/DRSDavidSoft/zb4ft1qq/1/)