# OpenStack Xena : Konfigurasi Nova #3

Instal OpenStack Compute Service (Nova).
Contoh ini didasarkan pada lingkungan seperti berikut.
Jika Anda ingin menginstal Nova Compute di Host lain, lihat di sini .
<pre>
eth0|10.0.0.30
+-----------+-----------+
| [ Simpul Kontrol ] |
| |
| MariaDB RabbitMQ |
| Memcache httpd |
| Sekilas Keystone |
| Nova API,Komputasi |
+-----------------------+</pre>

## [1]	 Instal KVM HyperVisor di Compute Host, lihat di sini .
Tidak perlu mengatur jaringan Bridge di bagian [2] dari tautan.

## [2]	Instal Nova Compute.
<pre>root@dlp ~(batu kunci)#apt -y install nova-compute nova-compute-kvm</pre>

##  [3]	Selain pengaturan dasar Nova , tambahkan pengaturan berikut.
<pre>
root@dlp ~(batu kunci)#vi /etc/nova/nova.conf
# tambahkan berikut (aktifkan VNC)
[vnc]
diaktifkan = Benar
server_listen = 0.0.0.0
server_proxyclient_address = 10.0.0.30
novncproxy_base_url = http://10.0.0.30:6080/vnc_auto.html</pre>


## [4]	Mulai layanan Nova Compute.
<pre>
root@dlp ~(batu kunci)#systemctl restart nova-compute nova-novncproxy
# temukan Compute Node
root@dlp ~(batu kunci)#su -s /bin/bash nova -c "nova-manage cell_v2 find_hosts"
# tampilkan status
root@dlp ~(batu kunci)#daftar layanan komputasi openstack
+----+----------------+---------------+----------+ ---------+-------+----------------------------+
| ID | Biner | Tuan rumah | Zona | Status | Negara | Diperbarui Pada |
+----+----------------+---------------+----------+ ---------+-------+----------------------------+
| 3 | nova-scheduler | dlp.srv.world | internal | diaktifkan | naik | 2021-10-07T00:44:01.000.000 |
| 7 | nova-konduktor | dlp.srv.world | internal | diaktifkan | naik | 2021-10-07T00:43:58,000,000 |
| 13 | nova-komputasi | dlp.srv.world | nova | diaktifkan | naik | 2021-10-07T00:44:05.000000 |
+----+----------------+---------------+----------+ ---------+-------+----------------------------+</pre>
