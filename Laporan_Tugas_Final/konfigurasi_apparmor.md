# Konfigurasi AppArmor

- Buka Terminal pada mutillidae
- Cek status profile Apparmor yang sedang berjalan dengan menggunakan perintah: 
  ```sudo aa-status```
- Disable profile mysqld dengan menjalankna perintah berikut
  ```sudo ln -s /etc/apparmor.d/usr.sbin.mysqld /etc/apparmor.d/disable/```
- Restart Apparmor
  ```sudo /etc/init.d/apparmor restart```
- Cek status profile kembali
  ```sudo aa-status```