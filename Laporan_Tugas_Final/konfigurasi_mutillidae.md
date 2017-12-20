# Konfigurasi Mutillidae

- Buka konfigurasi mutillidae yang ada pada metasploit
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
    $dbpass = '';
    $dbname = 'owasp10';
?>
```
- Save konfigurasi 