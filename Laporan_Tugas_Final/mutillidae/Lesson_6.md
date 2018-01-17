# Lesson 5 : SQL Injection, Burpsuite, cURL, Man-In-The-Middle Attack

## Start Web Browser Session to Mutillidae
- Buka browser kemudian akses Mutillidae ip metasploit pada alamat http://10.0.1.100/mutillidae
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/1/start_browser.png "Home page")


## Go To Login Page
- Buka halaman Login/Register
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/1/start_browser.png "Home page")

## Configure Firefox Proxy Settings
1. View Preferences
   - Klik toggle menu yang ada di pojok kanan atas
   - Pilih **Preferences**
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/1/toggle_menu.png "Home page")
2. Advanced **Settings**
   - Pilih Menu **Advanced**
   - Pilih **Network** Tabs
   - Pilih **Settings**
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/1/network_tab.png "Home page")
3. Connection Settings
   - Gunakan **Manual proxy**
   - Pada kolom HTTP Proxy isikan **127.0.0.1**
   - Pada kolom Port **8080**
   - Centang bagian Use the proxy server for all protocols 
   - Klik **Ok**, Kemudian Close
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/1/set_config_proxy.png "Home page")

## Configure Burpsuite Settings
1. Buka Aplikasi Burpsuite
   - Akan muncul halaman aplikasi, pilih next
   - Use Burp defaults, kemudian klik start Burp
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/2/use_default.png "Home page")
2. Configure proxy
   - Pilih tab proxy, kemudian options
   - Set port ke **8080** seperti berikut
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/2/proxy_port_8080.png "Home page")
3. Turn off intercept
   - Pilih tab intercept
   - Pastikan intercept button dalam keadaan off (**intercept is off**), jika dalam keadaan on maka set ke off
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/2/intercept_off.png "Home page")

## SQL Injection: By-Pass Password Without Username (Obtain Access #1)
1. Login Tanpa Menggunakan Password
   - Pada kolom textbox **Name** isikan **`' or 1=1 -- `**
   - Pastikan anda menambahkan spasi setelah karakter **`--`**
   - Klik tombol **Login**
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/2/login_without_uname.png "Home page")
2. View Post Data (With Burp Suite)
   - Klik pada Tab **Proxy**
   - Klik pada Tab **History**
   - Klik cari line yang urlnya mengandung **/mutillidae/index.php?page=login.php** dengan Method POST
   - Klik pada Tab **Request**
   - Klik pada Tab **Raw**
   - Perhatikan isi string pada username, string tersebut  akan digunakan untuk melancarkan serangan
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/3/result.png "Home page")

## Simulate CURL SQL Injection: (Obtain Access #1)
1. Use Curl to Login with POST Data
   - Buka terminal, ketikkan perintah-perintah berikut : 
   ```bash
   curl -b crack_cookies.txt -c crack_cookies.txt --user-agent "Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)" --data "username=%27+or+1%3D1+--+&password=&login-php-submit-button=Login" --location "http://10.0.1.100/mutillidae/index.php?page=login.php" > login1.txt
   grep "Logged In" login1.txt
   cat crack_cookies.txt
   ```
   - Session cookies akan disimpan pada file crack_cookies.txt
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/3/result_obtain_1.png "Home page")

## SQL Injection: Single Quote Test On Password Field (Obtain Access #2)
1. Inspect Password Box Element
   - Buka halaman **Login/Register**
   - Pada kolom textbox **Name** isikan **samurai**
   - Pada kolom textbox **Password** klik kanan, kemudian inspect element
2. Edit Password Box Element
   - Replace string **password** menjadi **text**
   - Pada bagian size dan maxlength tambahkan kapasistasnya dari yg semula **20** menjadi **50**
   ![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/4/inspect_samurai.png "Home page")
3. Berikan Kondisi Selalu True pada kolom Textbox Password
   - Pada kolom textbox **Password** isikan **`' or (1=1 and username='samurai')--`**
   - Pastikan anda menambahkan spasi setelah karakter **`--`**
   - Klik tombol Login
   ![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/4/samurai_with_pass.png "Home page")
4. View Post Data (With Burp Suite)
   - Klik pada Tab **Proxy**
   - Klik pada Tab **History**
   - Klik cari line yang urlnya mengandung **/mutillidae/index.php?page=login.php** dengan Method POST
   - Klik pada Tab **Request**
   - Klik pada Tab **Raw**
   - Highligt text, kemudian klik kanan
   - Klik "Copy to File" 
5. Save File
   - Save file di dalam folder **root**
   - File Name: **burp2.txt**
   - Klik tombol **Save**
   ![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/4/save_burp.png "Home page")
6. View Post Data (With Burpsuite)
   - cd /root
   - Jalankan perintah berikut:
   ```bash
   grep -i cookie burp2.txt
   grep -i username burp2.txt
   ```
   ![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/4/run_command.png "Home page")

Simulate cURL SQL Injection: (Obtain Access #2)
1. Use Curl to Login with POST Data
   - Buka terminal, ketikkan perintah-perintah berikut : 
   ```bash 
   rm crack_cookies.txt
   curl -b crack_cookies.txt -c crack_cookies.txt --user-agent "Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)" --data "username=samurai&password=%27+or+%281%3D1+and+username%3D%27samurai%27%29+--+&login-php-submit-button=Login" --location "http://10.0.1.100/mutillidae/index.php?page=login.php" > login2.txt
   grep "Logged In" login2.txt
   cat crack_cookies.txt
   ```
   - Session cookies akan disimpan pada file crack_cookies.txt
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/4/run_command_2.png "Home page")

## Restore Firefox Original Proxy Configurations
1. View Preferences
   - Klik toggle menu yang ada di pojok kanan atas
   - Pilih **Preferences**
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/1/toggle_menu.png "Home page")
2. Advanced Settings
   - Pilih Menu **Advanced**
   - Pilih **Network** Tabs
   - Klik **Setting**
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/1/network_tab.png "Home page")
3. Connection Settings
   - Pilih **No proxy**
   - Klik **Ok**, Kemudian Close
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/5/set_no_proxy.png "Home page")

## Simulate Man-In-The-Middle Attack
1. Start Cookies Manager+
   - Pilih tab Tools > Cookies Manager+ > Klik Cookies Manager+
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/6/tab_cookie.png "Home page")
2. Add Cookie Entry
   - Klik New Cookie
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/6/cookie_view.png "Home page")
3. Add PHPSESSID Cookie Entry
   - Name: **PHPSESSID**
   - Content: **9bb112f7cbd1f361cec74b94466c9267**
   - Host: **10.0.1.100**
   - Path: **/**
   - Klik tombol **Save**
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/6/phpsessid.png "Home page")
4. Add username Cookie Entry
   - Name: **username**
   - Content: **samurai**
   - Host: **10.0.1.100**
   - Path: **/mutillidae/**
   - Klik tombol **Save**
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/6/username.png "Home page")
5. Add uid Cookie Entry
   - Name: **uid**
   - Content: **6**
   - Host: **10.0.1.100**
   - Path: **/mutillidae/**
   - Klik tombol **Save**
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/6/uid.png "Home page")
6. Verifikasi Result
   - Buka halaman http://10.0.1.100/mutillidae/index.php
   - Jika berhasil, anda akan login sebagai samurai
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_6/6/result.png "Home page")