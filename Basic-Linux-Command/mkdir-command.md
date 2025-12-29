# PERINTAH `mkdir` PADA LINUX

## PENGERTIAN
Perintah `mkdir` (*make directory*) digunakan di Linux untuk membuat direktori (folder) baru. Perintah ini sangat penting dalam pengelolaan filesystem karena hampir semua struktur direktori dibangun menggunakan `mkdir`.

---

## SINTAKS
Sintaks umum perintah `mkdir` adalah:
```bash
mkdir [option] nama_direktori
```
---

## OPTION
**1. -p atau --parents** : Opsi ini digunakan untuk membuat direktori beserta subdirektori di dalamnya, meskipun direktori induknya belum ada. Jika direktori induk belum ada, maka mkdir akan membuatnya secara otomatis.

Contoh:
```bash
mkdir -p belajar/alphabet/huruf_a
```
Tanpa opsi -p, perintah di atas akan menghasilkan error jika direktori belajar belum ada
```bash
mkdir: cannot create directory ‘belajar/alphabet/huruf_a’: No such file or directory
```

**2. -v atau --verbose** : Opsi ini menampilkan informasi atau laporan mengenai direktori yang sedang dibuat. Opsi ini berguna untuk memastikan bahwa direktori berhasil dibuat serta memantau proses pembuatan folder

Contoh:
```bash
mkdir -v folder_baru1 folder_baru2
```
Maka terminal akan menampilkan :
```bash
mkdir: created directory 'folder_baru1'
mkdir: created directory 'folder_baru2'
```

**3. -m atau --mode** : Opsi ini digunakan untuk mengatur hak akses (permission) direktori saat direktori tersebut dibuat tanpa repot-repot menggunakan command `chmod`.

Contoh:
```bash
mkdir -m 750 nama_direktori
```
---

## KONSEP PERMISSION PADA DIREKTORI DAN FILE LINUX
Perlu diketahui bahwa Linux menggunakan sistem permission untuk mengatur siapa saja yang boleh mengakses sebuah file ataupun direktori. Jenis Izin : 

***r (read)*** → melihat isi direktori

***w (write)*** → menambah atau menghapus file di dalam direktori

***x (execute)*** → masuk ke dalam direktori



**NILAI ANGKA PERMISSION :**

Setiap izin memiliki nilai numerik:

***read (r)*** = 	4

***write (w)***	= 2

***execute (x)***	= 1

**CONTOH GABUNGAN NILAI :**

**7 → rwx** (akses penuh)

**5 → r-x** (lihat dan masuk)

**6 → rw-** (lihat dan ubah isi, tapi tidak bisa masuk)

**0 → ---** (tidak ada izin)

**KELOMPOK PERMISSION :**

Permission selalu dibagi menjadi tiga kelompok:

**Owner** → pemilik file atau direktori

**Group** → kelompok user

**Others** → semua user lain

Urutan penulisan permission numerik:

**Owner | Group | Others**


Contoh:
```bash
mkdir -m 750 data
```

Artinya:

**Owner → 7 → rwx** (akses penuh)

**Group → 5 → r-x** (lihat dan masuk)

**Others → 0 → ---** (tidak ada akses)

---

## CATATAN TAMBAHAN
**1. Direktori tersembunyi** : Pada Linux terdapat konsep Direktori tersembunyi yang hanya bisa terlihat ketika menggunakan command `ls -la`. Direktori tersembunyi tersebut, dibuat dengan menambahkan titik (.) pada awal nama direktori.

Contoh:
```bash
mkdir .folder_tersembunyi
```
**2. Direktori dengan Spasi**
Adapun konsep nama direktori yang terdapat karakter spasi di dalamnya. Nama direktori yang seperti ini tidak bisa dibuat dengan cara yang biasa, karena jika kalian mengetikkan perintah seperti ini di shell :
```bash
mkdir folder dengan spasi
```
Maka, kalian tidak membuat satu folder tetapi 3 folder :
```bash
ls
dengan  folder  spasi
```

Oleh karena itu, Direktori dengan spasi dapat dibuat dengan beberapa cara:

**a. Menggunakan tanda petik tunggal**:
```bash
mkdir 'nama folder'
```

**b. Menggunakan tanda petik ganda**:
```bash
mkdir "nama folder"
```

**c. Menggunakan escape sequence**:
```bash
mkdir nama\ folder
```
