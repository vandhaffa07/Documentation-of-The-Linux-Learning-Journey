# PERINTAH `file` PADA LINUX

## PENGERTIAN
Perintah `file` pada Linux digunakan untuk mengidentifikasi tipe sebenarnya dari sebuah file. Perintah ini tidak bergantung pada ekstensi file (seperti .txt atau .jpg), melainkan menganalisis isi file secara langsung. Perintah ini bekerja dengan cara membaca magic number atau file signature pada byte awal sebuah file. Data ini kemudian dicocokkan dengan database magic number yang terdapat pada file magic.mgc. magic.mgc adalah file yang telah dikompilasi ke dalam bahasa mesin agar proses identifikasi file berjalan sangat cepat. Jika kita mencoba membukanya secara manual, kita tidak akan bisa membacanya karena isinya adalah kode biner (binary), bukan teks yang bisa dibaca manusia (human readable).

---

## SINTAKS
```bash
file [OPTION] [FILE]
```
---

## OPTION

**1. No option** : Tanpa menambahkan opsi apa pun, perintah file secara default akan melakukan serangkaian pengujian terhadap konten file (seperti magic numbers) untuk menentukan tipe atau format aslinya, lalu menampilkannya di layar.

Sebagai contoh, saya akan menyimulasikan bagaimana perintah file bekerja pada suatu file teks walaupun file tersebut tidak memiliki ekstensi .txt. 

Mula-mula saya akan menampilkan isi file bernama "contoh" yang tidak memiliki ekstensi apapun menggunakan command cat seperti ini : 
```bash
cat contoh
Example
```
Dapat terlihat bahwa file tersebut berisi teks biasa, namun secara sistem kita belum tahu apakah ini benar-benar file teks murni atau file biner yang kebetulan bisa dibaca.

Selanjutnya, saya akan melakukan pengecekan untuk mengetahui identitas asli file tersebut menggunakan command file seperti ini:
```bash
file contoh
contoh: ASCII text
```
Dapat terlihat bahwa meskipun nama filenya polos tanpa .txt, sistem tetap mampu mengenali bahwa file tersebut adalah dokumen teks biasa (ASCII text). Hasil ini membuktikan bahwa perintah file tidak bergantung pada nama atau ekstensi file, melainkan pada analisis byte di dalamnya. Sistem mencocokkan signature file tersebut dengan database di magic.mgc sehingga identitas aslinya terungkap secara akurat.

**2. -b atau --brief** : Opsi ini digunakan untuk menampilkan hasil identifikasi tipe file secara singkat (brief). Jika pada penggunaan biasa (No Option) perintah file akan menampilkan nama file diikuti oleh tipenya, maka dengan opsi -b, nama file tersebut akan dihilangkan dari output sehingga hanya menyisakan informasi tipe filenya saja.

Sebagai contoh, saya akan menggunakan perintah `file -b` pada file yang saya gunakan pada penjelasan No Option sebelumnya, yakni file yang bernama "contoh" seperti ini:
```bash
file -b contoh
ASCII text
```
Dapat terlihat bahwa nama file sudah menghilang dari layar. Output yang dihasilkan menjadi jauh lebih bersih dan hanya menyisakan informasi teknis mengenai tipe file tersebut. 

**3. -c atau --checking-printout** : Opsi ini digunakan untuk melakukan pengecekan apakah file konfigurasi/database magic seperti magic.mgc sudah benar dan tidak ada kesalahan format di dalamnya. Singkatnya, opsi ini digunakan untuk proses debugging database magic. Alih-alih mengidentifikasi file milik kita, sistem justru akan membedah aturan internalnya sendiri untuk memastikan tidak ada kerusakan data. 

Saat dijalankan pada sistem yang sehat, perintah ini akan menampilkan daftar aturan teknis dengan header kolom seperti ini :
```bash
file -c
cont  offset  type  opcode  mask  value  desc
```
Dapat terlihat bahwa sistem menampilkan header tabel interpretasi dari database magic. Ini menandakan semua aturan deteksi file masih terbaca dengan benar oleh sistem.

Akan tetapi, jika database magic.mgc mengalami kerusakan (misalnya ada baris kode yang hilang, korup, atau salah tulis), maka saat perintah ini dijalankan, shell tidak akan menampilkan output header tabel seperti di atas. Melainkan akan mengeluarkan pesan error yang menunjukkan lokasi atau baris mana yang sedang bermasalah. Akibatnya, jika file -c mengeluarkan output error, maka command file tidak akan bisa berjalan dengan lancar. Perintah file mungkin akan memberikan hasil identifikasi yang salah, atau bahkan tidak bisa mengenali tipe file apa pun sama sekali karena database intinya sedang rusak.


**4. -i atau --mime** : Opsi ini digunakan untuk menampilkan tipe file dalam format MIME (Multipurpose Internet Mail Extensions). Jika biasanya perintah file memberikan deskripsi yang panjang dan mudah dibaca manusia (seperti "ASCII text"), opsi -i akan mengubahnya menjadi format standar yang bisa dipahami oleh komputer, web server, dan protokol internet (seperti text/plain).

Sebagai contoh, saya akan membandingkan output yang dikeluarkan command file ketika digunakan tanpa opsi tambahan (standar) dengan saat menggunakan opsi -i pada file foto saya yang bernama bunga-sakura.jpeg.

Mula-mula, saya akan jalankan perintah file tanpa opsi tambahan untuk melihat detail teknis yang lengkap namun panjang seperti ini : 
```bash
file bunga-sakura.jpeg
bunga-sakura.jpeg: JPEG image data, JFIF standard 1.02, resolution (DPI), density 72x72, segment length 16, progressive, precision 8, 2736x3648, components 3
```
Dapat terlihat bahwa outputnya sangat mendalam, mencakup resolusi hingga densitas warna. Informasi ini bagus untuk manusia, tapi sulit diproses oleh skrip atau web server secara cepat.

Selanjutnya saya akan menggunakan file dengan tambahan opsi -i untuk mendapatkan identitas file dalam format standar internet seperti ini :
```bash
file bunga-sakura.jpeg
bunga-sakura.jpeg: JPEG image data, JFIF standard 1.02, resolution (DPI), density 72x72, segment length 16, progressive, precision 8, 2736x3648, components 3
```
Dapat terlihat pula bahwa outputnya menjadi jauh lebih ringkas, yaitu image/jpeg. Selain itu, sistem menyertakan informasi charset=binary yang memberitahu bahwa file ini bukan berisi teks, melainkan data biner.

**5. -L atau --dereference** : Opsi ini digunakan agar perintah file mengikuti Symbolic Link yang menuju ke file aslinya. Secara default, jika Anda menjalankan perintah file pada sebuah Symbolic Link, sistem hanya akan melaporkan bahwa itu adalah sebuah link. Namun, dengan opsi -L, perintah file akan menembus link tersebut dan mengidentifikasi tipe file asli yang dituju.

Mula-mula, saya akan membuat sebuah symbolic link bernama shortcut_file_kosong_rahasia yang mengarah ke sebuah file tersembunyi jauh di dalam folder arsip seperti ini :
```bash
ln -s /home/vandhaffa/arsip/.rahasia/file_kosong_rahasia shortcut_file_kosong_rahasia
```
Selanjutnya mari kita pastikan bahwa link tersebut sudah terbentuk dan melihat ke mana arah tujuannya menggunakan command ls seperti ini :
```bash
ls -l shortcut_file_kosong_rahasia
lrwxrwxrwx 1 vandhaffa vandhaffa 50 Jan  5 15:43 shortcut_file_kosong_rahasia -> /home/vandhaffa/arsip/.rahasia/file_kosong_rahasia
```
Dapat terlihat bahwa file tersebut memang sebuah link (ditandai dengan huruf l di awal izin akses dan tanda panah -> yang menunjuk ke path file aslinya).

Jika kita mengecek symlink tersebut menggunakan perintah file biasa, sistem hanya akan membaca identitas luarnya saja sebagai symlink seperti ini :
```bash
file shortcut_file_kosong_rahasia
shortcut_file_kosong_rahasia: symbolic link to /home/vandhaffa/arsip/.rahasia/file_kosong_rahasia
```
Dapat terlihat bahwa sistem tidak memberitahu kita isi dari file aslinya, melainkan hanya memberitahu ke mana tautan ini mengarah.

Akan tetapi, jika kita menggunakan opsi -L setelah perintah file, maka akan sistem mengikuti link tersebut dan memeriksa file aslinya seperti ini :
```bash
file -L shortcut_file_kosong_rahasia
shortcut_file_kosong_rahasia: empty
```
Dapat terlihat bahwa hasil outputnya berubah menjadi empty. Hal ini dikarenakan file asli yang dituju (file_kosong_rahasia) memang merupakan sebuah file kosong. Ini berarti opsi -L berhasil menunjukkan tipe konten asli di balik shortcut tersebut.

**6. -k atau --keep-going** : Secara default, perintah file akan berhenti mencari tipe file segera setelah ia menemukan kecocokan pertama yang paling kuat dalam database magic. Namun, dengan opsi -k, perintah file tidak akan langsung berhenti. Ia akan terus memeriksa seluruh database untuk menemukan kemungkinan tipe atau format lain yang juga cocok dengan isi file tersebut.

Sebagai contoh, saya akan menyimulasikan bagaimana sebuah file bisa memiliki lebih dari satu identitas. Kita akan membuat sebuah file yang memiliki karakteristik ganda, yaitu sebagai skrip shell sekaligus sebagai teks biasa.

Mula-mula, saya akan membuat file bernama identitas_ganda.sh dan kita isi dengan shebang (penanda skrip) di baris pertama, diikuti dengan kalimat teks biasa di baris kedua seperti ini :
```bash
echo '#!/bin/bash' > identitas_ganda.sh && echo "Ini adalah teks biasa tapi punya identitas skrip." >> identitas_ganda.sh
```
Mari kita lihat isi file tersebut untuk memastikan struktur barisnya sudah benar menggunakan perintah cat seperti ini :
```bash
cat identitas_ganda.sh
#!/bin/bash
Ini adalah teks biasa tapi punya identitas skrip.
```
Dapat terlihat bahwa baris pertama mengandung identitas skrip Bash, sedangkan baris kedua adalah teks manusia biasa.

Selanjutnya saya akan menggunakan commnad file biasa pada file tersebut, maka sistem akan langsung berhenti pada hasil pertama yang dianggap paling valid (yaitu skrip Bash) seperti ini :
```bash
file identitas_ganda.sh
identitas_ganda.sh: Bourne-Again shell script, ASCII text executable
```
Dapat terlihat bahwa sistem hanya memberikan satu kesimpulan utama. Ia menganggap file ini adalah skrip shell dan tidak mencari identitas lainnya. Kalimat "ASCII text executable" di belakang koma hanyalah keterangan tambahan dari aturan skrip tersebut. Ibaratnya, sistem bilang: "Ini adalah satu benda: Skrip Bash yang tipenya teks."

Selanjutnya, mari kita gunakan opsi -k untuk melihat semua identitas yang terdeteksi oleh database magic seperti ini :
```bash
file -k identitas_ganda.sh
identitas_ganda.sh: Bourne-Again shell script text executable\012- a /bin/bash script, ASCII text executable
```
Dapat terlihat bahwa output-nya berubah. Sistem tidak lagi berhenti di satu hasil. \012 adalah representasi karakter newline (LF) dalam format oktal. Ia adalah pemisah yang menunjukkan bahwa ada identitas kedua yang ditemukan, yaitu sebagai /bin/bash(yang kembali menegaskan bahwa file ini adalah script yang dijalankan oleh /bin/bash) script dan ASCII text.

**7. -m atau --magic-file** : Secara default, perintah file menggunakan database standar sistem (biasanya di /usr/share/misc/magic.mgc) untuk mengenali tipe file. Namun, dengan opsi -m, Anda bisa memaksa perintah file untuk menggunakan database kustom milik Anda sendiri. Ini sangat berguna jika Anda ingin mendeteksi format file khusus yang belum dikenali oleh Linux secara umum.

Sebagai contoh, kita akan membuat sebuah aturan untuk mengenali file buatan kita sendiri (format .vandhaffa) yang diawali dengan kode biner khusus.

Mula-mula, Kita buat file bernama mymagic. Isinya adalah aturan: "Jika di offset 0 (awal file) terdapat string biner \x01\x02\x03\x04, maka identifikasi file tersebut sebagai vandhaffa_file". Maka saya cukup menggunakan perintah seperti ini :
```bash
echo "0 string \x01\x02\x03\x04 vandhaffa_file" > mymagic
```
```bash
cat mymagic
0 string \x01\x02\x03\x04 vandhaffa_file
```
Dapat terlihat bahwa kita baru saja membuat sebuah "otak" atau referensi baru bagi perintah file untuk mengenali sebuah format.

Selanjutnya, kita buat file dengan ekstensi khusus bernama vandhaffa.vandhaffa yang berisi teks biasa "TEST" seperti ini : 
```bash
cat > vandhaffa.vandhaffa
TEST
```
```bash
cat vandhaffa.vandhaffa
TEST
```
Dapat terlihat bahwa file ini sekarang hanyalah file teks biasa tanpa kode biner apa pun di dalamnya.

Jika kita cek sekarang, baik menggunakan database standar maupun database kustom(mymagic), sistem tetap menganggapnya sebagai teks biasa karena kode biner 01 02 03 04 belum kita masukkan ke dalam file tersebut. Seperti ini outputnya :
```bash
file vandhaffa.vandhaffa
vandhaffa.vandhaffa: ASCII text
```
```bash
file -m mymagic vandhaffa.vandhaffa
vandhaffa.vandhaffa: ASCII text
```
Selanjutnya, Mari kita gunakan tools bernama hexedit untuk memasukkan kode biner 01 02 03 04 ke baris paling awal file vandhaffa.vandhaffa :
```bash
hexedit vandhaffa.vandhaffa
```
Setelah memasuki antarmuka hexedit, ubah byte awal file yang awalnya seperti ini :

<img width="1893" height="952" alt="image" src="https://github.com/user-attachments/assets/b5f08ce1-6083-4c8a-9d15-5107a7457dc3" />

Menjadi seperti ini :

<img width="1890" height="947" alt="image" src="https://github.com/user-attachments/assets/72247ee0-3ada-4ce0-8225-63e9d2825c66" />

Langkah terakhir, kita tinggal membandingkan hasil pengecekan file vandhaffa.vandhaffa dengan menggunakan database standar serta database kustom seperti ini :
```bash
file vandhaffa.vandhaffa
vandhaffa.vandhaffa: data
```
```bash
file -m mymagic vandhaffa.vandhaffa
vandhaffa.vandhaffa: vandhaffa_file
```
Dapat terlihat bahwa database standar kebingungan karena ada kode biner tak dikenal, sehingga ia hanya menyebutnya sebagai "data" mentah. Dapat terlihat pula bahwa saat menggunakan opsi -m, perintah file berhasil mendeteksi identitas kustom yang kita buat karena ia menggunakan referensi dari file mymagic. 

**8. -z atau --uncompress** :





