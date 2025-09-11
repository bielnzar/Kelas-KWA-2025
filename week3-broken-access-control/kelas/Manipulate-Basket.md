# Juice-Shop: Manipulate Basket

[Akses Challenge disini](https://juice-shop.herokuapp.com/#/score-board?categories=Broken%20Access%20Control)

## Overview
- **Nama:** Manipulate Basket
- **Kategori:** Broken Access Control

### 1. Menganalisis Fungsi Keranjang
Saat kita menambahkan produk ke keranjang, aplikasi mengirim request `POST` ke server. Dengan Burp Suite, kita bisa melihat bahwa request ini berisi `ProductId` dan `BasketId` kita. `BasketId` inilah yang menjadi target kita.

### 2. Mencoba Serangan IDOR Sederhana
Percobaan pertama adalah mengganti `BasketId` kita dengan `BasketId` milik user lain (misalnya, user B). Ternyata gagal. Server memiliki mekanisme keamanan yang memeriksa apakah `BasketId` tersebut milik kita.

![Image 1](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week3-broken-access-control/kelas/images/manipulate/1.png)

### 3. Bypass Keamanan dengan Parameter Overloading
Di sinilah triknya. Kita bisa mengelabui server dengan mengirimkan parameter `BasketId` **dua kali** dalam satu request JSON. Teknik ini disebut **Parameter Overloading** atau **HTTP Parameter Pollution**.

Payload JSON yang akan kita gunakan:
```json
{
    "ProductId": 1,
    "BasketId": "1", // ID Keranjang kita (untuk lolos pengecekan)
    "BasketId": "2", // ID Keranjang target (untuk eksekusi)
    "quantity": 1
}
```
Server akan memprosesnya seperti ini:
1.  **Pengecekan Keamanan:** Server hanya membaca `BasketId` yang pertama (`"1"`), memvalidasinya, dan memberikan izin.
2.  **Eksekusi Aksi:** Saat akan menambahkan produk, server justru menggunakan `BasketId` yang kedua (`"2"`).

![Image 2](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week3-broken-access-control/kelas/images/manipulate/2.png)

### 4. Eksekusi via Burp Suite
1. Tambahkan produk apa saja ke keranjang untuk menangkap request-nya.
2. Kirim request ke Repeater.
3. Modifikasi body JSON dengan payload di atas (sesuaikan `BasketId` dengan ID kita dan ID target).
4. Kirim request.

![Image 3](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week3-broken-access-control/kelas/images/manipulate/3.png)

Jika berhasil, produk akan ditambahkan ke keranjang belanja user lain.
