# Writeup PicoCTF: Irish-Name-Repo 1

[Akses Challenge disini](https://play.picoctf.org/practice/challenge/80?category=1&page=5&search=)

![Image 1](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/irish-1/1.png)

## Langkah-langkah Eksploitasi

### 1. Menemukan Halaman Login Admin

Website menampilkan galeri foto dan nama selebriti. Dengan sedikit eksplorasi, kita bisa menemukan halaman login admin. kita juga menemukan petunjuk bahwa proses login ditangani oleh `login.html`.

### 2. Bypass Login dengan SQL Injection

Kita akan mencoba membobol form login ini. Kali ini, kita akan menggunakan Burp Suite untuk mencegat dan memodifikasi request yang dikirim ke server.

Payload yang akan kita gunakan di kolom username adalah:

```sql
admin' -- 
```

### 3. Mengirim Payload dengan Burp Suite
1. Buka Burp Suite dan aktifkan intercept.
2. Isi form login dengan username admin dan password apa saja, lalu klik Login.
3. Burp Suite akan menangkap request ini.
4. Ubah nilai username menjadi payload kita: `admin'--`.
5. Kirim request yang sudah dimodifikasi (Forward).

![Image 2](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/irish-1/2.png)

> Flag: `picoCTF{s0m3_SQL_fb3fe2ad}`