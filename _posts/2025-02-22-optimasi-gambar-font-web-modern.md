---
title: "Optimasi Gambar dan Font: Rahasia Web Secepat Kilat di Tahun 2026"
author: Ilham
description: Meningkatkan skor Google Core Web Vitals dengan optimasi aset digital. Pelajari teknik kompresi AVIF, Self-hosting fonts, dan pencegahan Layout Shift.
date: 2025-02-22 10:00:00 +0800
categories: [Programing, Frontend]
tags: [frontend, performance, core-web-vitals, nextjs, images]
---

Pernahkah Anda membuka sebuah website yang gambarnya muncul perlahan dari atas ke bawah, atau teksnya tiba-tiba berubah ukuran dan menggeser seluruh layout? Hal ini tidak hanya mengganggu user, tapi juga membuat Google memberikan nilai buruk pada website Anda dalam **Core Web Vitals**.

Di tahun 2026, browser semakin cerdas, namun aset yang tidak dioptimalkan tetap akan menjadi beban. Berikut adalah strategi terbaik untuk menangani gambar dan font.

---

### 1. Gunakan Format Gambar Masa Depan: AVIF

Lupakan JPEG atau bahkan WebP jika Anda mengejar performa maksimal. **AVIF** adalah standar baru yang menawarkan kompresi jauh lebih baik (ukuran file lebih kecil) tanpa mengurangi kualitas gambar.

**Tips:** Di Next.js, Anda bisa mengaktifkan dukungan AVIF melalui konfigurasi `next.config.js`:
```javascript
module.exports = {
  images: {
    formats: ['image/avif', 'image/webp'],
  },
}
```

### 2. Hindari "Cumulative Layout Shift" (CLS)

CLS terjadi saat gambar dimuat dan tiba-tiba "mendorong" konten di bawahnya. Ini sangat menyebalkan bagi user yang sedang membaca.

**Solusinya:** Selalu berikan atribut `width` dan `height` pada tag image, atau gunakan pembungkus dengan aspek rasio yang tetap. Next.js secara otomatis menangani ini jika Anda menggunakan komponen `next/image`.

### 3. Optimasi Font: Jangan Pakai CDN Langsung!

Memuat font langsung dari Google Fonts CDN (`https://fonts.googleapis.com/...`) sebenarnya bisa memperlambat website karena browser harus melakukan koneksi ke domain lain.

**Strategi Terbaik:**
- **Self-hosting:** Simpan file font di server Anda sendiri.
- **Variable Fonts:** Gunakan satu file font yang menyimpan semua ketebalan (Bold, Light, Regular) untuk mengurangi jumlah file yang didownload.
- **Font Display Swap:** Gunakan `font-display: swap` di CSS agar teks tetap muncul menggunakan font sistem sementara menunggu font utama selesai didownload.

### 4. Preloading Aset Kritikal

Jika ada gambar "Hero" di bagian paling atas halaman, beritahu browser untuk mendownloadnya lebih awal menggunakan tag `link rel="preload"`.

```html
<link rel="preload" href="/images/hero.avif" as="image">
```

### Kesimpulan

Optimasi aset bukan hanya tentang membuat gambar terlihat bagus, tapi tentang memberikan pengalaman yang mulus dan cepat bagi user. Website yang ringan akan mendapatkan ranking lebih tinggi di mesin pencari dan membuat user betah berlama-lama. Mulailah mengganti format gambar Anda ke AVIF hari ini!

---

**Artikel Sebelumnya:** [State Management: Kapan Pakai Zustand, Kapan Context API?](/posts/state-management-zustand-vs-context/)
**Artikel Selanjutnya:** [Membangun UI Dashboard yang Sangat Responsif: Teknik Grid dan Flexbox Modern](/posts/membangun-ui-dashboard-responsif/)
