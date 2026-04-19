---
title: "Functional Programming Dasar: Menulis Kode yang Bersih dan Minim Bug"
author: Ilham
description: Mengenal konsep Pure Functions, Immutability, dan Higher-Order Functions. Pelajari cara mengubah gaya koding Anda agar lebih modern dan mudah ditest.
date: 2025-03-12 10:00:00 +0800
categories: [Programing, JavaScript]
tags: [javascript, functional-programming, pure-functions, immutability, cleaner-code]
---

Ada dua gaya besar dalam menulis kode: **Object-Oriented** (yang fokus pada objek dan state) dan **Functional Programming** (yang fokus pada fungsi dan transformasi data). 

Di dunia JavaScript modern, terutama jika Anda menggunakan React, gaya **Functional Programming (FP)** menjadi sangat dominan. Mengapa? Karena FP membuat kode kita lebih mudah diprediksi, lebih mudah diuji, dan jauh lebih jarang terkena bug yang aneh.

---

### 1. Pure Functions: Hasil yang Selalu Sama

Sebuah fungsi disebut "Pure" jika ia selalu menghasilkan output yang sama untuk input yang sama, dan tidak mengubah variabel apa pun di luar dirinya (**No Side Effects**).

**Impure (Bisa bikin bingung):**
```javascript
let total = 0;
function tambah(angka) { 
  total += angka; // Mengubah variabel di luar fungsi
  return total;
}
```

**Pure (Mudah diprediksi):**
```javascript
function tambah(a, b) {
  return a + b; // Memberikan hasil murni tanpa mengubah dunia luar
}
```

### 2. Immutability: Jangan Ubah Data, Buat Baru!

Dalam FP, kita tidak mengubah data yang sudah ada. Kita membuat salinan baru dengan perubahan yang kita inginkan.

```javascript
const user = { name: 'Ilham', age: 25 };

// Alih-alih: user.age = 26; (Mutable)
// Lakukan: (Immutable)
const updatedUser = { ...user, age: 26 };
```
Ini sangat krusial di React agar sistem tahu kapan sebuah data berubah dan perlu melakukan render ulang.

### 3. Higher-Order Functions: Fungsi Di Dalam Fungsi

JavaScript memperlakukan fungsi sebagai "Warga Kelas Satu". Artinya, fungsi bisa dimasukkan ke variabel, atau dikirimkan sebagai argumen ke fungsi lain. 

Contoh yang sering kita pakai: `map`, `filter`, dan `reduce`.

```javascript
const angka = [1, 2, 3, 4];

// Menggunakan gaya FP untuk menyaring angka genap dan mengalikannya dengan 2
const hasil = angka
  .filter(n => n % 2 === 0)
  .map(n => n * 2);

console.log(hasil); // [4, 8]
```

### 4. Mengapa Harus Pakai FP?

- **Mudah Dites:** Karena Pure Function hanya bergantung pada input, Anda tidak perlu menyiapkan lingkungan yang rumit untuk melakukan Testing.
- **Minim Bug:** Dengan *Immutability*, Anda tidak akan kaget karena tiba-tiba ada variabel yang nilainya berubah di tempat lain tanpa Anda tahu.
- **Deklaratif:** Kode FP biasanya lebih menjelaskan "Apa yang ingin didapat" daripada "Bagaimana langkah manualnya".

### Kesimpulan

Belajar Functional Programming akan mengubah cara Anda berpikir saat ngoding. Anda akan mulai memandang aplikasi sebagai kumpula transformasi data yang rapi dan terukur. Mulailah dengan membuat fungsi-fungsi kecil yang murni, maka project Anda akan menjadi jauh lebih berkualitas.

---

**Artikel Sebelumnya:** [JavaScript Design Patterns: Membangun Kode yang Terstruktur dan Scalable](/posts/javascript-design-patterns-guide/)
**Artikel Selanjutnya:** [Era AI Agents: Dari Autocomplete ke Autonomous yang Pintar](/posts/era-ai-agents-autonomous-future/)
