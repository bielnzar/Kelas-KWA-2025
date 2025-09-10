# Juice-Shop: Login Bender

[Akses Challenge disini](https://juice-shop.herokuapp.com/#/score-board?categories=Injection&showDisabledChallenges=false)

## Ringkasan Tantangan
- **Nama:** Login Bender
- **Kategori:** SQL Injection

### Membuat Payload
Untuk masuk sebagai user spesifik, kita perlu mengetahui email atau username-nya. Di Juice Shop, format email user biasanya `namauser@juice-sh.op`. Jadi, email untuk Bender adalah `bender@juice-sh.op`.

Payload yang akan kita gunakan adalah:
```sql
bender@juice-sh.op'--
```

- `bender@juice-sh.op'` akan melengkapi bagian email dalam query.
- `--` (diikuti spasi) akan berfungsi sebagai komentar, membuat sisa query (termasuk pengecekan password) diabaikan.

### 3. Eksekusi Payload
1. Buka halaman login.
2. Masukkan payload `bender@juice-sh.op'-- ` di kolom email.
3. Isi kolom password dengan teks acak.
4. Klik tombol "Log in".

![Image 1](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/login-binder/1.png)

Server akan memproses query yang telah kita manipulasi dan memberikan kita akses ke akun Bender.

![Image 2](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/login-binder/2.png)