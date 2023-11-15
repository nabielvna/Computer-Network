# Jarkom Modul 3 E04 2023

## Anggota Kelompok:

1. Yoga Firman Syahputra (5025221212)
2. Vidiawan Nabiel Rasyid (5025221231)

### Prefix IP: 192.208

## Daftar Isi

1. [Pembuatan Topologi](#topologi)
2. [Konfigurasi Arjuna](#soal-2)
3. [Konfigurasi Abimanyu](#soal-3)
4. [Parikesit Abimanyu](#soal-4)
5. [Reverse Domain Abimanyu](#soal-5)
6. [Werkudara DNS Slave](#soal-6)

# Praktikum
### Topologi
[

**Soal**  
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni.
</br></br>Berikut adalah topologi yang diminta kepada kelompok E04
![Topologi 1](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/2nd-Module/Assets/Topologi%201.png?raw=true)
</br></br>Berikut adalah hasil yang kami buat  
![1](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/2nd-Module/Assets/1.png?raw=true)
#### Step:  
Buka console Yudhistira, buat domainnya dengan nama sesuai node nya “yudhistira.com”:
```
apt-get update
apt-get install bind9 -y
cd /etc/bind  
```
Kemudian akses named.conf.local dengan:
```
nano named.conf.local
```
Edit konfigurasinya dengan menambahkan:
```
zone "yudhistira.com" {
	type master;
	file "/etc/bind/yudhistira/yudhistira.com";
};
```
Buat folder yudhistira pada etc/bind
```
mkdir /etc/bind/yudhistira
```
Copy file db local
```
cp db.local yudhistira/yudhistira.com
```
Kemudian buka filenya
```
nano yudhistira/yudhistira.com
```
Edit konfigurasinya menjadi seperti ini:
![2](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/2nd-Module/Assets/2.png?raw=true)
Kemudian lakukan ping dari node mana saja ke Node Yudhistira dengan mengarahkan node tersebut ke IP Node Yudhistira)

Selanjutnya, jadikan Yudhistira sebagai DNS Master dengan mengedit kembali konfigurasi pada named.conf.local:
![3](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/2nd-Module/Assets/3.png?raw=true)

</br> Restart bind9
```
service bind9 restart
```

</br>Selanjutnya, jadikan Werkudara sebagai DNS slave dengan mengedit konfigurasi pada named.conf.local:
```
apt-get update
apt-get install bind9 -y
cd /etc/bind
```
Kemudian akses named.conf.local dengan:
```
nano named.conf.local
```
Edit konfigurasinya dengan menambahkan:
![4](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/2nd-Module/Assets/4.png?raw=true)
Restart bind9
```
service bind9 restart
```

Berikutnya, kita bisa melakukan testing dengan menghentikan service bind9 pada Yudhistira dan lakukan PING kepada “yudhistira.com” dari node mana saja dengan mengarahkan pada juga node tersebut pada alamat IP dari Werkudara dan akan kita dapatkan upaya PING akan berhasil!

### Soal 2
**Soal**  
Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias 
www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

#### Step:
Buka konsol Arjuna
```
apt-get update
apt-get install bind9 -y
```
Kemudian akses named.conf.local dengan:
```
nano named.conf.local
```
Edit konfigurasinya dengan menambahkan:
![5](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/2nd-Module/Assets/5.png?raw=true)

Buat folder arjuna
```
Yudhistira: mkdir arjuna
```
Copy file db local
```
Yudhistira: cp db.local arjuna/arjuna.e04.com
```
Kemudian akses named.conf.local dengan:
```
nano arjuna/arjuna.e04.com
```
Edit konfigurasinya dengan menambahkan:
(anggap kita belum memberi www)
![6](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/2nd-Module/Assets/6.png?raw=true)

Buka konsol pada Yudhistira:
```
 service bind9 restart
```
Saatnya kita membuat Canonical Name-nya, lakukan edit konfigurasi pada named.conf.local
```
nano named.conf.local
```
```
service bind9 restart
```

Lakukan cek dengan melakukan dan hasilnya harus mengarah ke host dengan IP Arjuna
```
host -t CNAME www.arjuna.e04.com
	atau
ping arjuna.e04.com -c 5
```
### Soal 3
**Soal**  
Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke 
abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

**Step:**
Buka konsol Abimanyu
```
apt-get update
apt-get install bind9 -y
cd /etc/bind
```
Kemudian akses named.conf.local dengan:
```
nano named.conf.local
```
Edit konfigurasinya dengan menambahkan:
```
zone "abimanyu.e04.com" {
type master;
   file "/etc/bind/abimanyu/abimanyu.e04.com";
    };
)
```
Restart bind9
```
service bind9 restart
```
Buat folder abimanyu
```
mkdir abimanyu
```
Copy file db local
```
cp dc.local abimanyu/abimanyu.e04.com
```
Buka file
```
nano abimanyu/abimanyu.e04.com
```
Edit konfigurasi menjadi seperti di bawah ini:
![7](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/2nd-Module/Assets/7.png?raw=true)
Restart bind9
```
service bind9 restart
```
Lakukan cek dengan melakukan dan hasilnya harus mengarah ke host dengan IP abimanyu
```
host -t CNAME www.abimanyu.e04.com
	atau
www.abimanyu.e04.com -c 5
```
### Soal 4
**Soal**
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.
   
**Step:**  
Buka konsol Yudhistira
```
nano /etc/bind/yudhistira/yudhistira.com
```
Edit konfigurasi menjadi seperti di bawah ini:
![8](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/2nd-Module/Assets/8.png?raw=true)
Restart bind9
```
service bind9 restart
```

### Soal 5
**Soal**  
Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)

**Step:**  
Buka konsol Yudhistira
```
nano named.conf.local
```

Tambahkan syntax sebagai berikut: 
![9](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/2nd-Module/Assets/9.png?raw=true)
```
cp db.local yudhistira/3.208.192.in-addr.arpa
```
```
nano named.conf.local
```
Edit konfigurasi sehingga menjadi seperti di bawah ini:
![10](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/2nd-Module/Assets/10.png?raw=true)
Restart bind9
```
service bind9 restart
```

### Soal 6
**Soal**  
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.

**Step:**  
Buka konsol Yudhistira
```
nano named.conf.local
```

Edit konfigurasi menjadi seperti di bawah ini:
![11](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/2nd-Module/Assets/11.png?raw=true)
Restart bind9
```
service bind9 restart
```

Beralih kepada Werkudara sebagai DNS Slave  
Buka konsol werkudara
```
apt-get update
apt-get install bind9 -y
```
```
nano named.conf.local
```
Tambahkan syntax seperti di bawah ini:
![12](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/2nd-Module/Assets/12.png?raw=true)

Yudhistira
```
service bind9 restart
```

Beralih kepada Werkudara sebagai DNS Slave
```
apt-get update
apt-get install bind9 -y
```
```
nano named.conf.local
```
Tambahkan syntax seperti di bawah ini:
![13](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/2nd-Module/Assets/13.png?raw=true)
Werkudara
```
service bind9 restart
```

Berikutnya testing dari Nakula dengan mengarahkannya ke IP Yudhistira dan IP Werkudara
Buka konsol Nakula
```
nano /etc/resolv.conf
```
Arahkan ke dua IP tersebut:
![14](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/2nd-Module/Assets/14.png?raw=true)

Matikan bind9 pada Yudhistira
```
service bind9 stop
```
Buka konsol Nakula
```
ping abimanyu.e04.com
```
![15](https://github.com/nabielvna/Computer-Network/blob/main/Practicum/2nd-Module/Assets/15.png?raw=true)
