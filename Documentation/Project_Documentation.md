# Project Documentation
## Automated Data Distribution System

---

## 1. Project Overview

### Background

Proses distribusi data campaign WhatsApp Blast sebelumnya dilakukan secara manual dengan cara membuka banyak spreadsheet, menyalin data pelanggan, kemudian menempelkannya ke spreadsheet target sesuai area distribusi.

Proses ini membutuhkan waktu yang cukup lama dan berisiko menyebabkan:
- Human error
- Duplikasi data
- Salah distribusi target
- Sulit melakukan monitoring

Untuk mengatasi masalah tersebut, dibuat sistem otomatis menggunakan Google Apps Script yang dapat mendistribusikan data secara otomatis berdasarkan konfigurasi yang telah ditentukan.

---

## 2. Business Problem

Beberapa kendala yang ditemukan pada proses manual:

1. Distribusi data dilakukan secara berulang setiap hari.
2. Membutuhkan pembukaan banyak spreadsheet.
3. Sulit melakukan tracking histori distribusi.
4. Tidak ada validasi hari libur dan hari Minggu.
5. Risiko salah penempatan data ke target campaign.

---

## 3. Project Objective

Tujuan proyek ini adalah:

- Mengotomatisasi distribusi data campaign.
- Mengurangi pekerjaan manual.
- Mengurangi human error.
- Menyediakan log distribusi otomatis.
- Memastikan distribusi berjalan sesuai jadwal.

---

## 4. System Architecture

Workflow sistem:

Source Data
↓
ID_Sumber
↓
PlanDistribusi
↓
Google Apps Script
↓
Target Spreadsheet
↓
LogDistribusi

---

## 5. Main Components

### Daily Plan Distribusi WL

Berfungsi sebagai dashboard utama untuk mengatur:

- Materi campaign
- Area distribusi
- Spreadsheet sumber
- Spreadsheet target
- Jumlah data yang didistribusikan

---

### ID_Sumber

Digunakan untuk:

- Menyimpan URL spreadsheet sumber
- Mengekstrak Spreadsheet ID secara otomatis
- Menjadi referensi Apps Script

---

### ID_Target

Digunakan untuk:

- Menyimpan URL spreadsheet tujuan
- Mengekstrak Spreadsheet ID
- Menentukan lokasi distribusi

---

### Source Database

Berisi database pelanggan yang akan didistribusikan.

Contoh cluster:

- AMB_WA_1
- SBB_WA_1
- TUL_WA_1
- AMB_WA_2
- SBB_WA_2
- TUL_WA_2

---

### Log Distribusi

Mencatat seluruh aktivitas sistem:

- Waktu eksekusi
- Campaign
- Area
- Jumlah data
- Status distribusi
- Pesan error

---

## 6. Key Features

### Automatic Data Distribution

Sistem secara otomatis:

- Mengambil data dari spreadsheet sumber
- Menyalurkan data ke spreadsheet target
- Menyimpan posisi terakhir data yang digunakan

---

### Auto Reset Position

Ketika seluruh data telah digunakan:

- Posisi akan kembali ke awal
- Counter reset akan bertambah otomatis

---

### Holiday Validation

Distribusi dibatalkan otomatis ketika:

- Hari Minggu
- Hari libur nasional Indonesia

---

### Logging System

Seluruh aktivitas dicatat otomatis untuk kebutuhan monitoring dan audit.

---

## 7. Technologies Used

- Google Sheets
- Google Apps Script
- JavaScript
- Spreadsheet Automation

---

## 8. Results

Manfaat yang diperoleh:

- Mengurangi proses manual distribusi data
- Menghemat waktu operasional harian
- Meminimalkan human error
- Meningkatkan akurasi distribusi campaign
- Mempermudah monitoring distribusi

---

## 9. Screenshots

### Daily Plan Distribution
(Screenshot Daily Plan Distribusi WL)

### Source ID Management
(Screenshot ID_Sumber)

### Target ID Management
(Screenshot ID_Target)

### Source Database
(Screenshot WL Sumber)

### Distribution Log
(Screenshot LogDistribusi)

---

## 10. Future Improvements

Beberapa pengembangan yang dapat dilakukan:

- Email notification setelah distribusi selesai
- Dashboard monitoring otomatis
- Duplicate checking
- Campaign performance reporting
- Multi-channel distribution

---

## 11. Author

Reza Ikhy

Role:
- Admin Support
- Full Stack Developer Certified
- Automation & Data Processing Enthusiast

Project Year:
2026