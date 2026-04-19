---
title: "Keamanan API di Laravel: Pilih Sanctum atau Passport?"
author: Ilham
description: Bingung memilih antara Laravel Sanctum dan Laravel Passport? Artikel ini membedah perbedaan teknis, kemudahan penggunaan, dan skenario terbaik untuk aplikasi Anda.
date: 2025-02-07 10:00:00 +0800
categories: [Programing, Laravel]
tags: [laravel, api, security, authentication]
---

Salah satu pertanyaan paling umum yang ditanyakan oleh developer Laravel saat membangun API adalah: **"Pakai Sanctum atau Passport?"**

Keduanya adalah alat resmi (official packages) dari tim Laravel untuk mengamankan aplikasi, tetapi mereka dirancang untuk tujuan yang berbeda. Salah pilih bisa membuat aplikasi Anda menjadi terlalu rumit tanpa alasan, atau sebaliknya, kurang aman dari yang dibutuhkan.

---

### 1. Laravel Sanctum: Simpel dan Elegan

Sanctum (sebelumnya bernama Airlock) dirancang sebagai sistem autentikasi ringan untuk aplikasi Single Page Application (SPA), aplikasi mobile sederhana, dan token API dasar.

#### Cara Kerja Sanctum:
1. **Cookie-based Authentication:** Saat Anda membangun aplikasi dengan React/Vue/Next.js yang berjalan di domain yang sama (atau subdomain), Sanctum menggunakan cookie session normal. Ini sangat aman karena mencegah serangan CSRF secara otomatis.
2. **Mobile API Tokens:** Jika Anda butuh token panjang untuk aplikasi Android/iOS, Sanctum bisa membuatkan "Personal Access Token" yang mirip dengan GitHub Token.

#### Kapan Pilih Sanctum?
- Anda membangun SPA sendiri (Next.js, Vue, React).
- Anda membangun aplikasi mobile internal.
- Anda tidak butuh standar OAuth2 yang kompleks.

### 2. Laravel Passport: Standar OAuth2 Lengkap

Passport adalah implementasi server OAuth2 lengkap. Jika Sanctum adalah "tangga", maka Passport adalah "lift gedung pencakar langit". Ia sangat powerful namun butuh setup yang lebih banyak.

#### Keunggulan Passport:
- Mendukung berbagai macam **Grant Types** (Authorization Code, Client Credentials, dll).
- Cocok jika Anda ingin mengizinkan aplikasi pihak ketiga (third-party) untuk mengakses data user Anda (seperti fitur "Login with Google" tapi untuk aplikasi Anda sendiri).
- Mendukung fitur "Scope" untuk membatasi izin token (misalnya: token ini cuma boleh baca, tidak boleh nulis).

#### Kapan Pilih Passport?
- Anda membangun platform besar yang membuka API untuk publik/partner eksternal.
- Anda membutuhkan keamanan standar industri OAuth2.
- Anda memiliki banyak aplikasi mikro yang butuh berbagi sistem autentikasi.

### 3. Perbandingan Singkat

| Fitur | Laravel Sanctum | Laravel Passport |
|-------|-----------------|------------------|
| **Setup** | Sangat Mudah | Cukup Rumit |
| **Penyimpanan Token** | Satu Tabel DB | Banyak Tabel DB |
| **OAuth2 Support** | Tidak | Ya (Penuh) |
| **Cocok Untuk** | Mayoritas Project | Project Enterprise |

### Tips Pemilihan

Aturan praktisnya adalah: **Mulailah dengan Sanctum.**
Hampir 90% aplikasi modern yang ada saat ini cukup menggunakan Sanctum. Jika di masa depan aplikasi Anda berkembang pesat dan butuh integrasi OAuth2 dengan partner besar, pindah ke Passport pun tidak mustahil dilakukan.

### Kesimpulan

Memilih sistem keamanan bukan soal mana yang "lebih hebat", tapi mana yang "lebih pas". Jangan biarkan kerumitan menghambat progress coding Anda. Fokuslah pada fitur aplikasi, dan gunakan Sanctum untuk keamanan yang cepat, ringan, namun tetap kokoh.

---

**Artikel Sebelumnya:** [Optimasi Query Database: Membongkar Rahasia Kecepatan Aplikasi Laravel](/posts/optimasi-query-database-lambat/)
**Artikel Selanjutnya:** [Testing Strategi: Mengapa Senior Developer Selalu Menulis Kode Tes?](/posts/testing-strategi-feature-vs-unit/)
