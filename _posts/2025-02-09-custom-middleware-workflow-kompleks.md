---
title: "Custom Middleware: Cara Pintar Mengamankan dan Memproses Request di Laravel"
author: Ilham
description: Pelajari teknik membuat Middleware kustom untuk berbagai kebutuhan, mulai dari pembatasan akses berdasarkan role hingga logging aktivitas user secara cerdas.
date: 2025-02-09 10:00:00 +0800
categories: [Programing, Laravel]
tags: [laravel, middleware, security, request-lifecycle]
---

Bayangkan aplikasi Anda sebagai sebuah klub malam yang eksklusif. Middleware di Laravel adalah "Bouncer" atau penjaga pintu yang berada tepat di depan pintu masuk. 

Setiap orang (HTTP Request) yang ingin masuk ke dalam klub (Controller) harus melewati penjaga ini. Sang penjaga bisa memeriksa apakah mereka punya kartu identitas, apakah mereka sudah membayar tiket, atau bahkan sekadar menyapa dan mencatat siapa saja yang datang.

---

### 1. Kapan Anda Butuh Middleware Kustom?

Meskipun Laravel sudah menyediakan middleware bawaan seperti `auth` dan `guest`, Anda akan membutuhkan middleware sendiri jika memiliki aturan bisnis yang unik, misalnya:
- Memastikan user memiliki langganan aktif sebelum mengakses kursus.
- Memastikan user memiliki alamat IP Indonesia (Geo-blocking).
- Mencatat log setiap kali ada yang mengubah data sensitif.
- Mengubah format input (misalnya: membuat semua teks nama menjadi Huruf Kapital).

### 2. Dasar Pembuatan Middleware

Mari kita buat sebuah middleware sederhana untuk mendeteksi apakah user adalah seorang Admin.

```bash
php artisan make:middleware EnsureUserIsAdmin
```

Laravel akan membuat file di `app/Http/Middleware/EnsureUserIsAdmin.php`.

### 3. Struktur Logic Middleware

Di dalam file tersebut, Anda akan menemukan method `handle`. Di sinilah "pemeriksaan" dilakukan.

```php
public function handle(Request $request, Closure $next)
{
    // 1. Sebelum Request sampai ke Controller
    if (! $request->user() || ! $request->user()->is_admin) {
        // Jika bukan admin, tendang ke halaman home
        return redirect('home')->with('error', 'Akses ditolak!');
    }

    // 2. Jika lolos, biarkan lanjut ke "ruangan" berikutnya
    return $next($request);
}
```

### 4. Mendaftarkan Middleware

Sejak Laravel 11, pendaftaran middleware menjadi jauh lebih simpel di file `bootstrap/app.php`:

```php
->withMiddleware(function (Middleware $middleware) {
    $middleware->alias([
        'admin' => \App\Http\Middleware\EnsureUserIsAdmin::class,
    ]);
})
```

Sekarang Anda bisa menggunakannya di file `routes/web.php`:

```php
Route::get('/admin-dashboard', function () {
    // Hanya bisa diakses jika lolos middleware 'admin'
})->middleware('admin');
```

### 5. Middleware Parameter

Terkadang kita butuh middleware yang lebih fleksibel. Misalnya: `middleware('check_role:editor')`.

```php
public function handle($request, Closure $next, $role)
{
    if (! $request->user()->hasRole($role)) {
        abort(403);
    }
    return $next($request);
}
```

### Kesimpulan

Middleware adalah cara yang sangat bersih dan terpusat untuk mengelola keamanan serta manipulasi request. Dengan menggunakan middleware kustom, Anda tidak perlu lagi menulis kode cek permission yang berulang-ulang di setiap Controller (**Don't Repeat Yourself**). Kode Anda menjadi lebih modular, mudah dipelihara, dan aplikasi Anda menjadi lebih aman.

---

**Artikel Sebelumnya:** [Testing Strategi: Mengapa Senior Developer Selalu Menulis Kode Tes?](/posts/testing-strategi-feature-vs-unit/)
**Artikel Selanjutnya:** [Menangani File Upload di S3: Skalabilitas Storage untuk Aplikasi Modern](/posts/upload-file-s3-laravel/)
