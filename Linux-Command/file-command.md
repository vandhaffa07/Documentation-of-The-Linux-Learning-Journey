# PERINTAH `file` PADA LINUX

## PENGERTIAN
Perintah `file` pada linux digunakan untuk mengidentifikasi tipe sebenarnya dari sebuah file, bukan dari ekstesensi nya, melainkan dengan menganalisis isi file secara langsung. Perintah ini bekerja dengan cara membaca magic number/signature file pada byte awal file kemudian mencocokkannya dengan database magic number yang terdapat pada file magic.mgc( magic.mgc adalah file yang telah di compile untuk bahasa mesin sehingga ketika kita membukanya, kita tidak bisa membacanya karena isinya adalah binary bukan human readeable )

---

## SINTAKS
```bash
file [OPTION] [FILE]
```
---

## OPTION

**1. No option** : Tanpa menambahkan opsi apapun, file secara default hanya akan menampilkan tipe atau format dari sebuah file

Sebagai contoh, saya akan menyimulasikan bagaimana perintah file bekerja pada suatu file teks walaupun file tersebut tidak memiliki ekstensi .txt. 

Mula-mula saya akan menampilkan isi file bernama "contoh" yang tidak memiliki ekstensi apapun menggunakan command cat seperti ini : 
```bash
cat contoh
Example
```
Selanjutnya, saya akan melakukan pengecekan untuk mengetahui identitas asli file tersebut menggunakan command file seperti ini:
```bash
file contoh
contoh.txt: ASCII text
```
Dapat terlihat bahwa meskipun nama filenya polos tanpa .txt, sistem tetap mampu mengenali bahwa file tersebut adalah dokumen teks biasa (ASCII text). Hal ini membuktikan bahwa command file tidak bergantung pada nama file, melainkan pada isinya.




