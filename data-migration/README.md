# 4 SKENARIO MIGRASI DATA

Catatan: Di zaman sekarang, yang sering sulit bukan membuat ulang aplikasinya, tetapi memindahkan data lama dari aplikasi lama ke aplikasi baru.


## SKENARIO 1
strategi paling mudah
- Buat 2 aplikasi:
  - Aplikasi lama tetap berjalan.
  - Aplikasi baru dibuat dari awal dan gunakan aplikasi baru.
- Data lama tidak perlu langsung dipindahkan semuanya ke aplikasi baru.
- Kalau ingin melihat data lama, tetap gunakan aplikasi lama.
- Yang dipindahkan dari database lama ke database baru cukup master data yang memang dibutuhkan.

contoh di file :
- [data-migration\data-migration.excalidraw.png](data-migration\data-migration.excalidraw.png)


## SKENARIO 2
strategi sharing database:
- database lama di refactor untuk app baru, entah tambah kolom aja atau tambah tabel juga
- jadi 1 database bisa di pakai 2 app, baru & lama

contoh di file :
- [data-migration\data-migration.excalidraw.png](data-migration\data-migration.excalidraw.png)

## SKENARIO 3

- Database lama menggunakan **MySQL**.
- Database baru menggunakan **PostgreSQL**.
- Aplikasi baru mengambil data lama melalui **API dari aplikasi lama**.

Alurnya:

1. User meminta data lama di **aplikasi baru**.
2. Aplikasi baru meminta data lama tersebut ke **API aplikasi lama**.
3. Aplikasi lama melakukan query ke **database lama (MySQL)**.
4. Data lama dari database lama dikirim kembali ke aplikasi baru melalui API.
5. Aplikasi baru menyimpan data tersebut ke **database baru (PostgreSQL)**.
6. Setelah itu, data ditampilkan ke user.

Jadi sederhananya:

**User → App Baru → API App Lama → DB Lama → App Lama → App Baru → DB Baru → User**

Dengan cara ini, data lama **dipindahkan sedikit demi sedikit saat data tersebut dibutuhkan**.

*note : skenario ini dilakukan jika ada data yang di hashing

contoh di file :
- [data-migration\data-migration.excalidraw.png](data-migration\data-migration.excalidraw.png)



## SOURCE
- [https://www.youtube.com/watch?v=dj9etrhib64](https://www.youtube.com/watch?v=dj9etrhib64)