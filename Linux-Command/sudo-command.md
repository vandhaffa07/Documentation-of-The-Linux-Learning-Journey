# PERINTAH `sudo` PADA LINUX

## PENGERTIAN
`sudo` (SuperUser DO) merupakan sebuah perintah yang memungkinkan pengguna untuk menjalankan instruksi dengan hak akses pengguna lain bernama root(akun superuser dengan hak akses tertinggi untuk mengelola seluruh sistem operasi dan file). sudo memberikan izin akses tersebut secara sementara untuk perintah/command yang sedang kita dijalankan. Secara teknis, mekanisme ini bekerja dengan memberikan UID 0 (identitas unik milik root) kepada proses yang dipicu oleh perintah tersebut. Karena kernel Linux memverifikasi setiap izin akses berdasarkan UID dan GID, proses yang telah mengantongi identitas root ini akan mendapatkan lampu hijau untuk mengakses sumber daya sistem yang biasanya diproteksi. 

---

## SINTAKS
Sintaks umum perintah `sudo` adalah : 
```bash
sudo [OPTION] [COMMAND] [ARGUMENT]
```

---

## OPTION

**1. No option** : Tanpa menggunakan opsi tambahan, sudo secara default akan menjalankan satu perintah dengan hak akses user lain, yaitu root. Saat pertama kali dijalankan, sudo akan meminta kata sandi pengguna (user password) untuk memverifikasi identitas. Setelah berhasil diverifikasi, sistem akan menyimpan status autentikasi tersebut dalam bentuk timestamp sementara, sehingga pengguna dapat menjalankan perintah sudo berikutnya tanpa memasukkan kata sandi ulang selama beberapa menit (umumnya sekitar 5–15 menit). Mekanisme ini memudahkan administrasi sistem tanpa harus berulang kali memasukkan kata sandi.

Sebagai contoh saya ingin membaca file /etc/shadow, yaitu file sistem yang menyimpan informasi kata sandi terenkripsi. File ini bersifat sangat rahasia, sehingga secara default, pengguna biasa tidak memiliki izin untuk membukanya demi alasan keamanan.

Mula-mula saya akan mencoba melihat isi file tersebut dengan menggunakan perintah cat sebagai pengguna biasa, seperti ini :
```bash
cat /etc/shadow
cat: /etc/shadow: Permission denied
```
Dapat terlihat bahwa sistem menolak akses tersebut dengan pesan Permission denied. Hal ini dikarenakan atribut keamanan pada file /etc/shadow hanya mengizinkan akun root untuk membacanya.

Kemudian, saya akan mengulang perintah yang sama, namun kali ini dengan menambahkan sudo di awal perintah untuk meminta izin akses administratif seperti ini : 
```bash
sudo cat /etc/shadow
```
Setelah menjalankan perintah tersebut, sistem akan meminta kata sandi akun saya sendiri (user password) untuk memastikan bahwa memang saya yang memegang kendali atas sesi terminal ini. Biasanya outputnya akan seperti ini : 
```bash
[sudo] password for vandhaffa:
```
Setelah kata sandi yang dimasukkan benar, dapat terlihat bahwa isi file yang tadinya diproteksi kini muncul di layar seperti ini:
```bash
root:*:********:0:99999:7:::
daemon:*:********:0:99999:7:::
bin:*:********:0:99999:7:::
sys:*:********:0:99999:7:::
```
Catatan : karena alasan keamanan, output /etc/shadow berikut telah disamarkan

Dan jika saya menjalankan perintah sudo lain tepat setelah ini, sistem tidak akan menanyakan kata sandi lagi karena saya masih berada dalam waktu akses yang diberikan sebelumnya. 

Sebagai contoh, setelah melihat file /etc/shadow, saya ingin memeriksa konfigurasi shell milik root yang berada di direktori /root/ seperti ini :
```bash
sudo cat /root/.bashrc
# ~/.bashrc: executed by bash(1) for non-login shells.
# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
# for examples
....................dst
```
Karena saya baru saja memasukkan kata sandi beberapa saat yang lalu, dapat terlihat bahwa sistem langsung menampilkan isi filenya tanpa memunculkan baris permintaan password lagi

**2. -U user atau --other-user=user** : Opsi ini memungkinkan kita untuk menjalankan perintah sebagai pengguna lain yang terdaftar di dalam sistem kita(tidak selalu root). Hal ini sangat berguna untuk mengelola aplikasi atau layanan yang berjalan di bawah identitas pengguna khusus tanpa harus memberikan akses root penuh.

Kita bisa menggunakan perintah `cat /etc/passwd` untuk melihat informasi mengenai pengguna (user) yang terdaftar dalam sistem kita seperti ini :
```bash
cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
..............dst
```
Sebagai contoh, mari kita bandingkan identitas asli kita dengan identitas yang kita pinjam menggunakan opsi -u.

Mula-mula, saya akan memastikan siapa diri kita sebenarnya di dalam sistem saat ini menggunakan command `whoami && id` seperti ini :
```bash
whoami && id
vandhaffa
uid=1000(vandhaffa) gid=1000(vandhaffa) groups=1000(vandhaffa),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),100(users)
```
Dapat terlihat bahwa saat ini saya masuk sebagai user vandhaffa dengan UID 1000. Ini adalah identitas saya sebagai pengguna biasa.

Selanjutnya, saya akan menjalankan perintah yang sama namun, dengan tambahan opsi `-u root` seperti ini : 
```bash
sudo -u root whoami && sudo -u root id
root
uid=0(root) gid=0(root) groups=0(root)
```
Dapat terlihat bahwa meskipun saya tetap masuk di akun vandhaffa, perintah tersebut dijalankan dengan identitas root yang memiliki UID 0. Ini membuktikan bahwa sudo berhasil memberikan hak akses superuser secara sementara.

Selain root, kita juga bisa meminjam identitas user layanan lainnya asalkan nama user tersebut terdaftar di dalam file /etc/passwd. Misalnya, kita menggunakan user daemon:
```bash
sudo -u daemon whoami && sudo -u daemon id
daemon
uid=1(daemon) gid=1(daemon) groups=1(daemon)
```
Dapat terlihat pula bahwa identitas proses berubah menjadi daemon dengan UID 1. Hal ini sangat berguna jika kita perlu memanipulasi file milik layanan sistem tertentu tanpa harus mengganggu file milik root atau user lainnya.

**3. -i atau --login** : Opsi ini digunakan untuk masuk/login ke dalam lingkungan superuser secara penuh. Berbeda dengan sudo biasa yang hanya meminjamkan hak akses untuk setiap satu perintah, -i akan memindahkan kita ke shell baru sebagai root, lengkap dengan memuat pengaturan lingkungan (environment variables) milik root seperti $HOME, $PATH, dan direktori kerja yang berpindah ke /root.

Sebagai contoh, Mari kita bandingkan identitas dan variabel lingkungan default milik user biasa dengan kondisi ketika kita sudah masuk ke dalam lingkungan root secara penuh menggunakan opsi -i.

Mula-mula, saya ingin memastikan identitas dan beberapa variabel lingkungan sebelum masuk ke lingkungan root menggunakan command seperti ini : 
```bash
whoami && echo $PWD && echo $HOME
vandhaffa
/home/vandhaffa
/home/vandhaffa
```
Dapat terlihat bahwa saya sedang masuk sebagai pengguna biasa (vandhaffa), variabel $PWD (direktori saat ini) berada di folder user, dan variabel $HOME juga mengarah ke direktori personal saya.

Selanjutnya saya akan menjalankan perintah sudo -i untuk login kedalam lingkungan root sepenuhnya. Maka sistem akan memuat profil root dan menampilkan sambutan khas kali linux seperti ini : 
```bash
 sudo -i
┏━(Message from Kali developers)
┃
┃ This is a minimal installation of Kali Linux, you likely
┃ want to install supplementary tools. Learn how:
┃ ⇒ https://www.kali.org/docs/troubleshooting/common-minimum-setup/
┃
┗━(Run: “touch ~/.hushlogin” to hide this message)
┌──(root㉿LAPTOP-BVIKQQAI)-[~]
└─#
```
Dapat terlihat bahwa tanda prompt terminal berubah menjadi #, yang menandakan bahwa kita sudah memiliki hak akses penuh sebagai administrator.

Selanjutnya, saya akan menjalankan perintah yang sama seperti saat menjadi user biasa(vandhaffa) untuk melihat perubahan yang terjadi pada variabel lingkungan : 
```bash
whoami && echo $PWD && echo $HOME
root
/root
/root
```
Dapat terlihat  bahwa variabel $HOME dan $PWD kini telah berubah total menjadi /root. Ini membuktikan bahwa opsi -i tidak sekadar meminjam izin akses, tapi benar-benar memindahkan sesi kerja kita ke dalam rumah milik root.

Untuk melakukan logout dari mode root dan kembali ke akun biasa, maka kita cukup menggunakan command `exit`  seperti ini :
```bash
exit
logout
┌──(vandhaffa㉿LAPTOP-BVIKQQAI)-[~]
└─$
```
Dapat terlihat bahwa setelah mengetik exit, saya kembali menjadi user vandhaffa dan berada di direktori semula.

**4. -s atau --shell** : Opsi ini digunakan untuk menjalankan shell interaktif sebagai root, namun tetap mempertahankan direktori kerja (current directory) dari pengguna saat ini. Berbeda dengan -i yang memindahkan Anda sepenuhnya ke lingkungan root, opsi -s memberikan Anda kekuatan root tetapi membiarkan Anda tetap berada di lokasi folder tempat Anda bekerja sebelumnya.

Sebagai contoh, mari kita bandingkan identitas, direktori kerja, dan beberapa variabel lingkungan default milik user biasa dengan kondisi ketika kita sudah masuk ke dalam lingkungan root menggunakan opsi -s.

Mula-mula, saya akan memastikan identitas dan variabel lingkungan saat masih menjadi pengguna biasa menggunakan command seperti ini :
```bash
whoami && echo $PWD && echo $HOME
vandhaffa
/home/vandhaffa
/home/vandhaffa
```
Dapat terlihat bahwa saat ini saya masuk sebagai vandhaffa, dengan direktori kerja ($PWD) dan direktori rumah ($HOME) yang keduanya mengarah ke folder user biasa.

Selanjutnya, saya mengeksekusi perintah -s untuk mendapatkan akses root tanpa harus berpindah lokasi seperti ini :
```bash
sudo -s
┌──(root㉿LAPTOP-BVIKQQAI)-[/home/vandhaffa]
└─#
```
Dapat terlihat bahwa prompt terminal telah berubah menjadi milik root, namun teks di dalam kurung siku masih tetap berada di direktori /home/vandhaffa. Hal ini menunjukkan bahwa opsi ini sangat praktis jika kita sedang membutuhkan akses root tanpa ingin kehilangan posisi direktori kerja Anda saat ini.

Selanjutnya, kita akan melihat hasil dari perintah whoami dan beberapa variabel lingkungannya menggunakan command yang sama seperti saat menjadi user biasa(vandhaffa) :
```bash
whoami && echo $PWD && echo $HOME
root
/home/vandhaffa
/root
```
Dapat terlihat bahwa identitas pengguna telah berubah menjadi root, dan variabel lingkungan $HOME kini mengarah ke direktori /root. Namun demikian, variabel $PWD (direktori kerja saat ini) tetap menunjuk ke direktori awal, yaitu /home/vandhaffa. Kondisi inilah yang menegaskan perbedaan mendasar antara opsi -s dan -i pada perintah sudo. Dimana pada opsi -i, sudo mensimulasikan login shell sebagai root, sehingga direktori kerja secara otomatis dipindahkan ke /root dan lingkungan root dimuat sepenuhnya. Sebaliknya, pada opsi -s, pengguna memang memperoleh hak akses root, tetapi direktori kerja tidak diubah dan tetap berada pada lokasi sebelumnya.


