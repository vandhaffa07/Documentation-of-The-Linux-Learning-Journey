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





