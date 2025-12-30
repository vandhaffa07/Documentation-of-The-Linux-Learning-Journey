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
**1. -a atau --all** : Digunakan untuk menampilkan semua file pada sebuah direktori tanpa terkecuali, termasuk file tersembunyi yang diawali tanda titik (.) serta file link(entry/pintu masuk) default seperti (.) untuk dirinya sendiri dan (..) yang menunjuk ke direktori induknya.

Contoh (Disini saya menggunakan folder pribadi untuk percobaan) :

Tanpa option a
```bash
ls
folder_biasa
```
Dengan option a :
```bash
ls -a
.  ..  folder_biasa  .folder_tersembunyi
```

**2. -l** : Digunakan untuk Menampilkan informasi dalam format panjang (long listing) yang memuat permission, jumlah hard link, owner & group, ukuran file, waktu perubahan terakhir(secara default menggunakan patokan mtime( waktu perubahan isi/konten terakhir. )), dan nama file. 

Contoh (Disini saya menggunakan folder pribadi untuk percobaan) :
```bash
ls -l
total 8
-rw-r--r-- 1 username username   0 Dec 29 20:29 file_percobaan.txt
drwxr-xr-x 2 username username 4096 Dec 29 20:28 folder1
drwxr-xr-x 2 username username 4096 Dec 29 20:28 folder2
```
**3. -t** : Digunakan untuk mengurutkan output berdasarkan waktu. Secara default, waktu yang digunakan adalah mtime (modification time(waktu perubahan isi/konten terakhir.)). Akan tetapi, jika digunakan bersama option lain yang mengubah patokan waktu, seperti -c (ctime), maka -t akan mengikuti patokan waktu tersebut.

Misalnya saya mempunyai susunan folder seperti ini :
```bash
testing/
├── file_a
└── file_b
```
Kemudian saya ubah isi file_b menggunankan command cat :
```bash
cat > file_b
INI ADALAH ISI YANG BARU
^C
```
Selanjutnya saya akan mengecek bagaimana kedua file tersebut diurutkan menggunakan ls tanpa option dan ls -t :
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

Misalnya saya mempunyai susunan folder seperti ini :
```bash
alphabet/
├── a
├── b
└── c
```
**Catatan : saya membuat direktori a, b, dan c diwaktu yang sama secara bersamaan**

Kemudian saya ubah nama direktori c menjadi ce :
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
Dapat kita lihat disini bahwa pengurutan tetap kembali seperti semula yakni alphabetical, akan tetapi kolom mtime berubah menjadi ctime. Hal ini juga sesuai dengan statistik dari direktori ce yang mennunjukkan bahwa ctime terakhirnya ada pada pukul 20.08 :
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

**5. -d atau --directory** : Menampilkan output hanya direktori saja, file tidak ditampilkan

Misalnya saya mempunyai susunan folder seperti ini :
```bash
eksperimen/
├── file_a.txt
└── folder_a
```
Kemudian saya menggunakan option d, maka outputnya akan menjadi :
```bash
ls -d
folder_a
```
**6. -h atau --human-readable** : Digunakan bersama -l(wajib agar terlihat) untuk menampilkan ukuran file dalam format yang mudah dibaca manusia seperti dalam KB, MB, dan GB

Contoh (Disini saya menggunakan folder pribadi untuk percobaan) :
```bash
ls -lh
total 1.4M
-rw-r--r-- 1 username username 1.2M Dec 30 08:47 foto1.jpeg
-rw-r--r-- 1 username username 209K Dec 30 08:48 foto2.jpg
```


**7. -i** : Digunakan untuk menampilkan nomor inode(Nomor khusus yang dimiliki oleh setiap file/direktori untuk mengidentifikasi, mengelola, dan menemukan lokasi data file/direktori pada disk) di depan nama file atau direktori.

Contoh(Disini saya menggunakan folder yang sama dengan penjelasan command sebelumnya) :
```bash
ls -i
45469 foto1.jpeg   2669 foto2.jpg
```
**8. -R atau --recursive** Digunakan untuk menampilkan isi direktori secara rekursif, termasuk semua subdirektori dan file- file yang ada didalamnya

Misalnya saya mempunyai susunan folder seperti ini :
```bash
Folder_Belajar
├── Belajar1
│   └── Sub_Folder1
│       ├── catatan1.txt
│       └── catatan2.txt
│       
└──Belajar2
   └── Sub_Folder1
       ├── Note
       │   ├── note1.txt
       │   └── note2.txt
       └── Dokumentasi
```
Maka ketika saya menggunakan command ls -R, outputnya akan menjadi :
```bash
ls -R
.:
Belajar1  Belajar2

./Belajar1:
Sub_Folder1

./Belajar1/Sub_Folder1:
catatan1.txt  catatan2.txt

./Belajar2:
Sub_Folder1

./Belajar2/Sub_Folder1:
Dokumentasi  Note

./Belajar2/Sub_Folder1/Dokumentasi:

./Belajar2/Sub_Folder1/Note:
note1.txt  note2.txt
```
---

## ARTI WARNA PADA OUTPUT `ls`
**Biru** : Direktori

**Hijau**	: File executable / program

**Putih / Abu-abu** : File biasa

**Cyan** : Symbolic link

**Merah**	: File arsip / kompresi

## CATATAN PENTING ( mtime dan ctime )
**KONSEP DASAR :**

**mtime** → berubah jika isi konten file/direktori berubah

**ctime** → berubah jika metadata berubah

**Jika mtime berubah** → **ctime pasti ikut berubah**

**Jika ctime berubah** → **mtime belum tentu berubah**

**BEBERAPA SKENARO PENTING :**

**1. Mengedit Isi File** : Saat file diedit (misalnya dengan nano), yang berubah bukan hanya isi konten di dalam file tersebut, tetapi juga metadata inode-nya. Perubahan konten file dapat menyebabkan ukuran file berubah dan alokasi blok data diperbarui, yang merupakan bagian dari metadata. Karena ctime mencatat waktu terakhir perubahan metadata inode, maka setiap modifikasi isi file akan otomatis memperbarui mtime (waktu modifikasi isi) dan juga ctime (waktu perubahan metadata).

**2. Menambah Subdirektori** : 
Perlu diketahui pula bahwa secara default, setiap direktori di Linux memiliki 2 link( pintu masuk ), yakni satu link untuk dirinya sendiri (.) dan satu link yang menunjuk ke direktori induknya (..). Saat kita membuat subdirektori baru di dalam suatu direktori, otomatis jumlah link pada direktori induk akan bertambah. Karena link termasuk bagian dari metadata, maka perubahannya juga mempengaruhi perubahan ctime pada direktori induk.

Analogi silsilah:
```bash
~ (home) → kakek
└─Direktori_Induk → ayah
  └─ Subdirektori( yang baru dibuat ) → anak
```
Ketika si “anak” lahir (subdirektori dibuat): Maka jumlah link ayah akan bertambah 1, karena "anak" (subdirektori baru) memiliki entry (..) yang menunjuk ke direktori induk yakni sang ayah.

**3. Membuat File Baru dalam Direktori** : Ketika sebuah file baru dibuat, metadata direktori induk otomatis berubah. Hal ini terjadi karena direktori tidak menyimpan isi file itu sendiri, melainkan informasi tentang file lain, seperti nama file, inode, dan atribut lainnya.

Contoh: ( Misalnya terdapat susunan folder seperti ini )
```bash
ALPHABET/
├── A.txt  → "A adalah huruf pertama"
└── B.txt  → "B adalah huruf kedua"
```
Jika direktori menyimpan konten file secara langsung, tampilannya akan seperti ini:
```bash
A adalah huruf pertamaB adalah huruf kedua
```
Namun faktanya, yang ditampilkan saat melihat isi direktori hanya berupa nama file:
```bash
A.txt
B.txt
```
Hal ini membuktikan bahwa isi direktori bersifat metadata, bukan konten file.

**4. Mengubah Permission File** : Ketika kita mengubah permission sebuah file, misalnya dari 755 menjadi 700, yang terjadi adalah metadata file berubah. Karena ctime mencatat waktu terakhir perubahan metadata inode, maka ctime file akan ikut berubah. Namun, karena isi file tidak disentuh, maka mtime tetap sama.



