# PERINTAH `rm` PADA LINUX

## PENGERTIAN
Perintah `rm` di Linux digunakan untuk menghapus file atau direktori. Penghapusan dengan `rm` bersifat permanen dan tidak masuk ke recycle bin, sehingga data yang dihapus tidak dapat dibatalkan dengan mudah. Karena sifatnya tersebut, `rm` termasuk salah satu perintah Linux yang paling berisiko jika digunakan tanpa kehati-hatian.

---

## SINTAKS
Sintaks umum perintah `rm` adalah:

```bash
rm [option] [file atau direktori]
```

Contoh Sederhana :

```bash
rm file.txt
```

---

## OPTION

**1. -f atau --force** : Opsi ini memaksa penghapusan file tanpa meminta konfirmasi, bahkan jika file tidak ada atau memiliki batasan permission.
⚠️ Peringatan : Penggunaan opsi ini sangat berbahaya, terutama jika dikombinasikan dengan -r.

Contoh:
```bash
rm -f nama_file
```

**2. -r, -R, atau --recursive** : Opsi ini menyebabkan penghapusan rekursif. Digunakan untuk menghapus direktori beserta seluruh isinya (subdirektori dan file yang ada di dalamnya).

Contoh:
```bash
rm -r nama_folder/
```

**3. -i atau --interactive** : Meminta konfirmasi sebelum menghapus setiap file atau direktori. Opsi ini berguna untuk mencegah penghapusan tidak disengaja, menjadikannya option paling aman digunakan serta disarankan saat menggunakan command rm

Contoh:
```bash
rm -i nama_file
```
Maka nanti terminal akan menampilkan : 
```bash
rm: remove regular file 'nama_file'?
```

**4. -v atau --verbose** : Digunakan untuk menampilkan informasi file atau direktori yang sedang dihapus. Opsi ini membantu untuk memastikan apa saja yang terhapus serta memantau proses penghapusan

Contoh:
```bash
rm -rv nama_folder/
```
Maka nanti terminal akan menampilkan :
```bash
removed 'nama_folder/nama_file1'
removed 'nama_folder/nama_file2'
dst....
removed directory 'nama_folder/'
```


**5. -I atau --interactive=once** : Meminta konfirmasi hanya satu kali saat menghapus banyak file atau direktori.  Berbeda dengan -i yang menanyakan konfirmasi setiap satu file sebelum dihapus

Contoh:
```bash
rm -rI nama_folder/
```
Maka nanti terminal akan menampilkan : 
```bash
rm: remove 1 argument recursively? 
```

