# Jarkom-Modul-2-B26-2023

Anggota Kelompok 

|   Nama                             | NRP      |
| ------                             | ------   |
|  Nur Azizah                        |5025211252|
|  Rayhan Almer Kusumah              |5025211115|
|  Faiz Haq Noviandra Ciptadi Putra  |5025211132|

## Setting Config
Sebelum memulai pertama-tama kita harus mengatur configurasi tiap node sebagai berikut
Pandudewanata
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.191.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.191.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
      address 192.191.3.1
      netmask 255.255.255.0
```
Yudhistira
```
auto eth0
iface eth0 inet static
	address 192.191.1.2
	netmask 255.255.255.0
	gateway 192.191.1.1
```
Werkudara
```
auto eth0
iface eth0 inet static
	address 192.191.1.3
	netmask 255.255.255.0
	gateway 192.191.1.1
```
Nakula
```
auto eth0
iface eth0 inet static
	address 192.191.2.2
	netmask 255.255.255.0
	gateway 192.191.2.1
```
Sadewa
```
auto eth0
iface eth0 inet static
	address 192.191.2.3
	netmask 255.255.255.0
	gateway 192.191.2.1
```
Abimanyu
```
auto eth0
iface eth0 inet static
	address 192.191.3.2
	netmask 255.255.255.0
	gateway 192.191.3.1
```
Prabukusuma
```
auto eth0
iface eth0 inet static
	address 192.191.3.3
	netmask 255.255.255.0
	gateway 192.191.3.1
```
Wisanggeni
```
auto eth0
iface eth0 inet static
	address 192.191.3.4
	netmask 255.255.255.0
	gateway 192.191.3.1
```
Arjuna
```
auto eth0
iface eth0 inet static
	address 192.191.3.5
	netmask 255.255.255.0
	gateway 192.191.3.1
```

## IP Nodes
Pandudewanata     : 192.191.1.1 (Switch 1)
Yudhistira        : 192.191.1.2
Werkudara         : 192.191.1.3
Pandudewanata     : 192.191.2.1 (Switch 2)
Nakula            : 192.191.2.2
Sadewa            : 192.191.2.3
Pandudewanata     : 192.191.3.1 (Switch 3)
Abimanyu          : 192.191.3.2
Prabukusuma       : 192.191.3.3
Wisanggeni        : 192.191.3.4
Arjuna            : 192.191.3.5

## Soal 1
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni

### Jawaban
Kita tinggal menjalankan setting sesuai dengan instruksi diatas kemudian kita tes pada node client (Nakula/Sadewa) dengan ``` ping google.com -c 5 ```


## Soal 2
Buatlah website utama dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok
