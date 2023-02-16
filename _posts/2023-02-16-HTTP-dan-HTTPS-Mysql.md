---
title: HTTP dan HTTPS Mysql
tags: [HTTP, Security, Mysql,Database]
style: border
color: warning
description: Hypertext Transfer Protocol (HTTP) adalah sebuah protokol jaringan lapisan aplikasi yang digunakan untuk sistem infromasi terdistribusi, kolaboratif dan menggunakan hipermedia.
---

### Materi
#### A. HTTP

Hypertext Transfer Protocol (HTTP) adalah sebuah protokol jaringan lapisan aplikasi yang digunakan untuk sistem infromasi terdistribusi, kolaboratif dan menggunakan hipermedia.

Penggunanya banyak pada pengembalian sumber daya yang saling terhubung dengan tautan, yang disebut dengan dokumen hiperteks. Hingga kini, ada dua versi mayor dari protokol HTTP yakni HTTP/1.0 yang menggunakan koneksi terpisah untuk setiap dokumen, dan HTTP/1.1 yang dapat menggunakan koneski yang sama untuk melakukan transaksi. Dengan demikian HTTP/1.1 bisa lebih cepat karena memenag tidak membuang waktu untuk melakukan koneksi berulang ulang.

HTTP tidak lah terbatas untuk penggunaan dengan TCP/IP, meskipun HTTP merupakan salah satu protokol aplikasi TCP/IP paling populer melalui internet. Memang HTTP dapat diimplementasikan diatas protokol lainnya di atas internet, tapi HTTP membutuhkan sebuah protokol lapisan transport yang dapat diandalkan. Protokol lainnya yang menyediakan layanan dan jaminan seperti itu juga dapat digunakan.

#### B. HTTPS

Hypertext Transfer Protocol Secure (HTTPS) adalah kombinasi dari HTTP dan Protokol Kriptografi. Sambungan HTTPS sering digunakan untuk pembayaran transaksi (uang, data penting) di internet. HTTPS dapat diartikan sebagai bentuk protokol internet yang paling valid dan paling aman. 

HTTPS ini akan melindungi Integritas serta kerahasian antara client dan server. Sehingga penggunaan HTTPS ini sangat sulit untuk dibajak oleh orang lain saat proses transaksi berlangsung. Protokol keamanan yang biasanya digunakan dalam HTTPS adalah SSL dan TLS

#### C. Perbandingan HTTP dan HTTPS
1. Port

Hal pertama yang menjadi pembeda adalah dibagian port untuk mengirim data dan penerimaan data.

HTTP menggunakan port 80, dimana kondisi data masih menggunakan teks biasa (plainteks) sehingga dapat dibaca dengan jelas data apa yang sedang terjadi di jaringan.

HTTPS menggunakan port 443 dimana port ini lebih aman karena setiap komunikasi atau transfer data terenkripsi, sehingga tidak mudah dibaca pihak lain.

2. SSL

SSL adalah metode akses website menggunakan sambungan protokol terenkripsi. HTTP adalah protokol yang belum memanfaatkan sertifikat keamanan SSL. Sedangkan HTTPS sudah menggunakan SSL/TLS untuk enkripsi data.

Dengan SSL aktif apada HTTPS maka terjadi transaksi data maka semua data yang dikirim dan diterima akan terenkripsi.

3. Keamanan Data

HTTPS menggunakan 3 prosedur keamanan data yaitu CIA (Confidentiality, Intergrity, Authentication) yang mana hal ini tidak terpadat pada protokol HTTP.





### Implementasi


#### A. Koneksi HTTP ke Database
set up system:

1. Web Server (Ubuntu) yang sudah terinstal Mysql Client

![image](https://user-images.githubusercontent.com/94363381/219381545-c82724a3-92de-4195-9940-7149edcd1e4c.png)



2.	XAMPP sebagai database

Perlu Configurasi di bagian Mysql yaitu perlu di enable di bagian bind address dan ubah menjadi **bind-address=”0.0.0.0”** agar semua ip dapat mengakses mysql server nya. Atau dibuat secara spesifik juga bisa tinggak ubah ip **0.0.0.0** menjadi ip dari web server atau server yang menginstal **mysql-client**

![image](https://user-images.githubusercontent.com/94363381/219382222-69020aa3-0f66-4ac1-84ec-6ec5d3ce88e2.png)

Lokasi file secara default **“C:\xampp\mysql\bin\my.ini”**

Kemudian Perlu juga untuk mebuat user baru dengan hostnya IP dari Web Server (Ubuntu Server) dan berikan Privilage

![image](https://user-images.githubusercontent.com/94363381/219382339-241b7ef6-6040-4a9a-8527-ec2013849988.png)



2.	Wireshark sebagai analisis paket yang sedang terjadi antara Client dengan Server.
- Buka Wireshark kemudian pilih trafic yang mau di analisis 

Disini saya akan milih trafic Ethernet karena koneksi antara Database dan Webserver (Ubuntu Server) menggunakan jaringan Host-only


![image](https://user-images.githubusercontent.com/94363381/219382598-7c22dea7-5778-495a-9298-79a2ff3425b6.png)


- Buka Web Server, kemudian masuk ke mysql server dengan menggunakan command **“ mysql -u {nama user} -p -h {ip dari database}**

![image](https://user-images.githubusercontent.com/94363381/219382793-00eb34e7-5ba3-4649-b493-50607e538b72.png)
---
![image](https://user-images.githubusercontent.com/94363381/219383051-9d7c28d8-06c2-451e-9400-212aa65d72d8.png)


Dilihat dari gambar diatas kita tidak menggunakan protokol SSL/TLS sehinggak koneksi tidak terenkripsi atau menggunakan protokol HTTP

-	Setelah itu lakukan beberapa aktifitas di mysql server, seperti show tables, database dan lain lain. Sebagai contoh :

Saya menjalankan perintah seperti dibawah;

![image](https://user-images.githubusercontent.com/94363381/219383365-b0150ff2-a3a4-4554-915f-8f7048e8e5b0.png)

Dan liat apakah paket berupa plainteks atau sudah terenkripsi koneksi dari client dan server

![image](https://user-images.githubusercontent.com/94363381/219383420-2ece025b-ec2c-45c9-ac16-0f0aaf0d5f5d.png)

Analisis;

Dapat dilihat dari gambar diatas bahwa perintah yang kita jalankan di mysql server dapat terdeteksi oleh wireshark dan berupa plainteks tanpa ada nya enkripsi.

![image](https://user-images.githubusercontent.com/94363381/219383516-fd388042-2354-4314-aeb5-8e4b30669996.png)

Analisis ;

Respond dari server pun berupa plainteks sehingga semua dapat terbaca dengan jelas sehingga kita dapat melihat value dari perintah yang dijalankan diatas.


-	Pengukuran Performasi
 
Pengukuran performansi Mysql tanpa SSL 

![image](https://user-images.githubusercontent.com/94363381/219383789-f4b4f927-e33f-4676-8640-639ed0ac7285.png)





#### B.	Koneksi HTTS ke Database
Setup System :

1. Web Server (Ubuntu) yang sudah terinstal Mysql Client
![image](https://user-images.githubusercontent.com/94363381/219384239-64863413-93b0-4a88-aed7-0ae60a02116f.png)



2.	Database Server (Ubuntu) 
Perlu Configurasi di bagian Mysql yaitu perlu di enable di bagian bind address dan ubah menjadi bind-address=”0.0.0.0” agar semua ip dapat mengakses mysql server nya. 

Atau dibuat secara spesifik juga bisa tinggak ubah ip 0.0.0.0 menjadi ip dari web server atau server yang menginstal mysql-client

![image](https://user-images.githubusercontent.com/94363381/219384764-13b85219-39f4-4af3-bdda-1ce4e4338f4b.png)

Lokasi file secara default **“/etc/mysql/mysql.conf.d/mysqld.cnf”**

Kemudian Perlu juga untuk mebuat user baru dengan hostnya, IP dari Web Server (Ubuntu Server) dan berikan Privilage

![image](https://user-images.githubusercontent.com/94363381/219384895-2b8c20a6-9def-4cdb-8c13-33c86159ece2.png)



3. Install SSL Database Mysql

Secara default, ketika kita menginstall mysql-server di Database server akan secara otomatis terinstall sertifikat SSL karena itu sudah menjadi standart pada MySql Database. 

Untuk memeriksanya kita dapat menggunakan perintah **“sudo find /var/lib/mysql -name ‘*.pem’ -ls” **

![image](https://user-images.githubusercontent.com/94363381/219385181-26d81d07-cf65-4df7-9635-5a6d570e330e.png)

Dapat dilihat dari gambar diatas bahwa ssl sudah ikut terinstall saat menginstall **mysql-server.** Dan untuk memastikan lagi apakah SSL ini sudah terpasang di mysql-server. Kita dapat menjalankan perintah **‘show variables like ‘%ssl%’; **

![image](https://user-images.githubusercontent.com/94363381/219385376-0cf09672-1bde-4345-84a3-3133aba4f292.png)
---
![image](https://user-images.githubusercontent.com/94363381/219385433-9d93b070-81fd-4200-8faf-3ad277c5c087.png)

Dapat dilihat dari kedua gambar diatas bahwa didalam mysql nya sudah ada SSL dengan menggunakan **TLS_AES_256_GCM_SHA384.**



4. Wireshark sebagai analisis paket yang sedang terjadi antara Client dengan Server.
-	Buka Webserver dan jalankan perintah **sudo tcpdump -i {interface} -s 65539 -w {nama file}**

![image](https://user-images.githubusercontent.com/94363381/219385732-4e8c130b-2745-4766-ab1e-67e84249257f.png)

Gunakan extensi .pcap agar dapat di buka di wireshark

Perintah diatas digunakan untuk menangkap/record semua paket yang sedang terjadi pada interface tertentu.

- Kemudian masuk ke mysql server dengan menggunakan command “ mysql -u {nama user} -p -h {ip dari database}”

![image](https://user-images.githubusercontent.com/94363381/219385904-730e2494-2280-4e40-867a-754394b9dd9c.png)

- Setelah itu lakukan beberapa aktifitas di mysql server, seperti show tables, database dan lain lain. Sebagai contoh :
Saya menjalankan perintah seperti dibawah;

![image](https://user-images.githubusercontent.com/94363381/219386007-a438af16-03c4-46d0-bbf1-40e40e969a67.png)

Dan liat apakah paket berupa plainteks atau sudah terenkripsi koneksi dari client dan server

![image](https://user-images.githubusercontent.com/94363381/219386081-8297ff1d-f014-42fc-bbbf-0abcd0e745a6.png)

Analisis;

Dapat dilihat dari atas bahwa Protokol dari Mysql sudah berubah yaitu TLSv1.3 dan paket/data juga sudah terenkripsi sehinggak tidak dapat dibaca dengan mudah.



5. Pengukuran Performansi

Pengukuran performansi Mysql menggunakan SSL 

![image](https://user-images.githubusercontent.com/94363381/219386865-172fcce7-0cc7-4ac7-9fab-c4b74eb9bbf0.png)





#### C.	Perbandingan Performansi antara menggunakan SSL dengan tidak menggunakan SSL
Untuk pengukurannya kita mneggunakan perintah mysqlslap yang mana ini merupakan tools yang digunakan untuk menirukan beban klien pada server Mysql dan melaporkan waktu setiap tahapan yang dilewati.

Dilihat Performansi dari HTTP tanpa SSL:

    Benchmark
        Average number of seconds to run all queries: 7.047 seconds
        Minimum number of seconds to run all queries: 5.703 seconds
        Maximum number of seconds to run all queries: 8.587 seconds
        Number of clients running queries: 20
      Average number of queries per client: 0

Dilihat Performansi HTTP dengan menggunakan SSL:

    Benchmark
          Average number of seconds to run all queries: 6.904 seconds
          Minimum number of seconds to run all queries: 5.712 seconds
          Maximum number of seconds to run all queries: 9.238 seconds
          Number of clients running queries: 20
      Average number of queries per client: 0 

Analisis :

Untuk HTTPS cendrung membuthkan waktu lebih lama ketimbang HTTP karena HTTPS membutuhkan waktu untuk proses enkripsi data terlebih dahulu baru kemudian dikirim ke klien, sehingga membutuhkan waktu cukup lama.
