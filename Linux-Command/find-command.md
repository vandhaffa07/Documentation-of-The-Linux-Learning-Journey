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





