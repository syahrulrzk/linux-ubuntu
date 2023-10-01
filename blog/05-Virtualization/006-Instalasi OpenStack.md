# Cara Menginstal OpenStack di Ubuntu dengan DevStack

OpenStack adalah infrastruktur komputasi awan (IaaS) yang membantu mengendalikan kumpulan besar daya komputasi, penyimpanan, dan sumber daya jaringan di seluruh pusat data. Ia melakukannya dengan bantuan API. Singkatnya, OpenStack membantu dalam membangun dan mengelola Cloud Publik dan Pribadi, dengan menggunakan sumber daya virtual yang dikumpulkan.

Devstack adalah serangkaian skrip yang dapat diperluas, yang digunakan untuk mengatur lingkungan OpenStack dengan mudah. Ini banyak digunakan, karena memberikan lingkungan interaktif untuk pengembangan dengan OpenStack.

Pada artikel ini, kita akan belajar cara mengatur OpenStack di sistem Ubuntu kita dengan bantuan Devstack.

## Prasyarat untuk menggunakan OpenStack
Ada beberapa prasyarat dasar yang perlu Anda penuhi, sebelum menyiapkan OpenStack di sistem Anda.

<ul>
  <li>OS Ubuntu</li>
  <li>RAM minimal 4 GB</li>
  <li>Prosesor berkemampuan multi-core</li>
  <li>Setidaknya 10GB ruang hard disk kosong</li>
  <li>Koneksi internet yang bagus</li>
</ul>
  
Ada beberapa persyaratan perangkat lunak tambahan juga, yang harus Anda penuhi.

<li>Git</li>
<li>Peramban web(chrome,firefox, dll)</li>

## Langkah-langkah untuk Menginstal Openstack di Ubuntu dengan Devstack
Menginstal OpenStack di Ubuntu adalah proses yang agak rumit. Tapi itu dipermudah oleh Devstack. Langkah-langkah untuk menginstalnya, cukup mudah bahkan jika Anda tidak terlalu mahir dengan baris perintah, cukup ikuti langkah-langkahnya dan jalankan.

### Langkah 1: Mempersiapkan sistem
Sebelum kita memulai, kita perlu memastikan bahwa sistem kita diperbarui , untuk itu jalankan perintah berikut:

<pre>sudo apt-get update && sudo apt-get upgrade -y</pre>

Perintah akan meminta hak akses root . Masukkan kata sandi pengguna Anda dan tunggu hingga sistem Anda ditingkatkan. Setelah pemutakhiran selesai, pastikan untuk me- reboot sistem Anda . Ini akan menginisialisasi dan mengatur upgrade Anda di reboot berikutnya.

### Langkah 2: Membuat pengguna tumpukan dengan hak istimewa Sudo
Sekarang, saatnya untuk memulai dengan langkah-langkah penting untuk menginstal Openstack di Ubuntu.

Kami pertama-tama akan membuat pengguna baru bernama stack untuk sistem kami untuk mengatur OpenStack, karena itu harus diinstal pada pengguna non-root dengan sudo diaktifkan.
Buka terminal baru, dan jalankan perintah useradd :

<pre>sudo useradd stack</pre>
<pre>sudo -i</pre>

Anda juga perlu mengaktifkan pengguna tumpukan untuk memiliki hak akses root dan berjalan tanpa kata sandi, untuk menjalankan itu:

<pre>echo "stack ALL=(ALL) NOPASSWD: ALL">>/etc/sudoers.d/stack
Outputnya akan terlihat seperti ini -</pre>

exit< dan caoba login ke user stack
Setelah Anda membuat pengguna stack , saatnya untuk masuk menggunakan perintah berikut:


### Langkah 3: Mengunduh Devstack
Untuk langkah ini, kami menganggap Anda sudah menginstal git di sistem Anda. Sekarang, masukkan perintah ini untuk mengunduh/mengkloning devstack dari repositorinya ke sistem Anda:

<pre>git clone https://opendev.org/openstack/devstack</pre>
Repo Devstack berisi skrip stack.sh , yang akan kita gunakan untuk menyiapkan OpenStack. Ini juga berisi template untuk file konfigurasi.

### Langkah 4: Membuat file konfigurasi (.conf) untuk Devstack
Sekarang, kami telah mengunduh DevStack dan perlu mengatur file konfigurasi kami untuk itu.

Anda harus terlebih dahulu menavigasi ke folder devstack, dengan menjalankan:

<pre>cd devstack</pre>
Setelah itu, buat file local.conf, dengan menjalankan:

<pre>	
vim local.conf</pre>
dan rekatkan konten berikut -
```
1 [[local|localr]]
2
3 ADMIN_PASSWORD=StrongAdminSecret
4 DATABASE_PASSWORD=$ADMIN_PASSWOCinder
5 RABBIT_PASSWORD=$ADMIN_PASSWORD
6 SERVICE_PASSWORD=$ADMIN_PASSWORD

```

Jika Anda tidak terlalu mengenal Vim, Anda dapat membaca tutorial vim . Untuk saat ini, Anda cukup menempelkan menggunakan mouse dengan mengklik kanan dan mengklik Tempel dan masuk :x untuk menyimpan & keluar.

Di sini, saya menggunakan konfigurasi minimal untuk menyiapkan DevStack, Anda dapat menjelajahi

Catatan:
1. <b>StrongAdminSecret</b> adalah kata sandi yang kami gunakan di sini, Anda dapat mengubahnya dengan pilihan Anda.
2. Anda dapat menemukan file konfigurasi sampel untu kb>local.conf</b> di direktori Samples di bawah repositori Devstack.

### Langkah 5: Menginstal Openstack dengan Devstack
Sekarang, karena kami telah mengatur file konfigurasi dengan benar.

Mari kita jalankan skrip untuk mengatur OpenStack di sistem kita, menggunakan perintah berikut:
<pre>./stack.sh</pre>
(Skrip yang kami gunakan ini adalah bagian dari DevStack itu sendiri)

Script akan menginstal fitur yang terdaftar untuk lingkungan OpenStack Anda –

<ul>
  <li>Horizon – Dasbor OpenStack</li>
  <li>Keystone – Layanan Identitas</li>
  <li>Nova – Layanan Komputasi</li>
  <li>Sekilas – Layanan Gambar</li>
  <li>Neutron – Layanan Jaringan</li>
  <li>Penempatan – Penempatan API</li>
  <li>Cinder – Layanan Penyimpanan Blok</li>
</ul>
Penyiapan akan memakan waktu sekitar 10 hingga 20 menit, berdasarkan kinerja sistem dan kecepatan internet Anda, karena banyak pohon git dan paket diinstal selama proses.

Setelah instalasi Anda berhasil selesai, terminal Anda akan terlihat seperti gambar di bawah ini.

-----------

Sekarang, kita dapat melihat bahwa dikatakan bahwa Horizon (Openstack Dashboard) tersedia di URL yang diberikan, itu akan bervariasi dari sistem ke sistem.

### Langkah 6: Mengakses OpenStack menggunakan browser web
Sekarang, karena kita telah berhasil mengatur OpenStack menggunakan Devstack, mari kita akses melalui browser kita.

Jelajahi URL ini di browser Anda –

<pre> https://server-ip/dashboard </pre>
Atau coba
<pre>https://localhost/dashboard</pre>

Ini akan membuka halaman login OpenStack, seperti yang ditunjukkan di bawah ini.

Sekarang, masukkan kredensial. Anda juga dapat masuk sebagai admin di sini, dengan memiliki Nama Pengguna sebagai admin & untuk Kata Sandi menggunakan yang kami tambahkan ke file <b>local.conf</b>.

Setelah masuk, dasbor Anda akan terlihat seperti ini.
