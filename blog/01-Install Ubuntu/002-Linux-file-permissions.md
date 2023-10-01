# File Permisions
Linux menerapkan hak akses file yang sangat ketat, setiap file akan dibeli label (file attribute) yang menjabarkan hak akses untuk user dan group terhadap file tersebut.
Hanya user dan group tertentu yang bisa membaca,memodifikasi dan mengeksekusi file tersebut.
Atribut ini sering disebut dengan file permission.

## Untuk Membatasi Akses user dan process ke File
Tujuan diberlakukan/set file permission pada linux adalah untuk alasan keamanan  yang tujuan utamanya adalah membatasi akses user terhadap suatu file.
Cara ini sangat efektif untuk memproteksi file sistem dari modifikasi user yang tidak bertanggung jawab (hacker).

# 

File dan directory di Unix mempunyai tiga (3) tipe ijin: read ("r"), write ("w"), dan execute ("x"). Setiap permission dapat di "on" atau "off" untuk masing-masing dari tiga (3) kategori dari pengguna:

owner dari file / directory
user lain dalam group yang sama dengan owner
semua yang lain (user se dunia)

## File
Untuk mengetahui mode / permission / ijin dari sebuah file dapat menggunakan

<pre>ls -lg filename</pre>
Perintah ini akan membuat message kira-kira,

<pre>-rwxr-x--x 1 owner    2300 Jul 14 14:38 filename</pre>
### Penjelasan / cara membacanya:

10 karakter di kiri, menunjukan mode / permission.
"-" karakter di paling awal menunjukan tipe file. "-" menunjukan file biasa. "d" menunjukan sebuah directory.
karakter 2-4 menunjukan permission bagi owner, "r", "w" atau "x" menunjukan ijin yang bagi owner. "-" jika tidak diberi permission.
karakter 5-7 menunjukan permission bagi group.
karakter 8-10 menunjukan permission bagi yang lain (seluruh dunia).
string ke dua menunjukan banyaknya link ke file tersebut
string ke tiga menunjukan owner dari file

Untuk mengubah mode dari sebuah file dapat menggunakan chmod, secara umum adalah,

<pre>chmod X@Y file1 file2 ...</pre>
dimana

X adalah huruf kombinasi dari "u" (untuk owner), "g" (untuk group), "o" (untuk others), "a" (untuk all; yaitu untuk "ugo")
@ bisa berisi "+" untuk menambahkan permission, "-" untuk membuang permission, atau "=" untuk mengalokasikan absolut permission
Y adalah kombinasi dari "r", "w", "x".
Contoh:

<pre>
chmod u=rx file        (Beri owner rx permission, tapi tidak w)
chmod go-rwx file      (Buang rwx permission untuk group, dan others)
chmod g+w file         (Beri write permission untuk group)
chmod a+x file1 file2  (Beri execute permission ke everybody)
chmod g+rx,o+x file    (OK untuk combine menggunakan koma)
</pre>

## Directory
Skema izin yang dijelaskan di atas juga berlaku untuk direktori. Untuk sebuah direktori,

siapa pun yang memiliki izin `read' dapat melihat daftar file menggunakan perintah ls (dan dengan demikian menemukan file apa yang ada di sana)
siapa pun yang memiliki izin `write' dapat membuat dan menghapus file dalam direktori itu;
Siapa pun yang telah melakukan izin bisa mengakses file atau subdirektori dari nama yang dikenal.
Untuk mengetahui mode sebuah directory,

<pre>ls -dl dir   - Perlihatkan permission untuk dir directory
ls -al dir   - Perlihatkan daftar panjang dari semua file yang ada di dir (directory), termasuk nama yang dimulai dengan '.'</pre>

Jika tidak ada direktori yang ditentukan, daftar untuk semua file dalam direktori saat ini. Outputnya akan terlihat seperti:
<pre>
drwx------12 fred        592 Jul 11 13:46 .
drwxr-xr-x24 root       1424 Jul 10 13:07 ..</pre>

Awal `d 'dalam string mode 10 karakter menunjukkan bahwa file adalah sebuah direktori. Nama file `. ' Selalu mengacu pada direktori saat ini; Nama file `.. 'selalu mengacu pada induk dari direktori saat ini.
Dengan demikian, output ini menunjukkan hak akses untuk direktori saat ini dan induknya.

## umask
Variable "umask" digunakan sebagai permission mask untuk semua file dan directory yang baru dibuat. umask adalah 3 digit octal.

Default mask adalah 022 = 000 010 010 binary. dua 1 bit tersebut akan menghalangi "group" dan "other" untuk write permission. Oleh karena itu, file yang baru akan mempunyai ijin
<pre>
rwx r-x r-x (user group other)</pre>
umask 077 = 000 111 111 akan menyebabkan no permissions untuk group and others, atau

<pre>rwx --- ---</pre>
Untuk mengunakan umask selain default, kita harus memasukan kalimat

<pre>umask num (dimana num adalah octal number)</pre>
di .cshrc file.

Untuk mengetahui lebih lanjut tentang umask, baca-baca

<pre>man umask</pre>

## chmod dengan angka
Sampai saat ini, kami telah mengatur mode dengan huruf. Ternyata kita juga bisa mengatur mode numerik. Begini cara kerjanya:

Tuliskan izin yang anda inginkan untuk dimiliki file tersebut. Agar hidup anda lebih mudah, tulislah izin yang dikelompokkan menjadi tiga set huruf. Sebagai contoh, katakanlah anda ingin file info.sh memiliki izin ini
<pre>- rwx r-x r-- info.sh</pre>
Di bawah setiap huruf, tuliskan angka 1; di bawah setiap "-" tulis angka nol. Abaikan "-" di awal yang memberitahu anda apakah itu file atau direktori. Ini memberi anda tiga bilangan biner.
<pre>- rwx r-x r-- info.sh
  111 101 100</pre>
Sekarang ubah setiap tiga digit menjadi satu digit dengan menggunakan tabel ini:
<pre>
Binary	Menjadi
000	0
001	1
010	2
011	3
100	4
101	5
110	6
111	7</pre>
Dari contoh kita, 111 101 100 diterjemahkan ke nomor 754.

Sekarang gunakan nomor itu dalam perintah chmod untuk mengatur izin yang anda inginkan pada file tersebut:

<pre>chmod 754 info.sh</pre>
### Contoh
Kadang kita perlu mengcopy sebuah file dari directory seseorang. Bagaimana caranya agar kita dapat mengakses directory tersebut?

Misalnya user 'joe' ingin mengcopy file 'prog.f' dari user 'fred' Di CLI, Fred perlu mengetik

<pre>chmod go+x ~</pre>
Perintah ini mengubah mode home directory Fred (direpresentasikan dengan ~), memberikan ijin ke semua user untuk mengambil file di directory tersebut. Oleh karena itu, Joe dapat mengakses semua file, yang dia ketahui namanya, di home directory Fred. Fred telah memberitahukan Joe bahwa file yang di inginkannya adalah 'prog.f', oleh karena itu Joe dapat mengetik

<pre>cp ~fred/prog.f prog.f</pre>
Jika Joe sudah ada file dengan nama 'prog.f', agar tidak di timpa, dia dapat menulis,

cp ~fred/prog.f prog2.f
Jika Joe menerima error message dari system saat mengakses karena 'permission deny', Fred harus membuat file tersebut bisa di baca oleh yang lain, dengan cara,

<pre>chmod go+r prog.f</pre>
Jika Joe ingin membuat beberpa copy file dari home directory Fred, contoh 'prog.a', 'prog.b', 'prog.c', dan memberikan nama yang sama di hpme directorynya, Fred dapat mengetik,

<pre>cp ~fred/prog.a ~fred/prog.b ~fred/prog.c .</pre>
Titik (.) di akhor perintah memberitahukan bahwa file tersebut di copykan ke directory tempat dia berada sekarang.

Setelah Joe selesai mengcopy semua file, Fred perlu mengubah mode dari home directory-nya supaya tidak bisa akses lagi. Untuk itu Fred perlu mengetik,

<pre>chmod go-rx ~</pre>
Tanda - di chmod akan membuang akses. Tanpa + di chmod akan menambahkan / memberikan akses. Untuk keterangan lebih lanjut tentang chmod bisa di baca di,

<pre>man chmod</pre>
Informasi Lanjut
Untuk membaca lebih lanjut bisa menggunakan man / user manual

<pre>man chmod
man ls</pre>
