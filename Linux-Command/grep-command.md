# PERINTAH `grep` PADA LINUX

## PENGERTIAN
`grep` (Global Regular Expression Print) adalah perintah yang digunakan untuk mencari pola teks spesifik dalam file atau output perintah dan menampilkannya pada terminal. Perintah ini berfungsi untuk mengecek keberadaan suatu entri dengan cepat, terutama dalam file yang panjang seperti file log.

---

## SINTAKS
Sintaks umum perintah `grep` adalah :
```bash
grep [OPTION][POLA][FILE]
```

---

## OPTION

**1. No Option** : Tanpa menggunakan tambahan opsi apa pun, grep akan bekerja secara case-sensitive (disiplin terhadap huruf besar dan kecil). Pada kondisi default ini, grep hanya akan mencocokkan pola teks yang penulisan hurufnya benar-benar sama, termasuk kapitalisasi. Dalam mode ini juga, grep dapat digunakan untuk mencari pola pada satu file maupun banyak file sekaligus.

**Contoh :**

Mula-mula, saya akan membuat 3 file teks percobaan dengan variasi penulisan kata Linux seperti ini : 
```bash
echo "Belajar Linux itu seru" > file1.txt && echo "belajar linux itu mudah" > file2.txt && echo "LINUX ADALAH KUNCI" > file3.txt
```
Setelah file berhasil dibuat, saya akan memverifikasi isi dari masing-masing file untuk memastikan bahwa konten file sudah sesuai dengan apa yang saya inginkan dengan menggunakan command cat seperti ini :
```bash
cat file1.txt file2.txt file3.txt
Belajar Linux itu seru
belajar linux itu mudah
LINUX ADALAH KUNCI
```
Dapat terlihat bahwa kita sekarang memiliki tiga baris teks dengan variasi penulisan kata "Linux" yang berbeda-beda (Kapital awal, kecil semua, dan kapital semua)

Selanjutnya saya akan mencoba menggunakan command grep untuk mencari kata "Linux" pada file1.txt. Maka saya cukup menggunakan perintah seperti ini :
```bash
grep Linux file1.txt
Belajar Linux itu seru
```
Dapat terlihat bahwa grep berhasil menemukan pola "Linux" pada file1.txt karena penulisan hurufnya sesuai(identik) dengan pola yang kita cari.

Sebaliknya, jika saya mencoba pola yang sama pada file lain:
```bash
grep Linux file2.txt
```
Dapat terlihat bahwa perintah tersebut tidak menghasilkan output apa pun. Hal ini terjadi karena kata "linux" pada file2.txt ditulis menggunakan huruf kecil seluruhnya, sehingga dianggap tidak cocok oleh sistem case-sensitive grep.

Selanjutnya saya akan mencoba untuk menggunakan command grep langsung pada 3 file sekaligus yang telah kita buat sebelumnya:
```bash
grep Linux file1.txt file2.txt file3.txt
file1.txt:Belajar Linux itu seru
```
Dapat terlihat bahwa pada pencarian multi-file, grep secara otomatis menampilkan nama file di awal baris yang kemudian diikuti oleh konten yang cocok. Fitur ini sangat memudahkan kita untuk memetakan dari mana asal baris teks tersebut ditemukan.

**2. -i atau --ignore-case**: Opsi ini digunakan untuk mengabaikan perbedaan huruf besar dan kecil (case-insensitive). Dengan opsi ini, grep tidak lagi membedakan antara huruf kapital dan huruf kecil, sehingga pola pencarian akan dianggap sama meskipun penulisannya berbeda.

**Contoh** :

Disini, saya masih menggunakan file file1.txt, file2.txt, dan file3.txt yang telah dibuat sebelumnya, saya akan mencoba mencari pola linux dengan mengaktifkan opsi -i, seperti ini:
```bash
grep -i linux file1.txt
Belajar Linux itu seru
```
Dapat terlihat bahwa meskipun pada file1.txt kata Linux ditulis dengan huruf kapital di awal, baris tersebut tetap ditampilkan karena grep tidak lagi membedakan kapitalisasi huruf.

Jika pencarian yang sama diterapkan pada banyak file sekaligus, hasilnya akan lebih jelas terlihat, seperti ini:
```bash
grep -i linux file1.txt file2.txt file3.txt
file1.txt:Belajar Linux itu seru
file2.txt:belajar linux itu mudah
file3.txt:LINUX ADALAH KUNCI
```
Dapat dilihat bahwa seluruh variasi penulisan kata Linux berhasil ditemukan. Hal ini berbeda dengan perilaku default grep seperti pada percobaan sebelumnya yang hanya mencocokkan teks dengan penulisan huruf yang benar-benar sama.

**3. -n atau --line-number** : Opsi ini digunakan untuk menampilkan nomor baris dari setiap baris yang cocok dengan pola pencarian. Informasi ini sangat berguna ketika kita perlu mengetahui lokasi pasti suatu teks di dalam file, misalnya saat menelusuri error pada log atau source code.

Sebagai contoh, saya akan menyimulasikan bagaimana opsi ini bekerja pada suatu file log imajiner yang saya buat sendiri 

Mula-mula, saya akan membuat bahan percobaan berupa sebuah file log sederhana bernama app.log yang berisi beberapa baris aktivitas sistem seperti ini :
```bash
cat > app.log 
[INFO] Application started
[INFO] Connecting to database
[WARNING] Connection is slow
[ERROR] Database connection failed
[INFO] Retrying connection
[ERROR] Retry failed
```
Setelah file berhasil dibuat, saya akan mencoba menggunakan perintah grep dengan option -n untuk menampilkan baris yang mengandung kata "ERROR" beserta nomor baris yang mengandung kata tersebut :
```bash
grep -n ERROR app.log
4:[ERROR] Database connection failed
6:[ERROR] Retry failed
```
Dapat terlihat bahwa grep dengan option -n memberikan informasi presisi mengenai letak kata "ERROR" pada file tersebut, yakni pada baris ke-4 dan ke-6

**4. -v atau --invert-match** : Opsi ini berbeda dengan perintah grep pada umumnya. Alih-alih menampilkan baris yang cocok dengan pola yang kita tentukan, opsi ini justru akan menampilkan semua baris yang tidak mengandung pola yang kita tentukan.

**Contoh :**

Disini saya akan menyimulasikan bagaimana opsi ini bekerja pada file log imajiner bernama app.log yang telah saya buat pada penjelasan sebelumnya. berikut adalah isi file app.log :
```bash
cat -n app.log
     1  [INFO] Application started
     2  [INFO] Connecting to database
     3  [WARNING] Connection is slow
     4  [ERROR] Database connection failed
     5  [INFO] Retrying connection
     6  [ERROR] Retry failed
```
Selanjutnya, saya akan mencari baris yang tidak mengandung kata "INFO" pada file tersebut menggunakan perintah seperti ini :
```bash
grep -v INFO app.log
[WARNING] Connection is slow
[ERROR] Database connection failed
[ERROR] Retry failed
```
Dapat terlihat bahwa grep hanya menyaring baris yang mengandung kata ERROR dan WARNING saja. Hal ini membantu kita untuk membersihkan terminal dari informasi yang tidak relevan, sehingga mempermudah kita dalam proses pemantauan (monitoring).

**5. -w atau --word-regexp** : Digunakan untuk mencari kata yang benar-benar utuh (whole word). Dengan kriteria ini, grep tidak akan mengambil baris di mana pola yang kita tentukan hanya menjadi bagian dari kata yang lebih panjang, sebuah baris hanya akan dianggap cocok oleh grep jika pola tersebut diapit oleh spasi, tanda baca, atau berada di awal/akhir baris.

**Contoh :**

Mula-mula, saya akan membuat bahan percobaan berupa file teks bernama akses.txt yang berisi campuran kata-kata serupa seperti ini :
```bash
cat > akses.txt
Akses ke log sistem
User sedang login
User sedang logout
Lihat log harian
```
Setelah file berhasil dibuat, saya akan menggunakan grep tanpa tambahan opsi apapun untuk mencari baris yang mengandung kata "log" seperti ini :
```bash
grep "log" akses.txt
Akses ke log sistem
User sedang login
User sedang logout
Lihat log harian
```
Dapat terlihat bahwa semua baris pada file tersebut muncul di terminal. Hal ini terjadi karena di dalam kata "login" dan "logout" terdapat pola huruf "log" yang sama, sehingga bagi grep grep default, hal ini dianggap sebagai kecocokan yang sah.

Berbeda dengan ketika menggunakan option -w seperti ini :
```bash
grep -w "log" akses.txt
Akses ke log sistem
Lihat log harian
```
Dapat terlihat bahwa baris "login" dan "logout" kini telah tereliminasi dan tidak ditampilkan pada terminal. Hal ini terjadi karena grep -w secara cerdas menyaring hasil dan hanya menyisakan baris di mana "log" muncul sebagai satu kata utuh yang berdiri sendiri tanpa menempel pada huruf lainnya.


**6. -c atau --count** : Opsi ini digunakan untuk menghitung jumlah baris yang cocok dengan pola pencarian yang kita tentukan. Dengan opsi ini, grep tidak akan menampilkan isi teks dari baris tersebut, melainkan hanya memberikan angka total dari temuannya.

**Contoh :**

Mula-mula, saya akan membuat file percobaan bernama transaksi.txt yang menampung data imajiner riwayat pembelian menggunakan command cat seperti ini :
```bash
cat > transaksi.txt 
[09:00] Status: SUCCESS - Pembelian Kopi
[09:15] Status: FAILED - Saldo Tidak Cukup
[09:30] Status: SUCCESS - Pembelian Roti
[10:00] Status: SUCCESS - Pembelian Susu
[10:15] Status: FAILED - Koneksi Timeout
```
Setelah file berhasil dibuat, saya akan mencoba menggunakan grep -c pada file tersebut untuk mengetahui berapa banyak transaksi yang tercatat memiliki status SUCCES :
```bash
grep -c "SUCCESS" transaksi.txt
3
```
Dapat terlihat bahwa grep tidak lagi menampilkan detail isi barisnya secara panjang lebar, melainkan langsung memberikan informasi angka yang presisi mengenai total temuan yang cocok. Dengan cara ini, saya bisa langsung mengetahui bahwa ada 3 transaksi sukses tanpa harus menghitungnya secara manual satu per satu di layar terminal.

