# Lesson 5 : Manual SQL Injection with Firebug

## Start Web Browser Session to Mutillidae
- Buka Multilade pada browser dengan mengakses alamat http://10.0.1.101/mutillidae


## SQL Injection: Single Quote Test On Username Field

- Pergi kehalaman login
- Set Security Level menjadi 0 (Hosed)
- Tes penggunaan Single Qoute (')
Pada kolom textbox **Name** isikan single quote ('), lalu klik tombol login
- Analisis Single Quote Result
Jika pada bagian **username** isinya berupa single quote (') maka dapat dipastikan program backend rentan terhadap sql injection
Pada bagian message query yang dihasilkan 
`SELECT * FROM accounts WHERE username='**'**' AND password=''`
Query pada umumnya
SELECT * FROM accounts WHERE username='admin' AND password='adminpass'

## SQL Injection: By-Pass Password Without Username (Obtain Access #1)
- Pada kolom textbox **Name** isikan `**' or 1=1 -- **`
- Pastikan anda menambahkan spasi setelah karakter **--**
- Kolom textbox Password tidak perlu diisikan
- Klik tombol Login
