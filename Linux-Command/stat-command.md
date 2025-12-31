# PERINTAH `stat` PADA LINUX

## PENGERTIAN
`stat` adalah perintah di Linux yang digunakan untuk menampilkan informasi metadata secara terperinci tentang sebuah file, folder atau filesystem. Berbeda dengan `ls` yang hanya menampilkan ringkasan, `stat` memberikan detail internal seperti inode, permission, kepemilikan, serta berbagai jenis timestamp.

---

## SINTAKS
Sintaks umum perintah `stat` adalah :
```bash
stat [OPTION] [FILE ATAU FOLDER]
```

---

## INFORMASI FILE YANG DITAMPILKAN (JIKA DIJALANKAN TANPA OPSI)
Saat `stat` dijalankan tanpa opsi apapun, maka informasi berikut akan ditampilkan:

* **File** : Menampilkann nama file yang diberikan. Jika file tersebut adalah symbolic link, maka nama target link akan ditampilkan terpisah.

* **File type** : Menampilkan Jenis file, seperti Regular file(Jenis file paling umum dalam sistem operasi yang menyimpan data aktual seperti teks, gambar, program, atau video), Symbolic link(file yang menunjuk ke path file lain), Directory(folder atau file khusus yang menyimpan daftar nama file), dll

* **Device** :  Menunjukkan identitas block device tempat filesystem yang mengelola suatu file berada. Informasi ini digunakan oleh Kernel Linux untuk mengetahui di perangkat penyimpanan mana filesystem tersebut berjalan, sehingga operasi baca dan tulis dapat diarahkan ke driver dan device yang tepat. Nilai yang ditampilkan pada field Device, misalnya 8,48, merupakan pasangan major number dan minor number. Angka pertama (8) adalah major number, yang menandakan driver kernel yang menangani device tersebut. Major number ini mengidentifikasi kelas perangkat, misalnya driver untuk disk berbasis SCSI/SATA atau media penyimpanan serupa. Angka kedua (48) adalah minor number, yang berfungsi sebagai kode identitas untuk device spesifik yang dikelola oleh driver tersebut. Minor number ini memungkinkan kernel membedakan satu unit disk atau partisi dari unit lain yang berada di bawah driver yang sama.

* **Inode** : Menampilkan nomor inode dari sebuah file. Inode adalah struktur data internal pada filesystem Linux yang digunakan untuk menyimpan keseluruhan metadata file, seperti ukuran file, permission, kepemilikan, timestamp, serta referensi ke blok-blok data tempat isi file disimpan. Sedangkan nomor inode adalah identitas unik yang digunakan filesystem untuk merujuk ke inode tersebut.

* **Links** : Jumlah hard link yang menunjuk ke inode yang sama. Nilai ini merepresentasikan berapa banyak entri yang saat ini merujuk ke inode yang sama. Dalam filesystem Linux, 1 inode dapat memiliki lebih dari satu nama melalui mekanisme hard link, dan selama inode   tersebut masih memiliki setidaknya satu hard link, data file akan tetap ada meskipun salah satu nama file dihapus. Selain   itu, 2 file yang berada pada filesystem yang berbeda dapat memiliki nomor inode yang sama tanpa menimbulkan konflik. Untuk   membedakan file secara absolut, kernel Linux menggunakan kombinasi Device ID dan inode number sebagai pasangan identitas yang unik.

* **Size** : Menampilkan ukuran file dalam byte.

* **Blocks** : Menampilkan jumlah blok disk yang benar-benar dialokasikan untuk menyimpan isi sebuah file. Filesystem Linux menyimpan data dalam satuan block, bukan byte satu per satu. Ketika sebuah file dibuat atau diisi, filesystem akan mengalokasikan sejumlah block untuk menyimpan data tersebut, dan jumlah block inilah yang ditampilkan pada field Blocks.

* **IO Block** : Menampilkan ukuran satuan data optimal yang digunakan oleh kernel dan filesystem saat melakukan operasi baca dan tulis terhadap file tersebut. IO Block bukanlah ukuran file, dan juga bukan jumlah blok yang dipakai file. IO Block menjelaskan bagaimana kernel berinteraksi dengan disk, bukan seberapa banyak disk yang dipakai. Ketika sebuah file diakses, baik untuk membaca isi file maupun hanya mengambil metadata seperti saat menjalankan stat, kernel mungkin perlu membaca data dari disk. Operasi baca kernel ini tidak dilakukan byte per byte, melainkan dalam potongan data berukuran tetap yang disebut IO Block. Misal nilai IO Block adalah 4096 byte, Maka setiap 4096 byte data yang dibaca oleh kernel dari disk kernel akan mencatat data tersebut dan menyimpannya ke page cache di memori sebagai 1 page. Jika data yang dibutuhkan lebih besar, kernel akan membaca IO Block berikutnya secara bertahap
  
* **Access (permissions)** : Menampilkan Hak akses file dalam format numerik (misalnya 755) maupun simbolik (misalnya rwxr-xr-x) [Sedikit Catatan Mengenai Konsep Permission](https://github.com/vandhaffa07/Documentation-of-The-Linux-Learning-Journey/blob/main/Linux-Command/mkdir-command.md#konsep-permission-pada-direktori-dan-file-linux)

* **UID / GID** : Menampilkan identitas pemilik file dan kelompok pemilik file yang digunakan kernel Linux untuk mengatur hak akses dan kontrol keamanan. UID (User ID) adalah identitas numerik pengguna yang tercatat sebagai pemilik file. GID (Group ID) adalah identitas numerik kelompok (group) yang tercatat sebagai pemilik grup file tersebut. Penting untuk dipahami bahwa kernel Linux tidak bekerja dengan nama pengguna atau nama grup, melainkan angka. Nama seperti root, user, atau www-data hanyalah representasi di level user space yang dipetakan ke UID dan GID. Di level kernel dan filesystem, yang disimpan dan diproses hanyalah identitas numerik UID dan GID.

* **Timestamp** : Menampilkan atime, mtime, ctime, dan btime [Sedikit Catatan Mengenai Konsep Timestamp](https://github.com/vandhaffa07/Documentation-of-The-Linux-Learning-Journey/blob/main/Linux-Command/touch-command.md#timestamp)

Contoh :
```bash
stat file_percobaan.txt
  File: file_percobaan.txt
  Size: 25              Blocks: 8          IO Block: 4096   regular file
Device: 8,48    Inode: 31438       Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/vandhaffa)   Gid: ( 1000/vandhaffa)
Access: 2025-12-31 21:58:30.143197508 +0700
Modify: 2025-12-31 21:58:39.531208235 +0700
Change: 2025-12-31 21:58:39.531208235 +0700
 Birth: 2025-12-31 21:58:30.143197508 +0700
```

---

## CATATAN KONSEP USER DAN GROUP
User adalah sebuah identitas abstrak yang dipakai kernel untuk menandai proses dan menentukan hak akses. Sedangkan Group adalah identitas kolektif yang memungkinkan beberapa user dikelompokkan agar bisa berbagi akses ke resource tertentu tanpa harus mengatur izin satu per satu.

Setiap user di Linux selalu memiliki satu primary group dan bisa memiliki nol atau lebih supplementary group. Primary group adalah group utama yang melekat langsung pada user dan digunakan secara default oleh sistem. Ketika seorang user membuat file atau direktori, file tersebut secara otomatis akan dimiliki oleh primary group user tersebut. Pada sistem Linux modern, primary group biasanya dibuat khusus untuk setiap user dan memiliki nama yang sama dengan username. Model ini dikenal sebagai User Private Group, di mana primary group tersebut secara efektif hanya berisi satu user. Tujuannya adalah keamanan, dimana file yang dibuat user secara default tidak langsung dapat diakses oleh user lain.

Selain primary group, user juga bisa menjadi anggota supplementary group. Supplementary group tidak digunakan sebagai group default saat membuat file, tetapi berperan penting saat pengecekan izin akses.Secara default, user di Linux biasanya dimasukkan ke beberapa supplementary groups agar bisa melakukan tugas teknis tanpa harus menjadi Roo. Seperti Group sudo agar kita bisa menjalankan perintah administrator, Group video agar kita punya izin mengakses webcam atau akselerasi kartu grafis, dll. Kita bisa mengecek group mana saja yang terafiliasi dengan kita dengan cara : 
```bash
id
uid=1000(username) gid=1000(username) groups=1000(budi),27(sudo),44(video),100(users),............
```

Ketika kernel mengevaluasi apakah sebuah proses boleh mengakses suatu file, kernel akan mengecek secara berurutan: apakah UID proses sama dengan owner file, jika tidak maka apakah GID proses (baik primary maupun supplementary) cocok dengan group file, dan jika tidak juga, barulah permission untuk others yang digunakan. Artinya, keanggotaan dalam supplementary group memberikan potensi akses tambahan, tetapi tetap dibatasi oleh permission bit pada file atau direktori tersebut. Penting untuk dipahami bahwa menjadi anggota suatu group tidak otomatis berarti memiliki akses penuh ke semua file yang dimiliki group tersebut. Group membership hanya menentukan bahwa user berhak diuji menggunakan permission group. Jika permission group pada file tidak mengizinkan akses, maka akses tetap akan ditolak, meskipun user adalah anggota group tersebut. Dengan kata lain, group adalah identitas, sedangkan keputusan akhir tetap berada pada permission.

Konsep user dan group ini sepenuhnya berlaku di dalam satu sistem Linux yang sama, atau lebih tepatnya di dalam satu kernel yang sama. Group dan permission tidak secara otomatis memungkinkan akses lintas komputer. Jika dua orang masing-masing menggunakan laptop sendiri, maka user dan group pada satu laptop tidak memiliki arti apa pun di laptop lain. Agar beberapa orang dapat bekerja bersama menggunakan mekanisme user dan group, mereka harus masuk ke mesin Linux yang sama, baik secara langsung maupun melalui layanan jaringan seperti SSH. Dalam konteks ini, satu mesin tersebut sering disebut sebagai server, meskipun secara fisik bisa saja hanya sebuah laptop biasa.

## OPTION
**1. -c atau --format** : Opsi ini memungkinkan pengguna untuk memfilter hanya informasi tertentu yang ingin ditampilkan menggunakan format specifiers.
### DAFTAR FORMAT SPECIFIER

%n : Nama berkas.

%i : Nomor Inode.

%N : Nama + target symlink

%F : Tipe File (Regular, Symlink, dll)

%s : Ukuran total dalam byte.

%b : Jumlah blok yang dialokasikan.

%B : Ukuran per blok dalam byte.

%o : Ukuran IO Block

%a : Izin akses dalam format oktal (contoh: 644).

%A : Izin akses dalam format simbolik (contoh: -rw-r--r--).

%u : UID owner

%U : Nama pengguna pemilik (Username).

%g : GID 

%G : Nama grup pemilik (Group name).

%x- : Waktu akses terakhir.

%y : Waktu modifikasi terakhir.

%z : Waktu perubahan atribut terakhir.

Contoh (Disini saya menggunakan file yang bernama file_percobaan.txt untuk melakukan eksperimen) :

Misalnya saya ingin mengetahui hanya nama owner dan permission oktal dari file_percobaan.txt, maka saya bisa menggunakan command : 
```bash
stat -c "%U %a" file_percobaan.txt
username 644
```

**2. -t atau --terse** : Digunakan untuk menampilkan informasi dalam format ringkas satu baris. Biasanya memiliki format urutan kolom yang tetap yakni Nama file, Size, Block, Permission(Dalam Heksadesimal), UID, GID, Device ID, Inode Number, Links, Device Type(Digunakan hanya jika file adalah device file (/dev/*), untuk file regullar selalu 0), Device Type Minor(Pasangan field sebelumnya), atime, mtime, ctime, dan btime (satuan waktu yang digunakan timestamp adalah detik sejak 1 jan 1970)

Contoh (Disini saya masih menggunakan file yang bernama file_percobaan.txt untuk melakukan eksperimen seperti pada penjelasan sebelumnya) : 
```bash
stat -t file_percobaan.txt
file_percobaan.txt 25 8 81a4 1000 1000 848 31438 1 0 0 1767193110 1767193119 1767193119 1767193110 4096
```

**










