---
title: "Error Handling yang Elegan: Panduan Menggunakan Try/Catch secara Profesional"
author: Ilham
description: Berhenti mengabaikan pesan error! Pelajari cara menangani kesalahan pada kode JavaScript Anda menggunakan Try-Catch, Custom Errors, dan strategi Async/Await.
date: 2025-03-10 10:00:00 +0800
categories: [Programing, JavaScript]
tags: [javascript, error-handling, debugging, best-practices, asynchronous]
---

"Aplikasi tiba-tiba berhenti tapi saya tidak tahu kenapa."

Pernahkah Anda mendengar keluhan ini? Masalah utamanya biasanya bukan karena ada bug, tapi karena developer tidak menangani error dengan baik. Di JavaScript, sebuah error kecil yang tidak ditangkap (uncaught error) bisa menghentikan seluruh aplikasi Anda seketika.

Inilah saatnya Anda mempelajari cara menangani error dengan cara yang elegan dan profesional.

---

### 1. Masalah: Catch Block yang Kosong (Anti-Pattern)

Banyak developer pemula menulis kode seperti ini:
```javascript
try {
  prosesData();
} catch (error) {
  // Kosong atau cuma console.log(error)
}
```
Ini sangat berbahaya! Jika terjadi error di `prosesData`, aplikasi akan terus berjalan seolah-olah semuanya baik-baik saja, namun data Anda mungkin korup atau tampilan web menjadi aneh.

### 2. Gunakan Custom Errors untuk Identifikasi Cepat

Jangan hanya mengandalkan pesan "Error" bawaan. Buatlah kelas error sendiri agar Anda tahu dari mana sumber masalahnya.

```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = "ValidationError";
  }
}

function updateEmail(email) {
  if (!email.includes('@')) {
    throw new ValidationError("Format email tidak valid!");
  }
}
```

### 3. Menangani Error pada Async/Await

Kode asinkron (seperti Fetch API atau Database query) wajib dibungkus dalam `try-catch`.

```javascript
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    if (!response.ok) throw new Error('Server bermasalah');
    const data = await response.json();
    return data;
  } catch (err) {
    // Tampilkan pesan yang ramah kepada user
    alert("Maaf, kami sedang kesulitan mengambil data. Silakan coba lagi.");
    // Log error asli ke server (misalnya pakai Sentry)
    logErrorToService(err);
  }
}
```

### 4. Kekuatan dari `finally`

Blok `finally` akan selalu dijalankan, baik kode Anda sukses maupun error. Ini adalah tempat terbaik untuk mematikan "Loading Spinner" atau menutup koneksi database.

```javascript
setIsLoading(true);
try {
  await doSomething();
} catch (err) {
  handleError(err);
} finally {
  setIsLoading(false); // Selalu dipanggil, apa pun yang terjadi!
}
```

### Kesimpulan

Menangani error bukan berarti membuat aplikasi Anda "tahan banting" terhadap bug, tapi membuat aplikasi Anda **bisa diprediksi** saat terjadi kesalahan. Dengan memberikan pesan yang jelas kepada user dan mencatat log teknis yang detail bagi developer, Anda membangun aplikasi yang jauh lebih matang dan profesional.

---

**Artikel Sebelumnya:** [Prototypes vs Classes: Membedah Cara Kerja Pewarisan Sifat di JavaScript](/posts/javascript-prototypes-vs-classes/)
**Artikel Selanjutnya:** [JavaScript Design Patterns: Membangun Kode yang Terstruktur dan Scalable](/posts/javascript-design-patterns-guide/)
