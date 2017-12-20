# Index

### [Instalasi Cuckoo](#instalasi)
### [Konfigurasi Cuckoo](#konfigurasi)
### [Uji Coba](#ujicoba)

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
