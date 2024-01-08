# OS-Server-Project
Tiara Citra Mustika (22.83.0864) 
Kelas : 22TK-OsServer 1. 

Dalam memenuhi tugas akhir mata kuliah OS Server dan Sistem Admin, saya membuat project yaitu:

  OS yang digunakan : DEBIAN 12

Pembuatan Web Server dengan beberapa service, yaitu:

   1. Web Server Software: Apache, untuk mengelola permintaan HTTP dari klien dan mengirimkan konten web.
   2. Database Server: Mysql/Mariadb, untuk penyimpanan data, dan sistem manajemen basis data (DBMS).
   3. Domain Name System (DNS) Service: Berkeley Internet Name Domain BIND, untuk memetakan nama domain ke alamat IP server web.
   4. Content Management System (CMS): Wordpress, untuk membuat, mengelola, dan mempublikasikan konten di situs web.
   5. SSL: OpenSSL, agar tercipta koneksi aman antara server web dan pengguna melalui HTTPS. (gagal)
   6. Monitoring Tools: Grafana, untuk membantu memahami perilaku pengguna dan kinerja situs web.



## Web Server (Apache)

### 1. Menyiapkan Server Web Apache

#### A. Instalasi Apache2

**Langkah 1: Instalasi Paket Apache2**
```
apt update
apt-get install apache2
```

#### B. Konfigurasi Apache2

**Langkah 1: Mengakses Berkas Konfigurasi Apache2**
```
nano /etc/apache2/sites-available/000-default.conf
```

**Langkah 2: Menyesuaikan Konfigurasi untuk Domain Anda**
```
<VirtualHost *:80>
        ServerAdmin finalproject@citra.com
        ServerName finalproject-citra.com
        DocumentRoot /var/www/html
```

**Langkah 3: Merestart Layanan Apache2**
```
systemctl restart apache2
```

#### C. Menyiapkan CMS Wordpress pada Apache2

**Langkah 1: Instalasi PHP**
```
apt-get update
apt-get install php php-mysql
```

**Langkah 2: Instalasi Server Database**
_*Dilangkah selanjutnya_

**Langkah 3: Membuat Database untuk Wordpress**

Akses Basis Data:
```
mysql -u root -p
```

Buat basis data dan pengguna untuk Wordpress:
```
CREATE DATABASE wordpress;
CREATE USER 'citra'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON wordpress.* TO 'citra'@'localhost';
FLUSH PRIVILEGES;
EXIT;

```

**Langkah 4: Mengunduh dan Mengekstrak Paket Wordpress**
```
curl -O https://wordpress.org/latest.tar.gz
tar xzvf latest.tar.gz
```

**Langkah 5: Memindahkan Isi Direktori Wordpress**
```
mv /tmp/wordpress/* /var/www/html/
```

**Langkah 6: Menyalin dan Menyesuaikan Berkas Konfigurasi Wordpress**
```
cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
nano /var/www/html/wp-config.php
```

**Langkah 7: Menyesuaikan Pengaturan Basis Data untuk Wordpress**
```
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'citra' );
define( 'DB_PASSWORD', '123' );
define( 'DB_HOST', 'localhost' );
```

**Langkah 8: Mengatur Izin**
```
chown -R www-data:www-data /var/www/html
```

**Langkah 9: Memasang Admin Wordpress**

(Ikuti langkah-langkah dalam antarmuka Wordpress untuk menyelesaikan pemasangan)


## Database Server (MySQL/MariaDB)

### 1. Konfigurasi MariaDB

#### A. Instalasi MariaDB

**Langkah 1: Menginstal Paket MariaDB**
```bash
sudo apt-get update
sudo apt-get install mariadb-server
```

#### B. Konfigurasi MariaDB

**Langkah 1: Menjalankan Perintah Berikut**
```bash
mysql_secure_installation
```

**Langkah 2: Menyesuaikan Konfigurasi**


#### C. Instalasi dan Konfigurasi Phpmyadmin

**Langkah 1: Instalasi Paket Phpmyadmin**
```bash
apt-get install phpmyadmin
```

**Langkah 2: Konfigurasi Phpmyadmin**

1. Konfigurasi Otomatis melalui Antarmuka Web
   

#### D. Pengujian Konfigurasi

Pastikan Phpmyadmin dapat diakses melalui server web Apache2 Anda.


## Domain Name System (DNS) 

### 1. Pengaturan Awal DNS Server (BIND9)

#### A. Instalasi BIND9

**Langkah 1: Instalasi Paket BIND9**
```bash
apt update
apt-get install bind9
```

#### B. Konfigurasi BIND9

**Langkah 1: Penyiapan Konfigurasi Forward dan Reverse**
```bash
cd /etc/bind
cp db.local db.forward
cp db.127 db.reverse
```

**Langkah 2: Konfigurasi File db.forward**
```bash
nano db.forward
```
Ubah konfigurasi sesuai dengan kebutuhan Anda.

**Langkah 3: Konfigurasi File db.reverse**
```bash
nano db.reverse
```
Sesuaikan konfigurasi dengan informasi yang sesuai.

**Langkah 4: Konfigurasi named.conf.local untuk Zona DNS**
```bash
nano named.conf.local
```
Ubah file konfigurasi untuk menunjukkan lokasi zona DNS.

**Langkah 5: Konfigurasi Forwarders**
```bash
nano named.conf.options
```
Tambahkan DNS forwarders.

**Langkah 6: Konfigurasi DNS pada Perangkat Server**
```bash
nano /etc/resolv.conf
```
Ubah file konfigurasi untuk mengarahkan ke DNS server.

**Langkah 7: Restart Layanan BIND9**
```bash
systemctl restart bind9
```

#### C. Pengujian Konfigurasi DNS

**Langkah 1: Instalasi Paket DNS Resolver**
```bash
apt-get install dnsutils
```

**Pengujian DNS**
```bash
nslookup finalprojectcitra.com
nslookup 192.168.56.4
```
Lakukan uji coba untuk mengonfirmasi konfigurasi DNS pada server.


## Monitoring Tools 

### 1. Instalasi Prometheus 

**Langkah 1: Unduh dan Ekstrak Prometheus:**

   ```bash
   # Unduh file tar Prometheus dari situs resmi
   wget https://github.com/prometheus/prometheus/releases/download/v2.30.0/prometheus-2.30.0.linux-amd64.tar.gz
   
   # Ekstrak file tar
   tar -xvzf prometheus-2.30.0.linux-amd64.tar.gz
   ```

**Langkah 2: Konfigurasi Prometheus:**

   Buka dan konfigurasi file `prometheus.yml` untuk menyesuaikan sumber target yang ingin dimonitor.

**Langkah 3: Menjalankan Prometheus:**

   ```bash
   # Navigasi ke direktori Prometheus
   cd prometheus-2.30.0.linux-amd64/

   # Jalankan Prometheus
   ./prometheus
   ```

   Ini akan memulai Prometheus dan mencatatkan output yang menyatakan bahwa Prometheus telah mulai memantau.

    **Output:**
    ```
    level=info ts=2024-01-09T13:12:33.820Z caller=main.go:330 msg="No time or size retention was set so using the default time retention" duration=15d
    level=info ts=2024-01-09T13:12:33.820Z caller=main.go:387 msg="Starting Prometheus" version="(version=2.30.0, branch=HEAD, revision=XXXX)"
    level=info ts=2024-01-09T13:12:33.820Z caller=main.go:388 build_context="(go=go1.17.6, user=root@4b65ab42d512, date=20211213-08:23:21)"
    level=info ts=2024-01-09T13:12:33.820Z caller=main.go:389 host_details="(Linux 5.4.0-97-generic #110-Ubuntu SMP Tue Jun 29 18:27:00 UTC 2021 x86_64 prometheus-2.30.0)"
    level=info ts=2024-01-09T13:12:33.820Z caller=main.go:390 fd_limits="(soft=1048576, hard=1048576)"
    level=info ts=2024-01-09T13:12:33.820Z caller=main.go:391 vm_limits="(soft=unlimited, hard=unlimited)"
    level=info ts=2024-01-09T13:12:33.821Z caller=main.go:711 msg="Starting TSDB ..."
    level=info ts=2024-01-09T13:12:33.821Z caller=main.go:721 msg="TSDB started"
    level=info ts=2024-01-09T13:12:33.821Z caller=main.go:782 msg="Loading configuration file" filename=prometheus.yml
    level=info ts=2024-01-09T13:12:33.821Z caller=main.go:820 msg="Completed loading of configuration file" filename=prometheus.yml
    level=info ts=2024-01-09T13:12:33.822Z caller=main.go:649 msg="Server is ready to receive web requests."
    ```


**Langkah 4: Akses Antarmuka Prometheus:**

   Buka browser dan akses `http://localhost:9090` untuk melihat antarmuka Prometheus.
