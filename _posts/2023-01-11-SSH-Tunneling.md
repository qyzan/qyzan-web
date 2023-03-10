---
title: SSH Tunneling
tags: [SSH, Security, Hack]
style: border
color: dark
description: SSH Tunneling atau bisa disebut dengan penerusan port SSH (Forwarding Port) secara sederhana tunneling berarti mengirimkan data melalui koneksi lain yang sudah terbentuk. Teknik ini sangat cocok dipakai sebagai backdoor, langsung menembus ke dalam ( behind enemy line) melewati semua firewall, IDS, IPS atau apapun itu di perbatasan.
---

Berikut adalah cara penggunaan SSH Tunneling;

## Penjelasan

#### A. SSH Tunneling.

SSH Tunneling atau bisa disebut dengan penerusan port SSH (Forwarding Port) secara sederhana tunneling berarti mengirimkan data melalui koneksi lain yang sudah terbentuk. Teknik ini sangat cocok dipakai sebagai backdoor, langsung menembus ke dalam ( behind enemy line) melewati semua firewall, IDS, IPS atau apapun itu di perbatasan.

#### B. Cara Kerja SSH Tunneling.

Forwarding Port memungkin kita mengakses jaringan lokal yang tidak terekspos ke internet. Misal kita ingin mengakses server database di Politeknik Negeri Batam dari rumah. Untuk alasan keamanan jaringan tertentu, server database tersebut hanya dapat diakses dan dikonfigurasi melalui koneksi dari jaringan local Politeknik Negeri Batam. Tetapi jika kita memiliki akses ke server SSH di kantor, dan server SSH itu memungkinkan koneksi dari luar jaringan local Politeknik Negeri Batam. Maka kita dapat terhubung ke server SSH itu dari rumah dan mengakses server database seolah-olah kita berada di Politeknik Negeri Batam. Ini lebih sering dilakukan di perusahaan besar, karena lebih mudah untuk mengamankan satu server SSH terhadap serangan daripada mengamankan berbagai sumber daya jaringan yang berbeda.

Untuk melakukan ini, kita perlu membuat koneksi SSH dengan server SSH dan memberitahu klien untuk meneruskan lalu lintas dari port tertentu dari Leptop (PC local) kita. Misal, port 1234 ke alamat server database dan port nya di jaringan Politeknik Negeri Batam. Jadi ketika kita mencoba mengakses server databases di port 1234 di Leptop kita saat ini, localhost, lalu lintas itu akan secara otomatis disalurkan melalui koneksi SSH dan dikirim ke server database. Server SSH duduk di tengah, meneruskan lalu lintas bolak balik. Untuk lebih jelasnya kita akan implementkan di server local buatan sendiri menggunakan VM.

#### C. Jenis SSH Port Forwarding.

1. Local SSH Port Forwarding ??? tipe ini mem-forward port yang diminta di local ke remote server (SSH Server). Misalnya kita ingin mengakses sebuah database atau aplikasi web yang berada di remote server (SSH Server) tapi terhalang firewall, sehingga tidak bisa di akses secara langsung, hanya komputer yang berada dalam satu private network yang bisa mengaksesnya.
2. Remote SSh Port Forwarding ??? tipe ini merupakan kebalikan dari Local SSH Port Forwarding. Misal database atau aplikasi web yang ada di komputer local ingin dapat diakses di internet. Kita dapat menggunakan tipe ini.
3. Dynamic SSH Port Forwarding - Tipe ini sama seperti proxy atau VPN. Misalnya kita ingin mengakses sebuah web secara aman, menyembunyikan Public IP Address, lokasi geografis atau mengakses website atau database yang hanya bisa diakses dari negara tertentu saja. SSH client akan membuat SOCKS proxy yang nantinya dikonfigurasi ke sistem operasi atau web browser.


## Implementasi

#### A.	Implementasi SSH Tunnel

Untuk melakukan implementasi kita memerlukan 2 VM yaitu server databases dan server web dengan menggukan OS Ubuntu. 

IP :
-	Database [192.168.56.113]
-	Web [192.168.56.111]
-	Host [192.168.56.1]
	

##### 1. Local SSH Port Forwarding

Kita akan melakukan SSH Tunneling ke server untuk format commandnya sendiri **???ssh -L [local_port]:[remote_address]:[remote_port] user@ip-server???** 
Kali ini saya akan ssh dari Host ke server Database
Kasusnya : di server database sudah setup phpmyadmin sehingga dapat diakses melalui web untuk koneksi ke database nya.

![image](https://user-images.githubusercontent.com/94363381/221343560-26bd3bf9-63a2-480f-8341-f7f3c96043b9.png)



Setelah menjalankan perintah tersebut, kita dapat mengakses database dengan port 3333. Ketika kita sedang mengakses sebenarnya kita sedang mengakses server database dengan port 3306 dan 80. Dikarenakan kita setup phpmyadmin di server database kita bisa liat koneksi nya melalui host dengan http://localhost[port_local], 


![image](https://user-images.githubusercontent.com/94363381/221343663-bb47473f-b428-40ef-ae12-ff773e86bfdd.png)

___

![image](https://user-images.githubusercontent.com/94363381/221343717-13497893-4422-4db3-bbbf-d81f16d169bc.png)


Sementara itu ketika kita mengakses aplikasi web di server web menggunakan ssh tunneling, kita dapat mengaksesnya dengan http://localhost[local_port], sama hal nya dengan server database, ketika kita sedangan mengakses localhost dengan local_port sebenarnya kita sedang mengakses http://[ip_server_web].

![image](https://user-images.githubusercontent.com/94363381/221343734-df5e0ae4-0e9b-4efc-a908-46aeb80a63c5.png)


##### 2. Remote SSH Port Forwarding

Kita akan melakukan SSH Tunneling ke server untuk format commandnya sendiri **???ssh -R [remote_port]:[local_address]:[local_port] user@ip-server???**

Kali ini saya akan SSH Tunneling melalui server ke Host

![image](https://user-images.githubusercontent.com/94363381/221343759-27853452-d01c-48a4-862b-f320a3de6b39.png)


Sekarang kita dapat mengakses http://[ip_server:[remote_port]], yang diakses sebenarnya adalah web server pada komputer local.

![image](https://user-images.githubusercontent.com/94363381/221343777-29718892-ef24-443f-8f2a-c590dfa6cba9.png)


Sementara itu ketika kita mengakses databases di server database, kita dapat mengaksesnya dengan http://[ip_server:[remote_port]/phpmyadmin], yang sebenarnya yang diakses adalah databases pada komputer local.
Karena di komputer saya menggunakan XAMPP maka kita dapat mengakses database hanya menggunakan phpmyadmin 


![image](https://user-images.githubusercontent.com/94363381/221343803-4d006c28-63ca-4ea7-b5a2-f6ade16056e6.png)


##### 3. Dynamic SSH Port Forwarding

Kita akan melakukan SSH Tunnelling ke server untuk format commandnya sendiri **??? ssh -D [ip_local]:[port_local] user@ip-server???.**

![image](https://user-images.githubusercontent.com/94363381/221343811-a64a5e9b-ee17-45ec-b785-216e6909426b.png)

Kemudian setup proxy di web browser.

![image](https://user-images.githubusercontent.com/94363381/221343821-07b3bfa0-6ad6-46cc-9461-2aaafe3eac99.png)

Dengan menggunakan perintah diatas kita akan membuat sebuah proxy dengan menggunakan SOCKS proxy, yang mana website yang kita akses melalui perantara server, sehingga yang terbaca mengunjungi website adalah IP server bukan IP dari host (local).
Biasanya akan terdapat log seperti dibawah ini jika kita sedang menggunakan SSH Tunneling Dynamis :

![image](https://user-images.githubusercontent.com/94363381/221343825-3099b97a-4854-4e87-b155-a035a3e1cd5d.png)

Kita scan port 8080 menggunakan nmap :

![image](https://user-images.githubusercontent.com/94363381/221343828-52344270-1104-4859-a7e0-7d0b07628c8d.png)




#### B.	Performa SSH Tunnel

Pada kasus ini saya akan melakukan percobaan untuk mengukur performa pada database saat menggunakan SSH, SSH Tunnel, dan secara local. 

Kita akan mencoba mengukur performa dari database mysql dari SSH tanpa Tunneling, command : **???mysqlslap --user=root --password --concurrency=5 --iterations=5 --create-schema=test --number-int-cols=5 --number-char-cols=5 --auto-generate-sql --verbose???**

Dalam perintah diatas kita menggunakan beberapa opsi, dengan penjelasan berikut.
-	**Concurrency** ??? Jumlah klien yang akan disimulasikan agar kueri dapat dijalankan.
-	**Iterations** ??? Banyaknya perulangan yang akan dilakukan untuk menjalankan tes.
-	**Create schema** ??? scema untuk menjalankan tes (nama database yang akan dipakai atau dibuat).
-	**Number int cols** ??? banyaknya kolom INT yang dibuat pada table.
-	**Number char cols** ??? banyaknya kolom VARCHAR yang dibuat pada table.
-	**Auto generate sql** ??? otomatis membuat SQL ketika tidak disediakan dati file atau baris perintah.

##### 1. SSH

![image](https://user-images.githubusercontent.com/94363381/211846261-39f64ffc-c3aa-4a65-b9ef-cbb7e21acc1a.png)

##### 2. SSH Tunneling

![image](https://user-images.githubusercontent.com/94363381/211846323-adae772a-f240-410c-bd4c-cced0e267081.png)

##### 3. Local

![image](https://user-images.githubusercontent.com/94363381/211846400-31853b5a-0eef-4b3c-9b57-0f31ed98fc72.png)

Dilihat dari beberapa percobaan diatas dapat disimpulkan bahwa ketika tidak diterapkan keamanan (enkripsi packet) maka waktu yang dipakai untuk menjalankan perintah lebih cepat dibandingkan ketika menggunakan keamanan enkripsi paket yang mana SSH sendiri, merupakan sebuah protokol jaringan yang berjalan pada protokol TCP/IP, dengan memfasilitasi teknik enkripsi saat pertukaran paket sedang berlangsung. 




## Penutup

#### A.	Kesimpulan.

SSH Tunneling memungkinkan kita mengakses server dari jarak jauh tanpa terhalang oleh firewall. SSH Tunneling sendiri memiliki tiga tipe yaitu Local SSH Tunneling, Remote SSH Tunneling, Dynamic SSH Tunneling yang mana masing masing memiliki kegunaan masing masing.

Untuk mengukur performa dari database mysql dan melakukan beberapa percobaan yang telah dilakukan, disimpulkan bahwa kecepatan database untuk mengeksekusi kueri yang dijalankan, tergantung dengan jenis komunikasi yang diterapkan, dimana ketika kita menggunakan keamanan jaringan yang terenkripsi atau bisa disebut sebagai jaringan yang aman akan membutuhkan waktu yang lebih lama dibandingkan ketika kueri dijalankan tanpa komunikasi atau jaringan tanpa enkripsi.
