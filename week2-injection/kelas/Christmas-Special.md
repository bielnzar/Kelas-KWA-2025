# Juice-Shop: Christmas Special

[Akses Challenge disini](https://juice-shop.herokuapp.com/#/score-board?categories=Injection&showDisabledChallenges=false)

## Ringkasan Tantangan
- **Nama:** Christmas Special
- **Kategori:** SQL Injection

### Titik Injeksi
Tantangan ini mengharuskan kita untuk menemukan produk yang tidak ditampilkan di halaman utama. Hipotesis kita adalah produk tersebut tidak benar-benar dihapus, melainkan hanya ditandai sebagai "dihapus" di database (kolom `deletedAt`-nya tidak kosong).

Celah yang akan kita manfaatkan sama seperti sebelumnya, yaitu fitur pencarian produk yang rentan terhadap SQL Injection.

### Mencari Produk yang "Dihapus"
Kita akan menggunakan payload `UNION SELECT` untuk mencari semua produk yang kolom `deletedAt`-nya tidak `NULL`.

Payload yang digunakan:
```sql
')) UNION SELECT * FROM Products WHERE deletedAt IS NOT NULL--
```
- `'))` untuk menutup query asli.
- `UNION SELECT * FROM Products` untuk mengambil semua kolom dari tabel `Products`.
- `WHERE deletedAt IS NOT NULL` adalah kondisi untuk mencari produk yang sudah ditandai terhapus.
- `--` untuk mengomentari sisa query.

### Menemukan Product ID
Setelah mengeksekusi payload di kolom pencarian, kita akan melihat produk "Christmas Surprise" muncul. Dari hasil ini, kita mengetahui bahwa `ProductId` untuk produk tersebut adalah **10**.

![Image 1](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/christmas-special/1.png)

### Menambahkan Produk ke Keranjang
Karena produk ini tidak bisa ditambahkan langsung, kita perlu trik:
1. Tambahkan produk lain (apa saja) ke keranjang belanja.
2. Gunakan Burp Suite untuk mencegat request `POST` yang dikirim ke `/api/BasketItems/`.
3. Ubah nilai `ProductId` di dalam request menjadi **10**.
4. Kirim (Forward) request yang sudah dimodifikasi.

![Image 2](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/christmas-special/2.png)

Setelah itu, produk spesial Natal akan masuk ke keranjang belanja kita dan tantangan pun selesai.