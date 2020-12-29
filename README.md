# Jarkom_Modul5_Lapres_C03

- Brananda Denta WP - 05111840000143
- R. Dafa Berlian Denmar - 05111840000149
### Outline
1. [Soal A](#Soal-A)
2. [Soal B](#Soal-B)
3. [Soal C](#Soal-C)
4. [Soal D](#Soal-D)
5. [Soal 1](#Soal-1)
6. [Soal 2](#Soal-2)
7. [Soal 3](#Soal-3)
8. [Soal 4](#Soal-4)
9. [Soal 5](#Soal-5)
10. [Soal 6](#Soal-6)
11. [Soal 7](#Soal-7)

## Soal A
Tugas pertama kalian yaitu membuat topologi jaringan sesuai dengan rancangan yang diberikan Bibah seperti dibawah ini :

![image](https://user-images.githubusercontent.com/57977401/103073832-45832180-4603-11eb-9d91-8e34c9dfe803.png)

Jawab :
- Membuat file `topologi.sh`
  
```sh
# Switch
uml_switch -unix switch0 > /dev/null < /dev/null & #A0
uml_switch -unix switch1 > /dev/null < /dev/null & #A1
uml_switch -unix switch2 > /dev/null < /dev/null & #A2
uml_switch -unix switch3 > /dev/null < /dev/null & #A3
uml_switch -unix switch4 > /dev/null < /dev/null & #A4
uml_switch -unix switch5 > /dev/null < /dev/null & #A5

# Router
xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,10.151.76.17 eth1=daemon,,,switch0 eth2=daemon,,,switch3 mem=96M &
xterm -T BATU -e linux ubd0=BATU,jarkom umid=BATU eth0=daemon,,,switch3 eth1=daemon,,,switch2 eth2=daemon,,,switch5 mem=96M &
xterm -T KEDIRI -e linux ubd0=KEDIRI,jarkom umid=KEDIRI eth0=daemon,,,switch0 eth1=daemon,,,switch1 eth2=daemon,,,switch4 mem=96M &

# Server
xterm -T PROBOLINGGO -e linux ubd0=PROBOLINGGO,jarkom umid=PROBOLINGGO eth0=daemon,,,switch1 mem=128M &
xterm -T MADIUN -e linux ubd0=MADIUN,jarkom umid=MADIUN eth0=daemon,,,switch1 mem=128M &
xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=128M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=128M &

# Klien 1
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch4 mem=96M &
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch5 mem=96M &
```


## Soal B
Karena kalian telah mempelajari Subnetting dan Routing, Bibah meminta kalian untuk membuat topologi tersebut menggunakan teknik CIDR atau VLSM.

Jawab : 

Kami menggunakan metode ***VLSM***.

### Langkah Pengerjaan

- Membuat pengelompokkan subnet dengan metode ***VLSM***. Berikut adalah hasil akhirnya:


![VLSM](/img/cisco.jpg)


- Membuat Tree dari hasil subnetting


![Praktikum5](/img/tree.jpg)


- Berikut adalah daftar detail informasi setiap subnet


![Pembagian IP](/img/tabelip.jpg)



- Pada setiap UML, tambahkan konfigurasi seperti berikut ini pada file `/etc/network/interfaces`.

### Surabaya
```sh
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.76.18
netmask 255.255.255.252
gateway 10.151.70.27

auto eth1
iface eth1 inet static
address 192.168.0.5
netmask 255.255.255.252

auto eth2
iface eth2 inet static
address 192.168.0.1
netmask 255.255.255.252
```
### Batu
```sh
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.0.2
netmask 255.255.255.252
gateway 192.168.0.1

auto eth1
iface eth1 inet static
address 10.151.77.33
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 192.168.1.1
netmask 255.255.255.0
```
### Kediri
```sh
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.0.6
netmask 255.255.255.252
gateway 192.168.0.5

auto eth1
iface eth1 inet static
address 192.168.0.9
netmask 255.255.255.248

auto eth2
iface eth2 inet static
address 192.168.2.1
netmask 255.255.255.0
```
### Sidoarjo
```sh
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
address 192.168.1.2
netmask 255.255.255.0
gateway 192.168.1.1
```
### Gresik
```sh
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
address 192.168.2.2
netmask 255.255.255.0
gateway 192.168.2.1
```
### Mojokerto
```sh
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.77.35
netmask 255.255.255.248
gateway 10.151.77.33
```
### Malang
```sh
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 10.151.77.34
netmask 255.255.255.248
gateway 10.151.77.33
```

### Probolinggo
```sh
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.0.10
netmask 255.255.255.248
gateway 192.168.0.9
```

### Madiun
```sh
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet static
address 192.168.0.11
netmask 255.255.255.248
gateway 192.168.0.9
```


## Soal C
Kalian juga diharuskan melakukan routing agar setiap perangkat pada jaringan tersebut dapat terhubung.

Jawab : 

Routing dilakukan pada UML Surabaya sebagai berikut 

```sh
route add -net 10.151.77.32 netmask 255.255.255.248 gw 192.168.0.1
route add -net 192.168.1.0 netmask 255.255.255.0 gw 192.168.0.1
route add -net 192.168.2.0 netmask 255.255.255.0 gw 192.168.0.5
route add -net 192.168.0.8 netmask 255.255.255.248 gw 192.168.0.5
```
## Soal D
Tugas berikutnya adalah memberikan ip pada subnet SIDOARJO dan GRESIK secara dinamis menggunakan bantuan **DHCP SERVER** (Selain subnet tersebut menggunakan ip static). Kemudian kalian mengingat bahwa kalian harus setting **DHCP RELAY** pada router yang menghubungkannya,seperti yang kalian telah pelajari di masa lalu.

Jawab : 

- UML MOJOKERTO ***(DHCP Server)***

1. Lakukan `apt-get update`
2. Lalu install dhcp dengan perintah `apt-get install isc-dhcp-server`
3. Buka file `/etc/default/isc-dhcp-server`
4. Masukkan `INTERFACESv4="eth0"`
5. Buka file dengan perintah `nano /etc/dhcp/dhcpd.conf`
6. Masukkan konfigurasi seperti berikut

![VLSM](/img/d.jpg)

7. Lakukan restart dengan `service isc-dhcp-server restart`
8. Masukkan pada UML GRESIK dan SIDOARJO konfigurasi dibawah
```sh
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp
```

- UML DHCP Relay
9. Pada UML BATU dan KEDIRI, ubah konfigurasi yang ada pada file /etc/default/isc-dhcp-relay menjadi seperti gambar dibawah ini

![VLSM](/img/relayd.jpg)

10. Jalankan service `isc-dhcp-relay restart`

## Soal 1
Agar topologi yang kalian buat dapat mengakses keluar, kalian diminta untuk mengkonfigurasi SURABAYA menggunakan iptables, namun Bibah tidak ingin kalian menggunakan `MASQUERADE`.

Jawab : 

Pada UML **Surabaya**, buat file `1.sh` berisi perintah seperti dibawah ini. lalu jalankan perintah `bash 1.sh`
```sh
iptables -t nat -A POSTROUTING -s 192.168.0.0/16 -o eth0 -j SNAT --to-source 10.151.76.18
```

Penjelasan : 
dengan menggunakan table nat chain POSTROUTING melalui eth0, setiap paket yang menuju keluar akan dirubah sourcenya menjadi 10.151.76.18

- **Testing**
1. ping its.ac.id di seluruh UML
2. Apabila hasilnya adalah sebagai berikut, mana konfigurasi berhasil

![No1](/img/pingits.jpg)


## Soal 2
Kalian diminta untuk men**drop** semua akses SSH dari luar Topologi (UML) Kalian pada server yang memiliki ip DMZ (DHCP dan DNS SERVER) pada **SURABAYA** demi menjaga keamanan.

Jawab : 

Pada UML **Surabaya**, buat file `2.sh` berisi perintah seperti dibawah ini. lalu jalankan perintah `bash 2.sh`

```sh
iptables -A FORWARD -d 10.151.77.32/29 -i eth0 -p tcp --dport 22 -j DROP #surabaya
```

Penjelasan :

pada UML SURABAYA dibuat iptables dengan chain FORWARD, setiap paket dengan protokol tcp, incoming interface eth0 9dari luar), dan port tujuan 22 serta subnet tujuan 10.151.77.89/29 akan didrop

- **Testing**
1. Lakukan `apt-get update`
2. Install netcat pada seluruh UML dengan perintah `apt-get install netcat`
3. pada putty, ketikkan `nc 10.151.77.34 22`
4. Pada UML MALANG atau MOJOKERTO, ketikkan perintah `nc -l -p 22`
5. Ketikkan sesuatu
6. Apabila hasilnya seperti dibawah, maka konfigurasi berhasil

![No2](/img/no2.jpg)

## Soal 3
Karena tim kalian maksimal terdiri dari 3 orang, Bibah meminta kalian untuk membatasi DHCP dan DNS server hanya boleh menerima maksimal 3 koneksi ICMP secara bersamaan yang berasal dari mana saja menggunakan iptables pada masing masing server, selebihnya akan di DROP.

- Pada UML MALANG dan MOJOKERTO
```sh
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP #malang/mojo
```

Penjelasan : 
dengan menggunakan chain INPUT setiap paket dengan protokol icmp akan dibatasi untuk membuat koneksi sebanyak 3 ( MAXIMAL 3 UML melakukan koneksi ke DHCP / DNS SERVER ) selebihnya akan didrop.

- **Testing**
1. Lakukan ping ke IP MALANG (10.151.77.34) atau IP MOJOKERTO (10.151.77.35) di 4 UML
2. Apabila hasilnya seperti dibawah, maka konfigurasi berhasil

**Ping ke IP MALANG (10.151.77.34)**

![No3 Malang](/img/no3.jpg)

## Soal 4
Akses dari subnet SIDOARJO hanya diperbolehkan pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat. Selain itu paket akan di REJECT.

- Pada UML MALANG
```sh
iptables -A INPUT -s 192.168.1.0/24 -m time --timestart 07:00 --timestop 17:00 --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
iptables -A INPUT -s 192.168.1.0/24 -j REJECT
```

Penjelasan :

menggunakan chain INPUT, setiap paket yang berasal subnet SIDOARJO hanya boleh diakses pada pukul 07.00 - 17.00 pada hari Senin sampai Jumat

- **Testing**
1. Ubah tanggal dan jam hari ini dengan perintah `date --set="2020/12/23 06:00"`
2. Cek tanggal sudah benar atau belum dengan perintah `date`
3. Lakukan `ping 10.151.77.34` pada UML SIDOARJO
4. Apabila hasilnya seperti dibawah, maka konfigurasi berhasil

![No4 dan 5 Satu](/img/no4.jpg)

## Soal 5
Akses dari subnet GRESIK hanya diperbolehkan pada pukul 17.00 hingga pukul 07.00 setiap harinya. Selain itu paket akan di REJECT.

- Pada UML MALANG
```sh
iptables -A INPUT -s 192.168.2.0/24 -m time --timestart 07:00 --timestop 17:00 -j REJECT
```

Penjelasan : 

menggunakan chain INPUT, akses dari subnet GRESIK akan didrop ketika mengakses pada pukul 07.01 sampai 16.59. Selain itu paket akan diaccept.

- **Testing**
1. Ubah tanggal dan jam hari ini dengan perintah `date --set="2020/12/23 06:00"`
2. Cek tanggal sudah benar atau belum dengan perintah `date`
3. Lakukan `ping 10.151.77.34` pada UML GRESIK
4. Apabila hasilnya seperti dibawah, maka konfigurasi berhasil

![No4 dan 5 Dua](/img/no5.jpg)


# Soal 6
Bibah ingin SURABAYA disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada PROBOLINGGO port 80 dan MADIUN port 80.

- Pada UML SURABAYA
```sh
iptables -A PREROUTING -t nat -p tcp -d 10.151.77.34 --dport 80 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.168.0.10:80
iptables -A PREROUTING -t nat -p tcp -d 10.151.77.34 --dport 80 -j DNAT --to-destination 192.168.0.11:80

iptables -t nat -A POSTROUTING -p tcp -d 192.168.0.10 --dport 80 -j SNAT --to-source 10.151.77.34:80
iptables -t nat -A POSTROUTING -p tcp -d 192.168.0.11 --dport 80 -j SNAT --to-source 10.151.77.34:80
```

Penjelasan :

Menggunakan chain PREORUTING dan table nat , setiap paket yang menuju  dns server dengan protocol tcp dan port tujuan 80 akan didistribusikan masing masing ke `192.168.2.10:80` (PROBOLINGGO) dan `192.168.2.11:80` (MADIUN)

- **Testing**
1. Lakukan `apt-get update`
2. Install netcat pada UML MADIUN, PROBOLINGGO, MALANG, SIDOARJO, dan GRESIK dengan perintah `apt-get install netcat`
3. pada 2 terminal Putty, ketikkan `nc 10.151.77.34 80`
4. Pada UML MADIUN dan PROBOLINGGO, ketikkan perintah `nc -l -p 80`
5. Ketikkan sesuatu 
6. Apabila hasilnya seperti dibawah (gantian antar MADIUN dan PROBOLINGGO), maka konfigurasi berhasil

![No6](/img/no6.jpg)



# Soal 7
Bibah ingin agar semua paket didrop oleh firewall (dalam topologi) tercatat dalam log pada setiap UML yang memiliki aturan drop.


- Pada UML SURABAYA, ganti isi dari `2.sh` menjadi sebagai berikut
```sh
#ganti no 2
iptables -A FORWARD -d 10.151.77.32/29 -i eth0 -p tcp --dport 22 -j LOG --log-prefix "FORWARD TCP-DROPPED: " #surabaya
iptables -A FORWARD -d 10.151.77.32/29 -i eth0 -p tcp --dport 22 -j DROP #surabaya
```

- Pada UML MALANG dan MOJOKERTO, ganti isi dari `3.sh` menjadi seperti di bawah ini
```sh
#ganti no 3
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j LOG --log-prefix "Connection Limit --> DROPPED : "
iptables -A INPUT -p icmp -m connlimit --connlimit-above 3 --connlimit-mask 0 -j DROP
```

Penjelasan :

Menambahkan LOG ketika suatu action (dalam hal ini **DROP**) dilakukan, jadi setiap terjadi drop packet, maka akan ditambahkan LOG tersebut.

- **Testing**
1. Jalankan `2.sh` di SURABAYA dan `3.sh` di MALANG dan MOJOKERTO
2. Apabila hasilnya seperti dibawah, maka konfigurasi berhasil

![No7](/img/no2.jpg)

![No7](/img/no7.jpg)

