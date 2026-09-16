# EMIS Cuy releases

### v2.7.1

fix(updater): show download progress and restart reliably

### v2.7.2

fix(updater): show download progress and restart reliably

### v2.7.3

"fix(updater): handle empty restart arguments

### v2.7.4

"fix(updater): handle empty restart arguments

### v3.0.0

new: convert Web interface to native webview

### v3.0.1

fix favicon assets issue

### v3.1.0

feat: modernisasi aplikasi EMIS dan perluas fitur scraping

  - Migrasikan antarmuka web ke Wails React native webview
  - Terapkan redesign UI dengan branding EMIS Cuy, logo, favicon, dan
    versi aplikasi
  - Tambahkan konfirmasi sebelum mengunduh pembaruan aplikasi
  - Perbaiki integrasi reCAPTCHA
  - Tampilkan progress setiap worker berdasarkan kategori dan urutan
    terbaru
  - Pertahankan data mentah API untuk ekspor Excel dinamis
  - Dukung ekspor seluruh field data Madrasah dan Pontren
  - Perkuat retry, timeout, serta pengujian scraper dan API
