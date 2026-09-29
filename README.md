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

### v3.2.0

feat: add manual retry and failed-data JSON recap

  - add per-item retry for failed Madrasah and Pontren requests
  - support JSON recap export, validated import, and retry preview
  - export recovered retry data to separate Excel files
  - prevent retry Excel filename collisions
  - show accurate remaining failed-item counts
  - limit imported JSON recap size for safer file handling

### v3.3.0

feat(emis): add Sarpras data fetching for Madrasah institutions

Fetch sarana & prasarana (asset) summary from EMIS API endpoint
`/v1/assets/summary-asset-kemenag/{id}` and merge the results directly
into the Lembaga sheet as `sarpras_`-prefixed columns.

Backend (Go):
- scraper/sarpras_api.go: new FetchSarpras() — calls the asset summary
  endpoint and returns an ExportRecord with all response fields prefixed
  with "sarpras_"; arrays (electricity, internet, room, asset) are kept
  as native []any so the dynamic Excel exporter serialises them as JSON
  strings, consistent with all other array fields in the system
- scraper/scraper.go: add Sarpras bool to DataTypesConfig; integrate
  FetchSarpras() into the institution worker pool — called sequentially
  after FetchInstitutionBiodata(), result merged into biodata.Raw
- scraper/models.go: add RetryMadrasahSarpras operation constant, add
  IncludeSarpras field to RetryTask, update Validate() and Key()
- scraper/retry.go: handle RetryMadrasahSarpras in retryOneTask() and
  retryTaskDisplay(); failed sarpras requests are retryable
- handlers/scrape.go: add Sarpras field to DataTypesRequest, validate
  that Sarpras requires Lembaga to be selected, block Sarpras for
  Pontren accounts at the handler layer

Frontend (TypeScript/React):
- api.ts: add sarpras: boolean to DataTypes type
- App.tsx: add sarpras: false to initialDataTypes; show "↳ Sarpras"
  sub-checkbox only for Madrasah accounts and only when Lembaga is
  checked; auto-clear sarpras when Lembaga is deselected; fix allSelected
  to only reflect the 4 primary data types (lembaga, siswa, guru, tendik)
  so the "Semua" checkbox works correctly; fix runScrape validation to
  exclude sarpras from the "at least one type selected" check
- styles.css: add .sarpras-sub-check styles — indented with a subtle
  green left border to visually indicate it is a sub-option of Lembaga

This feature is exclusive to EMIS Madrasah accounts (kabupaten/kota
and central). Pontren accounts are blocked at both handler and scraper
layers.
