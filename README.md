# PHP dan Database MySQL

## Langkah-langkah Praktikum

**Persiapan** Untuk memulai membuat aplikasi CRUD sederhana, yang perlu disiapkan adalah
database server menggunakan MySQL. Pastikan MySQL Server sudah dapat dijalankan
melalui XAMPP.

### Menjalankan MySQL

Untuk menjalankan MySQL Server dari menu XAMPP Control.

![XAMPP](img/XAMPP.png)

### Mengakses MySQL Cilent menggunakan PHHP MyAdmin

Pastikan webserver Apache dan MaySQL server sudah dijalankan. Kemudian melalui browser : http://localhost/phpmyadmin/

### Membuat Database: Studi Kasus Data Barang

![Studi_kasus_data_barang](img/Studi_Kasus.png)

### <u>Membuat Database</u>

![creat](source/img/creat.png)

### <u>Membuat Tabel</u>

![tabel](source/img/tabel.png)

![Create_Table](img/CreateTable.png)

### <u>Menambahkan Data</u>

![insert](source/img/insert.png)

![Tabel_barang](img/TabelBarang.png)

### Membuat Program CRUD

Buat folder **<u>lab8_php_database</u>** pada root directory web server (c:\xampp\htdocs)

![Htdocts](img/Htdocs.png)

Kemudian untuk mengakses direktory tersebut pada web server dengan mengakses URL:
http://locallhost/lab8_php_database/

![Data_web](img/Data_web.png)

### Membuat File Koneksi Database

Buat file baru dengan nama **koneksi.php**

![koneksi](source/img/koneksi.png)

Buka melalui browser untuk menguji koneksi database (untuk menyampaikan pesan koneksi berhasil, _**uncomment**_ pada perintah `echo "koneksi berhasil";`)

![Koneksi_terhubung](img/Koneksi_terhubung.png)

### Membuat File Index Untuk Menampilkan Data (_Read_)

Buat file dengan nama **index.php**

![index](source/img/index.png)

![Tabel_Barang_Web](img/Web_Tabel_Barang.png)

### Menambahkan Data (_Create_)

Buat file baru dengan nama **tambah.php**

![tambah](source/img/tambah.png)

![tambah1](source/img/tambah1.png)

![tambah2](source/img/tambah2.png)

Sebelum di tambah 
![Mengubah_barang](img/sebelum_ubah.png)

Setelah di tambah
![Data_di_ubah](img/menambah.png)

### Menghapus Data (_Delete_)

Buat file baru dengan nama **hapus.php**

![hapus](source/img/hapus.png)

Sebelum

![sebelum_dihapus](img/hapus.png)

Sesudah

![Sudah_dihapus](img/S_hapus.png)