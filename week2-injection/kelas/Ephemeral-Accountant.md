# Juice-Shop: Ephemeral Accountant

[Akses Challenge disini](https://juice-shop.herokuapp.com/#/score-board?categories=Injection&showDisabledChallenges=false)

## Ringkasan Tantangan
- **Nama:** Ephemeral Accountant
- **Kategori:** SQL Injection
- **Tujuan:** Login sebagai user `acc0unt4nt@juice-sh.op` yang sebenarnya tidak ada di database, dengan cara "menciptakannya" secara sementara melalui SQL Injection.

### 1. Analisis Tantangan
Tantangan ini unik karena kita harus login sebagai user yang tidak terdaftar. Ini berarti bypass login biasa tidak akan berhasil. Solusinya adalah dengan menggunakan `UNION SELECT` untuk membuat baris data user palsu langsung di dalam query SQL saat proses login.

### 2. Membuat Payload SQL Injection
Kita perlu membuat payload yang sangat spesifik. Payload ini harus berisi query `SELECT` yang menghasilkan satu baris data lengkap, seolah-olah itu adalah data user yang valid dari tabel `Users`. Kita harus mendefinisikan semua kolom yang diperlukan, seperti `id`, `email`, `password`, `role`, dll.

Payload yang akan kita suntikkan ke dalam field `email`:
```sql
' UNION SELECT * FROM (SELECT 20 AS `id`, 'acc0unt4nt@juice-sh.op' AS `username`, 'acc0unt4nt@juice-sh.op' AS `email`, 'test1234' AS `password`, 'accounting' AS `role`, '123' AS `deluxeToken`, '1.2.3.4' AS `lastLoginIp`, '/assets/public/images/uploads/default.svg' AS `profileImage`, '' AS `totpSecret`, 1 AS `isActive`, 12983283 AS `createdAt`, 133424 AS `updatedAt`, NULL AS `deletedAt`)--
```
- `' UNION SELECT * FROM (...)` adalah struktur utama untuk menyuntikkan data palsu kita.
- `(SELECT 20 AS id, ...)` adalah subquery yang "menciptakan" data user `acc0unt4nt` dengan semua atribut yang diperlukan.
- Password untuk user palsu ini kita set menjadi `test1234`.

![Image 1](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/ephemeral/1.png)

### 3. Eksekusi via Burp Suite
1. Tangkap request login menggunakan Burp Suite.
2. Ubah request `POST` ke `/rest/user/login`.
3. Masukkan payload SQL di atas ke dalam field `email`.
4. Atur `password` di dalam JSON menjadi `test1234` (sesuai dengan yang kita definisikan di payload).
5. Kirim request tersebut.

![Image 2](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/ephemeral/2.png)

Server akan memproses query yang kita manipulasi, menemukan "user" yang baru kita ciptakan, memvalidasi passwordnya, dan memberikan kita token akses (JWT) sebagai `acc0unt4nt`. Tantangan selesai.

![Image 3](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/kelas/images/ephemeral/3.png)
