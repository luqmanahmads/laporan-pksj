# Ubah Settingan Database
- Buka terminal pada metasploit
- Ubah password database mysql menjadi **saitama** dengan mengetikkan perintah-perintah berikut
```
mysql -u root -p
use mysql;
update user set password=PASSWORD('saitama') where User='root';
flush privileges;
quit
```
- Restart mysql dengan menggunakan perintah `sudo /etc/init.d/mysql restart`


# Konfigurasi Database Mutillidae
- Buka konfigurasi databases pada mutillidae yang ada pada metasploit
- Ketikkan `nano /var/www/mutillidae/config.inc`
- Default configurasi pada mutillidae adalah seperti berikut:
```php
<?php
    /* NOTE: On Samurai, the $dbpass password is "samurai" rather than blank */

    $dbhost = '0.0.0.0';
    $dbuser = 'root';
    $dbpass = '';
    $dbname = 'metasploit';
?>
```
- Ganti database yang digunakan (metasploit) menjadi **owasp10** (database yang sudah disediakan untuk sql injection)
```php
<?php
    /* NOTE: On Samurai, the $dbpass password is "samurai" rather than blank */

    $dbhost = '0.0.0.0';
    $dbuser = 'root';
    $dbpass = 'saitama';
    $dbname = 'owasp10';
?>
```
- Save konfigurasi 