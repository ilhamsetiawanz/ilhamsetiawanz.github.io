---
title: Arsitektur Clean Code dengan Laravel Action - Memisahkan Logika Bisnis dari Controller
author: Ilham
description: Pelajari teknik Senior Developer dalam merapikan Controller yang gemuk (Fat Controller) menggunakan pattern Action. Tingkatkan keterbacaan dan usabilitas kode Anda.
date: 2025-02-02 10:00:00 +0800
categories: [Programing, Laravel]
tags: [laravel, clean-code, design-pattern, best-practices]
---

Salah satu masalah klasik dalam pengembangan aplikasi Laravel adalah **Fat Controller**—sebuah controller yang memiliki ratusan baris kode karena menangani validasi, logika bisnis, interaksi database, pengiriman email, hingga response dalam satu tempat.

Bagaimana cara mengatasinya? Jawabannya adalah dengan menggunakan **Action Pattern**.

---

### 1. Masalah: Controller yang Terlalu Gemuk

Bayangkan Anda memiliki sistem pendaftaran user. Di dalam Controller, kodenya tampak seperti ini:

```php
public function store(Request $request) {
    // 1. Validasi
    $validated = $request->validate([...]);

    // 2. Buat User
    $user = User::create($validated);

    // 3. Beri Role default
    $user->assignRole('customer');

    // 4. Kirim Email Selamat Datang
    Mail::to($user)->send(new WelcomeMail($user));

    // 5. Beri Bonus Poin Pendaftaran
    $user->points()->create(['amount' => 100]);

    return response()->json(['message' => 'User registered']);
}
```

Sekilas tidak ada masalah. Tapi, apa yang terjadi jika Anda ingin membuat user dari perintah CMD (Artisan Command) atau dari API mobile? Anda harus **copy-paste** logika langkah 2 sampai 5 ke tempat lain. Ini adalah mimpi buruk untuk maintenance.

### 2. Solusi: Action Pattern

Action adalah sebuah kelas PHP sederhana (Plain Old PHP Object) yang hanya melakukan **satu tugas spesifik** dalam domain bisnis Anda.

#### Mengapa menggunakan Action?
- **Reusable**: Bisa dipanggil dari Controller, Command, API, maupun Job.
- **Testable**: Sangat mudah untuk membuat unit test untuk satu action saja.
- **Readable**: Nama file menunjukkan fitur (misalnya: `CreateUserAction.php`).

### 3. Implementasi Langkah-Demi-Langkah

Mari kita buat folder `app/Actions` dan buat file `CreateUserAction.php`.

```php
namespace App\Actions;

use App\Models\User;
use App\Mail\WelcomeMail;
use Illuminate\Support\Facades\Mail;

class CreateUserAction {
    public function execute(array $data): User {
        $user = User::create($data);
        $user->assignRole('customer');
        $user->points()->create(['amount' => 100]);
        
        Mail::to($user)->send(new WelcomeMail($user));

        return $user;
    }
}
```

Sekarang, Controller Anda menjadi sangat ramping:

```php
public function store(Request $request, CreateUserAction $createUserAction) {
    $validated = $request->validate([...]);
    
    $user = $createUserAction->execute($validated);

    return response()->json(['message' => 'User registered', 'user' => $user]);
}
```

### 4. Tips Profesional dalam Menggunakan Action

- **Nama Method**: Gunakan nama method yang konsisten, misalnya `execute()` atau `handle()`. Di Laravel community, `execute()` sangat populer.
- **Dependency Injection**: Anda bisa menggunakan Service Container di dalam constructor Action untuk menyuntikkan service lain.
- **Handle Validasi**: Tetap biarkan Controller (atau FormRequest) menangani validasi HTTP, sementara Action fokus pada logika bisnis murni.

### Kesimpulan

Dengan memindahkan logika "cara kerja aplikasi" ke dalam Action, Anda tidak hanya merapikan Controller, tetapi juga membangun fondasi aplikasi yang siap untuk skala besar. Kode Anda menjadi lebih modular, mudah dipindahkan, dan sangat mudah untuk diperbaiki jika ada bug.

---

**Artikel Sebelumnya:** [Keajaiban Service Container di Laravel - Panduan Terlengkap](/posts/keajaiban-service-container/)
**Artikel Selanjutnya:** [Real-time App dengan Laravel Reverb: Panduan Lengkap Tanpa Layanan Berbayar](/posts/real-time-app-laravel-reverb/)
