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

contoh gambar :
(DISTRIBUTED)[DISTRIBUTED.excalidraw]

## Pilih yang Mana?
- Pada saat pembuatan aplikasi, kadang kita bingung menentukan harus menggunakan arsitektur Monolithic atau Distributed
- Jika aplikasi yang akan kita buat sederhana atau masih belum jelas secara business requirement, sangat disarankan menggunakan Monolithic, karena mudah untuk dibuat
- Namun jika aplikasi yang akan kita buat sudah sangat jelas, dan juga memang membutuhkan scalability yang baik, termasuk fault tolerance yang baik, maka kita bisa gunakan arsitektur Distributed

* Note:
- Team sedikit pakai monolithic
- Team sudah banyak/besar pakai distributed


# Architecture Partitioning

## Architecture Partitioning
- Selain dilihat secara klasifikasi monolithic atau distributed, software architecture juga bisa dilihat secara struktur partisinya
- Software architecture baik itu monolithic atau distributed, secara partisi bisa dibagi berdasarkan teknis atau domain
- Partisi secara struktur bisa digunakan di monolithic atau distributed

## Technical Partitioning
- Arsitektur Technical Partitioning membagi komponen-komponen dalam system atau aplikasi secara teknis
- Contoh arsitektur klasik yang sering digunakan adalah Layered (N-Tier) Architecture
- Dalam Technical Partitioning, komponen dalam system dikelompokan secara teknis, misal :
- Presentation Layer merupakan komponen yang menangani user interface
- Business Layer, merupakan komponen yang menangani logika dan aturan bisnis
- Persistence Layer, merupakan komponen yang menangani interaksi dengan sistem basis data
- Database Layer, merupakan komponen untuk menyimpan data

* note : layer layer di atas tidak baku, boleh aja tambah layer lagi seperti service layer, api layer, dll

contoh gambar :
[TECHNICAL PARTIONING](TECHNICAL-PARTIONING.excalidraw.png)


## Domain Partitioning
- Berbeda dengan Technical Partitioning, dalam Domain Partitioning, pembagian struktur komponen dalam system dikelompokan berdasarkan Domain / Fitur / Departemen secara bisnis nya
- Artinya semua Technical Partitioning seperti Presentation, Business Logic, Persistence, digabung dalam per domain system
- Partisi system berdasarkan Domain, populer sejak dikenalkan dalam buku berjudul “Domain Driven Design” karya Eric Evans.

contoh gambar :
[DOMAIN PARTIONING](DOMAIN-PARTIONING.excalidraw.png)

## Pilih yang Mana?
- Technical ataupun Domain Partitioning memiliki kelebihan dan kekurangan, namun secara pribadi saya rekomendasikan gunakan :
- Technical Partitioning ketika aplikasi yang akan dibuat sederhana dan tidak kompleks. Selain itu ketika jumlah anggota tim pengembang tidak terlalu banyak.
- Domain Partitioning ketika aplikasi yang akan dibuat kompleks dan fitur aplikasi sangat banyak. Dan sangat cocok ketika anggota tim pengembang banyak pembagian tim berdasarkan jenis domain nya.

inti :
- solo dev (project level low-mid) pakai technical partiotioning
- team besar (project level high) pakai domain partiotioning


# Layered Architecture

## Layered Architecture

- Layered Architecture dikenal juga dengan N-Tier Architecture
- Arsitektur ini adalah salah satu standard arsitektur untuk kebanyakan aplikasi, dimana aplikasi dibagi secara teknis
- Karena arsitektur ini sangat banyak diketaui oleh pengembang perangkat lunak, hal ini menjadikan arsitektur ini menjadi salah satu pilihan yang banyak digunakan

## Komponen Layered Architecture

- Layered Architecture dibagi secara teknis, oleh karena itu komponen-komponen dalam Layered Architecture sama dengan Technical Partition
- Rata-rata banyak yang membagi menjadi 4 layer, Presentation Layer, Business Layer, Persistence Layer dan Database Layer. Namun tidak menutup kemungkinan bisa lebih banyak layer ketika aplikasi sangat kompleks contoh ada meessage broker tambahin producer layer

contoh gambar :
[LAYERED-ARCHITECTURE](LAYERED-ARCHITECTURE.excalidraw.png)

## Tugas Tiap Layer

- Setiap layer di Layered Architecture memiliki tugas masing-masing
- Presentation Layer bertanggung jawab menangani semua user interface 
- Business Layer bertanggung jawab mengeksekusi permintaan logika atau aturan bisnis yang sesuai permintaan
- Persistence Layer bertanggung jawab untuk berinteraksi dengan database untuk melakukan pengolaha data di database
- Database Layer bertanggung jawab untuk menyimpan semua data aplikasi

## Layers of Isolation
- Dalam Layered Architecture, tiap Layer akan terisolasi, dan arah komunikasi hanya dilakukan dengan layer dibawah atau diatasnya
- Artinya, misal ketika Presentation Layer ingin mengambil data Product, Presentation Layer akan mengirim request ke Business Layer, lalu akan mengambil data ke Persistence Layer dan baru masuk ke Database Layer
- Tidak direkomendasikan untuk langsung mengakses dari Presentation Layer langsung ke Database Layer misalnya

## Contoh
- Setelah tahu cara kerja Layered Architecture, maka kita bisa dengan mudah menggambarkan alur yang harus dibuat ketika membuat aplikasi
- Misal kita ingin menampilkan halaman Product, namu di dalam halaman Product kita juga ingin menampilkan data Seller nya
- Maka kita bisa buat alur aplikasi seperti pada diagram selanjutnya

[FLOW-LAYERED-ARCH](FLOW-LAYERED-ARCH.excalidraw.png)


## Pertimbangan

- Layered Architecture adalah salah satu arsitektur yang banyak dimengerti dan digunakan
- Gunakan Layered Architecture jika kita masih bingung ingin menggunakan arsitektur apa, karena ini adalah arsitektur yang bagus untuk memulai aplikasi
- Namun jangan gunakan Layered Architecture ketika pembagian tim di perusahaan sudah berdasarkan domain, lebih cocok menggunakan Distributed Architecture yang akan kita bahas di materi-materi selanjutnya



