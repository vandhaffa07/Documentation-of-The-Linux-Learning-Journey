# PERINTAH `mv` PADA LINUX

## PENGERTIAN
Perintah`mv` (move) di Linux adalah perintah dasar yang digunakan untuk memindahkan file atau direktori dari satu lokasi ke lokasi lain serta Mengganti nama file atau direktori tanpa benar-benar memindahkannya ke direktori lain. Secara konsep, mv bekerja dengan mengubah referensi lokasi file di sistem file. Jika pemindahan dilakukan dalam satu filesystem yang sama, prosesnya sangat cepat karena tidak menyalin data, melainkan hanya memperbarui metadata.

---

## SINTAKS
Sintak umum perintah `mv` : 
```bash
mv [OPTIONS] [SUMBER] [TUJUAN]
```
---

## OPTION
**1. No option** : Tanpa menggunakan opsi apa pun, perintah mv secara cerdas akan menentukan apakah dia harus mengganti nama (rename) atau memindahkan (move) file berdasarkan kondisi target yang kita berikan.

Jika kita memasukkan nama file sebagai sumber dan nama baru yang belum ada sebagai tujuan di direktori yang sama, maka mv akan berfungsi sebagai alat pengganti nama.

Contoh : 

Disini saya menggunakan direktori kerja yang hanya berisi 1 file bernama laporan.txt untuk melakukan percobaan.
```bash
ls
laporan.txt
```
```bash
mv laporan.txt catatan.txt
```
```bash
ls
catatan.txt
```
Dapat terlihat bahwa file tersebut tidak berpindah tempat, namun identitasnya berubah. Ini berarti sistem hanya mengubah label nama pada metadata file tanpa mengusik data fisiknya di dalam disk.

Berbeda dengan jika argumen tujuan adalah sebuah jalur direktori yang sudah ada, maka mv akan berfungsi untuk memindahkan file tersebut ke dalamnya.

Contoh

(Disini saya masih menggunakan direktori kerja yang sama seperti sebelumnya) 
```bash
mv catatan.txt ~/arsip/
```
```bash
ls

```
```bash
ls ~/arsip/
catatan.txt
```
Setelah perintah mv dijalankan, file catatan.txt menghilang dari direktori kerja saat ini. Hal ini membuktikan bahwa mv telah berhasil memindahkan file tersebut secara fisik ke lokasi baru di dalam folder ~/arsip/.

**2. -i atau --interactive** : Opsi ini membuat mv meminta konfirmasi sebelum menimpa file yang sudah ada di lokasi tujuan.

Misalnya saya mempunyai 2 file bernama data.txt dan data_backup.txt yang terletak pada 2 lokasi yang berbeda dimana data.txt berada pada direktori yang bernama data_penting sedangkan data_backup.txt berada pada direktori backup. Berikut adalah isi konten dari kedua file tersebut :
```bash
cat data_penting/data.txt
Nama    : vandhaffa
Hobi    : Belajar
Negara  : Indonesia
```
```bash
cat backup/data_backup.txt
Ini adalah backup ( KODE : 666 )
```
Jika kita ingin memindahkan file data.txt ke dalam direktori backup dengan nama data_backup.txt ( yang dalam kasus ini sudah ada file yang bernama sama pada direktori tersebut ) tanpa menggunakan option -i, maka file di dalam folder backup akan langsung terhapus dan digantikan oleh file baru. Namun, dengan -i, skenarionya menjadi lebih aman karena mv meminta konfirmasi
```bash
mv -i data_penting/data.txt backup/data_backup.txt
mv: overwrite 'backup/data_backup.txt'? y
```
Terlihat bahwa sistem mendeteksi adanya konflik nama dan tidak langsung mengeksekusi perintah, melainkan menampilkan pesan konfirmasi, jika kita mengetikkan y seperti yang saya lakukan diatas, maka sistem akan melanjutkan proses pemindahan dan menimpa file lama dengan file yang baru
```bash
cat backup/data_backup.txt
Nama    : vandhaffa
Hobi    : Belajar
Negara  : Indonesia
```
Dari sini dapat kita simpulkan bahwa penggunaan opsi -i sangat krusial saat hendak bekerja dengan data penting. Langkah ini memberikan kesempatan bagi pengguna untuk mengantisipasi tindakan sebelum kehilangan data lama secara permanen akibat penimpaan yang tidak disengaja.


**3. -f atau --force** : Opsi ini adalah kebalikan dari -i, dimana ketika dijalankan, opsi ini akan memaksa mv untuk menimpa file tanpa konfirmasi, bahkan jika file tujuan sudah ada.

Contoh :

Misalkan saya memiliki sebuah file konfigurasi baru bernama testing.cpp yang ingin saya pindahkan ke dalam folder bernama belajar_cpp/. Namun, tanpa saya sadari, ternyata di dalam folder tersebut sudah terdapat file lama dengan nama yang sama, yaitu belajar_cpp/testing.cpp
```bash
cat testing.cpp
#include <iostream> 
using namespace std;

int main() { 
int x, y;
int sum;
cout << "Type a number: ";
cin >> x;
cout << "Type another number: ";
cin >> y;
sum = x + y;
cout << "Sum is: " << sum;
}
```
```bash
cat belajar_cpp/testing.cpp
#include <iostream> 
using namespace std;

int main() { 
    cout << "Hello, World!" <<endl; 
    return 0; 
}

```
Saya kemudian menjalankan perintah mv dengan menambahkan opsi paksa (-f) :
```bash
 mv -f update.conf game/update.conf
```
```bash
cat update.conf
cat: update.conf: No such file or directory
```
```bash
cat game/update.conf
#include <iostream> 
using namespace std;

int main() { 
int x, y;
int sum;
cout << "Type a number: ";
cin >> x;
cout << "Type another number: ";
cin >> y;
sum = x + y;
cout << "Sum is: " << sum;
}
```
Dapat terlihat bahwa file update.conf baru yang ada pada direktori kerja saya saat ini langsung dipindahkan ke dalam folder game tanpa konfirmasi

**4. -n atau --no-clobber** : Opsi ini mencegah penimpaan file jika file tujuan sudah ada

Misalnya saya sedang merapikan koleksi dokumen dan ingin memindahkan file tugas.docx ke folder Arsip/. Namun, saya lupa bahwa di dalam folder Arsip/ sudah ada file bernama tugas.docx. Karena saya tidak ingin file lama di folder Arsip terganggu sama sekali, maka saya menggunakan option -n ketika memindahkannya 
```bash
cat tugas.docx
Ini adalah tugas VERSI BARU yang baru saja selesai.
```
```bash
cat Arsip/tugas.docx
Ini adalah tugas VERSI LAMA yang sudah tersimpan sejak bulan lalu.
```
```bash
mv -n tugas.docx Arsip/tugas.docx
```
```bash
cat Arsip/tugas.docx
Ini adalah tugas VERSI LAMA yang sudah tersimpan sejak bulan lalu.
```
Dapat terlihat bahwa file tugas.docx yang berada di dalam folder Arsip/ tetap utuh dan tidak tersentuh sedikit pun.

**4. -u atau --update** : Opsi ini hanya akan memindahkan file jika: File sumber lebih baru dari file tujuan, atau File tujuan belum ada.

Sebagai contoh, saya sedang melakukan sinkronisasi file konfigurasi dari direktori kerja ke direktori cadangan (backup/). Karena file konfigurasi sering diperbarui, saya ingin menggunakan opsi -u untuk memastikan bahwa hanya file dengan versi terbaru yang dipindahkan.

File konfigurasi di direktori kerja:
```bash
cat config.conf
version=1.2
last_update=2026-01-01
```
```bash
stat config.conf
  File: config.conf
  Size: 42              Blocks: 8          IO Block: 4096   regular file
Device: 8,48      Inode: 1287345     Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2026-01-01 20:55:33.478545608 +0700
Modify: 2026-01-01 20:54:18.398626407 +0700
Change: 2026-01-01 20:54:18.398626407 +0700
 Birth: 2026-01-01 20:54:10.106614478 +0700
```
File konfigurasi di direktori backup (lebih baru):
```bash
cat backup/config.conf
version=1.3
last_update=2026-01-02
```
```bash
stat backup/config.conf
  File: backup/config.conf
  Size: 42              Blocks: 8          IO Block: 4096   regular file
Device:8,48      Inode: 1287391     Links: 1
Access: 2026-01-02 14:39:34.014588764 +0700
Modify: 2026-01-02 14:40:19.802550008 +0700
Change: 2026-01-02 14:40:19.802550008 +0700
 Birth: 2026-01-02 14:39:34.014588764 +0700
```
Terlihat bahwa backup/config.conf memiliki waktu Modify yang lebih baru.
```bash
mv -u config.conf backup/config.conf
```
```bash
cat backup/config.conf
version=1.3
last_update=2026-01-02
```
Dapat terlihat bahwa isi file di folder backup/ tidak berubah. Ini berarti perintah mv -u tlah berhasil dan secara cerdas mengabaikan proses pemindahan karena file tujuan sudah memiliki versi yang lebih baru (atau sama), sehingga data penting kita terlindungi dari penimpaan oleh versi yang lebih lama

**5. -v atau --verbose** : Digunakan untuk menampilkan proses pemindahan file yang sedang terjadi

Contoh : 
```bash
mv -v catatan.txt note.txt
renamed 'catatan.txt' -> 'note.txt'
```
Dapat terlihat bahwa suatu file yang sedang diproses akan memunculkan laporan status di terminal. 


