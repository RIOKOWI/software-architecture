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



# DATABASE SCALING
## DATABASE SCALING
- saat data kita sudah menyentuh gb dan tb aplikasi kita sudah mulai lemot
- udah saatnya lakuin database scaling

## VERTICAL SCALING
- solusi sedererhana cukup upgrade hardware nya contoh upgrade storage, ram, cpu. itu solusi pertama kali, cuma biasanya hardware ada limit nya dan price hardwarenya mahal

## HORIZONTAL SCALING
- sama, beli hardware dengan spek hardware yang sama lalu tambah node
- yang tadinya data nya di simpan di 1 server sekarang di simpan di lebih dari 1 server
- untuk menyebar input data ke lebih dari 1 server gunakan routing yang dimana primary key 1 data di modulo% dengan jumlah node/server
- gimana kalo searching data? ya kita harus search ke semua partisi atau server
- gimana kalo paging? kita mengambil 20 data dari tiap server lalu jumlah nya di sorting agar datanya sesuai
- solusi untuk deep paging problem adalah implementasi kriteria rentang waktu
- untuk mengatasi 1 server mati dan gak bisa get data dari server tersebut gunakan teknik replication. tetapi kekurangannya sebelum replication data nya cuma 1 tb setelah replication akan x2 atau 2 tb
- caching: setelah kita mendapatkan data pertama kali setalahnya data di simpan di ram yang mengakibatkan get data sangat cepat(recommendation before implement vertical & horizontal scaling)

## SOURCE
- [https://www.youtube.com/watch?v=wH5LZ6ALxpw&list=PL-CtdCApEFH_-3qaCDJyZnU1U7z_kzrsQ&index=4](https://www.youtube.com/watch?v=wH5LZ6ALxpw&list=PL-CtdCApEFH_-3qaCDJyZnU1U7z_kzrsQ&index=4)

contoh di file
- [software-architecture\database-scaling.excalidraw.png](software-architecture\database-scaling.excalidraw.png)


# API GATEWAY
## STUDY CASE
inventory case :
- disuruh bikin app,web,desktop system yang bisa integrasi third party 
- kita develop backend dan client jika ingin akses fitur via app/web harus connect ke backend kita
- tetapi bayangkan jika bukan 1 aplikasi saja bayangkan 5 - 100 app
- gimana caranya mengakses lebih dari 1 sistem, biasanya kita bedakan jenis aplikasi dari url/endpointnya, contoh :
```json
api.resbob.com/inventory
api.resbob.com/payroll
api.resbob.com/sales
api.resbob.com/......
dst..
```
- gimana caranya app kita bisa akses beda beda endpoint backend? biasanya di tengah tengah antara app dan aplikasi backend di pasang proxy server(nginx, dll), tujuan nya untuk merouting url yang sudah di tentukan ke service yang di tuju
- secara tidak sadar contoh implemen di atas adalah konsep API GATEWAY(proxy server), jarang juga bikin api gateway untuk proxy server doang biasanya untuk centralized authentication
- jadi authentication di buat centralized di api gateway karena agar client bisa akses dari berbagai jenis app, web, desktop dengan auth yang berbeda beda(Oauth, web session, basic auth)
- nah di api gateway juga terjadi authorization atau pembagian role bisa akses aplikasi apa aja contoh admin bisa akses aplikasi finance dan customer cuma boleh akses aplikasi e commerce
- api gateway juga bisa di implemen rate limiter, misal ada lebih dari 2 request di 1 service api gateway akan menolak request tersebut. intinya bisa membatasi request
- api gateway juga bisa menjadi api orchestrator jadi dia bisa get data ke lebih dari 1 service sekaligus
- api gateway juga bisa menjadi standart api yaitu bisa mentranslate data kebeberapa jenis data(soap, xml, rcp, json) untuk tiap service
- api gateway juga bisa menjadi backend for front end yaitu dia bisa mengembalikan data yang dibutuhkan saja ke client yang berbeda beda (mobile, web, desktop, third party, dll) jadi harus membuat 3 api gateway. ada alternatif jika tidak mau membuat 3 api gateway dengan cara implemen graph ql

## KEKURANGAN
- jadi lebih lambat(1/2second) response time nya, karna menambah layer api gateway yang sebelumnya langsung ke service

## SOURCE
- [https://www.youtube.com/watch?v=aZWbBcgS16s&list=PL-CtdCApEFH_-3qaCDJyZnU1U7z_kzrsQ&index=5](https://www.youtube.com/watch?v=aZWbBcgS16s&list=PL-CtdCApEFH_-3qaCDJyZnU1U7z_kzrsQ&index=5)

contoh di file
- [software-architecture\api-gateway.excalidraw.png](software-architecture\api-gateway.excalidraw.png)


# APPLICATION SCALING
## SCALING VERTICAL
- scaling dengan cara meng upgrade hardware aplikasi

## SCALING HORIZONTAL
- tinggal menambah node dan menambah load balancer di tengah tengah
- tetapi aplikasi kita haru stateless yaitu kita tidak boleh menyimpan data dalam aplikasi/state di usahakan simpan data ke dalam database agar data nya konsisten pada tiap node

## NON-BLOCKING
- pada dasar nya sistem i/o atau input output secara synchronous jadi kalo kita bikin 100 thread 100 thread itu harus di kerjakan secara berurutan 
- nah di sini implementasi teknologi non blocking yang bisa menerima request sebanyak apapun

## REACTIVEX
- reactivex adalah membeda bedakan worker pada tiap proses/service yang kita inginkan
- jadi jika ada salah satu fitur yang sibuk fitur lain tidak kena impact nya

## SPLIT APLIKASI
- kita gak perlu scale 1 aplikasi bulat bulat, mungkin cukup 1-2 fitur aja yang sudah saatnya di scale contoh di e commerce yang utama di scaling adalah fitur search product nya
- daripada ribet scale/refactor 1 apk ke non blocking & reactivex lebih baik split aplikasi nya yang low traffic tetap yang high traffic keluarin dan di bikin apk baru di scale bebas mau non blocking atau reactivex yang mana akan lebih mudah untuk scale 1 per 1 fitur dari pada langsung 1 aplikasi

## DATABASE SCALING
- sebenernya investigasi dulu masalah nya di apk atau database, kalo gak ada masalah di apk ngapain kita scaling apk nya
- rata rata biasanya bottleneck nya ada di database nya(query lambat, n+1, dll) lebih baik scaling database nya

## GUNAKAN TOOL BANTUAN
- beberapa teknologi di ciptakan tidak siap by default production tetapi development, jadi pastiin dulu set up aplikasi kita dalam mode production

## OPTIMIZE CODE
- saat awal development pasti code kita belum optimize karna mengejar mvp, seiring berjalan nya waktu kita wajib optimize code
- cara cek nya dari monitoring aplikasi kita by reponse time api or method
- konsekuensi code optimize, code nya tidak readable

## AGGRESIVE CACHING
- jika aplikasi lambat usahakan caching terlebih dahulu
- misal query ke db lambat, dll
- setelah ambil data ke db hasilnya kita cache di ram atau in memory database(redis)


## GA USAH DIPIKIRIN
- kejar mvp dulu/deliver app dulu
- setelah user banyak baru scaling


## SOURCE
- [https://www.youtube.com/watch?v=quWvEH-xp3w&list=PL-CtdCApEFH_-3qaCDJyZnU1U7z_kzrsQ&index=6](https://www.youtube.com/watch?v=quWvEH-xp3w&list=PL-CtdCApEFH_-3qaCDJyZnU1U7z_kzrsQ&index=6)

contoh di file
- [software-architecture\application-scaling.excalidraw.png](software-architecture\application-scaling.excalidraw.png)