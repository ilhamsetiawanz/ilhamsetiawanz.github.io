---
title: "Web Performance Audit: Menembus Skor 100 di Lighthouse"
author: Ilham
description: Panduan praktis mengaudit performa website menggunakan Google Lighthouse. Pelajari langkah-langkah teknis untuk mendapatkan skor sempurna di semua aspek.
date: 2025-02-25 10:00:00 +0800
categories: [Programing, Frontend]
tags: [performance, lighthouse, core-web-vitals, seo, web-audit]
---

Memiliki website yang indah tidak ada gunanya jika user meninggalkannya karena loading yang terlalu lama. Google mencatat bahwa probabilitas user meninggalkan website meningkat 32% saat waktu load meningkat dari 1 detik ke 3 detik.

Lalu, bagaimana cara mengukur performa website Anda secara objektif? Alat paling standar industri saat ini adalah **Google Lighthouse**.

---

### 1. Apa itu Google Lighthouse?

Lighthouse adalah alat otomatis yang bersifat open-source untuk meningkatkan kualitas halaman web. Ia memberikan skor dari 1-100 pada 4 aspek utama:
1. **Performance:** Seberapa cepat konten muncul.
2. **Accessibility:** Apakah orang dengan disabilitas bisa menggunakan web Anda?
3. **Best Practices:** Apakah kode Anda mengikuti standar keamanan dan pengembangan web modern?
4. **SEO:** Apakah website Anda mudah ditemukan oleh Google?

### 2. Rahasia Mendapatkan Skor 100 di Performance

Skor Performance adalah yang paling sulit didapat. Berikut adalah langkah "wajib" untuk menembusnya:

- **LCP (Largest Contentful Paint):** Pastikan elemen visual terbesar (seperti gambar hero) dimuat dalam kurang dari 2.5 detik. Gunakan `priority` pada image tersebut.
- **TBT (Total Blocking Time):** Kurangi penggunaan JavaScript yang terlalu berat saat awal loading agar browser tidak "macet".
- **Eliminate Render-Blocking Resources:** Pindahkan file CSS atau JS yang tidak diperlukan ke bagian bawah halaman atau gunakan atribut `defer/async`.

### 3. Accessibility: Lebih dari Sekadar Desain

Skor 100 di sini berarti semua orang bisa mengakses konten Anda.
- Gunakan atribut `alt` pada semua gambar.
- Pastikan kontras warna teks dan background cukup tinggi agar mudah dibaca.
- Pastikan semua tombol dan link memiliki label yang jelas untuk *screen reader*.

### 4. Best Practices dan SEO

- **HTTPS:** Pastikan website menggunakan koneksi aman.
- **Meta Tags:** Judul dan deskripsi halaman harus ada dan relevan.
- **Image Aspect Ratio:** Jangan biarkan gambar menyebabkan layout bergeser saat dimuat.

### 5. Jangan Terobsesi Hanya pada Skor

Mendapatkan skor 100 memang membanggakan, namun prioritas utama tetaplah **Real User Experience**. Terkadang, skor 90-95 sudah sangat luar biasa untuk aplikasi yang kompleks. Yang terpenting adalah user merasa website Anda cepat dan nyaman digunakan.

### Kesimpulan

Lighthouse bukan hanya alat audit, ia adalah guru yang memberi tahu di mana letak kelemahan website Anda. Lakukan audit secara rutin setiap kali ada pembaruan fitur besar untuk memastikan kualitas website Anda tetap terjaga di level tertinggi.

---

**Artikel Sebelumnya:** [Animasi Halus dengan Framer Motion: Menghidupkan Antarmuka Web Anda](/posts/animasi-halus-framer-motion/)
**Artikel Selanjutnya:** [Next.js Edge Runtime: Menjalankan Kode Lebih Dekat dengan User Anda](/posts/nextjs-edge-runtime/)
