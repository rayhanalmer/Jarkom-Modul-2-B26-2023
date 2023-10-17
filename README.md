# Jarkom-Modul-2-B26-2023

Anggota Kelompok 

|   Nama                             | NRP      |
| ------                             | ------   |
|  Nur Azizah                        |5025211252|
|  Rayhan Almer Kusumah              |5025211115|
|  Faiz Haq Noviandra Ciptadi Putra  |5025211132|

## Setting Config
Sebelum memulai pertama-tama kita harus mengatur configurasi tiap node sebagai berikut
Pandudewanata
```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 192.191.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 192.191.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
      address 192.191.3.1
      netmask 255.255.255.0
```
Yudhistira
```
auto eth0
iface eth0 inet static
	address 192.191.1.2
	netmask 255.255.255.0
	gateway 192.191.1.1
```
Werkudara
```
auto eth0
iface eth0 inet static
	address 192.191.1.3
	netmask 255.255.255.0
	gateway 192.191.1.1
```
Nakula
```
auto eth0
iface eth0 inet static
	address 192.191.2.2
	netmask 255.255.255.0
	gateway 192.191.2.1
```
Sadewa
```
auto eth0
iface eth0 inet static
	address 192.191.2.3
	netmask 255.255.255.0
	gateway 192.191.2.1
```
Abimanyu
```
auto eth0
iface eth0 inet static
	address 192.191.3.2
	netmask 255.255.255.0
	gateway 192.191.3.1
```
Prabukusuma
```
auto eth0
iface eth0 inet static
	address 192.191.3.3
	netmask 255.255.255.0
	gateway 192.191.3.1
```
Wisanggeni
```
auto eth0
iface eth0 inet static
	address 192.191.3.4
	netmask 255.255.255.0
	gateway 192.191.3.1
```
Arjuna
```
auto eth0
iface eth0 inet static
	address 192.191.3.5
	netmask 255.255.255.0
	gateway 192.191.3.1
```

## IP Nodes
```
Pandudewanata     : 192.191.1.1 (Switch 1)
Yudhistira        : 192.191.1.2
Werkudara         : 192.191.1.3
Pandudewanata     : 192.191.2.1 (Switch 2)
Nakula            : 192.191.2.2
Sadewa            : 192.191.2.3
Pandudewanata     : 192.191.3.1 (Switch 3)
Abimanyu          : 192.191.3.2
Prabukusuma       : 192.191.3.3
Wisanggeni        : 192.191.3.4
Arjuna            : 192.191.3.5
```

## Soal 1
Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni

### Jawaban
Kita tinggal menjalankan setting sesuai dengan instruksi diatas kemudian kita tes pada node client (Nakula/Sadewa) dengan ``` ping google.com -c 5 ```


## Soal 2
Buatlah website utama dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok

### Jawaban
Yudhistira
```
echo 'zone "arjuna.b26.com" {
        type master;
        file "/etc/bind/jarkom/arjuna.b26.com";
        allow-transfer { 192.191.3.5; }; // IP Arjuna
};' > /etc/bind9/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/arjuna.b26.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     arjuna.b26.com. root.arjuna.b26.com. (
                        2023101001      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      arjuna.b26.com.
@       IN      A       192.191.1.2     ; IP Yudhistira
www     IN      CNAME   arjuna.b26.com.' > /etc/bind/jarkom/arjuna.b26.com

service bind9 restart
```
Arjuna
```
echo -e '
nameserver 192.173.1.2 # IP Yudhistira
nameserver 192.173.2.2 # IP Werkudara
nameserver 192.168.122.1
' > /etc/resolv.conf

ping arjuna.b26.com -c 5
ping www.arjuna.b26.com -c 5
```

## Soal 3
Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

### Jawaban
Yudhistira
```
echo 'zone "arjuna.b26.com" {
        type master;
        file "/etc/bind/jarkom/arjuna.b26.com";
        allow-transfer { 192.191.3.5; }; // IP Arjuna
};

zone "abimanyu.b26.com" {
        type master;
        file "/etc/bind/jarkom/abimanyu.b26.com";
        allow-transfer { 192.191.3.2; }; // IP Abimanyu
};' > /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/jarkom/abimanyu.b26.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.b26.com. root.abimanyu.b26.com. (
                        2023101001      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.b26.com.
@       IN      A       192.191.1.2     ; IP Yudhistira
www     IN      CNAME   abimanyu.b26.com.' > /etc/bind/jarkom/abimanyu.b26.com

service bind9 restart
```
Abimanyu
Kita setup nameserver dulu ke node Yudhistira
```
echo -e '
nameserver 192.173.1.2 # IP Yudhistira
' > /etc/resolv.conf
```
Lalu kita coba ping
```
ping abimanyu.b26.com -c 5
ping www.abimanyu.b26.com -c 5
```

## Soal 4
Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu

### Jawaban
Yudhistira
```
echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.b26.com. root.abimanyu.b26.com. (
                        2023101001      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.b26.com.
@       IN      A       192.173.1.2     ; IP Yudhistira
www     IN      CNAME   abimanyu.b26.com.
parikesit IN    A       192.173.3.2     ; IP Abimanyu' > /etc/bind/jarkom/abimanyu.b26.com

service bind9 restart
```

## Soal 5
Buat juga reverse domain untuk domain utama. (di Abimanyu saja)

### Jawaban
Yudhistira
```
echo 'zone "3.191.192.in-addr.arpa" {
        type master;
        file "/etc/bind/jarkom/3.191.192.in-addr.arpa";
};' > /etc/bind/named.conf.local

cp /etc/bind/db.local /etc/bind/jarkom/3.191.192.in-addr.arpa

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.b26.com. root.abimanyu.b26.com. (
                        2003101001      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
3.191.192.in-addr.arpa. IN      NS      abimanyu.b26.com.
2                       IN      PTR     abimanyu.b26.com.
5                       IN      PTR     arjuna.b26.com.' > /etc/bind/jarkom/3.191.192.in-addr.arpa

service bind9 restart
```

## Soal 6
Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama

### Jawaban
Yudhistira
```
echo 'zone "arjuna.b26.com" {
        type master;
        file "/etc/bind/jarkom/arjuna.b26.com";
        allow-transfer { 192.191.1.3; }; // IP Werkudara
};

zone "abimanyu.b26.com" {
        type master;
        notify yes;
        also-notify { 192.191.1.3; }; // IP Werkudara
        allow-transfer { 192.191.1.3; }; // IP Werkudara
        file "/etc/bind/jarkom/abimanyu.b26.com";
};

zone "3.191.192.in-addr.arpa" {
        type master;
        file "/etc/bind/jarkom/3.191.192.in-addr.arpa";
};' > /etc/bind/named.conf.local

// Jangan lupa restart lalu stop bind9, untuk melakukan testing slave

service bind9 restart
service bind9 stop
```
Werkudara
```
echo 'zone "abimanyu.b26.com" {
    type slave;
    masters { 192.191.1.2; }; // IP Yudhistira
    file "/var/lib/bind/abimanyu.b26.com";
};' >> /etc/bind/named.conf.local

service bind9 restart
```
Abimanyu
Lalu kita coba ping
```
ping abimanyu.b26.com -c 5
ping www.abimanyu.b26.com -c 5
```

## No 7
Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda

### Jawaban
Yudhistira
```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.b26.com. root.abimanyu.b26.com. (
                        2023101001      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.b26.com.
@       IN      A       192.191.1.2     ; IP Yudhistira
www     IN      CNAME   abimanyu.b26.com.
parikesit IN    A       192.191.3.2     ; IP Abimanyu
ns1     IN      A       192.191.1.3     ; IP Werkudara
baratayuda IN   NS      ns1' > /etc/bind/jarkom/abimanyu.b26.com

echo "options {
    directory \"/var/cache/bind\";

    // If there is a firewall between you and nameservers you want
    // to talk to, you may need to fix the firewall to allow multiple
    // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

    // If your ISP provided one or more IP addresses for stable
    // nameservers, you probably want to use them as forwarders.
    // Uncomment the following block, and insert the addresses replacing
    // the all-0's placeholder.

    // forwarders {
    //      0.0.0.0;
    // };

    //========================================================================
    // If BIND logs error messages about the root key being expired,
    // you will need to update your keys.  See https://www.isc.org/bind-keys
    //========================================================================
    //dnssec-validation auto;

    allow-query { any; };
    auth-nxdomain no;
    listen-on-v6 { any; };
};" > /etc/bind/named.conf.options

service bind9 restart
```
Werkudara
```
echo 'zone "baratayuda.abimanyu.b26.com" {
        type master;
        file "/etc/bind/jarkom/baratayuda.abimanyu.b26.com";
};' >> /etc/bind/named.conf.local

mkdir /etc/bind/jarkom

cp /etc/bind/db.local /etc/bind/jarkom/baratayuda.abimanyu.b26.com

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     baratayuda.abimanyu.b26.com. root.baratayuda.abimanyu.b26.com. (
                        2023101001      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      baratayuda.abimanyu.b26.com.
@       IN      A       192.191.3.2     ; IP Abimanyu
www     IN      CNAME   baratayuda.abimanyu.b26.com.' > /etc/bind/jarkom/baratayuda.abimanyu.b26.com

echo "options {
    directory \"/var/cache/bind\";

    // If there is a firewall between you and nameservers you want
    // to talk to, you may need to fix the firewall to allow multiple
    // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

    // If your ISP provided one or more IP addresses for stable
    // nameservers, you probably want to use them as forwarders.
    // Uncomment the following block, and insert the addresses replacing
    // the all-0's placeholder.

    // forwarders {
    //      0.0.0.0;
    // };

    //========================================================================
    // If BIND logs error messages about the root key being expired,
    // you will need to update your keys.  See https://www.isc.org/bind-keys
    //========================================================================
    //dnssec-validation auto;

    allow-query { any; };
    auth-nxdomain no;
    listen-on-v6 { any; };
};" > /etc/bind/named.conf.options

service bind9 restart
```

## Soal 8
Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

### Jawaban
Werkudara
```
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     baratayuda.abimanyu.b26.com. root.baratayuda.abimanyu.b26.com. (
                        2023101001      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      baratayuda.abimanyu.b26.com.
@               IN      A       192.191.3.2     ; IP Abimanyu
www             IN      CNAME   baratayuda.abimanyu.b26.com.
rjp             IN      A       192.191.3.2     ; IP Abimanyu
www.rjp         IN      CNAME   rjp.baratayuda.abimanyu.b26.com.' > /etc/bind/jarkom/baratayuda.abimanyu.b26.com

service bind9 restart
```

## Soal 9
Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker

### Jawaban
Arjuna
```
echo 'upstream myweb {
  server 192.191.3.2; # IP Abimanyu
  server 192.191.3.3; # IP PrabuKusuma
  server 192.191.3.4; # IP Wisanggeni
}

server {
  listen 80;
  server_name arjuna.b26.com www.arjuna.b26.com;

  location / {
    proxy_pass http://myweb;
  }
}
' > /etc/nginx/sites-available/jarkom

ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled/jarkom

rm /etc/nginx/sites-enabled/default

service nginx restart
```
Prabukusuma, Abimanyu, Wisanggeni
```
service php7.0-fpm start

echo 'server {
        listen 80;

        root /var/www/jarkom;
        index index.php index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }

        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/run/php/php7.0-fpm.sock;
        }

        location ~ /\.ht {
                deny all;
        }
}' > /etc/nginx/sites-available/jarkom

ln -s /etc/nginx/sites-available/jarkom /etc/nginx/sites-enabled/jarkom

rm /etc/nginx/sites-enabled/default

service nginx restart
```
client (Nakula/Sadewa)
```
lynx http://192.191.3.2
lynx http://192.191.3.3
lynx http://192.191.3.4
lynx http://192.191.3.5
lynx http://arjuna.b26.com
```

## Soal 10
Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh (Prabakusuma:8001, Abimanyu:8002, Wisanggeni:8003)

### Jawaban
Arjuna
```
upstream myweb {
  server 192.191.3.3:8001; # IP PrabuKusuma
  server 192.191.3.2:8002; # IP Abimanyu
  server 192.191.3.4:8003; # IP Wisanggeni
}

```
Prabukusuma, Abimanyu, Wisanggeni
```
echo 'server {
        listen 800X;

        root /var/www/jarkom;
        index index.php index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                try_files $uri $uri/ /index.php?$query_string;
        }

        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/run/php/php7.0-fpm.sock;
        }

        location ~ /\.ht {
                deny all;
        }
}' > /etc/nginx/sites-available/jarkom

```
## Soal 11
Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy

### Jawaban
Yudhistira
```
# 11
echo ';
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     abimanyu.b26.com. root.abimanyu.b26.com. (
                        2023101001      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      abimanyu.b26.com.
@       IN      A       192.191.3.2     ; IP Abimanyu
www     IN      CNAME   abimanyu.b26.com.
parikesit IN    A       192.191.3.2     ; IP Abimanyu
ns1     IN      A       192.191.1.3     ; IP Werkudara
baratayuda IN   NS      ns1' > /etc/bind/jarkom/abimanyu.b26.com

service bind9 restart

```
Abimanyu
```
cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/abimanyu.b26.com.conf

rm /etc/apache2/sites-available/000-default.conf

echo -e '<VirtualHost *:80>
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/abimanyu.b26

  ServerName abimanyu.b26.com
  ServerAlias www.abimanyu.b26.com

  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>' > /etc/apache2/sites-available/abimanyu.b26.com.conf

a2ensite abimanyu.b26.com.conf

service apache2 restart

```
## Soal 12 
Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

### Jawaban
Kita akan menggunakan directory untuk melakukan rewrite indexes
```
<Directory /var/www/abimanyu.b26/index.php/home>
  Options +Indexes
</Directory>

Alias "/home" "/var/www/abimanyu.b26/index.php/home"

```
Abimanyu
```
echo -e '<VirtualHost *:80>
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/abimanyu.b26
  ServerName abimanyu.b26.com
  ServerAlias www.abimanyu.b26.com

  <Directory /var/www/abimanyu.b26/index.php/home>
          Options +Indexes
  </Directory>

  Alias "/home" "/var/www/abimanyu.b26/index.php/home"

  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>' > /etc/apache2/sites-available/abimanyu.b26.com.conf

service apache2 restart

```
Client (Nakula/Sadewa)
```
lynx abimanyu.b26.com/home
curl abimanyu.b26.com/home

```

## Soal 13
Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy

### Jawaban
Abimanyu
```
echo -e '<VirtualHost *:80>
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/parikesit.abimanyu.b26
  ServerName parikesit.abimanyu.b26.com
  ServerAlias www.parikesit.abimanyu.b26.com

  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>' > /etc/apache2/sites-available/parikesit.abimanyu.b26.com.conf

a2ensite parikesit.abimanyu.b26.com.conf

service apache2 restart

```
Client (Nakula/Sadewa)
```
lynx parikesit.abimanyu.b26.com
curl parikesit.abimanyu.b26.com

```

## Soal 14
Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden)

### Jawaban
Abimanyu
```
echo -e '<VirtualHost *:80>
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/parikesit.abimanyu.b26
  ServerName parikesit.abimanyu.b26.com
  ServerAlias www.parikesit.abimanyu.b26.com

  <Directory /var/www/parikesit.abimanyu.b26/public>
          Options +Indexes
  </Directory>

  <Directory /var/www/parikesit.abimanyu.b26/secret>
          Options -Indexes
  </Directory>

  Alias "/public" "/var/www/parikesit.abimanyu.b26/public"
  Alias "/secret" "/var/www/parikesit.abimanyu.b26/secret"

  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>' > /etc/apache2/sites-available/parikesit.abimanyu.b26.com.conf

service apache2 restart

```
Client (Nakula/Sadewa)
```
lynx parikesit.abimanyu.b26.com/public
lynx parikesit.abimanyu.b26.com/secret

```
