# Juice-Shop: Login Admin

[Akses Challenge disini](https://juice-shop.herokuapp.com/#/score-board?categories=Injection&showDisabledChallenges=false)

## Ringkasan Tantangan
- **Nama:** Login Admin
- **Kategori:** SQL Injection

## Ikuti tutorial yang diarahkan oleh website

### Membuat Payload SQL Injection
Kita akan memodifikasi payload JSON pada bagian `email`. Payload yang akan kita gunakan adalah:
```sql
admin@juice-sh.op' OR '1'='1' --
```

![Image 1](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/login-admin/1.png)

Payload ini akan membuat kondisi query SQL di server **selalu bernilai benar** (`true`), sehingga kita bisa login tanpa perlu password yang valid.

### Eksekusi dan Login
Request yang sudah dimodifikasi tadi kita kirim ulang ke server. Karena kondisi `OR '1'='1'` selalu benar, server akan mengira proses otentikasi berhasil dan mengizinkan kita masuk sebagai admin.

![Image 2](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/login-admin/2.png)