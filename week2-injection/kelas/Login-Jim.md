# Juice-Shop: Login Jim

[Akses Challenge disini](https://juice-shop.herokuapp.com/#/score-board?categories=Injection&showDisabledChallenges=false)

## Ringkasan Tantangan
- **Nama:** Login Jim
- **Kategori:** SQL Injection
- **Tujuan:** Masuk sebagai user "Jim" dengan SQL Injection.

## Langkah-langkah Penyelesaian

### 1. Analisis Target
Sama seperti tantangan sebelumnya, kita akan mengeksploitasi celah SQL Injection di halaman login. Kali ini, target kita adalah akun milik **Jim**.

### 2. Membuat Payload
Kita perlu membuat payload yang spesifik untuk akun Jim. Dengan asumsi format email adalah `namauser@juice-sh.op`, maka email untuk Jim adalah `jim@juice-sh.op`.

Payload yang akan kita gunakan:
```sql
jim@juice-sh.op'--
```
- `jim@juice-sh.op'` digunakan untuk mencocokkan email user Jim.
- `--` (diikuti spasi) akan mengomentari sisa query, sehingga validasi password akan dilewati.

### 3. Eksekusi Payload
1. Pergi ke halaman login.
2. Masukkan payload `jim@juice-sh.op'-- ` pada kolom email.
3. Masukkan password apa saja (acak).
4. Klik tombol "Log in".

![Image 1](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/login-jim/1.png)

Setelah dieksekusi, kita akan berhasil masuk ke akun Jim dan menyelesaikan tantangan.

![Image 2](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/login-jim/2.png)