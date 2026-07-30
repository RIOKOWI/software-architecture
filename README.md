# SLIDE
`https://docs.google.com/presentation/d/1eaaTjJLmRcSMQK9VfT3zHuEJp7LXL-uJVuFw4e6ZIcM/edit`


# Pengenalan Software Architecture Patterns

## Pendahuluan
- Sudah jadi hal yang biasa, biasanya programmer akan membuat perangkat lunak tanpa persiapan yang matang
- Hal ini kadang membuat perangkat lunak menjadi sulit di-maintain dan dikembangkan ketika sudah terlanjur dibuat
- Oleh karena itu, sebelum melakukan pembuatan perangkat lunak yang besar, salah satu hal yang wajib dilakukan adalah memilih arsitektur yang akan digunakan
- Dalam pemilihan arsitektur perangkat lunak, kita harus mengerti kelebihan dan kekurangannya, agar membantu dalam pembuatan perangkat lunak yang akan dilakukan

## Software Architecture Patterns
- Architecture Patterns merupakan kumpulan gaya arsitektur dalam pembuatan perangkat lunak.
- Ada banyak sekali Architecture Patterns, dan setiap pattern memiliki kelebihan dan kekurangan masing-masing
- Programmer perlu tahu tentang materi ini agar bisa menentukan pattern apa yang akan digunakan ketika membuat perangkat lunak

## Tujuan Architecture Patterns
- Tiap Architecture Pattern memiliki karakteristik seperti mudah dikembangkan, scalable dan mudah di-maintain
- Harapannya, dengan menggunakan Architecture Pattern yang tepat saat pembuatan perangkat lunak, hasil perangkat lunak yang kita buat akan mudah dikembangkan kedepannya


# Architecture Classification

## Architecture Classification
- Software Architecture Patterns secara garis besar dibagi menjadi dua klasifikasi, Monolithic dan Distributed
- Monolithic adalah single deployment unit atau berjalan dalam satu aplikasi
- Distributed adalah multiple deployment units, biasanya terdiri dari beberapa aplikasi


## Monolithic Architectures
- Monolithic Architecture secara umum sangat sederhana dibandingkan Distributed Architecture
- Desain aplikasi Monolithic lebih sederhana dan mudah diimplementasikan
- Aplikasi yang menggunakan Monolithic Architecture akan sangat cepat dibuat dan didistribusikan

contoh gambar :
(MONOLITHIC)[MONOLITHIC.excalidraw.png]


## Distributed Architecture
- Distributed Architecture biasanya terdiri dari beberapa aplikasi yang bekerja bersamaan, berbeda dengan aplikasi Monolithic yang hanya satu aplikasi
- Biasanya dalam Distributed Architecture, setiap unit aplikasi disebut Service
- Pengembangan Distributed Architecture memang lebih kompleks dibandingkan dengan Monolithic Architecture, namun menawarkan keuntungan seperti toleransi kesalahan (fault tolerance), misal ketika ada satu Service gagal, Service lain masih bisa berjalan dengan normal




