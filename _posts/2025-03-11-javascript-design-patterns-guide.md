---
title: "JavaScript Design Patterns: Membangun Kode yang Terstruktur dan Scalable"
author: Ilham
description: Mengenal 3 pola desain (Design Patterns) paling populer di JavaScript. Tingkatkan kualitas arsitektur kode Anda dengan Singleton, Factory, dan Observer Pattern.
date: 2025-03-11 10:00:00 +0800
categories: [Programing, JavaScript]
tags: [javascript, design-patterns, singleton, observer-pattern, factory-pattern, clean-code]
---

Pernahkah Anda merasa kode Anda berantakan saat aplikasi mulai membesar? Anda mengulang logika yang sama berkali-kali, atau satu bagian kode terikat terlalu kuat dengan bagian lainnya. Solusi dari masalah ini adalah **Design Patterns**.

Design Patterns adalah pola atau solusi umum yang sudah teruji waktu untuk menyelesaikan masalah yang sering muncul dalam software development. Mari kita pelajari 3 yang terpenting bagi developer JavaScript.

---

### 1. Singleton Pattern: Satu untuk Semua

Singleton memastikan sebuah kelas hanya memiliki **satu instance** di seluruh aplikasi. Ini sangat berguna untuk hal-hal seperti koneksi database atau konfigurasi global.

```javascript
const Config = (function() {
  let instance;

  function createInstance() {
    return { theme: 'dark', lang: 'id' };
  }

  return {
    getInstance: function() {
      if (!instance) instance = createInstance();
      return instance;
    }
  };
})();

const a = Config.getInstance();
const b = Config.getInstance();
console.log(a === b); // true (Objek yang sama!)
```

### 2. Factory Pattern: Sang Pembuat Objek

Factory Pattern digunakan untuk membuat objek tanpa harus menentukan kelas spesifiknya secara langsung. Ini sangat membantu saat jenis objek yang dibuat bisa berubah-ubah.

```javascript
class Car { constructor() { this.type = 'Mobil'; } }
class Bike { constructor() { this.type = 'Motor'; } }

function VehicleFactory(type) {
  if (type === 'car') return new Car();
  if (type === 'bike') return new Bike();
}

const myRide = VehicleFactory('car');
```

### 3. Observer Pattern: Sistem Notifikasi

Pola ini sangat populer di frontend (terutama di React atau Vue). Satu objek ("Subject") memberi tahu banyak objek lain ("Observers") saat terjadi perubahan status.

**Bayangkan seperti Subscriber YouTube:**
- Saat YouTuber mengunggah video (Status berubah), semua subscribers (Observers) mendapatkan notifikasi secara otomatis.

### Mengapa Harus Peduli?

Menggunakan Design Patterns bukan tentang mengikuti aturan yang kaku, melainkan tentang **komunikasi**. Saat Anda memberi tahu rekan setim, "Saya pakai Singleton untuk modul ini," mereka akan langsung paham cara kerjanya tanpa harus membaca setiap baris kode.

### Kesimpulan

Design Patterns adalah bahasa universal para software engineer. Mempelajarinya akan mengubah cara Anda memandang kode—dari sekadar "instruksi untuk komputer" menjadi "arsitektur yang cerdas dan mudah dikelola". Mulailah terapkan pola-pola ini agar aplikasi Anda lebih stabil seiring bertambahnya fitur.

---

**Artikel Sebelumnya:** [Error Handling yang Elegan: Panduan Menggunakan Try/Catch secara Profesional](/posts/javascript-error-handling-best-practices/)
**Artikel Selanjutnya:** [Functional Programming Dasar: Menulis Kode yang Bersih dan Minim Bug](/posts/javascript-functional-programming-basics/)
