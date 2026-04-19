---
title: "Server Actions vs API Routes: Cara Baru Berinteraksi dengan Database di Next.js"
author: Ilham
description: Membandingkan dua cara utama untuk menangani form submission dan interaksi database di Next.js. Pelajari kapan harus meninggalkan API Routes yang tradisional.
date: 2025-02-27 10:00:00 +0800
categories: [Programing, Frontend]
tags: [nextjs, react, api, server-actions, database]
---

Salah satu perubahan paling revolusioner di Next.js versi terbaru adalah diperkenalkannya **Server Actions**. Fitur ini benar-benar mengubah cara kita mengirim data dari frontend ke server. 

Dulu, kita wajib membuat API Route terpisah dan memanggilnya menggunakan `fetch` dari frontend. Sekarang, kita bisa melakukan interaksi database langsung dari fungsi di komponen kita. Tapi, apakah API Route sudah mati? Mari kita bedah.

---

### 1. Traditional API Routes: Cara Lama

Pada cara lama, Anda harus membuat file di `app/api/hello/route.ts`.
1. Anda membuat endpoint `/api/hello`.
2. Anda menulis `fetch('/api/hello', { method: 'POST', body: ... })` di frontend.
3. Anda harus menangani validasi status code (`200`, `400`, `500`) secara manual.

**Masalahnya:** Anda harus menulis banyak kode "boilerplate" hanya untuk mengirim satu form sederhana.

### 2. Server Actions: Revolusi "Function-Based"

Dengan Server Actions, Anda cukup membuat fungsi dengan direktif `'use server'`.

```javascript
// Di dalam komponen React
async function createPost(formData) {
  'use server'
  
  const title = formData.get('title');
  await db.post.create({ data: { title } });
}

return (
  <form action={createPost}>
    <input name="title" />
    <button type="submit">Kirim</button>
  </form>
)
```

**Kelebihannya:**
- **Zero API Management:** Tidak perlu lagi memikirkan endpoint URL.
- **Type Safety:** Jika Anda pakai TypeScript, tipe data antara form dan database tersinkronisasi otomatis.
- **Progressive Enhancement:** Form tetap bekerja meskipun JavaScript di browser user dimatikan!

### 3. Kapan Masih Butuh API Routes?

Meskipun Server Actions keren, API Routes tetap diperlukan jika:
- Anda ingin API tersebut bisa diakses oleh aplikasi lain (misal: Aplikasi Mobile atau Partner Eksternal).
- Anda membangun Webhook (seperti notifikasi pembayaran dari Midtrans).
- Anda menyediakan data publik yang ingin di-consume oleh user secara bebas.

### 4. Perbandingan Singkat

| Fitur | Server Actions | API Routes |
|-------|----------------|------------|
| **Setup** | Sangat Instan | Butuh File Terpisah |
| **Kebutuhan JS** | Bisa tanpa JS | Wajib ada JS |
| **Akses Eksternal** | Tidak Bisa | Bisa |
| **Guna Utama** | Interaksi Internal | Public API / Webhooks |

### Kesimpulan

Untuk 90% kebutuhan aplikasi web internal (seperti posting status, update profil, atau hapus data), **Server Actions** adalah pemenangnya. Ia membuat pekerjaan developer jauh lebih cepat dan kode jauh lebih rapi. Namun, tetap simpan keahlian API Routes Anda untuk kebutuhan integrasi pihak ketiga.

---

**Artikel Sebelumnya:** [Next.js Edge Runtime: Menjalankan Kode Lebih Dekat dengan User Anda](/posts/nextjs-edge-runtime/)
**Artikel Selanjutnya:** [Dark Mode yang Sempurna: Menggabungkan CSS Variables dan Preferensi User](/posts/dark-mode-css-variables/)
