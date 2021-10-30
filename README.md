# Jarkom-Modul-2-T13-2021

## Anggota Kelompok

| No. | Nama | NRP |
| ------ | ------ | ------ |
| 1. | Heaven Happyna Putra F. | (05311940000026) |
| 2. | Gavin Bagus Kenzie Narain | (05311940000028) |
| 3. | Tera Nurwahyu Pratama | (05311940000039) |

## Daftar Isi
* [Soal 1](#soal-1)
* [Soal 2](#soal-2)
* [Soal 3](#soal-3)
* [Soal 4](#soal-4)
* [Soal 5](#soal-5)
* [Soal 6](#soal-6)
* [Soal 7](#soal-7)
* [Soal 8](#soal-8)
* [Soal 9](#soal-9)
* [Soal 10](#soal-10)
* [Soal 11](#soal-11)
* [Soal 12](#soal-12)
* [Soal 13](#soal-13)
* [Soal 14](#soal-14)
* [Soal 15](#soal-15)
* [Soal 16](#soal-16)
* [Soal 17](#soal-17)

## Soal 1
Luffy adalah seorang yang akan jadi Raja Bajak Laut. Demi membuat Luffy menjadi Raja Bajak Laut, Nami ingin membuat sebuah peta, bantu Nami untuk membuat peta berikut:

![image](https://user-images.githubusercontent.com/73151823/139530926-59de427a-47c8-420d-80c3-ca8934b17d65.png)

EniesLobby akan dijadikan sebagai DNS Master, Water7 akan dijadikan DNS Slave, dan Skypie akan digunakan sebagai Web Server. Terdapat 2 Client yaitu Loguetown, dan Alabasta. Semua node terhubung pada router Foosha, sehingga dapat mengakses internet.
### Penyelesaian
Pada no 1, kami buat topologi sesuai contoh:
<br>
![image](https://user-images.githubusercontent.com/73151823/139530988-894cbdc2-c804-4884-876f-218902626496.png)
<br>
Untuk router Foosha, konfigurasi nodenya seperti berikut:
<br>
``` 
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.48.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.48.2.1
	netmask 255.255.255.0
```
<br>
Untuk konfigurasi node lainnya seperti berikut:
<br>
<br>
Loguetown
<br>

```
auto eth0
iface eth0 inet static
	address 10.48.1.2
	netmask 255.255.255.0
	gateway 10.48.1.1
  
 ```
 
 Alabasta
<br>
```
auto eth0
iface eth0 inet static
	address 10.48.1.3
	netmask 255.255.255.0
	gateway 10.48.1.1
 ```
 <br>
 
 <br>
 Karena Loguetown dan Alabasta digunakan sebagai client, install dnsutils. Setelah itu ganti nameservernya.
 <br>
 
 ```
apt-get update
apt-get install nano -y
apt-get install dnsutils
echo nameserver 10.48.2.2 > /etc/resolv.conf
echo nameserver 10.48.2.3 >> /etc/resolv.conf
 ```
 <br>
 
 EniesLobby

```
auto eth0
iface eth0 inet static
	address 10.48.2.2
	netmask 255.255.255.0
	gateway 10.48.2.1
 ```
 
<br>

<br>
EniesLobby sebagai DNS Master

```
apt-get update
apt-get install bind9 -y
apt-get install nano -y
```

<br>

Water7

```
auto eth0
iface eth0 inet static
	address 10.48.2.3
	netmask 255.255.255.0
	gateway 10.48.2.1
 ```
 <br>
 
 <br>
Water7 sebagai DNS Slave

```
apt-get update
apt-get install bind9 -y
apt-get install nano -y
```
 
 Skypie
 
```
auto eth0
iface eth0 inet static
	address 10.48.2.4
	netmask 255.255.255.0
	gateway 10.48.2.1
 ```
 <br>
 
  <br>
Skypie sebagai webserver

```
apt-get update
apt-get install bind9 -y
apt-get install nano -y
apt-get install apache2
apt-get install php
apt-get install libapache2-mod-php7.0
```
 
 Kemudian, mengetikkan ```iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.48.0.0/16``` pada Foosha
 <br>
 Agar setiap node bisa mengakses internet, masukkan ```nameserver 192.168.122.1``` pada ```/etc/resolv/conf```
 
 


## Soal 2
Luffy ingin menghubungi Franky yang berada di EniesLobby dengan denden mushi. Kalian diminta Luffy untuk membuat website utama dengan mengakses franky.yyy.com dengan alias www.franky.yyy.com pada folder kaizoku.
### Penyelesaian
#### EniesLobby
gunakan ```nano /etc/bind/named.conf.local``` untuk menambah franky.t13.com pada file konfigurasi
```
zone "franky.t13.com" {
        type master;
        file "/etc/bind/kaizoku/franky.t13.com";
};
```
buat folder baru dengan ```mkdir /etc/bind/kaizoku``` lalu tambahkan file konfigurasi
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     franky.t13.com. root.franky.t13.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      franky.t13.com.
@       IN      A       10.48.2.2
```
restart bind9 ```service bind9 restart```

#### Alabasta/Loguetown
Jangan lupa jalankan install2 pada soal no 1. Jika sudah bisa langsung testing

```ping franky.t13.com```

```ping www.franky.t13.com```

```host -t CNAME www.franky.t13.com```

![image](https://user-images.githubusercontent.com/73151823/139534369-a08e723e-af72-4ff5-b7cb-6f6da561bb0d.png)



## Soal 3
Setelah itu buat subdomain super.franky.yyy.com dengan alias www.super.franky.yyy.com yang diatur DNS nya di EniesLobby dan mengarah ke Skypie.
### Penyelesaian
#### EniesLobby
menambah 3 line baru pada ```/etc/bind/kaizoku/franky.t13.com```
```
www     IN      CNAME   franky.t13.com.
super   IN      A       10.48.2.4
www.super     IN      CNAME   super.franky.t13.com.
```
#### Alabasta/Loguetown
Jika sudah bisa langsung testing

```ping super.franky.t13.com```

```ping www.super.franky.t13.com```

```host -t CNAME www.super.franky.t13.com```



## Soal 4
Buat juga reverse domain untuk domain utama.
### Penyelesaian
#### EniesLobby
tambahkan zone baru  pada ```/etc/bind/named.conf.local```
```
zone "2.48.10.in-addr.arpa" {
    type master;
    file "/etc/bind/kaizoku/2.48.10.in-addr.arpa";
};
```
kemudian buat file konfigurasinya di ```/etc/bind/kaizoku/2.48.10.in-addr.arpa```
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     franky.t13.com. root.franky.t13.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
2.48.10.in-addr.arpa.   IN      NS      franky.t13.com.
2                       IN      PTR     franky.t13.com.
```
#### Alabasta/Loguetown
jika sudah, bisa ditest dengan ```host -t PTR 10.48.2.2```



## Soal 5
Supaya tetap bisa menghubungi Franky jika server EniesLobby rusak, maka buat Water7 sebagai DNS Slave untuk domain utama.
### Penyelesaian
#### EniesLobby
tambahkan 3 line baru pada ```/etc/bind/named.conf.local``` untuk mengkonfigurasi DNS Slave ke Water7
```
notify yes;
        also-notify { 10.48.2.3; };
        allow-transfer { 10.48.2.3; };
```
```
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";
zone "franky.t13.com" {
        type master;
        notify yes;
        also-notify { 10.48.2.3; };
        allow-transfer { 10.48.2.3; };
        file "/etc/bind/kaizoku/franky.t13.com";
};
zone "2.48.10.in-addr.arpa" {
    type master;
    file "/etc/bind/kaizoku/2.48.10.in-addr.arpa";
};
```
kemudian untuk mengetes DNS slave, matikan bind9 pada EniesLobby dengan ```service bind9 stop```


#### Water7
Pastikan sudah menginstall bind9 seperti yang sudah dijelaskan di no 1. 
<br>
Tambahkan zone franky.t13.com pada ```/etc/bind/named.conf.local``` dengan masters mengarah ke IP EniesLobby
```
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";

zone "franky.t13.com" {
    type slave;
    masters { 10.48.2.2; }; // Masukan IP EniesLobby tanpa tanda petik
    file "/var/lib/bind/franky.t13.com";
};
```
Kemudian restart bind9 
```service bind9 restart```
<br>

#### Alabasta/Loguetown
Jika sudah menyalakan service bind9 pada Water7 dan mematikan bind9 pada EniesLobby, lakukan ping pada server Alabasta/Loguetown



## Soal 6
Setelah itu terdapat subdomain mecha.franky.yyy.com dengan alias www.mecha.franky.yyy.com yang didelegasikan dari EniesLobby ke Water7 dengan IP menuju ke Skypie dalam folder sunnygo.
### Penyelesaian
#### EniesLobby
tambahkan 2 line pada ```/etc/bind/kaizoku/franky.t13.com```
```
ns1     IN      A       10.48.2.3
mecha   IN      NS      ns1
```
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     franky.t13.com. root.franky.t13.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      franky.t13.com.
@       IN      A       10.48.2.2
@       IN      AAAA    ::1
www     IN      CNAME   franky.t13.com.
super   IN      A       10.48.2.4
www.super     IN      CNAME   super.franky.t13.com.
ns1     IN      A       10.48.2.3
mecha   IN      NS      ns1
```
Kemudian pada ```/etc/bind/named.conf.options```, 
comment line 
```dnssec-validation auto;```
dan tambahkan line
```allow-query{any;};```
<br>
Kemudian restart bind9 ```service bind9 restart```

#### Water7
Lakukan hal yang sama pada ```/etc/bind/named.conf.options```, 
comment line 
```dnssec-validation auto;```
dan tambahkan line
```allow-query{any;};```

kemudian tambahkan zone untuk delegasi subdomain pada ```/etc/bind/named.conf.local```
```
zone "mecha.franky.t13.com" {
    type master;
    file "/etc/bind/sunnygo/mecha.franky.t13.com";
};
```
```
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";

zone "franky.t13.com" {
    type slave;
    masters { 10.48.2.2; }; // Masukan IP EniesLobby tanpa tanda petik
    file "/var/lib/bind/franky.t13.com";
};

zone "mecha.franky.t13.com" {
    type master;
    file "/etc/bind/sunnygo/mecha.franky.t13.com";
};
```
karena mintanya folder sunnygo, buat folder baru dengan ```mkdir /etc/bind/sunnygo```
<br>
kemudian tambahkan file konfigurasi pada ```/etc/bind/sunnygo/mecha.franky.t13.com```
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     mecha.franky.t13.com. root.mecha.franky.t13.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      mecha.franky.t13.com.
@       IN      A       10.48.2.4
www     IN      CNAME   mecha.franky.t13.com.
```
Kemudian restart bind9 ```service bind9 restart```

#### Alabasta/Loguetown
jika sudah, lakukan test dengan ```ping www.mecha.franky.t13.com```, ```ping mecha.franky.t13.com```, dan ```host -t CNAME www.mecha.franky.t13.com```


## Soal 7
Untuk memperlancar komunikasi Luffy dan rekannya, dibuatkan subdomain melalui Water7 dengan nama general.mecha.franky.yyy.com dengan alias www.general.mecha.franky.yyy.com yang mengarah ke Skypie.
### Penyelesaian
#### Water7
tambahkan 2 line untuk konfigurasi subdomain general pada ```/etc/bind/sunnygo/mecha.franky.t13.com```
```
general IN      A       10.48.2.4
www.general     IN      CNAME   general.mecha.franky.t13.com.
```
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     mecha.franky.t13.com. root.mecha.franky.t13.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      mecha.franky.t13.com.
@       IN      A       10.48.2.4
www     IN      CNAME   mecha.franky.t13.com.
general IN      A       10.48.2.4
www.general     IN      CNAME   general.mecha.franky.t13.com.
```
Kemudian restart bind9 ```service bind9 restart```
#### Alabasta/Loguetown
jika sudah, lakukan test dengan ```ping www.general.mecha.franky.t13.com```, ```ping general.mecha.franky.t13.com```, dan ```host -t CNAME www.general.mecha.franky.t13.com```


## Soal 8
Setelah melakukan konfigurasi server, maka dilakukan konfigurasi Webserver. Pertama dengan webserver www.franky.yyy.com. Pertama, luffy membutuhkan webserver dengan DocumentRoot pada /var/www/franky.yyy.com.
### Penyelesaian
Sebelumnya pastikan install2 pada nomor 1 sudah dijalankan
#### EniesLobby
Record A dan PTR pada ```/etc/bind/kaizoku/franky.t13.com``` sudah harus mengarah ke IP Skypie (10.48.2.4)
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     franky.t13.com. root.franky.t13.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      franky.t13.com.
@       IN      A       10.48.2.4
@       IN      AAAA    ::1
www     IN      CNAME   franky.t13.com.
super   IN      A       10.48.2.4
www.super     IN      CNAME   super.franky.t13.com.
ns1     IN      A       10.48.2.3
mecha   IN      NS      ns1
```
#### Skypie
Buat file konfigurasi baru pada ```/etc/apache2/sites-available/franky.t13.com.conf```. Atur DocumentRoot sesuai permintaan soal.
```
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/franky.t13.com
        ServerName franky.t13.com
        ServerAlias www.franky.t13.com

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```
Enable konfigurasinya dengan ```a2ensite franky.t13.com```, lalu restart apache ```service apache2 restart```
<br>
Kemudian, karena file yang ditentukan berada di github, install dan konfigurasi github dahulu agar bisa melakukan git clone. Setelah itu, unzip dan pindah ke /var/www. Jangan lupa tambahkan .com pada akhir nama foldernya.
```
apt-get install unzip -y
apt-get install git -y
git config --global http.sslVerify false
git clone https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom.git
unzip /root/Praktikum-Modul-2-Jarkom/franky.zip
unzip /root/Praktikum-Modul-2-Jarkom/general.mecha.franky.zip
unzip /root/Praktikum-Modul-2-Jarkom/super.franky.zip
cp -r /root/franky /var/www
cp -r /root/general.mecha.franky /var/www
cp -r /root/super.franky /var/www
mv /var/www/franky /var/www/franky.t13.com
mv /var/www/general.mecha.franky /var/www/general.mecha.franky.t13.com
mv /var/www/super.franky /var/www/super.franky.t13.com
```

#### Alabasta/Loguetown
lakukan test dengan ```lynx franky.t13.com``` dan ```www.franky.t13.com```. hasilnya akan menampilkan halaman yang sama.


## Soal 9
Setelah itu, Luffy juga membutuhkan agar url www.franky.yyy.com/index.php/home dapat menjadi menjadi www.franky.yyy.com/home. 
### Penyelesaian
#### Skypie
Tambahkan konfigurasi pada ```/etc/apache2/sites-available/franky.t13.com.conf```
```
<Directory /var/www/franky.t13.com>
                Options +FollowSymLinks -Multiviews
                AllowOverride All
        </Directory>
```
```AllowOverride All``` ditambahkan agar konfigurasi .htaccess dapat berjalan.
<br>
```+FollowSymLinks``` ditambahkan agar konfigurasi mod_rewrite dapat berjalan.
<br>
```-Multiviews``` ditambahkan agar konfigurasi mod_negotiation tidak dapat berjalan. mod_negotiation bisa 'rewrite' requests sehingga menimpa dan mengganggu mod_rewrite.
<br>
Tambahkan file .htaccess pada ```/var/www/franky.t13.com/.htaccess``` kemudian isi dengan rewrite rule:
```
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule (.*) /index.php/$1 [L]
```
Pada rewrite condition, request akan dicek apakah requestnya akan ke fila (-f) atau direktori (-d), jika kondisinya terpenuhi maka RewriteRule akan dijalankan. Rule ini akan meredirect /home ke /index.php/$1 ,  di mana $1 adalah home.

#### Alabasta/Loguetown
Jika sudah, bisa test menggunakan ```lynx www.franky.t13.com/home```. Jika benar akan diperlihatkan isi file ```/var/www/franky.t13.com/home.html```


## Soal 10
Setelah itu, pada subdomain www.super.franky.yyy.com, Luffy membutuhkan penyimpanan aset yang memiliki DocumentRoot pada /var/www/super.franky.yyy.com.
### Penyelesaian
#### Skypie
Buat file konfigurasi baru ```/etc/apache2/sites-available/super.franky.t13.com.conf``` . Atur DocumentRoot sesuai dengan permintaan soal. Untuk direktori document root tadi sudah kita buat saat unzip melalui github pada soal 8
```
<VirtualHost *:80>
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/super.franky.t13.com
        ServerName super.franky.t13.com
        ServerAlias www.super.franky.t13.com
	
	<Directory /var/www/super.franky.t13.com>
                Options +FollowSymLinks -Multiviews
		AllowOverride All
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```
Jangan lupa enable konfigurasi dengan ```a2ensite super.franky.t13.com``` dan restart apache ```service apache2 restart```

#### Alabasta/Loguetown
Jika sudah, lakukan test dengan ```lynx www.super.franky.t13.com```


## Soal 11
Akan tetapi, pada folder /public, Luffy ingin hanya dapat melakukan directory listing saja.
### Penyelesaian
#### Skypie
Tambah konfigurasi pada ```/etc/apache2/sites-available/super.franky.t13.com.conf```. ```+Indexes``` digunakan agar dapat melakukan directory listing pada folder /public
```
	<Directory /var/www/super.franky.t13.com/public>
                Options +Indexes
        </Directory>
```
restart apache ```service apache2 restart```

#### Alabasta/Loguetown
Jika sudah, lakukan test dengan ```lynx www.super.franky.t13.com/public```. Jika benar akan menampilkan directory listing pada folder /public


## Soal 12
Tidak hanya itu, Luffy juga menyiapkan error file 404.html pada folder /error untuk mengganti error kode pada apache.
### Penyelesaian
#### Skypie
buat file .htaccess pada ```/var/www/super.franky.t13.com/.htaccess``` dan tambahkan:
```
ErrorDocument 404 /error/404.html
```
Line tersebut mengarahkan error 404 ke file /error/404.html .  Untuk filenya sudah disiapkan dari github
<br>
Agar file .htaccess berjalan, tambahkan opsi berikut pada ```/etc/apache2/sites-available/super.franky.t13.com.conf```:
```
	<Directory /var/www/super.franky.t13.com>
                Options +FollowSymLinks -Multiviews
		AllowOverride All
        </Directory>
```
restart apache ```service apache2 restart```

#### Alabasta/Loguetown
Lakukan testing dengan sengaja mengakses link sudah pasti mengarah ke 404, misalnya ```lynx www.super.franky.t13.com/publics```. Jika benar akan muncul pesan seperti ini


## Soal 13
Luffy juga meminta Nami untuk dibuatkan konfigurasi virtual host. Virtual host ini bertujuan untuk dapat mengakses file asset www.super.franky.yyy.com/public/js menjadi www.super.franky.yyy.com/js.
### Penyelesaian
#### Skypie
Tambah konfigurasi  alias pada ```/etc/apache2/sites-available/super.franky.t13.com.conf```
```
Alias "/js" "/var/www/super.franky.t13.com/public/js"
```
Jangan lupa restart apache ```service apache2 restart```

#### Alabasta/Loguetown
Gunakan ```lynx www.super.franky.t13.com/js```. Jika berhasil maka akan terlihat seperti ini


## Soal 14
Dan Luffy meminta untuk web www.general.mecha.franky.yyy.com hanya bisa diakses dengan port 15000 dan port 15500.
### Penyelesaian
#### SKypie
Buat file konfigurasi baru ```/etc/apache2/sites-available/general.mecha.franky.t13.com.conf``` . Ganti virtual hostnya dengan 15000 dan 15500. Atur DocumentRoot sesuai dengan permintaan soal. Untuk direktori document root tadi sudah kita buat saat unzip melalui github pada soal 8
```
<VirtualHost *:15000> 
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/general.mecha.franky.t13.com
        ServerName general.mecha.franky.t13.com
        ServerAlias www.general.mecha.franky.t13.com

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

<VirtualHost *:15500>  
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/general.mecha.franky.t13.com
        ServerName general.mecha.franky.t13.com
        ServerAlias www.general.mecha.franky.t13.com

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```
enable konfigurasi dengan ```a2ensite general.mecha.franky.t13.com```.
<br>
karena menggunakan port 15000 dan 15500, atur ```/etc/apache2/ports.conf``` agar melakukan listen pada port tersebut
```
# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

Listen 80
Listen 15000
Listen 15500
<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
``` 
<br>
restart apache 

```service apache2 restart```

#### Alabasta/Loguetown
Lakukan test menggunakan ```lynx www.general.mecha.franky.t13.com:15000``` dan ```lynx www.general.mecha.franky.t13.com:15500```. Jika benar, maka akan keluar hasil yang sama. 



## Soal 15
dengan autentikasi username luffy dan password onepiece dan file di /var/www/general.mecha.franky.yyy
### Penyelesaian
#### Skypie
Tambah konfigurasi untuk auth pada ```/etc/apache2/sites-available/general.mecha.franky.t13.com.conf```
```
	<Directory "/var/www/general.mecha.franky.t13.com">
                AuthType Basic
                AuthName "Restricted Content"
                AuthUserFile /etc/apache2/.htpasswd
                Require valid-user
        </Directory>
```
Kemudian set kredensial dengan ```htpasswd -c /etc/apache2/.htpasswd luffy -b onepiece```
<br>
restart apache 

```service apache2 restart```

#### Alabasta/Loguetown
Lakukan test menggunakan ```lynx www.general.mecha.franky.t13.com:15000``` dan ```lynx www.general.mecha.franky.t13.com:15500```. Jika benar, nanti akan disuruh memasukkan username dan password sebelum diarahkan ke web



## Soal 16
Dan setiap kali mengakses IP Skypie akan dialihkan secara otomatis ke www.franky.yyy.com.
### Penyelesaian

## Soal 17
Dikarenakan Franky juga ingin mengajak temannya untuk dapat menghubunginya melalui website www.super.franky.yyy.com, dan dikarenakan pengunjung web server pasti akan bingung dengan randomnya images yang ada, maka Franky juga meminta untuk mengganti request gambar yang memiliki substring “franky” akan diarahkan menuju franky.png.
### Penyelesaian

## Kendala yang Dialami
1. Script nya sempat eror
