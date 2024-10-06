# Jarkom-Modul2-IT37-20244

Sebuah kerajaan besar di Indonesia sedang mengalami pertempuran dengan penjajah. Kerajaan tersebut adalah Sriwijaya. Karena merasa terdesak Sriwijaya meminta bantuan pada Majapahit untuk mempertahankan wilayahnya. Pertempuran besar tersebut berada di Nusantara. 

## Topologi:  
![App Screenshot](  )

## Konfigurasi
Konfigurasi Network di Nusantara
```bash
mengubah config nusantara untuk switch 1
auto eth1
iface eth1 inet static
	address 192.235.1.1
	netmask 255.255.255.0

mengubah config nusantara untuk switch 3
auto eth2
iface eth2 inet static
	address 192.235.2.1
	netmask 255.255.255.0

mengubah config nusantara untuk switch 4
auto eth3
iface eth3 inet static
	address 192.235.3.1
	netmask 255.255.255.0
```
Konfigurasi Network di HayamWuruk
```bash
auto eth0
iface eth0 inet static
	address 192.235.1.2
	netmask 255.255.255.0
	gateway 192.235.1.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
Konfigurasi Network di Sriwijaya
```bash
auto eth0
iface eth0 inet static
	address 192.235.1.3
	netmask 255.255.255.0
	gateway 192.235.1.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
Konfigurasi Network di TanjungKulai
```bash
auto eth0
iface eth0 inet static
	address 192.235.1.4
	netmask 255.255.255.0
	gateway 192.235.1.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
Konfigurasi Network di Bedahulu
```bash
auto eth0
iface eth0 inet static
	address 192.235.1.5
	netmask 255.255.255.0
	gateway 192.235.1.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
Konfigurasi Network di KotaLingga
```bash
auto eth0
iface eth0 inet static
	address 192.235.1.6
	netmask 255.255.255.0
	gateway 192.235.1.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
Konfigurasi Network di GajahMada
```bash
auto eth0
iface eth0 inet static
	address 192.235.3.2
	netmask 255.255.255.0
	gateway 192.235.3.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
Konfigurasi Network di ThomasAlfaEdison
```bash
auto eth0
iface eth0 inet static
	address 192.235.3.3
	netmask 255.255.255.0
	gateway 192.235.3.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
Konfigurasi Network di Solok
```bash
auto eth0
iface eth0 inet static
	address 192.235.2.2
	netmask 255.255.255.0
	gateway 192.235.2.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```
Konfigurasi Network di Majapahit
```bash
auto eth0
iface eth0 inet static
	address 192.235.2.3
	netmask 255.255.255.0
	gateway 192.235.2.1
	up echo nameserver 192.168.122.1 > /etc/resolv.conf
```


## No.1
#### Untuk mempersiapkan peperangan World War MMXXIV (Iya sebanyak itu), Sriwijaya membuat dua kotanya menjadi web server yaitu Tanjungkulai, dan Bedahulu, serta Sriwijaya sendiri akan menjadi DNS Master. Kemudian karena merasa terdesak, Majapahit memberikan bantuan dan menjadikan kerajaannya (Majapahit) menjadi DNS Slave. 

pertama install bind9 dan dnsutils pada sriwijaya
buat domain : mkdir /etc/bind/jarkom
	            nano /etc/bind/jarkom/it37.com
       
lalu konfigurasi:
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     it37.com.  root.it37.com. (
                          2          ; Serial
                      604800         ; Refresh
                       86400         ; Retry
                     2419200         ; Expire
                      604800 )       ; Negative Cache TTL
;
@       IN      NS      it37.com.
@       IN      A       192.235.1.3       ; IP Sriwijaya
@       IN      AAAA    ::1
```

konfigurasi dns master pada nano /etc/bind/named.conf.local:
```
zone "it37.com" {
    type master;
    notify yes;
    also-notify { 192.235.2.3; };
    allow-transfer { 192.235.2.3; };
    file "/etc/bind/jarkom/it37.com";
};

service bind9 restart
```
setelah itu kita install bind9 dan dnsutils pada majapahit
lalu konfigurasi dns slave pada nano /etc/bind/named.conf.local
```
zone "it37.com" {
    type slave;
    masters { 192.235.1.3; };
    file "/etc/bind/jarkom/it37.com";
};

service bind9 restart
```

lalu install apache pada tangjungkulai dan bendahulu:
``` apt-get install apache2 ```

pada client(hayamwuruk,gajahmada,thomas) tambahkan konfigurasi di  nano /etc/resolv.conf
```
nameserver 192.253.1.3 
nameserver 192.253.2.3 
nameserver 192.168.122.1
```

### No.2-4
### + Karena para pasukan membutuhkan koordinasi untuk melancarkan serangannya, maka buatlah sebuah domain yang mengarah ke Solok dengan alamat sudarsana.xxxx.com dengan alias www.sudarsana.xxxx.com, dimana xxxx merupakan kode kelompok. Contoh: sudarsana.it01.com.

### + Para pasukan juga perlu mengetahui mana titik yang akan diserang, sehingga dibutuhkan domain lain yaitu pasopati.xxxx.com dengan alias www.pasopati.xxxx.com yang mengarah ke Kotalingga.

### + Markas pusat meminta dibuatnya domain khusus untuk menaruh informasi persenjataan dan suplai yang tersebar. Informasi dan suplai meme terbaru tersebut mengarah ke Tanjungkulai dan domain yang ingin digunakan adalah rujapala.xxxx.com dengan alias www.rujapala.xxxx.com.

di sriwijaya nano /etc/bind/named.conf.local
lalu konfigurasi ini:
```
zone "sudarsana.it37.com" {
	type master;
	notify yes;
	file "/etc/bind/jarkom/sudarsana.it37.com";
};

zone "pasopati.it37.com" {
	type master;
	notify yes;
	file "/etc/bind/jarkom/pasopati.it37.com";
}; 

zone "rujapala.it37.com" {
	type master;
	notify yes;
	file "/etc/bind/jarkom/rujapala.it37.com";
}; 
```
nano /etc/bind/jarkom/sudarsana.it37.com
lalu tambahkan konfigurasi seperti ini:
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it37.com.  root.sudarsana.it37.com. (
                          2          ; Serial
                      604800         ; Refresh
                       86400         ; Retry
                     2419200         ; Expire
                      604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it37.com.
@       IN      A       192.235.2.2       ; IP Solok
www 	  IN      CNAME   sudarsana.it37.com.
```
nano /etc/bind/jarkom/pasopati.it37.com
lalu tambahkan konfigurasi seperti ini:
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it37.com.   root.pasopati.it37.com. (
                          2          ; Serial
                      604800         ; Refresh
                       86400         ; Retry
                     2419200         ; Expire
                      604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it37.com.
@       IN      A       192.235.1.6       ; IP Kotalingga
www	    IN      CNAME   pasopati.it37.com.
```
nano /etc/bind/jarkom/rajapala.it37.com
lalu tambahkan konfigurasi seperti ini:
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     rujapala.it37.com.   root.rujapala.it37.com. (
                          2          ; Serial
                      604800         ; Refresh
                       86400         ; Retry
                     2419200         ; Expire
                      604800 )       ; Negative Cache TTL
;
@       IN      NS      rujapala.it37.com.
@       IN      A       192.235.1.4       ; IP Tanjungkulai
www	    IN      CNAME   rujapala.it37.com.
```
## No.5
### Pastikan domain-domain tersebut dapat diakses oleh seluruh komputer (client) yang berada di Nusantara.
```
ping sudarsana.it237.com
ping pasopati.it37.com
ping rujapala.it37.com
```

## No.6
### Beberapa daerah memiliki keterbatasan yang menyebabkan hanya dapat mengakses domain secara langsung melalui alamat IP domain tersebut. Karena daerah tersebut tidak diketahui secara spesifik, pastikan semua komputer (client) dapat mengakses domain pasopati.xxxx.com melalui alamat IP Kotalingga (Notes: menggunakan pointer record).

di sriwijaya kita masuk ke /etc/bind/named.conf.local
tambahkan konfigurasi reverse dns:
```
zone "1.235.192.in-addr.arpa" {
	type master;
  	file "/etc/bind/jarkom/1.235.192.in-addr.arpa";
};
```
lalu kita konfigurasi pada file /etc/bind/jarkom/1.235.192.in-addr.arpa seperti dibawah:
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it37.com.   root.pasopati.it37.com. (
                          2          ; Serial
                      604800         ; Refresh
                       86400         ; Retry
                     2419200         ; Expire
                      604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it37.com.
6	IN	NS	pasopati.it37.com.
```

lalu pada setiap client kita install dnsutils
```apt install dnsutils -y```

### No.7
### Akhir-akhir ini seringkali terjadi serangan brainrot ke DNS Server Utama, sebagai tindakan antisipasi kamu diperintahkan untuk membuat DNS Slave di Majapahit untuk semua domain yang sudah dibuat sebelumnya yang mengarah ke Sriwijaya.

pada sriwijaya kita tambahkan konfigurasi pada named.conf.local seperti dibawah:
```
zone "sudarsana.it37.com" {
        type master;
        notify yes;
        also-notify { 192.235.2.3; };
        allow-transfer { 192.235.2.3; };
        file "/etc/bind/jarkom/sudarsana.it37.com";
};

zone "pasopati.it37.com" {
        type master;
        notify yes;
        also-notify { 192.235.2.3; };
        allow-transfer { 192.235.2.3; };
        file "/etc/bind/jarkom/pasopati.it37.com";
};

zone "rujapala.it37.com" {
        type master;
        notify yes;
        also-notify { 192.235.2.3; };
        allow-transfer { 192.235.2.3; }; 
        file "/etc/bind/jarkom/rujapala.it37.com";
};

service bind9 restart
```
pada majapahit kita masuk ke /etc/bind/named.conf.local
lalu menambah konfigurasi dibawah:
```
zone "sudarsana.it37.com" {
	type master;
	masters { 192.235.1.3; };
	file "/etc/bind/jarkom/sudarsana.it37.com";
};

zone "pasopati.it37.com" {
	type master;
	masters { 192.235.1.3; };
	file "/etc/bind/jarkom/pasopati.it37.com";
}; 

zone "rujapala.it37.com" {
	type slave;
	masters { 192.235.1.3; };
	file "/etc/bind/jarkom/rujapala.it37.com";
}; 

zone "1.235.192.in-addr.arpa" {
  	type slave;
	masters { 192.235.1.3; };
  	file "/etc/bind/jarkom/1.235.192.in-addr.arpa";
};

service bind9 restart
```

## No.8
### Kamu juga diperintahkan untuk membuat subdomain khusus melacak kekuatan tersembunyi di Ohio dengan subdomain cakra.sudarsana.xxxx.com yang mengarah ke Bedahulu.

pada sriwijaya menambahkan config di /etc/bind/jarkom/sudarsana.it37.com
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     sudarsana.it37.com.  root.sudarsana.it37.com. (
                          2          ; Serial
                      604800         ; Refresh
                       86400         ; Retry
                     2419200         ; Expire
                      604800 )       ; Negative Cache TTL
;
@       IN      NS      sudarsana.it37.com.
@       IN      A       192.235.2.2       ; IP Solok
@ 	IN      CNAME   sudarsana.it37.com.
cakra   IN      A       192.235.1.5       ; IP Bedahulu

service bind9 restart
```

## No.9
### Karena terjadi serangan DDOS oleh shikanoko nokonoko koshitantan (NUN), sehingga sistem komunikasinya terhalang. Untuk melindungi warga, kita diperlukan untuk membuat sistem peringatan dari siren man oleh Frekuensi Freak dan memasukkannya ke subdomain panah.pasopati.xxxx.com dalam folder panah dan pastikan dapat diakses secara mudah dengan menambahkan alias www.panah.pasopati.xxxx.com dan mendelegasikan subdomain tersebut ke Majapahit dengan alamat IP menuju radar di Kotalingga.

disriwijaya
pertama kita menambahkan config /etc/bind/jarkom/pasopati.it37.com
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     pasopati.it37.com.   root.pasopati.it37.com. (
                          2          ; Serial
                      604800         ; Refresh
                       86400         ; Retry
                     2419200         ; Expire
                      604800 )       ; Negative Cache TTL
;
@       IN      NS      pasopati.it37.com.
@       IN      A       192.235.1.6       ; IP Kotalingga
www     IN      CNAME   pasopati.it37.com.
ns1     IN      A       192.235.2.3       ; IP Majapahit
panah   IN      NS      ns1
```
lalu kita menambahkan config pada /etc/bind/named.conf.local
```
zone "panah.pasopati.it37.com" {
	type master;
	notify yes;
	also-notify { 192.235.2.3; };
	also-transfer { 192.235.2.3; };
	file "/etc/bind/panah/panah.pasopati.it37.com";
};
```
lalu juga config pada /etc/bind/named.conf.options
```
        //dnssec-validation auto;
        allow-query{any;};

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
```
setelah itu mkdir /etc/bind/panah
lalu buat config pada /etc/bind/panah/panah.pasopati.it37.com
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     panah.pasopati.it37.com. root.panah.pasopati.it37.com. (
                        2024050301      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      panah.pasopati.it37.com.
@       IN      A       192.235.1.6     ; IP Kotalingga
www     IN      CNAME   panah.pasopati.it37.com.
```

## No.10
### Markas juga meminta catatan kapan saja meme brain rot akan dijatuhkan, maka buatlah subdomain baru di subdomain panah yaitu log.panah.pasopati.xxxx.com serta aliasnya www.log.panah.pasopati.xxxx.com yang juga mengarah ke Kotalingga.

pertama pada majapahit kita masuk /etc/bind/named.conf.local
mengconfig tambahan :
```
zone "log.panah.pasopati.it37.com" {
        type master;
        file "/etc/bind/panah/log.panah.pasopati.it37.com";
};
```
tambah folder /etc/bind/panah
lalu menambahkan config baru pada /etc/bind/panah/log.panah.pasopati.it37.com
```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     log.panah.pasopati.it37.com. root.log.panah.pasopati.it37.com. (
                        2024050301      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      log.panah.pasopati.it37.com.
@       IN      A       192.235.1.6     ; IP Kotalingga
www     IN      CNAME   log.panah.pasopati.it37.com.
log	    IN	    A	      192.235.1.6	; IP Kotalingga
www.log	IN	    CNAME	  log.panah.pasopati.it37.com
```
