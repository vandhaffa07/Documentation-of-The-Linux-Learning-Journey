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
---

## MEMAHAMI ESCAPE SEQUENCE DAN CARET NOTATION
Sebelum masuk ke Option berikutnya yakni -v dan -A, kita harus tahu bahwa komputer mempunyai "Karakter hantu". Ini adalah karakter yang sebenarnya ada tapi tidak terlihat mata, seperti tombol Enter atau Tab.

**1. Caret Notation**

Caret Notation adalah cara lama untuk menuliskan "perintah kontrol" menggunakan tanda (^) yang kemudian diikuti oleh satu huruf kapital. Bisa dibilang ini adalah cara komputer menampilkan atau melaporkan kepada kita apa yang sedang terjadi di sistemnya, terutama untuk karakter yang tidak bisa diprint (tidak terlihat). Tujuannya sederhana, agar manusia bisa melihat karakter yang sebenarnya tidak punya bentuk visual (seperti tab atau Enter).

Contoh: 

Disini saya akan melakukan percobaan dengan membuka file /bin/ls yang merupakan file binary atau instruksi mesin pada sistem Linux
```bash
cat /bin/ls
```
Maka layar akan penuh dengan karakter acak serta terdapat banyak sekali Caret Notation, hal ini dikarenakan command cat mencoba membaca isi file sebagai huruf yang dipahami manusia, sedangkan file binary sepenuhnya telah dicompile dan ditulis dengan bahasa mesin yang tidak dapat dipahami manusia. seperti ini contoh outputnya :
```bash
^@^@^A^Y^@^H^Z^P^G ...
```



**2.  Escape Sequence**
Jika Caret Notation lebih ke arah "output" visual untuk manusia, maka escape sequence lebih ke arah "input" perintah dari manusia. Escape Sequence adalah cara kita menuliskan karakter khusus di dalam kode program supaya komputer tidak bingung. Biasanya diawali dengan tanda garis miring terbalik atau backslash (\). Ini adalah cara kita memberikan instruksi spesifik kepada komputer saat kita sedang menulis kode atau perintah.

Contoh:

Disini saya akan membuat program C++. Daripada menulis kode seperti ini :
```bash
cout<<"Halo semuanya"<<endl;
cout<<"Selamat pagi"<<endl;
```
Dengan Escape Sequence, kita bisa meringkas kode tersebut agar lebih efisien :
```bash
cout<<"Halo semuanya\nSelamat pagi"<<endl;
```
**Pertanyaan 1 : Mengapa pada text editor seperti nano atau notepad kita tidak perlu repot-repot menggunakan Escape Sequence untuk mengedit isi konten sebuah file?**

Jawabannya adalah karena aplikasi tersebut memiliki Penerjemah (Interpreter) yang bekerja secara real-time. Editor teks adalah sebuah program yang cukup canggih, dimana dia mendengarkan keyboard kita setiap kali kita klik. Misalnya saat kita menekan tombol enter, keyboard akan mengirim kode angka (13 dalam kode ASCII) kepada text editor kita, setelah diterima oleh text editor kita, ia akan menerjemahkannya: "Oh, dalam ASCII nomer 13 adalah perintah \n (Escape Sequence).", ia kemudian memperbarui tampilan di monitor kita dengan memindahkan teks ke baris bawah.

**Pertanyaan 2 : Mengapa saat menggunakan tools yang dapat melihat karakter non-printable ( dalam kasus ini yang saya maksud adalah command cat dengan option tertentu ) semua karakter non-printable dari file yang diedit nano tidak terlihat, akan tetapi ketika menggunakan echo -e untuk memasukkan string ke suatu file, karakter non-printable tetap terlihat?**

Jawabannya adalah karena nano bukanlah sebuah alat perekam, melainkan alat yang bekerja secara real-time, saat nano menerima sinyal dari tombol pada keyboard kita, maka ia akan menangkap sinyal tersebut dan langsung menghapus karakter non-printable di memorinya setelah memberikan output yang kita inginkan. Nano tidak pernah berniat memasukkan karakter-karakter non-printable ke dalam file, dia hanya ingin menyimpan hasil bersih tulisan akhir kita. Berbeda dengan saat Menggunakan echo -e.
echo -e bukanlah sebuah editor melainkan pengirim pesan. Misal, ketika kita mengetikkan echo -e "Halo\bA", sebenarnya kita sedang memerintahkan sistem : "Tolong tuliskan huruf H, a, l, o, lalu masukkan karakter Backspace (non-printable), lalu huruf A ke dalam file.". Karena echo tidak punya kecerdasan untuk "merapikan" tulisan seperti nano, dia menuruti perintah kita dan memasukkan karakter non-printable sebagai data mentah.

---

## DAFTAR KARAKTER NON-PRINTABLE ( CARET NOTATION DAN ESCAPE SEQUENCE )

### 1. Newline (Baris Baru)
* **Escape Sequence:** `\n`
* **Caret Notation:** `^J` 
* **Kegunaan:** Memindahkan kursor ke baris bawahnya. Di Linux, ini adalah standar "Enter".

### 2. Tab (Jarak Horizontal)
* **Escape Sequence:** `\t`
* **Caret Notation:** `^I`
* **Kegunaan:** Memberikan jarak lebar (indentasi). Cocok untuk merapikan kolom teks.

### 3. Backspace (Hapus Kiri)
* **Escape Sequence:** `\b`
* **Caret Notation:** `^H`
* **Kegunaan:** Mundur satu langkah dan menghapus karakter sebelumnya.

### 4. Bell (Suara Peringatan)
* **Escape Sequence:** `\a`
* **Caret Notation:** `^G`
* **Kegunaan:** Memerintahkan komputer mengeluarkan suara "Beep" atau kedipan layar.

### 5. Carriage Return
* **Escape Sequence:** `\r`
* **Caret Notation:** `^M`
* **Kegunaan:** Mengembalikan kursor ke awal baris. 

### 6. Null Character
* **Escape Sequence:** `\0`
* **Caret Notation:** `^@`
* **Kegunaan:** Menandakan akhir dari sebuah data atau string dalam bahasa tingkat rendah seperti C.

### 7. Escape Key
* **Escape Sequence:** `\e` (atau `\x1b`)
* **Caret Notation:** `^[`
* **Kegunaan:** Memulai perintah kontrol, seperti mewarnai teks di terminal atau perintah navigasi di editor Vim.
  
---

## LANJUTAN OPTION
**6. -v atau Show Non-printing** : Digunakan untuk menampilkan karakter yang non-printable (karakter yang tidak bisa dicetak atau tidak terlihat secara visual seperti Caret Notation dan Escape Sequence yang ada pada catatan diatas) kecuali karakter Tab dan Newline.

Misalnya saya memasukkan sebuah string kedalam file baru bernama tes_karakter.txt seperti ini :
```bash
echo -e "TAB_di_sini:\t[Selesai]\nBACKSPACE_test: ABC\b\b\b[Hapus]\nCARRIAGE_RETURN: Teks_Lama\r[Timpa]\nBELL_bunyi: ^G\a\nESCAPE_warna: \e[31mTeks_Merah\e[0m" > tes_karakter.txt
```
Selanjutnya saya akan membandingkan perbedaan penggunaan command cat tanpa option dengan command cat yang disertai option v pada file tersebut :
```bash
cat tes_karakter.txt
TAB_di_sini:  [Selesai]
BACKSPACE_test: [Hapus]
[Timpa]E_RETURN: Teks_Lama
BELL_bunyi: ^G
ESCAPE_warna: Teks_Merah ( Pada terminal saya teks ini berwarna merah )
```
```bash
cat -v tes_karakter.txt
TAB_di_sini:  [Selesai]
BACKSPACE_test: ABC^H^H^H[Hapus]
CARRIAGE_RETURN: Teks_Lama^M[Timpa]
BELL_bunyi: ^G^G
ESCAPE_warna: ^[[31mTeks_Merah^[[0m
```
**7. -A atau Show All** : Opsi ini adalah paket lengkap yang menggabungkan beberapa fungsi sekaligus. Menggunakan -A sama saja dengan menjalankan perintah -vET.

Contoh( Disini saya masih menggunakan file tes_karakter.txt seperti pada penjelasan sebelumnya untuk melakukan percobaan ) :
```bash
cat -A tes_karakter.txt
TAB_di_sini:^I[Selesai]$
BACKSPACE_test: ABC^H^H^H[Hapus]$
CARRIAGE_RETURN: Teks_Lama^M[Timpa]$
BELL_bunyi: ^G^G$
ESCAPE_warna: ^[[31mTeks_Merah^[[0m$
```

