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
2. 
3. 

## 

