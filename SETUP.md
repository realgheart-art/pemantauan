# Panduan Setup — Sistem Pemantauan Sekolah JPN Kedah

## 1. Sediakan Google Sheet (Database)
1. Cipta Google Sheet baharu, namakan cth **"DB Pemantauan Sekolah JPN Kedah"**.
2. Buka **Extensions → Apps Script**.
3. Padam kandungan `Code.gs` default, salin-tampal kandungan fail `Code.gs` yang disediakan.
4. Dalam editor Apps Script, jalankan fungsi **`setupSheets`** sekali (pilih fungsi di dropdown atas, klik Run).
   - Ini akan auto-cipta 6 sheet: Sekolah, Pengguna, Kategori, ItemSemakan, Pemantauan, Dapatan.
   - Kategori & item permulaan (Kelas, Kantin, Tandas, dll — 11 kategori) akan di-seed sekali.
5. Beri kebenaran (authorize) apabila diminta.

## 2. Isikan Sheet "Sekolah"
Salin senarai 753 sekolah dari **Dashboard MAP Kedah** (kod, nama, PPD, gred, lat, lng) ke sheet **Sekolah** — susunan lajur mesti sepadan: `KodSekolah | NamaSekolah | PPD | Gred | Lat | Lng`.

## 3. Isikan Sheet "Pengguna"
Tambah baris untuk setiap pegawai yang akan guna sistem:
| ID | Nama | Emel | PIN | Peranan | PPD | Aktif |
|----|------|------|-----|---------|-----|-------|
| 1 | Cikgu Shahril | shahril@moe-dl.edu.my | 1234 | Admin | JPN | TRUE |
| 2 | Pegawai A | a@moe-dl.edu.my | 5678 | Pegawai | PPD Kota Setar | TRUE |

> PIN sementara guna nombor ringkas — boleh naik taraf ke sistem login lebih selamat kemudian.

## 4. Deploy sebagai Web App
1. Dalam Apps Script: **Deploy → New deployment**.
2. Jenis: **Web app**.
3. Execute as: **Me**. Who has access: **Anyone** (atau "Anyone within organization" jika mahu had kepada e-mel MOE sahaja).
4. Klik **Deploy**, salin **URL Web App** yang dihasilkan (`https://script.google.com/macros/s/XXXX/exec`).

## 5. Sambungkan Frontend
1. Buka fail `index.html`, cari baris:
   ```js
   const API_URL = 'https://script.google.com/macros/s/GANTI_DENGAN_ID_DEPLOYMENT/exec';
   ```
2. Gantikan dengan URL Web App dari Langkah 4.
3. Upload `index.html` ke repo GitHub Pages anda (ikut cara sedia ada untuk sistem-sistem lain).

## Nota Teknikal
- **Gambar bukti** disimpan dalam folder Google Drive bernama *"Pemantauan Sekolah - Gambar Bukti"*, dikongsi sebagai "anyone with link — view".
- **Skala markah**: 1 (Sangat Lemah) hingga 5 (Cemerlang). Catatan digalakkan wajib bila skor ≤2 (boleh ditambah validasi JS jika perlu diperketatkan).
- **Kategori & item semakan** boleh diurus terus dari panel Admin (tambah kategori/item baharu) — tiada perlu edit kod.
- **Laporan PDF per lawatan** dan **fungsi eksport** belum dibina dalam versi ini — boleh ditambah pada fasa seterusnya (guna Google Docs API atau library docx di Apps Script).
- Semua rekod "Senarai Pemantauan" memaparkan nama pegawai & tarikh/masa pemantauan seperti diminta.

## Cadangan Fasa Seterusnya
- [ ] Laporan PDF automatik per sesi lawatan (format SULIT ikut gaya dokumen rasmi JPN)
- [ ] Kunci kira-kira: wajibkan catatan bila skor ≤2 (validasi sebelum hantar)
- [ ] Peta Leaflet untuk papar taburan sekolah yang telah/belum dipantau (guna koordinat dari Dashboard MAP Kedah)
- [ ] Eksport Excel senarai pemantauan (guna SheetJS)
- [ ] Notifikasi automatik ke Admin bila skor kategori kritikal (≤2) direkodkan
