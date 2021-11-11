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
echo ‘# What servers should the DHCP relay forward requests to?
SERVERS="192.186.2.4"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS="" ‘> /etc/default/isc-dhcp-relay
```
Selanjutnya  edit file `/etc/sysctl.conf` dengan command:
```echo 'net.ipv4.ip_forward=1'>/etc/sysctl.conf```

Lalu restart dhcp relaynya dengan perintah
```service isc-dhcp-relay restart```
## 2-6
**Jipangu**

Mengetikan command pada .bashrc sebagai berikut
```
echo 'nameserver 192.168.122.1' >  /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-server -y
```
Edit file `/etc/default/isc-dhcp-server` dengan perintah:
```
echo `INTERFACES="eth0"`> /etc/default/isc-dhcp-server
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
	option domain-name-servers 192.186.2.4, 192.168.122.1;
	default-lease-time 360;
	max-lease-time 7200;
}

subnet 192.186.3.0 netmask 255.255.255.0 {
	range 192.186.3.30 192.186.3.50;
	option routers 192.186.3.1;
	option broadcast-address 192.186.3.255;
	option domain-name-servers 192.186.2.4;
	default-lease-time 720;
	max-lease-time 7200;
}
#nomer 6
host Skypie {
    hardware ethernet 56:c5:59:f7:95:d0;
    fixed-address 192.186.3.69;
}' > /etc/dhcp/dhcpd.conf
```
Kemudian restart dhcp server dengan perintah: `service isc-dhcp-server restart`

**EinesLobby**

Mengetikan command pada .bashrc sebagai berikut
```
echo ‘nameserver 192.168.122.1’ > /etc/resolv.conf
apt-get update 
apt-get install bind9 -y
```
```
echo '
options {
    	directory "/var/cache/bind";

    	// If there is a firewall between you and nameservers you want
    	// to talk to, you may need to fix the firewall to allow multiple
    	// ports to talk.  See http://www.kb.cert.org/vuls/id/800113

    	// If your ISP provided one or more IP addresses for stable
    	// nameservers, you probably want to use them as forwarders.
    	// Uncomment the following block, and insert the addresses replacing
    	// the all-0's placeholder.

    	forwarders {
            	192.168.122.1;
    	};

    	//========================================================================
    	// If BIND logs error messages about the root key being expired,
    	// you will need to update your keys.  See https://www.isc.org/bind-keys
    	//========================================================================
    	//dnssec-validation auto;
    	allow-query{any;};
    	auth-nxdomain no;	# conform to RFC1035
    	listen-on-v6 { any; };
};' > /etc/bind/named.conf.options
```
Setelah itu lakukan restart bind9 dengan perintah `service bind9 restart`
