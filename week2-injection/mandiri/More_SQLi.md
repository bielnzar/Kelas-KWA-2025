# Writeup PicoCTF: More SQLi

[Akses Challenge disini](https://play.picoctf.org/practice/challenge/358?page=1&search=sql)

![Image 1](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/more-sqli/1.png)

## Ringkasan Tantangan

-   **Nama Challenge**: More SQLi
-   **Kategori**: Web Exploitation

## Langkah-langkah Eksploitasi

### 1. Bypass Halaman Pertama

Halaman awal menampilkan form yang sepertinya untuk mencari data. Jika kita mencoba memasukkan input sembarangan (`admin`), kita akan melihat query SQL yang digunakan oleh server. Ini petunjuk

![Image 2](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/more-sqli/2.png)

![Image 3](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/more-sqli/3.png)

![Image 4](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/more-sqli/4.png)

Untuk melewati halaman ini, kita bisa menggunakan payload sederhana seperti pada tantangan sebelumnya:

```sql
' OR '1'='1' --
```

Setelah berhasil, kita akan diarahkan ke halaman selanjutnya.

![Image 5](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/more-sqli/5.png)
![Image 6](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/more-sqli/6.png)

### 2. Mencari Nama Tabel (Information Schema)
Di halaman kedua, kita mendapatkan petunjuk "sqlilite". Ini menandakan bahwa database yang digunakan adalah SQLite.

Dalam database SQLite, ada sebuah tabel master bernama `sqlite_master`. Tabel ini berisi informasi tentang semua tabel lain yang ada di dalam database. Kita bisa memanfaatkannya untuk mencari tahu nama-nama tabel yang ada.

Kita akan menggunakan serangan UNION-based SQL Injection. Payload-nya adalah:
```sql
Algiers' UNION SELECT sql,1,1 FROM sqlite_master --
```

Dari hasil payload di atas, kita menemukan ada tabel lain bernama more_table yang memiliki kolom flag.

![Image 7](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/more-sqli/7.png)

### 3. Mengambil Flag dari Tabel more_table
Sekarang kita tahu nama tabel (more_table) dan nama kolom (flag) tempat flag disimpan. Kita tinggal membuat payload terakhir untuk mengambil data tersebut.
```sql
Algiers' UNION SELECT flag,id,1 FROM more_table --
```

![Image 8](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/more-sqli/8.png)

#### Penjelasan Payload:
- UNION SELECT flag,id,1 FROM more_table akan memilih kolom flag dan id dari tabel more_table dan menampilkannya di halaman.

> Flag: `picoCTF{G3tting_5QL_1nJ3c7I0N_l1k3_y0u_sh0ulD_98236ce6}`
