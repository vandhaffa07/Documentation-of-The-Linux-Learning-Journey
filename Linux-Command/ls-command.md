# PERINTAH `ls` PADA LINUX

## PENGERTIAN
Perintah `ls` (*list*) di Linux digunakan untuk menampilkan daftar file dan direktori di dalam suatu direktori, baik direktori kerja saat ini (*current working directory*) maupun direktori tertentu yang kita tentukan.

---

## SINTAKS
Sintaks umum perintah `ls` adalah :
```bash
ls [OPTION] [DIRECTORY]
```

## OPTION
**1. -a atau --all** : Digunakan untuk menampilkan semua file pada sebuah direktori tanpa terkecuali, termasuk file tersembunyi yang diawali tanda titik (.) serta file-file seperti '.'(direktori saat ini) dan '..'(direktori induk)

**2. -l** : Digunakan untuk Menampilkan informasi dalam format panjang (long listing) yang memuat permission, jumlah hard link, owner & group, ukuran file, waktu perubahan terakhir(secara default menggunakan patokan mtime( waktu perubahan isi/konten terakhir. )), dan nama file. Sama dengan penggunaan tanpa option-nya dimana option ini mengurutkannya dalam alphabetical
Contoh : ( Disini saya membuat folder yang bernama 'percobaan' )
```bash
ls -l
total 8
-rw-r--r-- 1 username username   0 Dec 29 20:29 file_percobaan.txt
drwxr-xr-x 2 username username 4096 Dec 29 20:28 folder1
drwxr-xr-x 2 username username 4096 Dec 29 20:28 folder2
```
**3. -t** : Digunakan untuk mengurutkan output berdasarkan waktu. Secara default, waktu yang digunakan adalah mtime (modification time(waktu perubahan isi/konten terakhir.)). Akan tetapi, jika digunakan bersama option lain yang mengubah patokan waktu, seperti -c (ctime), maka -t akan mengikuti patokan waktu tersebut.
Contoh :
Misalnya saya mempunyai susunan folder seperti ini :
```bash
testing/
├── file_a
└── file_b

0 directory, 2 files
```
Kemudian saya ubah isi file_b menggunankan command cat :
```bash
cat > file_b
INI ADALAH ISI YANG BARU
^C
```
Selanjutnya saya akan mengecek cara kedua file tersebut diurutkan menggunakan ls tanpa option dan ls -t :
```bash
ls
file_a  file_b
```
```bash
ls -t
file_b  file_a
```
Dapat dilihat bahwa dengan menggunakan option t maka, file diurutkan berdasarkan perubahan mtime nya

**4. -c** : Digunakan untuk menggunakan ctime (change time), yaitu waktu perubahan metadata terakhir (permission, owner, jumlah link, dan atribut lainnya) sebagai patokan waktunya. Jika digunakan tanpa option lain, ls -c akan mengurutkan output berdasarkan ctime (newest first). Jika digunakan bersama -l, kolom waktu yang biasanya menampilkan mtime akan berubah menjadi ctime, dan urutan kembali ke nama file (alphabetical). Jika digunakan bersama -l dan -t (ls -lct), maka ctime akan ditampilkan menggantikan mtime, dan urutan file akan disusun berdasarkan ctime dari yang paling baru ke yang paling lama.
Contoh :
Misalnya saya mempunyai susunan folder seperti ini :
```bash
alphabet/
├── a
├── b
└── c

3 directories, 0 files
```
Dimana saya membuat direktori a, b, dan c secara bersamaan. Kemudian saya ubah nama c menjadi ce :
```bash
mv c ce
```
Lalu saya mengecek menggunakan ls no option dan ls -c :
```bash
ls
a  b  ce
```
```bash
ls -c
ce  b  a
```
Dapat dilihat bahwa penggunaan option c secara tunggal akan mengurutkan output berdasarkan perubahan ctime terakhir yang dimana dalam hal ini ctime berubah karena nama direktorinya juga berubah.

Akan tetapi ketika saya menggunakannya bersama option -l : 
```bash
ls -lc
total 12
drwxr-xr-x 2 username username 4096 Dec 29 20:06 a
drwxr-xr-x 2 username username 4096 Dec 29 20:06 b
drwxr-xr-x 2 username username 4096 Dec 29 20:08 ce
```
Dapat kita lihat disini bahwa pengurutan tetap kembali seperti semula yakni alphabetical, akan tetapi kolom mtime berubah menjadi ctime dan ini sesuai dengan command stat pada direktori ce :
```bash
stat ce
  File: ce
  Size: 4096            Blocks: 8          IO Block: 4096   directory
Device: 8,48    Inode: 11835       Links: 2
Access: (0755/drwxr-xr-x)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2025-12-29 20:06:45.155232694 +0700
Modify: 2025-12-29 20:06:39.583237674 +0700
Change: 2025-12-29 20:08:14.943151567 +0700
 Birth: 2025-12-29 20:06:39.583237674 +0700
```
Selanjutnya, ketika saya menggunakan option -l dan -t secara bersamaan, maka output akan menjadi :
```bash
ls -ltc
total 12
drwxr-xr-x 2 username username 4096 Dec 29 20:08 ce
drwxr-xr-x 2 username username 4096 Dec 29 20:06 b
drwxr-xr-x 2 username username 4096 Dec 29 20:06 a
```
Dari sini dapat dilihat bahwa urutan file disusun berdasarkan ctime dari yang paling baru ke yang paling lama.

**5. -d / --directory** : Menampilkan output hanya direktori saja, file tidak ditampilkan

Contoh:
Misalnya saya mempunyai susunan folder seperti ini :
```bash
eksperimen/
├── file_a.txt
└── folder_a

1 directory, 1 files
```
Kemudian saya menggunakan option d, maka outputnya akan menjadi :
```bash
ls -d
folder_a
```
**6. -h / --human-readable** : Digunakan bersama -l(wajib agar terlihat) untuk menampilkan ukuran file dalam format yang mudah dibaca manusia seperti dalam KB, MB, dan GB
Contoh :
```bash
ls -lh
```

**7. -i** : Digunakan untuk menampilkan nomor inode(Nomor khusus yang dimiliki oleh setiap file/direktori untuk mengidentifikasi, mengelola, dan menemukan lokasi data file/direktori pada disk) di depan nama file atau direktori.
Contoh :
```bash
ls -i
```
