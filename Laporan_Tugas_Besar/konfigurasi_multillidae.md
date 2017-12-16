# Konfigurasi Multillidae

- Buka konfigurasi multillidae yang ada pada metasploit
- Ketikkan `nano /var/www/multilidae/config.inc`
- Default configurasi pada multillidae adalah seperti berikut:
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
    $dbpass = '';
    $dbname = 'owasp10';
?>
```
- Save konfigurasi 