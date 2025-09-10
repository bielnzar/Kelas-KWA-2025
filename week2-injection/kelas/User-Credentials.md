# Juice-Shop: User Credentials

[Akses Challenge disini](https://juice-shop.herokuapp.com/#/score-board?categories=Injection&showDisabledChallenges=false)

## Ringkasan Tantangan
- **Nama:** User Credentials
- **Kategori:** SQL Injection
- **Tujuan:** Mengekstrak data sensitif (username, email, dan hash password) semua pengguna dari database.

### 1. Menemukan Titik Injeksi
Setelah mencoba beberapa tempat, kita menemukan bahwa celah SQL Injection yang paling berguna ada di fitur **pencarian produk**, sama seperti beberapa tantangan sebelumnya.

### 2. Menentukan Struktur Query (Jumlah Kolom)
Untuk bisa mengambil data dari tabel lain (`Users`), kita perlu menggunakan serangan `UNION SELECT`. Langkah pertama adalah mencari tahu berapa jumlah kolom yang diambil oleh query pencarian asli. Melalui trial-and-error (atau dari tantangan "Database Schema"), kita tahu bahwa query tersebut mengambil **9 kolom**.

![Image 1](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/user/1.png)

### 3. Membuat Payload untuk Mengekstrak Data
Sekarang kita bisa membuat payload untuk mengambil data dari tabel `Users` dan menampilkannya seolah-olah itu adalah hasil pencarian produk. Kita harus memilih 9 kolom dari tabel `Users` untuk dicocokkan.

Payload final yang akan kita gunakan:
```sql
')) UNION SELECT username,email,password,role,deluxeToken,lastLoginIp,profileImage,totpSecret,id FROM Users--
```
- `'))` untuk menutup query pencarian yang asli.
- `UNION SELECT ... FROM Users` untuk mengambil data dari tabel `Users`. Kita memilih kolom-kolom seperti `username`, `email`, dan `password` untuk ditampilkan.
- `--` untuk mengomentari sisa query.

### 4. Eksekusi dan Mendapatkan Kredensial
Masukkan payload di atas ke dalam kolom pencarian. Hasilnya, aplikasi akan menampilkan seluruh data pengguna, termasuk username, email, dan hash password mereka, langsung di halaman hasil pencarian.

**Penting:** Karena kita "meminjam" tampilan hasil pencarian produk, data pengguna akan dipetakan ke kolom produk. Berdasarkan payload kita:
- `username` akan muncul sebagai `id` produk.
- `email` akan muncul sebagai `name` produk.
- **`password` (hash) akan muncul sebagai `description` produk.**

![Image 2](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/user/2.png)
![Image 3](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/user/3.png)

Dengan tereksposnya data ini, tantangan pun berhasil diselesaikan.

![Image 4](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/user/4.png)