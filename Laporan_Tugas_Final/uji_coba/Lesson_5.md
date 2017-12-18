# Lesson 5 : Manual SQL Injection with Firebug

## Start Web Browser Session to Multillidae
- Buka browser kemudian akses Multillidae sesuai ip metasploit pada alamat http://10.0.1.101/mutillidae
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_5/1/home_multillidae.png "Home page")

## SQL Injection: Single Quote Test On Username Field
1. Buka halaman Login/Register
2. Set Security Level menjadi **level 0 (Hosed)**
3. Tes penggunaan Single Qoute (')  
   - Pada kolom Textbox **Name** isikan single quote ('), lalu klik tombol login
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_5/1/login_page.png "Login Page")
4. Analisis Single Quote Result  
   - Jika pada bagian **username** isinya berupa single quote (') maka dapat dipastikan program backend rentan terhadap serangan sql injection
   - Pada bagian message query yang dihasilkan:  
   `SELECT * FROM accounts WHERE username=''' AND password=''`
   - Query pada umumnya:  
   `SELECT * FROM accounts WHERE username='admin' AND password='adminpass'`
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_5/1/test_single_quote.png "Test single quote")

## SQL Injection: By-Pass Password Without Username (Obtain Access #1)
1. Login Tanpa Menggunakan Password
   - Pada kolom Textbox **Name** isikan **`' or 1=1 -- `**
   - Pastikan anda menambahkan spasi setelah karakter **`--`**
   - Klik tombol Login
2. Verifikasi Result
   - Perhatikan anda login sebagai apa. Berdasarkan desain code Multillade, anda akan login sebagai admin. Hal tersebut terjadi karena admin adalah user pertama yang ada pada tabel accounts

**Note:**
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_5/1/error_table_not_found.png "Error message")
> Jika terjadi error, maka analisis error messagenya. 
> Pada contoh ini errornya terjadi karena table account yang ada di database metasploit tidak ada. Untuk mengatasi hal tersebut lakukan konfigurasi pada multilade yang ada di metasploit.
> [Konfigurasi multillidae](https://github.com/luqmanahmads/laporan-pksj/blob/master/Laporan_Tugas_Besar/konfigurasi_multillidae.md)

## SQL Injection: Single Quote Test On Password Field
1. Inspect Password Box Element
   - Buka halaman Login/Register
   - Pada kolom Textbox Name isikan samurai 
   - Pada kolom Textbox Password klik kanan, kemudian inspect element
2. Edit Password Box Element
   - Replace string password menjadi text
3. Single Quote (') Test
   - Pada kolom Password isikan single quote ('), lalu klik tombol login
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_5/1/replace_password_to_string.png "To text")
4. Analisis Single Quote Result
   - Jika pada bagian **password** isinya berupa single quote (') maka dapat dipastikan program backend rentan terhadap serangan sql injection
   - Pada bagian message query yang dihasilkan:  
   `SELECT * FROM accounts WHERE username='samurai AND password='''`
   - Query pada umumnya:  
   `SELECT * FROM accounts WHERE username='samurai' AND password='samurai'`
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_5/1/error_samurai.png "Error message")

## SQL Injection: Single Quote Test On Password Field (Obtain Access #2)
1. Inspect Password Box Element
   - Buka halaman Login/Register
   - Pada kolom Textbox Name isikan samurai
   - Pada kolom Textbox Password klik kanan, kemudian inspect element
2. Edit Password Box Element
   - Replace string password menjadi text
3. Berikan Kondisi Selalu True pada kolom Textbox Password
   - Pada kolom Textbox Password isikan **`' or 1=1 -- `**
   - Pastikan anda menambahkan spasi setelah karakter **`--`**
   - Klik tombol Login
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_5/2/replace_with_string.png "True condition")
4. Verifikasi Result
   - Berdasarkan desain code Multillade, anda akan login sebagai admin. Hal tersebut terjadi karena admin adalah user pertama yang ada pada tabel accounts
   - Akan tetapi, disini tujuan kita adalah login sebagai user samurai, bukan sebagai admin
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_5/2/result.png "Result")

## SQL Injection: Single Quote Test On Password Field (Obtain Access #3)
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
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_5/2/login_as_samurai.png "True condition")
4. Verifikasi Result
   - Anda akan login sebagai samurai
![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/lesson_5/3/result_login.png "Result")