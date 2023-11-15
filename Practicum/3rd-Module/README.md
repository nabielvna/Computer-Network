# Jarkom Modul 3 E04 2023

## Anggota Kelompok:

1. Yoga Firman Syahputra (5025221212)
2. Vidiawan Nabiel Rasyid (5025221231)

### Prefix IP: 192.208

## Daftar Isi

0. [Topologi](#0.-topologi)
1. [Pembuatan Domain](#1.-pembuatan-domain)
2. [DHCP](#2.-dhcp)

# Praktikum
### 0. Topologi
![Topologi](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/3rd-Module/Asset/Topologi.png?raw=true)

### 1. Pembuatan Domain
riegel.canyon.yyy.com untuk `worker Laravel` </br>
granz.channel.yyy.com untuk `worker PHP` </br>
mengarah pada worker yang memiliki IP `[prefix IP].x.1.` 

<b>Heiter (DNS Server):</b>
```
apt-get update </br>
apt-get install bind9 -y
```

Edit konfigurasi
```
nano /etc/bind/named.conf.local
```
```
zone "canyon.e04.com" {
	type master;
	file "/etc/bind/canyon/canyon.e04.com";
};

zone "channel.e04.com" {
	type master;
	file "/etc/bind/channel/channel.e04.com";
};
```
![Konfigurasi domain](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/3rd-Module/Asset/Konfigurasi%20domain.png?raw=true)

Kemudian buat direktori dan copy ke `db.local`
```
mkdir /etc/bind/canyon 
mkdir /etc/bind/channel
cp /etc/bind/db.local /etc/bind/canyon/canyon.e04.com
cp /etc/bind/db.local /etc/bind/channel/channel.e04.com 
```

Edit konfigurasi canyon
```
nano /etc/bind/canyon/canyon.e04.com
```
![Konfigurasi canyon](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/3rd-Module/Asset/Konfigurasi%20canyon.png?raw=true)

Edit konfigurasi channel
```
nano /etc/bind/channel/channel.e04.com
```
![Konfigurasi channel](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/3rd-Module/Asset/Konfigurasi%20channel.png?raw=true)

Restart bind9
```
service bind9 restart
```

<b>Himmel:</b> </br>
Lakukan testing 
```
echo nameserver 192.208.1.2 > /etc/resolv.conf
```
ping domain dan sub domain
```
ping canyon.e04.com
```
![ping canyon](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/3rd-Module/Asset/ping%20canyon.png?raw=true)
```
ping channel.e04.com
```
![ping channel](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/3rd-Module/Asset/ping%20channel.png?raw=true)
```
ping riegel.canyon.e04.com 
```
![ping riegel](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/3rd-Module/Asset/ping%20riegel.png?raw=true)
```
ping granz.channel.e04.com
```
![ping granz](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/3rd-Module/Asset/ping%20granz.png?raw=true)

### 2. DHCP
