---
layout: post
title:  'Upload Progress Bar Dengan Vue dan Ajax'
date:   2017-12-19 00:05:03
tags:
- Javascript
- VueJS
description: ''
categories:
- Programming
serie: Javascript
---

Akhirnya setelah sekian lama beberapa bulan terakhir ini saya kembali menulis blog, hampa rasanya tanpa mengabadikan moment lewat tulisan, tapi apalah daya waktu dan prioritas yang menentukan. Halah, kita sudahi saja dulu puitisnya kita langsung aja ke inti topik pembahasan (eh tapi bener loh hampir 3 bulan ini saya super sibuk dengan project yang saya jalani).

Jadi gini ceritanya, di project saya sekarang menggunakan vue js sebagai frontend frameworknya, kenapa vue js? Ceritanya agak lumayan panjang, jadi baiknya saya ceritakan di postingan tersendiri saja. Saya sedang mengimplementasikan fitur upload video menggunakan vue, saya mengadopsi codenya dari [sini](https://codepen.io/Atinux/pen/qOvawK) yang sebetulnya itu adalah script untuk upload image tapi tentu saya sesuaikan dengan kebutuhan saya untuk impelement fitur upload video.

Logic sederhananya adalah, saya menggunakan input file html biasa yang mengambil file video yang di validasi ekstensinya menggunakan library vee-validate, lalu file video tersebut saya simpan di dalam cache browser yang di gunakan sebagai embed preview sebelum di upload, setelah itu data form saya submit menggunakan object javascript FormData. Kurang lebih scriptnya seperti ini,

```javascript
<template>
	<div id="ElementImageFile">
		<input 
			v-if="!videoData"
			name="video"
			type="file" 
			id="file" 
			v-on:change="onFileChange"
			v-validate="'required|ext:mp4,mov,m4v,avi,mpg'"
			data-vv-delay="1000"
			:class="{'input': true, 'is-danger': errors.has('video') }" />
		<span v-show="errors.has('video')" class="is-danger-notif">
			{{ errors.first('video') }}
		</span>
		<div v-if="image">
			<br>
			<img :src="image" class="img-thumbnail" />
		</div>
		<div v-if="!video && videoData">
			<a class="btn btn-danger btn-sm btn-remove" @click="removeImage">Remove Image</a>
			<br>
			<img :src="imageData" class="img-thumbnail" />
		</div>
	</div>
</template>

<script>
export default {
	name: 'ElementImageFile',
	props: [
		'dataModel'
	],
	data() {
		return {
			video: '',
			videoData: ''
		}
	},
	watch: {
		dataModel(value) {
			// mengambil data dari parent component
			this.videoData = value 
		}
	},
	methods: {
		onFileChange(e) {
			let files = e.target.files || e.dataTransfer.files;
			
			if (!files.length) {
				return;
			}

			this.createVideo(files[0]);	

			// mengirim data ke parent component
			this.$emit('eventChange', files[0]);
		},
		createImage(file) {
      		let image = new Image();
      		let reader = new FileReader();
      		let vm = this;
     		 
      		reader.onload = (e) => {
				vm.image = e.target.result;
      		};
      
      		reader.readAsDataURL(file);
    	},	
		removeImage() {
			this.imageData = '';
		},
	},
	inject: ['$validator']
}
</script>
```

Code di atas merupakan component file upload saja, saya pisahkan dengan component input lainnya di parent component. Untuk pembahasan mengenai penggunaan component pada vue saya bahas di kesempatan berikutnya. Untuk validasi menggunakan `vee-validate` bisa di baca2 [di sini](http://vee-validate.logaretm.com/).

Sampai disini fitur pun berhasil di implementasikan. Tidak ada masalah sampai ada kebutuhan untuk mengupload file lebih dari 20mb sampai ratusan megabyte, lalu upload pun menjadi memakan waktu, client menunggu sekian lama tanpa ada kejelasan di akhir file sukses terupload atau gagal, lalu client meminta di buatkan indikator progress sudah berapa persen data terupload. Dan jika seperti ini `bottleneck` nya pun sudah terlihat jelas, dari menyimpan file di cache browser untuk menampilkan preview sudah pasti lama, apa lagi setelahnya harus mengupload ke server. Mau tidak mau saya pun harus merubah flow data.

Flow saya rubah menjadi, video harus terupload dulu sebelum saya mensubmit data form. Jadi di data form saya tinggal menyimpan deskripsi video dan nama filenya saja. Sehingga preview video yang semula saya ambil sourcenya dari cache browser sekarang saya langsung mengambilnya dari server karena sudah di simpan dan di kirim sebelumnya ke backend. Kebayang kan?

Sebelum di upload saya harus mentracking berapa data yang sudah di kirim kan ke server. Untuk ini ada 2 cara sebetulnya, pertama merubah fitur dengan menggunakan library upload seperti jquery upload, atau membuat sendiri indikator progress bar menggunakan mekanisme ajax. Pilihan pun jatuh kepada menggunakan mekanisme ajax, dengan pertimbangan jika menggunakan library mungkin effortnya lebih besar untuk mengcustom library tersebut, karena kita tahu bahwa library sangat kompleks fitur yang sebetulnya tidak kita butuhkan. Juga saya sendiri ingin mengurangi penggunaan library untuk sebuah fitur yang krusial agar tidak terlalu ter-dependency.

Untuk mentracking byte yang terupload saya mengadopsi kodenya dari [sini](http://christopher5106.github.io/web/2015/12/13/HTML5-file-image-upload-and-resizing-javascript-with-progress-bar.html), dan yang sebelumnya saya menggunakan axios untuk mengakses http request, sekarang saya harus menggunakan ajax untuk mengirim data ke server.

```javascript
let xhr = new XMLHttpRequest();
xhr.upload.addEventListener('progress', function(e){
	if (e.lengthComputable) {
		let percentComplete = e.loaded / e.total;
		let progressValue = Math.round(percentComplete*100);

		// tracking persentase upload
		console.log(progressValue);
	}
}, false);

xhr.responseType = 'json';
xhr.onreadystatechange = function() {
	if (this.readyState == 4 && this.status == 200) {
		let response = this.response;
		console.log(response);
	}	
};

let data = new FormData();
data.append('video', files[0]) 

xhr.open('POST', 'http://menuju/endpoint/api');
xhr.send(data);
```

Ada satu masalah muncul menggunakan ajax, yaitu variabel tidak bisa di akses di luar function ajax, sementara kita harus men-assign data empty agar dapat di gunakan kembali dan di tampilkan pada template. Untuk mengakses dom kita masih bisa menggunakan innerHtml di dalam function ajax untuk mengakses data dan menampilkannya pada indikator progress bar.

Tapi saya harus mengirim data filename kepada parent component agar bisa di submit bersama data lainya, hmm, menarik. Ini bukan memanipulasi dom, tetapi memang mau tidak mau saya harus mengeluarkan data hasil response keluar function ajax, sedangkan menggunakan object bawaan vue di dalam ajax pun di indikasikan sebagai undefined.

```javascript
let xhr = new XMLHttpRequest();
xhr.upload.addEventListener('progress', function(e){
	if (e.lengthComputable) {
		let percentComplete = e.loaded / e.total;
		let progressValue = Math.round(percentComplete*100);
		let progressStyle = 'width: ' + progressValue + '%';
		let progressText = 'Upload '+ progressValue + '% complete.';	
		document.getElementById('progressBar').innerHTML = progressText;
		document.getElementById('progressBar').style = progressStyle;

		// tracking persentase upload
		console.log(progressValue);
	}
}, false);

xhr.responseType = 'json';
xhr.onreadystatechange = function(vm) {
	if (this.readyState == 4 && this.status == 200) {
		let response = this.response;
		this.filename = response.data.filename;
	}	
}

console.log(this.filename) // hasilnya adalah undefined
```

Setelah googling, berkutat dengan stackoverflow dan bertapa di gunung kidul (yang ini emang agak lebay, serius!) saya menemukan jawabanya. Adalah menggunakan method bind yang di sematkan kepada function ajax yang mau kita assign datanya ke data vue. Karena usut punya usut keyword this di dalam function ajax adalah kepunyaan ajax sehingga jika kita ingin menggukan keyword this yang mengakses suatu instance di luar ajax kita bisa menggunakan method bind pada vue.

Nah akhirnya berhasil juga deh ya, ini dia code lengkap component vue upload file video yang saya implementasikan dalam project yang sedang saya kerjakan,

```javascript
<template>
	<div id="ElementVideoFile">
		<input 
			v-if="formatValidation && !videoData"
			name="video"
			type="file" 
			id="file" 
			v-on:change="onFileChange"
			v-validate="'required|ext:mp4,mov,m4v,avi,mpg'"
			:class="{'input': true, 'is-danger': errors.has('video') }" />
		<span v-show="errors.has('video')" class="is-danger-notif">
			{{ errors.first('video') }}
		</span>
		<div v-if="progressActive" class="progress">
  			<div 
				id="progressBar"
				class="progress-bar progress-bar-success" 
				aria-valuemin="0" 
				aria-valuemax="100" 
				style="width:0%">
					<span>Upload 0% complete.</span>
  			</div>
		</div>
		<div v-if="video && !formatValidation">
			<a class="btn btn-danger btn-sm btn-remove" @click="removeVideo">Remove Video</a>
			<br>
			<video width="400" controls>
  				<source :src="video">
    			Your browser does not support HTML5 video.
			</video>
		</div>
		<div v-if="!video && videoData">
			<a class="btn btn-danger btn-sm btn-remove" @click="removeVideoExist">Remove Video</a>
			<br>
			<video width="400" controls>
				<source :src="videoData">
				Your browser does not support HTML5 video.
			</video>
		</div>
	</div>
</template>

<script>
export default {
	name: 'ElementVideoFile',
	props: [
		'dataModel',
		'artikelType'
	],
	data() {
		return {
			video: '',
			videoData: '',
			formatValidation: true,
			filename: '',
			progressActive: false
		}
	},
	watch: {
		dataModel(value) {
			// mengambil data dari parent component
			this.videoData = value 
		}
	},
	methods: {
		onFileChange(e) {
			let files = e.target.files || e.dataTransfer.files;
			this.progressActive = true;
			
			this.$validator.validate('video').then(result => {
				if (result) {
					let xhr = new XMLHttpRequest();
					xhr.upload.addEventListener('progress', function(e){
      					if (e.lengthComputable) {
							let percentComplete = e.loaded / e.total;
							let progressValue = Math.round(percentComplete*100);
							let progressStyle = 'width: ' + progressValue + '%';
							let progressText = 'Upload '+ progressValue + '% complete.';	
							document.getElementById('progressBar').innerHTML = progressText;
							document.getElementById('progressBar').style = progressStyle;
      					}
    				}, false);

					xhr.responseType = 'json';
					xhr.onreadystatechange = function(vm) {
						if (this.readyState == 4 && this.status == 200) {
							let response = this.response;

							// vue di luar function ajax dapat mengakses this.filename
							vm.filename = response.data.filename;

							// mengirim data ke parent component
							vm.$emit('eventChange', vm.filename);

							vm.progressActive = false;
							vm.createVideo(vm.filename);
						}	
					}.bind(xhr, this); // menggunakan method bind dari vue untuk dapat mengakses this bawaan vue
				
					let data = new FormData();
					data.append('video', files[0]) 

					xhr.open('POST', 'http://menuju/endpoint/api');
					xhr.send(data);

					this.formatValidation = false;
				}
			});
		},
		createVideo(filename) {
			this.video = 'http://menuju/upload/path/' + filename;
    	},	
		removeVideo() {
			this.video = '';
			this.videoData = '';
			this.formatValidation = true;
		},
		removeVideoExist() {
			this.video = '';
			this.videoData = '';
			this.formatValidation = true;
		}
	},
	inject: ['$validator']
}
</script>

<style>
.btn-remove {
	color: #fff !important;
    margin-bottom: 7px;
}
.progress {
  position: relative;
}

.progress span {
    position: absolute;
    display: block;
    width: 100%;
    color: #7d7d7d;
}
</style>
```

Nah segitu dulu ketak ketik curhatan dan pengalaman saya dalam membuat fitur upload menggunakan progress bar pada vue dan ajax. Jika ada yang ingin bertanya kita diskusikan di kolom komentar ya.
