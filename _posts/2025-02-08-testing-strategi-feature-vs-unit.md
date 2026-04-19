---
title: "Testing Strategi: Mengapa Senior Developer Selalu Menulis Kode Tes?"
author: Ilham
description: Memahami pentingnya Automated Testing di Laravel. Pelajari perbedaan Unit Test dan Feature Test, serta cara membangun kebiasaan TDD (Test Driven Development).
date: 2025-02-08 10:00:00 +0800
categories: [Programing, Laravel]
tags: [laravel, testing, quality-assurance, professional-dev]
---

"Tes otomatis membuang-buang waktu. Saya bisa mengetesnya sendiri di browser."

Ini adalah kalimat yang sering diucapkan oleh Junior Developer. Namun, saat project mulai besar dan memiliki ribuan baris kode, mengubah satu baris saja bisa merusak fitur di tempat lain yang tidak terduga. Di sinilah **Automated Testing** di Laravel menjadi penyelamat hidup Anda.

---

### 1. Mengapa Kita Butuh Testing?

Testing bukan tentang mencari bug sekarang, tapi tentang **mencegah bug di masa depan**.
- **Refactoring Tanpa Takut:** Anda bisa mengubah kode lama dengan tenang. Jika tes tetap hijau (pass), berarti fungsi aplikasi tidak berubah.
- **Dokumentasi Hidup:** Tes menunjukkan bagaimana sebuah fitur seharusnya bekerja.
- **Hemat Waktu:** Capek mengisi form register berkali-kali untuk tes manual? Biarkan kode yang melakukannya dalam 1 detik.

### 2. Unit Test: Fokus pada Hal Kecil

Unit Test digunakan untuk menguji fungsionalitas terkecil dari aplikasi, biasanya satu baris logic atau satu method di sebuah kelas.

**Karakteristik:**
- Tidak boleh menyentuh database.
- Tidak boleh menyentuh jaringan (internet).
- Sangat cepat (eksekusi ratusan tes dalam sekejap).

```php
public function test_calculate_discount_logic() {
    $price = 100000;
    $discount = (new DiscountService())->calculate($price);
    
    $this->assertEquals(90000, $discount);
}
```

### 3. Feature Test: Menguji Keseluruhan Fitur

Feature Test (dulu disebut Functional Test) mensimulasikan bagaimana user berinteraksi dengan aplikasi. Ia mengirimkan HTTP Request dan mengecek HTTP Response.

**Karakteristik:**
- Boleh menggunakan database (menggunakan Database Transactions).
- Menguji alur dari Route -> Middleware -> Controller -> Model -> Response.

```php
public function test_user_can_login_with_correct_credentials() {
    $user = User::factory()->create([
        'password' => bcrypt('rahasia123'),
    ]);

    $response = $this->post('/login', [
        'email' => $user->email,
        'password' => 'rahasia123',
    ]);

    $response->assertRedirect('/dashboard');
    $this->assertAuthenticatedAs($user);
}
```

### 4. Kapan Menggunakan Mana?

- **Gunakan Unit Test** untuk logika bisnis yang rumit (seperti rumus matematika, parsing teks, atau pengolahan data).
- **Gunakan Feature Test** untuk alur utama aplikasi (Sign Up, Checkout, Posting Artikel).

**Aturan Emas:** Jika Anda baru memulai, fokuslah buat **Feature Test** sebanyak-banyaknya. Ini memberikan perlindungan paling luas untuk aplikasi Anda dengan usaha yang paling minimal.

### 5. Menjalankan Tes

Laravel sudah menyertakan PHPUnit secara default. Anda cukup menjalankan:
```bash
php artisan test
```

### Kesimpulan

Menulis tes adalah investasi. Mungkin di awal terasa lambat, tapi saat aplikasi Anda berjalan 2 tahun kemudian dan Anda harus mengganti sistem payment, Anda akan sangat bersyukur memiliki barisan tes yang menjaga aplikasi Anda tetap stabil.

---

**Artikel Sebelumnya:** [Keamanan API di Laravel: Pilih Sanctum atau Passport?](/posts/keamanan-api-sanctum-vs-passport/)
**Artikel Selanjutnya:** [Custom Middleware: Cara Pintar Mengamankan dan Memproses Request di Laravel](/posts/custom-middleware-workflow-kompleks/)
