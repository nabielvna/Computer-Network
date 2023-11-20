# Jarkom Modul 3 E04 2023

## Anggota Kelompok:

1. Yoga Firman Syahputra (5025221212)
2. Vidiawan Nabiel Rasyid (5025221231)

### Prefix IP: 192.208

## Daftar Isi
1. [Topologi](#1-topologi)
2. [Pembuatan Domain](#2-pembuatan-domain)
3. [DHCP](#3-dhcp)
   - [DHCP Server](#31-dhcp-server)
   - [DHCP Relay](#32-dhcp-relay)
   - [Client DHCP](#33-client-dhcp)

# Praktikum
#### 0. Setelah mengalahkan Demon King, perjalanan berlanjut. Kali ini, kalian diminta untuk melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP mengarah pada worker yang memiliki IP [prefix IP].x.1.
  ### Solve: 
![Topologi](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/3rd-Module/Asset/Topologi.png?raw=true)

riegel.canyon.yyy.com untuk `worker Laravel` </br>
granz.channel.yyy.com untuk `worker PHP` </br>
mengarah pada worker yang memiliki IP `[prefix IP].x.1.` 

<b>Heiter (DNS Server):</b>
```
apt-get update
apt-get install bind9 -y
```

</br> Edit konfigurasi
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

</br> Kemudian buat direktori dan copy ke `db.local`
```
mkdir /etc/bind/canyon 
mkdir /etc/bind/channel
cp /etc/bind/db.local /etc/bind/canyon/canyon.e04.com
cp /etc/bind/db.local /etc/bind/channel/channel.e04.com 
```

</br> Edit konfigurasi canyon
```
nano /etc/bind/canyon/canyon.e04.com
```
![Konfigurasi canyon](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/3rd-Module/Asset/Konfigurasi%20canyon.png?raw=true)

</br> Edit konfigurasi channel
```
nano /etc/bind/channel/channel.e04.com
```
![Konfigurasi channel](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/3rd-Module/Asset/Konfigurasi%20channel.png?raw=true)

</br> Restart bind9
```
service bind9 restart
```

</br> Lakukan testing pada domain dan subdomain yang telah dibuat
</br> <b>Himmel (node tetangga):</b> 
```
echo nameserver 192.208.1.2 > /etc/resolv.conf
```
</br>Lakukan ping domain dan sub domain
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

#### 1. Lakukan konfigurasi sesuai dengan peta yang sudah diberikan.
   ### Solve:
   Konfigurasi alamat-alamat IP Static pada node Himmel, Heiter, Denken, Eisen, Frieren, Flamme, Fern, Lawine, Linie, dan Lugner
![Screenshot 2023-11-20 111435](https://github.com/nabielvna/Computer-Network/assets/121499055/309ad726-83e1-4191-ab5f-91ca3214916f)
Ulangi langkah tersebut pada node-node lainnya yang memerlukan IP Static.

#### 2. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.16 - [prefix IP].3.32 dan [prefix IP].3.64 - [prefix IP].3.80
#### 3. Client yang melalui Switch4 mendapatkan range IP dari [prefix IP].4.12 - [prefix IP].4.20 dan [prefix IP].4.160 - [prefix IP].4.168
   ### Solve (2) (3):
   Buka Himmel sebagai DHCP Server dan konfigurasikan seperti di bawah ini,
   #### DHCP Server
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-server -y
```
Menentukan `interface`
```
nano /etc/default/isc-dhcp-server
```
```
INTERFACESv4="eth0"
```
![DHCP Interface](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/3rd-Module/Asset/DHCP%20Server%20interface.png?raw=true)

</br> Konfigurasi `DHCP Server`
```
nano /etc/dhcp/dhcpd.conf
```

Kemudian isi dengan
```
subnet 192.208.1.0 netmask 255.255.255.0 {  
}

subnet 192.208.2.0 netmask 255.255.255.0 {
}

subnet 192.208.3.0  netmask 255.255.255.0 {
  range 192.208.3.16 192.208.3.32;
  range 192.208.3.64 192.208.3.80;
  option routers 192.208.3.0;
  option broadcast-address 192.208.3.255;
  option domain-name-servers 192.208.1.2;
  default-lease-time 180;
  max-lease-time 5760;
}

subnet 192.208.4.0  netmask 255.255.255.0 {
  range 192.208.4.12 192.208.4.20;
  range 192.208.4.160 192.208.4.168;
  option routers 192.208.4.0;
  option broadcast-address 192.208.4.255;
  option domain-name-servers 192.208.1.2;
  default-lease-time 720;
  max-lease-time 5760; 
}
```

</br> Restart `DHCP Server`
```
service isc-dhcp-server restart
```
Kemudian, setting DHCP Relay agar client dari berbagai ethernet dan DHCP Server dapat saling terkoneksi 
#### DHCP Relay
```
apt-get update
apt-get install isc-dhcp-relay -y
```

</br> Lakukan konfigurasi `DHCP Relay`
```
nano /etc/default/isc-dhcp-relay
```
```
SERVERS="192.208.1.1"  
INTERFACES="eth1 eth2 eth3 eth4"
OPTIONS=
```
![Konfigursi dhcp relay](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/3rd-Module/Asset/Konfigurasi%20dhcp%20relay.png?raw=true)

</br> Lakukan konfigurasi IP Forwarding
```
nano /etc/sysctl.conf
```
Kemudian isi dengan
```
net.ipv4.ip_forward=1
```
![ip forwarding](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/3rd-Module/Asset/IP%20forwarding%20dhcp%20relay.png?raw=true)

</br> Restart `DHCP Relay`
```
service isc-dhcp-relay restart
```

kemudian, setting DHCP Client agar dapat memiliki protokol dari DHCP
#### Client DHCP
Ubah konfigurasi interface client
```
nano /etc/network/interfaces
```
Kemudian isi dengan 
```
auto eth0
iface eth0 inet dhcp
```
![konfigurasi client dhcp](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/3rd-Module/Asset/Konfigurasi%20client%20dhcp.png?raw=true)

</br> Restart `client dhcp`
</br> Dengan cara `GNS3 → klik kanan pada client DHCP → klik Stop → klik kanan kembali pada client DHCP → klik Start`

</br> Cek apakah client mendapatkan IP yang sesuai
![berhasil](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/3rd-Module/Asset/client%20dhcp%20berhasil.png)
