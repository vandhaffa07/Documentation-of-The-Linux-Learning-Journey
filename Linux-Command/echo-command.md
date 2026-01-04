# PERINTAH `echo` PADA LINUX

## PENGERTIAN
`echo` adalah salah satu perintah dasar di Linux yang digunakan untuk menampilkan teks, variabel, atau string ke terminal(standard output). Selain itu, `echo` juga bisa digunakan untuk mengirim output ke file atau menyalurkan output ke perintah lain menggunakan pipe (`|`). Dengan kata lain, `echo` berfungsi sebagai cara paling sederhana untuk melihat hasil teks atau nilai variabel secara langsung di terminal. 

---

## SINTAKS DASAR
Sintaks umum perintah `echo` adalah : 
```bash
echo [OPTION] [STRING]
```
---

## DASAR PENGGUNAAN PERINTAH `echo` :
**1. Menampilkan teks ke terminal (Tanpa Option)**
```bash
echo "Selamat Belajar"
Selamat Belajar
```
**2. Menampilkan environment variable(Nilai dinamis yang tersimpan di dalam sistem operasi dan dapat diakses oleh berbagai proses atau aplikasi yang sedang berjalan)**

Kita bisa melihat apa saja environment variable yang terdapat pada sistem kita dengan menggunakan command "env" : 
```bash
env
```
Setelah dijalankan nanti akan muncul berbagai variabel seperti ini :
```bash
SHELL=/bin/bash
WSL2_GUI_APPS_ENABLED=1
WSL_DISTRO_NAME=kali-linux
NAME=LAPTOP-BVIKQQAI
PWD=/home/vandhaffa
LOGNAME=vandhaffa
HOME=/home/vandhaffa
..........dst
```
Selanjutnya saya akan menampilkan beberapa environment variable menggunakan command echo :
```bash
echo $SHELL
/bin/bash
```
```bash
echo $HOME
/home/vandhaffa
```
```bash
echo $NAME
LAPTOP-BVIKQQAI
```

**3. Menampilkan variabel sementara yang kita buat sendiri**
```bash
nama="vandhaffa"
```
```bash
echo "nama saya adalah $nama"
nama saya adalah vandhaffa
```
⚠️Catatan : Variabel seperti ini bersifat sementara dan hanya berlaku di session saat ini, ketika membuka terminal baru maka variabel tersebut akan hilang

**4. Mengirim output ke file**

Misalnya disini saya membuat file bernama menyapa.txt :
```bash
cat > menyapa.txt
HALO
```
```bash
cat menyapa.txt
HALO
```
Kemudian saya ingin menimpa file tersebut menggunakan echo :
```bash
echo "HAI" > menyapa.txt
```
```bash
cat menyapa.txt
HAI
```
Dapat dilihat bahwa isi file menyapa.txt benar benar tertimpa oleh output echo. Jika kita tidak ingin menimpa sebuah, melainkan hanya menambahkan teks baru pada file tersebut, maka kita bisa menggunakan sintaks seperti ini : 
```bash
echo "SELAMAT PAGI" >> menyapa.txt
```
```bash
cat menyapa.txt
HAI
SELAMAT PAGI
```

---

## OPTION
**1. -n** : Digunakan untuk menampilkan teks tanpa membuat baris baru di akhir output.

Contoh :

Tanpa -n :
```bash
echo "Hello"; echo " World"
Hello
 World
```
Dengan -n : 
```bash
echo -n "Hello"; echo " World"
Hello World
```
**2. -e** : Digunakan untuk mengaktifkan interpretasi escape sequence dalam string. Escape sequence adalah karakter khusus yang memengaruhi tampilan teks, seperti tab, newline, dan lainnya.

📚[Catatan Mengenai Konsep Escape Sequence](https://github.com/vandhaffa07/Documentation-of-The-Linux-Learning-Journey/blob/main/Linux-Command/cat-command.md#memahami-escape-sequence-dan-caret-notation)

📚[Daftar Karakter Non-printable (Escape Sequence dan Caret Notation)](https://github.com/vandhaffa07/Documentation-of-The-Linux-Learning-Journey/blob/main/Linux-Command/cat-command.md#daftar-karakter-non-printable--caret-notation-dan-escape-sequence-)

Contoh : 
```bash
echo -e "Nama\t: vandhaffa\nNegara\t: Indonesia"
Nama    : vandhaffa
Negara  : Indonesia
```

---

## PENERAPAN ECHO DALAM SKRIP BASH
```bash
nano skrip_bash.sh
```
Kemudian didalam nano saya membuat skrip ini :
```bash
#!/bin/bash

# Membuat variabel
nama="vandhaffa"
hobi="Belajar"

# Menampilkan teks sederhana
echo "Halo, nama saya $nama!"

# Menampilkan teks tanpa baris baru di akhir
echo -n "Hobi saya adalah: "
echo "$hobi"

# Menggunakan escape sequence (-e)
echo -e "Ini baris ketiga\nIni baris keempat\tdengan tab"

# Menyimpan output ke file
echo "Skrip ini dijalankan oleh $nama dan hobinya $hobi" > output.txt

# Menampilkan isi file
echo "Isi file output.txt:"
cat output.txt
```
Kemudian saya jalankan skrip bash tersebut :
```bash
bash skrip_bash.sh
Halo, nama saya vandhaffa!
Hobi saya adalah: Belajar
Ini baris ketiga
Ini baris keempat     dengan tab
Isi file output.txt:
Skrip ini dijalankan oleh vandhaffa dan hobinya Belajar
```


