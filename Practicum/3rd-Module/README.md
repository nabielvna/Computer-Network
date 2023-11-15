# Jarkom Modul 3 E04 2023

## Anggota Kelompok:

1. Yoga Firman Syahputra (5025221212)
2. Vidiawan Nabiel Rasyid (5025221231)

### Prefix IP: 192.208

## Daftar Isi

1. [Topologi](#topologi)
2. [Pembuatan Domain](#pembuatan-domain)
3. [Konfigurasi Abimanyu](#soal-3)
4. [Parikesit Abimanyu](#soal-4)
5. [Reverse Domain Abimanyu](#soal-5)
6. [Werkudara DNS Slave](#soal-6)

# Praktikum
### Topologi
![Topologi](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/3rd-Module/Asset/Topologi.png?raw=true)

### Pembuatan Domain
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
FOTO "Konfigurasi domain"

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
FOTO "Konfigurasi canyon"

Edit konfigurasi channel
```
nano /etc/bind/channel/channel.e04.com
```
FOTO "Konfigurasi channel"

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
FOTO "ping canyon"
```
ping channel.e04.com
```
FOTO "ping channel"
```
ping riegel.canyon.e04.com 
```
FOTO "ping riegel"
```
ping granz.channel.e04.com
```
FOTO "ping granz"
