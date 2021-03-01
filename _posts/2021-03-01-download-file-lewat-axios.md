---
layout: post
title:  'Download File Lewat Axios'
image: ''
date:   2021-03-01 00:00:00
tags:
- Javascript
description: ''
categories:
- My Daily Bug
---

Jika kita ingin mendownload sebuah file melalui direct link pada sebuah web server biasanya tinggal panggil aja linknya, dan boomm..!! file langsung terdownload atau bisa juga klik kanan save as untuk file yang punya viewer di browser seperti image atau file lainnya.

Tetapi lain halnya jika kita ingin mendownload file dari sebuah Response API, biasanya hal ini terjadi ketika server ingin men-generate file sesuai dengan parameter request body dari client, maka setelah file ter-generate biasanya server akan mengirimkan file kita pada response berupa type data blob.

Hal ini bisa kita lakukan hanya dengan sebuah button download yang sudah kita buat pada browser, dan memanggil sebuah function `downloadFile` seperti contoh di bawah ini.

```javascript
import axios from 'axios'

function downloadFile () { 
  const instance = axios.create({responseType: 'blob'}) // ini penting..!!
  instance.post('http://endpoint-url.example/download', {
    // contoh parameter body request
    parameter: {
      id: 1,
      username: 'ekaprasasti'
    }
  }).then(response => {
    this.handleBlobResponse(response.data)
  })
}
```

Hal pertama yang kita lakukan adalah membuat instance dengan axios, untuk dokumentasi detail tentang instance pada axios silahkan ke link berikut, lalu kita panggil parameter `responseType: 'blob'` karena kita akan menghandle response berupa blob, instance dengan parameter ini wajib di sertakan agar axios tau tipe data pada response yang akan kita terima untuk kita olah atau handle pada function `handleBlobResponse`.

```javascript
function handleBlobResponse (blobFileData) {
  const url = window.URL.createObjectURL(new Blob([blobFileData]))
  const link = document.createElement('a')
  link.href = url
  link.setAttribute('download', filename.txt) // nama file dan extension sesuaikan dengan file yang di download
  document.body.appendChild(link)
  link.click()
}
```

Selanjutnya pada function `handleBlobResponse` kita akan mensimulasikan trigger `click` sebuah button tag `<a>` menggunakan javascript DOM, yang kita sisipkan attribute href dengan value blob yang sudah kita terima pada response axios. Setelah kita simulasikan trigger click dengan memanggil function `click()` pada dom, maka file akan terdownload pada browser.