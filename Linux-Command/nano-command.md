# PERINTAH `nano` PADA LINUX

## PENGERTIAN
`nano` merupakan text editor ringan pada sistem operasi Linux yang berfungsi untuk mengelola file teks. Alat ini dirancang secara efisien untuk membantu pengguna dalam membuat, membuka, maupun memodifikasi file teks secara ringan melalui antarmuka yang sederhana

---

## SINTAKS DASAR

```bash
nano [OPTION] [NAMA_FILE]
```

---

## PENGGUNAAN NANO TANPA OPTION
Jika nama file sudah ada di direktori saat ini, Nano akan langsung membuka dan mengarahkan kita masuk ke dalam editor untuk menyuntingnya. Namun, jika file belum tersedia, Nano akan otomatis menciptakan ruang baru dan membawa Anda langsung ke antarmuka pengeditan. 

Contoh :

Disini saya akan melihat terlebih dahulu, pada direktori saat ini ada file apa saja dan isinya apa
```bash
ls
file1.txt
```
```bash
cat file1.txt
Ini adalah file pertama
```
Dapat terlihat bahwa pada direktori tersebut hanya ada 1 file bernama file1.txt yang isinya "Ini adalah file pertama". Selanjutnya saya akan mengedit file1.txt menggunakan nano :
```bash
nano file1.txt
```
<img width="1919" height="946" alt="image" src="https://github.com/user-attachments/assets/5d5082be-9932-448e-a1b7-506e93961b7f" />

Setelah menambah teks baru pada file tersebut, tekan CTRL+O untuk menyimpan dan memberi nama pada file, kemudian enter dan tekan CTRL+X untuk keluar dari editor nano
```bash
cat file1.txt
Ini adalah file pertama
Halo, semuanya. Apa Kabar?
```
Dapat terlihat bahwa penambahan teks yang kita lakukan telah tersimpan pada file tersebut. Selanjutnya, saya akan melakukan percobaan command nano kepada file yang belum tersedia :
```bash
nano file2.txt
```
<img width="1919" height="947" alt="image" src="https://github.com/user-attachments/assets/1e97b334-50d7-4900-a41d-804b004f1002" />

Terlihat dengan jelas bahwa saat file belum tersedia belum tersedia, Nano akan otomatis menciptakan ruang baru dan membawa kita langsung ke antarmuka pengeditan. Selanjutnya saya tinggal melakukan pengecekan apakah pengeditan saya berhasil atau tidak :
```bash
cat file2.txt
Ini adalah file kedua
```

---

## OPTION
**1. -B atau --backup** : Digunakan untuk membuat salinan backup sebelum mengedit file. Option ini penting untuk file-file penting yang membutuhkan pengeditan dan pemulihan agar jika terjadi kesalahan kita bisa mengembalikan file ke kondisi semula, misalnya file konfigurasi. File backup biasanya diberi akhiran ~

Contoh :
```bash
cat > testing.txt
Ini untuk backup
```
```bash
 nano -B testing.txt
```
<img width="1919" height="949" alt="image" src="https://github.com/user-attachments/assets/f02920cb-d07e-4ea4-82f2-d7338d8f3efc" />

```bash
ls
testing.txt  testing.txt~
```
```bash
cat testing.txt && cat testing.txt~
Telah diedit
Ini untuk backup
```
Dapat terlihat bahwa file backup dari testing.txt serta file aslinya memiliki isi/konten yang sama seperti saat saya melakukan pengeditan.

**2. -C atau --backupdir** : Jika -B menyimpan backup pada direktori yang sama dimana file asli berada, maka -C dapat menyimpan file backup pada direktori lain sesuai keinginan dengan menggunakan path lengkap direktori tersebut.

Catatan : option -C wajib digunakan bersamaan dengan -B. Jika tidak, maka nano tidak akan menyalin file yang kita edit untuk dijadikan file backup. 

Misalnya saya mempunyai susunan folder seperti ini :
```bash
folder_percobaan/
├── folder_untuk_backup
└── folder_untuk_file
    └── file_percobaan.txt
```
```bash
cat file_percobaan.txt
Ini adalah file backup
```
Kemudian saya ingin mengedit file_percobaan.txt dan menyimpan backupnya di dalam folder "folder_untuk_backup" :
```bash
nano -B -C ~/folder_percobaan/folder_untuk_backup/ file_percobaan.txt
```
<img width="1919" height="949" alt="image" src="https://github.com/user-attachments/assets/a73421d2-4029-465c-b4e1-cc165b9faa1c" />

Kemudian kita cek bagaimana susunan terbaru folder_percobaan ini dan isi file nya apa : 
```bash
tree folder_percobaan/
folder_percobaan/
├── folder_untuk_backup
│   └── !home!vandhaffa!folder_percobaan!folder_untuk_file!file_percobaan.txt~
└── folder_untuk_file
    └── file_percobaan.txt

3 directories, 2 files
```
```bash
cat folder_percobaan/folder_untuk_file/file_percobaan.txt && cat folder_percobaan/folder_untuk_backup/\!home\!vandhaffa\!folder_percobaan\!folder_untuk_file\!file_percobaan.txt~
Ini adalah file backup yang sudah diedit
Ini adalah file backup
```
Dapat terlihat bahwa lokasi file backup telah sesuai dengan apa yang kita ketikkan, selain itu isi kedua file tersebut juga susah sesuai dengan pengeditan kita.

**3. -E atau --tabtospaces** : Digunakan untuk mengubah tab menjadi beberapa spasi saat mengetik. Secara default, option ini akan mengubah tab menjadi 8 karakter spasi bukan 1 karakter tunggal
Contoh :
```bash
nano -E nama_file
```

**4. -T atau --tabsize** : Digunakan untuk mengatur lebar visual tab sesuai keinginan pengguna. Berbeda dengan opsi -B yang secara otomatis mengonversi satu tab menjadi 8karakter spasi statis, -T memberikan fleksibilitas untuk menentukan sendiri lebar indentasi tersebut. Perbedaan utamanya terletak pada struktur filenya. Jika -B mengubah tab menjadi rentetan spasi, -T hanya mengatur tampilan lebarnya saja sementara karakter di dalamnya tetap terbaca sebagai satu karakter tab tunggal.
Contoh : 
```bash
nano -T 2 nama_file
```
Maka command tersebut akan mengubah lebar tab menjadi 2 spasi, walaupun tetap terbaca sebagai satu karakter tab tunggal.

Catatan : Jika opsi -E digunakan bersamaan dengan -T, maka lebar tab yang telah ditentukan melalui -T akan langsung dikonversi oleh -E menjadi rentetan spasi sungguhan saat Anda mengetik.

Contoh : 
```bash
nano -E -T 4 nama_file
```
Maka, Nano akan masuk ke mode pengeditan di mana setiap kali Anda menekan tombol Tab, ia akan otomatis berubah menjadi 4 karakter spasi.

**5 -R atau --restricted** : Digunakan untuk menjalankan editor dalam mode terbatas, di mana pengguna hanya dapat mengedit file yang diberikan tanpa kemampuan untuk menjalankan perintah shell, membuka file lain secara bebas, atau menyimpan file ke lokasi berbeda. Mode ini berguna dalam lingkungan multi-user dan sistem dengan kebutuhan keamanan dasar.

Contoh : 

* **Nano normal (tanpa -R)** :
```bash
nano catatan.txt
```
Kemudian saat di dalam nano saya melakukan aksi CTRL + T. Aksi ini merupakan fitur nano yang digunakan untuk menjalankan perintah shell walaupun sedang didalam text editor, dimana output dari perintah tersebut akan masuk dan ditampilkan ke dalam editor menjadi isi dari file yang kita edit.

<img width="1919" height="948" alt="image" src="https://github.com/user-attachments/assets/e8f938eb-8f94-4bd7-80b8-d4d31efe1a36" />

<img width="1919" height="950" alt="image" src="https://github.com/user-attachments/assets/95bb778b-932a-481f-b2fe-768341a894aa" />


* **Nano dengan option -R** :
```bash
nano -R catatan.txt
```
Kemudian saya melakukan aksi yang sama yakni CTRL+T, maka yang terjadi adalah : 

<img width="1919" height="949" alt="image" src="https://github.com/user-attachments/assets/0d183c80-e083-41d0-be81-024ea6993533" />


Dapat dilihat bahwa nano -R menolak untuk membuka prompt command. Dengan kata lain, option ini sangat berguna untuk membatasi akses sistem dari dalam editor sehingga memberikan kita kontrol dan keamanan yang tinggi.

**6. -S atau --softwrap** : Digunakan untuk membungkus baris panjang agar tidak horizontal scroll.

Contoh :

Tanpa option S : 
```bash
nano S_option_testing.txt
```
<img width="1919" height="952" alt="image" src="https://github.com/user-attachments/assets/bd9fd554-7ef6-48ff-b191-620a39965af4" />

Terlihat bahwa file tersebut hanya terdiri dari satu baris yang sangat panjang. Secara default, saat teks mencapai batas layar, Nano akan memunculkan tanda ">" sebagai indikator bahwa masih ada kelanjutan string di sisi kanan. Namun, jika kita menggunakan opsi -S (softwrap), Nano akan otomatis membungkus (wrap) baris panjang tersebut agar seluruh teks turun ke bawah dan terlihat di dalam jendela layar tanpa perlu menggeser kursor ke kanan : 
```bash
nano -S S_option_testing.txt
```
<img width="1919" height="953" alt="image" src="https://github.com/user-attachments/assets/c511fffd-38ca-437c-a3e7-468788c8ffd3" />

**7. -c atau --constantshow** : Digunakan untuk memantau posisi kursor secara presisi dan real-time saat menavigasi isi file. Begitu masuk ke editor, baris status di bagian bawah layar akan terus diperbarui secara otomatis untuk menampilkan detail lokasi kursor, mulai dari nomor baris, nomor kolom, hingga persentase progres posisi Anda di dalam file tersebut.

Contoh :
```bash
nano -c c-option-testing.txt
```

<img width="1919" height="950" alt="image" src="https://github.com/user-attachments/assets/d5cdd7c8-bf5e-4c59-be5c-33e3f519fce4" />

**8. -i atau --autoindent** : Digunakan untuk untuk mengaktifkan indentasi otomatis pada setiap baris baru. Saat kita menekan Enter, kursor akan langsung melompat ke baris bawah dengan posisi indentasi (jarak dari kiri) yang sejajar dengan baris sebelumnya. Fitur ini sangat memudahkan saat menulis kode program agar struktur penulisan tetap rapi tanpa harus menekan spasi secara manual di setiap baris.

Contoh :
```bash
nano -i i-option-testing.cpp
```

<img width="1919" height="946" alt="image" src="https://github.com/user-attachments/assets/9e7bc904-6611-45e3-b21b-9e0948bd3a79" />

Dapat dilihat bahwa setiap kali saya membuat identasi baru, baris dibawahnya akan langsung mengikuti level indentasi dari baris sebelumnya.

**9. -l atau --linenumbers** : Digunakan untuk menampilkan nomor baris di sisi kiri layar.

Contoh : 
```bash
nano -l buah.txt
```
<img width="1919" height="953" alt="image" src="https://github.com/user-attachments/assets/792a66eb-f390-4d9d-851e-4641e97f6733" />

**10. -m atau --mouse** : Mengaktifkan dukungan mouse sepenuhnya di dalam lingkungan Nano. Dengan fitur ini, kita bisa langsung melompat ke posisi kursor mana pun hanya dengan satu klik, melakukan scrolling untuk menelusuri file, serta menyeleksi teks secara instan. Ini memberikan fleksibilitas navigasi modern tanpa harus bergantung sepenuhnya pada tombol panah (arrow keys) di keyboard.

Contoh :
```bash
nano -m nama_file.txt
```

## SHORTCUT NANO
* **File & Session Control**
  
| Shortcut | Fungsi | Penjelasan |
| :--- | :--- | :--- |
| **Ctrl + O** | Write Out | Menyimpan file yang sedang diedit. |
| **Ctrl + X** | Exit | Keluar dari Nano (meminta konfirmasi jika ada perubahan yang belum disimpan). |
| **Ctrl + R** | Read File | Menyisipkan konten dari file lain ke posisi kursor saat ini. |
| **Ctrl + G** | Help | Menampilkan menu bantuan dan panduan penggunaan Nano. |
| **Ctrl + C** | Cursor Info | Menampilkan informasi posisi kursor (nomor baris dan kolom) di bagian bawah. |

* **Navigasi Cepat**

| Shortcut | Fungsi | Penjelasan Rinci |
| :--- | :--- | :--- |
| **Ctrl + A** | Start of Line | Melompat ke awal baris tempat kursor berada. |
| **Ctrl + E** | End of Line | Melompat ke akhir baris tempat kursor berada. |
| **Alt + G** | Go To Line | Melompat ke nomor baris tertentu sesuai input pengguna. |





