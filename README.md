# Kantin Multi-Tenant

Aplikasi web **Kantin Multi-Tenant** menggunakan Laravel 13 dengan Livewire 4 sebagai starter kit.


# 1. Requirements

### Software

| Software          | Versi / Keterangan              |
| ----------------- | ------------------------------- |
| PHP               | **8.3 atau lebih baru**         |
| Laravel           | **13.x**                        |
| Composer          | Versi yang mendukung Laravel 13 |
| Node.js           | Dibutuhkan untuk frontend asset |
| NPM               | Package manager JavaScript      |
| Git               | Version control                 |
| MariaDB           | Database transaksi              |
| Redis             | Session, cache, cart, dan queue |
| Livewire          | **4.x**                         |
| Reverb            | Laravel WebSocket / realtime    |

# 2. Tahap 1 — Persiapan Repository

## 2.1 Membuat Folder Project

Gunakan nama folder tanpa spasi agar lebih mudah digunakan melalui terminal.

Contoh:

```bash
mkdir kantin-multi-tenant
cd kantin-multi-tenant
```

## 2.2 Repository Git

Inisialisasi Git:

```bash
git init
```

Atur branch utama:

```bash
git branch -M main
```

Konfigurasi Git jika belum dilakukan:

```bash
git config --global user.name "rezahuditama-web"
git config --global user.email "rezahuditama@gmail.com"
```

------

# 3. Tahap 2 — Membuat Proyek Laravel 13

Tahap ketiga digunakan untuk membuat aplikasi Laravel 13 dengan starter kit Livewire.

## 3.1 Membuat Project

Jalankan:

```bash
 composer create-project -–prefer-dist laravel/laravel kantin-multi-tenant
```

Pada proses instalasi:

* Pilih **Livewire** sebagai starter kit.

## 3.2 Masuk ke Project

```bash
cd kantin-multi-tenant
```

## 3.3 Install Dependency Frontend

```bash
npm install
```

## 3.4 Build Asset

```bash
npm run build
```
---

# 4. Tahap 3 — Konfigurasi MariaDB dan Redis

Tahap keempat menghubungkan Laravel dengan MariaDB dan Redis melalui `.env`.

## 4.1 Membuat File Environment

Jika `.env` belum tersedia:

```bash
copy .env.example .env
```

## 4.2 Generate Application Key

```bash
php artisan key:generate
```

## 4.3 Konfigurasi Database

Contoh konfigurasi `.env`:

```env
DB_CONNECTION=mariadb
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=project
DB_USERNAME=root
DB_PASSWORD=ServBay.dev
```


## 4.4 Konfigurasi Redis

Contoh:

```env
SESSION_DRIVER=database
SESSION_LIFETIME=120
SESSION_ENCRYPT=false
SESSION_PATH=/
SESSION_DOMAIN=null

BROADCAST_CONNECTION=log
FILESYSTEM_DISK=local
QUEUE_CONNECTION=database

CACHE_STORE=database
# CACHE_PREFIX=

MEMCACHED_HOST=127.0.0.1

REDIS_CLIENT=phpredis
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379
```

Redis digunakan untuk:

* Session
* Cache
* Cart
* Queue

Sedangkan MariaDB tetap menjadi sumber data utama untuk data transaksional.

## 4.5 Konfigurasi Broadcasting

Untuk Reverb:

```env
BROADCAST_CONNECTION=reverb
REVERB_APP_ID=762007
REVERB_APP_KEY=svbylocalkey6777
REVERB_APP_SECRET=svbylocalsecret7777
REVERB_HOST="localhost"
REVERB_PORT=8080
REVERB_SCHEME=http
```

## 4.6 Membuat Database

Buat database bernama:

```text
project
```

## 4.7 Menjalankan Migration

```bash
php artisan migrate
```

Jika database masih kosong dan ingin menjalankan migration serta seeder dari awal:

```bash
php artisan migrate:fresh --seed
```

## 4.8 Verifikasi MariaDB

```bash
php artisan db:show
```

Jika berhasil, Laravel dapat terhubung dengan database.

# 5. Tahap 4 — Instalasi Livewire dan Reverb

Tahap kelima memastikan Livewire dan Reverb dapat digunakan untuk kebutuhan interaksi realtime.

## 5.1 Memastikan Livewire

Jika Livewire belum tersedia:

```bash
composer require livewire/livewire
```

Periksa package:

```bash
composer show livewire/livewire
```

## 5.2 Install Broadcasting

Jalankan:

```bash
php artisan install:broadcasting
```

Isi konfigurasi Reverb sesuai environment lokal.

## 5.3 Menjalankan Reverb

Jika Reverb belum berjalan melalui script development:

```bash
php artisan reverb:start
```

## 5.4 Menjalankan Development Server

Jalankan:

```bash
composer run dev
```

Proses development dapat menjalankan beberapa komponen sekaligus:

* HTTP server
* Vite
* Queue worker
* Reverb

Jika Reverb tidak termasuk dalam script `composer run dev`, jalankan pada terminal terpisah:

```bash
php artisan reverb:start
```

## 5.5 Pengujian Login

Buka:

```text
http://localhost:8000
```

Kemudian uji:

1. Registrasi
2. Login
3. Logout

Pantau juga:

* Browser console
* Laravel log
* Terminal Reverb
* Terminal queue worker

---

# 6. Tahap 5 — Quality Gate dan Baseline Test

Tahap keenam memastikan aplikasi memenuhi pemeriksaan dasar sebelum kode dianggap siap.

## 6.1 Menjalankan Tes

```bash
php artisan test
```

Target:

```text
Tests: ... passed
```

Contoh hasil yang ditunjukkan modul:

```text
Tests: 33 passed (81 assertions)
```

Hasil tersebut merupakan bukti eksekusi pada lingkungan modul, bukan jaminan jumlah test yang sama pada setiap instalasi.

## 6.2 Menjalankan Laravel Pint

Periksa format kode:

```bash
./vendor/bin/pint --test
```

Pada Windows jika perintah tersebut tidak berjalan, dapat menggunakan:

```cmd
vendor\bin\pint.bat --test
```

Jika ditemukan masalah format, jalankan:

```bash
./vendor/bin/pint
```

Kemudian periksa kembali:

```bash
./vendor/bin/pint --test
```

## 6.3 Build Frontend

```bash
npm run build
```

Pastikan tidak terdapat error asset.

## 6.4 Memastikan `.env` Tidak Masuk Git

Periksa:

```bash
git status
```

`.env` harus berada di dalam `.gitignore`.

Pastikan tidak ada credential atau secret yang akan di-commit:

```bash
git diff --cached
```

File `.env` berisi konfigurasi khusus mesin lokal seperti credential database dan token sehingga tidak boleh dimasukkan ke repository. Gunakan `.env.example` untuk placeholder yang aman.

## 6.5 Quality Gate

Semua pemeriksaan berikut harus berhasil:

```bash
php artisan test
```

```bash
npm run build
```

```bash
./vendor/bin/pint --test
```

Dan:

```bash
git status
```

harus menunjukkan bahwa tidak ada secret yang akan di-commit.

---

# 7. Run Application

Setelah Tahap 1 sampai Tahap 6 selesai, jalankan aplikasi dengan:

```bash
composer run dev
```

Kemudian buka:

```text
http://localhost:8000
```

Jika `composer run dev` tidak menjalankan Reverb secara otomatis, gunakan terminal lain:

```bash
php artisan reverb:start
```

Untuk memastikan database dapat digunakan:

```bash
php artisan db:show
```

Untuk memastikan Redis:

```bash
redis-cli -p 6379 ping
```

Output:

```text
PONG
```

---

# 9. Kriteria Selesai

Setup Modul 1 dianggap selesai apabila checklist berikut terpenuhi:

* [ ] PHP versi 8.3 atau lebih baru terdeteksi.
* [ ] Composer dapat digunakan.
* [ ] Node.js dan NPM dapat digunakan.
* [ ] Git dapat digunakan.
* [ ] Extension PHP yang diperlukan tersedia.
* [ ] Repository `kantin-multi-tenant` telah dibuat.
* [ ] Dokumentasi SRS, mockup, ERD, dan SQL berada di `docs/`.
* [ ] Laravel Framework 13.x terdeteksi.
* [ ] Starter kit Livewire berhasil digunakan.
* [ ] Halaman login dapat dirender.
* [ ] MariaDB dapat terhubung.
* [ ] Redis memberikan respons `PONG`.
* [ ] Migration berhasil dijalankan.
* [ ] Livewire 4 terpasang.
* [ ] Reverb dapat dijalankan.
* [ ] HTTP server berjalan.
* [ ] Vite berjalan.
* [ ] Queue worker berjalan.
* [ ] Registrasi, login, dan logout dapat diuji.
* [ ] `php artisan test` berhasil.
* [ ] Pint berhasil.
* [ ] `npm run build` berhasil.
* [ ] `.env` tidak masuk Git.
* [ ] Tidak ada secret di staging area.
* [ ] README dapat digunakan untuk melakukan setup ulang.

Kriteria utama modul mencakup Laravel 13/PHP ≥8.3, registrasi/login, migrasi dan Redis, test/build/formatter, serta README yang dapat diikuti ulang.

---

## Quick Start

Jika seluruh requirements telah tersedia, alur singkatnya:

```bash
# 1. Buat project
composer global require laravel/installer
laravel new kantin-multi-tenant

# 2. Masuk project
cd kantin-multi-tenant

# 3. Install frontend
npm install
npm run build

# 4. Konfigurasi Laravel
php artisan key:generate

# 5. Database
php artisan migrate:fresh --seed

# 6. Livewire
composer require livewire/livewire

# 7. Broadcasting / Reverb
php artisan install:broadcasting

# 8. Quality Gate
php artisan test
./vendor/bin/pint --test
npm run build

# 9. Jalankan aplikasi
composer run dev
```

Buka aplikasi melalui:

```text
http://localhost:8000
```

### Health Check

```bash
php artisan --version
php artisan db:show
redis-cli -p 6379 ping
php artisan test
npm run build
./vendor/bin/pint --test
```

Jika seluruh pemeriksaan berhasil, environment development Laravel 13 telah memenuhi baseline Modul 1. Target output modul adalah repository Git yang bersih, Laravel 13 + Livewire 4 berjalan, MariaDB dan Redis sehat, `.env` tidak masuk Git, serta test, build, dan Pint berhasil.
