# Laporan Audit Backend dan Geo Location - Week 1

**Oleh:** Nafhan Zahrah Apriawan

---

## Audit Route & Controller (API)
Telah dilakukan pemetaan terhadap file `routes/api.php` dan Controller terkait

### 1. TrendsController (`app/Http/Controllers/API/TrendsController.php`)
- **Deskripsi:** Mengolah data tren mention, reach, dan analisis sentimen publik.
- **Model yang Digunakan:** `ScraperNews`, `ScraperSocmed`, `ScraperGoogleTrendsRelated`, `Keyword`, `Campaign`.
- **Method & API Endpoint:**

| Method | Endpoint | Deskripsi |
| :--- | :--- | :--- |
| `mention_reach_graph()` | `POST /trends-graph-mention` | Menghasilkan data grafik tren mention dan reach berdasarkan periode (day, week, month). |
| `mention_circle()` | `POST /trends-circle-mention` | Menghitung persentase mention dari berita dan media sosial dalam bentuk diagram lingkaran. |
| `reach_circle()` | `POST /trends-circle-reach` | Menghitung persentase view dari platform media sosial (TikTok, YouTube, Instagram, Facebook, Twitter). |
| `stats_mention()` | `POST /trends-stats-mention` | Total statistik jumlah mention, reach, likes, interaction, user generated, dan video views. |
| `profile_stats()` | `POST /trends-stats-profile` | Mengambil 5 akun author teratas berdasarkan total view. |
| `sources_stats()` | `POST /trends-stats-sources` | Perbandingan persentase jumlah mention berdasarkan platform sumber data. |
| `sources_posts()` | `POST /trends-sources-posts` | Menampilkan daftar postingan berita/sosmed terbaru atau berdasarkan filter. |
| `sentiment_stats()` | `POST /sentiment-stats` | Menghitung total akumulasi sentimen (positive, neutral, negative) dari berita dan media sosial. |
| `sentiment_category_stats()` | `POST /sentiment-category-stats` | Statistik sentimen berdasarkan kategori platform (FB, IG, TikTok, YouTube, Twitter, Website). |
| `related_queries()` | `POST /trends-related-queries` | Mengambil 10 pencarian teratas dari Google Trends. |
| `summary_stats()` | `POST /trends-stats-summary` | Menghitung performa dibanding dengan periode sebelumnya. |
| `summary_mentions()` | `POST /trends-summary-mentions` | Menampilkan 10 postingan terbaru atau terpopuler dari berita dan media sosial. |

---

### 2. SourceController (`app/Http/Controllers/API/SourceController.php`)
- **Deskripsi:** Mengambil daftar seluruh platform (News, Instagram, TikTok, YouTube, Twitter, Facebook), mengagregasi total mention serta total views/visits, lalu menghitung skor pengaruh (*influence score*) untuk setiap sumber.
- **Model yang Digunakan:** `ScraperNews`, `ScraperSocmed`, `Keyword`.
- **Method & API Endpoint:**

| Method | Endpoint | Deskripsi |
| :--- | :--- | :--- |
| `sources()` | `POST /sources-list` | Memuat daftar platform sumber data, menghitung kalkulasi Influence Score, dan mengembalikan data berformat JSON.[cite: 1, 7, 10] |

---

### 3. ReportController (`app/Http/Controllers/API/ReportController.php`)
- **Deskripsi:** Mengumpulkan seluruh ringkasan data analisis dan mengekspornya ke dalam format Excel (.xlsx), PDF, atau dikirimkan via Email.
- **Model yang Digunakan:** `ScraperNews`, `ScraperSocmed`, `ScraperGoogleTrendsRelated`.
- **Method & API Endpoint:**

| Method | Endpoint | Deskripsi |
| :--- | :--- | :--- |
| `excel()` | `POST /report-excel` | Mengumpulkan data via `populateData()` lalu mengunduh file spreadsheet Excel menggunakan `DataExport`. |
| `pdf()` | `POST /report-pdf` | Mengolah data diagram/chart SVG, me-render tampilan blade `pdf.report-tailwind`, dan mengonversinya menjadi dokumen PDF via `Browsershot`. |
| `email()` | `POST /report-email` | Memvalidasi input email, merender PDF menggunakan `Mpdf`, menyimpannya sementara di storage internal, dan mengirimkannya ke alamat email penerima. |

---

## Audit Database Shared (`database/migrations`)
- **Tabel Scraper Utama:**
  - `scraper_news`: Menyimpan data artikel berita (`title`, `content`, `author`, `source`, `sentiment`, `publish_date`).
  - `scraper_socmeds`: Menyimpan data postingan sosial media (`title`, `content`, `author`, `author_follower`, `source`, `sentiment`, `total_view`, `total_like`, `total_share`).
- **Tabel Google Trends:**
  - `scraper_google_trends` & `scraper_google_trends_related`: Menyimpan metrik tren dan memuat kolom string `region`.

---

## Identifikasi Modul Geo Location
- **Status Saat Ini:** *Not Implemented* (Belum ada).
- **Hasil Temuan:**
  1. Pada layer Controller (`TrendsController`, `SourceController`, `ReportController`), tidak terdapat query atau pemrosesan variabel spasial (`latitude`, `longitude`, `city`, `country`).
  2. Pada layer Database (`scraper_news` dan `scraper_socmeds`), belum tersedia kolom penyimpan data koordinat atau lokasi geografis tempat artikel/postingan dibuat.