# Pendahuluan

# Dasar Teori

## A. VirtualBox

VirtualBox merupakan perangkat lunak virtualisasi sistem operasi. Perangkat lunak ini dapat mengeksekusi sistem operasi di suatu sistem operasi utama. Dengan VirtualBox, kita dapat menajalankan sistem operasi linux meskipun komputer yang digunakan menggunakan sistem operasi windows. VirtualBox berguna untuk melakukan simulasi tanpa perlu kehilangan sistem yang ada.

Pada pengamatan ini, kami menggunakan VirtualBox untuk melakukan virtualisasi sistem operasi berikut.
- Ubuntu Dekstop 16.04, sebagai SSH server
- Kali Linux 2017.2, sebagai client yang akan melakukan penetrasi

Dengan VirtualBox, Ubuntu Server 17.10 dan Kali Linux 2017.2 dapat berjalan bersamaan pada komputer yang sama.

## B. Kali Linux 2017.2

Kali Linux merupakan sistem operasi turunan Debian berbasis Linux. Kali Linux memiliki lebih dari 600 penetration testing program, diantaranya Hydra, Ncrack, dan Medusa. Kali Linux berperan sebagai client yang akan melakukan penetrasi ke Ubuntu Server.

## C. Ubuntu 16.04

Ubuntu 16.04 merupakan salah satu sistem operasi distribusi Linux yang berbasiskan Debian. Ubuntu bersifat Open source dan dirancang untuk kepentingan penggunaan pribadi.

## D. Wordpress

Wordpress adalah aplikasi open source yang biasa digunakan dalam pembuatan blog dan CMS (Content Management System) karena kemampuannya untuk dimodifikasi dan disesuaikan dengan kebutuhan penggunanya. WordPress dibangun dengan bahasa pemrograman PHP dan basis data (database) MySQL. PHP dan MySQL.

## E. Video Player 1.5.16

Spider Video Player adalah salah satu plugin video Wordpress yang memungkinkan kita unutuk menambahkan video ke situs web dengan mudah. Selain itu, Spider Video Player memiliki banyak fitur sehingga memudahkan kita dalam penggunaan plugin ini, seperti mengatur playlist, memilih tata letak video dan masih banyak lagi.

## F. LeagueManager 3.9.1.1

League Manager adalah plugin Wordpress yang didesain untuk memudahkan kita dalam mengelola konten olah raga pada blog atau situ kita. League Manager memiliki fitur unutuk menampilkan tim dan pertandingan, statistik pertandingan, peraturan pada pertandingan dan masih banyak lagi.

# Menyiapkan Environtment

## A. Environtment Ubuntu 

Setelah kita menginstall VirtualBox, langkah selanjutnya adalah menyiapkan environment untuk Ubuntu Server
1. Buka Virtualbox, kemudian pilih **New**
2. Masukkan nama OS, Pilih type **Linux**, Pilih version **Ubuntu (64-bit)**
3. Set memory size minimal **1024 MB**
4. Pilih opsi **Create a virtual hardisk now**
5. Pilih opsi **VHD (Virtual Hard Disk)**
6. Pilih opsi **Dynamically allocated**
7. Set alokasi disk minimal **20 GB** sesuai rekomendasi
8. Pilih **create**

## C. Setting NAT pada Virtualbox

Agar kedua device dapat saling terhubung dan dapat terkoneksi dengan internet maka kita harus men-setup NAT pada kedua device tersebut. Setelah kita men-setup NAT, maka kita tidak perlu lagi mengkonfigurasikan ip address pada kedua device tersebut. Berikut ini adalah langkah - langkahnya.

**Membuat NAT**
1. Buka Virtualbox, Klik **File**, kemudian pilih **Preferences**
2. Pilih **Network**, kemudian tambahkan NAT network dengan mengklik **icon** yang bertanda **Plus** pada sisi kanan
3. Setelah ditambahkan, kemudian edit NAT tersebut. Pada bagian Network name anda bisa merubah nama NAT sesuai keinginan.
4. Pada bagian **Network CIDR** isikan sesuai koneksi jaringan dimana anda terhubung. Sebagai contoh **10.151.32.0/24**
5. Centang pada bagian **Supports DHCP** agar pembagian ip bersifat dinamis
6. Klik **OK**

**Men-setup NAT pada Device**
1. Klik kanan pada virtual mesin
2. Pilih bagian **Network**
3. Pilih **Adapter 1**, kemudian centang pada bagian **Enable Network Adapter**.
4. Pada bagian Attached to pilih **NAT Network**. Kemudian pilih NAT yang sudah kita buat tadi.
5. Klik **OK**

**Catatan**
> - Lakukan langkah untuk men-setup NAT pada kedua device

## D. Download Plugin Video Player 1.5.16
1. Buka link *https://wordpress.org/plugins/player/advanced/*
2. Pada bagian Previous Version, pilih versi **1.5.16**
3. Klik tombol **Download**

## E. Download LeagueManager 3.9.1.1
1. Buka link *https://wordpress.org/plugins/leaguemanager/advanced/*
2. Pada bagian Previous Version, pilih versi **3.9.1.1**
3. Klik tombol **Download**

# Uji Coba

## Instalasi Ubuntu

Langkah selanjutnya adalah mengintalasi kali linux pada virtual mesin
1. Klik kanan pada virtual mesin yang sudah kita buat sebelumnya
2. Pilih **Start**, kemudian pilih **Normal Start**
3. Pada menu Select start-up disk pilih disk Kali Linux yang sudah kita download tadi, kemudain Pilih **Start**
4. Lakukan Instalasi Kali Linux pada umumnya, kita bisa memilih opsi **Graphical install** untuk lebih memudahkan dalam proses penginstallan

## Setting NAT pada Virtualbox

Agar kedua device dapat saling terhubung dan dapat terkoneksi dengan internet maka kita harus men-setup NAT pada kedua device tersebut. Setelah kita men-setup NAT, maka kita tidak perlu lagi mengkonfigurasikan ip address pada kedua device tersebut. Berikut ini adalah langkah - langkahnya.

**Membuat NAT**
1. Buka Virtualbox, Klik **File**, kemudian pilih **Preferences**
2. Pilih **Network**, kemudian tambahkan NAT network dengan mengklik **icon** yang bertanda **Plus** pada sisi kanan
3. Setelah ditambahkan, kemudian edit NAT tersebut. Pada bagian Network name anda bisa merubah nama NAT sesuai keinginan.
4. Pada bagian **Network CIDR** isikan sesuai koneksi jaringan dimana anda terhubung. Sebagai contoh **10.151.32.0/24**
5. Centang pada bagian **Supports DHCP** agar pembagian ip bersifat dinamis
6. Klik **OK**

**Men-setup NAT pada Device**
1. Klik kanan pada virtual mesin
2. Pilih bagian **Network**
3. Pilih **Adapter 1**, kemudian centang pada bagian **Enable Network Adapter**.
4. Pada bagian Attached to pilih **NAT Network**. Kemudian pilih NAT yang sudah kita buat tadi.
5. Klik **OK**

**Catatan**
> - Lakukan langkah untuk men-setup NAT pada kedua device

## Instalasi Wordpress

Referensi
...[How To Install Wordpress on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-install-wordpress-on-ubuntu-14-04)
...[How To Install Linux, Apache, MySQL, PHP (LAMP) stack on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-16-04)
...[How to Install WordPress 4.7 On Ubuntu 16.10/16.04 Using LAMP Stack](https://www.tecmint.com/install-wordpress-on-ubuntu-16-04-with-lamp/)

1. Lakukan update terlebih dahulu `sudo apt-get update`
2. Install Apache `sudo apt-get install apache2`
3. Set Global ServerName agar server berjalan sesuai dengan alamat IP yang kita definisikan. Buka `/etc/apache2/apache2.conf` lalu tambahkan ServerName localhost agar server berjalan di local
4. Cek konfigurasi apakah sudah benar atau belum `sudo apache2ctl configtest`
5. Restart Apache `sudo systemctl restart apache2`
6. Set Firewall agar server dapat diakses menggukanakan metode HTTP dan HTTPS. Jalankan `sudo ufw app list` . Enable traffic  Apache Full denga perintah `sudo ufw allow in "Apache Full"`
7. Kemudian cek pada browser jika  server usdah bisa diakses
8. Install Mysql menggukanan perintah `sudo apt-get install mysql-server`
9. Masukkan Password dan Confirm password
10. Install PHP menggukanan perintah `sudo apt-get install php7.0 php7.0-mysql libapache2-mod-php7.0 php7.0-cli php7.0-cgi php7.0-gd`
11. Buatlah sebuah file `sudo nano /var/www/html/info.php`
12. Tambahkan kode berikut 
```php
<?php 
	phpinfo();
?>
``` 
13. Ubah urutan pada /etc/apache2/mods-enabled/dir.conf agar apache membaca file index.php terlebih dahulu
```
<IfModule mod_dir.c>
	DirectoryIndex **index.php** index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```
14. Restart Apache `sudo systemctl restart apache2`
15. Pada browser cek halaman info.php
16. Buat database baru pada Mysql, jalankan perintah berikut satu persatu:
```
mysql -u root -p
CREATE DATABASE wordpress;
CREATE USER wordpressuser@localhost IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON wordpress.* TO wordpressuser@localhost;
FLUSH PRIVILEGES;
exit
```
17. Download Wordpress `wget -c http://wordpress.org/latest.tar.gz`
18. Ekstrak `tar -xzvf latest.tar.gz`
19. Pindahkan folder ke /var/www/html `sudo rsync -av wordpress/* /var/www/html/`
20. Set permission
```
sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 755 /var/www/html/
```
21. Pindah ke direktori html `cd /var/www/html`
22. Amankan Wordpress `curl -s https://api.wordpress.org/secret-key/1.1/salt/`
23. Pada browser buka halaman `localhost/wp-admin/setup-config.php`
24. Masukkan Database name, Database username, Database password dan Database host sesuai dengan yang dibuat sebelumnya

## Instalasi Video Player 1.5.16

## Instalasi LeagueManager 3.9.1.1

## Instalasi WPscan

## Instalasi sqlmap

# Uji Coba Penetrasi

# Kesimpulan dan Saran

Perancangan Keamanan Sistem Jaringan
