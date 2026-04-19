---
title: "Event Loop JavaScript: Mengapa Anda Harus Benar-benar Memahami Macrotasks vs Microtasks?"
author: Ilham
description: Bongkar cara kerja internal mesin JavaScript. Pelajari urutan eksekusi antara Promise, setTimeout, dan DOM Updates agar kode asinkron Anda tidak lagi membingungkan.
date: 2025-03-03 10:00:00 +0800
categories: [Programing, JavaScript]
tags: [javascript, event-loop, asynchronous, web-performance, deep-dive]
---

Pernahkah Anda bertanya-tanya mengapa sebuah `Promise` yang didefinisikan *setelah* `setTimeout` terkadang justru dijalankan *lebih dulu*? Jika Anda pernah bingung dengan urutan eksekusi kode asinkron di JavaScript, selamat, Anda sedang berhadapan dengan **Event Loop**.

Memahami Event Loop bukan hanya untuk pamer saat interview, tapi sangat krusial agar Anda bisa membangun aplikasi yang responsif dan bebas bug asinkron yang misterius.

---

### 1. JavaScript itu Single-Threaded

Artinya, JavaScript hanya bisa mengerjakan satu hal dalam satu waktu. Ia punya satu tangan. Namun, website modern butuh melakukan banyak hal secara bersamaan (ambil data API, putar animasi, klik tombol). Bagaimana caranya? JavaScript menitipkan tugas berat ke **Web APIs** (di browser) dan menunggu hasilnya lewat **Event Loop**.

### 2. Antrean Tugas: Macrotasks vs Microtasks

Tidak semua tugas asinkron diciptakan setara. JavaScript membagi tugas menjadi dua antrean:

#### a. Macrotasks (Task Queue)
Tugas yang datang dari dunia luar atau butuh waktu tunggu.
- Contoh: `setTimeout`, `setInterval`, Ajax Request, Event Listener.

#### b. Microtasks (Microtask Queue) - PRIORITAS TINGGI
Tugas yang harus segera diselesaikan tepat setelah kode sinkron saat ini selesai, SEBELUM browser melakukan render ulang.
- Contoh: `Promise.then()`, `process.nextTick` (Node.js), `MutationObserver`.

### 3. Urutan Eksekusi yang Benar

Mari kita lihat contoh jebakan ini:

```javascript
console.log('1. Mulai');

setTimeout(() => {
  console.log('2. SetTimeout (Macrotask)');
}, 0);

Promise.resolve().then(() => {
  console.log('3. Promise (Microtask)');
});

console.log('4. Selesai');
```

**Hasilnya:**
1. `1. Mulai`
2. `4. Selesai`
3. `3. Promise (Microtask)`  <-- Microtask didahulukan!
4. `2. SetTimeout (Macrotask)`

Meskipun `setTimeout` disetel ke `0` detik, ia tetap dianggap Macrotask yang harus menunggu seluruh Microtask selesai.

### 4. Mengapa Ini Penting?

Jika Anda menaruh proses yang sangat berat di dalam Microtask (seperti loop jutaan data di dalam `.then()`), browser tidak akan sempat merender ulang tampilan. Akibatnya, website akan terasa macet atau "freeze".

### Kesimpulan

Event Loop adalah jantung dari asinkronitas JavaScript. Ingatlah rumus emasnya:
**Call Stack (Sinkron) → Microtasks (Promise) → Browser Render → Macrotasks (setTimeout).**

Dengan memahami urutan ini, Anda bisa mengatur kapan sebuah kode harus dijalankan agar performa aplikasi tetap optimal dan user experience tetap mulus.

---

**Artikel Sebelumnya:** [Micro-frontend: Strategi Memecah Aplikasi Besar Menjadi Bagian-Bagian Kecil](/posts/micro-frontend-scalability-strategy/)
**Artikel Selanjutnya:** [Membongkar Misteri Closures di JavaScript: Kekuatan Tersembunyi di Balik Fungsi](/posts/javascript-closures-mystery/)
