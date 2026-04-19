---
title: "Hydration Error: Musuh Terbesar Next.js dan Cara Mengatasinya"
author: Ilham
description: Sering melihat error 'Text content did not match' di Next.js? Pelajari penyebab umum Hydration Error dan solusi terbaik untuk memperbaikinya.
date: 2025-02-18 10:00:00 +0800
categories: [Programing, Frontend]
tags: [nextjs, react, frontend, debugging, hydration]
---

Jika Anda adalah pengguna Next.js, Anda pasti pernah melihat pesan error berwarna merah yang sangat menjengkelkan: **"Hydration failed because the initial UI does not match what was rendered on the server."** 

Bagi pemula, error ini terasa membingungkan karena kode tampak benar tapi browser tetap komplain. Artikel ini akan menjelaskan mengapa ini terjadi dan bagaimana cara menghilangkannya dari project Anda.

---

### 1. Apa itu Hydration?

Dalam Next.js, "Hydration" adalah proses di mana React di browser mengambil HTML statis yang dikirim dari server dan "menghidupkannya" dengan menambahkan event listener dan interactivity.

Gampangnya:
1. Server mengirimkan "Patung" (HTML).
2. Browser menerima Patung tersebut.
3. React datang dan memberikan "Jiwa" (JavaScript) ke Patung itu agar bisa bergerak.

**Hydration Error terjadi jika Patung yang diterima browser berbeda dengan Patung yang ingin dibuat oleh React.**

### 2. Penyebab Umum Hydration Error

#### a. Struktur HTML yang Tidak Valid
Ini adalah penyebab paling sering. Misalnya, Anda memasukkan tag `<div>` di dalam tag `<p>`. Browser akan otomatis mengeluarkan `<div>` tersebut, sehingga struktur HTML berubah sebelum React sempat menaikinya.
```html
<p>
  <div>Ini salah</div>
</p>
```

#### b. Penggunaan Data Non-Deterministik
Jika Anda menampilkan data yang berbeda antara server dan browser, seperti Waktu (`new Date()`) atau Angka Acak (`Math.random()`).
- Server merender jam 10:00.
- Browser merender jam 10:01.
- **Hasilnya:** Error!

#### c. Akses Browser API Terlalu Cepat
Menggunakan `window` atau `localStorage` langsung di dalam fungsi komponen. Server tidak tahu apa itu `window`, sehingga ia merender sesuatu yang berbeda dari browser.

### 3. Solusi 1: Menggunakan `useEffect` (Cara Tradisional)

Pastikan kode yang menggunakan browser API hanya dijalankan setelah komponen "mounted" (terpasang) di browser.

```javascript
const [isClient, setIsClient] = useState(false);

useEffect(() => {
  setIsClient(true);
}, []);

return (
  <div>
    {isClient ? localStorage.getItem('theme') : 'Loading...'}
  </div>
);
```

### 4. Solusi 2: Mematikan SSR untuk Komponen Tertentu

Jika komponen Anda sangat bergantung pada browser (seperti peta atau editor teks), Anda bisa memuatnya secara dinamis dengan opsi `ssr: false`.

```javascript
import dynamic from 'next/dynamic';

const MapsComponent = dynamic(() => import('./Maps'), { ssr: false });
```

### 5. Solusi 3: Atribut `suppressHydrationWarning`

Jika Anda hanya ingin menampilkan waktu dan tidak keberatan dengan sedikit perbedaan, Anda bisa menggunakan atribut ini (hanya bekerja pada tag tersebut).
```html
<p suppressHydrationWarning>{new Date().getFullYear()}</p>
```

### Kesimpulan

Hydration Error adalah cara React memberitahu Anda bahwa "Ada yang tidak sinkron antara server dan browser!". Jangan mengabaikannya, karena ini bisa merusak performa dan fungsionalitas aplikasi Anda di production. Periksalah struktur HTML Anda dan pastikan data yang Anda tampilkan bersifat deterministik.

---

**Artikel Sebelumnya:** [Memahami React Server Components: Masa Depan Web yang Lebih Ringan dan Cepat](/posts/memahami-react-server-components/)
**Artikel Selanjutnya:** [TypeScript untuk Developer Senior: Menguasai Utility Types demi Kode yang Lebih Bersih](/posts/typescript-utility-types-senior/)
