# Jarkom-Modul-3-B03-2021

Nama Anggota :
1. Dewangga Dharmawan (05111940000029)
2. Ahmad Syafiq Aqil Wafi (05111940000089)

## Nomor 1

Membuat peta topologi dengan kriteria :
- EniesLobby menjadi DNS Server
- Jipangu menjadi DHCP Server
- Water7 menjadi Proxy Server

![image](https://user-images.githubusercontent.com/73766205/140901778-d0400777-b891-488c-b9f8-8f84b9286936.png)

1. EniesLobby menjadi DNS Server
Di dalam EniesLobby, setelah dihubungkan dengan internet melewati Foosha, install bind9 dengan command :
    
```
apt-get update
apt-get install bind9 -y
```

2. Jipangu menjadi DHCP Server

2.1. Di dalam Jipangu, install isc-dhcp-server :

```
apt-get update
apt-get install isc-dhcp-server
```

2.2 Lalu, mengecek versinya
   
![image](https://user-images.githubusercontent.com/73766205/140765985-f0d76994-af79-4b11-ac0b-d1f4ac3ff1cf.png)

2.3 Edit file /etc/default/isc-dhcp-server dengan menambahkan eth0, kabel menuju switch 2 dan Foosha, di INTERFACES
   
![image](https://user-images.githubusercontent.com/73766205/140766402-4440438e-0daa-4431-973a-805de166d9d6.png)
   
3. Water7 menjadi Proxy Server
    
3.1 Di dalam Water7, install squid :

```
apt-get update
apt-get install squid
```

3.2 Cek versi squidnya :
   
```
service squid status
```
    
![image](https://user-images.githubusercontent.com/73766205/140767397-78aacdca-d46d-40ce-ab26-553214eb3944.png)

## Nomor 2

Menjadikan Foosha sebagai DHCP Relay

1. Di dalam Foosha, install DHCP Relay, lalu jika diminta untuk memasukkan apapun, seperti mendengarkan ke server mana dsb., tekan enter tiga kali

![image](https://user-images.githubusercontent.com/73766205/140902330-692e7467-bee6-4f14-b32f-52978e64c5a7.png)

2. Edit file /etc/default/isc-dhcp-relay dengan memasukkan tujuan relay DHCP ke alamat IP Jipangu (192.178.2.4) :

![image](https://user-images.githubusercontent.com/73766205/140768604-29452f1c-8940-4339-85d2-06cbb177e63d.png)

## Nomor 3

Client yang melewati switch 1, Loguetown dan Alabasta, memiliki range IP 192.178.1.20 - 192.178.1.99 dan IP 192.178.1.150 - 192.178.1.169

Di dalam Jipangu, edit file /etc/dhcp/dhcpd.conf dengan menambahkan kodingan ini :

![image](https://user-images.githubusercontent.com/73766205/140903186-6be51a08-eb53-46cf-b42b-51e12c56d63d.png)

## Nomor 4

Client yang melewati switch 3, Skypie dan TottoLand, memiliki range IP 192.178.3.30 - 192.178.3.50

Masih di Jipangu, edit file /etc/dhcp/dhcpd.conf dengan menambahkan kodingan ini :

![image](https://user-images.githubusercontent.com/73766205/140903621-812c0156-1e68-4d92-996a-800dd3bc0a17.png)

Setelah switch 1 dan 3 memiliki subnet, ditambahkan subnet untuk switch 2 dengan IP 192.178.2.100 - 192.178.2.150 tanpa alamat broadcast dan waktu lease

![image](https://user-images.githubusercontent.com/73766205/140903931-fb8f0203-2cef-4e9f-be87-613114ff986d.png)

Lalu, restart DHCP Server di Jipangu dengan command :

```
service isc-dhcp-server restart
```

## Nomor 5

Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut.

1. Edit konfigurasi di Loguetown dan Alabasta sehingga hanya ada kodingan seperti ini :

```
auto eth0
iface eth0 inet dhcp
```

2. Restart Loguetown dan Alabasta dengan menghentikan masing-masing dari kedua server lalu menjalankannya lagi.

3. Cek IP-IP-nya menggunakan `ip a`

Loguetown :

![image](https://user-images.githubusercontent.com/73766205/140905925-40faa6ca-d8c3-46cd-9bae-e90210836272.png)

![image](https://user-images.githubusercontent.com/73766205/140906408-6a573995-e9c6-4ad7-8424-b6c77b98b457.png)

Alabasta :

![image](https://user-images.githubusercontent.com/73766205/140906148-098db451-c3dd-44dd-86d9-d376c76b0c95.png)

![image](https://user-images.githubusercontent.com/73766205/140906330-387d6088-5b66-461d-a327-0c3208aa0690.png)

## Nomor 6

Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit.

Di Jipangu, edit file /etc/dhcp/dhcpd.conf sehingga waktu masing-masing switch sesuai yang tertera

![image](https://user-images.githubusercontent.com/73766205/140903432-f82dc8e7-81b2-4e66-8a6b-6f2b077fdc37.png)

![image](https://user-images.githubusercontent.com/73766205/140766884-fc7afbe4-f6e3-4f69-96b0-31f10669c4ad.png)

## Nomor 7

Skypie dijadikan server dengan alamat IP tetap 192.178.3.69

1. Di Skypie7, command `ip a` untuk mencari hardware address

![image](https://user-images.githubusercontent.com/73766205/140906964-86c117ce-30cc-4dd7-b922-0005a2f19fde.png)

2. Lalu di Jipangu, edit file /etc/dhcp/dhcpd.conf dan ditambahkan kodingan berikut, sesuai dengan hardware address di Skypie7 dan IP 192.178.3.69 :

![image](https://user-images.githubusercontent.com/73766205/140767029-930b8281-42b2-434c-b70d-c3ff7836c1c8.png)

3. Lalu, restart DHCP Server di Jipangu dengan command :

```
service isc-dhcp-server restart
```

## Nomor 8

Pada Loguetown, proxy harus bisa diakses dengan nama jualbelikapal.yyy.com dengan port yang digunakan adalah 5000

### Penyelesaian

1. Pada Water7 install squid proxy menggunakan command berikut.

```
apt-get update
apt-get install squid -y
```

2. Kemudian modifikasi konfigurasi squid proxy pada `/etc/squid/squid.conf` menjadi seperti berikut.

```
http_port 5000
http_access allow all
visible_hostname jualbelikapal.b03.com
```

3. Pada konfigurasi diatas kita test allow seluruh target, kemudian kita settings proxy berjalan pada IP Water7 port 5000.

4. Agar proxy bisa berjalan pada domain jualbelikapal.b03.com pada proxy client, maka kita atur DNS pada EnniesLobby.

5. Pada EnniesLobby, buat file baru pada `/etc/bind/kaizoku/jualbelikapal.b03.com`. Pada file tersebut isi konfigurasi berikut.

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     jualbelikapal.b03.com. root.jualbelikapal.b03.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      jualbelikapal.b03.com.
@       IN      A       192.178.2.3
www     IN      CNAME   jualbelikapal.b03.com.
@       IN      AAAA    ::1
```

6. Kemudian modifikasi file `/etc/bind/named.conf.local` dengan menambahkan konfigurasi berikut.

```
zone "jualbelikapal.b03.com" {
        type master;
        file "/etc/bind/kaizoku/jualbelikapal.b03.com";
};
```

7. Setelah itu restart bind9 pada EnniesLobby.

```
service bind9 restart
```

8. Kembali pada Water7 lalu restart Squid proxy.

```
service squid restart
```

9. Pada file `/etc/resolv.conf` di Water7 modifikasi isinya menjadi berikut. Hal ini dilakukan karena untuk mengetahui DNS EnniesLobby maka Water7 perlu menggunakan nameserver dari EnniesLobby yaitu pada IP `192.178.2.2`. IP `192.168.122.1` digunakan untuk akses internet.

```
nameserver 192.178.2.2
nameserver 192.168.122.1
```

10. Harusnya Squid proxy pada Water7 bisa berjalan dengan baik. Untuk test kita gunakan Loguetown sebagai client proxy.

11. Pada Loguetown, install Lynx dengan command berikut.

```
apt-get update
apt-get install lynx -y
```

12. Kemudian run command berikut untuk memasang web proxy, agar Lynx dapat menggunakan proxy.

```
export http_proxy="http://jualbelikapal.b03.com:5000"
```

13. Dengan Lynx coba kunjungi `google.com` atau alamat lainnya.

```
lynx google.com
```

13. Sebagai bukti proxy berhasil adalah sebagai berikut. PS: bukti berikut didapat dari penambahan auth karena jika konfigurasi squid masih seperti pada langkah di atas, bukti bahwa proxy berhasil tidak bisa didapatkan.

![image](https://user-images.githubusercontent.com/16128257/140787852-d15a502a-aa6d-4d6f-be91-78846a22d50c.png)

## Nomor 9

Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu luffybelikapalyyy dengan password luffy_yyy dan zorobelikapalyyy dengan password zoro_yyy

### Penyelesaian

1. Pada Water7 install `apache2-utils` dengan command berikut.

```
apt-get update
apt-get install apache2-utils -y
```

2. `apache2-utils` di-install agar bisa menggunakan command `htpasswd`.

3. Buat auth proxy dengan menjalankan command berikut.

```
htpasswd -c -m -b /etc/squid/passwd luffybelikapalb03 luffy_b03
htpasswd -m -b /etc/squid/passwd zorobelikapalb03 zoro_b03
```

4. Pada command diatas flag `-c` digunakan untuk membuat file `htpasswd`, pada command kedua tidak menggunakan flag `-c` karena bersifat overwrite. Flag `m` digunakan untuk menggunakan enkripsi MD5. Flag `-b` digunakan untuk menuliskan inline password karena tanpa flag `-b` password akan dituliskan secara interaktif melalu terminal.

5. Modifikasi file `/etc/squid/squid.conf` menjadi berikut ini.

```
http_port 5000
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy Authentication Required
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
visible_hostname jualbelikapal.b03.com
```

6. Kemudian restart squid proxy pada Water7.

```
service squid restart
```

7. Selanjutnya pindah ke Loguetown. Gunakan alamat proxy lalu kunjungi google.com dengan menggunakan Lynx.

```
export http_proxy="http://jualbelikapal.b03.com:5000"
lynx google.com
```

8. Maka untuk mengunjungi google.com perlu login menggunakan salah satu dari akun yang telah dibuat.

![image](https://user-images.githubusercontent.com/16128257/140790892-e1a8d8c1-2c28-4d2b-9723-c1f9960881df.png)

![image](https://user-images.githubusercontent.com/16128257/140790977-a048fc0f-1118-4754-af63-eba455d48056.png)

![image](https://user-images.githubusercontent.com/16128257/140790810-f95a248b-d495-4a5a-b6f0-d387fcd0c628.png)

## Nomor 10

Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum’at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00).

### Penyelesaian

1. Buat file baru pada `/etc/squid/acl.conf` lalu modifikasi isinya sebagai berikut ini.

```
acl AVAILABLE_WORKING_1 time MTWH 07:00-11:00
acl AVAILABLE_WORKING_2 time TWHF 17:00-24:00
acl AVAILABLE_WORKING_3 time WHFA 00:00-03:00
```

2. Buka konfigurasi squid proxy `/etc/squid/squid.conf` lalu modifikasi menjadi berikut ini.

```
include /etc/squid/acl.conf

http_port 5000

http_access deny !AVAILABLE_WORKING_1 !AVAILABLE_WORKING_2 !AVAILABLE_WORKING_3

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy Authentication Required
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS

visible_hostname jualbelikapal.b03.com
```

3. Kemudian restart squid proxy pada Water7.

```
service squid restart
```

4. Selanjutnya pindah ke Loguetown. Gunakan alamat proxy lalu kunjungi google.com dengan menggunakan Lynx.

```
export http_proxy="http://jualbelikapal.b03.com:5000"
lynx google.com
```

5. Maka untuk mengunjungi google.com perlu login menggunakan salah satu dari akun yang telah dibuat.

6. Berikut adalah tampilan pada Lynx jika akses internet tidak pada waktu yang ditentukan.

![image](https://user-images.githubusercontent.com/16128257/140797485-2c05bb88-cd3c-458a-aadf-a3598fffc776.png)

7. Berikut adalah tampilan pada Lynx jika akses internet pada waktu yang ditentukan.

![image](https://user-images.githubusercontent.com/16128257/140790892-e1a8d8c1-2c28-4d2b-9723-c1f9960881df.png)

![image](https://user-images.githubusercontent.com/16128257/140790977-a048fc0f-1118-4754-af63-eba455d48056.png)

![image](https://user-images.githubusercontent.com/16128257/140790810-f95a248b-d495-4a5a-b6f0-d387fcd0c628.png)

## Nomor 11

Agar transaksi bisa lebih fokus berjalan, maka dilakukan redirect website agar mudah mengingat website transaksi jual beli kapal. Setiap mengakses google.com, akan diredirect menuju super.franky.yyy.com dengan website yang sama pada soal shift modul 2. Web server super.franky.yyy.com berada pada node Skypie

### Penyelesaian

1. Membuat konfigurasi DNS pada EnniesLobby dengan cara membuat baru pada `/etc/bind/kaizoku/franky.b03.com`. Pada file tersebut isi konfigurasi berikut.

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     franky.b03.com. root.franky.b03.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      franky.b03.com.
@       IN      A       192.178.3.69
www     IN      CNAME   franky.b03.com.
super   IN      A       192.178.3.69
www.super       IN      CNAME super.franky.b03.com.
@       IN      AAAA    ::1
```

2. Kemudian modifikasi file `/etc/bind/named.conf.local` dengan menambahkan konfigurasi berikut.

```
zone "franky.b03.com" {
        type master;
        file "/etc/bind/kaizoku/franky.b03.com";
};
```

3. Setelah itu restart bind9 pada EnniesLobby.

```
service bind9 restart
```

4. Kemudian install web server pada Skypie dengan menjalankan command berikut.

```
echo "nameserver 192.168.122.1" > /etc/resolv.conf

apt-get update
apt-get install unzip -y
apt-get install wget -y
apt-get install apache2 -y
apt-get install php -y
apt-get install libapache2-mod-php7.0 -y

```

5. Download data zip webserver dari repository lalu unzip datanya dengan command berikut.

```
cd /var/www
wget https://raw.githubusercontent.com/FeinardSlim/Praktikum-Modul-2-Jarkom/main/super.franky.zip

unzip super.franky.zip
mv super.franky super.franky.b03.com
```

6. Buat file module baru pada `/etc/apache2/sites-available/super.franky.b03.com.conf` kemudian tambahkan / ubah menjadi konfigurasi berikut.

```
ServerName super.franky.b03.com
ServerAlias www.super.franky.b03.com

ServerAdmin webmaster@localhost
DocumentRoot /var/www/super.franky.b03.com
```

7. Aktifkan module `super.franky.b03.com` dan restart apache2 dengan command berikut.

```
a2ensite super.franky.b03.com
service apache2 restart
```

8. Pada Water 7, buat file baru pada `/etc/squid/restrict-sites.acl` lalu modifikasi isinya sebagai berikut.

```
google.com
```

9. Pada Water7, buka konfigurasi squid proxy `/etc/squid/squid.conf` lalu modifikasi menjadi berikut ini.

```
include /etc/squid/acl.conf
http_port 5000

http_access deny !AVAILABLE_WORKING_1 !AVAILABLE_WORKING_2 !AVAILABLE_WORKING_3

acl BLACKLISTS dstdomain "/etc/squid/restrict-sites.acl"
http_access deny BLACKLISTS
deny_info http://super.franky.b03.com BLACKLISTS

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy Authentication Required
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS

visible_hostname jualbelikapal.b03.com
```

10. Kemudian restart squid proxy pada Water7.

```
service squid restart
```

11. Selanjutnya pindah ke Loguetown. Gunakan alamat proxy lalu kunjungi google.com dengan menggunakan Lynx.

```
export http_proxy="http://jualbelikapal.b03.com:5000"
lynx google.com
```

12. Maka untuk mengunjungi google.com perlu login menggunakan salah satu dari akun yang telah dibuat.

13. Jika berhasil maka saat mengunjungi google.com proxy akan langsung melakukan redirect menuju super.franky.b03.com seperti tampilan Lynx berikut.

![image](https://user-images.githubusercontent.com/16128257/140800570-6b328dc3-2264-4d1c-81f2-bea73925edf8.png)

![image](https://user-images.githubusercontent.com/16128257/140790892-e1a8d8c1-2c28-4d2b-9723-c1f9960881df.png)

![image](https://user-images.githubusercontent.com/16128257/140790977-a048fc0f-1118-4754-af63-eba455d48056.png)

![image](https://user-images.githubusercontent.com/16128257/140800653-8d905ecc-3fec-4250-8f2f-313fa0537c94.png)

## Nomor 12

Saatnya berlayar! Luffy dan Zoro akhirnya memutuskan untuk berlayar untuk mencari harta karun di super.franky.yyy.com. Tugas pencarian dibagi menjadi dua misi, Luffy bertugas untuk mendapatkan gambar (.png, .jpg), sedangkan Zoro mendapatkan sisanya. Karena Luffy orangnya sangat teliti untuk mencari harta karun, ketika ia berhasil mendapatkan gambar, ia mendapatkan gambar dan melihatnya dengan kecepatan 10 kbps.

### Penyelesaian

1. Buka konfigurasi squid proxy `/etc/squid/squid.conf` lalu modifikasi menjadi berikut ini.

```
include /etc/squid/acl.conf
http_port 5000

http_access deny !AVAILABLE_WORKING_1 !AVAILABLE_WORKING_2 !AVAILABLE_WORKING$

acl BLACKLISTS dstdomain "/etc/squid/restrict-sites.acl"
http_access deny BLACKLISTS
deny_info http://super.franky.b03.com BLACKLISTS

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy Authentication Required
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED

# UNTUK USER LUFFY
acl USER_LUFFY proxy_auth luffybelikapalb03
acl IMAGE_PNG_REGEX urlpath_regex \.png$
acl IMAGE_JPG_REGEX urlpath_regex \.jpg$

delay_pools 1
delay_class 1 1
delay_parameters 1 8000/8000
delay_access 1 allow USER_LUFFY

http_access allow USER_LUFFY IMAGE_PNG_REGEX
http_access allow USER_LUFFY IMAGE_JPG_REGEX
http_access deny USERS USER_LUFFY

visible_hostname jualbelikapal.b03.com
```

2. Kemudian restart squid proxy pada Water7.

```
service squid restart
```

3. Gunakan wget untuk test download menggunakan user luffy dengan command berikut.

```
wget --proxy-user=luffybelikapalb03 --proxy-password=luffy_b03 super.franky.b03.com/public/images/franky.png

wget --proxy-user=luffybelikapalb03 --proxy-password=luffy_b03 super.franky.b03.com/public/images/car.jpg

wget --proxy-user=luffybelikapalb03 --proxy-password=luffy_b03 super.franky.b03.com/public/images/frankysupersecretfood.cursed
```

4. Untuk setiap command diatas, jika file yang di-download merupakan file `.png` atau `.jpg` maka proses download akan di-limit 10 kbps seperti berikut.

![image](https://user-images.githubusercontent.com/16128257/140803225-97cfb8ab-79bf-4c1f-840e-cd70650dabfd.png)

![image](https://user-images.githubusercontent.com/16128257/140803320-4149caaa-1a9c-494b-9ad4-0a0de7a5f3cf.png)

5. Jika file yang di-download bukan merupakan `.png` ataupun `.jpg` maka proses download akan ditolak.

![image](https://user-images.githubusercontent.com/16128257/140803414-9fe44376-d8eb-4921-9257-781618aff6e1.png)

## Nomor 13

Sedangkan, Zoro yang sangat bersemangat untuk mencari harta karun, sehingga kecepatan kapal Zoro tidak dibatasi ketika sudah mendapatkan harta yang diinginkannya.

### Penyelesaian

1. Buka konfigurasi squid proxy `/etc/squid/squid.conf` lalu modifikasi menjadi berikut ini.

```
include /etc/squid/acl.conf
http_port 5000

http_access deny !AVAILABLE_WORKING_1 !AVAILABLE_WORKING_2 !AVAILABLE_WORKING$

acl BLACKLISTS dstdomain "/etc/squid/restrict-sites.acl"
http_access deny BLACKLISTS
deny_info http://super.franky.b03.com BLACKLISTS

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy Authentication Required
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED

# UNTUK USER LUFFY
acl USER_LUFFY proxy_auth luffybelikapalb03
acl IMAGE_PNG_REGEX urlpath_regex \.png$
acl IMAGE_JPG_REGEX urlpath_regex \.jpg$

delay_pools 1
delay_class 1 1
delay_parameters 1 8000/8000
delay_access 1 allow USER_LUFFY

http_access allow USER_LUFFY IMAGE_PNG_REGEX
http_access allow USER_LUFFY IMAGE_JPG_REGEX
http_access deny USERS USER_LUFFY

# UNTUK USER ZORO
acl USER_ZORO proxy_auth zorobelikapalb03

http_access deny USER_ZORO IMAGE_PNG_REGEX
http_access deny USER_ZORO IMAGE_JPG_REGEX
http_access allow USERS USER_ZORO

visible_hostname jualbelikapal.b03.com
```

2. Kemudian restart squid proxy pada Water7.

```
service squid restart
```

3. Gunakan wget untuk test download menggunakan user zoro dengan command berikut.

```
wget --proxy-user=zorobelikapalb03 --proxy-password=zoro_b03 super.franky.b03.com/public/images/franky.png

wget --proxy-user=zorobelikapalb03 --proxy-password=zoro_b03 super.franky.b03.com/public/images/car.jpg

wget --proxy-user=zorobelikapalb03 --proxy-password=zoro_b03 super.franky.b03.com/public/images/frankysupersecretfood.cursed
```

4. Untuk setiap command diatas, jika file yang di-download bukan merupakan file `.png` atau `.jpg` maka proses download akan berhasil tanpa ada limit kecepatan seperti berikut.

![image](https://user-images.githubusercontent.com/16128257/140804708-09da8237-10ec-4e4d-9576-0e0526a4ccba.png)

5. Jika file yang di-download merupakan `.png` ataupun `.jpg` maka proses download akan ditolak.

![image](https://user-images.githubusercontent.com/16128257/140804812-f84496ed-0e13-4a58-9763-7c3f64382d78.png)

![image](https://user-images.githubusercontent.com/16128257/140804848-0d04370e-7132-4684-9495-c7d53fa07ae9.png)

## References

- https://askubuntu.com/questions/857659/extend-simple-dhcp-address-range-to-include-more-addresses
- https://support.cumulusnetworks.com/hc/en-us/articles/201787526-Configuring-DHCP-Relays
- http://yourtoolbox.blogspot.com/2013/01/dhcp-forwarding-with-relay-server.html
- https://stackoverflow.com/questions/11815121/how-can-i-use-md5-passwords-in-htpasswd-file
- https://henri.web.id/redirect-url-menggunakan-squid-proxy-server/
- https://kifarunix.com/how-to-setup-squid-proxy-basic-authentication-with-username-and-password/
- https://serverfault.com/questions/844527/multiple-http-access-logic-at-the-same-time-for-squid-proxy
