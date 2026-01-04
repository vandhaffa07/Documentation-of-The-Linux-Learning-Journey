# PERINTAH `touch` PADA LINUX

## PENGERTIAN
`touch` command adalah perintah di Linux yang digunakan untuk membuat file baru yang kosong (jika file belum ada) serta memanipulasi stempel waktu/timestamp (waktu akses dan modifikasi) dari file yang sudah ada 

---

## SINTAKS
Sintaks umum perintah `touch` adalah :
```bash
touch [OPTIONS] [FILE]
```
---

## TIMESTAMP
**atime (Access Time)** : 
  Waktu terakhir file diakses (dibaca).
  
**mtime (Modification Time)** :
  Waktu terakhir isi/konten file dimodifikasi.
  
**ctime (Change Time)** : 
  Waktu terakhir metadata file berubah (misalnya permission, ownership, atau timestamp itu sendiri).

**btime (Birth Time)** : Waktu saat file pertama kali dibuat di filesystem.

📚📄 [Sedikit Catatan Mengenai ctime dan mtime](https://github.com/vandhaffa07/Documentation-of-The-Linux-Learning-Journey/blob/main/Linux-Command/ls-command.md#catatan-penting--mtime-dan-ctime-)

---

## OPTION
**1. No Option** : Digunakan untuk membuat file kosong baru jika nama file belum ada . Jika nama file sudah ada, command ini akan memperbarui atime dan mtime file tersebut menjadi waktu saat ini.

Misalnya dalam sebuah folder, saya hanya mempunyai 1 file seperti ini :
```bash
ls
file1.txt
```
Kemudian saya akan membuat file kosong baru menggunakan command `touch` :
```bash
touch file2.txt
```
```bash
ls -l
total 4
-rw-r--r-- 1 username username 23 Dec 31 09:19 file1.txt
-rw-r--r-- 1 username username 0 Dec 31 09:22 file2.txt
```
Dapat dilihat bahwa file2.txt telah terbuat karena pada direktori tersebut tidak ada nama file yang sama dengannya. Terlihat juga bahwa file2.txt memiliki size 0, yang berarti file tersebut benar benar kosong atau tidak ada isinya. 

Selanjutnya saya akan melakukan percobaan menggunakan command touch kepada file yang sudah ada pada direktori tersebut yakni file1.txt. Namun, sebelumnya saya akan melakukan statistik pada file tersebut agar bisa membandingkan perbedaan timestamp-nya :
```bash
stat file1.txt
  File: file1.txt
  Size: 23              Blocks: 8          IO Block: 4096   regular file
Device: 8,48    Inode: 25783       Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2025-12-31 09:19:27.983010807 +0700
Modify: 2025-12-31 09:19:37.007630891 +0700
Change: 2025-12-31 09:19:37.007630891 +0700
 Birth: 2025-12-31 09:19:27.983010807 +0700
```
```bash
touch file1.txt
```
Kemudian saya akan melakukan statistik timestamp lagi pada file1.txt yang telah menerima command touch :
```bash
stat file1.txt
  File: file1.txt
  Size: 23              Blocks: 8          IO Block: 4096   regular file
Device: 8,48    Inode: 25783       Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2025-12-31 09:30:26.613811431 +0700
Modify: 2025-12-31 09:30:26.613811431 +0700
Change: 2025-12-31 09:30:26.613811431 +0700
 Birth: 2025-12-31 09:19:27.983010807 +0700
```
Dapat dilihat bahwa ctime juga mengalami perubahan yang sama seperti yang terjadi pada atime dan mtime. Hal ini dikarenakan mtime adalah bagian dari metadata file. Sehingga setiap kali mtime diperbarui, berarti ada perubahan pada inode. Setiap kali ada perubahan pada inode, sistem secara otomatis akan memperbarui ctime dan mencatat bahwa metadata file tersebut telah berubah. Ini adalah fitur keamanan agar sistem audit tahu kapan terakhir kali properti file dimodifikasi, sehingga pengguna tidak bisa menyembunyikan jejak perubahan metadata dengan mudah.


**2. -a** : Digunakan hanya untuk mengubah atime dari sebuah file.

Disini saya masih menggunakan file1.txt untuk percobaan seperti penjelasan sebelumnya. Saya akan menampilkan statistik timestamp dari file tersebut dan kemudian saya akan menggunakan command touch -a kepadanya :
```bash
stat file1.txt
  File: file1.txt
  Size: 23              Blocks: 8          IO Block: 4096   regular file
Device: 8,48    Inode: 25783       Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2025-12-31 09:30:26.613811431 +0700
Modify: 2025-12-31 09:30:26.613811431 +0700
Change: 2025-12-31 09:30:26.613811431 +0700
 Birth: 2025-12-31 09:19:27.983010807 +0700
```
```bash
touch -a file1.txt
```
Kemudian saya akan menampilkan ulang statistik timestamp file1.txt setelah digunakan bersama command touch -a :
```bash
 stat file1.txt
  File: file1.txt
  Size: 23              Blocks: 8          IO Block: 4096   regular file
Device: 8,48    Inode: 25783       Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2025-12-31 09:33:38.066766807 +0700
Modify: 2025-12-31 09:30:26.613811431 +0700
Change: 2025-12-31 09:33:38.066766807 +0700
 Birth: 2025-12-31 09:19:27.983010807 +0700
```
Dapat dilihat bahwa timestamp yang mengalami perubahan hanyalah atime dan ctime. Mengapa ctime ikut berubah? Hal ini bisa terjadi dikarenakan touch -a bukanlah alat untuk membaca suatu file, melainkan alat pengirim perintah ke sistem untuk memodifikasi properti inode secara eksplisit. Saat menjalankan command touch -a kita secara aktif meminta sistem untuk menulis ulang atau mengubah salah satu kolom di metadata (yaitu kolom atime). Akibatnya sistem menganggap ini sebagai perubahan pada metadata inode. Sehingga ctime wajib diperbarui.

Berbeda dengan perintah cat, yang berfungsi sebagai alat untuk membaca isi file. Saat cat dijalankan, sistem hanya melakukan operasi baca terhadap data file. Aktivitas membaca ini dicatat sebagai akses data, sehingga yang berubah hanya atime. Tidak ada penulisan ulang metadata inode, sehingga ctime tidak ikut berubah. 

Bukti : 
```bash
cat file1.txt
INI ADALAH FILE PERTAMA
```
```bash
stat file1.txt
  File: file1.txt
  Size: 23              Blocks: 8          IO Block: 4096   regular file
Device: 8,48    Inode: 25783       Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2025-12-31 09:41:33.521712204 +0700
Modify: 2025-12-31 09:30:26.613811431 +0700
Change: 2025-12-31 09:33:38.066766807 +0700
 Birth: 2025-12-31 09:19:27.983010807 +0700
```
Dapat dilihat bahwa ketika menggunakan cat yang berubah hanyalah timestamp atime

**3. -m** : Digunakan hanya untuk mengubah mtime dari sebuah file.

Disini saya masih menggunakan file1.txt untuk percobaan seperti pada penjelasan sebelum-sebelumnya. Saya akan menampilkan statistik timestamp dari file tersebut dan kemudian saya akan menggunakan command touch -m kepadanya :
```bash
stat file1.txt
  File: file1.txt
  Size: 23              Blocks: 8          IO Block: 4096   regular file
Device: 8,48    Inode: 25783       Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2025-12-31 09:41:33.521712204 +0700
Modify: 2025-12-31 09:30:26.613811431 +0700
Change: 2025-12-31 09:33:38.066766807 +0700
 Birth: 2025-12-31 09:19:27.983010807 +0700
```
```bash
touch -m file1.txt
```
Kemudian saya akan menampilkan ulang statistik timestamp file1.txt setelah digunakan bersama command touch -m :
```bash
stat file1.txt
  File: file1.txt
  Size: 23              Blocks: 8          IO Block: 4096   regular file
Device: 8,48    Inode: 25783       Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2025-12-31 09:41:33.521712204 +0700
Modify: 2025-12-31 09:50:29.613811431 +0700
Change: 2025-12-31 09:50:29.066766807 +0700
 Birth: 2025-12-31 09:19:27.983010807 +0700
```
Dapat dilihat bahwa ctime mengalami perubahan yang sama seperti yang terjadi pada mtime. Sama seperti pembahasan sebelumnya, hal ini bisa terjadi dikarenakan touch -m mengirim perintah ke sistem untuk menulis ulang atau mengubah salah satu kolom di metadata (yaitu kolom mtime).

**4. -t** : Digunakan untuk mengubah timestamp file ke waktu yang spesifik sesuai keinginan kita, dengan format [[CC]YY]MMDDhhmm[.ss]

* CC → 2 digit pertama tahun

* YY → 2 digit terakhir tahun

* MM → Bulan (01–12)

* DD → Tanggal (01–31)

* hh → Jam (00–23)

* mm → Menit (00–59)

* ss → Detik (00–59)

Contoh (Disini saya masih menggunakan file1.txt untuk melakukan percobaan) :

Sebelumnya saya akan menampilkan statistik timestamp dari file tersebut, barulah kemudian saya menggunakan option -t untuk merubah atime dan mtime file1.txt menjadi tanggal kemerdekaan negara Indonesia (17 Agustus 1945 Pukul 10.00 ) :
```bash
stat file1.txt
  File: file1.txt
  Size: 23              Blocks: 8          IO Block: 4096   regular file
Device: 8,48    Inode: 25783       Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2025-12-31 09:41:33.521712204 +0700
Modify: 2025-12-31 09:50:29.613811431 +0700
Change: 2025-12-31 09:50:29.066766807 +0700
 Birth: 2025-12-31 09:19:27.983010807 +0700
```
```bash
touch -t 194508171000.00 file1.txt
```
```bash
 stat file1.txt
  File: file1.txt
  Size: 23              Blocks: 8          IO Block: 4096   regular file
Device: 8,48    Inode: 25783       Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 1945-08-17 10:00:00.000000000 +0900
Modify: 1945-08-17 10:00:00.000000000 +0900
Change: 2025-12-31 10:46:11.699708507 +0700
 Birth: 2025-12-31 09:19:27.983010807 +0700
```

**5. -d atau date** : Digunakan untuk Mengatur atime dan mtime menggunakan format waktu yang mudah dipahami manusia. Seperti :
```bash
touch -d "2024-12-31 14:00" file.txt
```
```bash
touch -d "yesterday" file.txt
```
```bash
touch -d "next friday" file.txt
```
```bash
touch -d "2 hours ago" file.txt
```
Misalnya, saya mempunyai file bernama file_percobaan.txt dengan statistik seperti ini :
```bash
 stat file_percobaan.txt
  File: file_percobaan.txt
  Size: 0               Blocks: 0          IO Block: 4096   regular empty file
Device: 8,48    Inode: 27854       Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2025-12-31 11:00:53.362950316 +0700
Modify: 2025-12-31 11:00:53.362950316 +0700
Change: 2025-12-31 11:00:53.362950316 +0700
 Birth: 2025-12-31 11:00:53.358950319 +0700
```
Saat ini adalah adalah tanggal 31 desember tahun 2025 pukul 11.01.47 dan saya ingin mengganti atime dan mtime file tersebut menjadi waktu besok yakni tanggal 1 januari tahun 2026 pukul 11.01.47, daripada repot-repot menulis format seperti option t, kita dapat dengan mudah mengetik tomorrow sebagai string dari option d seperti ini : 
```bash
 touch -d "tomorrow" file_percobaan.txt
```
```bash
 stat file_percobaan.txt
  File: file_percobaan.txt
  Size: 0               Blocks: 0          IO Block: 4096   regular empty file
Device: 8,48    Inode: 27854       Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2026-01-01 11:01:47.135432062 +0700
Modify: 2026-01-01 11:01:47.135432062 +0700
Change: 2025-12-31 11:01:47.134900872 +0700
 Birth: 2025-12-31 11:00:53.358950319 +0700
```

**6. -r atau Reference** : Digunakan untuk menyalin atime dan mtime dari satu file ke file lainnya tanpa perlu mengetik angka atau tanggal secara manual.

Misalnya saya mempunyai 2 file dengan statistik seperti ini :
```bash
 stat A.txt
  File: A.txt
  Size: 0               Blocks: 0          IO Block: 4096   regular empty file
Device: 8,48    Inode: 25783       Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2025-12-31 11:11:56.142369850 +0700
Modify: 2025-12-31 11:11:56.142369850 +0700
Change: 2025-12-31 11:11:56.142369850 +0700
 Birth: 2025-12-31 11:11:56.142369850 +0700
```
```bash
stat B.txt
  File: B.txt
  Size: 0               Blocks: 0          IO Block: 4096   regular empty file
Device: 8,48    Inode: 27254       Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2025-12-31 11:14:29.190259386 +0700
Modify: 2025-12-31 11:14:29.190259386 +0700
Change: 2025-12-31 11:14:29.190259386 +0700
 Birth: 2025-12-31 11:14:29.190259386 +0700
```
Selanjutnya saya ingin mengubah timestamp atime dan mtime dari file B.txt agar sama dengan file A.txt :
```bash
touch -r A.txt B.txt
```
```bash
 stat B.txt
  File: B.txt
  Size: 0               Blocks: 0          IO Block: 4096   regular empty file
Device: 8,48    Inode: 27254       Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2025-12-31 11:11:56.142369850 +0700
Modify: 2025-12-31 11:11:56.142369850 +0700
Change: 2025-12-31 11:16:35.742139482 +0700
 Birth: 2025-12-31 11:14:29.190259386 +0700
```






