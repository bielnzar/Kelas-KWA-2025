# Juice-Shop: Login Jim

[Akses Challenge disini](https://juice-shop.herokuapp.com/#/score-board?categories=Injection&showDisabledChallenges=false)

## Ringkasan Tantangan
- **Nama:** Login Jim
- **Kategori:** SQL Injection

### Membuat Payload
Kita perlu membuat payload yang spesifik untuk akun Jim. Dengan asumsi format email adalah `namauser@juice-sh.op`, maka email untuk Jim adalah `jim@juice-sh.op`.

Payload yang akan kita gunakan:
```sql
jim@juice-sh.op'--
```
- `jim@juice-sh.op'` digunakan untuk mencocokkan email user Jim.
- `--` (diikuti spasi) akan mengomentari sisa query, sehingga validasi password akan dilewati.

### Eksekusi Payload
1. ke halaman login.
2. Masukkan payload `jim@juice-sh.op'-- ` pada kolom email.
3. Masukkan password apa saja.
4. Klik tombol "Log in".

![Image 1](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/login-jim/1.png)

Setelah dieksekusi, kita akan berhasil masuk ke akun Jim dan menyelesaikan tantangan.

![Image 2](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/login-jim/2.png)