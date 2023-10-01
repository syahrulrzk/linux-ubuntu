# Daftar Perintah Dasar Linux dan Fungsinya
Untuk dapat mengoperasikan Linux, Anda perlu mengakses shell sebuah interface berbasis teks untuk menulis sebuah perintah (command) dan menginstruksikan komputer untuk melakukan tasks tertentu.

## 1. $ pwd
pwd atau print working directory akan memberikan informasi mengenai direktori aktif yang sedang Anda akses. Hasil dari command adalah seperti /home/username.    
 
## 2. $ cd
Jika direktori yang Anda akses saat itu ternyata belum tepat, Anda bisa memanfaatkan perintah cd (change directory) untuk mengubah atau membuka direktori tertentu.

Contohnya, Anda sedang berada di direktori Home dan ingin mengakses folder Downloads. Nah, Anda hanya perlu menulis perintah cd Downloads. 

## $ mkdir
Perintah mkdir (make directory) berfungsi untuk membuat folder atau direktori baru. Untuk menambah direktori Movies, misalnya, Anda bisa menulis mkdir Movies.

Selain itu, Anda juga bisa membuat direktori dalam direktori lain yang sudah ada. Caranya, ketik nama direktori lama diikuti dengan direktori baru, seperti mkdir Movies/Actions.

## $ rmdir
Jika Anda ingin menghapus direktori, Anda bisa menggunakan perintah rmdir. Namun perlu diketahui, perintah ini hanya bisa menghapus direktori yang masih kosong.

Sebagai alternatif, Anda juga bisa menulis rm -r untuk mendapatkan fungsi yang sama seperti rmdir.  

Namun harus diingat, Anda perlu berhati-hati dalam menggunakan rm -r. Pastikan Anda telah menulis nama folder/file setelah perintah rm -r (contohnya seperti: rm -r Movies/Actions).

Jika Anda lupa mencantumkan nama folder/file tersebut, maka yang terjadi adalah Anda akan menghapus seluruh direktori pada server.

## $ rm
Kalau Anda ingin menghapus direktori beserta seluruh files yang ada di dalamnya, Anda bisa memanfaatkan perintah rm.

Sedangkan bagi Anda yang ingin menghapus satu file secara khusus, Anda bisa menambahkan nama file tersebut setelah perintah rm (contoh: rm filename).

## $ cat
Sebagaimana namanya, perintah cat (concatenate) berfungsi untuk menggabungkan files.

Contohnya, tulis cat filename1 filename2>filename3 untuk menggabung filename1 dan filename2, dan menjadikannya sebagai filename3.

Di samping itu, cat juga menawarkan beberapa fungsi lain, seperti mengetahui konten suatu file (contoh: cat file.txt) dan membuat file baru (contoh: cat > newfile).  

## $ echo
Perintah echo menawarkan banyak fungsi, salah satunya adalah untuk memasukkan data (yang biasanya berbentuk teks) ke dalam file.

Sebagai contoh, Anda ingin menambah teks hello there, welcome! ke file bernama sample.txt. Nah untuk melakukannya, Anda bisa menulis echo hello there welcome! >> sample.txt. 

## $ ls
Bagi Anda yang ingin mengetahui konten apa saja yang terdapat dalam working directory atau direktori aktif, Anda bisa menggunakan perintah ls.

Selain itu, Anda juga bisa melihat hidden files dengan perintah ls -a, atau melihat konten di direktori lain dengan menulis nama direktorinya seperti ls /home/username/Documents.

## $ locate
Anda merasa bingung di mana suatu file tersimpan? Tenang, Anda bisa memanfaatkan perintah locate.

Jika Anda lupa dengan nama file tersebut, Anda bisa menambahkan -i, sehingga perintahnya menjadi locate -i filename.

Sedangkan jika nama file yang dicari lebih dari satu kata, Anda perlu menambahkan asterisk (*) ke dalam perintahnya (contoh: locate -i data*sekolah).   

## $ find
Anda juga bisa menggunakan find command untuk menemukan suatu file atau direktori. Bedanya, pencarian dengan perintah ini hanya akan difokuskan ke direktori tertentu.

Misalnya Anda menulis find /nama direktori/ -name sample.txt, maka pencarian tersebut hanya dilakukan pada direktori home yang dituliskan.

## $ touch
Perintah touch bisa digunakan untuk membuat file baru dengan berbagai jenis format; seperti txt, zip, maupun html.

Untuk membuat file dengan format teks di direktori Documents, misalnya, Anda dapat menulis perintah touch /home/username/Documents/sample.txt,.

## $ sudo
Singkatan dari SuperUser Do; sudo adalah sebuah perintah dasar Linux yang memungkinkan Anda menjalankan berbagai macam tasks yang memerlukan root atau administrative permissions.

Contoh perintahnya adalah sudo visudo, yang berfungsi untuk mengedit file konfigurasi atau sudoers.

## $  cp
Perintah cp digunakan untuk menyalin file dari direktori aktif ke direktori lain. Contohnya, Anda ingin menyalin sample.txt ke direktori Documents.

Nah, Anda bisa menulis cp sample.txt /home/username/Documents/. 

Baca Juga: Kenali Apa Itu Dedicated Server, Kelebihan, dan Kekurangannya

## $  mv
Di sistem operasi Linux, Anda juga dapat memindahkan file ke direktori lain dengan bantuan perintah mv.

Sebagai contoh, Anda menulis mv sample.txt /home/username/Documents/ untuk memindahkan sample.txt ke direktori Documents.

Tidak hanya itu, mv juga bisa dimanfaatkan untuk mengubah nama file. Sebagai contoh, Anda ingin mengganti nama file txt Anda, maka perintahnya akan menjadi mv oldsample.txt newsample.txt.  

## $  ping
Sering mendengar ping command? Atau malah sering menggunakannya? Perintah dasar Linux yang satu ini menawarkan fungsi untuk mengetahui atau mengecek koneksi Anda ke server.

Jika Anda menulis ping google.com, contohnya, Anda nantinya akan memperoleh detail informasi mengenai koneksi Anda ke Google.  

## $  zip dan unzip
Perintah zip dan unzip tentunya sudah tak asing lagi di telinga Anda bukan? Zip command digunakan untuk mengompres file menjadi zip archives.

Contoh perintahnya seperti zip -r archivename.zip filename1 folder2. Sebaliknya, unzip berfungsi untuk mengekstrak zip archives (contoh: unzip archivename.zip). 

## $ hostname
Ada beberapa informasi yang bisa Anda dapatkan dari perintah hostname ini. Salah satunya adalah mengenai nama DNS (Domain Name System).

Di samping itu, Anda juga bisa mengetahui IP address Anda dengan menambahkan -i atau -I, sehingga perintahnya menjadi hostname -i atau hostname -I.   

## $ chown
Tahukah Anda kalau setiap file dalam Linux dibekali dengan keamanan dan proteksi berupa access permissions? Artinya, setiap file dan direktori hanya bisa diakses oleh pengguna tertentu saja. 

Ada tiga klasifikasi user permissions dalam Linux, yaitu: 

User—pemilik file atau pengguna yang membuat file tersebut
Group—terdiri dari beberapa pengguna. Nantinya, hanya anggota grup yang bisa mengakses file. 
Others—pengguna yang memiliki akses ke suatu file, tetapi bukan user/pemilik maupun anggota dari group. 
Lalu, bagaimana jika Anda ingin mengubah atau mentransfer kepemilikan suatu file? Caranya mudah, yakni dengan menggunakan perintah chown.

Misalnya jika Anda menulis chown user2 sample.txt, Anda berarti telah menjadikan user2 sebagai pemilik file sample.txt.

Selain itu, chown juga bisa digunakan untuk mengganti kepemilikan suatu grup (contoh: chown :group1 sample.txt).

## $  chmod
Perintah chmod (change mode) berfungsi untuk mengganti izin akses terhadap suatu file atau direktori. Urutan perintahnya adalah chmod [opsi izin akses] [nama file].

Contohnya seperti chmod 754 filename. 

Adapun 754 dalam kode tersebut bisa dirincikan sebagai berikut:

7 adalah kombinasi dari 4+2+1; berarti user dapat membaca (angka 4), memodifikasi dan menghapus (angka 2), serta menjalankan/mengeksekusi file tersebut (angka 1). 
5 merupakan gabungan dari 4+0+1; menandakan bahwa anggota group bisa membaca (4) dan menjalankan/mengeksekusi file (1), tetapi tidak bisa memodifikasi dan menghapusnya (0).
4 adalah kombinasi dari 4+0+0, yang berarti kalau others hanya bisa membaca (4), namun tidak dapat memodifikasi atau menghapus (0) dan menjalankan file (0).      
Baca Juga: Mengenal Server: Pengertian, Jenis, Fungsi, dan Manfaatnya 

## $  uname
Perintah uname atau Unix Name digunakan untuk mengetahui informasi dasar  mengenai sistem Linux yang Anda gunakan— versi kernel, tanggal rilis, tipe prosesor, nama sistem operasi, dan sebagainya. 

Pola perintahnya adalah uname [opsi]. Nantinya, anda akan mengganti [opsi] sesuai dengan informasi yang ingin Anda cari. Misalnya untuk mengetahui nama sistem operasi yang digunakan, Anda dapat menulis uname -o.     

## $  df 
Gunakan perintah df untuk memperoleh informasi mengenai penggunaan disk space sistem Anda, serta mengetahui berapa banyak kapasitas yang tersisa.

Nantinya, detail tersebut akan disajikan dalam persentase dan kilobytes. Namun jika Anda ingin melihatnya dalam megabytes, maka tuliskan perintah df -m.

## $  diff 
Perintah diff (singkatan dari difference) digunakan untuk membandingkan konten antar file, baris per baris.

Menariknya, Anda akan mendapatkan informasi mengenai baris mana saja yang perlu diubah agar files tersebut menjadi sama.

Untuk menjalankan perintah ini, Anda hanya perlu menulis diff diikuti dengan nama file yang akan dibandingkan (contoh: diff file1.txt file2.txt). 

Sebagai output, Anda akan memperoleh simbol yang berisi instruksi bagaimana mengubah file pertama agar bisa menyerupai file kedua.

Adapun simbol-simbolnya terdiri dari a (add/tambahkan), c (change/ubah), dan d (delete/hapus).      

## $  du 
Anda bisa melacak atau mengecek seberapa banyak kapasitas disk yang digunakan oleh suatu file dan direktori dengan bantuan perintah du (disk usage).

Sebagai contoh, Anda ingin mengetahui penggunaan disk dari direktori /home/username. Nah, Anda cukup menulis du /home/username.

Sedangkan jika ingin melihat hasilnya dalam bytes, kilobytes, megabytes, gigabytes dan seterusnya, maka tambahkan -h sehingga menjadi du -h /home/username.     

## $  tar
Perintah tar sebenarnya hampir mirip dengan zip. Bedanya, tar hanya mengumpulkan files dalam satu arsip dengan ekstensi .tar atau tarball, tanpa mengompresnya seperti pada zip.

Pola perintahnya adalah tar [opsi] [nama arsip] [file atau directory yang akan diarsip]. 

Sebagai contoh, Anda menulis perintah tar -czvf filename.tar.gz /home/username/. Di sini, opsi yang Anda gunakan adalah czvf, yang berarti:

c = create/membuat arsip .tar baru
z = membuat arsip dengan kompresi gzip
v = verbose/perlihatkan progress pembuatan file tar
f = buat arsip tar sesuai dengan nama yang diberikan, yang dalam contoh ini adalah filename.tar.gz
25. grep
grep merupakan salah satu perintah dasar Linux yang paling sering digunakan. Fungsinya adalah untuk mencari teks atau pattern di dalam suatu file. Pola perintahnya yakni grep [opsi] text/pattern [files]. 

Sebagai contoh, Anda menulis perintah grep -i UNix sample.txt. Artinya, Anda sedang mencari kata UNix dalam file sample.txt.

Adapun opsi -i berarti pencarian tersebut akan menampilkan semua baris yang mengandung kata UNix dengan mode case insensitive— tanpa memperdulikan apakah kata tersebut ditulis dalam huruf besar atau kecil. Sehingga kata UNIX, Unix, atau unix pun nantinya akan ditampilkan dalam hasil pencarian.

## $  kill 
Jika suatu program yang Anda jalankan di Linux tidak responsif atau kedapatan menggunakan kapasitas sumber daya sistem yang berlebih, mengapa tidak menghentikan prosesnya saja. Untuk melakukannya, Anda bisa memanfaatkan perintah kill.

Yang menarik, ada lebih dari 60 signals yang bisa digunakan dalam command ini. Untuk mengetahuinya, Anda bisa menulis perintah kill -l. 

Di antara signals tersebut, dua opsi yang paling banyak digunakan adalah: (15) SIGTERM (menghentikan program dan memberikan waktu untuk menyimpan progress-nya) dan (9) SIGKILL (mematikan program secara paksa tanpa perlu merekam progress yang belum disimpan). 

Contoh perintahnya adalah kill -9 1223. Di sini, 1223 adalah PID (Process ID) yang hendak di-kill. Anda dapat menemukan PID dari proses yang hendak Anda kill pada command top atau ps (contoh: ps -A | grep ‘firefox’)

## $  man 
Anda masih bingung dengan beberapa perintah/commands dan ingin mempelajari fungsinya lebih lanjut?

Tak usah khawatir! Linux juga menyediakan man command—sebuah perintah yang akan menampilkan halaman manual berisi instruksi atau deskripsi mengenai command tertentu.

Caranya, cukup tulis perintah man diikuti dengan nama command yang ingin Anda ketahui (contoh: man rmdir).
<pre>man rmdir</pre
