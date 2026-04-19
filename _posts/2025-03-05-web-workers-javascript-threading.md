---
title: "Web Workers: Solusi Menjalankan Perhitungan Berat Tanpa Membuat Browser Macet"
author: Ilham
description: Pelajari teknik Multithreading di browser menggunakan Web Workers. Jalankan tugas intensif di background agar antarmuka website Anda tetap responsif.
date: 2025-03-05 10:00:00 +0800
categories: [Programing, JavaScript]
tags: [javascript, web-workers, performance, multithreading, frontend]
---

JavaScript secara alami adalah makhluk yang **Single-Threaded**. Ia hanya punya satu "jalan" untuk mengerjakan semua tugas. Jika Anda menyuruh JavaScript melakukan perhitungan matematika yang sangat rumit atau memproses ribuan data di tab utama, website Anda akan "freeze" (macet) sampai tugas itu selesai. User tidak bisa mengeklik tombol, memilih teks, atau bahkan melihat animasi.

Untuk mengatasi ini, browser modern menyediakan fitur **Web Workers**.

---

### 1. Apa itu Web Workers?

Web Workers memungkinkan kita menjalankan kode JavaScript di **Thread terpisah** (background). Ia berjalan sejajar dengan thread utama browser namun tidak bisa menginterupsi antarmuka (UI).

**Analogi:** Thread utama adalah kasir restoran yang melayani pelanggan. Web Worker adalah koki di dapur. Kasir tetap bisa menerima pesanan (UI responsif) sementara koki sedang memasak hidangan yang rumit (proses data berat).

### 2. Apa yang Bisa dan Tidak Bisa Dilakukan?

- **Bisa:** Melakukan perhitungan matematika berat, manipulasi data besar, request API lewat Fetch.
- **Tidak Bisa:** Mengakses DOM (Anda tidak bisa mengubah teks HTML langsung dari Worker), tidak bisa mengakses `window` atau `document`.

### 3. Cara Penggunaan Dasar

**File Utama (main.js):**
```javascript
const worker = new Worker('worker.js');

// Kirim data ke worker
worker.postMessage({ data: 1000000 });

// Terima hasil dari worker
worker.onmessage = function(e) {
  console.log('Hasil dari Worker: ' + e.data);
};
```

**File Worker (worker.js):**
```javascript
onmessage = function(e) {
  // Lakukan perhitungan berat di sini
  let result = 0;
  for (let i = 0; i < e.data.data; i++) {
    result += i;
  }
  
  // Kirim balik ke main thread
  postMessage(result);
};
```

### 4. Kapan Harus Menggunakan Web Workers?

- **Image/Video Processing:** Mengubah ukuran foto atau memberi filter secara langsung di browser.
- **Data Parsing Besar:** Mengolah file CSV atau JSON berukuran puluhan Megabyte.
- **Enkripsi/Dekripsi:** Melakukan proses keamanan data yang butuh CPU tinggi.

### Kesimpulan

Web Workers adalah kunci untuk membangun aplikasi web tingkat profesional yang terasa sangat cepat dan responsif. Meskipun ada batasan akses ke DOM, kemampuan untuk memindahkan beban kerja berat ke background akan membuat website Anda jauh lebih unggul dalam hal pengalaman pengguna (UX).

---

**Artikel Sebelumnya:** [Membongkar Misteri Closures di JavaScript: Kekuatan Tersembunyi di Balik Fungsi](/posts/javascript-closures-mystery/)
**Artikel Selanjutnya:** [Bun: Runtime JavaScript Masa Depan yang Sangat Cepat dan Efisien](/posts/bun-runtime-javascript-modern/)
