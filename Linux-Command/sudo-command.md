# PERINTAH `sudo` PADA LINUX

## PENGERTIAN
`sudo` (SuperUser DO) merupakan sebuah perintah yang memungkinkan pengguna untuk menjalankan instruksi dengan hak akses pengguna lain(defaultnya adalah root). Alih-alih mengharuskan kita untuk login sepenuhnya sebagai superuser yang seringkali berisiko bagi keamanan sistem, sudo memberikan izin akses tersebut secara sementara hanya untuk perintah tertentu yang sedang dijalankan. Secara teknis, mekanisme ini bekerja dengan memberikan UID 0 (identitas unik milik root) kepada proses yang dipicu oleh perintah tersebut. Karena kernel Linux memverifikasi setiap izin akses berdasarkan UID dan GID, proses yang telah mengantongi identitas root ini akan mendapatkan lampu hijau untuk mengakses sumber daya sistem yang biasanya diproteksi. 

---

## SINTAKS
Sintaks umum perintah `sudo` adalah : 
```bash
sudo [OPTION] [COMMAND] [ARGUMENT]
```

---

## OPTION

**1. No option** : Memungkingkan pengguna untuk menjalankan command sebagai root



