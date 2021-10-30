# Jarkom Modul 2 A08 - 2021

Anggota:
-   05111940000030 - Bunga Fairuz Wijdan
-   05111940000062 - Thomas Felix Brilliant
-   05111940000145 - Ikhlasul Amal Rivel

**Daftar Soal:**

* [Soal 1](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021#Soal-1)
* [Soal 2](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021#Soal-2)
* [Soal 3](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021#Soal-3)
* [Soal 4](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021#Soal-4)
* [Soal 5](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021#Soal-5)
* [Soal 6](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021#Soal-6)
* [Soal 7](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021#Soal-7)
* [Soal 8](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021#Soal-8)
* [Soal 9](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021#Soal-9)
* [Soal 10](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021#Soal-10)
* [Soal 11](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021#Soal-11)
* [Soal 12](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021#Soal-12)
* [Soal 13](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021#Soal-13)
* [Soal 14](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021#Soal-14)
* [Soal 15](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021#Soal-15)
* [Soal 16](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021#Soal-16)
* [Soal 17](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021#Soal-17)

## Narasi Pendahuluan

Luffy adalah seorang yang akan jadi Raja Bajak Laut. Demi membuat Luffy menjadi Raja Bajak Laut, Nami ingin membuat sebuah peta, bantu Nami untuk membuat peta berikut:

<img src="https://user-images.githubusercontent.com/37539546/139521519-b07ec09a-047a-40af-bc7e-ec79be91fc8c.JPG" width=600>

## Soal 1

### EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypiea akan digunakan sebagai Web Server. Terdapat 2 client yaitu Loguetown dan Alabasta. Semua node terhubung pada router Foosha sehingga dapat mengakses internet.

### Jawaban:

Pertama, membuat topologi sesuai permintaan soal. Kemudian *setting network* masing-masing *node* dengan fitur `Edit network configuration` yang ada di menu `Configure`. *Setting* awal yang sudah ada dapat dihapus dan diganti dengan konfigurasi berikut:

- Foosha
  ```
  auto eth0
  iface eth0 inet dhcp

  auto eth1
  iface eth1 inet static
      address 10.3.1.1
      netmask 255.255.255.0

  auto eth2
  iface eth2 inet static
      address 10.3.2.1
      netmask 255.255.255.0
  ```
  
- Loguetown
  ```
  auto eth0
  iface eth0 inet static
      address 10.3.1.2
      netmask 255.255.255.0
    gateway 10.3.1.1
  ```
  
- Alabasta
  ```
  auto eth0
  iface eth0 inet static
	    address 10.3.1.3
	    netmask 255.255.255.0
	    gateway 10.3.1.1
  ```
  
- EniesLobby
  ```
  auto eth0
  iface eth0 inet static
      address 10.3.2.2
      netmask 255.255.255.0
      gateway 10.3.2.1
  ```
  
- Water7
  ```
  auto eth0
  iface eth0 inet static
      address 10.3.2.3
      netmask 255.255.255.0
      gateway 10.3.2.1
  ```
  
- Skypiea
  ```
  auto eth0
  iface eth0 inet static
      address 10.3.2.4
      netmask 255.255.255.0
      gateway 10.3.2.1
  ```

### Foosha

*Restart* semua *node*. Lalu jalankan *command* berikut pada *router* `Foosha` untuk pengaturan lalu lintas komputer.
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.3.0.0/16
```
(Note: *Prefix* IP yang digunakan sesuai *Prefix* IP Kelompok, dalam hal ini kelompok A8 adalah **10.3**).

Dan ketikkan *command* ini pada `Foosha` untuk melihat IP DNS:
```
cat /etc/resolv.conf
```
Akan muncul *nameserver* yang akan digunakan pada konfigurasi selanjutnya.

<img src="https://user-images.githubusercontent.com/37539546/139521833-df4ae23e-08eb-4927-bd0e-992ec548670f.JPG" width="350">

### Semua node (kecuali Foosha)

Agar *node*-*node* lainnya dapat mengakses internet, jalankan *command* berikut dan gunakan IP DNS dari `Foosha` tadi.
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
```

*Restart* semua *node* kembali. Lalu, *testing* semua *node* apakah sudah terkoneksi dengan internet dengan `ping` ke [**google.com**](google.com). Sebagai contoh pada `Loguetown`:

<img src="https://user-images.githubusercontent.com/37539546/139522122-43ddb61d-0b3a-484f-a4a5-433109dd6529.JPG" width="600">

## Soal 2

### Luffy ingin menghubungi Franky yang berada di EniesLobby dengan denden mushi. Kalian diminta Luffy untuk membuat website utama dengan mengakses [**franky.yyy.com**](franky.yyy.com) dengan alias **www.franky.yyy.com** pada folder `kaizoku`.

### Jawaban:

### EniesLobby

Melakukan instalasi **bind9** terlebih dahulu pada `EniesLobby` dengan *update package list*. *Command* yang dijalankan adalah sebagai berikut.
```
apt-get update
apt-get install bind9 -y
```

Setelah instalasi selesai, buat domain [**franky.yyy.com**](franky.yyy.com). Lakukan *command* seperti berikut pada `EnniesLobby`.
```
nano /etc/bind/named.conf.local
```

Isi konfigurasi domain [**franky.yyy.com**](franky.yyy.com) sesuai sintaks berikut.
```
zone "franky.a08.com" {
    type master;
    file "/etc/bind/kaizoku/franky.a08.com";
};
```

Buat folder baru, yaitu **kaizoku** pada **/etc/bind**.
```
mkdir /etc/bind/kaizoku
```

*Copy file* **db.local** ke dalam folder **kaizoku** yang baru dibuat dan ubah namanya menjadi [**franky.a08.com**](franky.a08.com).
```
cp /etc/bind/db.local /etc/bind/jarkom/franky.a08.com
```

Buka *file* [**franky.a08.com**](franky.a08.com) dan edit seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139529629-1223d83b-da67-4cfe-a57a-8950972f0b59.JPG" width="600">

Dalam konfigurasi ini sudah ditambahkan *record* **CNAME** [**www.franky.a08.com**](www.franky.a08.com) untuk membuat alias yang mengarahkan domain ke [**franky.a08.com**](franky.a08.com).

*Restart* **bind9**.
```
service bind9 restart
```

### Loguetown atau Alabasta

Lakukan *testing* dengan menambahkan `nameserver 10.3.2.2` (IP EniesLobby) pada `Loguetown` dan `Alabasta` untuk cek apakah [**franky.a08.com**](franky.a08.com) atau [**www.franky.a08.com**](www.franky.a08.com) dapat diakses. Jika sukses, maka akan memunculkan hasil seperti berikut.

<img src="https://user-images.githubusercontent.com/37539546/139522919-ded4c8b1-6574-4f97-a8d5-53c86dace204.JPG" width="500">

<img src="https://user-images.githubusercontent.com/37539546/139522948-93560bd1-891c-42e7-ac4c-d26980132d40.JPG" width="500">

## Soal 3

### Setelah itu buat subdomain [**super.franky.yyy.com**](super.franky.yyy.com) dengan alias **www.super.franky.yyy.com** yang diatur DNS-nya di EniesLobby dan mengarah ke Skypiea.

### Jawaban:

### EniesLobby

Buka *file* [**franky.a08.com**](franky.a08.com) dan edit seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139523149-b1759e13-7cea-4cbc-aebb-210e1e4c624c.JPG" width="600">

Buat *file* lagi, yaitu [**super.franky.a08.com**](super.franky.a08.com) dan edit seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139529702-14292d61-9ef5-437b-99d6-7896f3a64211.JPG" width="600">

Tambahkan konfigurasi berikut pada **/etc/bind/named.conf.local** di `EnniesLobby`.
```
zone "super.franky.a08.com" {
    type master;
    file "/etc/bind/kaizoku/super.franky.a08.com";
};
```

### Loguetown atau Alabasta

Lakukan *testing* pada `Loguetown` dan `Alabasta` untuk cek apakah [**super.franky.a08.com**](super.franky.a08.com) atau [**www.super.franky.a08.com**](www.super.franky.a08.com) dapat diakses. Jika sukses, maka akan memunculkan hasil seperti berikut.

<img src="https://user-images.githubusercontent.com/37539546/139523365-e34b96a3-692e-41a7-a0d5-66bcf0ba5eb2.JPG" width="500">

<img src="https://user-images.githubusercontent.com/37539546/139523384-21b48893-eb45-4d8a-a88c-8197453bcdef.JPG" width="500">

## Soal 4

### Buat juga reverse domain untuk domain utama.

### Jawaban:

### Enies Lobby

Edit *file* **/etc/bind/named.conf.local** pada `EniesLobby` dan tambahkan konfigurasi berikut.  Tambahkan *reverse* dari 3 *bytes* awal dari IP yang ingin dilakukan **Reverse DNS**. Dalam hal ini IP **10.3.2** untuk IP dari *record* sehingga *reverse*-nya adalah **2.3.10**.
```
zone "2.3.10.in-addr.arpa" {
    type master;
    file "/etc/bind/kaizoku/2.3.10.in-addr.arpa";
};
```

*Copy file* **db.local** ke dalam folder **kaizoku** dan ubah namanya menjadi [**2.3.10.in-addr.arpa**](2.3.10.in-addr.arpa).
```
cp /etc/bind/db.local /etc/bind/jarkom/2.3.10.in-addr.arpa
```

Buka *file* [**2.3.10.in-addr.arpa**](2.3.10.in-addr.arpa) dan edit seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139523893-6b7d1253-49c3-4864-9e6e-a5bdf89a879d.JPG" width="600">

*Restart* **bind9**.
```
service bind9 restart
```

### Loguetown

Untuk mengecek apakah konfigurasi sudah benar atau belum, lakukan perintah berikut pada `Loguetown`.
```
// Install package dnsutils, ubah nameserver ke 192.168.122.1
apt-get update
apt-get install dnsutils -y

// Kembalikan nameserver agar tersambung dengan EniesLobby
host -t PTR 10.3.2.2
```

Akan muncul seperti ini.

<img src="https://user-images.githubusercontent.com/37539546/139524715-85d1796b-09f9-4432-98a2-38e3c57670e5.JPG" width="450">

## Soal 5

### Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama.

### Jawaban:

### EniesLobby

Modifikasi konfigurasi berikut pada **/etc/bind/named.conf.local** di `EnniesLobby`.
```
zone "super.franky.a08.com" {
    type master;
    notify yes;
    also-notify { 10.3.2.3; }; // IP Water7
    allow-transfer { 10.3.2.3; }; // IP Water7
    file "/etc/bind/kaizoku/super.franky.a08.com";
};
```

*Restart* **bind9**.
```
service bind9 restart
```

### Water7

Melakukan instalasi **bind9** pada `Water7` dengan *update package list*. *Command* yang dijalankan adalah sebagai berikut.
```
apt-get update
apt-get install bind9 -y
```

Tambahkan konfigurasi berikut pada **/etc/bind/named.conf.local** di `Water7`.
```
zone "super.franky.a08.com" {
    type slave;
    masters { 10.3.2.2; }; // IP EniesLobby
    file "/var/lib/bind/franky.a08.com";
};
```

*Restart* **bind9**.
```
service bind9 restart
```

### EniesLobby

Lakukan *testing* pada `Loguetown` dan `Alabasta` untuk cek apakah **DNS Slave** berhasil dibuat pada `Water7`. *Stop service* bind9 terlebih dahulu pada `EniesLobby`.
```
service bind9 stop
```

### Loguetown atau Alabasta

Pada `Loguetown` dan `Alabasta` jangan lupa untuk menambahkan *nameserver* `Water7`, yaitu **10.3.2.3** pada **/etc/resolv.conf**, sehingga menjadi seperti ini:

<img src="https://user-images.githubusercontent.com/37539546/139530207-a51f307c-8945-4c1a-8692-8da693d5ec11.JPG" width="500">

Hasilnya akan seperti di bawah ketika dijalankan di `Loguetown`:

<img src="https://user-images.githubusercontent.com/37539546/139530356-13e882cc-3525-4479-b9c5-c9f11d768a88.JPG" width="500">

## Soal 6

### Setelah itu, terdapat subdomain [**mecha.franky.yyy.com**](mecha.franky.yyy.com) dengan alias **www.mecha.franky.yyy.com** yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypiea dalam folder `sunnygo`.

### Jawaban:

### EniesLobby

Buka *file* [**franky.a08.com**](franky.a08.com) dan edit seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139530527-b4750810-fee9-4da7-9a9e-bc1c4a19ebb9.JPG" width="600">

Buka *file* **/etc/bind/named.conf.options** dan edit seperti konfigurasi berikut. *Comment* bagian `dnssec-validation auto` dan tambahkan di baris bawahnya `allow-query{any;}`.

<img src="https://user-images.githubusercontent.com/37539546/139530616-d90b6191-713f-4673-9edf-152070e667b5.JPG" width="600">

### Water7

Tambahkan konfigurasi berikut pada **/etc/bind/named.conf.local** di `Water7`.
```
zone "mecha.franky.a08.com" {
    type master;
    file "/etc/bind/sunnygo/mecha.franky.a08.com";
};
```

Buat folder baru, yaitu **sunnygo** pada **/etc/bind**.
```
mkdir /etc/bind/sunnygo
```

*Copy file* **db.local** ke dalam folder **sunnygo** dan ubah namanya menjadi [**mecha.franky.a08.com**](mecha.franky.a08.com).
```
cp /etc/bind/db.local /etc/bind/jarkom/mecha.franky.a08.com
```

Buka *file* [**mecha.franky.a08.com**](mecha.franky.a08.com) dan edit seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139531047-82e0fa32-6851-4859-be7b-faf07e04109f.JPG" width="600">

Dalam konfigurasi ini sudah ditambahkan *record* **CNAME** [**www.mecha.franky.a08.com**](www.mecha.franky.a08.com) untuk membuat alias yang mengarahkan domain ke [**mecha.franky.a08.com**](mecha.franky.a08.com).

*Restart* **bind9**.
```
service bind9 restart
```

### Loguetown atau Alabasta

Lakukan *testing* pada `Loguetown` dan `Alabasta` untuk cek apakah [**mecha.franky.a08.com**](mecha.franky.a08.com) atau [**www.mecha.franky.a08.com**](www.mecha.franky.a08.com) dapat diakses. Jika sukses, maka akan memunculkan hasil seperti berikut.

<img src="https://user-images.githubusercontent.com/37539546/139531613-c191c6b2-8c7a-4d01-82b7-def42514118f.JPG" width="500">

<img src="https://user-images.githubusercontent.com/37539546/139531624-56f01b46-11de-4543-a02a-988f1ce00cd3.JPG" width="500">

## Soal 7

### Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama [**general.mecha.franky.yyy.com**](general.mecha.franky.yyy.com) dengan alias **www.general.mecha.franky.yyy.com** yang mengarah ke Skypiea.

### Jawaban:

### Water7

Tambahkan konfigurasi berikut pada **/etc/bind/named.conf.local** di `Water7`.
```
zone "general.mecha.franky.a08.com" {
    type master;
    file "/etc/bind/sunnygo/general.mecha.franky.a08.com";
};
```

*Copy file* **db.local** ke dalam folder **sunnygo** dan ubah namanya menjadi [**general.mecha.franky.a08.com**](mecha.franky.a08.com).
```
cp /etc/bind/db.local /etc/bind/jarkom/general.mecha.franky.a08.com
```

Buka *file* [**general.mecha.franky.a08.com**](general.mecha.franky.a08.com) dan edit seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139531485-d16ce9c1-ca0b-497c-9cf2-7f32e1b026b6.JPG" width="600">

Dalam konfigurasi ini sudah ditambahkan *record* **CNAME** [**www.general.mecha.franky.a08.com**](www.general.mecha.franky.a08.com) untuk membuat alias yang mengarahkan domain ke [**general.mecha.franky.a08.com**](general.mecha.franky.a08.com).

*Restart* **bind9**.
```
service bind9 restart
```

### Loguetown atau Alabasta

Lakukan *testing* pada `Loguetown` dan `Alabasta` untuk cek apakah [**general.mecha.franky.a08.com**](general.mecha.franky.a08.com) atau [**www.general.mecha.franky.a08.com**](www.general.mecha.franky.a08.com) dapat diakses. Jika sukses, maka akan memunculkan hasil seperti berikut.

<img src="https://user-images.githubusercontent.com/37539546/139531547-b314ca87-c5ea-4be1-9174-5b909464ee60.JPG" width="500">

<img src="https://user-images.githubusercontent.com/37539546/139531556-772c390d-1c65-4eb9-9851-5a3764831a98.JPG" width="500">

## Soal 8

### Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Web Server. Pertama dengan Web Server **www.franky.yyy.com**. Pertama, Luffy membutuhkan Web Server dengan DocumentRoot pada `/var/www/franky.yyy.com`.

### Jawaban:

### Skypiea

Melakukan instalasi **PHP**, **Apache2**, dan **Library Apache2** terlebih dahulu pada `Skypiea` dengan *update package list*. *Command* yang dijalankan adalah sebagai berikut.
```
apt-get update
apt-get install apache2 -y
apt-get install php -y
apt-get install libapache2-mod-php7.0 -y

apt-get install wget -y // Opsional, jika di perangkat tidak ada
apt-get install unzip -y // Opsional, jika di perangkat tidak ada
```

*Download file requirement* yang [sudah diberikan](https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom) lewat *notes* di bawah menggunakan `wget`.
```
https://raw.githubusercontent.com/FeinardSlim/Praktikum-Modul-2-Jarkom/main/franky.zip
https://raw.githubusercontent.com/FeinardSlim/Praktikum-Modul-2-Jarkom/main/super.franky.zip
https://raw.githubusercontent.com/FeinardSlim/Praktikum-Modul-2-Jarkom/main/general.mecha.franky.zip
```
(Note: Di sini menggunakan [RAW](https://raw.githubusercontent.com/) dari Github karena ada masalah saat proses *unzip*).

*Unzip* **.zip** yang telah diunduh dan diletakkan di folder **root**.
```
unzip ~/franky.zip -d ~/
```

Buat folder baru, yaitu **franky.a08.com** pada **/var/www**.
```
mkdir /var/www/franky.a08.com
```

*Copy* semua *file* yang ada di folder hasil *unzip* ke folder **/var/www/franky.a08.com**.
```
cp ~/franky/home.html /var/www/franky.a08.com
cp ~/franky/index.php /var/www/franky.a08.com
```

*Copy file* **000-default.conf** ke dalam folder **sites-available** dan ubah namanya menjadi **franky.a08.com.conf**.
```
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/franky.a08.com.conf
```

Buka *file* **franky.a08.com.conf** dan edit seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139532319-6b6ae9af-b613-4b78-9c9a-c6f00cf45e15.JPG" width="600">

Aktifkan konfigurasi website dengan *command* berikut.
```
a2ensite franky.a08.com.conf
```

*Restart* **apache2**.
```
service apache2 restart
```

### Loguetown atau Alabasta

Lakukan *testing* pada `Loguetown` dan `Alabasta` untuk cek apakah [**franky.a08.com**](franky.a08.com) atau [**www.franky.a08.com**](www.franky.a08.com) dapat diakses. Menggunakan **Lynx** untuk mengeceknya. *Install* terlebih dahulu **Lynx** jika belum ada.
```
// Ubah nameserver menjadi 192.168.122.1 agar bisa mengunduh, jangan lupa untuk mengembalikan ke bentuk semula
apt-get install lynx -y
```

Kemudian, lakukan perintah ini untuk membuka website.
```
lynx franky.a08.com
lynx www.franky.a08.com
```

Jika sukses, maka akan memunculkan hasil seperti berikut.

<img src="https://user-images.githubusercontent.com/37539546/139532643-22796155-cfc2-4983-a7bc-53818e75ea2b.JPG" width="600">

## Soal 9

### Setelah itu, Luffy juga membutuhkan agar URL **www.franky.yyy.com/index.php/home** dapat menjadi menjadi **www.franky.yyy.com/home**.

### Jawaban:

### Skypiea

Buka *file* **franky.a08.com.conf** dan tambahkan seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139532756-0c25df64-c231-4b55-b239-a5a76036db18.JPG" width="600">

Buat *file* **.htaccess** pada folder **/var/www/franky.a08.com** dan tambahkan seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139533042-8b995690-a43b-4a7d-a3e3-275cfaa13a7b.JPG" width="350">

Aktifkan **module rewrite** agar penulisan URL menjadi lebih rapi.
```
a2enmod rewrite
```

*Restart* **apache2**.
```
service apache2 restart
```
### Loguetown atau Alabasta

Lakukan *testing* pada `Loguetown` dan `Alabasta` untuk cek apakah **module rewrite** berhasil dilakukan.
```
lynx franky.a08.com/home
```

Jika sukses, maka akan memunculkan hasil seperti berikut.

<img src="https://user-images.githubusercontent.com/37539546/139532643-22796155-cfc2-4983-a7bc-53818e75ea2b.JPG" width="600">

## Soal 10

### Setelah itu, pada subdomain **www.super.franky.yyy.com**, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada `/var/www/super.franky.yyy.com`.

### Jawaban:

### Skypiea

*Unzip* **.zip** yang telah diunduh dan diletakkan di folder **root**.
```
unzip ~/super.franky.zip -d ~/
```

Buat folder baru, yaitu **super.franky.a08.com** pada **/var/www**.
```
mkdir /var/www/super.franky.a08.com
```

*Copy* semua *file* yang ada di folder hasil *unzip* ke folder **/var/www/super.franky.a08.com**.
```
cp -r ~/super.franky/error /var/www/franky.a08.com
cp -r ~/super.franky/public /var/www/franky.a08.com
```

*Copy file* **000-default.conf** ke dalam folder **sites-available** dan ubah namanya menjadi **super.franky.a08.com.conf**.
```
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/super.franky.a08.com.conf
```

Buka *file* **super.franky.a08.com.conf** dan tambahkan seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139533323-3de741e8-9c1e-4281-a67c-12db8235fe98.JPG" width="600">

Aktifkan konfigurasi website dengan *command* berikut.
```
a2ensite super.franky.a08.com.conf
```

*Restart* **apache2**.
```
service apache2 restart
```

### Loguetown atau Alabasta

Lakukan *testing* pada `Loguetown` dan `Alabasta` untuk cek apakah [**super.franky.a08.com**](super.franky.a08.com) atau [**www.super.franky.a08.com**](www.super.franky.a08.com) dapat diakses. 
```
lynx super.franky.a08.com
lynx www.super.franky.a08.com
```

Jika sukses, maka akan memunculkan hasil seperti berikut.

<img src="https://user-images.githubusercontent.com/37539546/139533423-055eece5-87dd-4bed-9b86-5bfeb3400273.JPG" width="600">

## Soal 11

### Akan tetapi, pada folder `/public`, Luffy ingin hanya dapat melakukan directory listing saja.

### Jawaban:

### Skypiea

Buka *file* **super.franky.a08.com.conf** dan tambahkan seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139533527-07703fd8-ca6b-49f6-a7a9-d55a9494f054.JPG" width="600">

*Restart* **apache2**.
```
service apache2 restart
```

### Loguetown atau Alabasta

Lakukan *testing* pada `Loguetown` dan `Alabasta` untuk cek apakah **directory listing** berhasil.
```
lynx super.franky.a08.com/public
lynx www.super.franky.a08.com/public
```

Jika sukses, maka akan memunculkan hasil seperti berikut.

<img src="https://user-images.githubusercontent.com/37539546/139533619-16eb1ea6-7dd0-428e-a8d4-89167c26169c.JPG" width="600">

## Soal 12

### Tidak hanya itu, Luffy juga menyiapkan error file `404.html` pada folder `/error` untuk mengganti error kode pada apache.

### Jawaban:

### Skypiea

Buka *file* **super.franky.a08.com.conf** dan tambahkan seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139533705-2880407c-a6d6-4e0e-bcc0-d91acf922616.JPG" width="600">

*Restart* **apache2**.
```
service apache2 restart
```

### Loguetown atau Alabasta

Lakukan *testing* pada `Loguetown` dan `Alabasta` untuk cek apakah *error file* **404.html** berhasil dimunculkan.
```
lynx super.franky.a08.com/wangy // Penamaan URL bebas
lynx www.super.franky.a08.com/wangy // Penamaan URL bebas
```

Jika sukses, maka akan memunculkan hasil seperti berikut.

<img src="https://user-images.githubusercontent.com/37539546/139533780-8d0c02e5-e355-4259-b2af-c4bd580e1d23.JPG" width="600">

## Soal 13

### Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset **www.super.franky.yyy.com/public/js** menjadi **www.super.franky.yyy.com/js**.

### Jawaban:

### Skypiea

Buka *file* **super.franky.a08.com.conf** dan tambahkan seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139533893-3993743f-74a7-4689-b390-08cdf7275951.JPG" width="600">

*Restart* **apache2**.
```
service apache2 restart
```

### Loguetown atau Alabasta

Lakukan *testing* pada `Loguetown` dan `Alabasta` untuk cek apakah **virtual host** berhasil dibuat.
```
lynx super.franky.a08.com/public/js
lynx www.super.franky.a08.com/public/js
```

Jika sukses, maka akan memunculkan hasil seperti berikut.

<img src="https://user-images.githubusercontent.com/37539546/139533938-687bc140-7d31-4424-a998-fbb57c493f34.JPG" width="600">

## Soal 14

### Dan Luffy meminta untuk web **www.general.mecha.franky.yyy.com** hanya bisa diakses dengan port 15000 dan port 15500.

### Jawaban:

### Skypiea

*Unzip* **.zip** yang telah diunduh dan diletakkan di folder **root**.
```
unzip ~/general.mecha.franky.zip -d ~/
```

Buat folder baru, yaitu **general.mecha.franky.a08.com** pada **/var/www**.
```
mkdir /var/www/general.mecha.franky.a08.com
```

*Copy* semua *file* yang ada di folder hasil *unzip* ke folder **/var/www/general.mecha.franky.a08.com**.
```
cp ~/general.mecha.franky/* /var/www/general.mecha.franky.a08.com
```

*Copy file* **000-default.conf** ke dalam folder **sites-available** dan ubah namanya menjadi **general.mecha.franky.a08.com.conf**.
```
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/general.mecha.franky.a08.com.conf
```

Buka *file* **general.mecha.franky.a08.com.conf** dan tambahkan seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139534099-19ff3a94-7fea-42e7-bd8c-348361af7701.JPG" width="600">

Buka *file* **ports.conf** pada folder **/etc/apache2** dan tambahkan seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139534133-9b2e3259-64ff-4cab-9eb5-e968e78f1d3d.JPG" width="600">

Aktifkan konfigurasi website dengan *command* berikut.
```
a2ensite general.mecha.franky.a08.com.conf
```

*Restart* **apache2**.
```
service apache2 restart
```

### Loguetown atau Alabasta

Lakukan *testing* pada `Loguetown` dan `Alabasta` untuk cek apakah [**general.mecha.franky.a08.com**](general.mecha.franky.a08.com) atau [**www.general.mecha.franky.a08.com**](www.general.mecha.franky.a08.com) dapat diakses. 
```
lynx general.mecha.franky.a08.com:15000 atau general.mecha.franky.a08.com:15500 
lynx www.general.mecha.franky.a08.com:15000 atau www.general.mecha.franky.a08.com:15500
```

Jika sukses, maka akan memunculkan hasil seperti berikut.

<img src="https://user-images.githubusercontent.com/37539546/139534298-175a5cc6-ef59-4546-9c1a-cf450331147b.JPG" width="600">

## Soal 15

### Dengan autentikasi username `luffy` dan password `onepiece` dan file di `/var/www/general.mecha.franky.yyy`.

### Jawaban:

### Skypiea

Buat *username* dan *password* baru dengan *command* berikut. *Username* yang dipakai adalah **luffy** dan *password*-nya **onepiece**.
```
htpasswd -c /etc/apache2/.htpasswd luffy
```

Akan muncul di terminal seperti ini:

<img src="https://user-images.githubusercontent.com/37539546/139534557-bd043099-7d52-4366-b980-97f827784ef1.JPG" width="600">

Buka *file* **general.mecha.franky.a08.com.conf** dan tambahkan seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139534673-82a35121-0dee-497a-8950-61bc68648667.JPG" width="600">

Buat *file* **.htaccess** pada folder **/var/www/general.mecha.franky.a08.com** dan tambahkan seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139534798-e1dae63d-9bed-43f0-b96e-11ce348bb9ba.JPG" width="350">

*Restart* **apache2**.
```
service apache2 restart
```

### Loguetown atau Alabasta

Lakukan *testing* pada `Loguetown` dan `Alabasta` untuk cek apakah autentikasi berhasil dibuat.
```
lynx general.mecha.franky.a08.com:15000 atau general.mecha.franky.a08.com:15500
lynx www.general.mecha.franky.a08.com:15000 atau www.general.mecha.franky.a08.com:15500
```

Jika sukses, maka akan memunculkan hasil seperti berikut.

<img src="https://user-images.githubusercontent.com/37539546/139535026-36a9c9aa-5876-42a4-8f9a-ebc363c00500.JPG" width="600">

<img src="https://user-images.githubusercontent.com/37539546/139535030-1ed55f47-fb0a-40cf-b3d1-66813596785f.JPG" width="600">

<img src="https://user-images.githubusercontent.com/37539546/139534298-175a5cc6-ef59-4546-9c1a-cf450331147b.JPG" width="600">

Di gambar terlihat diminta *username* dan *password* untuk proses autentikasi.

## Soal 16

### Dan setiap kali mengakses IP Skypiea akan dialihkan secara otomatis ke **www.franky.yyy.com**.

### Jawaban:

### Skypiea

Buka *file* **000-default.conf** dan tambahkan seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139535316-ed5e9d9e-0918-4f90-994c-3053aca2b099.JPG" width="600">

Buat *file* **.htaccess** pada folder **/var/www/html** dan tambahkan seperti konfigurasi berikut untuk *redirect* menggunakan **RewriteEngine**.

<img src="https://user-images.githubusercontent.com/37539546/139535381-0d542410-62eb-402e-836e-e7647850e01f.JPG" width="350">

*Restart* **apache2**.
```
service apache2 restart
```

### Loguetown atau Alabasta

Lakukan *testing* pada `Loguetown` dan `Alabasta` untuk cek apakah *redirect* berhasil.
```
lynx 10.3.2.4
```

Jika sukses, maka akan memunculkan hasil seperti berikut.

<img src="https://user-images.githubusercontent.com/37539546/139536223-0a358f30-94cc-4dc1-a6fb-bc86c229b0d0.jpg" width="600">

<img src="https://user-images.githubusercontent.com/37539546/139532643-22796155-cfc2-4983-a7bc-53818e75ea2b.JPG" width="600">

**RewriteCond** melakukan filter pada *request* dengan IP **10.3.2.4** (IP Skypiea), setelah itu **RewriteRule** akan menerima segala *request* dengan IP Skypiea dengan folder atau *path* apapun dan segera melakukan *redirect* 301 ke [**www.franky.a08.com**](www.franky.a08.com).

## Soal 17

### Dikarenakan Franky juga ingin mengajak temannya untuk dapat menghubunginya melalui website **www.super.franky.yyy.com** dan karena pengunjung Web Server pasti akan bingung dengan random-nya image yang ada, maka Franky juga meminta untuk mengganti request gambar yang memiliki substring "franky" dan akan diarahkan menuju `franky.png`. Bantulah Luffy untuk membuat konfigurasi DNS dan Web Server ini!

### Jawaban:

### Skypiea

Buka *file* **super.franky.a08.com.conf** dan tambahkan seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/139536428-74d85d3d-5263-4fc6-91d7-df66ed3f07e8.JPG" width="600">

Buat *file* **.htaccess** pada folder **/var/www/super.franky.a08.com** dan tambahkan seperti konfigurasi berikut.
```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(.*)franky(.*)$ http://www.super.franky.c08.com/public/images/franky.png [L,R]
```

*Restart* **apache2**.
```
service apache2 restart
```

### Loguetown atau Alabasta

Lakukan *testing* pada `Loguetown` dan `Alabasta` untuk cek apakah *request* gambar berhasil.
```
lynx super.franky.a08.com/public/images/frankyrobot.png
lynx www.super.franky.a08.com/public/images/frankyrobot.png
```

Jika sukses, maka akan memunculkan hasil seperti berikut.

<img src="https://user-images.githubusercontent.com/37539546/139536846-46eb001d-65f8-44c2-a15a-abbad3bbf9ee.jpg" width="600">

<img src="https://user-images.githubusercontent.com/37539546/139536865-5ff21508-58f5-4693-ac4e-3fae18989bcd.jpg" width="600">

Muncul perintah untuk mengunduh gambar.

<img src="https://user-images.githubusercontent.com/37539546/139536877-d7279025-f026-4608-a1f1-5e93a5a20746.JPG" width="600">

<img src="https://user-images.githubusercontent.com/37539546/139536881-1c34d487-85cf-4000-80f2-6e4dff3c05f7.JPG" width="600">

## Notes
1.  **yyy** pada URL adalah kode kelompok.
2.  *File requirement* [Github](https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom).

## Kendala
1. Sedikit kesulitan no. 9 karena memakai Module Rewrite
2. Server sempet kereset dan harus mengerjakan ulang dari no. 8
