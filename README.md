# Jarkom-Modul-3-C05-2021
Pengerjaan Soal Shift 3 Jaringan Komputer 2021/2022 ( Modul DHCP, Proxy )         
.                                                           
Ghifari Astaudi U. (05111940000012)                             
Ricky Supriyanto (05111940000036)                                       
Cahyadesthian R. W. (05111940000156)                                   
.                                                               
# Praktikum Modul 3
### 📅 7-9 November 2021       

## KONFIGURASI NODE

**Foosha** 
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.186.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.186.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
  address 192.186.3.1
	netmask 255.255.255.0
```

**Longutown**
```
auto eth0
iface eth0 inet static
	address 192.186.1.2
	netmask 255.255.255.0
	gateway 192.186.1.1

Alabasta
auto eth0
iface eth0 inet static
	address 192.186.1.3
	netmask 255.255.255.0
	gateway 192.186.1.1
```

**EinesLobby**
```
auto eth0
iface eth0 inet static
	address 192.186.2.2
	netmask 255.255.255.0
	gateway 192.186.2.1
```

**Water7**
```
auto eth0
iface eth0 inet static
	address 192.186.2.3
	netmask 255.255.255.0
	gateway 192.186.2.1
```

**Jipangu**
```
auto eth0
iface eth0 inet static
	address 192.186.2.4
	netmask 255.255.255.0
	gateway 192.186.2.1
```

**Skypie**
```
auto eth0
iface eth0 inet static
	address 192.186.3.2
	netmask 255.255.255.0
	gateway 192.186.3.1
```

**TottoLand**
```
auto eth0
iface eth0 inet static
	address 192.186.3.3
	netmask 255.255.255.0
	gateway 192.186.3.1
```

## 1
### JAWAB
**Foosha**

Mengetikan command pada .bashrc sebagai berikut
```
ip a
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.186.0.0/16
cat /etc/resolv.conf
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-relay -y
```
Kemudian edit file `/etc/default/isc-dhcp-relay` dengan perimtah:
```
echo '# What servers should the DHCP relay forward requests to?
SERVERS="192.186.2.4"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS="" '> /etc/default/isc-dhcp-relay
```
Selanjutnya  edit file `/etc/sysctl.conf` dengan command:
```echo 'net.ipv4.ip_forward=1'>/etc/sysctl.conf```

Lalu restart dhcp relaynya dengan perintah
```service isc-dhcp-relay restart```
## 2-7
**Jipangu**

Mengetikan command pada .bashrc sebagai berikut
```
echo 'nameserver 192.168.122.1' >  /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-server -y
```
Edit file `/etc/default/isc-dhcp-server` dengan perintah:
```
echo 'INTERFACES="eth0"'> /etc/default/isc-dhcp-server
```
Edit file  `/etc/dhcp/dhcpd.conf` dengan perintah
```
echo '
subnet 192.186.2.0 netmask 255.255.255.0 {
}

subnet 192.186.1.0 netmask 255.255.255.0 {
	range 192.186.1.20 192.186.1.99;
	range 192.186.1.150 192.186.1.169;
	option routers 192.186.1.1;
	option broadcast-address 192.186.1.255;
	option domain-name-servers 192.186.2.4, 192.186.2.2;
	default-lease-time 360;
	max-lease-time 7200;
}

subnet 192.186.3.0 netmask 255.255.255.0 {
	range 192.186.3.30 192.186.3.50;
	option routers 192.186.3.1;
	option broadcast-address 192.186.3.255;
	option domain-name-servers 192.186.2.4, 192.186.2.2;
	default-lease-time 720;
	max-lease-time 7200;
}
#nomer 7
host Skypie {
    hardware ethernet 56:c5:59:f7:95:d0; #'hwaddress_milik_Skypie'
    fixed-address 192.186.3.69;
}' > /etc/dhcp/dhcpd.conf
```
Kemudian restart dhcp server dengan perintah: `service isc-dhcp-server restart`

**EinesLobby**

Mengetikan command pada .bashrc sebagai berikut
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update 
apt-get install bind9 -y
```
Edit file `/etc/bind/named.conf.options` dan uncomment pada bagian forwarders seperti berikut
```
forwarders {
	192.168.122.1;
};
```
Lalu comment pada bagian `// dnssec-validation auto;` dan tambahkan `allow-query{any;};`

Setelah itu lakukan restart bind9 dengan perintah `service bind9 restart`

**Longutown, Alabasta TottoLand dan Skypie**

Edit file `/etc/network/interfaces` dengan comment atau hapus konfigurasi yang lama (konfigurasi IP statis) dan tambahkan:
```
auto eth0
iface eth0 inet dhcp
```
**Skypie**
Edit file `/etc/network/interfaces` dengan menambahkan
```
hwaddress ether 56:c5:59:f7:95:d0; #'hwaddress_milik_Skypie'
```
Kemudian restart semua client `(Longutown, Alabasta TottoLand dan Skypie)` pada GNS3

Lalu periksa semua client dengan mengetikkan command `ip a`

## 8
**Water7**

Mengetikan command pada .bashrc sebagai berikut
```
echo 'nameserver 192.168.122.1' >  /etc/resolv.conf
apt-get update
apt-get install squid -y
apt-get install apache2-utils -y
```
Backup file konfigurasi default yang disediakan Squid dengan perintah
```
mv /etc/squid/squid.conf /etc/squid/squid.conf.bak
```
Kemudian buat konfigurasi Squid baru dengan file `/etc/squid/squid.conf` dan tambahkan:
```
http_port 5000
visible_hostname jualbelikapal.c05.com
http_access allow all
```
Kemudian restart squid dengan perintah `service squid restart`
**EinesLobby**

Buat zone jualbelikapal.c05.com pada `/etc/bind/named.conf.local`
```
echo 'zone "jualbelikapal.c05.com" {
        type master;
        allow-transfer { 192.186.2.3; };
        file "/etc/bind/kaizoku/jualbelikapal.c05.com";
}; '> /etc/bind/named.conf.local
```
Buat directory kaizoku dengan command `mkdir /etc/bind/kaizoku'

Edit file `/etc/bind/kaizoku/jualbelikapal.c05.com` menjadi:
```
echo -e '
$TTL     604800
@          IN          SOA      jualbelikapal.c05.com. root.jualbelikapal.c05.com. (
                                2021102601     ; Serial
                                604800              ; Refresh
                                86400                 ;Retry
                                2419200            ; Expire
                                604800 )            ; Negative Cache TTL
;
@          IN          NS             jualbelikapal.c05.com.
@          IN          A                192.186.2.3      ; IP Skypie
www        IN          CNAME            jualbelikapal.c05.com. '>/etc/bind/kaizoku/jualbelikapal.c05.com
```
Kemudian restart dengan command `service bind9 restart` 
**Longutown**
Instalasi beberapa paket pada file script.sh dengan petintah
```
apt-get update
apt-get install lynx -y
apt-get install install speedtest-cli -y
```
Kemudian aktifkan konfigurasi proxynya dengan perintah
```
export http_proxy="http://192.186.2.3:5000"
```
Kemudian `lynx http://google.com`
## 9
Buat username `luffybelikapalc05` dengan password `luffy_c05` dan username `zorobelikapalc05` dengan password `zoro_c05` dengan enkripsi MD5
```
htpasswd -c -m /etc/squid/passwd luffybelikapalc05
htpasswd -m /etc/squid/passwd zorobelikapalc05
```
**Keterangan:**

-c : Buat new passwdfile. Jika passwdfile sudah ada maka akan membuat file password baru.

-m : Menggunakan enkripsi MD5 untuk password.

Edit file pada `/etc/squid/squid.conf` menjadi:
```
http_port 5000
visible_hostname jualbelikapal.c05.com
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
```
Lalu restart squid dengan perintah
```service squid restart```
**Longuetown**

Kemudian `lynx http://google.com`

## 10
**Water7**
Edit file `/etc/squid/acl.conf` dengan perintah
```
echo 'acl JUAL_BELI_KAPAL1 time MTWH 07:00-11:00
acl JUAL_BELI_KAPAL2 time TWHF 17:00-23:59
acl JUAL_BELI_KAPAL3 time WHFA 00:00-03:00'>/etc/squid/acl.conf
```
Edit file pada `/etc/squid/squid.conf` menjadi:
```
echo 'include /etc/squid/acl.conf
http_port 5000
visible_hostname jualbelikapal.c05.com
auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
http_access deny !JUAL_BELI_KAPAL1 !JUAL_BELI_KAPAL2 !JUAL_BELI_KAPAL3
acl USERS proxy_auth REQUIRED
http_access allow USERS
'>/etc/squid/squid.conf
```
Lalu restart squid dengan perintah `service squid restart`

**Longuetown**

Kemudian `lynx http://google.com`

## 11
**EniesLobby**
Buat domain super.franky.C05.com pada file `/etc/bind/named.conf.local`
```
zone "super.franky.C05.com" {
        type master;
        allow-transfer { 192.186.3.69; };
        file "/etc/bind/kaizoku/super.franky.C05.com";
};
```

Bash command.sh yang berisi ```mkdir /etc/bind/kaizoku```
Bash super-franky-C05.sh yang berisi					
<img src="https://github.com/Cahyadesthian-156/Jarkom-Modul-3-C05-2021/blob/main/screenshot/nomer11_1.png" width="800">  			
kemudian jalankan command ```service bind9 restart```

**Pada Skypie**
```Bash command.sh```																								
<img src="https://github.com/Cahyadesthian-156/Jarkom-Modul-3-C05-2021/blob/main/screenshot/nomer11_2.png" width="800">  
```Bash super-franky.sh	```																																
<img src="https://github.com/Cahyadesthian-156/Jarkom-Modul-3-C05-2021/blob/main/screenshot/nomer11_3.png" width="800">  		
kemudian						
```cd /etc/apache2/sites-available```																				
```a2ensite super.franky.C05.com.conf```																					
```service apache2 restart```																											

**Pada Water7**
Bash script3.sh
```
echo '
#nameserver 192.168.122.1
nameserver 192.186.2.2
nameserver 192.186.3.69
' > /etc/resolv.conf
```
```
echo '#include /etc/squid/acl.conf
include /etc/squid/acl-bandwidth.conf

http_port 5000
visible_hostname jualbelikapal.c05.com

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Login
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on

#acl lan src 192.168.3.69
#acl BLACKLISTS dstdomain "/etc/squid/restrict-sites.acl"
#deny_info http://super.franky.C05.com:5000 lan
#deny_info  http://super.franky.C05.com:5000 BLACKLISTS
#http_reply_access deny BLACKLISTS lan
#http_reply_access deny BLACKLISTS
acl USERS proxy_auth REQUIRED
acl google dstdomain google.com
http_access deny google
deny_info http://super.franky.C05.com/ google
http_access allow USERS
http_access deny all


#http_access deny !JUAL_BELI_KAPAL1 !JUAL_BELI_KAPAL2 !JUAL_BELI_KAPAL3
'>/etc/squid/squid.conf
```

**Pada Loguetown**
```Lynx google.com```				
```Lynx super.franky.c05```
<img src="https://github.com/Cahyadesthian-156/Jarkom-Modul-3-C05-2021/blob/main/screenshot/nomer11_4.png" width="800">  




