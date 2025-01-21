---
title: Pengenalan Laravel
author: Ilham
description: Artikel ini akan memperkenalkan ke anda mengenai Laravel secara singkat
date: 2025-01-21 15:40:00 +0800
categories: [Programing, Laravel]
tags: [laravel]
---

## Apa Itu Laravel?
Sebelum masuk ke pembahasan inti mengenai _framework_ ini, kita harus mengetahi hal hal dasar mengenainya terlebih dahulu.
Pada dasarnya Laravel merupakan _framework_ web dari PHP yang memiliki syntax yang ekspresif dan elegen. _framework_ ini menyediakan struktur folder dan juga _base point_ dalam membuat sebuah aplikasi, sehingga kita dapat fokus mengembangkan sesuatu yang luar biasa sementar _laravel_ akan mengerjakan sisanya.

_Framework_ ini mampu memberikan pengalaman yang menakjubkan bagi pengguna, sehingga dapat memberikan fitur yang lengkap dan cukup kuat seperti injeksi dependensi, lapisan abstraksi database, dan masih banyak lagi.

Laravel memiliki basis arsitektur Model-View-Controler (MVC), pendekatan ini memisahkan fungsi antara komponen untuk menciptakan sistem yang lebih terstrukur, rapi, dan mudah untuk dipahami. Dengan menerapkan arsitektur MVC, pengembangan sistem akan menjadi lebih efisien karena tiap model, vie dan controler memiliki logika dan perannya yang jelas. inilah yang menjadi alasan mengapa Laravel menjadi pilihan yang populer di kalangan developer.

## Sejarah Laravel
Laravel dikembangakn oleh Taylor Otwell sebagai bentuk alternatif yan lebih maju dari _framework_ pendahulunya yaitu **Codeigneter**, yang saat itu tidak menyediakan beberapa fitur - fitur yang mendukung seperti otentikasi dan otorisasi pengguna. Laravel versi beta pertama kali dirilis ke publik pada tanggal 9 Juni 2011, pada bulan yang sama Laravel versi 1 juga dirilis. Hingga saat ini Laravel telah mengeluarkan Laravel 11 yang rilis pada 12 Maret 2024. Perilisan ini diumumkan langsung di website resmi dan juga sosial media dari Laravel. Hal mengenai perilisan ini juga dibahas dengan detail pada Laracon di Amsterdam pada 5 hingga 6 Februari lalu.

Untuk versi paling baru dari Laravel diperkirakan akan rilis pada Kuartal pertama tahun 2025 dan akan dilakukan pemeliharaan terhadap bug hingga kuartal ketiga tahun 2026 dan perberbaikan keamanan hingga kuartal pertama tahun 2027. Untuk kompabilitasnya Laravel 12 diperkirakan dapat berjalan pada PHP Versi 8.2 - 8.3.

## Kenapa Harus Laravel dan Bukan PHP Native?
Terdapat beberapa hal yang perlu dijadikan alasan mengapa kebanyakan orang lebih memeilih menggunakan Laravel daripada menggunakan PHP secara langsung. Alasan tersebut diantara lain:

1. Kecepatan dalam pengembangan, hal ini didukung karena Laravel telah menyediakan beberapa fitur bawaan seperti Eloquent ORM untuk mempermudah interakasi antara database, Blade Template Engine yang mempermudah pengelolaan tampilan, Routing yang mudah serta kemampuan melakukan Migration dan Seedeng ke database dengan cepat tanpa harus membuat tabel secara manual.
2. Fitur Keamanan yang lebih baik, dibandingkan dengan PHP Native laravel telah memberikan berbagai jenis fitur keamanan seperti Proteksi _Cross-Site Request Forgery_ atau CSRF, Proteksi terhadap SQL Injection, Proteksi terhadap _Cross-Site Scripting_ dan Autentikasi serta otorisasi yang mudah diimplementasikan.
3. Struktur MVC, seperti yang dijelkaskan sebelumnya bahwa laravel menerapkan Arsitektur MVC yang mampu mempermudah developer dalam mengembangkan sistem. hal ini didukung dari pemisahan antara Model, View dan Controlle.
4. Performa dan Skalabilitas yang tinggi. Jika dibandingkan dengan PHP Native, Laravel berbagai fitur speerti caching, queue dan event broadcasting yang telah dibuat langsung tanpa perlu membuatnya secara menual. sementara PHP Native memerlukan upaya lebih banyak untuk mengimplemntasikan fitur ini secara manual.
5. Eksistem dan Dokumentasi yang lengkap, hal ini didukung oleh besarnya komunitas laravel dan juga tersedianya banyak _packages_ yang mampu mempercepat pengembangan aplikasi web. sementara untuk PHP Native untuk beberapa kasus mengharuskan developer untuk mencari solusi sendiri dari berbagai sumber.
6. Pengujian yang mudah, laravel telah menyediakan tools bawaannya sendiri untuk melakukan unit testing dan feature testing menggunakan PHPUnit secara langsung, disisi lain PHP harus membuat pengujian sendiri tanpa _framework_ pendukung.
7. Alasan terkahir antara laravel memberikan kemudahan bagi developer untuk melakukan integrasi dengan teknologi lain seperti AWS dan Google Cloud.

## Kesimpulan
Pada dasarnya penggunaan Laravel dapat membantu dalam proses pengembangan website lebih mudah dan lebih cepat. Namun developer harus memiliki pengetahuan dasar mengenai sintaksis PHP, karena Laravel sendiri merupakan _Framework_ yang dikembangkan unuk Laravel.

Untuk Materi kali ini, saya penulis akan mengajak teman teman sekalian untuk memahami lebih dalam mengenai Laravel dengan membuat sebuah projek API CRUD untuk mengelola data buku. Materi dari projek ini akan memberikan pengetahuan dasar mengenai Laravel, dengan harapan teman - teman sekalian mampu memahami dasar dari laravel.

## Daftar Pustaka

1. "Instalation Laravel" Laravel, 21-01-2025. [Laravel](https://laravel.com/docs/11.x)
2. "Laravel" Wikipedia, 21-01-2025. [Wikipedia](https://en.wikipedia.org/wiki/Laravel)

