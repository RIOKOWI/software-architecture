# SYSTEM DESIGN PART 2
[PART 1](README.md)


# STOLEN JWT
- gimana kalo ada orang yang mencuri jwt saya dan login sebagai saya menggunakan jwt saya?
- warning: hati hati ketika terkoneksi dengan wifi public ketika login aplikasi, karena traffic network kita bisa terakses oleh provider penyedia wifi tersebut dan jwt kita di ambil oleh orang
- untuk manangani jwt tercuri nya Oauth pakai access token & refresh token
- access token adalah credential yang orang lain tidak boleh tau kecuali pemilik sendiri(ada kadaluarsanya +- 1 jam an)
- refresh token adalah token untuk generate access token yang sudah kadaluarsa
- refresh token di simpan di database, jika refresh token tercuri kita tinggal hapus saja di database dengan cara logout device akun(use case facebook, ig, dll), sehingga pencuri tidak bisa generate access token lagi

- contoh gambar :
[system-design\STOLEN-JWT.excalidraw.png](system-design\STOLEN-JWT.excalidraw.png)

## SOURCE 
[https://www.youtube.com/watch?v=nfgxLMB4N2k&list=PL-CtdCApEFH8Kj1lGRLPJbAsVYIDwe1e_](https://www.youtube.com/watch?v=nfgxLMB4N2k&list=PL-CtdCApEFH8Kj1lGRLPJbAsVYIDwe1e_)

---

# SINGLE SIGN ON OAUTH
- cara centralized auth beberapa aplikasi rekomen menggunakan Oauth
- oauth spec : [https://datatracker.ietf.org/doc/html/rfc6749](https://datatracker.ietf.org/doc/html/rfc6749)
- 4 roles : resource owner(orang pemilik data), resource server(server yg menyimpan data), client(aplikasi perantara yang mengakses pemilik data), authorization server(kunci/jwt untuk akses data, authentication & authorization)
- single sign on adalah 1 akun resource owner bisa memakai ke beberapa app
- rekomendasi app untuk sso : keycloak, CAS, dll

- contoh gambar :
[system-design\SSO-OAUTH.excalidraw.png](system-design\SSO-OAUTH.excalidraw.png)

## SOURCE 
[https://www.youtube.com/watch?v=koZXdinR8jg&list=PL-CtdCApEFH8Kj1lGRLPJbAsVYIDwe1e_&index=4](https://www.youtube.com/watch?v=koZXdinR8jg&list=PL-CtdCApEFH8Kj1lGRLPJbAsVYIDwe1e_&index=4)

---

# HUBUNGAN JWT DENGAN OAUTH
- tidak ada hubunngannya jwt dengan oauth
- oauth itu aturan main untuk authentication & authorization yang mengharuskan implemen access token + refresh token
- jwt untuk pertukaran data antara 2 pihak apakah token ini benar atau tidak, sehingga 1 app tidak perlu verifikasi token/random string lagi ke authorization server
- keduanya di implemen atau di hubungkan antara jwt dan oauth untuk mengurangi request/validasi antara authorization server dan resource server
- informasi di jwt bisa di decode dengan mudah tetapi signature nya tidak bisa di duplikat atau dibuat manual hanya body nya saja yang rawan, makanya jangan selipkan payload sensitif di body jwt

- contoh gambar :
[system-design\JWT-OAUTH.excalidraw.png](system-design\JWT-OAUTH.excalidraw.png)

## SOURCE 
[https://www.youtube.com/watch?v=0rwwXecwuj8&list=PL-CtdCApEFH8Kj1lGRLPJbAsVYIDwe1e_&index=2](https://www.youtube.com/watch?v=0rwwXecwuj8&list=PL-CtdCApEFH8Kj1lGRLPJbAsVYIDwe1e_&index=2)

---


# BIAR APLIKASI GAK DOWN DI KUBERNETES
- pakai fitur horizontal pod auto scaler, intinya aplikasi kita boleh jalan/bertambah secara horizontal dengan konfigurasi cpu/ram yang di tentukan
- jika cpu/ram menyentuh 80% kita bisa trigger fitur horizontal pod auto scaler
- kita juga harus siapin hardware yang ingin kita scale max, misal kalo max ram set 10 ya kita harus siapin hardware ram 10
- kalo kubernetes crash otomatis akan restart selama 1-2 menit makanya minimal aplikasi harus di nyalain 2 jika salah 1 mati tidak akan crash
- sebelum implemen auto scale, lakukan performance testing untuk menguji kesanggupan aplikasi menerima seberapa banyak request/second aplikasi kita
- jadi perkiraan request dibagi dengan performa request aplikasi kita per second = jumlah pod max yang kita set
- startup time = waktu yang di perlukan aplikasi untuk menerima request pertama kali
- semakin lambat aplikasi kita startup time semakin bahaya
- solusinya set cpu sama ram nya di kecilin sehingga saat sudah menyentuh kapaitas pod baru prepare buat startup time untuk menerima request baru, karna butuh 1 menitan untuk bikin pod baru
- jika request melebihi ekspetasi kita ya korbankan saja dengan implemen rate limiter agar app kita gak mati di banjiri kelebihan request di luar ekspetasi kita
- jangan serakah menerima request

- contoh gambar :
[system-design\SCALE-KUBERNETES.excalidraw.png](system-design\SCALE-KUBERNETES.excalidraw.png)

## SOURCE 
[https://www.youtube.com/watch?v=Tt2bHtIChoo&list=PL-CtdCApEFH8Kj1lGRLPJbAsVYIDwe1e_&index=3](https://www.youtube.com/watch?v=Tt2bHtIChoo&list=PL-CtdCApEFH8Kj1lGRLPJbAsVYIDwe1e_&index=3)

---

# BEFORE MIGRATION INTO MICROSERVICES
- saat develop awal di sarankan monolith
- optimasi monolith
- lalu ke CQRS
- lalu ke microservices
- microservices di implement karna non teknis, project terlalu besar, tim sudah banyak, timeline project berbeda beda, ownership project, dll.


- contoh gambar :
[system-design\BEFORE-MICROSERVICES.excalidraw.png](system-design\BEFORE-MICROSERVICES.excalidraw.png)

## SOURCE 
[https://www.youtube.com/watch?v=Tt2bHtIChoo&list=PL-CtdCApEFH8Kj1lGRLPJbAsVYIDwe1e_&index=3](https://www.youtube.com/watch?v=Tt2bHtIChoo&list=PL-CtdCApEFH8Kj1lGRLPJbAsVYIDwe1e_&index=3)

---

# BEFORE USING KAFKA
## Scalability
- orang orang menggunakan kafka karna scalability, gimana kalo traffic nya naik data nya tambah besar dan terus bertambah
- setup awal minimal 3 server, emang agak mahal investasi awal
- dan jika ingin menambha server tambahnya ganjil jangan genap
- dan jika ada server baru kafka otomatis menambah server tersebut ke cluster yang sudah ada tanpa update configurasi lagi

## Fast
- kafka di buat oleh linkedin
- kafka sanggup memproses 2.000.000 write per second / 3 machine/server
- contoh kasus data 2.000.000 untuk data analitik user atau membaca behaviour user

## Message Size
- message kafka rekomendasi ukuran kecil, defautnya 1mb lebih dari 1mb akan di tolak
- untuk upload file ga cocok
- kafka di design untuk kirim message kecil kecil, contoh mengirim data : json, text, xml baru cocok
- kalo mau di akalin kirim file gede pake message broker lain atau file nya kirim ke cloud lalu kafka hanya mengirim lokasi/path/url file tersebut ke app lain
- app consumer bisa consume message lebih dari 1 message bahkan sampai 50 message

## Partition
- fungsi partisi agar tidak bottleneck, jadi jika ada file 30 gb dan 3 partisi, 1 file tersebut akan di pecah masing masing 10 gb ke 3 partisi
- 1 partisi hanya bisa di consume oleh 1 node/server app saja
- kalo kurang kecepata consume app nya, mau ga mau harus nambah partisi jika ke 3 partisi sudah di consume oleh 3 app
- semakin banyak partisi, penggunaan memori akan semakin besar dan antar partisi harus saling koordinasi/sinkronisasi

## Sequential Guarantee
- jika ada topik yang di store, app consumer akan consume secara sequential/berurut(FIFO)
- kalo stuck di tengah tengah ya error
- kalo di rabbitmq tidak ada sequential guarantee
- pastiin data nya di satu partisi yang sama, kalo bercecaran di partisi yang berbeda kafka tidak akan menjamin sequential guarantee

## Persistance
- kafka mengirim semua data dalam bentuk file berupa log file/.log otomatis tidak akan hilang
- consumer baca data bukan narik data sehingga datanya masih tetap ada


- contoh gambar :
[system-design\B4-USE-KAFKA.excalidraw.png](system-design\B4-USE-KAFKA.excalidraw.png)

## SOURCE 
[https://www.youtube.com/watch?v=ms6ltwRC1sE&list=PL-CtdCApEFH8Kj1lGRLPJbAsVYIDwe1e_&index=7](https://www.youtube.com/watch?v=ms6ltwRC1sE&list=PL-CtdCApEFH8Kj1lGRLPJbAsVYIDwe1e_&index=7)

---
