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

## Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server.
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
Kemudian edit file `/etc/default/isc-dhcp-relay` dengam perimtah:
```
echo ‘# What servers should the DHCP relay forward requests to?
SERVERS="192.186.2.4"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS="" ‘> /etc/default/isc-dhcp-relay
```
Selanjutnya  edit file `/etc/sysctl.conf` dengan command:
```echo 'net.ipv4.ip_forward=1'>/etc/sysctl.conf```

Lalu restart dhcp relaynya dengan commmand 
```service isc-dhcp-relay restart```
