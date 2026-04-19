---
title: "Real-time App dengan Laravel Reverb: Panduan Lengkap Tanpa Layanan Berbayar"
author: Ilham
description: Cara membangun fitur real-time seperti chat dan notifikasi langsung menggunakan Laravel Reverb. Hemat biaya dan kontrol penuh atas data Anda.
date: 2025-02-03 10:00:00 +0800
categories: [Programing, Laravel]
tags: [laravel, websocket, real-time, networking]
---

Membuat aplikasi "real-time" (seperti chat, feed berita, atau notifikasi langsung) dulunya adalah tugas yang menakutkan bagi developer PHP. Kita sering dipaksa membayar mahal untuk layanan seperti Pusher atau menghadapi kerumitan mengelola server Node.js terpisah dengan Socket.io.

Kini, tim Laravel telah merilis **Laravel Reverb**, sebuah server WebSocket berperforma tinggi yang ditulis murni dalam PHP dan terintegrasi langsung ke dalam ekosistem Laravel.

---

### 1. Mengapa Reverb? (Real-time vs Polling)

Sebelum WebSocket populer, kita menggunakan teknik **Polling**—browser akan memanggil API setiap 5 detik untuk cek data baru. Masalahnya:
- Memboroskan resource server.
- Ada penundaan (delay) sebelum data muncul.

**WebSocket** (dan Reverb) memungkinkan server "menghubungi" browser kapan pun ada data baru. Ini disebut koneksi **full-duplex**.

### 2. Persiapan dan Instalasi

Pastikan Anda menggunakan Laravel 11 (atau versi terbaru). Jalankan perintah berikut untuk menginstal semua kebutuhan broadcasting:

```bash
php artisan install:broadcasting
```

Perintah ini akan:
1. Mengatur konfigurasi broadcasting.
2. Menginstal Laravel Echo (untuk sisi frontend).
3. Menginstal Laravel Reverb (untuk sisi server).

### 3. Konfigurasi .env

Pastikan file environment Anda sudah diarahkan ke Reverb:

```env
BROADCAST_CONNECTION=reverb

REVERB_APP_ID=my-app-id
REVERB_APP_KEY=my-app-key
REVERB_APP_SECRET=my-app-secret
REVERB_HOST="localhost"
REVERB_PORT=8080
REVERB_SCHEME=http

VITE_REVERB_APP_KEY="${REVERB_APP_KEY}"
VITE_REVERB_HOST="${REVERB_HOST}"
VITE_REVERB_PORT="${REVERB_PORT}"
VITE_REVERB_SCHEME="${REVERB_SCHEME}"
```

### 4. Membuat Event Real-time

Misalkan kita ingin mengirimkan notifikasi setiap kali ada pesanan baru. Buat sebuah event:

```bash
php artisan make:event NewOrderReceived
```

Edit file event tersebut agar mengimplementasi `ShouldBroadcast`:

```php
class NewOrderReceived implements ShouldBroadcast {
    use Dispatchable, InteractsWithSockets, SerializesModels;

    public $order;

    public function __construct(Order $order) {
        $this->order = $order;
    }

    public function broadcastOn() {
        return new Channel('orders'); // Public channel
    }
}
```

### 5. Menjalankan Server Reverb

Di terminal Anda, jalankan server Reverb agar ia mulai mendengarkan koneksi:

```bash
php artisan reverb:start
```

### 6. Menangkap Notifikasi di Frontend (Laravel Echo)

Di file JavaScript Anda (misalnya `resources/js/app.js`), dengarkan channel `orders`:

```javascript
Echo.channel('orders')
    .listen('NewOrderReceived', (e) => {
        alert('Ada pesanan baru dari ' + e.order.customer_name);
        console.log(e.order);
    });
```

### Kesimpulan & Tips

- **Skalabilitas**: Reverb sangat cepat karena menggunakan library `ReactPHP` di bawah tendanya. 
- **Security**: Untuk data sensitif, selalu gunakan `PrivateChannel` atau `PresenceChannel` yang membutuhkan autentikasi.
- **Biaya**: Karena Reverb berjalan di server Anda sendiri, tidak ada lagi tagihan bulanan berdasarkan jumlah pesan atau koneksi!

---

**Artikel Sebelumnya:** [Arsitektur Clean Code dengan Laravel Action - Memisahkan Logika Bisnis dari Controller](/posts/arsitektur-clean-code-laravel-action/)
**Artikel Selanjutnya:** [Laravel Pulse: Dashboard Monitoring Real-time untuk Performa Aplikasi Anda](/posts/laravel-pulse-monitoring/)
