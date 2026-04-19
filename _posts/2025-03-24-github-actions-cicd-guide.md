---
title: "CI/CD dengan GitHub Actions: Automasi dari Commit hingga ke Tangan User"
author: Ilham
description: Pelajari cara membangun jalur automasi (pipeline) menggunakan GitHub Actions. Pastikan setiap kode yang di-push otomatis dites dan dideploy ke server.
date: 2025-03-24 10:00:00 +0800
categories: [Programing, DevOps]
tags: [github-actions, cicd, automation, devops, workflow]
---

Di jaman dulu, setelah selesai koding, developer harus mengunggah file secara manual via FTP, lalu menjalankan perintah migrasi database secara manual di server. Proses ini lambat dan sangat rawan *human error*. 

Sekarang, kita menggunakan **CI/CD (Continuous Integration / Continuous Deployment)**. Dengan CI/CD, semua proses tersebut dilakukan secara otomatis oleh robot sesaat setelah Anda melakukan `git push`. Alat terpopuler untuk melakukan ini adalah **GitHub Actions**.

---

### 1. Apa itu CI dan CD?

- **Continuous Integration (CI):** Setiap kali ada kode baru, sistem otomatis menjalankan **Testing** dan **Linting** (cek gaya koding). Jika tes gagal, kode tidak boleh digabungkan ke cabang utama.
- **Continuous Deployment (CD):** Jika tes berhasil, sistem akan otomatis mengirimkan kode tersebut ke server produksi (Production) tanpa campur tangan manusia.

### 2. Anatomi GitHub Actions (Workflow)

Untuk membuat automasi di GitHub, Anda cukup membuat folder `.github/workflows` dan menambahkan file `.yml` di dalamnya.

Komponen utamanya:
- **Events:** Apa yang memicu automasi? (Misal: `on: push`).
- **Jobs:** Kumpulan tugas yang ingin dijalankan (Misal: `build`, `test`, `deploy`).
- **Steps:** Langkah-langkah detail di dalam job.

### 3. Contoh Workflow Sederhana untuk PHP

```yaml
name: Laravel Test Workflow

on: [push]

jobs:
  laravel-tests:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Setup PHP
        uses: shivammathur/setup-php@v2
        with:
          php-version: '8.3'
      - name: Install dependencies
        run: composer install --no-ansi --no-interaction --no-scripts --no-progress
      - name: Run Tests
        run: php artisan test
```

### 4. Deployment Otomatis (CD)

Setelah tes lulus, Anda bisa menambahkan job untuk login ke server via SSH dan menjalankan perintah `git pull`. 

**Tips Keamanan:** Jangan pernah menulis password SSH langsung di file `.yml`. Gunakan fitur **GitHub Secrets** untuk menyimpan data sensitif tersebut.

### 5. Mengapa Ini Penting?

1. **Kepercayaan Diri:** Anda tahu kode Anda tidak akan merusak produksi karena sudah lolos tes otomatis.
2. **Kecepatan:** Tim bisa merilis fitur berkali-kali dalam sehari dengan sangat mudah.
3. **Standar Kualitas:** Semua developer dipaksa mengikuti aturan koding dan testing yang sama.

### Kesimpulan

GitHub Actions mengubah GitHub dari sekadar "tempat simpan kode" menjadi "stasiun automasi" yang bertenaga. Dengan menguasai CI/CD, Anda tidak hanya menjadi developer yang lebih produktif, tapi juga membangun sistem yang jauh lebih stabil dan profesional bagi user Anda.

---

**Artikel Sebelumnya:** [Dockerizing Anything: Panduan Lengkap Container untuk Pemula](/posts/docker-for-beginners-guide/)
**Artikel Selanjutnya:** [Kubernetes untuk Personal Project: Apakah Terlalu Berlebihan?](/posts/kubernetes-for-personal-projects/)
