---
title: "Mastering Eloquent Relationships: Strategi Query Database ala Professional Developer"
author: Ilham
description: Menyelami relasi database tingkat lanjut di Laravel. Pelajari Polymorphic, HasManyThrough, dan teknik optimasi relasi untuk aplikasi skala besar.
date: 2025-02-05 10:00:00 +0800
categories: [Programing, Laravel]
tags: [laravel, eloquent, database, advanced]
---

Eloquento ORM adalah salah satu fitur paling dicintai di Laravel. Ia membuat manipulasi database terasa seperti memanipulasi objek PHP biasa. Namun, banyak developer terjebak hanya pada relasi dasar `hasMany` dan `belongsTo`.

Untuk membangun aplikasi yang benar-benar modular dan efisien, Anda perlu menguasai relasi tingkat lanjut yang disediakan oleh Laravel.

---

### 1. Polymorphic Relationships: Satu Model untuk Semua

Bayangkan Anda memiliki fitur "Komentar". Anda ingin sistem komentar ini bisa digunakan di postingan blog, video tutorial, dan produk di toko online Anda.

Tanpa Polymorphic, Anda mungkin akan membuat tabel `post_comments`, `video_comments`, dan `product_comments`. Miris sekali.

**Dengan Polymorphic Relationship**, Anda cukup satu tabel `comments` dengan dua kolom ajaib: `commentable_id` dan `commentable_type`.

```php
// app/Models/Comment.php
class Comment extends Model {
    public function commentable() {
        return $this->morphTo();
    }
}

// app/Models/Post.php
class Post extends Model {
    public function comments() {
        return $this->morphMany(Comment::class, 'commentable');
    }
}
```

### 2. Has Many Through: Meloncati Tabel

Misalkan Anda memiliki data: **Negara** punya banyak **User**, dan **User** punya banyak **Postingan**. Bagaimana cara mengambil semua postingan dari sebuah negara secara langsung?

Gunakan `hasManyThrough`. Ia memungkinkan Anda "melompati" tabel `users` untuk sampai ke `posts`.

```php
// app/Models/Country.php
class Country extends Model {
    public function posts() {
        return $this->hasManyThrough(Post::class, User::class);
    }
}
```

### 3. Many-to-Many dengan Data Tambahan (Pivot Models)

Relasi `belongsToMany` sering digunakan untuk Tag atau Role. Tapi, bagaimana jika Anda ingin menyimpan data tambahan di tabel penghubung (pivot)? Misalnya, tabel `user_role` yang mencatat siapa yang memberikan role tersebut (`assigned_by`).

Anda bisa membuat model khusus untuk tabel pivot tersebut agar lebih mudah dikelola:

```php
// app/Models/User.php
public function roles() {
    return $this->belongsToMany(Role::class)
                ->withPivot('assigned_by')
                ->withTimestamps();
}
```

### 4. Tips Optimasi Relasi

- **Jangan Lupa Eager Loading:** Selalu gunakan `$model->with(['relation'])` jika Anda akan melakukan loop pada data tersebut. Ini mencegah masalah N+1 Query (cek artikel sebelumnya tentang optimasi query).
- **Default Models:** Gunakan `withDefault()` pada relasi `belongsTo` agar aplikasi Anda tidak crash saat mengambil properti dari objek yang ternyata NULL.
  ```php
  public function author() {
      return $this->belongsTo(User::class)->withDefault([
          'name' => 'Anonymous User',
      ]);
  }
  ```

### Kesimpulan

Menguasai relasi Eloquent bukan hanya tentang "memperpendek kode", tapi tentang bagaimana Anda merancang arsitektur database yang sehat. Dengan Polymorphic dan HasManyThrough, aplikasi Anda akan jauh lebih mudah dikembangkan (extendable) di masa depan.

---

**Artikel Sebelumnya:** [Laravel Pulse: Dashboard Monitoring Real-time untuk Performa Aplikasi Anda](/posts/laravel-pulse-monitoring/)
**Artikel Selanjutnya:** [Optimasi Query Database: Membongkar Rahasia Kecepatan Aplikasi Laravel](/posts/optimasi-query-database-lambat/)
