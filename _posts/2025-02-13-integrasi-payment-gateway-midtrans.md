---
title: "Integrasi Payment Gateway (Midtrans) di Laravel: Panduan Lengkap Transaksi Online"
author: Ilham
description: Cara menghubungkan aplikasi Laravel Anda dengan Midtrans untuk menerima pembayaran otomatis lewat Bank Transfer, E-Wallet, dan Kartu Kredit.
date: 2025-02-13 10:00:00 +0800
categories: [Programing, Laravel]
tags: [laravel, payment-gateway, midtrans, fintech, ecommerce]
---

Membangun sistem pembayaran sendiri adalah hal yang sangat berisiko dan rumit. Itulah sebabnya kita menggunakan **Payment Gateway**. Di Indonesia, **Midtrans** adalah salah satu pilihan terbaik bagi developer karena dokumentasinya yang lengkap dan proses integrasi yang relatif mudah.

Artikel ini akan memandu Anda mengintegrasikan Midtrans ke dalam aplikasi Laravel menggunakan **Snap**, cara termudah untuk menampilkan popup pembayaran.

---

### 1. Dapatkan API Key di Dashboard Midtrans

Daftar di [Midtrans Sandbox](https://dashboard.sandbox.midtrans.com/) untuk mendapatkan:
- **Client Key**
- **Server Key**

Simpan di file `.env` Anda:
```env
MIDTRANS_SERVER_KEY=SB-Mid-server-xxxxxxxxxxxx
MIDTRANS_CLIENT_KEY=SB-Mid-client-xxxxxxxxxxxx
MIDTRANS_IS_PRODUCTION=false
```

### 2. Instalasi Midtrans PHP Library

Gunakan Composer untuk menginstal library resmi dari Midtrans:
```bash
composer require midtrans/midtrans-php
```

### 3. Setup Konfigurasi di Laravel

Agar lebih bersih, buatlah file config baru di `config/midtrans.php`:

```php
return [
    'server_key' => env('MIDTRANS_SERVER_KEY'),
    'client_key' => env('MIDTRANS_CLIENT_KEY'),
    'is_production' => env('MIDTRANS_IS_PRODUCTION', false),
    'is_sanitized' => true,
    'is_3ds' => true,
];
```

### 4. Membuat Controller Transaksi

Di sini kita akan membuat "Snap Token" yang akan digunakan oleh frontend untuk menampilkan popup pembayaran.

```php
public function checkout(Request $request) {
    \Midtrans\Config::$serverKey = config('midtrans.server_key');
    \Midtrans\Config::$isProduction = config('midtrans.is_production');

    $params = [
        'transaction_details' => [
            'order_id' => 'ORDER-' . uniqid(),
            'gross_amount' => 100000, // Harga produk
        ],
        'customer_details' => [
            'first_name' => $request->user()->name,
            'email' => $request->user()->email,
        ],
    ];

    $snapToken = \Midtrans\Snap::getSnapToken($params);
    return view('checkout', compact('snapToken'));
}
```

### 5. Menampilkan Popup di Frontend

Di file Blade Anda (`checkout.blade.php`), tambahkan script Snap:

```html
<script src="https://app.sandbox.midtrans.com/snap/snap.js" data-client-key="{{ config('midtrans.client_key') }}"></script>
<button id="pay-button">Bayar Sekarang</button>

<script>
    document.getElementById('pay-button').onclick = function(){
        snap.pay('{{ $snapToken }}', {
            onSuccess: function(result){ alert("Pembayaran Sukses!"); console.log(result); },
            onPending: function(result){ alert("Menunggu Pembayaran..."); console.log(result); },
            onError: function(result){ alert("Pembayaran Gagal!"); console.log(result); }
        });
    };
</script>
```

### 6. Penting: Menangani Webhook (Notification)

Setelah user membayar, Midtrans akan mengirimkan data ke aplikasi Anda melalui **Webhook**. Anda harus membuat rute POST di `api.php` untuk menangkap data ini dan mengubah status transaksi di database Anda menjadi 'Paid'.

### Kesimpulan

Integrasi Midtrans memungkinkan Anda fokus pada fitur utama aplikasi, sementara urusan keamanan transaksi dan integrasi bank diserahkan pada ahlinya. Dengan Snap, Anda bisa memiliki sistem pembayaran berstandar industri hanya dalam hitungan jam.

---

**Artikel Sebelumnya:** [Laravel Folio & Volt: Revolusi Frontend yang Membuat Coding Lebih Menyenangkan](/posts/laravel-folio-volt-frontend/)
**Artikel Selanjutnya:** [Deployment Laravel ke VPS: Panduan Manual vs Menggunakan Laravel Forge](/posts/deployment-laravel-vps-forge/)
