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

---

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
(MONOLITHIC)[Arch/MONOLITHIC.excalidraw.png]


## Distributed Architecture
- Distributed Architecture biasanya terdiri dari beberapa aplikasi yang bekerja bersamaan, berbeda dengan aplikasi Monolithic yang hanya satu aplikasi
- Biasanya dalam Distributed Architecture, setiap unit aplikasi disebut Service
- Pengembangan Distributed Architecture memang lebih kompleks dibandingkan dengan Monolithic Architecture, namun menawarkan keuntungan seperti toleransi kesalahan (fault tolerance), misal ketika ada satu Service gagal, Service lain masih bisa berjalan dengan normal

contoh gambar :
(DISTRIBUTED)[Arch/DISTRIBUTED.excalidraw]

## Pilih yang Mana?
- Pada saat pembuatan aplikasi, kadang kita bingung menentukan harus menggunakan arsitektur Monolithic atau Distributed
- Jika aplikasi yang akan kita buat sederhana atau masih belum jelas secara business requirement, sangat disarankan menggunakan Monolithic, karena mudah untuk dibuat
- Namun jika aplikasi yang akan kita buat sudah sangat jelas, dan juga memang membutuhkan scalability yang baik, termasuk fault tolerance yang baik, maka kita bisa gunakan arsitektur Distributed

* Note:
- Team sedikit pakai monolithic
- Team sudah banyak/besar pakai distributed

---

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
(TECHNICAL PARTIONING)[Arch/TECHNICAL-PARTIONING.excalidraw.png]


## Domain Partitioning
- Berbeda dengan Technical Partitioning, dalam Domain Partitioning, pembagian struktur komponen dalam system dikelompokan berdasarkan Domain / Fitur / Departemen secara bisnis nya
- Artinya semua Technical Partitioning seperti Presentation, Business Logic, Persistence, digabung dalam per domain system
- Partisi system berdasarkan Domain, populer sejak dikenalkan dalam buku berjudul “Domain Driven Design” karya Eric Evans.

contoh gambar :
(DOMAIN PARTIONING)[Arch/DOMAIN-PARTIONING.excalidraw.png]

## Pilih yang Mana?
- Technical ataupun Domain Partitioning memiliki kelebihan dan kekurangan, namun secara pribadi saya rekomendasikan gunakan :
- Technical Partitioning ketika aplikasi yang akan dibuat sederhana dan tidak kompleks. Selain itu ketika jumlah anggota tim pengembang tidak terlalu banyak.
- Domain Partitioning ketika aplikasi yang akan dibuat kompleks dan fitur aplikasi sangat banyak. Dan sangat cocok ketika anggota tim pengembang banyak pembagian tim berdasarkan jenis domain nya.

inti :
- solo dev (project level low-mid) pakai technical partiotioning
- team besar (project level high) pakai domain partiotioning

---

# Layered Architecture

## Layered Architecture

- Layered Architecture dikenal juga dengan N-Tier Architecture
- Arsitektur ini adalah salah satu standard arsitektur untuk kebanyakan aplikasi, dimana aplikasi dibagi secara teknis
- Karena arsitektur ini sangat banyak diketaui oleh pengembang perangkat lunak, hal ini menjadikan arsitektur ini menjadi salah satu pilihan yang banyak digunakan

## Komponen Layered Architecture

- Layered Architecture dibagi secara teknis, oleh karena itu komponen-komponen dalam Layered Architecture sama dengan Technical Partition
- Rata-rata banyak yang membagi menjadi 4 layer, Presentation Layer, Business Layer, Persistence Layer dan Database Layer. Namun tidak menutup kemungkinan bisa lebih banyak layer ketika aplikasi sangat kompleks contoh ada meessage broker tambahin producer layer

contoh gambar :
(LAYERED-ARCHITECTURE)[Arch/LAYERED-ARCHITECTURE.excalidraw.png]

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

(FLOW-LAYERED-ARCH)[Arch/FLOW-LAYERED-ARCH.excalidraw.png]


## Pertimbangan

- Layered Architecture adalah salah satu arsitektur yang banyak dimengerti dan digunakan
- Gunakan Layered Architecture jika kita masih bingung ingin menggunakan arsitektur apa, karena ini adalah arsitektur yang bagus untuk memulai aplikasi
- Namun jangan gunakan Layered Architecture ketika pembagian tim di perusahaan sudah berdasarkan domain, lebih cocok menggunakan Distributed Architecture yang akan kita bahas di materi-materi selanjutnya


# Model-View-Controller Architecture

## Model-View-Controller Architecture

- Model View Controller (MVC) Architecture merupakan salah satu arsitektur yang sangat populer ketika membuat Web
- MVC mirip dengan Layered Architecture, hanya saja layer nya berbeda
- dalam MVC pusat interaksi antar layer biasanya dilakukan oleh Controller

contoh gambar :
(MVC-ARCH)[Arch/MVC-ARCH.excalidraw.png]

## Tugas Tiap Layer

- Dalam MVC, setiap layer memiliki tugas masing-masing
- Controller adalah layer yang bertanggung jawab untuk menerima request dan mengekseksi logika dan aturan bisnis yang terjadi di aplikasi
- Model merupakan layer yang bertanggung jawab sebagai representasi data di database dan juga berinteraksi ke database
- View merupakan layer yang berisi template untuk tampilan halaman web
- Database merupakan layer untuk menyimpan data aplikasi


* note : kareena mvc ini mirip seperti layered architecture jadi nanti saat web nya sudah besar kalau mau controller nya tidak handle banyak code bisa tambah business/service layer, itu sah sah saja.

## Contoh
- Misal kita memiliki web, dimana terdapat satu halaman web yang perlu untuk menampilkan data wishlist. Pada halaman itu, selain data wishlist, kita juga harus menampilkan detail product yang terdapat di wishlist tersebut

contoh gambar :
(MVC-LAYERED)[Arch/MVC-LAYERED.excalidraw.png]

## Pertimbangan

- MVC adalah pilihan utama ketika kita ingin membuat aplikasi Web.
- Banyak sekali web framework yang sudah secara otomatis menggunakan arsitektur ini, seperti Laravel, NestJS, Rails, Django, dan lain-lain
- Namun ketika kita membuat RESTful API misalnya, kita tidak wajib mengikuti aturan MVC, karena biasanya kita tidak memerlukan layer View. Pada kasus seperti ini, kita bisa saja menggabungkan arsitektur MVC ini dengan Layered Architecture

* noted : kalo mau bikin RESTful API kita tidak membutuhkan layer view

---

# Hexagonal Architecture

## Hexagonal Architecture
- Salah satu masalah ketika menggunakan Layered Architecture adalah, perubahan jenis teknologi yang digunakan, akan menyulitkan pengembangan
- Contoh ketika kita harus mengganti teknologi database yang digunakan, secara otomatis akan terjadi perombakan besar-besaran pada kode program Persistence Layer dan Database Layer, yang mungkin bisa menyebabkan perombakan juga pada Business Layer
- Hexagonal Architecture merupakan arsitektur yang dikembangkan dari Layered Architecture, namun lebih baik ketika menangani perubahan teknologi yang terjadi
- Hexagonal Architecture juga dikenal dengan Port & Adapter Architecture

contoh gambar : 
(HEXAGONAL-ARCH)[Arch/HEXAGONAL-ARCH.excalidraw.png]

## Port dan Adapter

- Hexagonal Architecture sebenarnya dikenalkan dengan nama Port & Adapter Architecture, namun saat ini lebih populer dengan nama Hexagonal Architecture
- Port merupakan technology-agnostic yang merupakan sebuah OOP (Object Oriented Programming) Interface, yang digunakan oleh kode aplikasi untuk berkomunikasi dengan pihak lain
- Adapter merupakan bagian yang berinteraksi langsung melalui Port, dan menggunakan teknologi yang sudah ditentukan

contoh gambar :
(HEXAGONAL-DETAIL-ARCH)[Arch/HEXAGONAL-DETAIL-ARCH.excalidraw.png]


## Application Layer
- Dalam Hexagonal Architecture, Application Layer akan berisi logika dan aturan bisnis, dimana kode yang dibuat akan berinteraksi hanya dengan Port, tanpa tahu Adapter yang digunakan
- Hal ini agar tercapainya technology agnostic, sehingga ketika Adapter diubah, Application Layer tidak perlu berubah
- Selain itu, di dalam Application Layer, terdapat Domain Layer, yang berisikan representasi data domain aplikasi
- Bagian Driving, biasanya disebut User Interface / Transport Layer / Gateway, yaitu sumber request masuk ke aplikasi
- Bagian Driven, biasanya disebut Infrastructure / Data Source / External Interfaces, yaitu target data keluar dari aplikasi

contoh gambar :
(DETAILING-HEXAGONAL)[Arch/DETAILING-HEXAGONAL.excalidraw.png]


## Clean Architecture

- Saat ini, terkenal juga arsitektur dengan nama Clean Architecture
- Jangan bingung jika harus memilih Hexagonal Architecture atau Clean Architecture
- Sebenarnya Clean Architecture tidak berbeda jauh dengan Hexagonal Architecture, bahkan penulis buku Clean Architecture pun memang sudah menjelaskan bahwa salah satu referensi Clean Architecture adalah dari Hexagonal Architecture
- `https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html` 

* note : jangan bingung, salah satu referensi clean arch itu dari hexagonal arch


## Contoh
- Misal kita memiliki aplikasi Web toko online, dimana kita akan membuat halaman pembayaran
- Dimana saat melakukan pembayaran, data pembayaran selain disimpan di database, akan dikirim ke Payment Gateway

contoh gambar :
(HEXAGONAL-ARCH-EXAMPLE)[Arch/HEXAGONAL-ARCH-EXAMPLE.excalidraw.png]

## Pertimbangan
- Hexagonal Architecture merupakan salah satu arsitektur yang bagus digunakan ketika kita akan membuat aplikasi yang kompleks dan agar aplikasinya technology agnostic sehingga mudah diganti-ganti
- Namun pengembangan Hexagonal Architecture tidaklah semudah Layered Architecture, sehingga jika pengembang belum familiar dengan Hexagonal Architecture, maka bisa terkendala ketika proses pengem


* note : pengembangan hexagonal arch tidak semudah layered architecture

---

# Microkernel Architecture


## Microkernel Architecture

- Microkernel Architecture sangat flexible dan extensible, sehingga pengembang bisa dengan mudah menambah fitur pada aplikasi dalam bentuk extension atau plug-in, tanpa takut menggangu fungsionalitas dari aplikasinya itu sendiri
- Oleh karena itu, Microkernel Architecture juga sering disebut sebagai Plug-in Architecture
- Microkernel Architecture sangat banyak digunakan pada aplikasi berbasis desktop biasanya, contoh seperti Visual Studio, Eclipse, dan lain-lain, dimana ada banyak sekali extension atau plug-in yang tersedia untuk aplikasi tersebut

contoh gambar :
(MICROKERNEL-ARCH.excalidraw)[Arch/MICROKERNEL-ARCH.excalidraw.png]


## Topology
- Dalam Microkernel Architecture, hanya terdapat dua layer, Core System dan juga Plugin Modules
- Logic aplikasi di Microkernel Architecture biasanya di tempatkan di Plugin, maka dari itu arsitektur ini terlihat seperti Hexagonal Architecture, tetapi sebenarnya bukan
- Dalam Hexagonal Architecture, logic aplikasi ditempatkan di aplikasi, sedangkan di Microkernel Architecture, detail logic aplikasi berada di tiap Plugin masing-masing

* note : intinya pada arch ini logic ada di plugin component nya bukan di core system nya

## Pertimbangan

- Microkernel Architecture sangat populer digunakan ketika membuat aplikasi Desktop berbasis plugin, seperti Visual Studio, Eclipse, IntelliJ IDEA, bahkan seperti Browser
- Gunakan Microkernel Architecture jika memang kita ingin membuat aplikasi yang berbasis Plugin
- Salah satu keunggulan Microkernel Architecture adalah, orang lain bisa berkontribusi membuat Plugin untuk aplikasi kita

---

# Client-Server Architecture

## Client-Server Architecture

- Client-Server Architecture adalah salah satu Distributed System Architecture yang banyak dikenal dan digunakan
- Arsitektur ini sangat sederhana, dimana terdapat aplikasi Client dan Server yang saling berinteraksi
- Biasanya komunikasi antara Client dan Server menggunakan jaringan TCP/IP, walaupun tidak tertutup kemungkinan menggunakan jaringan lain seperti UDP

* note : arch ini masuk kategori distributed arch

contoh gambar :
(CLIENT SERVER)[Arch/CLIENT-SERVER-ARCH.excalidraw.png]

## Topology

- Client adalah aplikasi yang digunakan oleh pengguna dan berinteraksi langsung dengan pengguna. Biasanya aplikasi Client adalah aplikasi yang berbasis user interface, baik itu berupa Web, Desktop atau bisa saja berbasis Terminal. Client akan mengirim semua request yang diminta oleh pengguna ke aplikasi Server
- Server adalah inti dari aplikasi, dimana semua logika dan aturan bisnis dilakukan di Server. Server akan menjalankan perintah yang diminta oleh Client, dan mengembalikan data yang diminta ke Client
- Keuntungan menggunakan Client-Server Architecture adalah, kita bisa menjalankan banyak aplikasi jenis Client hanya dengan menggunakan satu aplikasi Server

contoh gambar :

(CLIENTS SERVER)[Arch/CLIENTS-SERVER.excalidraw.png]


## Contoh

- Client-Server Architecture adalah arsitektur yang saat ini sangat banyak digunakan, bahkan mungkin secara tidak sadar, kita sudah menggunakan arsitektur ini
- Ketika kita membuat aplikasi Mobile lalu terkoneksi ke RESTful API, sebenarnya itu adalah Client-Server Architecture

## Pertimbangan
- Saat kita ingin membuat aplikasi, yang ingin memisahkan aplikasi Client, atau bahkah membuat banyak jenis aplikasi Client, maka arsitektur Client-Server sangat cocok digunakan
- Namun jika kita ingin membuat aplikasi yang standalone, tidak butuh menyimpan data di Server, maka kita tidak butuh membuat aplikasi Client-Server, contoh seperti aplikasi editor untuk Photo, Video dan sejenis nya


# Master-Slave Architecture


## Master-Slave Architecture

- Master-Slave Architecture merupakan arsitektur dimana aplikasi bisa mendistribusikan semua pekerjaannya ke aplikasi lain
- Aplikasi yang memerintah semua aplikasi dinamakan Master, dan aplikasi yang menerima perintah dinamakan Slave
- Sama seperti Client-Server Architecture, biasanya komunikasi Master dan Slave menggunakan jaringan TCP/IP

contoh gambar :

(MASTER SLAVE ARCH)[Arch/MASTER-SLAVE-ARCH.excalidraw.png]

## Topology

- Biasanya, semua request ke aplikasi akan lewat aplikasi Master
- Semua pekerjaan yang diterima oleh Master, biasanya akan didistribusikan ke aplikasi Slave
- Dengan begitu, pekerjaan bisa lebih cepat karena bisa dikerjakan oleh banyak aplikasi Slave
- Pada diagram, kita hanya membuat 3 aplikasi Slave, namun kenyataanya, tidak ada batasan jumlah aplikasi Slave


## Fault Torelance

- Salah satu keunggukan Master-Slave Architecture adalah, Fault Torelance, dimana ketika misal terjadi kesalahan yang menyebabkan salah satu aplikasi Slave mati, maka pekerjaan masih bisa dikerjaan oleh aplikasi Slave lain
- Begitu juga ketika terjadi masalah yang menyebabkan aplikasi Master mati, maka secara otomatis salah satu Slave akan dipromosikan menjadi Master baru

contoh gambar :
(FAULT TOLERANCE)[Arch/FAULT-TOLERANCE.excalidraw.png]

## Master-Master Architecture

- Salah satu kekurangan Master-Slave Architecture adalah, semua request harus melewati aplikasi Master, sehingga ketika terjadi kelambatan di aplikasi Master, secara otomatis semua pengguna akan merasakan dampaknya, padahal mungkin aplikasi Slave tidak lambat
- Pada kasus seperti itu, terdapat juga Master-Master Architecture, dimana ini adalah improvement dari Master-Slave Architecture
- Yang membedakan Master-Master Architecture adalah, semua aplikasi bisa berjalan sebagai Master, sehingga bisa menerima request dari pengguna secara mandiri
- Dan karena semua aplikasi adalah Master, tiap aplikasi bisa saja mengirim request ke aplikasi lain, ketika misal aplikasi sudah terlalu banyak menangani request dari pengguna

contoh gambar :
(MASTER MASTER ARCH)[Arch/MASTER-MASTER-ARCH.excalidraw.png]

## Replication

- Yang perlu diingat dalam Master-Slave / Master-Master Architecture, biasanya data di aplikasi selalu di replikasi/duplikasi ke semua Slave / Master
- Hal ini agar data selalu sama, sehingga pekerjaan yang dikerjakan oleh semua aplikasi Slave/Master akan selalu konsisten

## Contoh
- Master-Slave / Master-Master Architecture jangan digunakan ketika membuat aplikasi sistem informasi, arsitektur ini rata-rata sering digunakan ketika kita membuat aplikasi statefull (menyimpan data) seperti sistem basis data
- Hampir kebanyakan aplikasi sistem basis data seperti MySQL, PostgreSQL, MongoDB mengimplementasikan arsitektur Master-Slave Architecture
- Atau seperti aplikasi sistem basis data Elasticsearch dan Cassandra, yang menggunakan Master-Master Architecture


## Pertimbangan
- Gunakan arsitektur Master-Slave / Master-Master ketika kita ingin membuat aplikasi yang statefull (menyimpan data)
- Jika kita membuat aplikasi yang stateless (tidak menyimpan data), maka disarankan tidak menggunakan arsitektur ini



# SLIDE
`https://docs.google.com/presentation/d/1eaaTjJLmRcSMQK9VfT3zHuEJp7LXL-uJVuFw4e6ZIcM/edit`