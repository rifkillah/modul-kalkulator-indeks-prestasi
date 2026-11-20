# Modul Kalkulator Indeks Prestasi (IP) Mahasiswa

Modul Laravel untuk menghitung **Indeks Prestasi (IP) semester** mahasiswa dari total mutu dan total SKS, lengkap dengan predikat, kuota SKS semester berikutnya, dan penanganan error.

## Fitur

- Menampilkan daftar mahasiswa beserta tombol hitung IP.
- Menghitung IP dengan rumus `total_mutu / total_sks`.
- Predikat otomatis: Dengan Pujian (≥ 3.50), Sangat Memuaskan (≥ 3.00), Memuaskan (≥ 2.50), Cukup (≥ 2.00), Perlu Perbaikan (< 2.00).
- Kuota SKS semester depan: 24 SKS bila IP ≥ 3.00, selain itu 20 SKS.
- Penanganan error: pembagian dengan nol (mahasiswa baru), nilai negatif, dan IP melebihi 4.00.
- Halaman error 404 dan 500 kustom serta watermark pemilik sistem.
- Unit test logika perhitungan (`tests/Unit/KalkulatorIpServiceTest.php`) dan feature test.

## Teknologi

- PHP 8.3+
- Laravel
- Blade Template
- MySQL

## Instalasi

1. Buat basis data MySQL:

```sql
CREATE DATABASE db_kalkulator_ip;
```

2. Jalankan:

```bash
composer install
cp .env.example .env
php artisan key:generate
php artisan migrate --seed
php artisan serve
```

Sesuaikan `DB_DATABASE`, `DB_USERNAME`, dan `DB_PASSWORD` pada `.env` bila perlu.
Buka http://localhost:8000 (otomatis diarahkan ke `/uji-nilai`).

## Data Demo (hasil seeder)

| NIM | Nama | Total Mutu | Total SKS |
|---|---|---|---|
| 2410114001 | Rizky Pratama | 76 | 20 |
| 2410114002 | Siti Nabila | 78 | 20 |
| 2410114003 | Budi Hartono (Mahasiswa Baru) | 0 | 0 |

NIM ketiga sengaja memiliki 0 SKS untuk menguji penanganan pembagian dengan nol.

## Route

| Method | URI | Keterangan |
|---|---|---|
| GET | `/` | Redirect ke `/uji-nilai` |
| GET | `/uji-nilai` | Daftar mahasiswa |
| GET | `/uji-nilai/hitung/{nim}` | Hasil perhitungan IP |

## Pengujian

```bash
php artisan test
```

> `phpunit.xml` memakai database `db_kalkulator_ip_test`; pastikan database tersebut tersedia.

## Struktur Penting

- `app/Services/KalkulatorIpService.php` — logika perhitungan, predikat, dan kuota SKS.
- `app/Http/Controllers/NilaiAkademikController.php` — alur halaman dan validasi data.
- `app/Models/Mahasiswa.php`, `Pengaturan.php` — data mahasiswa dan pengaturan aplikasi.
