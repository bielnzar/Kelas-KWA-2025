# Juice-Shop: View Basket

[Akses Challenge disini](https://juice-shop.herokuapp.com/#/score-board?categories=Broken%20Access%20Control&showDisabledChallenges=false)

## Overview
- **Nama:** View Basket
- **Kategori:** Broken Access Control

### 1. Menganalisis Cara Kerja Keranjang Belanja
Saat kita melihat isi keranjang belanja kita, aplikasi akan membuat request ke sebuah API endpoint untuk mengambil datanya. Dengan menggunakan Burp Suite atau Developer Tools di browser, kita bisa melihat request ini.

Request yang dikirim adalah request `GET` ke alamat seperti:
`/rest/basket/1`

Angka `1` di akhir URL adalah **ID keranjang belanja** kita.

![Image 2](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week3-broken-access-control/kelas/images/view/2.png)
![Image 3](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week3-broken-access-control/kelas/images/view/3.png)

### 2. Mengeksploitasi Celah
Celah keamanan di sini adalah **Insecure Direct Object Reference (IDOR)**. Aplikasi tidak memeriksa apakah kita berhak melihat keranjang dengan ID tersebut. Kita bisa langsung memanipulasi ID di URL untuk melihat keranjang orang lain.

### 3. Mengakses Keranjang User Lain
1. Buka tab baru di browser atau gunakan Burp Suite Repeater.
2. Akses URL yang sama, tetapi ganti ID keranjang kita (`1`) dengan ID lain, misalnya `5`.
   `/rest/basket/5`
3. Server akan merespons dengan isi keranjang milik user dengan ID `5`.

![Image 1](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week3-broken-access-control/kelas/images/view/1.png)
![Image 4](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week3-broken-access-control/kelas/images/view/4.png)

Dengan begitu, kita berhasil melihat data pribadi (isi keranjang) milik pengguna lain.

