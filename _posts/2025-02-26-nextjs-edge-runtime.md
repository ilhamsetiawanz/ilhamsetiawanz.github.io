---
title: "Next.js Edge Runtime: Menjalankan Kode Lebih Dekat dengan User"
author: Ilham
description: Mengenal teknologi Edge Computing di Next.js. Pelajari cara mengurangi latensi dengan menjalankan logika aplikasi di server yang terdekat dengan lokasi geografis user.
date: 2025-02-26 10:00:00 +0800
categories: [Programing, Frontend]
tags: [nextjs, edge-computing, serverless, performance, middleware]
---

Pernahkah Anda bertanya mengapa website besar seperti Facebook atau Google terasa sangat cepat meskipun Anda berada di lokasi yang jauh dari kantor pusat mereka? Kuncinya adalah **Edge Computing**.

Next.js mengadopsi teknologi ini lewat **Edge Runtime**. Alih-alih menjalankan kode di satu pusat server pusat (misal di Amerika), kode Anda dijalankan di ratusan server kecil yang tersebar di seluruh dunia.

---

### 1. Apa itu Edge Runtime?

Edge Runtime adalah lingkungan eksekusi (runtime) yang jauh lebih ringan daripada Node.js biasa. Ia didasarkan pada engine V8 (engine yang sama dengan Chrome). 
**Filosofinya:** "Bawa kode ke user, bukan user ke kode."

### 2. Mengapa Butuh Edge? (Kecepatan & Latensi)

Bayangkan server pusat Anda ada di Singapura.
- **Node.js Normal:** User dari Jakarta kirim data ke Singapura → Singapura proses → Kirim balik ke Jakarta. (Ada sedikit delay).
- **Edge Runtime:** User dari Jakarta dilayani oleh server terdekat (mungkin di Jakarta itu sendiri). Proses menjadi instan!

### 3. Kapan Menggunakan Edge?

- **Middleware:** Ini adalah tempat paling umum. Anda bisa melakukan pengalihan rute (redirect), pengecekan login, atau A/B Testing seketika sebelum halaman dikirim ke user.
- **Geolokasi:** Menampilkan konten yang berbeda berdasarkan kota asal user secara otomatis.
- **Personalisasi Ringan:** Menampilkan nama user atau preferensi bahasa tanpa menunggu loading lama.

### 4. Batasan Edge Runtime

Karena dirancang sangat ringan, Edge Runtime memiliki batasan:
- Tidak bisa menggunakan semua API Node.js (seperti `fs` untuk baca file atau `net`).
- File size kode harus kecil (biasanya di bawah 1MB - 4MB).
- Tidak cocok untuk tugas berat seperti video processing.

### 5. Cara Mengaktifkan Edge di Next.js

Sangat mudah! Anda cukup menambahkan satu baris konfigurasi di file halaman atau rute API Anda:

```javascript
export const runtime = 'edge';

export default function MyPage() {
  return <div>Halaman ini di-render di Edge!</div>;
}
```

### Kesimpulan

Edge Runtime adalah langkah besar menuju web yang benar-benar tanpa batas geografis. Dengan memindahkan logika kritis ke "tepi" jaringan (edge), Anda memberikan pengalaman pengguna yang sangat responsif, terutama untuk kasus penggunaan global. Di tahun 2026, Edge bukan lagi sekadar opsi, melainkan standar untuk website performan tinggi.

---

**Artikel Sebelumnya:** [Web Performance Audit: Menembus Skor 100 di Lighthouse](/posts/web-performance-audit-lighthouse/)
**Artikel Selanjutnya:** [Server Actions vs API Routes: Cara Baru Berinteraksi dengan Database di Next.js](/posts/server-actions-vs-api-routes/)
