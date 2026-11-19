# Praktikum Lab 8 – PHP & Database (CRUD Data Barang)

## Identitas Mahasiswa

| Keterangan      | Data             |
| --------------- | ---------------- |
| **Nama**        | [Isi Nama Kamu]  |
| **Kelas**       | [Isi Kelas]      |
| **NIM**         | [Isi NIM]        |
| **Mata Kuliah** | Pemrograman Web  |
| **Dosen**       | [Isi Nama Dosen] |

## Deskripsi

Praktikum Lab 8 membuat aplikasi web sederhana menggunakan **PHP** dan **MySQL** untuk mengelola data barang. Aplikasi menerapkan konsep **CRUD (Create, Read, Update, Delete)** sehingga pengguna dapat menambah, menampilkan, mengubah, dan menghapus data barang.

Fitur yang dibuat meliputi:

- **Create** – menambahkan data barang melalui `tambah.php`
- **Read** – menampilkan data barang melalui `index.php`
- **Update** – mengubah data barang melalui `ubah.php`
- **Delete** – menghapus data barang melalui `hapus.php`

**Alat yang digunakan:** XAMPP, Apache, MySQL/MariaDB, PHP, Visual Studio Code, dan Google Chrome.

---

## Struktur File

```text
lab8_php_database/
├── koneksi.php      # koneksi PHP ke database
├── index.php        # menampilkan data barang
├── tambah.php       # form dan proses tambah data
├── ubah.php         # form dan proses ubah data
├── hapus.php        # proses hapus data
├── style.css        # pengaturan tampilan
└── gambar/          # folder penyimpanan gambar
```

---

## Langkah 1 – Menjalankan Apache dan MySQL di XAMPP

Buka **XAMPP Control Panel**, kemudian jalankan **Apache** dan **MySQL** dengan menekan tombol **Start**.

Apache digunakan sebagai web server untuk menjalankan file PHP, sedangkan MySQL digunakan sebagai database untuk menyimpan data barang.

Pastikan Apache dan MySQL sudah berstatus **Running** sebelum menjalankan project melalui browser.

**Screenshot:**

> Tambahkan screenshot XAMPP Apache dan MySQL yang sudah Running di sini.

---

## Langkah 2 – Membuat Database dan Tabel

Database yang digunakan dalam praktikum ini bernama `latihan1`, sedangkan tabel yang digunakan adalah `data_barang`.

Struktur tabel:

```sql
CREATE DATABASE latihan1;

USE latihan1;

CREATE TABLE data_barang (
    id_barang int(10) auto_increment PRIMARY KEY,
    kategori varchar(30),
    nama varchar(30),
    gambar varchar(100),
    harga_beli decimal(10,0),
    harga_jual decimal(10,0),
    stok int(4)
);
```

Data awal yang digunakan:

```sql
INSERT INTO data_barang
(kategori, nama, gambar, harga_beli, harga_jual, stok)
VALUES
('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```

Untuk melihat data:

```sql
SELECT * FROM data_barang;
```

**Penjelasan:**

- `CREATE DATABASE latihan1` digunakan untuk membuat database.
- `CREATE TABLE data_barang` digunakan untuk membuat tabel data barang.
- `id_barang` digunakan sebagai primary key dan bertambah otomatis.
- `INSERT INTO` digunakan untuk memasukkan data awal.
- `SELECT * FROM data_barang` digunakan untuk melihat seluruh isi tabel.

**Screenshot:**

> Tambahkan screenshot database `latihan1` dan tabel `data_barang` dari phpMyAdmin di sini.

---

## Langkah 3 – Membuat Folder Project

Buat folder project di:

```text
C:\xampp\htdocs\lab8_php_database
```

Semua file PHP, CSS, dan folder gambar disimpan di dalam folder tersebut.

Project kemudian dapat dijalankan melalui:

```text
http://localhost/lab8_php_database/
```

**Screenshot:**

> Tambahkan screenshot folder project di dalam `htdocs`.

---

## Langkah 4 – Membuat Koneksi Database (`koneksi.php`)

File `koneksi.php` digunakan untuk menghubungkan aplikasi PHP dengan database `latihan1`.

Kode koneksi:

```php
<?php

$host = "localhost";
$user = "root";
$pass = "";
$db   = "latihan1";

$conn = mysqli_connect($host, $user, $pass, $db);

if ($conn == false)
{
    echo "Koneksi ke server gagal.";
    die();
}

?>
```

**Penjelasan:**

- `$host` berisi alamat server database yaitu `localhost`.
- `$user` berisi username MySQL yaitu `root`.
- `$pass` berisi password MySQL.
- `$db` berisi nama database yaitu `latihan1`.
- `mysqli_connect()` digunakan untuk membuat koneksi ke database.
- Jika koneksi gagal, program akan menampilkan pesan error dan dihentikan.

**Screenshot:**

> Tambahkan screenshot ketika `koneksi.php` berhasil dijalankan.

---

## Langkah 5 – Menampilkan Data (`index.php`)

File `index.php` digunakan untuk menampilkan seluruh data yang terdapat pada tabel `data_barang`.

Bagian penting dari program:

```php
include("koneksi.php");

$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);
```

Kode tersebut mengambil seluruh data dari tabel `data_barang`.

Kemudian data ditampilkan menggunakan perulangan:

```php
while($row = mysqli_fetch_array($result)):
```

Setiap data dimasukkan ke dalam baris tabel HTML.

Kolom yang ditampilkan meliputi:

- Gambar
- Nama Barang
- Kategori
- Harga Jual
- Harga Beli
- Stok
- Aksi

Pada bagian **Aksi** terdapat link:

```text
Ubah
Hapus
```

Link **Tambah Barang** digunakan untuk menuju halaman `tambah.php`.

**Screenshot:**

> Tambahkan screenshot tampilan `index.php` yang menampilkan Data Barang.

---

## Langkah 6 – Menambah Data (`tambah.php`)

File `tambah.php` digunakan untuk membuat form penambahan data barang.

Form memiliki beberapa input:

- Nama Barang
- Kategori
- Harga Jual
- Harga Beli
- Stok
- File Gambar

Form menggunakan:

```html
<form method="post" action="tambah.php" enctype="multipart/form-data"></form>
```

`multipart/form-data` diperlukan karena form digunakan untuk mengunggah file gambar.

Saat tombol **Simpan** ditekan, data diproses menggunakan `$_POST`, sedangkan file gambar diproses menggunakan `$_FILES`.

Data kemudian dimasukkan ke database menggunakan query `INSERT`.

Setelah berhasil disimpan, halaman diarahkan kembali ke:

```text
index.php
```

**Screenshot:**

> Tambahkan screenshot halaman `Tambah Barang`.

### Contoh Pengisian

Contoh data yang dapat dimasukkan:

```text
Nama Barang : Laptop ASUS
Kategori    : Komputer
Harga Jual  : 7000000
Harga Beli  : 6000000
Stok        : 3
```

**Screenshot:**

> Tambahkan screenshot form yang sudah diisi sebelum menekan Simpan.

### Hasil Setelah Disimpan

Setelah tombol **Simpan** ditekan, data baru akan muncul pada tabel `Data Barang`.

**Screenshot:**

> Tambahkan screenshot data baru yang sudah muncul di `index.php`.

---

## Langkah 7 – Mengubah Data (`ubah.php`)

File `ubah.php` digunakan untuk mengubah data barang yang sudah tersimpan.

Saat pengguna memilih **Ubah**, ID barang dikirim melalui URL, contohnya:

```text
ubah.php?id=1
```

Program kemudian mengambil data berdasarkan `id_barang`.

```php
$sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
```

Data lama kemudian ditampilkan kembali pada form agar dapat diedit.

Setelah tombol **Simpan** ditekan, data diperbarui menggunakan query:

```sql
UPDATE data_barang
SET ...
WHERE id_barang = ...
```

Setelah proses selesai, halaman kembali ke `index.php`.

**Screenshot:**

> Tambahkan screenshot halaman Ubah Barang sebelum diedit.

### Contoh Perubahan Data

Contohnya, mengubah:

```text
Stok : 3
```

menjadi:

```text
Stok : 5
```

**Screenshot:**

> Tambahkan screenshot setelah data berhasil diubah.

---

## Langkah 8 – Menghapus Data (`hapus.php`)

File `hapus.php` digunakan untuk menghapus data barang berdasarkan `id_barang`.

Kode utama:

```php
<?php

include_once 'koneksi.php';

$id = $_GET['id'];

$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";

$result = mysqli_query($conn, $sql);

header('location: index.php');

?>
```

**Penjelasan:**

- `$_GET['id']` mengambil ID barang dari URL.
- Query `DELETE` digunakan untuk menghapus data.
- Setelah data dihapus, halaman diarahkan kembali ke `index.php`.

**Screenshot:**

> Tambahkan screenshot sebelum data dihapus.

### Konfirmasi Hapus

Jika fitur konfirmasi sudah diterapkan, saat tombol **Hapus** diklik akan muncul pesan konfirmasi.

Pengguna dapat memilih:

```text
OK
```

untuk melanjutkan penghapusan atau:

```text
Batal
```

untuk membatalkan.

**Screenshot:**

> Tambahkan screenshot kotak konfirmasi penghapusan.

### Hasil Setelah Dihapus

Setelah penghapusan berhasil, data tersebut tidak lagi muncul pada tabel `Data Barang`.

**Screenshot:**

> Tambahkan screenshot hasil setelah data berhasil dihapus.

---

## Hasil Akhir

Setelah seluruh proses CRUD selesai, aplikasi dapat melakukan beberapa fungsi utama:

| Fitur    | File          | Fungsi                         |
| -------- | ------------- | ------------------------------ |
| Read     | `index.php`   | Menampilkan data barang        |
| Create   | `tambah.php`  | Menambah data barang           |
| Update   | `ubah.php`    | Mengubah data barang           |
| Delete   | `hapus.php`   | Menghapus data barang          |
| Database | `koneksi.php` | Menghubungkan PHP dengan MySQL |
| Tampilan | `style.css`   | Mengatur tampilan aplikasi     |

---

## Kesimpulan

Praktikum Lab 8 memberikan pemahaman mengenai penggunaan PHP untuk terhubung dengan database MySQL serta penerapan operasi CRUD pada aplikasi web. Melalui praktikum ini, dapat dipahami cara menampilkan data dari database, menambahkan data baru, mengubah data yang sudah ada, dan menghapus data.

Selain CRUD, praktikum juga memperkenalkan proses upload gambar menggunakan PHP dan pengelolaan file pada folder project. Dengan adanya `koneksi.php`, `index.php`, `tambah.php`, `ubah.php`, dan `hapus.php`, seluruh proses pengelolaan data barang dapat dilakukan melalui aplikasi web.
