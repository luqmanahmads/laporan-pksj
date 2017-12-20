# Lesson 9 : SQL Injection Union Exploit #2 (Create Output File)

## Start Web Browser Session to Mutillidae
- Buka browser kemudian akses Mutillidae sesuai ip metasploit pada alamat http://10.0.1.100/mutillidae
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/1/start_browser.png "Home page")

## Go To User Info Page
1. Go to User Info
   - OWASP Top 10 --> A1 - SQL Injection --> SQLi - Extract Data --> User Info 

## SQL Injection (Refresher Union Examples)
1. Inspect the Name Textbox with Firebug
   - Klik kanan pada Textbox Name, kemudian inspect element
2. Change Text Box Size
   - Pada bagian size, tambahkan kapasistasnya dari yg semula 20 menjadi 100
3. Second Union SQL Injection Attempt
   - Pada kolom Textbox **Name** isikan 
   ```' union select ccid,ccnumber,ccv,expiration,null from credit_cards -- ```
   - Pastikan anda menambahkan spasi setelah karakter **`--`**
   - Klik tombol View Account Details
4. Viewing the Results
	- Scroll kebawah untuk melihat hasilnya
	- Perhatikan hasilnya
	- Pada hasilnya, terdapat 5 record ditemukan

## SQL Injection (Union Example with Curl #5)
1. Go to User Info
   - OWASP Top 10 --> A1 - SQL Injection --> SQLi - Extract Data --> User Info 
2. Inspect the Name Textbox with Firebug
   - Klik kanan pada Textbox Name, kemudian inspect element
3. Change Text Box Size
   - Pada bagian size, tambahkan kapasistasnya dari yg semula 20 menjadi 100
4. Execute MySQL Union Injection
   - Pada kolom Textbox **Name** isikan 
   ```' union select ccid,ccnumber,ccv,expiration,null from credit_cards INTO OUTFILE '/var/www/mutillidae/CCN2.txt' FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"' LINES TERMINATED BY '\n' -- ```
   - Pastikan anda menambahkan spasi setelah karakter **`--`**
   - Klik tombol View Account Details
5. Viewing the Results
	- Scroll kebawah untuk melihat hasilnya
	- Perhatikan hasilnya
6. View Union Injection Output File
   - Pada address bar masukkan alamat url berikut http://10.0.1.100/mutillidae/CCN2.txt
   - Jika berhasil, maka akan tampil record data pada file CCN2.txt