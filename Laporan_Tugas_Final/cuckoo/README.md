# Index

### [Instalasi Cuckoo](#instalasi)
### [Konfigurasi Cuckoo](#konfigurasi)
### [Testing](#testing)

-----------------------------
Instalasi Cuckoo <a name="instalasi"/>
-----------------------------

Cuckoo merupakan software Open Source untuk menganalisa file yang dicurigai sebagai malware secara otomatis. Untuk melakukan hal tersebut, Cuckoo menggunakan suatu component yang memonitor malicious process ketika berjalan di environtment yang terisolasi. Cuckoo manjalankan file yang akan dianalisa pada suatu virtual machine. Tempat cuckoo berjalan disebut host, sementara virtual machine yang digunakan cuckoo untuk melakukan analisa disebut guest.

#### A. Pre-installation

Sebelum melakukan instalasi Cuckoo, perlu menginstall package dan library yang dibutuhkan.

##### Meng-install python dan depedency-nya.

Ketikkan command berikut untuk menginstall python dan depedencynya.

```
sudo apt-get install python python-pip python-dev libffi-dev libssl-dev
sudo apt-get install python-virtualenv python-setuptools
sudo apt-get install libjpeg-dev zlib1g-dev swig
```

Untuk dapat menggunakan web interface cuckoo, perlu menginstall mongodb. ketikkan command berikut untuk menginstall mondodb.

```
sudo apt-get install mongodb
```

##### Meng-install tcpdump

Untuk sniffing packet, perlu menginstall tcpdump dan memastikan cuckoo dapat menggunakan tcpdump tanpa root permission. Ketikkan command berikut.

```
apt-get install tcpdump
setcap cap_net_raw,cap_net_admin=eip /usr/sbin/
```

Pastikan instalasi dengan command berikut. 

```
getcap /usr/sbin/tcpdump
```

akan menghasilkan line berikut

```
/usr/sbin/tcpdump = cap_net_admin,cap_net_raw+eip
```


##### Meng-install pydeep dan yara

Kita dapat menginstall Yara dan Pydeep sebagai optional plugin.
Untuk menginstall Yara ketikkan command berikut.

```
wget https://github.com/plusvic/yara/archive/v3.4.0.tar.gz -O yara-3.4.0.tar.gz
tar -zxf yara-3.4.0.tar.gz
cd yara-3.4.0
./bootstrap.sh
./configure --with-crypto --enable-cuckoo --enable-magic
make
sudo make install
```

validasi instalasi dengan mengetikkan command berikut.

```
yara -v
```

Untuk menginstall ekstensi yara-python, masuk ke dalam folder yara-python. Ketikkan command berikut.

```
cd yara-python
python setup.py build
sudo python setup.py install
```
Cek apakah yara-python telah terinstall dengan command berikut.

```
pip show yara-python
```

Pydeep bergantung pada ssdeep 2.8+. Untuk menginstall ssdeep ketikkan command berikut.

```
wget http://sourceforge.net/projects/ssdeep/files/ssdeep-2.12/ssdeep-2.12.tar.gz
tar xvzf ssdeep-2.12.tar.gz
cd ssdeep-2.12/
./configure && make && make install
```

Cek instalasi dengan command berikut.

```
ssdeep -V
```

Setelah itu install pydeep dengan command berikut.

```
pip install pydeep
```
cek instalasi.

```
pip show pydeep
```

##### Meng-install Volatility

Kita juga dapat menginstall Volatility, yang memerlukan distorm library.
Ketikkan command berikut untuk menginstall keduanya.

```
wget https://distorm.googlecode.com/files/distorm3.zip
unzip distorm3.zip
cd distorm3/
python setup.py build
python setup.py install

wget https://volatility.googlecode.com/files/volatility-2.3.1.tar.gz
tar xvzf volatility-2.3.1.tar.g
cd volatility-2.3.1/
python setup.py build
python setup.py install
```

#### B. Installing Cuckoo

Untuk menginstall cuckoo, pastikan pip dan setuptools telah terupdate. Kemudian instalasi cuckoo menggunakan pip.

```
sudo pip install -U pip setuptools
sudo pip install -U cuckoo
```

Coba jalankan Cuckoo dengan command `cuckoo`.
Cuckoo yang sedang berjalan akan tampak pada gambar berikut.

![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/cuckoo/1.png "Cuckoo")

#### C. Membuat Virtual Machine sebagai Guest

Cuckoo memerlukan paling tidak terdapat 1 guest untuk dapat melakukan analisa.
Untuk menginstall virtualbox, ketikkan command berikut.

```
apt-get install linux-headers-`uname -r` libvpx1
sudo apt-get install vitualbox
```

Karena konfigurasi default cuckoo menggunakan IP address 192.168.56.1 yang dimiliki oleh VirtualBox, kita perlu membuat networking insterface.

```
VBoxManage hostonlyif create
ip link set vboxnet0 up
ip addr add 192.168.56.1/24 dev vboxnet0
```

Buat virtual machine baru, atur memory dan jenis os dan harddisk dengan command berikut

```
VBoxManage createvm --name "WindowsXPSP3" --register
VBoxManage modifyvm "WindowsXPSP3" --memory 256 --acpi off --boot1 dvd
VBoxManage modifyvm "WindowsXPSP3" --ostype WindowsXP
VBoxManage createhd --filename windowsxpsp3.vdi --size 10000
```

definikan file harddisk dan file iso instalasi Windows XP yang akan digunakan.

```
BoxManage storagectl "WindowsXPSP3" --name "IDE Controller" --add ide
VBoxManage storageattach "WindowsXPSP3" --storagectl "IDE Controller" --port 0 --device 0 --type hdd --medium windowsxpsp3.vdi
VBoxManage storageattach "WindowsXPSP3" --storagectl "IDE Controller" --port 1 --device 0 --type dvddrive --medium windowsxpsp3.iso
VBoxManage modifyvm "WindowsXPSP3" --vrde on
``` 

Jalankan virtualbox dengan command berikut, dan lakukan instalasi Windows XP

```
VBoxManage startvm "WindowsXPSP3"
```
Windows XP yang telah terinstall akan tampak pada gambar berikut.

![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/cuckoo/2.png "Windows")

Pada virtual machine Windows lakukan download python melalui website nya https://www.python.org/downloads/ dan instal.
Cek instalasi python.

![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/cuckoo/3.png "Python Install")

Agar proses analisa berjalan lancar, Pastikan Windows Firewall dan Automatic Updates mati.

![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/cuckoo/4.png "Python Install")

Kemudian copykan script `agent.py` dari `.cuckoo/agent/agent.py` ke windows vitual machine. (dapat menggunakan sharedfolder atau membuat link download)
Letakkan script `agent.py` didalam folder `C:\Python27`. Script ini berguna untuk komunikasi antara cuckoo host dengan guest.

jalankan script `agent.py` dengan command berikut.

```
python.exe agent.py
```

![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/cuckoo/5.png "Agent")

Kemudian buat snapshot pada saat menajalankan agent.py agar agent.py dalam kondisi run ketika virtual machine digunakan oleh cuckoo.

![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/cuckoo/6.png "Snapshot")

Pengaturan virtual machine sebagai guest telah selesai. Selanjutkan, kita akan melakukan konfigurasi cuckoo agar menjalankan virtual machine yang telah dibuat untuk proses analisa.

-----------------------------
Konfigurasi Cuckoo <a name="konfigurasi"/>
-----------------------------

Seluruh konfigurasi cuckoo terletak pada folder `.cuckoo/conf`. Kali ini kita akan melakukan konfigurasi virtualbox melalui file `.cuckoo/conf/virtualbox.conf`. Command berikut akan menampilkan seluruh option configuration dengan menghilangkan seluruh comment.

```
cat virtualbox.conf | grep -v ^# | grep -v
```

![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/cuckoo/7.png "virtualbox.conf")

Konfigurasikan seperti gambar diatas.
`mode = headlesss` mendefinisikan agar pemanggilan virtual machine berjalan di background
`path = /usr/bin/VBoxManage` mendefinisikan direktori program yang menjalankan virtual machine
`machine = WindowsXPSP3` mendefinisikan nama virtual machine
`[WindowsXPSP3]` mendefinisikan blok konfigurasi untuk machine WindowsXPSP3
`label = WindowsXPSP3` mendefinisikan nama virtual machine yang terdapat pada konfigurasi virtualbox
`platform = windows` mendefiniskan platform os
`ip = 192.168.56.101` mendefinisika alamat ip virtual machine.

Save file `virtualbox.conf`.
t
Jalankan cuckoo host. Terlihat pada gambar cuckoo telah berhasil me-load virtual machine dan menunggu analysis task.

![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/cuckoo/8.png "Cuckoo Run")

Instalasi dan konfigurasi telah selesai. Selanjutnya kita akan mencoba menganalisis suatu malware menggunakan cuckoo

-----------------------------
Testing <a name="testing"/>
-----------------------------

Pada bagian ini, kita akan mencoba menganalisis malware menggunakan cuckoo. Kita akan mensubmit file malware melalui cuckoo web interface, mengamati apa terjadi pada virtual machine, dan melihat hasil analisa nya.

Pertama - tama, kita membutuhkan file malware.
File - file malware dapat diunduh di https://github.com/ytisf/theZoo/tree/master/malwares

Kita akan menggunakan file Ransomware Petya.
Download file Ransomware Petya dari link diatas kemudian extract.

Jalankan cuckoo dengan command berikut.

```
cuckoo
```

Jalankan cuckoo web interface pada terminal lain.

```
cuckoo web
```

Akses alamat localhost:8000.
Web interface akan terlihat seperti gambar berikut.

![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/cuckoo/9.png "Cuckoo Web")

Klik submit file for analysis. Pilih ransomware yang telah kita download tadi. Klik `analyze`.

Cuckoo akan mulai menjalankan file ransomware pada guest.

![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/cuckoo/10.png "Cuckoo Web")

Setelah cuckoo selesai menganalisa, coba jalankan virtual machine melalui virtualbox gui untuk melihat dampak yang dihasilkan oleh ransomeware tersebut.

Berikut ini dampak yang dihasilkan oleh ransomware petya.

![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/cuckoo/13.png "Cuckoo Web")

![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/cuckoo/14.png "Cuckoo Web")
 
Untuk melihat analisa melalui web, pilih tab `recent`. Disana akan ditampilkan list analysis task yang telah dilakukan. Dengan menklik salah satu task, akan menampilkan hasil detail hasil analisa. 

![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/cuckoo/11.png "Analysis Task")

![alt text](https://github.com/luqmanahmads/laporan-pksj/blob/master/assets/cuckoo/12.png "Report")
