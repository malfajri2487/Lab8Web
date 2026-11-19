## Identitas Mahasiswa

| Keterangan      | Data             |
| --------------- | ---------------- |
| **Nama**        | Muhammad Al Fajri|
| **Kelas**       | I251D            |
| **NIM**         | 312510353        |
| **Mata Kuliah** | Pemrograman Web  |

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

<img width="493" height="314" alt="Screenshot 2026-11-19 140143" src="https://github.com/user-attachments/assets/ea281c42-1a3a-4814-9fec-1ee584e27f23" />


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

<img width="641" height="126" alt="Screenshot 2026-11-19 140417" src="https://github.com/user-attachments/assets/e1c6d28c-3c2a-407e-b13d-60f58cac0cb0" />

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

<img width="699" height="163" alt="image" src="https://github.com/user-attachments/assets/a90ce7be-aad8-4e7c-8846-4951d977fb0d" />

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

<img width="291" height="88" alt="Screenshot 2026-11-19 110055" src="https://github.com/user-attachments/assets/63ce36ca-6371-4cb8-a468-2e04ce4251a3" />


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

<img width="959" height="400" alt="Screenshot 2026-11-19 131314" src="https://github.com/user-attachments/assets/7e1a84a5-0b88-4c11-9fa3-5e5ad684e8e4" />


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

<img width="294" height="247" alt="Screenshot 2026-11-19 132315" src="https://github.com/user-attachments/assets/331bf15f-6240-44ab-86b1-0b94c13915b2" />


### Contoh Pengisian

Contoh data yang dapat dimasukkan:

```text
Nama Barang : Realme
Kategori    : Hand Phone
Harga Jual  : 2000000
Harga Beli  : 2500000
Stok        : 5
```

<img width="302" height="298" alt="Screenshot 2026-11-19 133429" src="https://github.com/user-attachments/assets/99538b06-a257-4988-bcdb-40f6c4aaeb4d" />


### Hasil Setelah Disimpan

Setelah tombol **Simpan** ditekan, data baru akan muncul pada tabel `Data Barang`.

<img width="825" height="445" alt="Screenshot 2026-11-19 133510" src="https://github.com/user-attachments/assets/b1b2f7ad-f757-4675-99e9-5323169c0d0b" />


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

<img width="430" height="306" alt="Screenshot 2026-11-19 132604" src="https://github.com/user-attachments/assets/69e50289-96a2-4544-a6ca-1e9a2c47ed54" />


### Contoh Perubahan Data

Contohnya, mengubah:

```text
Nama Barang : Realme
Kategori    : Hand Phone
Stok : 5
```
<img width="808" height="417" alt="Screenshot 2026-11-19 141543" src="https://github.com/user-attachments/assets/2fbd0d2e-ef37-4a41-90bd-1282d1ca9f22" />


menjadi:

```text
Nama Barang : Realme
Kategori    : Hand Phone
Stok : 3
```

<img width="314" height="307" alt="Screenshot 2026-11-19 141339" src="https://github.com/user-attachments/assets/ef6d29b3-c3e6-491d-8851-92aedb5e3539" />
<img width="830" height="488" alt="Screenshot 2026-11-19 141354" src="https://github.com/user-attachments/assets/a0193b59-14e8-4530-8527-f2852355a47a" />


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

<img width="830" height="488" alt="Screenshot 2026-11-19 141354" src="https://github.com/user-attachments/assets/8dbadadc-d58c-42a5-a922-04983f12a168" />


### Konfirmasi Hapus

Jika fitur konfirmasi sudah diterapkan, saat tombol **Hapus** diklik akan muncul pesan konfirmasi.

Pengguna dapat memilih:

```text
OK
```
<img width="851" height="473" alt="Screenshot 2026-11-19 141722" src="https://github.com/user-attachments/assets/6656b33d-bd08-4ea2-8f81-67278a835940" />


### Hasil Setelah Dihapus

Setelah penghapusan berhasil, data tersebut tidak lagi muncul pada tabel `Data Barang`.

<img width="839" height="436" alt="Screenshot 2026-11-19 141747" src="https://github.com/user-attachments/assets/55919032-ef74-476e-91bb-ab44b69bbd30" />


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
