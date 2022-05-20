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

```PHP
        CREATE DATABASE latihan1;
```

### <u>Membuat Tabel</u>

```php
CREATE TABLE data_barang (
    id_barang int(10) auto_increment Primary Key,
    kategori varchar(30),
    nama varchar(30),
    gambar varchar(100),
    harga_beli decimal(10,0),
    harga_jual decimal(10,0),
    stok int(4)
);
```

![Create_Table](img/CreateTable.png)

### <u>Menambahkan Data</u>

```php
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```

![Tabel_barang](img/TabelBarang.png)

### Membuat Program CRUD

Buat folder **<u>lab8_php_database</u>** pada root directory web server (c:\xampp\htdocs)

![Htdocts](img/Htdocs.png)

Kemudian untuk mengakses direktory tersebut pada web server dengan mengakses URL:
http://locallhost/lab8_php_database/

![Data_web](img/Data_web.png)

### Membuat File Koneksi Database

Buat file baru dengan nama **koneksi.php**

```php
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db = "latihan1";
$conn = mysqli_connect($host, $user, $pass, $db);
if ($conn == false)
{
echo "Koneksi ke server gagal.";
die();
} else echo "Koneksi berhasil";
?>
```

Buka melalui browser untuk menguji koneksi database (untuk menyampaikan pesan koneksi berhasil, _**uncomment**_ pada perintah `echo "koneksi berhasil";`)

![Koneksi_terhubung](img/Koneksi_terhubung.png)

### Membuat File Index Untuk Menampilkan Data (_Read_)

Buat file dengan nama **index.php**

```php
<?php
include("koneksi.php");
// query untuk menampilkan data
$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);
?>

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Data Barang</title>
</head>

<body>
    <div class="container">
        <h1>Data Barang</h1>
        <div class="main">
            <td> <a href="">Tambah Barang</a> </td>
            <table>
                <tr>
                    <th>Gambar</th>
                    <th>Nama Barang</th>
                    <th>Kategori</th>
                    <th>Harga Jual</th>
                    <th>Harga Beli</th>
                    <th>Stok</th>
                    <th>Aksi</th>
                </tr>

                <?php if ($result) : ?> <?php while ($row = mysqli_fetch_array($result)) : ?>
                        <tr>
                            <td>
                                <img src="gambar/
                        <?= $row['gambar']; ?>" alt="
                        <?= $row['nama']; ?>">
                            </td>
                            <td> <?= $row['nama']; ?> </td>
                            <td> <?= $row['kategori']; ?> </td>
                            <td> <?= $row['harga_beli']; ?> </td>
                            <td> <?= $row['harga_jual']; ?> </td>
                            <td> <?= $row['stok']; ?> </td>
                            <td> <?= $row['id_barang']; ?> </td>
                            <td> <a href="">Ubah</a> / <a href="">Hapus</a> </td>
                        </tr> <?php endwhile;
                                    else : ?> <tr>
                        <td colspan="7">Belum ada data</td>
                    </tr> <?php endif; ?>
            </table>
        </div>
    </div>
</body>

</html>
```

![Tabel_Barang_Web](img/Web_Tabel_Barang.png)

### Menambahkan Data (_Create_)

Buat file baru dengan nama **tambah.php**

```php

<?php
error_reporting(E_ALL);
include_once 'koneksi.php';
if (isset($_POST['submit'])) {
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;
    if ($file_gambar['error'] == 0) {
        $filename = str_replace(' ', '_', $file_gambar['name']);
        $destination = dirname(__FILE__) . '/gambar/' . $filename;
        if (move_uploaded_file($file_gambar['tmp_name'], $destination)) {
            $gambar = 'gambar/' . $filename;;
        }
    }
    $sql = 'INSERT INTO data_barang (nama, kategori,harga_jual, harga_beli,
stok, gambar) ';
    $sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}',
'{$harga_beli}', '{$stok}', '{$gambar}')";
    $result = mysqli_query($conn, $sql);
    header('location: index.php');
}
?>
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Tambah Barang</title>
  </head>

  <body>
    <div class="container">
      <h1>Tambah Barang</h1>
      <div class="main">
        <form method="post" action="tambah.php" enctype="multipart/form-data">
          <div class="input">
            <label>Nama Barang</label>
            <input type="text" name="nama" />
          </div>
          <div class="input">
            <label>Kategori</label>
            <select name="kategori">
              <option value="Komputer">Komputer</option>
              <option value="Elektronik">Elektronik</option>
              <option value="Hand Phone">Hand Phone</option>
            </select>
          </div>
          <div class="input">
            <label>Harga Jual</label>
            <input type="text" name="harga_jual" />
          </div>
          <div class="input">
            <label>Harga Beli</label>
            <input type="text" name="harga_beli" />
          </div>
          <div class="input">
            <label>Stok</label>
            <input type="text" name="stok" />
          </div>
          <div class="input">
            <label>File Gambar</label>
            <input type="file" name="file_gambar" />
          </div>
          <div class="submit">
            <input type="submit" name="submit" value="Simpan" />
          </div>
        </form>
      </div>
    </div>
  </body>
</html>
```
![Menambahkan_data](img/tambah_barang.png)

### Mengubah Data (_Update_)

Buat file baru dengan nama **ubah.php**

```php
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';
if (isset($_POST['submit'])) {
    $id = $_POST['id'];
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;
    if ($file_gambar['error'] == 0) {
        $filename = str_replace(' ', '_', $file_gambar['name']);
        $destination = dirname(__FILE__) . '/gambar/' . $filename;
        if (move_uploaded_file($file_gambar['tmp_name'], $destination)) {
            $gambar = 'gambar/' . $filename;;
        }
    }
    $sql = 'UPDATE data_barang SET ';
    $sql .= "nama = '{$nama}', kategori = '{$kategori}', ";
    $sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', stok
= '{$stok}' ";
    if (!empty($gambar))
        $sql .= ", gambar = '{$gambar}' ";
    $sql .= "WHERE id_barang = '{$id}'";
    $result = mysqli_query($conn, $sql);
    header('location: index.php');
}
$id = $_GET['id'];
$sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
if (!$result) die('Error: Data tidak tersedia');
$data = mysqli_fetch_array($result);
function is_select($var, $val)
{
    if ($var == $val) return 'selected="selected"';
    return false;
}
?>
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Ubah Barang</title>
</head>

<body>
    <div class="container">
        <h1>Ubah Barang</h1>
        <div class="main">
            <form method="post" action="ubah.php" enctype="multipart/form-data">
                <div class="input">
                    <label>Nama Barang</label>
                    <input type="text" name="nama" value="<?php echo
                                                            $data['nama']; ?>" />
                </div>
                <div class="input">
                    <label>Kategori</label>
                    <select name="kategori">
                        <option <?php echo is_select('Komputer', $data['kategori']); ?> value="Komputer">Komputer</option>
                        <option <?php echo is_select('Komputer', $data['kategori']); ?> value="Elektronik">Elektronik</option>
                        <option <?php echo is_select('Komputer', $data['kategori']); ?> value="Hand Phone">Hand Phone</option>
                    </select>
                </div>
                <div class="input">
                    <label>Harga Jual</label>
                    <input type="text" name="harga_jual" value="<?php echo
                                                                $data['harga_jual']; ?>" />
                </div>
                <div class="input">
                    <label>Harga Beli</label>
                    <input type="text" name="harga_beli" value="<?php echo
                                                                $data['harga_beli']; ?>" />
                </div>
                <div class="input">
                    <label>Stok</label>
                    <input type="text" name="stok" value="<?php echo
                                                            $data['stok']; ?>" />
                </div>
                <div class="input">
                    <label>File Gambar</label>
                    <input type="file" name="file_gambar" />
                </div>
                <div class="submit">
                    <input type="hidden" name="id" value="<?php echo
                                                            $data['id_barang']; ?>" />
                    <input type="submit" name="submit" value="Simpan" />
                </div>
            </form>
        </div>
    </div>
</body>

</html>
```
Sebelum di tambah 
![Mengubah_barang](img/sebelum_ubah.png)

Setelah di tambah
![Data_di_ubah](img/menambah.png)

### Menghapus Data (_Delete_)

Buat file baru dengan nama **hapus.php**

```php
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>
```
Sebelum

![sebelum_dihapus](img/hapus.png)

Sesudah

![Sudah_dihapus](img/S_hapus.png)