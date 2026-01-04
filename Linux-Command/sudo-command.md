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

**5. -l atau --list** : Opsi ini digunakan untuk menampilkan daftar hak akses dan perintah apa saja yang diizinkan (atau dilarang) untuk dijalankan oleh pengguna melalui sudo. Opsi ini akan membaca file konfigurasi sistem yang disebut /etc/sudoers dan menampilkan ringkasan aturannya dengan format :

`(User yang diizinkan : Group yang diizinkan) Perintah yang diizinkan`

Sebagai contoh, disini saya ingin melihat hak akses apa saja yang dimiliki oleh akun yang saya gunakan saat ini yakni vandhaffa, maka saya cukup menggunakan command seperti ini : 
```bash
sudo -l
Matching Defaults entries for vandhaffa on LAPTOP-BVIKQQAI:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin, use_pty

User vandhaffa may run the following commands on LAPTOP-BVIKQQAI:
    (ALL : ALL) ALL
```
Dapat terlihat bahwa pada baris terakhir terdapat keterangan (ALL : ALL) ALL. Ini adalah tingkat akses tertinggi yang berarti user vandhaffa diizinkan menjalankan semua perintah sebagai semua user dan semua grup.

Tidak hanya itu, opsi -l juga bisa dikombinasikan dengan opsi -U (User list) untuk melakukan pengecekan hak akses milik user lain. Format perintahnya adalah: 
```bash
sudo -U nama_user -l
```
Sebagai contoh, saya ingin memastikan dan membandingkan hak akses antara user root(administrator tertinggi) dengan user www-data(user untuk layanan web server). Maka saya cukup menggunakan command seperti ini : 
```bash
sudo -U root -l
Matching Defaults entries for root on LAPTOP-BVIKQQAI:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin, use_pty

User root may run the following commands on LAPTOP-BVIKQQAI:
    (ALL : ALL) ALL
```
Dapat terlihat bahwa pada baris terakhir, user root memiliki izin (ALL : ALL) ALL. Ini mengonfirmasi bahwa root adalah superuser yang memiliki kekuasaan mutlak untuk menjalankan perintah apa pun sebagai siapa pun.

Selanjutnya, saya ingin memeriksa apakah user www-data memiliki hak untuk menggunakan sudo dengan menggunakan command yang sama :
```bash
sudo -U www-data -l
User www-data is not allowed to run sudo on LAPTOP-BVIKQQAI.
```
Dapat terlihat pula bahwa sistem memberikan keterangan tegas bahwa user www-data tidak diizinkan menjalankan perintah sudo. Ini adalah praktik keamanan yang benar, karena user layanan aplikasi seharusnya tidak memiliki hak administratif demi mencegah eksploitasi jika server web diretas.

**6. -v atau --validate** : Opsi ini digunakan untuk memperbarui autentikasi atau masa berlaku kata sandi sudo kita tanpa menjalankan perintah apa pun. Seperti yang saya katakan pada penjelasan [No Option](https://github.com/vandhaffa07/Documentation-of-The-Linux-Learning-Journey/edit/main/Linux-Command/sudo-command.md#:~:text=OPTION-,1.%20No%20option,-%3A%20Tanpa%20menggunakan%20opsi), sudo akan meminta password saat pertama kali kita jalankan dan akan mengingat kita dalam kurun waktu 5–15 menit. Selama kurun waktu itu, kita bisa menggunakan sudo sepuasnya tanpa harus input password setiap kali kita menjalankannya. Tapi setelah kurun waktu tersebut habis, maka akan ada konfirmasi ulang lagi berupa input password. Opsi -v hadir untuk memperbarui stempel waktu (timestamp) tersebut agar masa tenggangnya kembali penuh dari awal.

Sebagai contoh, misalkan saya ingin menginstal software Firefox. Karena proses ini melibatkan beberapa tahap yang mungkin memakan waktu lama (seperti men-download data yang besar), saya akan menggunakan kombinasi perintah berikut:

Mula-mula saya akan menjalankan perintah update untuk menyegarkan daftar aplikasi. Karena ini adalah perintah pertama, sistem akan meminta password seperti ini :
```bash
sudo apt update
[sudo] password for vandhaffa:
# Proses update sedang berlangsung...
```
Setelah proses update selesai. Selanjutnya, saya akan menjalankan perintah upgrade. Karena masa autentikasi saya belum habis, maka saya tidak perlu input password lagi untuk menjalankan perintah ini : 
```bash
sudo apt upgrade
# Proses upgrade berjalan langsung...
```
Setelah proses update dan upgrade selesai, maka sebenarnya kita sudah bisa melakukan instalasi software firefox. Akan tetapi, dikarenakan proses upgrade tadi memakan waktu cukup lama (misalnya 10 menit) dan saya khawatir sesi sudo akan segera habis sebelum instalasi selesai, maka saya menjalankan perintah -v seperti ini :
```bash
sudo -v
```
Dapat terlihat bahwa sistem tidak mengeluarkan output apa pun dan tidak meminta password. Namun, di balik layar, stempel waktu (timestamp) dan autentikasi saya telah diperbarui sehingga saya punya waktu penuh kembali.

Pada akhirnya, saya bisa menjalankan perintah instalasi dengan tenang tanpa takut terputus oleh permintaan password di tengah jalan : 
```bash
sudo apt install firefox
# Proses instalasi
```
**7. -k atau --reset-timestamp** : Opsi ini digunakan untuk menghapus atau membunuh sesi autentikasi sudo kita secara instan. Jika opsi -v berfungsi untuk memperpanjang masa tenggang password, maka -k adalah kebalikannya. Begitu perintah ini dijalankan, sistem akan langsung melupakan bahwa kita baru saja memasukkan password, sehingga perintah sudo berikutnya akan dipaksa untuk meminta konfirmasi password kembali. 

Opsi ini sangat krusial untuk aspek keamanan. Bayangkan ketika kita baru saja selesai menginstal aplikasi menggunakan sudo. Karena sesi sudo masih aktif selama 15 menit ke depan, orang lain yang memegang komputer kita bisa menjalankan perintah administratif tanpa password. Dengan sudo -k, kita mengunci kembali pintu keamanan tersebut secara manual sebelum meninggalkan komputer. 

Sebagai contoh, Misalnya saya baru saja menjalankan perintah sudo dan sesi atutentikasi-nya masih terekam oleh sistem, sehingga sistem tidak meminta password karena saya masih dalam masa tenggang :
```bash
sudo whoami
root
```
Dapat terlihat bahwa pada perintah tersebut saya tidak perlu memasukkan password lagi karena saya masih dalam masa tenggang.

Kemudian, saya ingin mengakhiri sesi akses tersebut menggunakan option -k demi keamanan agar tidak bisa disalahgunakan. Maka, saya cukup menjalankan command seperti ini :
```bash
sudo -k
```
Dapat terlihat bahwa tidak ada output apa pun, namun di balik layar, stempel waktu (timestamp) akses kita telah dihapus total dari memori sistem.

Untuk membuktikannya, saya mencoba menjalankan perintah sudo lagi tepat setelah melakukan -k : 
```bash
sudo whoami
[sudo] password for vandhaffa:
root
```
Dapat terlihat pula bahwa sistem langsung meminta password kembali. Ini membuktikan bahwa sesi sebelumnya telah berhasil dibunuh dan sistem tidak lagi mengingat Anda.

**8. -n atau --non-interactive** : Opsi ini digunakan untuk menjalankan perintah sudo dalam mode non-interaktif. Artinya, Ketika dijalankan, sudo akan memverifikasi apakah user masih memiliki sesi autentikasi yang valid (timestamp belum kedaluwarsa); jika masih valid, perintah akan langsung dijalankan, tetapi jika tidak valid, sudo akan berhenti dan menampilkan pesan error

Sebagai contoh, misalnya saya baru saja menyalakan komputer dan membuka terminal linux, lalu saya mencoba menjalankan perintah dengan opsi -n seperti ini :
```bash
sudo -n apt update
sudo: a password is required
```
Dapat terlihat bahwa sistem tidak memberikan kesempatan kepada saya untuk mengetik password. Ia langsung mengeluarkan pesan "a password is required" dan menghentikan proses. Hal ini terjadi karena ketika kita baru saja membuka terminal, kita tidak memiliki sesi autentikasi apa pun yang valid, sehingga mode non-interaktif menolak untuk melanjutkan.

**9. -E atau --preserve-env** : Secara default, sudo akan menghapus atau menolak sebagian besar variabel lingkungan milik user biasa agar tidak memengaruhi proses root. Namun, dengan opsi -E, kita memaksa sistem untuk tetap mempertahankan variabel lingkungan (environment variables) milik penggunHa ketika menjalankan perintah sebagai root.

Sebagai contoh, mari kita bandingkan bagaimana variabel yang kita buat sendiri berperilaku saat menggunakan sudo biasa dan saat menggunakan sudo -E. 

Mula-mula, saya akan membuat variabel baru bernama TESTING_VARIABLE di sesi user biasa seperti ini :
```bash
export TESTING_VARIABLE=testing
```
Kemudian saya akan memastikan bahwa variabel tersebut sudah terdaftar di lingkungan (environment) saya saat ini menggunakan kombinasi perintah env dan grep seperti ini :
```bash
env | grep "TESTING_VARIABLE"
TESTING_VARIABLE=testing
```
Dapat terlihat bahwa pada akun user biasa, variabel tersebut aktif dan terdeteksi dengan benar.

Selanjutnya, saya mencoba melihat apakah root bisa melihat variabel tersebut melalui sudo biasa :
```bash
sudo env | grep "TESTING_VARIABLE"
```
Dapat terlihat bahwa tidak ada output yang muncul. Ini membuktikan bahwa secara default, sudo melakukan pembersihan keamanan dan membuang variabel lingkungan milik user biasa sebelum menjalankan perintah sebagai root.

Terakhir, saya menjalankan perintah yang sama namun dengan menambahkan opsi -E setelah sudo seperti ini : 
```bash
sudo -E env | grep "TESTING_VARIABLE"
TESTING_VARIABLE=testing
```
Dapat terlihat pula bahwa variabel tersebut sekarang muncul di lingkungan root. Dengan kata lain, opsi -E berhasil memaksa sudo untuk mempertahankan atau mengangkut variabel lingkungan milik user vandhaffa ke dalam sesi root.

**10. -g atau --group=group** : Secara default, ketika kita menggunakan sudo, perintah akan dijalankan dengan grup utama milik root. Namun, dengan opsi -g, kita bisa menentukan identitas grup lain yang ada di sistem untuk mengeksekusi perintah tersebut. 

Kita bisa menggunakan perintah `cat /etc/group` untuk melihat informasi mengenai group apa saja yang terdaftar dalam sistem kita seperti ini :
```bash
cat /etc/group
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
..........dst
```
Sebagai contoh, mari kita bandingkan bagaimana sifat group saat kita menjalankan perintah id dengan sudo biasa dan saat kita menjalankan perintah id dengan sudo -g

Mula-mula saya akan menjalankan `sudo id` tanpa opsi tambahan seperti ini :
```bash
sudo id
uid=0(root) gid=0(root) groups=0(root)
```
Dapat terlihat bahwa GID (Group ID) yang digunakan adalah 0 (root). Hal ini membuktikkan jika kita menjalankan sudo tanpa opsi tambahan, secara otomatis kita akan menggunakan identitas root sepenuhnya (baik User ID maupun Group ID).

Selanjutnya, saya akan menjalankan perintah yang sama sebagai user saya sendiri (vandhaffa), tetapi menggunakan wewenang dari grup sys (ID 3) seperti ini :
```bash
sudo -g sys id
uid=1000(vandhaffa) gid=3(sys) groups=3(sys),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),100(users),1000(vandhaffa)
```
Dapat terlihat pula bahwa identitas user tetap 1000(vandhaffa), namun GID utamanya telah berubah menjadi 3(sys). Dengan kata lain, opsi -g berhasil meminjam wewenang grup sys untuk menjalankan perintah tersebut.













