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

Sebagai contoh, saya akan menyimulai
```bash
cat contoh.txt
Example
```
```bash
file contoh.txt
contoh.txt: ASCII text
```





