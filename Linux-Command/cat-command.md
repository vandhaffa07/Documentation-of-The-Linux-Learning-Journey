# PERINTAH `cat` PADA LINUX

## PENGERTIAN
Perintah `cat` (concatenate) di linux adalah perintah serbaguna untuk menampilkan isi file, membuat file baru, menggabungkan beberapa file, menyalin atau menimpa isi file, serta menambahkan teks ke file melalui terminal

---

## SINTAKS
Sintaks umum perintah `cat` adalah :
```bash
cat [OPTION] [NAMA_FILE]
```
---

## DASAR PENGGUNAAN

**1. Menampilkan isi file**

Misalnya saya mempunyai susunan folder seperti ini
```bash
ALPHABET/
├── A.txt  → "A adalah huruf pertama"
└── B.txt  → "B adalah huruf kedua"
```
Kemudian saya ingin menampilkan isi dari file A.txt :
```bash
cat A.txt
A adalah huruf pertama
```
**2. Menggabungkan dan menampilkan isi beberapa file**

(Masih menggunakan folder yang sama dengan penjelasan sebelumnya)
```bash
cat A.txt B.txt
A adalah huruf pertama
B adalah huruf kedua
```
**3. Membuat file baru lewat input keyboard di terminal**

Misalnya saya ingin membuat file baru tanpa menggunakan text editor seperti nano, maka saya bisa menggunakan command cat sebagai alternatifnya, contoh :
```bash
cat > sapaan.txt
HALO SEMUANYA (ini adalah isinya, jika sudah selesai tekan CTRL+D untuk menutupnya)
```
Kemudian saya ingin menampilkan isi file baru yang telah dibuat :
```bash
cat sapaan.txt
HALO SEMUANYA
```
**⚠️ Peringatan**  : Jangan gunakan sintaks command ini untuk file penting yang sudah ada isinya, karena command ini sifatnya menimpa teks file yang sudah ada.

**4. Menambahkan teks ke file tanpa menimpa lewat input keyboard**

Masih dengan file sapaan.txt, dimana saya ingin menambahkan sapaan lainnya pasa file tersebut :
```bash
cat >> file_nama
SELAMAT PAGI ( ini adalah teks yang saya tambahkan )
```
Kemudian saya ingin menampilkan isi file sapaan.txt yang telah diedit :
```bash
cat sapaan.txt
HALO SEMUANYASELAMAT PAGI
```
---
## OPTION
**1. -n atau Number all lines** : Digunakan untuk menomori semua baris

Misalnya saya mempunyai file bernama nama_hewan.txt yang isinya :
```bash
cat nama_hewan.txt
harimau
kucing

anjing
singa


buaya
```
Kemudian saya ingin menomori semua baris pada file tersebut menggunakan option n :
```bash
cat -n nama_hewan.txt
     1  harimau
     2  kucing
     3
     4  anjing
     5  singa
     6
     7
     8  buaya
```

**2. -b atau Blank Numbering** : Digunakan hanya untuk menomori baris yang tidak kosong

Masih dengan file bernama nama_hewan.txt yang sama dengan sebelumnya, ketika menggunakan option b, maka output akan menjadi :
```bash
 cat -b nama_hewan.txt
     1  harimau
     2  kucing

     3  anjing
     4  singa


     5  buaya
```

**3. -s atau Squeeze** : Digunakan untuk menghapus banyak baris kosong menjadi satu

Masih menggunakan file bernama nama_hewan.txt seperti sebelum-sebelumnya. Ketika menggunakan option s, maka output akan menjadi :
```bash
cat -s nama_hewan.txt
harimau
kucing

anjing
singa

buaya
```
Dapat dilihat bahwa string buaya yang sekarang hanya memliki jarak 1 baris kosong dengan string diatasnya, padahal sebelumnya berjarak 2 baris kosong

**4. -E atau Endline** : Digunakan untuk menampilkan tanda `$` di akhir setiap baris, berguna untuk melihat akhir baris serta membantu kita untuk mendeteksi trailing whitespace yang seringkali tidak terlihat tetapi dapat menyebabkan masalah dalam pemrograman.

Contoh :

Disini saya masih menggunakan file yang bernama nama_hewan.txt seperti sebelum sebelumnya
```bash
cat -E nama_hewan.txt
harimau$
kucing$
$
anjing$
singa$
$
$
buaya
```

**5. -T atau --show-tabs** : Digunakan untuk menampilkan karakter Tab. Di terminal, Spasi dan Tab terlihat sama-sama kosong. Opsi ini akan mengubah Tab menjadi simbol `^I` agar kita bisa membedakannya. Ini sangat penting untuk debugging file konfigurasi atau kode pemrograman (seperti Python) yang sensitif terhadap indentasi.

Contoh:

```bash
cat -T kode.py
^Iprint("Halo Dunia")
```

### MEMAHAMI ESCAPE SEQUENCE DAN CARET NOTATION
Sebelum masuk ke Option berikutnya yakni -v dan -A, kita harus tahu bahwa komputer mempunyai "Karakter hantu". Ini adalah karakter yang sebenarnya ada tapi tidak terlihat mata, seperti tombol Enter atau Tab.

**1. Caret Notation** : Caret notation adalah cara sederhana untuk merepresentasikan karakter kontrol (tombol perintah tersembunyi) menggunakan simbol "topi" atau caret (^) yang diikuti oleh sebuah huruf alfabet. Kita membutuhkannya karena komputer memiliki karakter yang tidak bisa "dicetak" atau tidak terlihat secara visual, seperti perintah untuk menghentikan proses, berpindah baris, atau menghapus karakter. Karena karakter ini tidak punya bentuk gambar, kita butuh cara untuk menuliskannya agar manusia bisa membacanya.

Misalnya, Jika saya melihat ^M pada sebuah file, itu artinya komputer sedang menjalankan Ctrl + M (ini sama saja dengan tombol Enter di kehidupan kita).

**2. Escape Sequence** : Jika Caret Notation adalah cara kita melihat karakter kontrol, maka Escape Sequence adalah cara kita mengetikkan atau memerintahkan karakter kontrol tersebut di dalam kode program atau teks. Escape Sequence diawali dengan simbol khusus (biasanya backslash \), Sederhananya ini adalah cara kita memberi tahu komputer bahwa jangan menganggap karakter setelah backslash adalah karakter biasa.

Misalnya, jika saya ingin menulis kalimat di dalam tanda kutip: "Dia berkata, "Halo" kepada saya". Komputer akan bingung karena ada tanda kutip di dalam tanda kutip. Komputer akan mengira bahwa kalimat tersebut selesai di kata "berkata,". Di sinilah Fungsi Escape Sequence, kita bisa merubahnya menjadi : "Dia berkata, \"Halo\" kepada saya". Tanda \ memberitahu komputer bahwa "Jangan baca tanda kutip setelah ini sebagai penutup kalimat, tapi anggap sebagai tulisan tanda kutip biasa!"

