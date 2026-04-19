---
title: "Prototypes vs Classes: Membedah Cara Kerja Pewarisan Sifat di JavaScript"
author: Ilham
description: Ternyata JavaScript tidak punya 'Class' yang sesungguhnya. Pelajari rahasia Prototypal Inheritance dan mengapa Class di JS hanyalah 'Syntactic Sugar'.
date: 2025-03-09 10:00:00 +0800
categories: [Programing, JavaScript]
tags: [javascript, oop, prototypes, classes, computer-science]
---

Sejak diperkenalkannya ES6 di tahun 2015, kita bisa menulis kode Object-Oriented (OOP) di JavaScript menggunakan keyword `class` yang tampak mirip dengan Java atau C#. Namun, tahukah Anda bahwa di bawah tendanya, JavaScript tetap menggunakan sistem **Prototypes**?

Memahami perbedaan antara Class dan Prototypes adalah kunci untuk memahami memori dan perilaku objek yang aneh di JavaScript.

---

### 1. Rahasia Terbesar: Class hanyalah "Syntactic Sugar"

Di bahasa seperti Java, Class adalah cetakan (blueprint) dan Objek adalah hasilnya. Mereka adalah dua entitas yang berbeda.

Di JavaScript, yang ada hanyalah **Objek yang berbicara dengan Objek lain**. Saat Anda menulis `class`, JavaScript sebenarnya sedang membuat fungsi konstruktor dan menempelkan method ke dalam sebuah properti bernama `prototype`.

### 2. Prototypal Inheritance: Rantai Koneksi

Setiap objek di JavaScript punya "link" rahasia ke objek lainnya. Link ini disebut `__proto__`. 

Jika Anda mencoba mengakses properti `user.nama` tapi properti itu tidak ada di objek `user`, JavaScript tidak langsung menyerah. Ia akan melihat ke "Bapak"-nya (Prototype). Jika di Bapak-nya juga tidak ada, ia melihat ke "Kakek"-nya. Inilah yang disebut **Prototype Chain**.

### 3. Keuntungan Prototypes: Hemat Memori!

Jika Anda memiliki 1.000 objek User, dan setiap User punya method `sayHello()`.
- **Jika pakai Class/Prototypes:** Method `sayHello` hanya disimpan SATU KALI di objek Prototype. Semua 1.000 user membagikan method yang sama.
- **Jika tanpa Prototypes:** Anda akan menyimpan 1.000 salinan method yang sama di memori. Sangat boros!

### 4. Contoh Perbandingan Kode

**Gaya Lama (Prototypes):**
```javascript
function User(name) {
  this.name = name;
}

User.prototype.sayHi = function() {
  console.log("Hi, " + this.name);
};
```

**Gaya Baru (Class):**
```javascript
class User {
  constructor(name) {
    this.name = name;
  }
  
  sayHi() {
    console.log("Hi, " + this.name);
  }
}
```
Kedua kode di atas melakukan hal yang **sama persis** di dalam memori.

### Kesimpulan

Jangan tertipu oleh tampilan cantik keyword `class`. Sebagai developer JavaScript yang handal, Anda harus sadar bahwa di baliknya terdapat sistem **Prototype** yang dinamis dan sangat fleksibel. Pengetahuan ini akan sangat membantu saat Anda berhadapan dengan masalah *context* (`this`) atau saat ingin melakukan optimasi memori pada aplikasi skala besar.

---

**Artikel Sebelumnya:** [WebAssembly (Wasm): Mengapa Web Developer Harus Mulai Peduli di Tahun 2026?](/posts/webassembly-wasm-future/)
**Artikel Selanjutnya:** [Error Handling yang Elegan: Panduan Menggunakan Try/Catch secara Profesional](/posts/javascript-error-handling-best-practices/)
