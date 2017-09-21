---
layout: post
title:  'Hijrah (lagi) ke Vim'
image: '/assets/img/hijrah-ke-vim/brain_after.png'
date:   2017-09-21 00:05:03
tags:
- Review
description: 'Sehingga setelah itu beberapa waktu saya kembali memakai Sublime. Tujuan saya hijrah editor adalah karena saya mencari yang build in atau setidaknya ada plugin untuk memakai terminal sehingga saya tidak perlu membuka jendela aplikasi baru dan bisa menghemat memori. Sempat ingin menggunakan Visual Studio, dan kebetulan saya sedang sering menggunakan Typescript dan Angular, yang sangat di dukung penuh oleh Visual Studio khususnya pemakaian Typescript yang sama2 product dari Microsoft.'
categories:
- My Daily Posts
serie: Review
---

![logo vim brain](/assets/img/hijrah-ke-vim/brain_after.png)

<p style="text-align: center;">Your brain on Vim by <a href="http://kevinw.github.io/2010/12/15/this-is-your-brain-on-vim/">Kevin Watters</a></p>

Untuk yang kedua kalinya atau kalo kata Raisa kali kedua :D saya harus migrasi text editor setelah sebelumnya saya menggunakan Atom dan saya juga sempat menulis reviewnya [migrasi ke atom](/migrasi-ke-atom) di blog ini. Akhirnya saya sudah tidak bisa mentolerir kekurangan dari Atom editor yaitu **Lemot**. Banyak pemakai editor ini pun mengeluhkan demikian, aplikasi editor yang keren setidaknya yang termasuk dari jajaran editor ternama dan tercanggih harus menghabiskan sebagian besar memori dari laptop saya. Dan itu sangat tidak cocok dengan memori ngepas yang saya pakai dan kebutuhan untuk multitasking.

Sehingga setelah itu beberapa waktu saya kembali memakai Sublime. Tujuan saya hijrah editor adalah karena saya mencari yang build in atau setidaknya ada plugin untuk memakai terminal sehingga saya tidak perlu membuka jendela aplikasi baru dan bisa menghemat memori. Sempat ingin menggunakan Visual Studio, dan kebetulan saya sedang sering menggunakan Typescript dan Angular, yang sangat di dukung penuh oleh Visual Studio khususnya pemakaian Typescript yang sama2 product dari Microsoft.

Tetapi atas saran dari mas [Agung Setiawan](https://agung-setiawan.com/) yang juga merupakan software engineer di bukalapak, menyarankan saya untuk memakai Vim. Dulu saya pernah mencoba Vim tetapi kesan pertama ketika saya mencoba adalah **SUSAH**, dan saya berfikir tidak mempunyai waktu untuk mempelajarinya. Tetapi setelah saya googling saya menemukan banyak manfaat dari memakai Vim, selain dengan memakai Vim kita lebih terlihat GEEK dan beda! hehee, Vim juga dapat mengefisiensikan pekerjaan kita, karena semuanya di operasikan dengan keyboard tanpa mouse. Dan yang terpenting adalah build in di terminal kita jika kita menggunakan Linux atau OSX. Karena susahnya saya harus mempelajari dan memakai Vim selama beberapa bulan baru berani untuk menulis artikel ini. hehee.

![comment mas agung](/assets/img/hijrah-ke-vim/comment-agung.png)

## Harus Memakai Plugin

Saya banyak belajar dari mas Agung mengenai Vim lewat website [idrails](https://idrails.com) nya. Sebagai text editor tercanggih, terenteng, dan tercepat Vim tidak dapat berdiri sendiri, Vim membutuhkan banyak plugin tambahan sehingga Vim menjadi text editor yang powerfull dan lebih mengefesiensikan tugas kita sebagai seorang programmer. Di akhir nanti saya akan memberitahukan configurasi dan plugin apa saja yang saya pakai di Vim lewat file `~/.vimrc`.

## Fokus Pada Tampilan

![screenshot Vim](/assets/img/hijrah-ke-vim/screenshot-vim.png)

<p style="text-align: center;">Screenshot Vim ketika saya menulis artikel ini</p>

Hal yang pertama saya fokuskan adalah membuat tampilan Vim seindah mungkin, karena menurut saya dengan kita fokus pada tampilan dan keindahan penglihatan mata kita di text editor membuat kita semangat belajar dan ngoprek. Berikut adalah daftar plugin dan konfigurasi yang saya pakai berkaitan dengan tampilan di Vim.

- [Solarized](http://ethanschoonover.com/solarized), untuk themes saya menggunakan solarized yang saya gunakan sebagai background dari Vim.

- [Base16 Vim](https://github.com/chriskempson/base16-vim), untuk colorscheme dan syntax highlight saya menggunakan Base16.

- [Vim Airline](https://github.com/vim-airline/vim-airline), dengan plugin ini di layar Vim kita dapat terlihat barbagai status dari mode Vim, nama file, status git, serta tab file yang sedang terbuka, dan masih banyak lagi.

- [Nerdtree](https://github.com/scrooloose/nerdtree), ini berfungsi sebagai tree explorer yang dapat muncul pada sidebar Vim.

## Fungsi Pada Umumnya Editor

Setelah tampilan cukup indah dan sedap di pandang, sekarang saatnya fokus pada fungsi. Supaya migrasi dari editor biasa ke Vim gak kaget saya mencari tahu gimana caranya secara fungsional dan environment mendekati editor saya sebelumnya yaitu sublime. Yaitu dengan menyesuaikan layout dengan Nerdtree dan plugin2 lain yang sudah saya jelaskan sebelumnya, merubah indentation tab, menginstall plugin autocomplete atau word suggestion, dan masih banyak lagi. Saya akan bahas satu persatu hal tersebut, tetapi sebelumnya buat sebuah file konfigurasi `.vimrc` yang terdapat di root. Kita akan menaruh file konfigurasi kita di sana.

- [Vundle](https://github.com/VundleVim/Vundle.vim), merupakan plugin manajemen. Kita bisa mengaturnya pada file `.vimrc`, dan dapat menginstall atau mengupdate plugin kita hanya dengan perintah `:PluginInstall` pada Vim.

- [Neocomplcache](https://github.com/shougo/neocomplcache.vim), plugin ini dapat memberi suggestion ketika kita mengetik. Dan yang paling saya suka plugin ini dapat menyimpan dan memberi suggestion dari kata yang sudah kita ketikan sebelumnya di file yang sama.

- [Emmet Vim](https://github.com/mattn/emmet-vim), plugin ini dapat menggenerate blok kode secara otomatis.

- [Vim Indent Guides](https://github.com/nathanaelkane/vim-indent-guides),
plugin ini berfungsi untuk penujuk indent tab, tetapi keseringan saya menon aktifkannya karena saya suka working space directory yang bersih, yang berisi hanya kodingan saja.

- Set number, konfigurasi yang memunculkan nomer baris pada editor. Cara mengaktifkannya dengan cara menambahkan `set number` pada file `.vimrc`.

- Jarak indent tab, kita dapat mengatur seberapa dalam jarak tab dengan menambahkan `set tabstop=2` di file `.vimrc`, konfigurasi terbut akan memberi jarak sebanya 2 spasi ketika kita menekan tombol tab. Jika kita ingin merubah jarak tab tinggal rubah nilai dari `tabstop`.

- Syntax enable, untuk mengaktifkan syntax highlight pada Vim. Cara dengan menambahkan `syntax enable` di file `.vimrc`.

## Konfigurasi Vim

Berikut saya akan share konfigurasi Vim saya di file `.vimrc`. Silahkan di copy di file `.vimrc` temen2 masing, untuk menginstall pluginnya lewat [Vundle](https://github.com/VundleVim/Vundle.vim) bisa dengan menulis syntax `:PluginInstall` pada Vim.

```
" konfigurasi require dari vundle
set nocompatible
filetype off
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

" daftar plugin yang di install
Plugin 'VundleVim/Vundle.vim'
Plugin 'vim-airline/vim-airline'
Plugin 'vim-airline/vim-airline-themes'
Plugin 'mattn/emmet-vim'
Plugin 'nathanaelkane/vim-indent-guides'
Plugin 'chriskempson/base16-vim'
Plugin 'shougo/neocomplcache.vim'

" konfigurasi require dari vundle
call vundle#end()
filetype plugin indent on

" dokumentasi vundle
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line

" konfigurasi panthogen
execute pathogen#infect()
call pathogen#helptags()

" konfigurasi workspace vim
syntax enable
colorscheme base16-default-dark
set number
set tabstop=2
set t_Co=256
set laststatus=2
set backspace=indent,eol,start

" konfigurasi plugin yang telah terinstall
let g:solarized_termcolors = 256
let g:airline#extensions#tabline#enabled = 1
let g:airline#extensions#branch#enabled = 1
let g:airline_powerline_fonts = 1
let g:neocomplcache_enable_at_startup = 1

" config emmet
let g:user_emmet_settings = {
  \    'indentation' : '  '
  \}

" Mapping for NERDTree
map <C-n> :NERDTreeToggle<CR>
nmap <leader>p :NERDTreeFind<CR>
```

## Tmux dan Theme Terminal

Sebagai tambahan saya juga mau share aplikasi keren lainnya yang mendukung penggunaan Vim dan mengefisiensikan penggunaan teriminal yaitu Tmux. Tmux merupakan aplikasi manajemen terminal, aplikasi ini sangat berguna untuk saya yang sehari2 bekerja menggunakan terminal, untuk menginstall Tmux silahkan menuju [ke sini](https://github.com/tmux/tmux). Di Tmux kita bisa menggunakan beberapa jendela sekaligus dalam satu layar terminal, dan bisa menambahkan halaman sebanyak yang kita mau pada terminal, selain itu Tmux juga bisa menyimpan session pekerjaan kita jika terminal kita tertutup selama komputer tidak di restart, dan masih banyak hal keren lainnya di Tmux.

Hal ke 2 sebagai tambahan dan penyemangat kita bekerja dengan terminal adalah themes yang bernana [powerlevel9k](https://github.com/bhilburn/powerlevel9k). Themes ini dapat memperindah tampilan dari terminal kita, dan dengan themes ini kita dapat melihat status git pada project tersebut yang di indikasikan dengan warna yang berbeda2. Keren kan!

![screenshot tmux](/assets/img/hijrah-ke-vim/screenshot-tmux.png)

<p style="text-align: center;">Screenshot tmux dan themes terminal</p>

## Kesimpulan

Kira2 sudah 4 bulanan terakhir ini saya memakai Vim, dan rasanya tidak ada masalah, editor yang sangat enteng, tujuan saya tercapai sudah build in dengan terminal bahkan bukan build in lagi, tapi ini adalah editor yang berjalan langsung di atas terminal. Oh ya untuk terminal saya menggunakan iTerm untuk OSX. Satu2nya kekurangan Vim adalah satu, yaitu **SULIT** dan membutuhkan waktu yang tidak sedikit untuk mempelajarinya dan membiasakan bekerja dengan editor ini. Tetapi hal ini tidak jadi masalah untuk seorang developer yang memang sifat dasarnya adalah senang belajar.

Saya pun belum selesai belajar Vim, masih banyak shortcut2 dari Vim yang belum saya tahu, maka dari itu ayo kita belajar bareng dengan teman2 bisa komen di bawah artikel ini. Berikut beberapa source yang saya tau dan bisa di jadikan bahan pembelajaran dalam menggunakan Vim.

- [vimawesome.com](https://vimawesome.com/), di website ini merupakan kumpulan dari plugin2 terbaik dari Vim.

- [idrails.com](https://www.idrails.com/), website milik mas Agung Setiawan ini adalah source berbahasa Indonesia yang bagus untuk di pelajari. 

- [Vim tutorials di youtube](https://www.youtube.com/watch?v=5givLEMcINQ&list=PL13bz4SHGmRxlZVmWQ9DvXo1fEg4UdGkr), source video dari playlist youtube ini saya rasa sangat baik untuk di simak dan di pelajari.

- [Vim Indonesia Forum](https://t.me/VimID), melalui forum telegram Imdonesia kita bisa berdiskusi dan mendapat update terbaru mengenai dunia Vim.
