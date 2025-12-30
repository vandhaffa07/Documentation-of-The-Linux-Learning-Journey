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

**2. -b (Blank Numbering)** : Digunakan hanya untuk menomori baris yang tidak kosong

Masih dengan file bernama nama_hewan.txt yang sama dengan sebelumnya, ketika menggunakan option b, maka output akan menjadi :
```bash
 cat -b nama_hewan.txt
     1  harimau
     2  kucing

     3  anjing
     4  singa


     5  buaya
```

**3. -s (Squeeze)** : Digunakan untuk menghapus banyak baris kosong menjadi satu

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

