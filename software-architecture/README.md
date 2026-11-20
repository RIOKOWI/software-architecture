# SOFTWARE ARCHITECTURE

# MESSAGE BROKER

## STUDY CASE
- contoh ada 2 service: order service dan notification service
- ketika ada user request ke order service, order service akan memanggil/atau menggunakan notification service untuk mengirim status pembayaran dll ke user
- jika notification service maka sistem tidak bisa digunakan. nah disinilah message broker ber peran untuk mengatasi case tersebut

## MESSAGE BROKER
- rabbitmq, apache kafka, dll
- sebelumnya order service akan kirim data langsung ke notif service
- dengan adanya message broker diantara 2 service tersebut, order service akan mengirim message/data ke message broker lalu notif service mengambil message/data ke message broker

## KEUNTUNGAN
- tidak ketergantungan. jadi sebelum ada message broker order service sangat ketergantungan dengan notification service, kalo notif service mati order service juga akan terganggu
- dengan adanya message broker di tengan tengah, order service tidak akan ketergantungan lagi ke notification service. mati/hidup notif service order service masih akan berjalan
- jadi ketika notif service mati dan sudah di fix, service tersebut akan menerima data yang tersimpan di message broker dan akan di eksekusi. sebelumnya tanpa message broker kan data nya hilang di tengah tengah jika 1 service mati
- LOAD BALANCE. ketika order service bisa handle 3 request per second tetapi notif service hanya bisa 1 request per second, maka notif service akan memproses request 1 per 1 dari message broker. tetapi teknik tersebut juga akan lama nah akalin nya dengan cara menambah 3 notif service lagi agar terdistribusi task task nya dan menaikkan performa service

## KEKURANGAN/TANTANGAN
- order dan notif service sekarang kertergantungan ke message broker, jadi kalo message broker bermasalah 2 service tersebut akan mengalami gangguan/masalah
- pilih message broker yang bagus contoh : apache kafka & rabbitMQ
- paradigma cara kerja aplikasi akan berbeda, jadi kita tidak tahu request order service gagal atau sukses, karna order service cuma ngirim data ke message broker, synchronous menjadi asynchronous

## SOURCE
- [https://www.youtube.com/watch?v=-s3SqRPYOKM&list=PL-CtdCApEFH_-3qaCDJyZnU1U7z_kzrsQ](https://www.youtube.com/watch?v=-s3SqRPYOKM&list=PL-CtdCApEFH_-3qaCDJyZnU1U7z_kzrsQ)

contoh di file
- [software-architecture\message-broker.excalidraw.png](software-architecture\message-broker.excalidraw.png)


# SERVICE DISCOVERY
## STUDY CASE
- ada 3 service : shoping cart service, order service, payment service
- pada suatu saat aplikasi order service lemot/lambat sekali, nah bisa juga dengan menambah 3 order service lagi untuk mengatasinya. proxy server(nginx dll) yang akan mendistribusikan ke 3 order service
- nah misal payment service jadi lambat juga, ya sama dengan mengatasi order service yang lemot dengan cara menambah 2 payment service lalu order service di distribusikan via proxy server -> 2 payment service
- tetapi aplikasi lama kelamaan lemot nah secara tidak sadar semua request masuk ke proxy server, dan jika proxy server mati semua service juga akan terganggu 
- nah gimana kalo kita menambah proxy server yang menghandle tiap service?, nah kalo 1 proxy server mati service yang di handle oleh proxy server tersebut tidak akan jalan
- nah gimana kalo tiap service punya 2 proxy server? bagaimana nanti kedepannya ada service lain dan banyak node tiap service? nambah 10++ proxy server dan ubah konfigurasi secara manual? boros & ribet
- jawaban nya pakai SERVICE DISCOVERY

## SERVICE DISCOVERY
- Service discovery adalah mekanisme dalam arsitektur software untuk mencari tahu service lain berada di mana dan bagaimana cara menghubunginya.
- untuk implemen service discovery harus implemen aplikasi service registry(consul, eureka dll). service registry itu akan menyimpan semua list lokasi semua service dan semua endpoint endpoint service nya

## KEUNTUNGAN
- gimana kalo 1 node service mati 
- jadi di service registry ada fitur yang namanya healtch check yang biasanya service registry tiap 5-10 detik mengharuskan cek tiap semua node service secara berkala. 
- jika service resgistry tidak mendapatkan response semestinya maka service registry akan menandai service tersebut bermasalah dengan begitu service registry akan mengembalikan node/service yang sehat ke service lain yang ingin mengakses. 
- jadi jika ada service bermasalah, service lain tidak di berikan request oleh service registry

## TANTANGAN
- jadi jika ada 10 request dari client, service registry harus bisa mendistribusikan 10 request tersebut
- harus implemen client load balancer pada service

## SOURCE
- [https://www.youtube.com/watch?v=rt2MWGNu7m8&list=PL-CtdCApEFH_-3qaCDJyZnU1U7z_kzrsQ&index=2](https://www.youtube.com/watch?v=rt2MWGNu7m8&list=PL-CtdCApEFH_-3qaCDJyZnU1U7z_kzrsQ&index=2)

contoh di file
- [software-architecture\service-directory.excalidraw.png](software-architecture\service-directory.excalidraw.png)


# JSON WEB TOKEN
## STUDY CASE
- toko online
- sebelum kita akses semua fitur/service kita wajib akses session/auth service dan menghasilkan cookies, nah cookies tersebut untuk mengakses fitur/service yang tersedia di web.
- konsekuensi nya app/web ketergantungan dengan session service yang mengakibatkan jika session service lambat/gangguan semua service akan terganggu juga
- gimana kalo cookies nya di simpan di database(mysql, redis, dll)? tentu bisa cepat, tapi tidak scalable dan ribet jika ingin mengubah column database nantinya 
- JSON WEB TOKEN/JWT adalah solusi dari kedua masalah metode tersebut, gak perlu nembak service gak perlu cek database dan mudah di implemen

## JSON WEB TOKEN
- jwt itu standar spesifikasi
- di rekomendasikan pake library yang sudah di sediakan contoh dari laravel gunakan sanctum
- jwt adalah metode pertukaran data antara 2 belah pihak secara aman
- token? token adalah semacam identitas/id
- token di jwt terdiri dari 3 bagian(header, payload & signature) di pisahkan oleh titik kalo di lihat seperti random string
```json
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjMiLCJuYW1lIjoiUmlvIiwicm9sZSI6ImFkbWluIiwiaWF0IjoxNzAwMDAwMDAwLCJleHAiOjE3MDAwMDM2MDB9.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

# eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9 adalah header
# eyJzdWIiOiIxMjMiLCJuYW1lIjoiUmlvIiwicm9sZSI6ImFkbWluIiwiaWF0IjoxNzAwMDAwMDAwLCJleHAiOjE3MDAwMDM2MDB9 adalah payload yang berisi data data umum user seperti name dan email
# SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c adalah siganture gabungan dari hashing dari base64 header dan base64 payload dan disisipkan oleh secret key
# ketiga bagian tersebut dipisahkan oleh .
```

- jadi flow nya : user login -> dapet jwt -> pakai jwt tersebut untuk akses service service lain

## KEKURANGAN
- tiap service harus hashing algoritma yang sudah di tentukan
- semakin berat algoritma yang di gunakan semakin besar cpu yang di pakai
- solusinya jangan gunakan algoritma yang berat

## SOURCE
- [https://www.youtube.com/watch?v=_w9ixlwUsgg&list=PL-CtdCApEFH_-3qaCDJyZnU1U7z_kzrsQ&index=3](https://www.youtube.com/watch?v=_w9ixlwUsgg&list=PL-CtdCApEFH_-3qaCDJyZnU1U7z_kzrsQ&index=3)

contoh di file
- [software-architecture\jwt.excalidraw.png](software-architecture\jwt.excalidraw.png)