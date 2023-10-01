# OpenStack Xena : Konfigurasikan Keystone #2

Tambahkan Proyek di Keystone.
Contoh ini didasarkan pada lingkungan seperti berikut.
<pre>
eth0|10.0.0.30
+-----------+-----------+
| [ Simpul Kontrol ] |
| |
| MariaDB RabbitMQ |
| Memcache httpd |
| Batu Kunci |
+-----------------------+</pre>

## [1]	Muat variabel lingkungan terlebih dahulu.
Nilai untuk [OS_PASSWORD] berasal dari kata sandi saat mengonfigurasi keystone bootstrap .
Untuk [OS_AUTH_URL], tentukan nama host atau alamat IP server Keystone.

<pre>
root@dlp:~#vi ~/keystonerc
ekspor OS_PROJECT_DOMAIN_NAME=
ekspor default OS_USER_DOMAIN_NAME=ekspor default
OS_PROJECT_NAME=admin
ekspor OS_USERNAME=admin
ekspor OS_PASSWORD= ekspor kata sandi admin
OS_AUTH_URL=http://10.0.0.30:5000/v3
ekspor OS_IDENTITY_API_VERSION=3
ekspor OS_IMAGE_API_VERSION=2
ekspor\ PS_VERSION=2 h \W(batu kunci)\$ '
root@dlp:~#chmod 600 ~/keystonerc
root@dlp:~#sumber ~/keystonerc
root@dlp ~(batu kunci)#echo "sumber ~/keystonerc" >> ~/.bashrc</pre>

## [2]	Tambahkan Proyek.
<pre>
# buat proyek [layanan]
root@dlp ~(batu kunci)#proyek openstack buat --domain default --deskripsi layanan "Proyek Layanan"
+-------------+----------------------------------+
| lapangan | Nilai |
+-------------+----------------------------------+
| deskripsi | Proyek Layanan |
| domain_id | default |
| diaktifkan | Benar |
| id | 611e3ba8938b475fb9dd8124603f18d3 |
| is_domain | Salah |
| nama | layanan |
| pilihan | {} |
| parent_id | default |
| tag | [] |
+-------------+----------------------------------+

# konfirmasi pengaturan
root@dlp ~(batu kunci)#daftar proyek openstack
+-----------------------------------+--------+
| ID | Nama |
+-----------------------------------+--------+
| 611e3ba8938b475fb9dd8124603f18d3 | layanan |
| f4431102f2d9415590e3ac11c616858a | admin |
+-----------------------------------+--------+</pre>
