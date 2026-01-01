

## PENGERTIAN
Nano adalah Text editor di terminal Linux yang digunakan untuk membuat, membuka, serta mengedit file teks. 

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
<img width="1919" height="1002" alt="Cuplikan layar 2026-01-01 105701" src="https://github.com/user-attachments/assets/df8284e8-d4f4-49db-9c39-3b04bedcb8d6" />

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
<img width="1919" height="1003" alt="image" src="https://github.com/user-attachments/assets/b7f6d3d3-755e-4426-9727-b008852428fe" />

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
<img width="1919" height="999" alt="image" src="https://github.com/user-attachments/assets/398f7eca-5010-4ea4-ba95-fa1f5b782ff7" />

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
<img width="1916" height="1000" alt="image" src="https://github.com/user-attachments/assets/f782c6ff-f98f-4029-8c6c-7afaf4b78e35" />

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

**5. -F atau --multibuffer** : Digunakan untuk membuka beberapa file sekaligus. Digunakan saat kita perlu beralih bolak-balik antara dua atau lebih file dalam satu sesi nano tanpa harus menutup salah satunya. Saat dijalankan nano akan memiliki bilah status yang menunjukkan file mana yang sedang aktif, kita dapat beralih menggunakan Alt+< dan Alt+>







