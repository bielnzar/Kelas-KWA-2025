# Writeup PicoCTF: Irish-Name-Repo 3

[Akses Challenge disini](https://play.picoctf.org/practice/challenge/8?category=1&page=5&search=)

![Image 1](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/irish-3/1.png)

## Ringkasan Tantangan

-   **Nama Challenge**: Irish-Name-Repo 3
-   **Kategori**: Web Exploitation

## Langkah-langkah Eksploitasi

### 1. Analisis Halaman Login

Setelah membuka halaman tantangan, kita menemukan halaman "Admin Login" yang hanya meminta password.

### 2. Menemukan Celah dengan Parameter `debug`

Kita akan menggunakan Burp Suite untuk mencegat dan menganalisis request yang dikirim saat mencoba login.

1.  Masukkan password sembarang dan klik Login sambil mengaktifkan intercept di Burp Suite.
2.  Request yang ditangkap adalah `POST` ke `login.html`. Di dalamnya, kita melihat ada parameter `password` dan `debug=0`.
3.  Kirim request ini ke Repeater (Ctrl+R).
4.  Di Repeater, ubah nilai `debug` dari `0` menjadi `1` dan kirim ulang request.

![Image 2](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/irish-3/2.png)

Dengan mode debug aktif, server akan membocorkan query SQL yang dieksekusi di backend. Ini adalah petunjuk kunci untuk langkah selanjutnya.

### 3. Mengidentifikasi Enkripsi ROT13

Dari respons debug, kita melihat bahwa input password kita diubah. Misalnya, jika kita memasukkan `adminbos`, di dalam query nilainya menjadi `nqzvaobf`.

Setelah dianalisis, pola pergeseran huruf ini adalah enkripsi **ROT13**, di mana setiap huruf digeser sebanyak 13 posisi dalam alfabet (A menjadi N, B menjadi O, dst.).

![Image 3](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/irish-3/3.png)

### 4. Membuat dan Mengirim Payload

Karena input kita dienkripsi, maka payload SQL Injection kita juga harus dienkripsi sebelum dikirim.

-   **Payload Asli**: `' or 1=1 -- `
-   **Payload setelah dienkripsi ROT13**: `' be 1=1 -- `

Kita kirimkan payload yang sudah dienkripsi ini (`' be 1=1 -- `) sebagai nilai parameter `password` melalui Burp Suite Repeater.

![Image 4](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/irish-3/4.png)

### 5. Mendapatkan Flag

Setelah mengirim payload yang benar, otentikasi berhasil dilewati, dan server akan memberikan respons yang berisi flag.

> Flag: `picoCTF{3v3n_m0r3_SQL_4424e7af}`