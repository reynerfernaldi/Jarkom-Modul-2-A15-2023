# Jarkom-Modul-2-A15-2023

### Kelompok A15

| **No** | **Nama**                   | **NRP**    |
| ------ | -------------------------- | ---------- |
| 1      | Frederick Hidayat          | 5025211152 |
| 2      | Reyner Fernaldi            | 5025201094 |


# PEMBAHASAN

## 1. Yudhistira akan digunakan sebagai DNS Master, Werkudara sebagai DNS Slave, Arjuna merupakan Load Balancer yang terdiri dari beberapa Web Server yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Buatlah topologi dengan pembagian sebagai berikut. Folder topologi dapat diakses pada drive berikut 
Membuat Topologi seperti berikut
![Alt text](img/topologi.png?raw=true "1a")

Berikut ini adalah konfigurasi setiap node
- Pandudewanata
    ```
    auto eth0
    iface eth0 inet dhcp

    auto eth1
    iface eth1 inet static
        address 	192.176.1.1
        netmask 255.255.255.0

    auto eth2
    iface eth2 inet static
        address	192.176.2.1
        netmask 255.255.255.0

    auto eth3
    iface eth3 inet static
        address 	192.176.3.1
        netmask 255.255.255.0

    auto eth4
    iface eth4 inet static
        address 	192.176.4.1
        netmask 255.255.255.0
    ```
-   Yudhistira
    ```
    auto eth0
    iface eth0 inet static
        address 192.176.2.2
        netmask 255.255.255.0
        gateway 192.176.2.1
    ```
-   Werkudara
    ```
    auto eth0
    iface eth0 inet static
        address 192.176.1.2
        netmask 255.255.255.0
        gateway 192.176.1.1
    ```

-   Sadewa
    ```
    auto eth0
        iface eth0 inet static
        address 192.176.3.2
        netmask 255.255.255.0
        gateway 192.176.3.1
    ```

-   Nakula
    ```
    auto eth0
    iface eth0 inet static
        address 192.176.3.3
        netmask 255.255.255.0
        gateway 192.176.3.1
    ```

-   Prabukusuma
    ```
    auto eth0
    iface eth0 inet static
        address 192.176.4.3
        netmask 255.255.255.0
        gateway 192.176.4.1
    ```

-   Abimanyu
    ```
    auto eth0
    iface eth0 inet static
        address 192.176.4.2
        netmask 255.255.255.0
        gateway 192.176.4.1
    ```

-   Wissanggeni
    ```
    auto eth0
    iface eth0 inet static
        address 192.176.4.4
        netmask 255.255.255.0
        gateway 192.176.4.1
    ```

-   Arjuna-LB
    ```
    auto eth0
    iface eth0 inet static
        address 192.176.4.5
        netmask 255.255.255.0
        gateway 192.176.4.1
    ```

    
## 2. Buatlah website utama pada node arjuna dengan akses ke arjuna.yyy.com dengan alias www.arjuna.yyy.com dengan yyy merupakan kode kelompok.

-   Melakukan konfigurasi pada `/etc/bind9/named.conf.local`
    ```
    zone "arjuna.a15.com"{
        type master;
        allow-transfer { 192.176.1.2; };
        file "/etc/bind/jarkom/arjuna.a15.com";
    };
    ```
-   Melakukan konfigurasi pada `/etc/bind/jarkom/arjuna.a15.com`
    ```
    ;
    ; BIND data file for local loopback interface
    ;
    $TTL    604800
    @       IN      SOA     arjuna.a15.com. root.arjuna.a15.com. (
                            202310130       ; Serial
                            604800         ; Refresh
                            86400         ; Retry
                            2419200         ; Expire
                            604800 )       ; Negative Cache TTL
    ;
    @       IN      NS      arjuna.a15.com.
    @       IN      A       192.176.4.5
    www     IN      CNAME   arjuna.a15.com.
    ```
-   Hasil
    </br><img src="img/2p.png?raw=true" alt="Alt text" title="1a" width="600">

## 3. Dengan cara yang sama seperti soal nomor 2, buatlah website utama dengan akses ke abimanyu.yyy.com dan alias www.abimanyu.yyy.com.

-   Menambahkan konfigurasi pada `/etc/bind9/named.conf.local`
    ```
    zone "abimanyu.a15.com"{
            type master;
            allow-transfer { 192.168.1.2; };
            file "/etc/bind/jarkom/abimanyu.a15.com";
    };
    ```
-   Melakukan konfigurasi pada `/etc/bind/jarkom/arjuna.a15.com`
    ```
    ;
    ; BIND data file for local loopback interface
    ;
    $TTL    604800
    @       IN      SOA     abimanyu.a15.com. root.abimanyu.a15.com. (
                                2         ; Serial
                            604800         ; Refresh
                            86400         ; Retry
                            2419200         ; Expire
                            604800 )       ; Negative Cache TTL
    ;
    @               IN      NS      abimanyu.a15.com.
    @               IN      A       192.176.4.2
    www             IN      CNAME   abimanyu.a15.com.
    ```
-   Hasil
    </br><img src="img/3p.png?raw=true" alt="Alt text" title="1a" width="600">


## 4. Kemudian, karena terdapat beberapa web yang harus di-deploy, buatlah subdomain parikesit.abimanyu.yyy.com yang diatur DNS-nya di Yudhistira dan mengarah ke Abimanyu.

-   Melakukan konfigurasi pada `/etc/bind/jarkom/arjuna.a15.com`
    ```
    ;
    ; BIND data file for local loopback interface
    ;
    $TTL    604800
    @       IN      SOA     abimanyu.a15.com. root.abimanyu.a15.com. (
                                2         ; Serial
                            604800         ; Refresh
                            86400         ; Retry
                            2419200         ; Expire
                            604800 )       ; Negative Cache TTL
    ;
    @               IN      NS      abimanyu.a15.com.
    @               IN      A       192.176.4.2
    www             IN      CNAME   abimanyu.a15.com.
    parikesit       IN      A       192.176.4.2
    ```
-   Hasil
    </br><img src="img/4p.png?raw=true" alt="Alt text" title="1a" width="600">

## 5. Buat juga reverse domain untuk domain utama. (Abimanyu saja yang direverse)
-   Menambahkan konfigurasi pada `/etc/bind9/named.conf.local`
    ```
    zone "4.176.192.in-addr.arpa" {
        type master;
        file "/etc/bind/jarkom/4.176.192.in-addr.arpa";
    };
    ```
-   Membuat directory pada /etc/bind/jarkom/4.176.192.in-addr.arpa
-   Melakukan konfigurasi pada `/etc/bind/jarkom/4.176.192.in-addr.arpa`
    ```
    ;
    ; BIND data file for local loopback interface
    ;
    $TTL    604800
    @       IN      SOA     abimanyu.a15.com. root.abimanyu.a15.com. (
                                2         ; Serial
                            604800         ; Refresh
                            86400         ; Retry
                            2419200         ; Expire
                            604800 )       ; Negative Cache TTL
    ;
    4.176.192.in-addr.arpa. IN      NS      abimanyu.a15.com.
    2                       IN      PTR     abimanyu.a15.com.
    ```

-   Hasil
    </br><img src="img/5p.png?raw=true" alt="Alt text" title="1a" width="600">


## 6. Agar dapat tetap dihubungi ketika DNS Server Yudhistira bermasalah, buat juga Werkudara sebagai DNS Slave untuk domain utama.
-   Membuat konfigurasi Yudhistira pada `/etc/bind9/named.conf.local`
    ```
    zone "abimanyu.a15.com"{
            type master;
            notify yes;
            also-notify { 192.168.1.2; };
            allow-transfer { 192.168.1.2; };
            file "/etc/bind/jarkom/abimanyu.a15.com";
    };
    ```
-   Membuat konfigurasi Werkudara pada `/etc/bind9/named.conf.local`
    ```
    zone "abimanyu.a15.com" {
            type slave;
            masters { 192.176.2.2; };
            file "/var/lib/bind/abimanyu.a15.com";
    };
    ```

-   Hasil ketika Yudhistira di `service bind9 stop`
    </br><img src="img/6p.png?raw=true" alt="Alt text" title="1a" width="600">

## 7. Seperti yang kita tahu karena banyak sekali informasi yang harus diterima, buatlah subdomain khusus untuk perang yaitu baratayuda.abimanyu.yyy.com dengan alias www.baratayuda.abimanyu.yyy.com yang didelegasikan dari Yudhistira ke Werkudara dengan IP menuju ke Abimanyu dalam folder Baratayuda.
-   Melakukan konfigurasi Yudhistira pada `/etc/bind/jarkom/abimanyu.a15.com`
    ```
    ;
    ; BIND data file for local loopback interface
    ;
    $TTL    604800
    @       IN      SOA     abimanyu.a15.com. root.abimanyu.a15.com. (
                                2         ; Serial
                            604800         ; Refresh
                            86400         ; Retry
                            2419200         ; Expire
                            604800 )       ; Negative Cache TTL
    ;
    @               IN      NS      abimanyu.a15.com.
    @               IN      A       192.176.4.2
    www             IN      CNAME   abimanyu.a15.com.
    parikesit       IN      A       192.176.4.2
    ns1             IN      A       192.176.1.2     ; IP Werkudara
    baratayuda      IN      NS      ns1 
    ```

-   Melakukan konfigurasi Yudhistira pada `/etc/bind/named.conf.options`
    ```
    options {
            directory "/var/cache/bind";

            // If there is a firewall between you and nameservers you want
            // to talk to, you may need to fix the firewall to allow multiple
            // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

            // If your ISP provided one or more IP addresses for stable
            // nameservers, you probably want to use them as forwarders.
            // Uncomment the following block, and insert the addresses replacing
            // the all-0's placeholder.

            // forwarders {
            //      192.168.122.1;
            // };

            //========================================================================
            // If BIND logs error messages about the root key being expired,
            // you will need to update your keys.  See https://www.isc.org/bind-keys
            //========================================================================
            //dnssec-validation auto;
            allow-query { any; };

            auth-nxdomain no;    # conform to RFC1035
            listen-on-v6 { any; };
    };
    ```
-   Melakukan konfigurasi Wekudara pada `/etc/bind/named.conf.local`
    ```
    zone "baratayuda.abimanyu.a15.com" {
            type master;
            file "/etc/bind/baratayuda/baratayuda.abimanyu.a15.com";
    };
    ```
-   Membuat directory /etc/bind/baratayuda
-   Melakukan konfigurasi Werkudara pada `/etc/bind/jarkom/baratayuda.abimanyu.a15.com`
    ```
    ;
    ; BIND data file for local loopback interface
    ;
    $TTL    604800
    @       IN      SOA     baratayuda.abimanyu.a15.com. root.baratayuda.abimanyu.a15.com. (
                                2         ; Serial
                            604800         ; Refresh
                            86400         ; Retry
                            2419200         ; Expire
                            604800 )       ; Negative Cache TTL
    ;
    @               IN      NS      baratayuda.abimanyu.a15.com.
    @               IN      A       192.176.4.2     ; IP Abimanyu
    www             IN      CNAME   baratayuda.abimanyu.a15.com.
    ```

-   Melakukan konfigurasi Werkudara pada `/etc/bind/named.conf.options`
    ```
    options {
            directory "/var/cache/bind";

            // If there is a firewall between you and nameservers you want
            // to talk to, you may need to fix the firewall to allow multiple
            // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

            // If your ISP provided one or more IP addresses for stable
            // nameservers, you probably want to use them as forwarders.
            // Uncomment the following block, and insert the addresses replacing
            // the all-0's placeholder.

            // forwarders {
            //      192.168.122.1;
            // };

            //========================================================================
            // If BIND logs error messages about the root key being expired,
            // you will need to update your keys.  See https://www.isc.org/bind-keys
            //========================================================================
            //dnssec-validation auto;
            allow-query { any; };

            auth-nxdomain no;    # conform to RFC1035
            listen-on-v6 { any; };
    };
    ```
-   Hasil
    </br><img src="img/7p.png?raw=true" alt="Alt text" title="1a" width="600">
## 8. Untuk informasi yang lebih spesifik mengenai Ranjapan Baratayuda, buatlah subdomain melalui Werkudara dengan akses rjp.baratayuda.abimanyu.yyy.com dengan alias www.rjp.baratayuda.abimanyu.yyy.com yang mengarah ke Abimanyu.

-   Melakukan konfigurasi Werkudara pada `/etc/bind/jarkom/baratayuda.abimanyu.a15.com`
    ```
    ;
    ; BIND data file for local loopback interface
    ;
    $TTL    604800
    @       IN      SOA     baratayuda.abimanyu.a15.com. root.baratayuda.abimanyu.a15.com. (
                                2         ; Serial
                            604800         ; Refresh
                            86400         ; Retry
                            2419200         ; Expire
                            604800 )       ; Negative Cache TTL
    ;
    @               IN      NS      baratayuda.abimanyu.a15.com.
    @               IN      A       192.176.4.2     ; IP Abimanyu
    www             IN      CNAME   baratayuda.abimanyu.a15.com.
    rjp             IN      A       192.176.4.2     ; IP Abimanyu
    www.rjp         IN      CNAME   baratayuda.abimanyu.a15.com.
    ```
    
-   Hasil
    </br><img src="img/8p.png?raw=true" alt="Alt text" title="1a" width="600">
## 9. Arjuna merupakan suatu Load Balancer Nginx dengan tiga worker (yang juga menggunakan nginx sebagai webserver) yaitu Prabakusuma, Abimanyu, dan Wisanggeni. Lakukan deployment pada masing-masing worker.

-   Membuat konfigurasi roundrobin pada Arjuna-LB
    ```
    upstream myweb  {
            server 192.176.4.3:8001;
            server 192.176.4.2:8002;
            server 192.176.4.4:8003;
    }

    server {
            listen 80;
            server_name arjuna.a15.com;

            location / {
            proxy_pass http://myweb;
            }
    }
    ```
-   Membuat konfigurasi pada Prabukusuma `/etc/nginx/sites-available/script`
    ```
    server {

        listen 8001;

        root /var/www/arjuna.a15.com;

        index index.php index.html index.htm;
        server_name arjuna.a15.com;

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
    }
    ```
-   Membuat konfigurasi pada Abimanyu `/etc/nginx/sites-available/script`
    ```
    server {

            listen 8002;

            root /var/www/arjuna.a15.com;

            index index.php index.html index.htm;
            server_name arjuna.a15.com;

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
    }
    ```

-   Membuat konfigurasi pada Wissanggeni `/etc/nginx/sites-available/script`
    ```
    server {

            listen 8003;

            root /var/www/arjuna.a15.com;

            index index.php index.html index.htm;
            server_name arjuna.a15.com;

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
    }
    ```
## 10. Kemudian gunakan algoritma Round Robin untuk Load Balancer pada Arjuna. Gunakan server_name pada soal nomor 1. Untuk melakukan pengecekan akses alamat web tersebut kemudian pastikan worker yang digunakan untuk menangani permintaan akan berganti ganti secara acak. Untuk webserver di masing-masing worker wajib berjalan di port 8001-8003. Contoh
    - Prabakusuma:8001
    - Abimanyu:8002
    - Wisanggeni:8003

Hasil
</br><img src="img/10ap.png?raw=true" alt="Alt text" title="1a" width="600">
</br><img src="img/10bp.png?raw=true" alt="Alt text" title="1a" width="600">
</br><img src="img/10cp.png?raw=true" alt="Alt text" title="1a" width="600">

## 11. Selain menggunakan Nginx, lakukan konfigurasi Apache Web Server pada worker Abimanyu dengan web server www.abimanyu.yyy.com. Pertama dibutuhkan web server dengan DocumentRoot pada /var/www/abimanyu.yyy
-   Melakukan konfigurasi pada Abimanyu
    ```
    <VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin abimanyu.a15.com
        DocumentRoot /var/www/abimanyu.a15
        ServerName abimanyu.a15.com
        ServerAlias www.abimanyu.a15.com

         <Directory /var/www/abimanyu.a15>
             Options +FollowSymLinks -Multiviews
             AllowOverride All
         </Directory>

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
    </VirtualHost>
    ```
-   Hasil
    ```
    lynx abimanyu.a15.com
    ```
    </br><img src="img/11p.png?raw=true" alt="Alt text" title="1a" width="600">

## 12. Setelah itu ubahlah agar url www.abimanyu.yyy.com/index.php/home menjadi www.abimanyu.yyy.com/home.

-   Hasil
    </br><img src="img/11p.png?raw=true" alt="Alt text" title="1a" width="600">

## 13. Selain itu, pada subdomain www.parikesit.abimanyu.yyy.com, DocumentRoot disimpan pada /var/www/parikesit.abimanyu.yyy
-   Melakukan konfigurasi pada `parikesit.abimanyu.a15.com`
    ```
    <VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        ServerName parikesit.abimanyu.a15.com
        ServerAlias www.parikesit.abimanyu.a15.com
        DocumentRoot /var/www/parikesit.abimanyu.a15.com

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
    </VirtualHost>
    ```

-   Hasil  `lynx parikesit.abimanyu.a15.com`
    </br><img src="img/13p.png?raw=true" alt="Alt text" title="1a" width="600">

## 14. Pada subdomain tersebut folder /public hanya dapat melakukan directory listing sedangkan pada folder /secret tidak dapat diakses (403 Forbidden).

-   Melakukan konfigurasi pada `parikesit.abimanyu.a15.com`
    ```
    <VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        ServerName parikesit.abimanyu.a15.com
        ServerAlias www.parikesit.abimanyu.a15.com
        DocumentRoot /var/www/parikesit.abimanyu.a15.com

    <Directory /var/www/parikesit.abimanyu.a15.com/public>
        Options +Indexes
    </Directory>

    <Directory /var/www/parikesit.abimanyu.a15.com/secret>
        Options -Indexes
    </Directory>

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
    </VirtualHost>
    ```

Hasil 
-   Hasil `lynx parikesit.abimanyu.a15.com/public`
    </br><img src="img/14ap.png?raw=true" alt="Alt text" title="1a" width="600">

-   Hasil `lynx parikesit.abimanyu.a15.com/secret`
    </br><img src="img/14bp.png?raw=true" alt="Alt text" title="1a" width="600">

## 15 Buatlah kustomisasi halaman error pada folder /error untuk mengganti error kode pada Apache. Error kode yang perlu diganti adalah 404 Not Found dan 403 Forbidden.

-   Melakukan konfigurasi pada `parikesit.abimanyu.a15.com.conf`
    ```
    <VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        ServerName parikesit.abimanyu.a15.com
        ServerAlias www.parikesit.abimanyu.a15.com
        DocumentRoot /var/www/parikesit.abimanyu.a15.com
    <Directory /var/www/parikesit.abimanyu.a15.com/public>
        Options +Indexes
    </Directory>

    <Directory /var/www/parikesit.abimanyu.a15.com/secret>
        Options -Indexes
    </Directory>

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        ErrorDocument 403 /var/www/parikesit.abimanyu.a15.com/error/403.html
        ErrorDocument 404 /var/www/parikesit.abimanyu.a15.com/error/404.html

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
    </VirtualHost>
    ```
- Hasil `lynx parikesit.abimanyu.a15.com/notfound`
    </br><img src="img/15ap.png?raw=true" alt="Alt text" title="1a" width="600">
- Hasil `lynx parikesit.abimanyu.a15.com/secret`
    </br><img src="img/15bp.png?raw=true" alt="Alt text" title="1a" width="600">

## 16. Buatlah suatu konfigurasi virtual host agar file asset www.parikesit.abimanyu.yyy.com/public/js menjadi  www.parikesit.abimanyu.yyy.com/js 

-   Melakukan konfigurasi pada `parikesit.abimanyu.a15.com.conf`
    ```
    <VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        ServerName parikesit.abimanyu.a15.com
        ServerAlias www.parikesit.abimanyu.a15.com
        DocumentRoot /var/www/parikesit.abimanyu.a15.com
    <Directory /var/www/parikesit.abimanyu.a15.com/public>
        Options +Indexes
    </Directory>

    <Directory /var/www/parikesit.abimanyu.a15.com/secret>
        Options -Indexes
    </Directory>

         <Directory /var/www/parikesit.abimanyu.a15.com>
             Options +FollowSymLinks -Multiviews
             AllowOverride All
         </Directory>

        Alias "/js" "/var/www/parikesit.abimanyu.a15.com/public/js"

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        ErrorDocument 403 /error/403.html
        ErrorDocument 404 /error/404.html

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
    </VirtualHost>
    ```
- Hasil `lynx parikesit.abimanyu.a15.com/js`
    </br><img src="img/16p.png?raw=true" alt="Alt text" title="1a" width="600">

## 17. Agar aman, buatlah konfigurasi agar www.rjp.baratayuda.abimanyu.yyy.com hanya dapat diakses melalui port 14000 dan 14400.

-   Melakukan konfigurasi pada `rjp.baratayuda.abimanyu.a15.com.conf`

    ```
    <VirtualHost *:14000 *:14400>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        ServerName rjp.baratayuda.abimanyu.a15.com
        ServerAlias www.rjp.baratayuda.abimanyu.a15.com
        DocumentRoot /var/www/rjp.baratayuda.abimanyu.a15.com

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
    </VirtualHost>
    ```
-   Hasil `lynx rjp.baratayuda.abimanyu.a15.com:14000` atau `lynx rjp.baratayuda.abimanyu.a15.com:14400`
    </br><img src="img/17p.png?raw=true" alt="Alt text" title="1a" width="600">
## 18. Untuk mengaksesnya buatlah autentikasi username berupa “Wayang” dan password “baratayudayyy” dengan yyy merupakan kode kelompok. Letakkan DocumentRoot pada /var/www/rjp.baratayuda.abimanyu.yyy.

-   Melakukan konfigurasi pada `rjp.baratayuda.abimanyu.a15.com.conf`
    ```
    <VirtualHost *:14000 *:14400>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        ServerName rjp.baratayuda.abimanyu.a15.com
        ServerAlias www.rjp.baratayuda.abimanyu.a15.com
        DocumentRoot /var/www/rjp.baratayuda.abimanyu.a15.com

         <Directory /var/www/rjp.baratayuda.abimanyu.a15.com>
                AuthType Basic
                AuthName "Restricted Content"
                AuthUserFile /etc/apache2/.htpasswd
                Require valid-user
         </Directory>

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
    </VirtualHost>
    ```
-   Gunakan `htpasswd -cb /etc/apache2/.htpasswd Wayang baratayudaa15` untuk membuat username dan password

-   Hasil `lynx rjp.baratayuda.abimanyu.a15.com:14000` atau `lynx rjp.baratayuda.abimanyu.a15.com:14400`
    </br><img src="img/18ap.png?raw=true" alt="Alt text" title="1a" width="600">
    </br><img src="img/18bp.png?raw=true" alt="Alt text" title="1a" width="600">
    </br><img src="img/17p.png?raw=true" alt="Alt text" title="1a" width="600">

## 19. Buatlah agar setiap kali mengakses IP dari Abimanyu akan secara otomatis dialihkan ke www.abimanyu.yyy.com (alias)

-   Melakukan konfigurasi pada `000-default.conf`
    ```
    <VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
        Redirect / http://www.abimanyu.a15.com/

    </VirtualHost>
    ```
-   Hasil `lynx 192.176.4.2`
    </br><img src="img/19p.png?raw=true" alt="Alt text" title="1a" width="600">

## 20. Karena website www.parikesit.abimanyu.yyy.com semakin banyak pengunjung dan banyak gambar gambar random, maka ubahlah request gambar yang memiliki substring “abimanyu” akan diarahkan menuju abimanyu.png.

-   Melakukan konfigurasi pada `/var/www/parikesit.abimanyu.a15.com/htaccess`
    ```
    RewriteEngine On

    RewriteCond %{REQUEST_URI} !^/public/images/abimanyu.png
    RewriteCond %{REQUEST_URI} abimanyu
    RewriteRule \.(jpg|jpeg|png)$ /public/images/abimanyu.png [R=301,L]
    ```
-   Melakukan konfigurasi pada `parikesit.abimanyu.a15.com.conf`
    ```
    <VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        #ServerName www.example.com

        ServerAdmin webmaster@localhost
        ServerName parikesit.abimanyu.a15.com
        ServerAlias www.parikesit.abimanyu.a15.com
        DocumentRoot /var/www/parikesit.abimanyu.a15.com
        <Directory /var/www/parikesit.abimanyu.a15.com/public>
            Options +Indexes
        </Directory>

        <Directory /var/www/parikesit.abimanyu.a15.com/secret>
            Options -Indexes
        </Directory>

         <Directory /var/www/parikesit.abimanyu.a15.com>
             Options +FollowSymLinks -Multiviews
             AllowOverride All
         </Directory>

        Alias "/js" "/var/www/parikesit.abimanyu.a15.com/public/js"

        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        ErrorDocument 403 /error/403.html
        ErrorDocument 404 /error/404.html

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
    </VirtualHost>
    ```
-   Hasil `lynx parikesit.abimanyu.a15.com/public/images/abimanyu-student.jpg`
    </br><img src="img/20ap.png?raw=true" alt="Alt text" title="1a" width="800">
    </br><img src="img/20bp.png?raw=true" alt="Alt text" title="1a" width="800">
