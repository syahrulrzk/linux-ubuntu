# Konfigurasikan Klien NTP.
 	
Klien NTP [systemd-timesyncd.service] berjalan secara default di Ubuntu, jadi mudah untuk mengatur Klien NTP.
Omong-omong, NTPd atau Chrony juga dapat digunakan sebagai Klien NTP. Jika Anda menggunakannya, cukup setel hanya server NTP untuk menyinkronkan waktu, jangan setel izin untuk menerima permintaan sinkronisasi waktu dari Host lain.

##	Konfigurasikan [systemd-timesyncd.service].

<pre>

root@klien:~#status systemctl systemd-timesyncd
* systemd-timesyncd.service - Sinkronisasi Waktu Jaringan
     Dimuat: dimuat (/lib/systemd/system/systemd-timesyncd.service; diaktifkan; ve>
     Aktif: aktif (berjalan) sejak Sen-27-04-2020 01:20:36 UTC; 2 menit 6 detik yang lalu
       Dokumen: man:systemd-timesyncd.service(8)
   PID Utama: 662 (systemd-timesyn)
     Status: "Sinkronisasi awal ke server waktu 91.189.94.4:123 (ntp.ubunt>
      Tugas: 2 (batas: 4621)
     Memori: 1.5M
     CGroup: /system.slice/systemd-timesyncd.service
             +- 662 /lib/systemd/systemd-timesyncd

root@klien:~#vi /etc/systemd/timesyncd.conf
# tambahkan ke akhir: atur server NTP untuk zona waktu Anda
NTP=dlp.srv.world
root@klien:~#systemctl restart systemd-timesyncd
root@klien:~#timedatectl timesync-status
       Server: 10.0.0.30 (dlp.srv.world)
Interval polling: 1 menit 4 detik (min: 32 detik; maks 34 menit 8 detik)
         Lompatan: normal
      Versi: 4
      Lapisan: 2
    Referensi: 3DCD7882
    Presisi: 1us (-25)
Jarak root: 7.964ms (maks: 5s)
       Offset: +6.050ms
        Penundaan: 412us
       Jitter: 0
 Jumlah paket: 1
    Frekuensi: +92.800ppm</pre>
