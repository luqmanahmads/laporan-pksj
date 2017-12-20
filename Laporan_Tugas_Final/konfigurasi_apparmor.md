# Konfigurasi AppArmor

- Buka Terminal pada mutillidae
- Cek status profile Apparmor yang sedang berjalan dengan menggunakan perintah: 
  ```bash
  sudo aa-status
  ```
- Disable profile mysqld dengan menjalankan perintah berikut:
  ```bash
  sudo ln -s /etc/apparmor.d/usr.sbin.mysqld /etc/apparmor.d/disable/
  ```
- Restart Apparmor
  ```bash
  sudo /etc/init.d/apparmor restart
  ```
- Cek status profile kembali untuk memastikan perubahan telah berjalan
  ```bash
  sudo aa-status
  ```