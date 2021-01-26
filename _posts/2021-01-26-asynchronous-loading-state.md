---
layout: post
title:  'Asynchronous Loading State'
image: ''
date:   2021-01-26 00:00:00
tags:
- Javascript
- ReactJS
- VueJS
description: 'Dalam proses asychronous kita tahu semua proses di jalankan bersamaan, tidak harus menunggu sebuah proses selesai baru menjalankan proses lain. Seringkali kita menemui kondisi di mana setiap proses saling berkaitan dan bergantung satu sama lain, sehingga dalam waktu tunggu proses hingga proses selesai kita harus mengirimkan sebuah pesan apa yang sedang terjadi antara proses satu dengan yang lainnya, dan pesan inilah yang di sebut dengan "Loading State" pada Asynchronous.'
categories:
- My Daily Bug
---

Dalam proses asychronous kita tahu semua proses di jalankan bersamaan, tidak harus menunggu sebuah proses selesai baru menjalankan proses lain. Seringkali kita menemui kondisi di mana setiap proses saling berkaitan dan bergantung satu sama lain, sehingga dalam waktu tunggu proses hingga proses selesai kita harus mengirimkan sebuah pesan apa yang sedang terjadi antara proses satu dengan yang lainnya, dan pesan inilah yang di sebut dengan "Loading State" pada Asynchronous.

Hal paling nyata yang kita hadapi adalah pada library seperti React JS ataupun Vue JS, ketika kita akan mengakses sebuah state yang sedang menjalani proses asynchronous data misal fetch data pada API atau apapun yang memerlukan waktu tunggu, tetapi template atau view tampilan lebih cepat menyajikan data pada state, dari pada proses asynchronous itu sendiri, sehingga yang tampil adalah state atau data kosong, atau pun biasanya pesan error karena data yang di tuju tidak dapat di akses, nah di sinilah kita perlu mengimplementasi "Loading State" pada proses asynchronous yang kita jalankan.

Contoh loading state pada React JS:

```javascript
import React from 'react'
import LoadingSpinner from './LoadingSpinner'

export default class HelloWorld extends React.Component {
  constructor(pros) {
    this.state = {
      data: {},
      isLoadData: true, // ini loading state
    }

    this.loadData = this.loadData.bind(this);
  }

  componentDidMount() {
    this.loadData();
  }

  loadData() {
    // jika setTimeout belum berakhir maka tampilkan icon loading
    this.setState({ isLoadData: true })

    this.setTimeout(() => {
      this.setState({
        data: {
          id: 1,
          description: 'Hello World..!!',
        },
        isLoadData: false,
      })
    }, 3000)
  }

  render() {
    const { data } = this.state;

    if (isLoadData) {
      return <LoadingSpinner />;
    }

    return <h1>{data.description}</h1>
  }
}
```

Jika kita menjalankan program di atas, react akan merender tampilan tag `<h1>` yang kita buat dan akan menampilkan description pada object data yaitu "Hello World..!!", tapi lihat pada function loadData() saya buat timeout setState dengan waktu 3 detik, sehingga jika proses simulasi asynchronous menggunakan setTimeout tersebut belum selesai, maka kita akan mendapatkan error,

```
description is undefined
```

Sehingga kita akan membuat loading state `isLoadData` yang bernilai true atau false, ketika proses asynchronous pada `loadData()` masih berjalan kita buat loading state bernilai true dan jika sudah selesai kita set kembali menjadi false, sehingga kita akan menampilkan `<LoadingSpinner />` terlebih dahulu sebelum proses asynchronous selesai dan menampilkan `<h1>{data.description}</h1>`.

Selain tipe boolean loading state ini juga bisa menggunakan tipe string jika menginginkan flag lebih dari 1, contohnya `loading`, `success`, dan `error`. Berikut contoh implementasinya pada Vue JS:

```javascript
<template>
  <div>
    <div v-if="loadDataState === 'success'">{{ data.description }}</div>
    <div v-else-if="loadDataState === 'loading'">
      <LoadingSpinner />
    </div>
    <div v-else>
      <EmptyState />
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import LoadingSpinner from './LoadingSpinner'
import EmptyState from './EmptyState'

export default {
  components: {
    LoadingState,
    EmptyState
  },
  data () {
    data: {},
    loadDataState: ''
  },
  created () {
    this.loadData()
  },
  methods: {
    loadData () {
      this.loadDataState = 'success'

      axios.get('data')
        .then(() => {
          this.data = { id: 1, description: 'Hello World..!!' }
          this.loadDataState = 'success'
        })
        .catch(() => this.loadDataState = 'error')
    }
  }
}
</script>
```

Kita membutuhkan lebih dari 1 kondisi loading state biasanya untuk menampilkan beberapa tampilan yang berbeda dari setiap kondisi.