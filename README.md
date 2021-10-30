
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

Agar *node*-*node* lainnya dapat mengakses internet, jalankan *command* berikut dan gunakan IP DNS dari `Foosha` tadi.
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
```

*Restart* semua *node* kembali. Lalu, *testing* semua *node* apakah sudah terkoneksi dengan internet dengan `ping` ke [**google.com**](google.com). Sebagai contoh pada `Loguetown`:

<img src="https://user-images.githubusercontent.com/37539546/139522122-43ddb61d-0b3a-484f-a4a5-433109dd6529.JPG" width="600">

## Soal 2

### Luffy ingin menghubungi Franky yang berada di EniesLobby dengan denden mushi. Kalian diminta Luffy untuk membuat website utama dengan mengakses [**franky.yyy.com**](franky.yyy.com) dengan alias **www.franky.yyy.com** pada folder `kaizoku`.

### Jawaban:

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
zone "franky.A08.com" {
    type master;
    file "/etc/bind/kaizoku/franky.B06.com";
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

<img src="https://user-images.githubusercontent.com/37539546/139522763-ee789d2f-0623-4ff5-a582-31833dfe5e83.JPG" width="600">

Dalam konfigurasi ini sudah ditambahkan *record* **CNAME** [**www.franky.a08.com**](www.franky.a08.com) untuk membuat alias yang mengarahkan domain ke alamat/domain yang lain.

*Restart* **bind9**.
```
service bind9 restart
```

Lakukan *testing* dengan menambahkan `nameserver 10.3.2.2` pada `Loguetown` dan `Alabasta` untuk cek apakah [**franky.a08.com**](franky.a08.com) atau [**www.franky.a08.com**](www.franky.a08.com) dapat diakses. Jika sukses, maka akan memunculkan hasil seperti berikut.

<img src="https://user-images.githubusercontent.com/37539546/139522919-ded4c8b1-6574-4f97-a8d5-53c86dace204.JPG" width="600">

<img src="https://user-images.githubusercontent.com/37539546/139522948-93560bd1-891c-42e7-ac4c-d26980132d40.JPG" width="600">

## Soal 3

### Setelah itu buat subdomain [**super.franky.yyy.com**](super.franky.yyy.com) dengan alias **www.super.franky.yyy.com** yang diatur DNS-nya di EniesLobby dan mengarah ke Skypiea.

### Jawaban:

## Soal 4

### Buat juga reverse domain untuk domain utama.

### Jawaban:

## Soal 5

### Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama.

### Jawaban:

## Soal 6

### Setelah itu, terdapat subdomain [**mecha.franky.yyy.com**](mecha.franky.yyy.com) dengan alias **www.mecha.franky.yyy.com** yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypiea dalam folder `sunnygo`.

### Jawaban:

## Soal 7

### Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama [**general.mecha.franky.yyy.com**](general.mecha.franky.yyy.com) dengan alias **www.general.mecha.franky.yyy.com** yang mengarah ke Skypiea.

### Jawaban:

## Soal 8

### Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Web Server. Pertama dengan Web Server **www.franky.yyy.com**. Pertama, Luffy membutuhkan Web Server dengan DocumentRoot pada `/var/www/franky.yyy.com`.

### Jawaban:

## Soal 9

### Setelah itu, Luffy juga membutuhkan agar URL **www.franky.yyy.com/index.php/home** dapat menjadi menjadi **www.franky.yyy.com/home**.

### Jawaban:

## Soal 10

### Setelah itu, pada subdomain **www.super.franky.yyy.com**, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada `/var/www/super.franky.yyy.com`.

### Jawaban:

## Soal 11

### Akan tetapi, pada folder `/public`, Luffy ingin hanya dapat melakukan directory listing saja.

### Jawaban:

## Soal 12

### Tidak hanya itu, Luffy juga menyiapkan error file `404.html` pada folder `/error` untuk mengganti error kode pada apache.

### Jawaban:

## Soal 13

### Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset **www.super.franky.yyy.com/public/js** menjadi **www.super.franky.yyy.com/js**.

### Jawaban:

## Soal 14

### Dan Luffy meminta untuk web **www.general.mecha.franky.yyy.com** hanya bisa diakses dengan port 15000 dan port 15500.

### Jawaban:

## Soal 15

### Dengan autentikasi username `luffy` dan password `onepiece` dan file di `/var/www/general.mecha.franky.yyy`.

### Jawaban:

## Soal 16

### Dan setiap kali mengakses IP Skypiea akan dialihkan secara otomatis ke **www.franky.yyy.com**.

### Jawaban:

## Soal 17

### Dikarenakan Franky juga ingin mengajak temannya untuk dapat menghubunginya melalui website **www.super.franky.yyy.com** dan karena pengunjung Web Server pasti akan bingung dengan random-nya image yang ada, maka Franky juga meminta untuk mengganti request gambar yang memiliki substring "franky" dan akan diarahkan menuju `franky.png`. Bantulah Luffy untuk membuat konfigurasi DNS dan Web Server ini!

### Jawaban:

## NOTES
1.  `yyy` pada URL adalah kode kelompok anda.
2.  File Requirement [Github](https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom).

## Kendala
