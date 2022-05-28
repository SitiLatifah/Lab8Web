# Lab8Web
**Nama	 : Siti Latifah** <br>
**NIM	   : 312010321** <br>
**Kelas	 : TI.20.A2** <br>
**Matkul : Pemrograman Web** <br>

# PHP DAN DATABASE MYSQL
## MEenjalankan MYSQL Server
1. Pastikan MySQL Server sudah dapat dijalankan melalui XAMPP.

![Screenshot (289)_LI](https://user-images.githubusercontent.com/73010098/170829921-222d1b9a-02c7-4591-be24-85f4b6e86dbd.jpg)
2. Pastikan webserver Apache dan MySQL server sudah dijalankan. Kemudian buka melalui browser: http://localhost
3. dan simpan file website tempatkan di direktori: \xampp\htdocs\

## Membuat Database 
Disini saya membuatnya menggunakan CMD <br>
![Screenshot (288)](https://user-images.githubusercontent.com/73010098/170830157-fd5aae6a-e3f2-4826-ba5c-0b7feb0c3bf8.png)

## Membuat Table
![Screenshot (289)](https://user-images.githubusercontent.com/73010098/170830218-210cf505-87b2-444e-9851-f343b804d7d8.png)

## Menambahkan Data
![Screenshot (290)](https://user-images.githubusercontent.com/73010098/170830405-aed3862a-bbc6-4fcd-9e0b-819e4c878ce0.png)

# MEMBUAT PROGRAM CRUD
Buat folder Lab8Web pada root directory web server (d:\xampp\htdocs)
Kemudian untuk mengakses direktory tersebut pada web server dengan mengakses URL:http://localhost/Lab8Web/

![Screenshot (291)](https://user-images.githubusercontent.com/73010098/170830693-26acb168-a4e9-4288-8db0-99839dbc2e8a.png)

## Membuat file koneksi database
Buat File Baru dengan nama konek.php
``` php
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
} #else echo "Koneksi berhasil";
?>
```
kemudian untuk mengakses hasilnya melalui URL:http://localhost/Lab8Web/konek.php
Buka melalui browser untuk menguji koneksi database untuk menyampilkan pesan koneksi berhasil, uncomment pada perintah echo “koneksi berhasil”;

![Screenshot (293)](https://user-images.githubusercontent.com/73010098/170830945-a9a19a1d-d428-45fb-8044-80758e643cc6.png)
## Membuat file index untuk menampilkan data (Read)
Buat file baru dengan nama index.php
``` php
<?php
include("konek.php");

// query untuk menampilkan data
$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);

?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Data Barang</title>
</head>
<body>
    <div class="container">
        <h1>Data Barang</h1>
        <div>
        <a href="tambah.php">Tambah Barang</a>
        </div>
        <br>
        <div class="main">
            <table>
                <tr>
                    <th>NO</th>
                    <th>Gambar</th>
                    <th>Nama Barang</th>
                    <th>Kategori</th>
                    <th>Harga Jual</th>
                    <th>Harga Beli</th>
                    <th>Stok</th>
                    <th>Aksi</th>
                </tr>
                <?php if($result): ?>
                <?php while($row = mysqli_fetch_array($result)): ?>
                <tr>
                    <td><?= $row['id_barang'];?></td>
                    <td><img src="gambar/<?= $row['gambar'];?>" alt="<?=$row['nama'];?>" width="100"></td>
                    <td><?= $row['nama'];?></td>
                    <td><?= $row['kategori'];?></td>
                    <td><?= $row['harga_beli'];?></td>
                    <td><?= $row['harga_jual'];?></td>
                    <td><?= $row['stok'];?></td>
                    <td>
                      <a class="ubah" href="ubah.php?id_barang=<?php echo $row['id_barang']; ?>">Ubah</a>
                      <a class="hapus" href="hapus.php?id_barang=<?php echo $row['id_barang']; ?>">Hapus</a>
                    </td>
                </tr>
                <?php endwhile; else: ?>
                <tr>
                    <td colspan="7">Belum ada data</td>
                </tr>
                <?php endif; ?>
            </table>
       </div>
     </div>
</body>
</html>
```
kemudian membuat file dengan nama style_index.css untuk memperindah tampilan, Sintaxnya seperti berikut.

``` css
.container {
  margin: 30px 20px;
  background-color: #f0f0f0;
  font-family: 'Times New Roman', Times, serif;
  border-radius: 5px;
  border-collapse: collapse;
}

h1 {
  padding-top: 40px;
  padding-left: 40px;
}

a {
  padding-left: 40px;
}

.main {
  padding-left: 40px;
  padding-top: 20px;
  padding-bottom: 40px;
  padding-right: 40px;
}

table {
  width: 100%;
  border-radius: 5px;
  font-family: 'Times New Roman', Times, serif;
  border-collapse: collapse;
}

th {
  height: 35px;
  text-align: center;
  padding-top: 12px;
  padding-bottom: 12px;
  background-color: #5f9ea0;
  color: black;
}

td {
  height: 100px;
  text-align: center;
  border: 1px solid #ddd;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}
tr:hover {
  background-color: #ddd;
}
```
## OUTPUT
![Screenshot (295)](https://user-images.githubusercontent.com/73010098/170834604-646d9651-c728-4afd-9536-0a915cab2486.png)

## Menambah Data (Create)
Buat file baru dengan nama tambah.php
``` php
<?php
error_reporting(E_ALL);
include_once 'konek.php';
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
      $gambar =  $filename;;
    }
  }
  $sql = 'INSERT INTO data_barang (nama, kategori,harga_jual, harga_beli, stok, gambar) ';
  $sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}', '{$harga_beli}', '{$stok}', '{$gambar}')";
  $result = mysqli_query($conn, $sql);
  
  header('location: index.php');
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="style_tambah.css" rel="stylesheet" type="text/css" />
    <title>Tambah Barang</title>
</head>
<body>
    <div class="container">
        <h1>Tambah Barang</h1>
        <div class="main">
            <form method="post" action="tambah.php" enctype="multipart/form-data">
                <div class="input">
                    <label>Nama Barang</label>
                    <input class="nama" type="text" name="nama"/>
                </div>
                <div class="input">
                    <label>Kategori</label>
                    <select class="kategori" name="kategori">
                        <option value="Komputer">Komputer</option>
                        <option value="Elektronik">Elektronik</option>
                        <option value="Handphone">Handphone</option>
                    </select>
                </div>
                <div class="input">
                    <label>Harga Jual</label>
                    <input class="harga_jual" type="text" name="harga_jual"/>
                </div>
                <div class="input">
                    <label>Harga Beli</label>
                    <input class="harga_beli" type="text" name="harga_beli"/>
                </div>
                <div class="input">
                    <label>Stok</label>
                    <input class="stok" type="text" name="stok"/>
                </div>
                <div class="input">
                    <label>File Gambar</label>
                    <input class="file" type="file" name="file_gambar"/>
                </div>
                <div class="submit">
                    <input class="button" type="submit" name="submit" value="Simpan"/>
                </div>
            </form>
        </div>
    </div> 
</body>
</html>
```
kemudian membuat file dengan nama style_tambah.css untuk memperindah tampilan, Sintaxnya seperti berikut
`` css
.container {
  margin: 30px 20px;
  background-color: #f0f0f0;
  font-family: 'Times New Roman', Times, serif;
  border-radius: 5px;
}

h1 {
  padding-top: 40px;
  padding-left: 40px;
}

.main {
  padding-left: 40px;
  padding-top: 10px;
  padding-bottom: 40px;
  padding-right: 40px;
}

.input {
  padding-top: 20px;
}

.nama {
  margin-left: 20px;
}

.kategori {
  margin-left: 51px;
}

.harga_jual {
  margin-left: 38px;
}

.harga_beli {
  margin-left: 38px;
}

.stok {
  margin-left: 76px;
}

.file {
  margin-left: 25px;
}

.submit {
  padding-top: 30px;
}

label {
  padding: 8px 8px 8px 0;
  display: inline-block;
}

.button:hover {
  opacity: 1;
}
```
## OUTPUT
![Screenshot (297)](https://user-images.githubusercontent.com/73010098/170835374-d560dd5d-8d20-4e6a-9d18-b890855b808a.png)





