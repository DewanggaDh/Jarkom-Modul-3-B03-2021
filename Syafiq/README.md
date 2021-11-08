# ReadMe

Sebuah catatan hati, dikirim oleh ujian dan cobaan praktek yang perlu diakhiri - syafiq

## Nomor 8

Pada Loguetown, proxy harus bisa diakses dengan nama jualbelikapal.yyy.com dengan port yang digunakan adalah 5000

### Penyelesaian

1. Pada Water7 install squid proxy menggunakan command berikut.

```
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

9. Harusnya Squid proxy pada Water7 bisa berjalan dengan baik. Untuk test kita gunakan Loguetown sebagai client proxy.

10. Pada Loguetown, install Lynx dengan command berikut.

```
apt-get update
apt-get install lynx -y
```

11. Kemudian run command berikut untuk memasang web proxy, agar Lynx dapat menggunakan proxy.

```
export http_proxy="http://jualbelikapal.b03.com:5000"
```

12. Dengan Lynx coba kunjungi `google.com` atau alamat lainnya.

```
lynx google.com
```

13. Sebagai bukti proxy berhasil adalah sebagai berikut. PS: bukti berikut didapat dari penambahan auth karena jika konfigurasi squid masih seperti pada langkah di atas, bukti bahwa proxy berhasil tidak bisa didapatkan.

![image](https://user-images.githubusercontent.com/16128257/140787852-d15a502a-aa6d-4d6f-be91-78846a22d50c.png)

## Nomor 9

Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu luffybelikapalyyy dengan password luffy_yyy dan zorobelikapalyyy dengan password zoro_yyy

### Penyelesaian

## Nomor 10

Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum’at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00).

### Penyelesaian

## Nomor 11

Agar transaksi bisa lebih fokus berjalan, maka dilakukan redirect website agar mudah mengingat website transaksi jual beli kapal. Setiap mengakses google.com, akan diredirect menuju super.franky.yyy.com dengan website yang sama pada soal shift modul 2. Web server super.franky.yyy.com berada pada node Skypie

### Penyelesaian

## Nomor 12

Saatnya berlayar! Luffy dan Zoro akhirnya memutuskan untuk berlayar untuk mencari harta karun di super.franky.yyy.com. Tugas pencarian dibagi menjadi dua misi, Luffy bertugas untuk mendapatkan gambar (.png, .jpg), sedangkan Zoro mendapatkan sisanya. Karena Luffy orangnya sangat teliti untuk mencari harta karun, ketika ia berhasil mendapatkan gambar, ia mendapatkan gambar dan melihatnya dengan kecepatan 10 kbps.

### Penyelesaian

## Nomor 13

Sedangkan, Zoro yang sangat bersemangat untuk mencari harta karun, sehingga kecepatan kapal Zoro tidak dibatasi ketika sudah mendapatkan harta yang diinginkannya.

### Penyelesaian