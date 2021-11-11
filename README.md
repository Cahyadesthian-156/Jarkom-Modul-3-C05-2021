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


