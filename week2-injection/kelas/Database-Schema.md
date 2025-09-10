# Juice-Shop: Database Schema

[Akses Challenge disini](https://juice-shop.herokuapp.com/#/score-board?categories=Injection&showDisabledChallenges=false)

## Ringkasan Tantangan
- **Nama:** Database Schema
- **Kategori:** Information Disclosure / SQL Injection

### Menemukan Titik Injeksi
Celah keamanan untuk tantangan ini berada pada fitur pencarian produk. Parameter `q` pada URL pencarian rentan terhadap SQL Injection, yang bisa kita manfaatkan untuk mengintip isi database.

![Image 1](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/database/1.png)

#### Kenapa Endpoint-nya `/rest/products/search`
padahal URL di browser adalah `/#/search?q=`.

Ini karena aplikasi web modern memisahkan antara **(frontend)** dan **(backend)**.
- **`/#/search?q=`**: Ini adalah URL frontend. Fungsinya hanya untuk menampilkan halaman pencarian di browser kita
- **`/rest/products/search?q=`**: Ini adalah **API Endpoint** backend. Ketika kita mencari sesuatu, frontend akan mengirim request ke alamat ini. Server di alamat inilah yang benar-benar menjalankan query ke database

Jadi, untuk melakukan injeksi, kita harus menargetkan API Endpoint-nya, bukan URL yang kita lihat di browser.

### Membuat Payload Injeksi
Kita akan menggunakan serangan `UNION SELECT` untuk menggabungkan hasil query kita dengan hasil pencarian produk. Tujuannya adalah untuk mengambil data dari tabel `sqlite_schema`, yang berisi informasi tentang semua tabel di database SQLite.

Payload yang akan kita gunakan:
```sql
test')) UNION SELECT 1, 2, 3, 4, 5, 6, 7, 8, sql FROM sqlite_schema--
```

- `'))` digunakan untuk menutup query pencarian yang ada.
- `UNION SELECT ...` menggabungkan hasil. Kita perlu menebak jumlah kolom yang benar (dalam kasus ini 9 kolom).
- `sql FROM sqlite_schema` akan mengambil informasi skema dari kolom `sql` di tabel `sqlite_schema`.
- `--` mengomentari sisa query asli.

### Mengeksekusi Payload

![Image 2](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/database/2.png)

Dengan payload ini, kita bisa melihat seluruh struktur database, termasuk nama-nama tabel dan kolom yang ada. Informasi ini sangat berharga untuk tantangan-tantangan selanjutnya.