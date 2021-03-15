# YOURLS

## SEKILAS TENTANG
**YOURLS** (Your Own URL Shortener) adalah aplikasi dengan coding php script yang bisa digunakan untuk membuat website layanan URL shortening (memendekkan alamat suatu website, agar mudah dituliskan di posting forum, blog, email, dll). 

## INSTALASI
### Kebutuhan Sistem :
- Linux atau Windows.
- Nginx / Apache (httpd) version 2.4.
- PHP version 5.3.
- MySQL 5.0+.
- RAM minimal 64 Mb+
- PHP cURL extension

### Proses Instalasi :
1.  Login kedalam server menggunakan SSH. Untuk pengguna windows bisa menggunakan aplikasi [PuTTY](http://www.putty.org/).
 ```
    $ 
 ```
### Install semua kebutuhan yang diperlukan
Sebelum menginstall kebutuhan yang diperlukan, pastikan seluruh paket pada sistem sudah up-to-date
 ```
    sudo apt-get update
 ```
 
Install seluruh kebutuhan yang diperlukan
 ```
    sudo apt install -y mariadb-server
    sudo apt install -y php php-fpm php-cli php-mysql php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath
    sudo apt install -y nginx
 ```
Setelah semua kebutuhan telah diinstall, jalankan Nginx dan MariaDB
 ```
    sudo systemctl start nginx
    sudo systemctl start mariadb
 ```
### Membuat database untuk YOURLS
Pertama masuk terlebih dahulu kedalam MariaDB
 ```
    sudo mysql
 ```
 
Setelah masuk buat database bernama yourls
 ```
    CREATE DATABASE yourls;
 ```
Berikan semua akses database yourls kepada user yourls@localhost dan tambahkan password
 ```
    GRANT ALL PRIVILEGES ON yourls.* TO 'yourls'@'localhost' IDENTIFIED BY "YOUR-PASSWORD";
 ```
 
Flush hak akses yang diberikan dan keluar dari MariaDb
 ```
   FLUSH PRIVILEGES;
   QUIT
 ```
 
### Download dan Install YOURSL
2. Tempatkan unduhan YOURLS ke `/srv` directory
```
    $ cd /srv
    $ git clone https://github.com/YOURLS/YOURLS.git
```
3.  Copy `user/config-sample.php to user/config.php`
```
    $ cd YOURLS/user
    $ cp config-sample.php config.php
```
4.  Set database connection
```
    $ sudo config.php
```
```
    *
    ** MySQL settings - You can get this info from your web host
    */

    /** MySQL database username */
    define( 'YOURLS_DB_USER', 'yourls' );

    /** MySQL database password */
    define( 'YOURLS_DB_PASS', 'StrongPassword' );
 
    /** The name of the database for YOURLS */
    define( 'YOURLS_DB_NAME', 'yourls' );

    /** MySQL hostname.
     ** If using a non standard port, specify it like 'hostname:port', eg. 'localhost:9999' or '127.0.0.1:666' */
    define( 'YOURLS_DB_HOST', 'localhost' );

    /** MySQL tables prefix */                                                                                                                            
    define( 'YOURLS_DB_PREFIX', 'yourls_' );
```
5.  Set website URL for YOURLS
 ```
    /** YOURLS installation URL -- all lowercase, no trailing slash at the end.
     ** If you define it to "http://sho.rt", don't use "http://www.sho.rt" in your browser (and vice-versa) */
    define( 'YOURLS_SITE', 'http://yourls.example.com' );
 ```
6.  Tambahkan Username(s) dan password(s) yang diizinkan untuk mengakses situs. Password dapat berupa teks biasa atau sebagai hash terenkripsi. YOURLS akan secara otomatis mengenkripsi sandi teks biasa dalam file ini
 ```
    /** Username(s) and password(s) allowed to access the site. Passwords either in plain text or as encrypted hashes
     ** YOURLS will auto encrypt plain text passwords in this file
     ** Read http://yourls.org/userpassword for more information */
    $yourls_user_passwords = array(
      'admin' => 'AdminPassword',
      'jmutai' => 'MyStrongPassword',
       // You can have one or more 'login'=>'password' lines
       );
 ```
7. Ketika selesai, simpan dan keluar dengan cara ctrl+o, enter, ctrl+x

#### Download dan Konfigurasi Nginx
8. Instal Nginx di Ubuntu 18.04 dengan menjalankan perintah:
```
    $ sudo apt install -y nginxt
```
9. Buat file `/etc/nginx/conf.d/yourls.conf` konfigurasi baru dengan konten di bawah ini
```
    $ sudo nano /etc/nginx/conf.d/yourls.conf
```
```
    server {
      listen 80;
      root /srv/YOURLS;
      index index.php index.html index.htm;
      listen [::]:80;
      location / {
        try_files $uri $uri/ /yourls-loader.php$is_args$args;
      }

      location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_index index.php;
        fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
        fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
        include fastcgi_params;
      }
    }
```
10. Periksa syntax nginx untuk memastikannya baik-baik saja (OK)
```
    $ nginx -t
```
11. Berikan kepemilikan pengguna web Nginx ke direktori /srv/YOURLS.
```
    $ sudo chown -R www-data:www-data /srv/YOURLS
```
12. Restart nginx service.
```
    $ sudo systemctl restart nginx
```

## KONFIGURASI
## CARA PEMAKAIAN
## PEMBAHASAN
## REFERENSI
