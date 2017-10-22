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

Hydra dan Ncrack merupakan program login cracker yang men-support beragam protokol untuk diserang, termasuk SSH. Program ini bekerja secara brute force dengan mencoba segala kemungkinan kombinasi karakter. Pada pengamatan ini, kami akan melakukan dictionary attack, yakni mencoba kemungkinan password dari daftar yang disediakan, menggunakan Hydra dan Ncrack.

# Hasil Uji Penetrasi 1

Sebelum kita melakukan uji penetrasi, terlebih dahulu kita harus menyiapkan tools yang diperlukan
- Virtualbox, bisa didownload di *www.virtualbox.org/wiki/Downloads*. Pilih yang bagian *Windows hosts*
- Ubuntu Server, bisa didownload di *www.ubuntu.com/download/server*. Pilih yang versi *Ubuntu Server 17.10*
- Kali Linux, bisa didownload di *www.kali.org/downloads/*. Pilih *Kali 64 bit* dengan menggunakan *HTTP*

## Langkah Instalasi Ubuntu Server

Setelah kita menginstall VirtualBox, langkah selanjutnya adalah menyiapkan environment untuk Ubuntu Server
1. Buka Virtualbox, kemudian pilih **New**
2. Masukkan nama OS, Pilih type **Linux**, Pilih version **Ubuntu (64-bit)**
3. Set memory size minimal **1024 MB**
4. Pilih opsi **Create a virtual hardisk now**
5. Pilih opsi **VHD (Virtual Hard Disk)**
6. Pilih opsi **Dynamically allocated**
7. Set alokasi disk minimal **10 GB** sesuai rekomendasi
8. Pilih **create**

Setelah kita men-setup environment, maka kita akan melakukan instalasi Ubuntu Server
1. Klik kanan pada virtual mesin yang sudah kita buat sebelumnya
2. Pilih **Start**, kemudian pilih **Normal Start**
3. Pada menu Select start-up disk pilih disk Ubuntu Server yang sudah kita download tadi, kemudain Pilih **Start**
4. Lakukan Instalasi Ubuntu Server pada umumnya

**Catatan**
> - Jangan lupa untuk mencentang **OpenSSH Server** ketika menu dialog box **Software selection** muncul, agar aplikasi tersebut otomatis terinstall pada Ubuntu Server

## Langkah Instalasi OS untuk Penetrasi

Sebelum kita menginstall Kali Linux, terlebih dahulu kita harus menyiapkan environment sama seperti sebelumnya
1. Buka Virtualbox, kemudian pilih **New**
2. Masukkan nama OS, Pilih type **Debian**, Pilih version **Debian (64-bit)**
3. Set memory size minimal **1024 MB**
4. Pilih opsi **Create a virtual hardisk now**
5. Pilih opsi **VHD (Virtual Hard Disk)**
6. Pilih opsi **Dynamically allocated**
7. Set alokasi disk minimal **20 GB** sesuai rekomendasi
8. Pilih **create**

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

## Langkah Instalasi SSH Server

Dikarenakan kita sudah menginstall SSH Server diawal maka kita tidak perlu melakukan langkah ini. Akan tetapi, jika kita lupa untuk mencentang **OpenSSH Server** pada saat instalasi Ubuntu Server maka kita harus melakukan instalasi secara manual.

- Masuk keterminal pada Ubuntu Server
- Ketikkan `sudo apt-get update`
- Ketikkan `sudo apt-get install openssh-server`

## Langkah Uji Penetrasi dengan SSH Brute Force Tools

Kita tidak perlu untuk menginstall tools untuk penetrasi, karena di Kali Linux tools tersebut semuanya sudah lengkap terinstall.
Sebelum kita memulai ujicoba, siapkan terlebih dahulu kumpulan list kemungkinan password. **Ingat** didalam kumpulan password tersebut harus berisi **password asli** dari komputer tujuan. Disini kami meletakkan kumpulan password tersebut pada file **passlist.txt**. Untuk skenario ujicoba, kami akan mencoba melakukan brute force attack ke komputer **hendri** dengan alamat ip **10.151.32.4**.

**Uji Penetrasi 1**

Hydra
- Buka Aplikasi Hydra
- Syntax penggunaan : `hydra -l username -P ListofPassword ssh://iptujuan`
- Maka perintah yang dimasukkan : `hydra -l hendri -P passlist.txt ssh://10.151.32.4` 

Ncrack
- Buka Aplikasi Ncrack
- Syntax penggunaan : `ncrack -u username -P ListofPassword ssh://iptujuan`
- Maka perintah yang dimasukkan : `ncrack -u hendri -P passlist.txt ssh://10.151.32.4` 

Medusa
- Buka Aplikasi Ncrack
- Syntax penggunaan : `medusa -u username -P ListofPassword -h iptujuan -M ssh`
- Maka perintah yang dimasukkan : `medusa -u hendri -P passlist.txt -h 10.151.32.4 -M ssh`

**Hasil Uji Penetrasi 1**
1. Ketiga aplikasi tersebut berhasil dalam melakukan brute force attack dengan mencoba semua kemungkinan password yang ada.
2. Dari ketiga aplikasi tersebut, **Hydra** paling cepat dalam menemukan hasil pencarian kemudian disusul oleh **Ncrack** dan terakhir **Medusa**

**Uji Penetrasi 2**

Lakukan instalasi terlebih dahulu **file2ban** pada Ubuntu Server
- Masuk keterminal pada Ubuntu Server
- Ketikkan `sudo apt-get update`
- Ketikkan `sudo apt-get install fail2ban`

Konfigurasi fail2ban
- Atur konfigurasi di /etc/fail2ban/jail.conf
- Tambahkan beberapa baris dibawah berikut :
``` 
	enable = true
 	port = ssh
 	filter = sshd
 	logpath = /var/log/auth.log
 	maxretry = 6
 	bantime = 120
```
- Jalankan fail2ban dengan memasukkan perantah `sudo service fail2ban start` pada terminal

Hydra
- Masukkan perintah : `hydra -l hendri -P passlist.txt ssh://10.151.32.4` 

Ncrack
- Masukkan perintah : `ncrack -u hendri -P passlist.txt ssh://10.151.32.4` 

Medusa
- Masukkan perintah : `medusa -u hendri -P passlist.txt -h 10.151.32.4 -M ssh`

**Hasil Uji Penetrasi 2**
1. Ketiga aplikasi tersebut tidak pada saat melakukan percobaan ke tujuh karena mengalami connection refuse sehingga, gagal dalam melakukan brute force attack
2. Setelah melakukan percobaan 6 kali dan gagal, maka otomatis server akan **mem-banned** ip yang berusaha untuk mengakses server tersebut selama 2 menit
3. Hasil percobaan untuk melakukan brute force attack dapat dilihat di `**/var/log/auth.log**`