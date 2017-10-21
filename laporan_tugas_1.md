# Pendahuluan

Kebutuhan informasi telah mendorong lahirnya internet sebagai teknologi pertukaran informasi tanpa barir jarak dan waktu.
Dengan adanya internet, informasi dapat berpindah dari suatu tempat ke tempat lain dalam hitungan mili detik. Tanpa biaya pengiriman dan hampir tanpa effort. Akibat kemudahan yang ditawarkan, internet telah menjadi teknologi yang diandalkan bahkan untuk proses penting seperti jual beli online, perbankan, pengiriman pesan pribadi, dan lain sebagainya. Namun, pada masa pengembangannya, internet tidak dirancang untuk menjaga kebenaran dan keaslian informasi sehingga informasi yang melalui internet mudah terekspos kepada pihak yang tidak berhak.

Salah satu mekanisme yang dapat diandalkan untuk menjamin keaslian informasi adalah authentication. Authentication berfungsi sebagai filter yang menentukan apakah seorang pengguna berhak atas suatu informasi atau tidak.

Pada tugas kali ini, kami akan mengamati celah kemananan mekanisme authentication dan bagaimana menanggulangi celah keamanan tersebut. Kami melakukan pengamatan ini dengan cara melakukan uji penetrasi brute force ke suatu SSH Server. Kami akan mencoba memecahkan password untuk login ke SSH Server dengan menggunakan tools Hydra dan Ncrack yang terdapat pada Kali Linux OS. Dengan pengamatan ini, kami berharap dapat meningkatkan awareness pada celah keamanan authentication ini sehingga eksploitasi yang memanfaatkan celah keamanan ini dapat diminimalisir.

# Dasar Teori

## VirtualBox

VirtualBox merupakan perangkat lunak virtualisasi sistem operasi. Perangkat lunak ini dapat mengeksekusi sistem operasi di suatu sistem operasi utama. Dengan VirtualBox, kita dapat menajalankan sistem operasi linux meskipun komputer yang digunakan menggunakan sistem operasi windows. VirtualBox berguna untuk melakukan simulasi tanpa perlu kehilangan sistem yang ada.

Pada pengamatan ini, kami menggunakan VirtualBox untuk melakukan virtualisasi sistem operasi berikut.
- Ubuntu Server 17.10, sebagai SSH server
- Kali Linux 2017.2, sebagai client yang akan melakukan penetrasi

Dengan VirtualBox, Ubuntu Server 17.10 dan Kali Linux 2017.2 dapat berjalan bersamaan pada komputer yang sama.

## SSH (Secure Shell )

SSH adalah protokol komukasi jaringan yang aman. SSH menyediakan secure channel diatas komunikasi jaringan client-server yang tidak aman. Infromasi yang melalui SSH akan didistribusikan dalam keadaan terenkripsi. Penggunaan SSH biasanya terdapat pada remote login atau remote command execution.

Pada pengamatan ini, kami menggunakan OpenSSH sebagai objek penetrasi. 

## Ubuntu Server 17.10 

Ubuntu Server 17.10 merupakan sistem operasi server berbasis Linux. Ubuntu Server berperan sebagai tempat berjalanya SSH Server, OpenSSH.

## Kali Linux 2017.2

Kali Linux merupakan sistem operasi turunan Debian berbasis Linux. Kali Linux memiliki lebih dari 600 penetration testing program, diantaranya Hydra dan Ncrack. Kali Linux berperan sebagai client yang akan melakukan penetrasi ke Ubuntu Server.

## Hydra dan Ncrack

Hydra dan Ncrack merupakan program login cracker yang men-support beragam protokol untuk diserang, termasuk SSH. Program ini bekerja secara brute force dengan mencoba segala kemungkinan kombinasi karakter. Program ini juga mensupport dictionary attack, yakni mencoba kemungkinan password dari daftar yang disediakan. 

# Hasil Uji Penetrasi 1

Sebelum kita melakukan uji penetrasi, terlebih dahulu kita harus menyiapkan tools yang diperlukan
> - Virtualbox, bisa didownload di *www.virtualbox.org/wiki/Downloads*. Pilih yang bagian *Windows hosts*
> - Ubuntu Server, bisa didownload di *www.ubuntu.com/download/server*. Pilih yang versi *Ubuntu Server 17.10*
> - Kali Linux, bisa didownload di *www.kali.org/downloads/*. Pilih *Kali 64 bit* dengan menggunakan *HTTP*

## Langkah Instalasi Ubuntu Server

Setelah kita menginstall VirtualBox, langkah selanjutnya adalah menyiapkan environment untuk Ubuntu Server
> - Buka Virtualbox, kemudian pilih **New**
> - Masukkan nama OS, Pilih type **Linux**, Pilih version **Ubuntu (64-bit)**
> - Set memory size **1024 MB**
> - Pilih opsi **Create a virtual hardisk now**
> - Pilih opsi **VHD (Virtual Hard Disk)**
> - Pilih opsi **Dynamically allocated**
> - Set alokasi disk minimal **10 GB** sesuai rekomendasi
> - Pilih **create**

Setelah kita menset-up environment, maka kita akan melakukan instalasi Ubuntu Server
> - Klik kanan pada virtual mesin yang sudah kita buat sebelumnya
> - Pilih **Start** kemudian **Normal Start**
> - Pada menu Select start-up disk pilih disk Ubuntu Server yang sudah kita download tadi, kemudain Pilih **Start**
> - Lakukan Instalasi Ubuntu Server yang pada umumnya

## Langkah Instalasi OS untuk Penetrasi

Sebelum kita menginstall Kali Linux

## Langkah Instalasi SSH Server

## Langkah Uji Penetrasi dengan SSH Brute Force Tools