# Index

### [Instalasi Cuckoo](#instalasi)
### [Konfigurasi Cuckoo](#konfigurasi)
### [Uji Coba](#ujicoba)

-----------------------------
Instalasi Cuckoo <a name="instalasi"/>
-----------------------------

Cuckoo merupakan software Open Source untuk menganalisa file yang dicurigai sebagai malware secara otomatis. Untuk melakukan hal tersebut, Cuckoo menggunakan suatu component yang memonitor malicious process ketika berjalan di environtment yang terisolasi. Cuckoo manjalankan file yang akan dianalisa pada suatu virtual machine. Tempat cuckoo berjalan disebut host, sementara virtual machine yang digunakan cuckoo untuk melakukan analisa disebut guest.

#### Pre-installation

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

Untuk sniffing packet, perlu menginstall tcpdump dan memastikan dapat menggunakan tcpdump tanpa root permission. Ketikkan command berikut.

```
apt-get install tcpdump
setcap cap_net_raw,cap_net_admin=eip /usr/sbin/
getcap /usr/sbin/tcpdump
```

##### Meng-install pydeep dan yara

Cuckoo Documentation mengatakan kita dapat menginstall Yara dan Pydeep sebagai optional plugin.
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


#### Installing Cuckoo
