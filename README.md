# Aplikasi Peta Sebaran Objek Pajak — Google Sheets + GitHub Pages

Aplikasi web responsif untuk menampilkan peta, mencari/filter data, menambah, mengubah, menghapus data, dan mengekspor CSV/JSON. Frontend di-host melalui GitHub Pages; data pusat disimpan pada Google Sheets melalui Google Apps Script.

## Berkas
- `index.html` — frontend GitHub Pages.
- `Code.gs` — API Google Apps Script.
- `PENDATAAN_GABUNGAN.xlsx` — data awal gabungan dari seluruh sheet sumber menjadi satu sheet.

## A. Siapkan Google Sheets
1. Unggah `PENDATAAN_GABUNGAN.xlsx` ke Google Drive.
2. Buka file tersebut dengan Google Sheets.
3. Pastikan sheet bernama persis `DATA GABUNGAN`, baris pertama berisi header, dan kolom `TITIK KOORDINAT` menggunakan format `-8.6800,115.4400`.
4. Salin ID spreadsheet dari URL: `https://docs.google.com/spreadsheets/d/ID_SPREADSHEET/edit`.

## B. Pasang API Apps Script
1. Dari spreadsheet Google Sheets, pilih **Ekstensi → Apps Script**.
2. Ganti isi `Code.gs` dengan isi file `Code.gs` dalam paket ini.
3. Di bagian `CONFIG`, ganti `SPREADSHEET_ID` dengan ID spreadsheet Anda.
4. Ganti `API_KEY` di `Code.gs` dengan nilai acak yang Anda tentukan. Salin nilai yang sama ke `API_KEY` di `index.html`.
5. Simpan, lalu jalankan fungsi `doGet` tidak diperlukan secara manual; saat pertama kali menjalankan fungsi yang memakai spreadsheet, setujui permintaan izin Google jika diminta.
6. Klik **Deploy → New deployment → Web app**.
7. **Execute as:** Me. **Who has access:** Anyone. Klik Deploy, lalu berikan izin yang diminta.
8. Salin URL Web App berakhiran `/exec`.

## C. Hubungkan frontend
1. Buka `index.html`.
2. Ganti `TEMPEL_URL_WEB_APP_APPS_SCRIPT_DI_SINI` pada baris `API_URL` dengan URL `/exec` dari deployment Apps Script.
3. Pastikan `API_KEY` sama persis dengan yang ada di `Code.gs`.
4. Simpan.

## D. Unggah ke GitHub Pages
1. Buat repository GitHub baru, misalnya `peta-sebaran-pajak`.
2. Unggah `index.html` ke root repository. `Code.gs` dan Excel boleh disimpan sebagai dokumentasi/backup, tetapi jangan taruh data sensitif di repository publik.
3. Buka **Settings → Pages**.
4. Pada Build and deployment pilih **Deploy from a branch**, branch `main`, folder `/ (root)`, lalu Save.
5. Tunggu alamat GitHub Pages tersedia. Buka alamat itu dan uji tambah data. Periksa apakah baris baru masuk ke Google Sheets.

## Catatan keamanan penting
- GitHub Pages bersifat publik jika repository publik. Jangan masukkan data pribadi/sensitif ke dalam `index.html` atau repository.
- Web App yang diset `Anyone` dapat diakses publik. API key yang ditanam di frontend juga dapat dilihat oleh pengunjung; ia bukan autentikasi kuat. Untuk data pajak operasional/internal, gunakan autentikasi yang sesuai (misalnya login Google/akses domain organisasi) sebelum produksi, dan jangan membuka hak ubah/hapus kepada publik.
- Jika data gagal dibaca, pastikan deployment URL benar, izin akses Web App benar, nama sheet persis `DATA GABUNGAN`, dan ID spreadsheet benar.
- Jika Anda mengubah kode Apps Script setelah deployment, buka **Deploy → Manage deployments → Edit → New version → Deploy**.
- Peta menggunakan Leaflet dan tile OpenStreetMap yang memerlukan koneksi internet.
