# jarkom-modul-3-IT42-2024

## Network Configuration
1. Paradis
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.84.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.84.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.84.3.1
	netmask 255.255.255.0

auto eth4
iface eth4 inet static
	address 10.84.4.1
	netmask 255.255.255.0
```

2. Annie
```
auto eth0
iface eth0 inet static
	address 10.84.1.2
	netmask 255.255.255.0
	gateway 10.84.1.1
```
3. Berdholdt
```
auto eth0
iface eth0 inet static
    address 10.84.1.3
    netmask 255.255.255.0
    gateway 10.84.1.1
```

4. Reiner
```
auto eth0
iface eth0 inet static
    address 10.84.1.4
    netmask 255.255.255.0
    gateway 10.84.1.1
```

5. Armin
```
auto eth0
iface eth0 inet static
    address 10.84.2.2
    netmask 255.255.255.0
    gateway 10.84.2.1
```

6. Eren
```
auto eth0
iface eth0 inet static
    address 10.84.2.3
    netmask 255.255.255.0
    gateway 10.84.2.1
```

7. Mikasa
```
auto eth0
iface eth0 inet static
    address 10.84.2.4
    netmask 255.255.255.0
    gateway 10.84.2.1
```

8. Beast
```
auto eth0
iface eth0 inet static
    address 10.84.3.2
    netmask 255.255.255.0
    gateway 10.84.3.1
```

9. Colossal
```
auto eth0
iface eth0 inet static
    address 10.84.3.3
    netmask 255.255.255.0
    gateway 10.84.3.1
```

10. Warhammer
```
auto eth0
iface eth0 inet static
    address 10.84.3.4
    netmask 255.255.255.0
    gateway 10.84.3.1
```

11. Fritz
```
auto eth0
iface eth0 inet static
    address 10.84.4.2
    netmask 255.255.255.0
    gateway 10.84.4.1
```

12. Tybur
```
auto eth0
iface eth0 inet static
    address 10.84.4.3
    netmask 255.255.255.0
    gateway 10.84.4.1
```

13. Zeke & Erwin
```
auto eth0
iface eth0 inet dhcp
```

### .bashrc
1. DHCP Relay (Paradis)
```bash
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.84.0.0/16
apt-get update
apt install isc-dhcp-relay -y
```

2. DHCP Server (Tybur)
```bash
echo 'nameserver 10.84.4.2' > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-server -y
```

3. DNS Server (Fritz)
```bash
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install bind9 -y

echo 'options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1;
        };

        // dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;
        listen-on-v6 { any; };
}; ' >/etc/bind/named.conf.options

service bind9 restart
```

4. PHP Worker (Armin, Eren, Mikasa)
```bash
echo 'nameserver 10.84.4.2' > /etc/resolv.conf
apt-get update
apt-get install lynx -y
apt-get install wget -y
apt-get install unzip -y
apt-get install nginx -y
apt install software-properties-common -y
apt install php7.3 -y
apt install php7.3-fpm -y
```

5. PHP Load Balancer (Colossal)
```bash
echo 'nameserver 10.84.4.2' > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
apt-get install apache2-utils -y
apt-get install nginx -y
apt-get install lynx -y

service nginx start
```

6. Database Server (Warhammer)
```bash
echo 'nameserver 10.84.4.2' > /etc/resolv.conf
apt-get update
apt-get install mariadb-server -y

service mysql start
```

7. Laravel Worker (Annie, Berdholdt, Reiner)
```bash
echo 'nameserver 10.84.4.2' > /etc/resolv.conf
apt-get update
apt-get install lynx -y
apt-get install mariadb-client -y
```

8. Client (Zeke, Erwin)
```bash
apt update
apt install lynx -y
apt install htop -y
apt install apache2-utils -y
apt-get install jq -y
```

