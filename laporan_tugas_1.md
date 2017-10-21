# Pendahuluan

Kebutuhan informasi telah mendorong lahirnya internet sebagai teknologi pertukaran informasi tanpa barir jarak dan waktu.
Dengan adanya internet, informasi dapat berpindah dari suatu tempat ke tempat lain dalam hitungan mili detik. Tanpa biaya pengiriman dan hampir tanpa effort. Akibat kemudahan yang ditawarkan, internet telah menjadi teknologi yang diandalkan bahkan untuk proses penting seperti jual beli online, perbankan, pengiriman pesan pribadi, dan lain sebagainya. Namun, pada masa pengembangannya, internet tidak dirancang untuk menjaga kebenaran dan keaslian informasi sehingga informasi yang melalui internet mudah terekspos kepada pihak yang tidak berhak.

Salah satu mekanisme yang dapat diandalkan untuk menjamin keaslian informasi adalah authentication. Authentication berfungsi sebagai filter yang menentukan apakah seorang pengguna memiliki berhak atas suatu informasi atau tidak.

Pada tugas kali ini, kami akan mengamati kelemahan mekanisme authentication dengan melakukan uji penetrasi brute force untuk login ke SSH Server menggunakan tools Hydra dan NCrack pada Kali Linux OS. 

# Dasar Teori

## Virtual Box

## SSH Server

## Ubuntu Server 17.10 

## Kali Linux OS

## Hydra

## Ncrack

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