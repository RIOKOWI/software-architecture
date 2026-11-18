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