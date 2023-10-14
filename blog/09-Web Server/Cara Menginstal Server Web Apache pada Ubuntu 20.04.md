Server HTTP Apache adalah server web yang paling banyak digunakan di dunia. Server web ini menyediakan banyak fitur canggih termasuk modul yang dapat dimuat secara dinamis, dukungan media yang kokoh, dan integrasi ekstensif dengan perangkat lunak populer lainnya.

## Prasyaratan 

## Langkah 1 - Menginstall Apache
Apache tersedia di dalam repositori perangkat lunak asali Ubuntu, yang memungkinkan Apache terinstal dengan menggunakan alat manajemen paket konvensional.

Mari kita mulai dengan memperbarui indeks paket lokal untuk mencerminkan perubahan hulu terbaru:
<pre>
sudo apt update
</pre>
Lalu, instal paket apache2:
<pre>
  sudo apt install apache2
</pre>

Setelah mengonfirmasi instalasi, apt akan menginstal Apache dan semua dependensi yang diperlukan.
## Langkah 2 — Menyesuaikan Firewall

Sebelum menguji Apache, Anda perlu memodifikasi pengaturan firewall untuk mengizinkan akses dari luar ke porta web asali. Dengan asumsi bahwa Anda telah mengikuti instruksi di prasyarat, Anda seharusnya memiliki firewall UFW yang terkonfigurasi untuk membatasi akses ke server Anda.

Selama instalasi, Apache mendaftarkan dirinya dengan UFW untuk menyediakan beberapa profil aplikasi yang dapat digunakan untuk mengaktifkan atau menonaktifkan akses ke Apache melalui firewall.

Buat daftar profil aplikasi ufw dengan mengetik:
<pre>
  sudo ufw app list

  -----------
  Output
Available applications:
  Apache
  Apache Full
  Apache Secure
  OpenSSH
</pre>

