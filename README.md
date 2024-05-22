# Jarkom-Modul-3-IT14-2024

| No. | Nama Anggota | NRP |
| :---         |     :---:      |          ---: |
| 1.   | Rizki Ramadhani     | 5027221013    |
| git diff     | git diff       | git diff      |

## Config

* Arrakis (DHCP Relay)
```
auto eth0
iface eth0 inet dhcp
up iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 192.240.0.0/16

auto eth1
iface eth1 inet static
address 192.240.1.0
netmask 255.255.255.0

auto eth2
iface eth2 inet static
address 192.240.2.0
netmask 255.255.255.0

auto eth3
iface eth3 inet static
address 192.240.3.0
netmask 255.255.255.0

auto eth4
iface eth4 inet static
address 192.240.4.0
netmask 255.255.255.0
```
* Mohiam (DHCP Server)
```
auto eth0
iface eth0 inet static
address 192.240.3.1
netmask 255.255.255.0
gateway 192.240.3.0
```
* Irulan (DNS Server)
```
auto eth0
iface eth0 inet static
address 192.240.3.2
netmask 255.255.255.0
gateway 192.240.3.0
```
* Chani (Database Server)
```
auto eth0
iface eth0 inet static
address 192.240.4.1
netmask 255.255.255.0
gateway 192.240.4.0
```
* Stilgar (Load Balancer)
```
auto eth0
iface eth0 inet static
address 192.240.4.2
netmask 255.255.255.0
gateway 192.240.4.0
```
* Vladimir (PHP Worker)
```
auto eth0
iface eth0 inet static
address 192.240.1.2
netmask 255.255.255.0
gateway 192.240.1.0
```
* Rabban (PHP Worker)
```
auto eth0
iface eth0 inet static
address 192.240.1.3
netmask 255.255.255.0
gateway 192.240.1.0
```
* Feyd (PHP Worker)
```
auto eth0
iface eth0 inet static
address 192.240.1.3
netmask 255.255.255.0
gateway 192.240.1.0
```
Leto (Laravel Worker)
```
auto eth0
iface eth0 inet static
address 192.240.2.2
netmask 255.255.255.0
gateway 192.240.2.0
```
* Duncan (Laravel Worker)
```
auto eth0
iface eth0 inet static
address 192.240.2.3
netmask 255.255.255.0
gateway 192.240.2.0
```
* Jessica (Laravel Worker)
```
auto eth0
iface eth0 inet static
address 192.240.2.4
netmask 255.255.255.0
gateway 192.240.2.0
```
* Paul (Client)
```
auto eth0
iface eth0 inet dhcp
```
* Dmitri (Fixed Client)
```
auto eth0
iface eth0 inet dhcp
hwaddress ether 92:6a:4b:8f:b3:cf
```
## Bash Files
* Arrakis (DHCP Relay)
```
apt-get update
apt-get install isc-dhcp-relay -y
```
* Mohiam (DHCP Server)
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-server -y
dhcpd --version

echo INTERFACES="eth0" > /etc/default/isc-dhcp-serverm
```
* Irulan (DNS Server)
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
apt-get update
apt-get install bind9 -y
```
* Chani (Database Server)
```
echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install mariadb-server -y
service mysql start
```
* Stilgar (Load Balancer)
```
echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install apache2-utils php7.3 php-fpm -y
apt-get install nginx -y
apt-get install lynx -y
```
* Vladimir, Rabban, Feyd (PHP Worker)
```
echo nameserver 192.240.3.2 > /etc/resolv.conf

apt-get update
apt-get install nginx -y
apt-get install lynx -y
apt-get install php php-fpm -y
apt-get install wget -y
apt-get install unzip -y
service nginx start
service php7.3-fpm start

wget -O '/var/www/harkonen.it14.com' 'https://docs.google.com/uc?export=download&id=1lmnXJUbyx1JDt2OA5z_1dEowxozfkn30'
unzip -o /var/www/harkonen.it14.com -d /var/www/
rm /var/www/harkonen.it14.com
mv /var/www/modul-3 /var/www/harkonen.it14.com
```
* Leto, Duncan, Jessica (Laravel Worker)
```
echo nameserver 192.168.122.1 > /etc/resolv.conf

apt-get update
apt-get install mariadb-client -y
apt-get install lynx -y
apt-get install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.li$apt-get update
apt-get install php8.0-mbstring php8.0-xml php8.0-cli php8.0-common php8.0-intl php8.0-opcache php8.0-readline php8.0-mysql php8.0-fpm php8.0-curl $apt-get install nginx -y
wget https://getcomposer.org/download/2.0.13/composer.phar
chmod +x composer.phar
mv composer.phar /usr/bin/composer
apt-get install git -y
git clone https://github.com/martuafernando/laravel-praktikum-jarkom /var/www/laravel-praktikum-jarkom
composer update
composer install
```

![Screenshot 2024-05-22 121927](https://github.com/rzkirmdhani/Jarkom-Modul-3-IT14-2024/assets/141987387/67f3a579-fe66-4294-a8e8-2ccb15439845)

## No.1
Melakukan semua config diatas serta membuat file bashrc pada root Lalu arahkan semua client agar menggunakan konfigurasi dhcp.
* Irulan.sh
```
echo 'zone "atreides.it14.com" {
        type master;
        file "/etc/bind/jarkom/atreides.it14.com";
};

zone "harkonen.it14.com" {
        type master;
        file "/etc/bind/jarkom/harkonen.it14.com";
};' > /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     atreides.it14.com. root.atreides.it14.com. (
                        2024051601      ; Serial
                        604800          ; Refresh
                        86400           ; Retry
                        2419200         ; Expire
                        604800 )        ; Negative Cache TTL
;
@               IN      NS      atreides.it14.com.
@               IN      A       192.240.2.2 ; IP Leto Laravel Workerr' > /etc/bind/jarkom/atreides.it14.com

echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     harkonen.it14.com.  harkonen.it14.com.  (
                        2024051601      ; Serial
                        604800          ; Refresh
                        86400           ; Retry
                        2419200         ; Expire
                        604800 )        ; Negative Cache TTL
;
@               IN      NS      harkonen.it14.com.
@               IN      A       192.240.1.2 ; IP Vladimir PHP Worker' > /etc/bind/jarkom/harkonen.it14.com

echo 'options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1;
        };

        // dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
}; ' >/etc/bind/named.conf.options

service bind9 restart
```
Test.
```
ping atreides.it14.com
ping harkonen.it14.com
```
## No.2
Client yang melalui House Harkonen mendapatkan range IP dari [prefix IP].1.14 - [prefix IP].1.28 dan [prefix IP].1.49 - [prefix IP].1.70.
* Arrakis.sh
```
apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start

relay="SERVERS=\"192.240.3.1\" 
INTERFACES=\"eth1 eth2 eth3 eth4\"
OPTIONS=\"\"
"
echo "$relay" > /etc/default/isc-dhcp-relay

echo net.ipv4.ip_forward=1 > /etc/sysctl.conf

service isc-dhcp-relay restart
```
* Mohiam.sh
```
subnet 192.240.1.0 netmask 255.255.255.0 {
    range 192.240.1.14 192.240.1.28;
    range 192.240.1.49 192.240.1.70;
    option routers 192.240.1.0;
    option broadcast-address 192.240.1.255;
    default-lease-time 300;
    max-lease-time 5220;
}
```
## No.3
Client yang melalui House Atreides mendapatkan range IP dari [prefix IP].2.15 - [prefix IP].2.25 dan [prefix IP].2 .200 - [prefix IP].2.210.
* Mohiam.ah
```
subnet 192.240.2.0 netmask 255.255.255.0 {
    range 192.240.2.15 192.240.2.25;
    range 192.240.2.200 192.240.2.210;
    option routers 192.240.2.0;
    option broadcast-address 192.240.2.255;
    default-lease-time 1200;
    max-lease-time 5220;
```
## No.4
Client mendapatkan DNS dari Princess Irulan dan dapat terhubung dengan internet melalui DNS tersebut.
* Mohiam.sh
```
subnet 192.240.1.0 netmask 255.255.255.0 {
    range 192.240.1.14 192.240.1.28;
    range 192.240.1.49 192.240.1.70;
    option routers 192.240.1.0;
    option broadcast-address 192.240.1.255;
    option domain-name-servers 192.240.3.2; #Gunakan IP Irulan
}

subnet 192.240.2.0 netmask 255.255.255.0 {
    range 192.240.2.15 192.240.2.25;
    range 192.240.2.200 192.240.2.210;
    option routers 192.240.2.0;
    option broadcast-address 192.240.2.255;
    option domain-name-servers 192.240.3.2; #IP Irulan
}
```
* Irulan.sh
```
echo 'options {
        directory "/var/cache/bind";

        forwarders {
                192.168.122.1;
        };

        // dnssec-validation auto;
        allow-query{any;};
        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
}; ' >/etc/bind/named.conf.options
```
Test.
```
ping google.com
ping atreides.it14.com
ping harkonen.it14.com
```
## No.5
Durasi DHCP server meminjamkan alamat IP kepada Client yang melalui House Harkonen selama 5 menit sedangkan pada client yang melalui House Atreides selama 20 menit. Dengan waktu maksimal dialokasikan untuk peminjaman alamat IP selama 87 menit.
* Mohiam.sh
```
subnet 192.240.1.0 netmask 255.255.255.0 {
    range 192.240.1.14 192.240.1.28;
    range 192.240.1.49 192.240.1.70;
    option routers 192.240.1.0;
    option broadcast-address 192.240.1.255;
    option domain-name-servers 192.240.3.2;
    default-lease-time 300; #5 Menit
    max-lease-time 5220; #87 Menit maksimal
}

subnet 192.240.2.0 netmask 255.255.255.0 {
    range 192.240.2.15 192.240.2.25;
    range 192.240.2.200 192.240.2.210;
    option routers 192.240.2.0;
    option broadcast-address 192.240.2.255;
    option domain-name-servers 192.240.3.2;
    default-lease-time 1200; #20 Menit
    max-lease-time 5220; #87 Menit maksimal
}
```
