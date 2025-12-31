# PERINTAH `stat` PADA LINUX

## PENGERTIAN
`stat` adalah perintah di Linux yang digunakan untuk menampilkan informasi metadata secara terperinci tentang sebuah file, folder atau filesystem. Berbeda dengan `ls` yang hanya menampilkan ringkasan, `stat` memberikan detail internal seperti inode, permission, kepemilikan, serta berbagai jenis timestamp.

---

## SINTAKS
Sintaks umum perintah `stat` adalah :
```bash
stat [OPTION] [FILE ATAU FOLDER]
```

---

## INFORMASI FILE YANG DITAMPILKAN (JIKA DIJALANKAN TANPA OPSI)
Saat `stat` dijalankan tanpa opsi apapun, maka informasi berikut akan ditampilkan:

* **File** : Menampilkann nama file yang diberikan. Jika file tersebut adalah symbolic link, maka nama target link akan ditampilkan terpisah.

* **File type** : Menampilkan Jenis file, seperti Regular file(Jenis file paling umum dalam sistem operasi yang menyimpan data aktual seperti teks, gambar, program, atau video), Symbolic link(file yang menunjuk ke path file lain), Directory(folder atau file khusus yang menyimpan daftar nama file), dll

* **Device** :  Menunjukkan identitas block device tempat filesystem yang mengelola suatu file berada. Informasi ini digunakan oleh Kernel Linux untuk mengetahui di perangkat penyimpanan mana filesystem tersebut berjalan, sehingga operasi baca dan tulis dapat diarahkan ke driver dan device yang tepat. Nilai yang ditampilkan pada field Device, misalnya 8,48, merupakan pasangan major number dan minor number. Angka pertama (8) adalah major number, yang menandakan driver kernel yang menangani device tersebut. Major number ini mengidentifikasi kelas perangkat, misalnya driver untuk disk berbasis SCSI/SATA atau media penyimpanan serupa. Angka kedua (48) adalah minor number, yang berfungsi sebagai kode identitas untuk device spesifik yang dikelola oleh driver tersebut. Minor number ini memungkinkan kernel membedakan satu unit disk atau partisi dari unit lain yang berada di bawah driver yang sama.

* **Inode** : Menampilkan nomor inode dari sebuah file. Inode adalah struktur data internal pada filesystem Linux yang digunakan untuk menyimpan keseluruhan metadata file, seperti ukuran file, permission, kepemilikan, timestamp, serta referensi ke blok-blok data tempat isi file disimpan. Sedangkan nomor inode adalah identitas unik yang digunakan filesystem untuk merujuk ke inode tersebut.

* **Links** : Jumlah hard link yang menunjuk ke inode yang sama. Nilai ini merepresentasikan berapa banyak entri yang saat ini merujuk ke inode yang sama. Dalam filesystem Linux, 1 inode dapat memiliki lebih dari satu nama melalui mekanisme hard link, dan selama inode   tersebut masih memiliki setidaknya satu hard link, data file akan tetap ada meskipun salah satu nama file dihapus. Selain   itu, 2 file yang berada pada filesystem yang berbeda dapat memiliki nomor inode yang sama tanpa menimbulkan konflik. Untuk   membedakan file secara absolut, kernel Linux menggunakan kombinasi Device ID dan inode number sebagai pasangan identitas yang unik.

* **Size** : Menampilkan ukuran file dalam byte.

* **Blocks** : Menampilkan jumlah blok disk yang benar-benar dialokasikan untuk menyimpan isi sebuah file. Filesystem Linux menyimpan data dalam satuan block, bukan byte satu per satu. Ketika sebuah file dibuat atau diisi, filesystem akan mengalokasikan sejumlah block untuk menyimpan data tersebut, dan jumlah block inilah yang ditampilkan pada field Blocks.
  
