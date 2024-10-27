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
# Soal 0
Fritz
```bash
echo 'zone "marley.it42.com" { 
        type master; 
        file "/etc/bind/marley/marley.it42.com";
};

zone "eldia.it42.com" {
        type master;
        file "/etc/bind/eldia/eldia.it42.com";
}; ' >> /etc/bind/named.conf.local

mkdir /etc/bind/marley
mkdir /etc/bind/eldia

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     marley.it42.com. root.marley.it42.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      marley.it42.com.
@       IN      A       10.84.1.2     ; IP Annie' > /etc/bind/marley/marley.it42.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     eldia.it42.com. root.eldia.it42.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      eldia.it42.com.
@       IN      A       10.84.2.2     ; IP Armin' > /etc/bind/eldia/eldia.it42.com

service bind9 restart
```
# Soal 1-5
```bash
echo 'INTERFACESv4="eth0"' > /etc/default/isc-dhcp-server

echo 'subnet 10.84.1.0 netmask 255.255.255.0 {
#Range IP Marley
        range 10.84.1.05 10.84.1.25;
        range 10.84.1.50 10.84.1.100;
        option routers 10.84.1.1;
# DNS Server Fritz
        option broadcast-address 10.84.1.255;
        option domain-name-servers 10.84.4.2;
# Durasi DHCP Marley
        default-lease-time 1800;
        max-lease-time 5220;
}

subnet 10.84.2.0 netmask 255.255.255.0 {
# Range IP Eldia
        range 10.84.2.09 10.84.2.27;
        range 10.84.2.81 10.84.2.243;
        option routers 10.84.2.1;
# DNS Server Fritz
        option broadcast-address 10.84.2.255;
        option domain-name-servers 10.84.4.2;
# Durasi DHCP Eldia
        default-lease-time 360;
        max-lease-time 5220;
}

subnet 10.84.3.0 netmask 255.255.255.0 {
        option routers 10.84.3.1;
}

subnet 10.84.4.0 netmask 255.255.255.0 {
        option routers 10.84.4.1;
} ' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart

```
# Soal 6
Install php 7.2.24 terlebih dahulu menggunakan script ini

```bash
# PHP
apt update
apt install -y php php-fpm
```
Setelah itu pada php worker jalankan script ini
```bash
# Soal 6: Jalankan pada setiap PHP worker 
mkdir -p /var/www/eldia.it42.com

wget --no-check-certificate 'https://drive.google.com/uc?export=download&id=1TvebIeMQjRjFURKVtA32lO9aL7U2msd6' -O /root/bangsaEldia.zip
unzip /root/bangsaEldia.zip -d /var/www/eldia.it42.com
rm -rf /root/bangsaEldia.zip

echo '
server {

        listen 80;

        root /var/www/eldia.it42.com;

        index index.php index.html index.htm;
        server_name _;

        location / {
                        try_files $uri $uri/ /index.php?$query_string;
        }

        # pass PHP scripts to FastCGI server
        location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
        }

location ~ /\.ht {
                        deny all;
        }

        error_log /var/log/nginx/jarkom_error.log;
        access_log /var/log/nginx/jarkom_access.log;
 }' > /etc/nginx/sites-available/eldia.it42.com

ln -s /etc/nginx/sites-available/eldia.it42.com /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service php7.2-fpm start
service php7.2-fpm restart
service nginx restart
nginx -t
```

# Soal 7
```bash
# Pada Fritz
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     eldia.it42.com. root.eldia.it42.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      eldia.it42.com.
@       IN      A       10.84.3.3     ; IP Colossal' > /etc/bind/eldia/eldia.it42.com

service bind9 restart

# Pada Colossal
echo '
 upstream myweb  {
        server 10.84.2.2; #IP Armin
        server 10.84.2.3; #IP Eren
        server 10.84.2.4; #IP Mikasa
 }

 server {
        listen 80;
        server_name eldia.it42.com;

        location / {
        proxy_pass http://myweb;
        }
 }' > /etc/nginx/sites-available/lb-php

ln -s /etc/nginx/sites-available/lb-php /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
nginx -t
```

# Soal 8
```bash
# Collosal
echo '
 upstream myweb  {
#    hash $request_uri consistent;
#    least_conn;
#    ip_hash;
        server 10.846.2.2; #IP Armin
        server 10.846.2.3; #IP Eren
        server 10.846.2.4; #IP Mikasa
 }

 server {
        listen 80;
        server_name eldia.it42.com;

        location / {
        proxy_pass http://myweb;
        }
 }' > /etc/nginx/sites-available/lb-php

ln -s /etc/nginx/sites-available/lb-php /etc/nginx/sites-enabled
rm -rf /etc/nginx/sites-enabled/default

service nginx restart
nginx -t
```

lakukan setiap kali algoritma load balancer diubah pada client
```bash
ab -n 1000 -c 75 http://eldia.it42.com/
```
# Soal 9
Ketikan ini pada stiap worker untuk mengetest
```bash
service nginx stop
```
```bash
#Pada Erwin lakukan setiap kali jumlah worker diubah
ab -n 1000 -c 10 http://eldia.it42.com/
```
