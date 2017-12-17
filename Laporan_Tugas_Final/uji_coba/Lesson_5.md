# Lesson 5 : Manual SQL Injection with Firebug

## Start Web Browser Session to Mutillidae
- Buka Multilade pada browser dengan mengakses alamat http://10.0.1.101/mutillidae


## SQL Injection: Single Quote Test On Username Field
1. Buka halaman Login/Register
2. Set Security Level menjadi **level 0 (Hosed)**
3. Tes penggunaan Single Qoute (')  
Pada kolom Textbox **Name** isikan single quote ('), lalu klik tombol login
4. Analisis Single Quote Result  
Jika pada bagian **username** isinya berupa single quote (') maka dapat dipastikan program backend rentan terhadap sql injection

  - Pada bagian message query yang dihasilkan:  
  `SELECT * FROM accounts WHERE username=''' AND password=''`

  - Query pada umumnya:  
  `SELECT * FROM accounts WHERE username='admin' AND password='adminpass'`

## SQL Injection: By-Pass Password Without Username (Obtain Access #1)
1. Login Tanpa Menggunakan Password
   - Pada kolom Textbox **Name** isikan **`' or 1=1 -- `**
   - Pastikan anda menambahkan spasi setelah karakter **--**
   - Klik tombol Login
2. Verifikasi Result
   - Perhatikan anda login sebagai apa. Berdasarkan desain code Multillade, anda akan login sebagai admin. Hal tersebut terjadi karena admin adalah user pertama yang ada pada tabel accountts

**Note:**
> Jika terjadi error, maka analisis error messagenya. 
> Pada contoh ini errornya terjadi karena table account yang ada di database metasploit tidak ada. Untuk mengatasi hal tersebut lakukan konfigurasi pada multilade yang ada di metasploit.
> [Konfigurasi multillidae](https://github.com/luqmanahmads/laporan-pksj/blob/master/Laporan_Tugas_Besar/konfigurasi_multillidae.md)

## SQL Injection: Single Quote Test On Password Field
- Buka halaman Login/Register
