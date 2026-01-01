# PERINTAH `cp` PADA LINUX

## PENGERTIAN
Perintah `cp` (*copy*) di Linux digunakan untuk menyalin atau menduplikasi file maupun direktori dari satu lokasi ke lokasi lain.  Perintah ini sangat fundamental dalam manajemen file karena hampir semua aktivitas administrasi sistem melibatkan proses penyalinan data.

⚠️Catatan : Secara default, `cp` hanya menyalin file, unntuk menyalin direktori, harus menggunakan opsi tertentu

---

## SINTAKS
Sintaks umum perintah `cp` adalah :
```bash
cp [OPTION] [YANG AKAN DICOPY] [NAMA/TUJUAN HASIL COPY]
```
---

## OPTION
**1. No option**: Digunakan untuk menyalin file ke lokasi tujuan

Misalnya saya mempunyai sebuah folder bernama "test" yang berisi hanya 1 file seperti ini :
```bash
tree test/
test/
└── menyapa.txt

1 directory, 1 file
```
Selanjutnya, untuk menduplikasi file tersebut di dalam direktori yang sama, saya cukup menjalankan perintah berikut:
```bash
cd test/
```
```bash
cp menyapa.txt salinan_menyapa.txt
```
Selanjutnya saya ingin mengecek apakah benar bahwa salinan_menyapa.txt telah tersimpan di direktori test serta memiliki isi yang sama dengan file menyapa.txt. Maka, saya cukup menggunakan command seperti ini : 
```bash
ls
menyapa.txt  salinan_menyapa.txt
```
```bash
cat menyapa.txt && cat salinan_menyapa.txt
HALOOHALOO
```
Terlihat dengan jelas bahwa file berhasil diduplikasi secara sempurna di dalam direktori yang sama. Penggunaan perintah cp tidak hanya menyalin nama file, tetapi juga mereplikasi seluruh isinya tanpa perubahan sedikit pun. 

Tidak sampai disitu, perintah cp juga bisa menyimpan salinan file ke lokasi atau folder yang berbeda dari file sumbernya. Misalnya, saya ingin menyalin kembali file menyapa.txt ke lokasi folder kosong bernama folder_salinan yang tidak memiliki file apapun :
```bash
tree ~/folder_salinan/
folder_salinan/

0 directories, 0 files
```
Maka saya cukup menggunakan command seperti ini :
```bash
cp menyapa.txt ~/folder_salinan/salinan_menyapa.txt
```
Selanjutnya saya ingin mengecek apakah benar bahwa salinan_menyapa.txt telah tersimpan di direktori folder_salinan serta memiliki isi yang sama dengan file menyapa.txt. Maka saya cukup menggunakan command seperti ini : 
```bash
ls ~/folder_salinan/
salinan_menyapa.txt
```
```bash
cat ~/folder_salinan/salinan_menyapa.txt
HALOO
```
Dari sini dapat disimpulkan bahwa command cp tanpa option bersifat sangat fleksibel karena mampu menduplikasi file baik di direktori yang sama maupun lintas direktori dengan cepat. Selain itu, cp juga menjamin isi file tetap identik sehingga manajemen file di terminal menjadi lebih efisien.

**2. -R/-r atau --recursive** : Digunakan untuk menyalin direktori beserta seluruh isinya secara rekursif, termasuk subfolder dan file di dalamnya hingga tingkat terdalam.

Misalnya saya mempunyai folder project yang memiliki susunan bertingkat seperti ini :
```bash
tree project/
project/
├── config
│   └── config.conf
├── data
│   └── sample.txt
├── docs
│   └── manual.md
├── logs
│   └── app.log
├── README.md
└── src
    ├── main.sh
    └── utils.sh

6 directories, 7 files
```
Kemudian, saya ingin menyalin keseluruhan folder tersebut, ke dalam folder kosong yang bernama backup :
```bash
tree backup/
backup/

0 directories, 0 files
```
Maka saya cukup menggunakan option r/R seperti ini :
```bash
 cp -r project backup/
```
Langkah selanjutnya adalah melakukan pengecekan apakah benar keseluruhan dari folder project telah diduplikasi ke dalam folder backup. Maka saya cukup menggunakan command seperti ini : 
```bash
tree backup
backup
└── project
    ├── config
    │   └── config.conf
    ├── data
    │   └── sample.txt
    ├── docs
    │   └── manual.md
    ├── logs
    │   └── app.log
    ├── README.md
    └── src
        ├── main.sh
        └── utils.sh

7 directories, 7 files
```
Terlihat dengan jelas bahwa keseluruhan folder project benar-benar terduplikasi ke dalam folder backup.

**3. -v atau --verbose** : Digunakan untuk menampilkan informasi file apa saja yang sedang disalin. Sangat berguna untuk monitoring proses copy, terutama saat menyalin banyak file.

Contoh : 
```bash
 cp -v file1.txt salinan_file1.txt
'file1.txt' -> 'salinan_file1.txt'
```

**4. -i atau --interactive** : Meminta konfirmasi sebelum menimpa (overwrite) ketika file tujuan sudah ada.

Misalnya dalam suatu folder terdapat 2 file yang isinya seperti ini :
```bash
ls
file1.txt  file2.txt
```
```bash
 cat file1.txt && cat file2.txt
Ini adalah file pertama
Ini adalah file kedua
```
Selanjutnya, saya akan menyalin file1.txt dengan menetapkan file2.txt sebagai targetnya. Karena file2.txt sudah ada di direktori tersebut, saya menggunakan opsi -i agar cp memberikan peringatan konfirmasi sebelum menimpa (overwrite) file lama. Ini adalah langkah pengamanan penting agar kita tidak sengaja menghapus data yang sudah ada 
```bash
cp -i file1.txt file2.txt
cp: overwrite 'file2.txt'? y
```
```bash
cat file1.txt && cat file2.txt
Ini adalah file pertama
Ini adalah file pertama
```
Dapat terlihat bahwa sistem tidak langsung melakukan eksekusi, melainkan menahan proses untuk meminta persetujuan kita terlebih dahulu. Begitu saya mengetik y (yes), barulah file tersebut ditimpa. Dapat terlihat pula bahwa sekarang isi file2.txt telah berubah total dan menjadi identik dengan file1.txt, yang membuktikan bahwa proses penyalinan sekaligus penimpaan file telah berhasil dilakukan sepenuhnya.

**5. -f atau --force** : Memaksa overwrite tanpa konfirmasi.

Misalnya saya mempunyai 2 file yang isinya :
```bash
cat halo.txt && cat selamat_pagi.txt
haloo
selamat pagii
```
Berbeda dengan opsi sebelumnya, jika saya menggunakan perintah cp -f, sistem akan langsung menimpa file tujuan tanpa memberikan peringatan apa pun, meskipun file tujuan sudah ada.
```bash
cp -f halo.txt selamat_pagi.txt
```
```bash
cat halo.txt && cat selamat_pagi.txt
haloo
haloo
```
Dapat terlihat bahwa proses berjalan secara instan tanpa munculnya pertanyaan konfirmasi. Selain itu, isi file selamat_pagi.txt kini telah digantikan sepenuhnya oleh isi dari halo.txt. Option ini harus digunakan dengan hati-hati karena risiko kehilangan data yang tidak disengaja menjadi lebih tinggi.

**6. -u atau --update** : Menyalin file hanya jika file sumber lebih baru daripada file tujuan, dimana sistem mendeteksi waktu perubahan file berdasarkan mtime( waktu terakhir isi konten suatu file berubah )

Misalnya saya mempunyai 2 file dengan isi dan statistik seperti ini :
```bash
cat file_sumber.txt && cat file_tujuan.txt
Ini adalah file sumber
Ini adalah file tujuan
```
```bash
stat file_sumber.txt
  File: file_sumber.txt
  Size: 23              Blocks: 8          IO Block: 4096   regular file
Device: 8,48    Inode: 32979       Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2026-01-01 20:55:33.478545608 +0700
Modify: 2026-01-01 20:54:18.398626407 +0700
Change: 2026-01-01 20:54:18.398626407 +0700
 Birth: 2026-01-01 20:54:10.106614478 +0700
```
```bash
stat file_tujuan.txt
  File: file_tujuan.txt
  Size: 22              Blocks: 8          IO Block: 4096   regular file
Device: 8,48    Inode: 45616       Links: 1
Access: (0644/-rw-r--r--)  Uid: ( 1000/username)   Gid: ( 1000/username)
Access: 2026-01-01 20:55:33.482545604 +0700
Modify: 2026-01-01 20:54:38.694608262 +0700
Change: 2026-01-01 20:54:38.694608262 +0700
 Birth: 2026-01-01 20:54:28.958621137 +0700
```
Terlihat bahwa file_tujuan.txt memiliki mtime yang lebih baru dari file_sumber.txt. Jika saya menjalankan perintah cp -u, sistem akan membandingkan mtime keduanya. Karena file_tujuan.txt lebih baru, maka proses penyalinan tidak akan dilakukan
```bash
cp -u file_sumber.txt file_tujuan.txt
```
```bash
cat file_sumber.txt && cat file_tujuan.txt
Ini adalah file sumber
Ini adalah file tujuan
```
Tetapi, jika saya mengedit file_sumber.txt, otomatis mtime dari file tersebut menjadi lebih baru daripada file_tujuan.txt sehingga proses penyalinan akan dilakukan :
```bash
cat > file_sumber.txt
Ini adalah file sumberrr
```
```bash
stat -c "%y" file_sumber.txt && stat -c "%y" file_tujuan.txt
2026-01-01 21:04:32.126093601 +0700
2026-01-01 20:54:38.694608262 +0700
```
```bash
cp -u file_sumber.txt file_tujuan.txt
```
```bash
cat file_sumber.txt && cat file_tujuan.txt
Ini adalah file sumberrr
Ini adalah file sumberrr
```
Dapat terlihat bahwa bahwa isi file_tujuan.txt kini telah diperbarui mengikuti isi terbaru dari file sumber.




