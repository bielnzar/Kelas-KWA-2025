# Writeup PicoCTF: SQLiLite

[Akses Challenge disini](https://play.picoctf.org/practice/challenge/304?page=1&search=sql)

Di tantangan kali ini, kita akan berhadapan dengan salah satu jenis serangan web: **SQL Injection**

![Image 1](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/sqlilite/1.png)

## Langkah-langkah Eksploitasi

### 1. Analisis Form Login

Kita diberikan halaman login standar. Dari judul tantangannya, "SQLiLite", kita bisa langsung menduga bahwa celah keamanannya adalah SQL Injection.

### 2. Mencoba SQL Injection

Kita akan menggunakan payload SQL Injection yang paling umum pada kolom username:

```sql
' OR '1'='1' --
```

![Image 2](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/sqlilite/2.png)

Biarkan kolom password kosong, lalu klik Login.

Payload ini akan memanipulasi query SQL di server. Jika query aslinya seperti ini:
```sql
SELECT * FROM users WHERE name='[USERNAME]' AND password='[PASSWORD]'
```
Maka setelah kita masukkan payload, query-nya akan berubah menjadi:
```sql
OR '1'='1' adalah kondisi yang akan selalu benar.
```
`--` adalah tanda komentar di SQL, yang membuat sisa query (termasuk pengecekan password) diabaikan oleh database.

![Image 3](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/sqlilite/3.png)

> Flag: `picoCTF{L00k5_l1k3_y0u_solv3d_it_ec8a64c7}`