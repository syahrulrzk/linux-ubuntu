# OpenStack Xena: Pra-Persyaratan

Ini adalah contoh Cloud Computiong oleh OpenStack Xena.
Instal beberapa layanan yang dibutuhkan beberapa komponen OpenStack untuk persyaratan sistem di sini.
Contoh ini didasarkan pada lingkungan seperti berikut.
<pre>
        eth0|10.0.0.30
+-----------+-----------+
| [ Simpul Kontrol ] |
| |
| MariaDB RabbitMQ |
| Memcache |
+-----------------------+</pre>

## Konfigurasi NTP untuk menyesuaikan tanggal, lihat di sini .

## Instal MariaDB Server, lihat di sini .

## Konfigurasikan repositori Openstack Xena.

<pre>
root@dlp:~#apt -y install software-properties-common
root@dlp:~#add-apt-repository cloud-archive:xena
root@dlp:~#pembaruan yang tepat
root@dlp:~#apt -y peningkatan</pre>

## Instal RabbitMQ, Memcached.

<pre>
root@dlp:~#apt -y install rabbitmq-server memcached python3-pymysql
# tambahkan pengguna ke RabbitMQ
# atur kata sandi apa pun untuk [kata sandi]
root@dlp:~#kelincimqctl add_user kata sandi openstack
Membuat pengguna "openstack" ...
root@dlp:~#rabbitmqctl set_permissions openstack ".*" ".*" ".*"
Mengatur izin untuk pengguna "openstack" di vhost "/" ...
root@dlp:~#vi /etc/mysql/mariadb.conf.d/50-server.cnf
# baris 28 : ubah
mengikat-alamat =0.0.0.0
# baris 40 : batalkan komentar dan ubah
# nilai default 151 tidak cukup di Openstack Env
max_koneksi =500
root@dlp:~#vi /etc/memcached.conf
# baris 35: ubah
-l0.0.0.0
root@dlp:~#systemctl restart mariadb rabbitmq-server memcached</pre>
