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


