# PERINTAH `find` PADA LINUX

## PENGERTIAN
`find` adalah perintah yang digunakan untuk mencari file dan direktori berdasarkan berbagai kriteria yang sangat spesifik. Berbeda dengan perintah pencarian biasa seperti locate yang mengandalkan database, find bekerja dengan menyisir struktur direktori secara real-time. Ia bisa mencari berdasarkan
Nama file atau ekstensi, ukuran file (lebih besar atau lebih kecil dari nilai tertentu), waktu (kapan file terakhir diubah, diakses, atau dibuat), kepemilikan (siapa pemilik file atau grupnya), izin akses (mencari file yang bisa dieksekusi atau dibaca saja), hingga tipe file (file biasa, direktori, atau link).

---

## SINTAKS 
Sintaks umum perintah `find` adalah :
```bash
find [PATH_LOKASI] [KRITERIA] [AKSI]
```

---

## WILAYAH PENCARIAN/PATH
| Simbol / Path | Nama Wilayah | Cakupan Pencarian | Contoh Perintah |
| :--- | :--- | :--- | :--- |
| **.** | Current Directory | Folder saat ini dan semua sub-foldernya. | `find . -name "catatan.txt"` |
| **..** | Parent Directory | Satu level di atas folder saat ini. | `find .. -name "config.py"` |
| **~** | Home Directory | Seluruh folder pribadi user (Home). | `find ~ -name "*.jpg"` |
| **/** | Root Directory | Seluruh sistem komputer (dari akar/root). | `sudo find / -name "vmlinuz"` |
| **/var/log** | Absolute Path | Folder spesifik yang ditentukan jalurnya secara lengkap. | `find /var/log -name "*.log"` |

---

## OPTION/KRITERIA
**1. -name** : -name digunakan untuk mencari file yang namanya pas atau cocok dengan pola tertentu. Perlu diingat bahwa opsi ini bersifat case-sensitive (membedakan huruf besar dan kecil).

Sebagai contoh, saya akan menyimulasikan bagaimana cara mencari file di dalam direktori kerja saya saat ini menggunakan kriteria -name

Mula-mula, kita akan membuat beberapa file dengan nama yang mirip tapi berbeda huruf kapitalnya sebagai bahan percobaan kita seperti ini : 
```bash
touch laporan.txt LAPORAN.txt 
```
Selanjutnya, saya akan mencoba mencari file dengan nama laporan.txt (huruf kecil semua) menggunakan perintah seperti ini :
```bash
find . -name "laporan.txt"
./laporan.txt
```
Dapat terlihat bahwa hanya satu file yang muncul yakni laporan.txt. Dapat terlihat pula bahwa file LAPORAN.txt diabaikan karena find sangat disiplin terhadap perbedaan huruf besar dan kecil.

**2. -iname** : Kriteria ini berfungsi untuk mencari file berdasarkan nama, sama seperti -name, namun bersifat case-insensitive (mengabaikan perbedaan huruf besar dan kecil).

Sebagai contoh, saya akan menggunakan file dari simulasi sebelumnya (laporan.txt, LAPORAN.txt) untuk melihat fleksibilitas kriteria -iname seperti ini:
```bash
find . -iname "laporan.txt"
./laporan.txt
./LAPORAN.txt
```
Dapat terlihat bahwa perintah ini menampilkan kedua file tersebut meskipun salah satunya menggunakan huruf besar semua sedangkan satunya lagi menggunakan huruf kecil semua. Hal ini membuktikan bahwa Kriteria -iname memperlakukan "L" dan "l" sebagai karakter yang sama(case-insensitive)

**3. -type** : Kriteria ini digunakan untuk memfilter pencarian berdasarkan jenis filenya

Berikut adalah daftar lengkap simbol tipe file yang didukung oleh kriteria -type:

| Simbol | Jenis File | Penjelasan Singkat |
| :--- | :--- | :--- |
| **f** | Regular File | File biasa seperti dokumen teks, gambar, video, atau eksekusi biner. |
| **d** | Directory | Folder yang berisi daftar nama file lain. |
| **l** | Symbolic Link | File penunjuk atau shortcut yang mengarah ke lokasi file lain. |
| **b** | Block Special | Perangkat keras penyimpan data dalam blok (seperti Hardisk/Flashdisk). |
| **c** | Character Special | Perangkat yang mengirim data karakter per karakter (seperti Keyboard/Terminal). |
| **p** | Named Pipe (FIFO) | Saluran komunikasi antar dua proses (First-In-First-Out). |
| **s** | Socket | Titik komunikasi untuk pertukaran data antar proses (jaringan/sistem lokal). |

Sebagai contoh, saya akan membuat berbagai jenis tipe file di dalam sebuah folder, lalu saya akan menggunakan find dengan kriteria -type untuk membedah beberapa simbolnya satu per satu.

Mula-mula, saya akan menyiapkan bahan percobaannya terlebih dahulu dengan membuat folder, file reguler, dan symlink menggunakan serangkaian perintah berikut:
```bash
mkdir folder_tes && touch file_biasa.txt
```
```bash
ln -s file_biasa.txt ini_link
```
```bash
ls
file_biasa.txt  folder_tes  ini_link
```
Dapat terlihat bahwa kita sekarang memiliki 3 file dengan tipe yang berbeda-beda dalam satu direktori.

Sebagai langkah awal, kita akan coba instruksikan find untuk melacak direktoi menggunakan kriteria -type d. Maka, seharusnya output yang muncul hanyalah folder_tes dan direktori saat ini (.), untuk mencobanya, kita bisa menggunakan perintah seperti ini :
```bash
find . -type d
.
./folder_tes
```
Dapat terlihat bahwa memang benar, hanya nama folder yang muncul pada output tersebut (termasuk titik . yang menandakan direktori saat ini). Hal ini membuktikan bahwa -type d berhasil dijalankan dengan cara mengeliminasi file-file lainnya karena tidak memenuhi syarat sebagai sebuah folder.

Beranjak ke kebutuhan yang lebih umum, kita akan mencoba memfilter file reguler dengan kriteria -type f. Logikanya, hasil yang keluar seharusnya hanyalah file_biasa.txt, untuk mencobanya, kita bisa menggunakan perintah seperti ini :
```bash
find . -type f
./file_biasa.txt
```
Dapat terlihat bahwa meskipun di sana terdapat banyak tipe dengan nama yang beragam, hanya file regulerlah yang ditampilkan. Ini membuktikan kriteria kita berhasil membedakan file data dari tipe-tipe lainnya.

Selanjutnya, kita akan menguji kemampuan find dalam melacak symbolic link bernama ini_link yang sebelumnya telah kita buat. Maka kita bisa menggunakan perintah seperti ini : 
```bash
 find . -type l
./ini_link
```
Dapat terlihat bahwa find dengan sigap mengidentifikasi tautan simbolis tersebut. Kemampuan ini sangat krusial bagi kita saat harus memetakan dependensi antar file di sistem yang kompleks.

Sebagai penutup percobaan ini, kita akan melompat ke wilayah sistem /dev untuk mencari perangkat keras penyimpanan (block device) dengan kriteria -type b. Seperti yang kita pahami pada dokumentasi sebelumnya mengenai [special file](https://github.com/vandhaffa07/Documentation-of-The-Linux-Learning-Journey/blob/main/Linux-Command/file-command.md#lanjutan-penjelasan-option) direktori /dev berisi file khusus yang berfungsi sebagai antarmuka ke perangkat keras fisik. Oleh karena itu, secara logika seharusnya output dari perintah ini akan menampilkan daftar file perangkat seperti sda, sdb, loop, dan sejenisnya:
```bash
sudo find /dev -type b
/dev/sdd
/dev/sdc
/dev/sdb
/dev/sda
/dev/loop7
/dev/loop6
...........dst
```
Dapat terlihat bahwa prediksi kita kembali terbukti secara akurat. Dengan kriteria ini, kita berhasil membedah isi /dev dan hanya mengambil file yang merepresentasikan perangkat penyimpanan blok. Perintah find secara cerdas mengabaikan file karakter (character devices) seperti terminal atau sensor, dan hanya menyajikan daftar hardware penyimpanan yang kita butuhkan.

**4. -size** : Kriteria ini digunakan untuk memfilter file berdasarkan ukurannya. 

**Satu hal krusial yang harus kita pahami adalah penggunaan operator unit dan simbol arah pada nilainya yakni :**

* (+): Untuk mencari file yang lebih besar dari nilai tersebut.

* (-) : Untuk mencari file yang lebih kecil dari nilai tersebut.

* Tanpa Simbol : Untuk mencari ukuran yang persis.
  
**Unit Ukuran yang Kita Gunakan :**

* c : Bytes

* k : Kilobytes

* M : Megabytes

* G : Gigabytes

Sebagai simulasi, kita akan menciptakan beberapa file dengan bobot yang berbeda untuk menguji seberapa akurat kriteria -size pada find bekerja.

Mula-mula, kita siapkan bahan percobaan menggunakan perintah truncate. Kita akan membuat tiga file dengan ukuran yang sangat kontras yakni 1MB, 10MB, dan 100MB :
```bash
truncate -s 1M kecil.dat && truncate -s 10M sedang.dat && truncate -s 100M besar.dat
```
```bash
ls -lh
total 0
-rw-r--r-- 1 vandhaffa vandhaffa 100M Jan  7 17:29 besar.dat
-rw-r--r-- 1 vandhaffa vandhaffa 1.0M Jan  7 17:29 kecil.dat
-rw-r--r-- 1 vandhaffa vandhaffa  10M Jan  7 17:29 sedang.dat
```
Dapat terlihat bahwa kita sekarang memiliki tiga target dengan rentang ukuran 1MB, 10MB, dan 100MB.

Mari kita mulai pengujiannya, sebagai langkah awal, saya akan mencoba instruksikan find untuk melacak file yang ukurannya lebih besar dari 50MB menggunakan kriteria -size +50M seperti ini : 
```bash
find . -size +50M
./besar.dat
```
Dapat terlihat bahwa hanya file besar.dat yang ditampilkan karena ia memiliki ukuran sebesar 100MB, dapat terlihat pula bahwa file-file lainnya seperti sedang.dat dan kecil.dat otomatis tereliminasi karena tidak memenuhi ambang batas minimal yang kita tentukan.

Beranjak ke percobaan berikutnya, kita ingin mencari file yang lebih kecil dari 5MB dengan kriteria -size -5M. Maka, seharusnya output yang kita dapatkan hanyalah kecil.dat seperti ini :
```bash
find . -size -5M
./kecil.dat
```
Kita juga bisa menggunakan rentang ukuran untuk mencari sebuah file dengan cara menggabungkan dua kriteria -size sekaligus. Misalnya, kita ingin mencari file yang ukurannya lebih besar dari 5MB tapi tidak lebih dari 20MB. Secara logika, hanya sedang.dat lah yang masuk dalam kriteria ini:
```bash
find . -size +5M -size -20M
./sedang.dat
```
Dapat terlihat bahwa prediksi kita tepat. Perintah ini akan menyaring semua file, membuang yang terlalu kecil dan menyingkirkan yang terlalu besar, hingga menyisakan target yang kita inginkan.

**5. -mtime, -atime, dan -ctime** : Kriteria ini digunakan untuk melacak jejak waktu sebuah file(timestamp)

Sama seperti kriteria ukuran, kita akan menggunakan operator + ( untuk lebih lama dari ) dan - (untuk lebih baru dari ). Angka yang kita masukkan di sini hitungannya adalah hari.

Agar kita bisa melihat perbedaannya secara nyata, kita akan memanipulasi waktu pembuatan file agar ada yang terlihat seperti file lama dan ada yang file baru

Mula-mula, kita buat 3 file dengan waktu modifikasi yang berbeda menggunakan bantuan perintah touch -d seperti ini :
```bash
touch -d "5 days ago" file_lama.log && touch -d "2 days ago" file_sedang.log && touch baru_banget.log
```
Sekarang, kita verifikasi jejak waktunya agar kita bisa memastikan apakah timestamp masing-masing file telah sesuai dengan yang kita inginkan menggunakan command stat -c seperti ini : 
```bash
stat -c "%x- %y %z" file_lama.log && stat -c "%x- %y %z" file_sedang.log && stat -c "%x- %y %z" baru_banget.log
2026-01-02 18:51:43.753077175 +0700- 2026-01-02 18:51:43.753077175 +0700 2026-01-07 18:51:43.750663965 +0700
2026-01-05 18:51:43.759252625 +0700- 2026-01-05 18:51:43.759252625 +0700 2026-01-07 18:51:43.754663965 +0700
2026-01-07 18:51:43.762663964 +0700- 2026-01-07 18:51:43.762663964 +0700 2026-01-07 18:51:43.762663964 +0700
```
(Catatan : Saya membuat catatan ini pada tanggal 7 Januari 2026)

Dapat terlihat bahwa kita sekarang punya koleksi file dengan rentang waktu yang berbeda-beda. Mari kita mulai eksperimennya.

Misalnya, kita ingin mencari file yang sudah dimodifikasi lebih dari 3 hari yang lalu. Maka kita gunakan kriteria -mtime +3 seperti ini :
```Bash
find . -mtime +3
./file_lama.log
```
Dapat terlihat bahwa find hanya memunculkan file_lama.log. Hal ini terjadi karenafile tersebut usianya sudah 5 hari yang artinya melewati ambang batas 3 hari yang kita tentukan.

Bagaimana jika kasusnya kita baru saja mengedit sebuah file tapi lupa namanya? Kita bisa mencari file yang dimodifikasi kurang dari 1 hari terakhir dengan kriteria -mtime -1 seperti ini :
```bash
find . -mtime -1
./baru_banget.log
```
Dapat terlihat bahwa find  menunjukkan file yang baru saja kita buat. Ini adalah cara tercepat untuk menemukan pekerjaan terakhir kita di tengah ribuan file lainnya.

**6. -user dan -group** : Kriteria ini digunakan untuk menyaring file berdasarkan identitas pemiliknya (User) atau kelompoknya (Group).

Sebagai contoh, disini kita akan melihat bagaimana cara find mengenali file milik user kita saat ini dan file milik user root.

Mula-mula, kita buat sebuah file di direktori kerja kita dan kita cek siapa pemilik aslinya:
```bash
stat -c "%U %u %G %g" file_saya.txt
vandhaffa 1000 vandhaffa 1000
```
Dapat terlihat bahwa file tersebut dimiliki oleh username dan group saya, yakni vandhaffa

Sekarang, mari kita instruksikan find untuk mencari semua file di direktori saat ini yang dimiliki oleh user vandhaffa menggunakan kriteria -user seperti ini :
```bash
 find . -user vandhaffa
./file_sedang.log
./baru_banget.log
./file_lama.log
./file_saya.txt
```
Dapat terlihat bahwa find hanya menampilkan file yang memang terdaftar atas nama kita(termasuk file-file yang kita gunakan pada percobaan kriteria sebelumnya karena saya menggunakan direktori yang sama untuk percobaan kali ini)

Selanjutnya, mari kita mencoba untuk mencari file di folder sistem /etc yang dimiliki oleh user root. Karena ini folder sistem, kita akan gunakan sudo seperti ini:
```bash
/etc
/etc/pulse
/etc/pulse/client.conf
/etc/pulse/client.conf.d
/etc/login.defs
/etc/apt
/etc/apt/sources.list.save
............................dst
```
Dapat terlihat bahwa outputnya akan dipenuhi oleh file-file yang digunakan untuk keperluan sistem operasi

Hal yang sama juga berlaku untuk kelompok atau grup. Jika kita ingin mencari file berdasarkan grupnya, kita tinggal menggunakan kriteria -group seperti ini :
```bash
find . -group vandhaffa
./file_sedang.log
./baru_banget.log
./file_lama.log
./file_saya.txt
```

**7. -perm** : Kriteria ini digunakan untuk memfilter file berdasarkan hak aksesnya. Sebagai praktisi Linux, kita tahu bahwa keamanan sistem sangat bergantung pada pengaturan izin yang tepat (seperti 755, 644, atau 777). Dengan kriteria ini, kita bisa mendeteksi file mana saja yang mungkin terlalu "terbuka" dan berbahaya bagi sistem. [Catatan Tentang Permission](https://github.com/vandhaffa07/Documentation-of-The-Linux-Learning-Journey/blob/main/Linux-Command/mkdir-command.md#konsep-permission-pada-direktori-dan-file-linux)

Mari kita buat simulasi dengan mengatur beberapa file agar memiliki izin yang berbeda-beda, lalu kita akan mencoba mencarinya menggunakan find -perm.

Mula-mula, kita siapkan 3 file berbeda dan kita atur izinnya menggunakan perintah chmod seperti ini :
```bash
touch rahasia.txt umum.txt skrip.sh && chmod 600 rahasia.txt && chmod 644 umum.txt && chmod 755 skrip.sh
```

Sekarang, mari kita verifikasi dulu izin aksesnya agar kita punya gambaran yang jelas:
```bash
stat -c "%a %A" rahasia.txt &&  stat -c "%a %A" umum.txt &&  stat -c "%a %A" skrip.sh
600 -rw-------
644 -rw-r--r--
755 -rwxr-xr-x
```
Dapat terlihat bahwa kita sekarang memiliki file dengan izin yang bervariasi. Mari kita coba lacak satu per satu.

Misalnya, kita ingin mencari file yang sangat privat, yaitu file yang memiliki izin 600 (hanya pemilik yang punya akses). Kita gunakan kriteria -perm 600 seperti ini : 
```bash
find . -perm 600
./rahasia.txt
```
Dapat terlihat bahwa find hanya menampilkan rahasia.txt dan ini memang sesuai dengan izin akses yang kita atur pada file tersebut

Selanjutnya, kita akan mencoba mencari file yang bisa dieksekusi oleh semua orang, yakni yang memiliki izin 755. Maka kita bisa menggunakan perintah seperti ini :
```bash
find . -perm 755
./skrip.sh
```









