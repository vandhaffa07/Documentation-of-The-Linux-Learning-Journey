# PERINTAH `find` PADA LINUX

## PENGERTIAN
`find` adalah perintah yang digunakan untuk mencari file dan direktori berdasarkan berbagai kriteria yang sangat spesifik. Berbeda dengan perintah pencarian biasa seperti locate yang mengandalkan database, find bekerja dengan menyisir struktur direktori secara real-time. Ia bisa mencari berdasarkan
Nama file atau ekstensi, ukuran file (lebih besar atau lebih kecil dari nilai tertentu), waktu (kapan file terakhir diubah, diakses, atau dibuat), kepemilikan (siapa pemilik file atau grupnya), izin akses (mencari file yang bisa dieksekusi atau dibaca saja), hingga tipe file (file biasa, direktori, atau link).

---

## SINTAKS 
Sintaks umum perintah `find` adalah :
```bash
find [PATH_LOKASI] [KRITERIA] [AKSI]
```

---

## WILAYAH PENCARIAN/PATH
| Simbol / Path | Nama Wilayah | Cakupan Pencarian | Contoh Perintah |
| :--- | :--- | :--- | :--- |
| **.** | Current Directory | Folder saat ini dan semua sub-foldernya. | `find . -name "catatan.txt"` |
| **..** | Parent Directory | Satu level di atas folder saat ini. | `find .. -name "config.py"` |
| **~** | Home Directory | Seluruh folder pribadi user (Home). | `find ~ -name "*.jpg"` |
| **/** | Root Directory | Seluruh sistem komputer (dari akar/root). | `sudo find / -name "vmlinuz"` |
| **/var/log** | Absolute Path | Folder spesifik yang ditentukan jalurnya secara lengkap. | `find /var/log -name "*.log"` |

---

## OPTION/KRITERIA
**1. -name** : -name digunakan untuk mencari file yang namanya pas atau cocok dengan pola tertentu. Perlu diingat bahwa opsi ini bersifat case-sensitive (membedakan huruf besar dan kecil).

Sebagai contoh, saya akan menyimulasikan bagaimana cara mencari file di dalam direktori kerja saya saat ini menggunakan kriteria -name

Mula-mula, kita akan membuat beberapa file dengan nama yang mirip tapi berbeda huruf kapitalnya sebagai bahan percobaan kita seperti ini : 
```bash
touch laporan.txt LAPORAN.txt 
```
Selanjutnya, saya akan mencoba mencari file dengan nama laporan.txt (huruf kecil semua) menggunakan perintah seperti ini :
```bash
find . -name "laporan.txt"
./laporan.txt
```
Dapat terlihat bahwa hanya satu file yang muncul yakni laporan.txt. Dapat terlihat pula bahwa file LAPORAN.txt diabaikan karena find sangat disiplin terhadap perbedaan huruf besar dan kecil.

**2. -iname** : Kriteria ini berfungsi untuk mencari file berdasarkan nama, sama seperti -name, namun bersifat case-insensitive (mengabaikan perbedaan huruf besar dan kecil).

Sebagai contoh, saya akan menggunakan file dari simulasi sebelumnya (laporan.txt, LAPORAN.txt) untuk melihat fleksibilitas kriteria -iname seperti ini:
```bash
find . -iname "laporan.txt"
./laporan.txt
./LAPORAN.txt
```
Dapat terlihat bahwa perintah ini menampilkan kedua file tersebut meskipun salah satunya menggunakan huruf besar semua sedangkan satunya lagi menggunakan huruf kecil semua. Hal ini membuktikan bahwa Kriteria -iname memperlakukan "L" dan "l" sebagai karakter yang sama(case-insensitive)


