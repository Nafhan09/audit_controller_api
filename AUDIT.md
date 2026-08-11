# Laporan Audit Backend & Geo Location - Week 1
**Oleh:** Nafhan Zahrah Apriawan  
**Tanggal:** 12 Agustus 2026  

---

## 1. Audit Route & Controller (API)
Telah dilakukan pemetaan terhadap file `routes/api.php` dan Controller terkait

### A. TrendsController (`app/Http/Controllers/API/TrendsController.php`)
- **Fungsi:** Mengolah data tren mention, reach, dan analisis sentimen publik.
- **Method dan API endpoint**
| Method | Endpoint | deskripsi |
| --- | --- | --- |
| mention_reach_graph() | post('trends-graph-mention' | Menghasilkan data grafik tren mention dan reach berdasarkan periode (day, week, month) |

### B. SourceController (`app/Http/Controllers/API/SourceController.php`)
- **Fungsi:** Mengambil daftar platform sumber data (News, TikTok, Instagram, YouTube, Facebook, Twitter) serta kalkulasi skor pengaruh (*influence score*).
- **Endpoint Utama:** `POST /sources-list`

### C. ReportController (`app/Http/Controllers/API/ReportController.php`)
- **Fungsi:** Mengenerate dan mengekspor laporan analisis ke dalam bentuk file PDF, Excel, atau pengiriman Email.
- **Endpoint Utama:** `POST /report-excel`, `POST /report-pdf`, `POST /report-email`

---

## 2. Audit Database Shared
Pemeriksaan pada struktur tabel database `scraper_news` dan `scraper_socmeds`:
- **Kolom Utama saat ini:** `title`, `content`, `author`, `source`, `sentiment`, `publish_date`, `total_view`, `total_like`, `total_share`.
- **Penggunaan Query:** Controller menggunakan query pencarian teks (`MATCH AGAINST`) dan filter tanggal (`publish_date`).

---

## 3. Status & Temuan Modul Geo Location
- **Status:** *Not Implemented* (Belum ada).
- **Temuan:**
  1. Pada file `routes/api.php` maupun Controller (`TrendsController`, `SourceController`, `ReportController`) belum terdapat *endpoint* atau logika kueri spasial/geospasial.
  2. Pada tabel `scraper_news` dan `scraper_socmeds` belum tersedia kolom penyimpan data lokasi (seperti `location`, `city`, `country`, `latitude`, atau `longitude`).