---
title: "Jobs & Queues: Memproses Tugas Berat di Background tanpa Membuat User Menunggu"
author: Ilham
description: Pelajari cara mempercepat respon aplikasi Laravel dengan memindahkan tugas berat seperti pengiriman email dan pengolahan gambar ke sistem antrean (Queue).
date: 2025-02-11 10:00:00 +0800
categories: [Programing, Laravel]
tags: [laravel, queue, background-jobs, performance, redis]
---

Pernahkah Anda menekan tombol "Daftar" di sebuah website, lalu browser berputar selama 10 detik sebelum akhirnya muncul pesan "Sukses"? Kemungkinan besar, website tersebut sedang mengirimkan email aktivasi di tengah-tengah proses pendaftaran.

Bagi user, menunggu 10 detik adalah pengalaman buruk. Di dunia profesional, kita menggunakan **Jobs & Queues** untuk menangani masalah ini.

---

### 1. Konsep Dasar: Sinkron vs Asinkron

- **Sinkron (Default):** User menunggu sampai email selesai dikirim, barulah halaman sukses muncul.
- **Asinkron (Queue):** User langsung melihat halaman sukses. Tugas pengiriman email diletakkan di "Antrean" dan akan dikerjakan oleh server di belakang layar (background).

### 2. Persiapan Driver Queue

Agar bisa menggunakan queue, Laravel butuh tempat untuk menyimpan antrean tersebut. Pilihan populer adalah:
1. **Database** (Mudah untuk pemula).
2. **Redis** (Sangat cepat, standar industri).

Ubah `.env` Anda:
```env
QUEUE_CONNECTION=database
```
Lalu buat tabel untuk menampung antrean:
```bash
php artisan queue:table
php artisan migrate
```

### 3. Membuat Job Pertama Anda

Mari kita buat Job untuk mengirim email selamat datang:

```bash
php artisan make:job SendWelcomeEmail
```

Edit file `app/Jobs/SendWelcomeEmail.php`:

```php
public function handle(): void
{
    // Logika pengiriman email yang berat diletakkan di sini
    Mail::to($this->user)->send(new WelcomeMail($this->user));
}
```

### 4. Memasukkan Tugas ke Antrean (Dispatching)

Di Controller Anda, alih-alih mengirim email secara langsung, Anda cukup memanggil Job tersebut:

```php
public function store(Request $request) {
    $user = User::create($request->all());

    // Masukkan ke antrean, kirim respon ke user seketika itu juga!
    SendWelcomeEmail::dispatch($user);

    return redirect()->back()->with('success', 'Pendaftaran berhasil!');
}
```

### 5. Menjalankan Worker

Job yang sudah masuk ke antrean tidak akan berjalan otomatis. Anda harus menjalankan "Worker" untuk mulai memprosesnya:

```bash
php artisan queue:work
```

### Tips Profesional: Menangani Kegagalan

Terkadang transmisi email gagal karena server down. Laravel bisa mencoba kembali (retry) secara otomatis:

```php
public $tries = 3; // Coba maksimal 3 kali jika gagal
public $backoff = 60; // Tunggu 60 detik sebelum coba lagi
```

Jika tetap gagal setelah 3 kali, job akan masuk ke tabel `failed_jobs` sehingga Anda bisa memperbaikinya nanti tanpa kehilangan data.

### Kesimpulan

Queues adalah kunci untuk membuat aplikasi Laravel yang terasa "ringan" dan responsif. Dengan memindahkan beban kerja ke background, server Anda bisa melayani lebih banyak user secara bersamaan dengan pengalaman yang jauh lebih mulus.

---

**Artikel Sebelumnya:** [Menangani File Upload di S3: Skalabilitas Storage untuk Aplikasi Modern](/posts/upload-file-s3-laravel/)
**Artikel Selanjutnya:** [Laravel Folio & Volt: Revolusi Frontend yang Membuat Coding Lebih Menyenangkan](/posts/laravel-folio-volt-frontend/)
