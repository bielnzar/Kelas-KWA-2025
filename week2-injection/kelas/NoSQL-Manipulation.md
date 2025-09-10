# Juice-Shop: NoSQL Manipulation

[Akses Challenge disini](https://juice-shop.herokuapp.com/#/score-board?categories=Injection&showDisabledChallenges=false)

## Ringkasan Tantangan
- **Nama:** NoSQL Manipulation
- **Kategori:** NoSQL Injection
- **Tujuan:** Memanipulasi query NoSQL untuk mengubah semua review produk dalam satu kali request.

### 1. Analisis Fitur Update Review
Tantangan ini berfokus pada fitur untuk mengedit review produk. Saat kita mengedit sebuah review, aplikasi mengirimkan request `PATCH` ke server untuk memperbarui data di database. Database yang digunakan kali ini bukan SQL, melainkan NoSQL (MongoDB).

### 2. Menemukan Celah Injeksi
Dengan Burp Suite, kita bisa melihat struktur request `PATCH` yang normal saat mengedit review:
```json
{
  "id": "id_review_spesifik",
  "message": "teks_review_baru"
}
```

![Image 1](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/nosql/1.png)

Celahnya ada pada bagaimana server memproses field `id`. Server tidak menyaring input dengan benar, sehingga kita bisa menyuntikkan operator query MongoDB langsung ke dalam field tersebut.

### 3. Membuat Payload NoSQL Injection
Untuk mengubah *semua* review sekaligus, kita perlu membuat kondisi yang akan cocok dengan semua dokumen review. Kita bisa menggunakan operator MongoDB `$ne` (Not Equal).

Payload yang akan kita gunakan pada field `id`:
```json
{"$ne": null}
```
Artinya: "pilih semua dokumen yang field `id`-nya **tidak sama dengan** `null`". Karena semua review pasti memiliki ID, kondisi ini akan menargetkan semua review yang ada.

### 4. Eksekusi via Burp Suite
1. Edit salah satu review produk untuk menangkap request `PATCH` di Burp Suite.
2. Kirim request tersebut ke Repeater.
3. Ubah nilai `id` dari string biasa menjadi objek JSON payload kita: `{"$ne": null}`.
4. Ubah juga `message` menjadi teks yang kita inginkan, misalnya "PWNED".
5. Kirim request yang sudah dimodifikasi.

![Image 2](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/nosql/2.png)

Setelah request dikirim, server akan menjalankan query yang telah kita manipulasi, dan semua review produk akan berubah sesuai dengan pesan yang kita kirim. Tantangan selesai.

![Image 3](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/nosql/3.png)
![Image 4](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/nosql/4.png)