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
