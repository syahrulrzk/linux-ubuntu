# Langkah 1 — Menginstal Apache dan Memperbarui Firewall
Server web Apache adalah salah satu server web paling populer di dunia. Server web Apache terdokumentasi dengan baik, memiliki komunitas pengguna yang aktif, dan digunakan secara luas dalam sejarah web, yang membuatnya menjadi pilihan asali yang hebat untuk menjadi hos situs web.

Instal Apache menggunakan manajer paket Ubuntu, apt:
<pre>sudo apt update
sudo apt install apache2</pre>

Jika ini adalah kali pertama Anda menggunakan sudo dalam sesi ini, Anda akan diminta memberikan kata sandi pengguna Anda untuk memastikan Anda memiliki privilese yang benar untuk mengelola paket sistem dengan apt. Anda juga akan diminta mengonfirmasi instalasi Apache dengan menekan Y, lalu ENTER.
Setelah instalasi selesai, Anda akan perlu menyesuaikan pengaturan firewall Anda untuk memperbolehkan lalu lintas HTTP. UFW memiliki berbagai profil aplikasi berbeda yang dapat Anda manfaatkan untuk menyelesaikannya. Untuk mendapatkan daftar semua profil aplikasi UFW yang tersedia, Anda dapat menjalankan:

<pre>sudo ufw app list</pre>
Anda akan melihat keluaran seperti ini:
<pre>Output :
Available applications:
  Apache
  Apache Full
  Apache Secure
  OpenSSH</pre>
Berikut adalah makna dari setiap profil ini:

<li>Apache: Profil ini hanya membuka porta 80 (lalu lintas web normal dan tidak terenkripsi).</li>
<li>Apache Full: Profil ini membuka baik porta 80 (lalu lintas web normal dan tidak terenkripsi) dan porta 443 (lalu lintas terenkripsi TLS/SSL).</li>
<li>Apache Secure: Profile ini hanya membuka porta 443 (lalu lintas terenkripsi TLS/SSL).</li>
Untuk saat ini, sebaiknya izinkan hanya koneksi pada porta 80, karena ini adalah instalasi Apache yang baru dan Anda belum memiliki sertifikat TLS/SSL yang dikonfigurasi untuk mengizinkan lalu lintas HTTPS di server Anda.

Untuk hanya memperbolehkan lalu lintas pada porta 80, gunakan profil Apache:
<pre>sudo ufw allow in "Apache"</pre>

Anda dapat memverifikasi perubahan dengan:
<pre>Output:
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                                
Apache                     ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)                    
Apache (v6)                ALLOW       Anywhere (v6)  </pre>

Lalu lintas pada porta 80 sekarang diperbolehkan melewati firewall.

Anda dapat melakukan pemeriksaan cepat untuk memastikan segalanya berjalan sesuai rencana dengan mengunjungi alamat IP publik server Anda di peramban web Anda (lihat catatan di bawah judul berikutnya untuk mengetahui alamat IP publik Anda jika Anda belum memiliki informasi ini):

http://your_server_ip ot ketikan "localhost" di navbar sear engine kalian.
Anda akan melihat laman web Apache Ubuntu 20.04 asali, yang tersedia dengan tujuan pengujian dan informasi.

## Cara Menemukan Alamat IP Publik Server Anda
Jika Anda tidak mengetahui alamat IP publik server Ada, ada sejumlah cara untuk mengetahuinya. Biasanya, ini adalah alamat yang digunakan untuk terhubung ke server Anda melalui SSH.

Ada beberapa cara untuk melakukannya dari baris perintah. Pertama, Anda dapat menggunakan alat iproute2 untuk mengetahui alamat IP dengan mengetik ini:
<pre>ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//' </pre>

Ini akan menampilkan dua atau tiga baris tanggapan. Semua adalah alamat yang benar, tetapi komputer Anda mungkin hanya dapat menggunakan salah satunya, jadi silakan mencoba masing-masing alamat itu.

Cara alternatifnya adalah menggunakan utilitas curl untuk menghubungi pihak luar supaya memberitahukan bagaimana pihak luar melihat server Anda. Hal ini dilakukan dengan menanyakan alamat IP Anda kepada server tertentu:
<pre>curl http://icanhazip.com</pre>
Terlepas dari cara yang digunakan untuk mengetahui alamat IP Anda, ketik alamat IP pada bilah alamat di peramban web Anda untuk melihat laman Apache asali.


# Langkah 2 — Menginstal MySQL
Setelah server web hidup dan berfungsi, Anda perlu menginstal sistem basis data agar dapat menyimpan dan mengelola data untuk situs Anda. MySQL adalah sistem manajemen basis data populer yang digunakan dalam lingkungan PHP.

Sekali lagi, gunakan apt untuk memperoleh dan menginstal perangkat lunak ini:

<pre>sudo apt install mysql-server</pre>
 
Ketika diminta, lakukan konfirmasi instalasi dengan mengetik Y, lalu ENTER.

Ketika instalasi selesai, Anda disarankan untuk menjalankan skrip keamanan yang sudah terinstal sebelumnya dengan MySQL. Skrip ini akan menghapus beberapa pengaturan asali yang tidak aman dan mengunci akses ke sistem basis data Anda. Mulai skrip interaktif dengan menjalankan:

<pre>sudo mysql_secure_installation</pre>
 
Anda akan ditanya apakah Anda ingin mengonfigurasi VALIDATE PASSWORD PLUGIN.

Catatan: Mengaktifkan fitur ini merupakan keputusan yang Anda pertimbangkan sendiri. Jika diaktifkan, kata sandi yang tidak cocok dengan kriteria yang ditentukan akan ditolak oleh MySQL dengan suatu kesalahan. Akan lebih aman jika Anda tetap menonaktifkan validasi, tetapi Anda harus selalu menggunakan kata sandi yang kuat dan unik untuk kredensial basis data.

Jawab Y untuk ya, atau jawaban lain untuk melanjutkan tanpa mengaktifkan.
<pre>
VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?

Press y|Y for Yes, any other key for No:</pre>
Jika Anda menjawab “ya”, Anda akan diminta untuk memilih tingkat validasi kata sandi. Harap ingat bahwa jika Anda memasukkan 2 sebagai tingkat terkuat, Anda akan menjumpai kesalahan saat berusaha menentukan kata sandi yang tidak mengandung angka, huruf kapital dan huruf kecil, serta karakter khusus, atau kata sandi yang berdasarkan pada kata-kata kamus umum.
<pre>
There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary              file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1</pre>
Terlepas dari pilihan pengaturan VALIDATE PASSWORD PLUGIN, server Anda akan meminta Anda untuk memilih dan mengonfirmasi kata sandi untuk pengguna root MySQL. Ini tidak sama dengan root sistem. Pengguna root basis data adalah pengguna administratif dengan privilese penuh terhadap sistem basis data. Meskipun metode autentikasi asali untuk pengguna root MySQL mengecualikan penggunaan kata sandi, sekalipun kata kata sandi sudah dibuat, Anda tetap harus menentukan kata sandi yang kuat di sini sebagai langkah keamanan tambahan. Kita akan membahas hal ini sebentar lagi.

Jika Anda mengaktifkan validasi kata sandi, Anda akan diperlihatkan kekuatan kata sandi untuk kata sandi root yang baru saja Anda masukkan dan server Anda akan bertanya apakah Anda ingin melanjutkan dengan kata sandi itu. Jika Anda puas dengan kata sandi ini, tekan Y untuk “ya” di prompt:
<pre>
Estimated strength of the password: 100
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y</pre>
Untuk pertanyaan lainnya, tekan Y dan tombol ENTER pada setiap pertanyaan. Ini akan menghapus sebagian pengguna anonim dan basis data percobaan, menonaktifkan log masuk root dari jarak jauh, dan memuat aturan-aturan baru ini sehingga MySQL segera menerapkan perubahan yang Anda buat.

Setelah Anda selesai, lakukan percobaan apakah Anda dapat melakukan log masuk ke konsol MySQL dengan mengetik:

<pre>sudo mysql</pre>
 
Ini akan menghubungkan ke server MySQL sebagai root pengguna basis data administratif, yang ditentukan berdasarkan penggunaan sudo saat menjalankan perintah ini. Anda akan melihat keluaran seperti ini:
<pre>
Output :
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 22
Server version: 8.0.19-0ubuntu5 (Ubuntu)

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

</pre>
Untuk keluar dari konsol MySQL, ketik:

<pre>mysql> exit</pre>

Perhatikan bahwa Anda tidak perlu memberikan kata sandi untuk terhubung sebagai pengguna root, meskipun Anda telah menentukannya saat menjalankan skrip mysql_secure_installation. Hal itu dikarenakan metode autentikasi asali untuk pengguna MySQL administratif adalah unix_socket alih-alih kata sandi. Meskipun awalnya ini mungkin terlihat seperti masalah keamanan, tetapi ini membuat server basis data menjadi lebih aman karena pengguna yang diizinkan melakukan log masuk sebagai pengguna MySQL root hanya pengguna sistem dengan privilese sudo yang terhubung dari konsol atau melalui aplikasi yang berjalan dengan privilese yang sama. Dalam praktiknya, itu berarti Anda tidak akan dapat menggunakan pengguna root basis data administratif untuk terhubung dari aplikasi PHP. Pengaturan kata sandi untuk akun MySQL root berfungsi sebagai perlindungan, apabila metode autentikasi asali diubah dari unix_socket menjadi kata sandi.

Untuk keamanan yang lebih baik, saran terbaiknya adalah memiliki akun pengguna khusus dengan pengaturan privilese yang lebih sempit untuk setiap basis data, terutama jika Anda berencana memiliki beberapa basis data di mana server Anda adalah hosnya.

Catatan: Saat menyusun tulisan ini, pustaka PHP MySQL asli mysqlnd tidak mendukung caching_sha2_authentication, metode autentikasi asali untuk MySQL 8. Karena itu, saat menciptakan pengguna basis data untuk aplikasi PHP di MySQL 8, Anda perlu memastikan pengguna telah dikonfigurasi untuk menggunakan mysql_native_password. Kami akan mendemonstrasikan caranya di Langkah 6.

Server MySQL Anda kini telah terinstal dan terlindungi. Selanjutnya, kita akan menginstal PHP, komponen terakhir dalam tumpukan LAMP.

# Langkah 3 — Menginstal PHP
Anda telah memiliki Apache terinstal untuk menyajikan konten dan MySQL terinstal untuk menyimpan dan mengelola data Anda. PHP adalah komponen persiapan kita yang akan memproses kode untuk menampilkan konten dinamis ke pengguna akhir. Selain paket 'php', Anda akan memerlukan 'php-mysql', suatu modul PHP yang memungkinkan PHP berkomunikasi dengan basis data yang berbasis MySQL. Anda juga akan memerlukan 'libapache2-mod-php' untuk memungkinkan Apache menangani berkas PHP. Paket PHP inti akan secara otomatis terinstal sebagai dependensi.

Untuk menginstal paket ini, jalankan:

<pre>sudo apt install php libapache2-mod-php php-mysql</pre>
 
Setelah instalasi selesai, Anda dapat menjalankan perintah berikut ini untuk mengonfirmasi versi PHP Anda:

<pre>php -v</pre>
<Pre> 
Output :
PHP 7.4.3 (cli) (built: Mar 26 2020 20:24:23) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
    with Zend OPcache v7.4.3, Copyright (c), by Zend Technologies</pre>
Di titik ini, tumpukan LAMP Anda sudah berfungsi sepenuhnya, tetapi sebelum Anda dapat menguji setelan Anda dengan skrip PHP, sebaiknya instal Apache Virtual Host yang tepat untuk menyimpan berkas dan folder situs web Anda. Kita akan melakukan itu di langkah selanjutnya.

# Langkah 4 — Menciptakan Hos Virtual untuk Situs Web Anda
Ketika menggunakan server web Apache, Anda bisa menciptakan hos virtual (serupa dengan blok server di Nginx) untuk merangkum detail konfigurasi dan menjadi hos dari lebih dari satu domain dari server tunggal. Dalam panduan ini, kita akan menyiapkan domain bernama your_domain, tetapi Anda harus menggantinya dengan nama domain Anda sendiri.

Catatan: Jika Anda menggunakan DigitalOcean sebagai penyedia hos DNS, lihat dokumen produk kami untuk instruksi mendetail tentang cara mempersiapkan nama domain baru dan mengarahkannya ke server Anda.

Apache pada Ubuntu 20.04 memiliki satu blok server yang aktif secara asali, yang dikonfigurasi untuk menyajikan dokumen-dokumen dari direktori /var/www/html. Meskipun ini berfungsi baik untuk situs tunggal, ini bisa menjadi sulit dijalankan jika Anda menjadi hos dari beberapa situs. Alih-alih memodifikasi /var/www/html, kita akan menciptakan suatu struktur direktori dalam /var/www untuk situs your_domain, dengan membiarkan /var/www/html sebagai direktori asali yang akan ditampilkan jika permintaan klien tidak cocok dengan situs lain.

Buat direktori untuk your_domain sebagai berikut:

<pre> sudo mkdir /var/www/your_domain</pre>
 
Selanjutnya, tentukan kepemilikan direktori dengan variabel lingkungan $USER, yang akan merujuk pada pengguna sistem Anda saat ini:

<pre> sudo chown -R $USER:$USER /var/www/your_domain</pre>
 
Kemudian, buka berkas konfigurasi baru dalam direktori sites-available Apache dengan menggunakan editor baris perintah yang Anda sukai. Di sini, kita akan menggunakan nano:

<pre>sudo nano /etc/apache2/sites-available/your_domain.conf</pre>
 
Ini akan menciptakan berkas kosong yang baru. Tempelkan konfigurasi dasar berikut:
<pre>
/etc/apache2/sites-available/your_domain.conf
<VirtualHost *:80>
    ServerName your_domain
    ServerAlias www.your_domain
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/your_domain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
</pre> 
Simpan dan tutup berkas setelah Anda selesai. Jika Anda menggunakan nano, Anda dapat melakukannya dengan menekan CTRL+X, kemudian Y dan ENTER.

Dengan konfigurasi VirtualHost ini, kita menyuruh Apache untuk menyajikan your_domain menggunakan /var/www/your_domain sebagai direktori root web. Jika Anda ingin menguji Apache tanpa nama domain, Anda dapat menghapus atau memberikan komentar pada opsi ServerName dan ServerAlias dengan menambahkan karakter # di depan masing-masing baris opsi.

Anda dapat menggunakan a2ensite untuk mengaktifkan hos virtual yang baru:

<pre>sudo a2ensite your_domain</pre>
 
Anda mungkin ingin menonaktifkan situs web asali yang terinstal dengan Apache. Ini diperlukan jika Anda tidak menggunakan nama domain khusus, karena dalam hal ini, konfigurasi asali Apache akan menimpa hos virtual Anda. Untuk menonaktifkan situs web asali Apache, ketikkan:

<pre>sudo a2dissite 000-default</pre>
 
Untuk memastikan berkas konfigurasi Anda tidak berisi kesalahan sintaks, jalankan:

<pre>sudo apache2ctl configtest</pre>
 
Terakhir, muat ulang Apache agar perubahan ini diterapkan:

<pre>sudo systemctl reload apache2</pre>
 
Situs web Anda yang baru kini sudah aktif, tetapi root web /var/www/your_domain masih kosong. Buat berkas index.html di dalam lokasi itu sehingga kita dapat menguji apakah hos virtual bekerja sesuai harapan:

<pre>nano /var/www/your_domain/index.html</pre>
 
Sertakan konten berikut di dalam berkas ini:
<pre>
/var/www/your_domain/index.html
<html>
  <head>
    <title>your_domain website</title>
  </head>
  <body>
    <h1>Hello World!</h1>

    <p>This is the landing page of <strong>your_domain</strong>.</p>
  </body>
</html>
</pre> 
Sekarang, buka peramban Anda dan akses nama domain server atau alamat IP Anda sekali lagi:

http://server_domain_or_IP
Anda akan melihat sebuah laman seperti ini: 
<img src="https://assets.digitalocean.com/articles/lemp_ubuntu2004/landing_page.png">

Jika Anda melihat laman ini, ini berarti hos virtual Apache Anda bekerja sesuai harapan.

Anda dapat meninggalkan berkas ini di lokasinya saat ini sebagai laman landas sementara untuk aplikasi Anda sampai Anda menyiapkan berkas index.php untuk menggantinya. Setelah Anda melakukannya, ingat untuk menghapus atau mengganti nama berkas index.html dari root dokumen Anda, karena berkas ini lebih diutamakan daripada berkas index.php secara asali.

Catatan Tentang DirectoryIndex pada Apache
Dengan pengaturan DirectoryIndex asali pada Apache, berkas yang diberi nama index.html akan selalu lebih diutamakan daripada berkas index.php. Ini berguna untuk menyiapkan laman pemeliharaan di aplikasi PHP, dengan menciptakan berkas index.html sementara yang mengandung suatu pesan informatif bagi pengunjung. Karena lebih diutamakan daripada laman index.php, laman ini akan menjadi laman landas untuk aplikasi. Setelah pemeliharaan selesai, index.html diubah namanya atau dihapus dari root dokumen, sehingga mengembalikan laman aplikasi reguler.

Jika Anda ingin mengubah perilaku ini, Anda akan perlu mengedit berkas /etc/apache2/mods-enabled/dir.conf dan memodifikasi urutan di mana berkas index.php terdaftar di dalam arahan DirectoryIndex:

<pre>sudo nano /etc/apache2/mods-enabled/dir.conf</pre>
<pre> 
/etc/apache2/mods-enabled/dir.conf
<IfModule mod_dir.c>
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
</pre> 
Setelah menyimpan dan menutup berkas, Anda perlu memuat ulang Apache agar perubahan tersebut diterapkan:

<pre>sudo systemctl reload apache2</pre>
 
Dalam langkah berikutnya, kita akan menciptakan skrip PHP untuk menguji apakah PHP telah terinstal dan terkonfigurasi dengan benar di server Anda.

# Langkah 5 — Menguji Pemrosesan PHP pada Server Web
Kini setelah Anda memiliki lokasi khusus untuk menjadi hos dari berkas dan folder situs web Anda, kita akan menciptakan skrip percobaan PHP untuk mengonfirmasi bahwa Apache dapat menangani dan memproses permintaan untuk berkas PHP.

Buat berkas baru bernama info.php di dalam folder root web khusus Anda:

<pre> nano /var/www/your_domain/info.php 
/var/www/your_domain/info.php
</pre>
 
Ini akan membuka suatu berkas kosong. Tambahkan teks berikut, yang merupakan kode PHP yang valid, di dalam berkas:
<pre>
/var/www/your_domain/info.php
?php
phpinfo();
</pre>
Setelah Anda selesai, simpan dan tutup berkas.

Untuk menguji skrip ini, buka peramban web Anda dan akses alamat IP atau nama domain server Anda, diikuti dengan nama skrip, yang dalam hal ini adalah info.php:

http://server_domain_or_IP/info.php
Anda akan melihat halaman yang serupa dengan ini:
<img src="https://assets.digitalocean.com/articles/lamp_ubuntu2004/phpinfo.png">

Laman ini menyajikan informasi tentang server Anda dari sudut pandang PHP. Ini berguna untuk mengawakutu dan memastikan pengaturan Anda telah diterapkan dengan benar.

Jika Anda dapat melihat laman ini di peramban Abda, maka instalasi PHP Anda bekerja sesuai harapan.

Setelah memeriksa informasi yang relevan mengenai server PHP Anda melalui laman itu, sebaiknya hapus berkas yang Anda buat, karena berkas tersebut mengandung informasi sensitif tentang lingkungan PHP A dan server Ubuntu Anda. Anda dapat menggunakan rm untuk melakukannya:

<pre> sudo rm /var/www/your_domain/info.php </pre>
 
Anda selalu dapat menciptakan kembali laman ini jika Anda perlu mengakses kembali informasi tersebut sewaktu-waktu

 
# Langkah 6 — Menguji Koneksi Basis Data dari PHP
Jika Anda ingin menguji apakah PHP dapat terhubung ke MySQL dan menjalankan kueri basis data, Anda dapat menciptakan tabel percobaan dengan kueri dan data semu sebagai kontennya dari skrip PHP. Sebelum melakukan itu, kita perlu menciptakan basis data percobaan dan pengguna MySQL baru yang dikonfigurasi dengan benar untuk mengaksesnya.

Saat menyusun tulisan ini, pustaka PHP MySQL asli mysqlndtidak mendukung caching_sha2_authentication, metode autentikasi asali untuk MySQL 8. Kita akan perlu menciptakan pengguna baru dengan metode autentikasi mysql_native_password agar dapat terhubung ke basis data MySQL dari PHP.

Kita akan menciptakan basis data bernama example_database dan pengguna bernama example_user, tetapi Anda dapat mengganti nama-nama ini dengan nilai yang berbeda.

Pertama, hubungkan ke konsol MySQL menggunakan akun root:

<pre> sudo mysql</pre>
 
Untuk menciptakan basis data baru, jalankan perintah berikut dari konsol MySQL:

<pre>CREATE DATABASE example_database;</pre>
 
Sekarang Anda dapat menciptakan pengguna baru dan memberikan mereka privilese penuh pada basis data khusus yang baru saja Anda ciptakan.

Perintah berikut menciptakan pengguna baru bernama example_user, yang menggunakan mysql_native_password sebagai metode autentikasi asali. Kita mendefinisikan kata sandi pengguna ini sebagai password, tetapi Anda harus mengganti nilai ini dengan kata sandi pilihan Anda yang aman.

<pre>CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';</pre>
 
Sekarang, kita perlu memberikan izin terhadap basis data example_database kepada pengguna ini:

<pre>GRANT ALL ON example_database.* TO 'example_user'@'%';</pre>
 
Ini akan memberikan pengguna example_user privilese penuh terhadap basis data example_database database, yang juga mencegah pengguna ini untuk tidak membuat atau memodifikasi basis data lainnya di server Anda.

Sekarang, keluar dari shell MySQL dengan:

<pre>exit</pre>
 
Anda dapat menguji apakah pengguna baru memiliki izin yang tepat dengan melakukan log masuk ke konsol MySQL lagi, kali ini dengan menggunakan kredensial pengguna khusus:

<pre>mysql -u example_user -p</pre>
 
Perhatikan tanda -p dalam perintah ini, yang akan meminta Anda kata sandi yang digunakan saat menciptakan pengguna example_user. Setelah log masuk ke konsol MySQL, pastikan Anda memiliki akses ke basis data example_database:

<pre>SHOW DATABASES;</pre>
 
Ini akan menampilkan keluaran berikut:
<pre>
Output :
+--------------------+
| Database           |
+--------------------+
| example_database   |
| information_schema |
+--------------------+
2 rows in set (0.000 sec)</pre>
Selanjutnya, kita akan menciptakan tabel percobaan bernama todo_list. Dari konsol MySQL, jalankan pernyataan berikut:
<pre>
CREATE TABLE example_database.todo_list (
    item_id INT AUTO_INCREMENT,
    content VARCHAR(255),
    PRIMARY KEY(item_id)
);
 </pre>
Masukkan beberapa baris konten ke dalam tabel percobaan. Anda mungkin ingin mengulangi perintah selanjutnya beberapa kali lagi, dengan menggunakan nilai yang berbeda:
<pre>
INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
 </pre>
Untuk memastikan data berhasil tersimpan di tabel, jalankan:
<pre>
SELECT * FROM example_database.todo_list;
 </pre>
Anda akan melihat keluaran berikut:
<pre>
Output
+---------+--------------------------+
| item_id | content                  |
+---------+--------------------------+
|       1 | My first important item  |
|       2 | My second important item |
|       3 | My third important item  |
|       4 | and this one more thing  |
+---------+--------------------------+
4 rows in set (0.000 sec)
</pre>
Setelah memastikan Anda memiliki data yang valid dalam tabel percobaan, Anda dapat keluar dari konsol MySQL:

<pre>exit</pre>
 
Sekarang Anda dapat menciptakan skrip PHP yang akan terhubung ke MySQL dan melakukan kueri untuk konten Anda. Buat berkas PHP baru pada direktori root web khusus Anda menggunakan editor pilihan Anda. Kita akan menggunakan nano untuk itu:

<pre>nano /var/www/your_domain/todo_list.php</pre>
 
Skrip PHP berikut terhubung ke basis data MySQL dan melakukan kueri untuk konten tabel todo_list, yang menampilkan hasilnya dalam sebuah daftar. Jika ada masalah dengan koneksi basis data, akan dilakukan pengecualian. Salin konten ini ke skrip todo_list.php:
<pre>
/var/www/your_domain/todo_list.php
<?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>";
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
</pre> 
Simpan dan tutup berkas setelah Anda selesai mengedit.

Anda dapat mengakses laman ini di peramban web Anda dengan mengunjungi nama domain atau alamat IP publik yang dikonfigurasi untuk situs web Anda, diikuti dengan /todo_list.php:

http://your_domain_or_IP/todo_list.php
Anda akan melihat laman seperti ini, yang menunjukkan konten yang telah Anda masukkan ke dalam tabel percobaan Anda:
<img src="https://assets.digitalocean.com/articles/lemp_debian10/todo_list.png">
Contoh todo list PHP

Ini berarti lingkungan PHP Anda telah siap terhubung dan berinteraksi dengan server MySQL.

Kesimpulan
Dalam panduan ini, kita telah membangun fondasi yang fleksibel untuk menyajikan aplikasi dan situs web PHP kepada pengunjung Anda, dengan menggunakan Apache sebagai server web dan MySQL sebagai sistem basis data.

Sebagai langkah berikutnya, Anda harus memastikan bahwa koneksi ke server web Anda sudah aman, dengan melayaninya melalui HTTPS. Untuk melakukannya, Anda dapat menggunakan Let’s Encrypt untuk mengamankan situs Anda dengan sertifikat TLS/SSL gratis.



## Layanan pengaktifan / penonaktifan sementara

Untuk menghentikan dan memulai layanan sementara (Tidak mengaktifkan / menonaktifkannya untuk booting di masa mendatang), Anda dapat mengetik service SERVICE_NAME. Sebagai contoh:

<pre>sudo service apache2 stop</pre>(Akan BERHENTI layanan Apache sampai Reboot atau sampai Anda memulainya lagi).

<pre>sudo service apache2 start</pre>(Akan MULAI layanan Apache dengan asumsi itu dihentikan sebelumnya.)

<pre>service apache2 status</pre> (Akan memberitahu Anda STATUS layanan, jika diaktifkan / dijalankan dinonaktifkan / TIDAK berjalan.).

<pre>sudo service apache2 restart</pre>(Akan MEMULAI layanan. Ini paling umum digunakan ketika Anda telah mengubah, file konfigurasi. Dalam hal ini, jika Anda mengubah konfigurasi PHP atau konfigurasi Apache. Restart akan menyelamatkan Anda dari keharusan berhenti / mulai dengan 2 baris perintah) )

<pre>service apache2</pre>(Dalam hal ini, karena Anda tidak menyebutkan TINDAKAN untuk mengeksekusi untuk layanan, itu akan menunjukkan kepada Anda semua opsi yang tersedia untuk layanan tertentu.) Aspek ini bervariasi tergantung pada layanan, misalnya, dengan MySQL hanya akan menyebutkan bahwa itu tidak memiliki parameter. Untuk layanan lain seperti layanan jaringan, akan disebutkan daftar kecil dari semua opsi yang tersedia.


## SYSTEMD
Dimulai dengan Ubuntu 15,04, Pemula akan ditinggalkan karena Systemd. Dengan Systemd untuk mengelola layanan, kami dapat melakukan hal berikut:

<li>`systemctl start SERVICE`- Gunakan untuk memulai layanan. Tidak bertahan setelah reboot</li>

<li>systemctl stop SERVICE- Gunakan untuk menghentikan layanan. Tidak bertahan setelah reboot</li>

<li>systemctl restart SERVICE - Gunakan untuk memulai kembali layanan</li>

<li>systemctl reload SERVICE - Jika layanan mendukungnya, itu akan memuat ulang file konfigurasi yang terkait dengannya tanpa mengganggu proses apa pun yang menggunakan layanan ini.</li>

<li>systemctl status SERVICE- Menunjukkan status layanan. Memberitahu apakah suatu layanan sedang berjalan.</li>

<li>systemctl enable SERVICE- Menyalakan layanan, pada reboot berikutnya atau pada acara mulai berikutnya. Ini berlanjut setelah reboot.</li>

<li>systemctl disable SERVICE- Menonaktifkan layanan pada boot ulang berikutnya atau pada perhentian berikutnya. Ini berlanjut setelah reboot.</li>

<li>systemctl is-enabled SERVICE - Periksa apakah suatu layanan saat ini dikonfigurasi untuk memulai atau tidak pada reboot berikutnya.</li>

<li>systemctl is-active SERVICE - Periksa apakah suatu layanan saat ini aktif.</li>

<li>systemctl show SERVICE - Tampilkan semua informasi tentang layanan ini.</li>

<li>sudo systemctl mask SERVICE- Menonaktifkan sepenuhnya layanan dengan menautkannya ke /dev/null; Anda tidak dapat memulai layanan secara manual atau mengaktifkan layanan.</li>

<li>sudo systemctl unmask SERVICE- Menghapus tautan ke /dev/nulldan mengembalikan kemampuan untuk mengaktifkan dan atau memulai layanan secara manual.</li>

<li>UPSTART (Tidak Digunakan Sejak 15.04)</li>
Jika kami ingin menggunakan cara Pemula resmi (Perhatikan bahwa, untuk saat ini, tidak semua layanan telah dikonversi ke Pemula), kami dapat menggunakan perintah berikut:</li>

<li>status SERVICE- Ini akan memberi tahu kami jika layanan yang dikonversi sedang berjalan atau tidak. Perhatikan bahwa ini usang dalam mendukung `start, stop, status& restart.` Ini juga akan memberi tahu kami jika layanan belum dikonversi ke pemula:</li>

Layanan yang dikonversi biasanya akan mengeluarkan status saat ini (Memulai, Menjalankan, Menghentikan ...) dan memproses ID. Layanan yang tidak dikonversi akan memberikan kesalahan tentang pekerjaan yang tidak dikenal .

Beberapa pintasan hanya dapat bekerja dengan serviceperintah di atas tetapi tidak dengan perintah di bawah kecuali jika 100% dikonversi ke layanan pemula:

<li>MULAI -sudo start mysql</li>

<li>BERHENTI -sudo stop mysql</li>

<li>Restart -sudo restart mysql</li>

<li>STATUS -sudo status smbd</li>

## Mengaktifkan / Menonaktifkan layanan
Untuk mengganti layanan dari memulai atau berhenti secara permanen, Anda perlu:

<pre>echo manual | sudo tee /etc/init/SERVICE.override</pre>
di mana bait manualakan menghentikan Pemula dari memuat layanan secara otomatis pada boot berikutnya. Setiap layanan dengan .overrideakhiran akan diutamakan daripada file layanan asli. Anda hanya akan dapat memulai layanan secara manual setelahnya. Jika Anda tidak ingin ini maka cukup hapus .override. Sebagai contoh:

<pre>echo manual | sudo tee /etc/init/mysql.override</pre>
Akan menempatkan layanan MySQL ke manualmode. Jika Anda tidak menginginkan ini, maka Anda bisa melakukannya

<pre>sudo rm /etc/init/mysql.override</pre>
dan Reboot untuk memulai layanan lagi secara otomatis. Tentu saja untuk mengaktifkan layanan, cara yang paling umum adalah dengan menginstalnya. Jika Anda menginstal Apache, Nginx, MySQL atau yang lain, mereka secara otomatis mulai setelah menyelesaikan instalasi dan akan mulai setiap kali komputer melakukan boot. Penonaktifan, seperti yang disebutkan di atas, akan menggunakan layanan ini manual.
