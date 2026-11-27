# Dashboard Pemantauan Pengawasan Peperiksaan — Plan

## Top-Level Overview

**Goal:** Bina satu fail HTML tunggal (`dashboard-pengawasan.html`) yang berfungsi sebagai dashboard pemantauan pengawasan peperiksaan — dark mode, Bootstrap 5 via CDN, Vanilla JS, dengan data mock.

**Scope:**
- Fail HTML baharu, berasingan dari `dashboard-peperiksaan-polycc.html` yang sedia ada
- Tiada backend — semua data adalah mock (hardcoded JS arrays)
- 6 modul lengkap: Ringkasan Statistik, Pemantauan Dewan, Senarai Pengawas, Pengurusan Insiden, Jadual Peperiksaan, Laporan
- Dark mode sepenuhnya
- Bootstrap 5 CDN + Bootstrap Icons CDN + Chart.js CDN

**Approach:**
- Single-page application (SPA) dengan navigasi sidebar
- Setiap modul dipaparkan sebagai seksyen yang boleh ditogol via sidebar
- Countdown timer untuk setiap dewan aktif menggunakan `setInterval`
- Modal interaktif untuk log insiden baru
- Chart.js untuk carta statistik di halaman Laporan

---

## Sub-Tasks

### Sub-Task 1 — Struktur Asas HTML & Dark Mode Layout

**Intent:** Bina kerangka HTML dengan Bootstrap 5 dark mode, sidebar navigasi, topbar, dan kawasan kandungan utama. Ini adalah asas untuk semua modul seterusnya.

**Expected Outcomes:**
- Fail `dashboard-pengawasan.html` wujud
- Bootstrap 5 dark mode berfungsi (`data-bs-theme="dark"`)
- Sidebar kiri dengan 6 item navigasi dan Bootstrap Icons
- Topbar dengan nama halaman dinamik, badge sesi, dan butang CTA
- 6 div halaman kosong dengan ID yang betul (`#page-dashboard`, `#page-dewan`, `#page-pengawas`, `#page-insiden`, `#page-jadual`, `#page-laporan`)
- Fungsi navigasi `showPage()` berfungsi — klik sidebar tukar halaman aktif
- Responsif pada skrin 1024px ke atas

**Todo List:**
1. Cipta fail `dashboard-pengawasan.html` dengan boilerplate HTML5
2. Tambah CDN links: Bootstrap 5, Bootstrap Icons, Chart.js di `<head>`
3. Tambah CSS variables dark mode dan override Bootstrap di `<style>`
4. Bina sidebar dengan logo, 6 nav items (icons + label), footer sesi
5. Bina topbar dengan `#page-title` span, badge sesi, butang "+ Log Insiden"
6. Cipta 6 div halaman kosong `.page` dalam `.content`
7. Tulis fungsi JS `showPage(id)` untuk navigasi SPA

**Relevant Context:**
- Rujuk `dashboard-peperiksaan-polycc.html` untuk pola navigasi `showPage()` dan struktur sidebar
- Sidebar dan topbar menggunakan Bootstrap utilities (`d-flex`, `flex-column`, `sticky-top`)
- Dark mode Bootstrap: `<html data-bs-theme="dark">`

**Status:** [ ] pending

---

### Sub-Task 2 — Halaman Dashboard (Ringkasan Statistik)

**Intent:** Bina halaman utama dashboard dengan 4 stat cards, mini kalender peperiksaan bulan semasa, senarai peperiksaan akan datang, dan carta bar penggunaan dewan.

**Expected Outcomes:**
- 4 stat cards: Jumlah Dewan Aktif, Jumlah Calon Hari Ini, Pengawas Bertugas, Insiden Aktif
- Mini kalender Januari 2025 dengan highlight hari ada peperiksaan
- Senarai 5 peperiksaan akan datang dengan status badge
- Carta bar mendatar penggunaan kapasiti dewan (Bootstrap progress bars)

**Todo List:**
1. Tambah data mock `JADUAL`, `DEWAN`, `PENGAWAS`, `INSIDEN` di `<script>`
2. Render 4 stat cards menggunakan Bootstrap card + Bootstrap Icons
3. Render mini kalender (grid 7-kolum, highlight exam days)
4. Render senarai peperiksaan akan datang (list-group Bootstrap)
5. Render carta bar kapasiti dewan menggunakan Bootstrap progress component

**Relevant Context:**
- Data mock mestilah konsisten antara semua halaman (satu sumber kebenaran)
- Stat card warna: biru (dewan), hijau (calon), kuning (pengawas), merah (insiden)
- Kalender: Jan 2025 mula Rabu (offset = 3)

**Status:** [ ] pending

---

### Sub-Task 3 — Halaman Pemantauan Dewan Masa Nyata

**Intent:** Bina halaman pemantauan dewan yang menunjukkan status setiap dewan secara kad, dengan countdown timer untuk dewan aktif, bar kemajuan kehadiran calon, dan badge status berwarna.

**Expected Outcomes:**
- Grid kad dewan (sekurang-kurangnya 6 dewan)
- Setiap kad menunjukkan: nama dewan, nama subjek, masa peperiksaan, bilangan calon (hadir/jumlah), nama pengawas, status (Aktif/Selesai/Bersedia)
- Countdown timer hidup menggunakan `setInterval` untuk dewan berstatus "Aktif"
- Bootstrap progress bar untuk % kehadiran calon
- Badge status: hijau (Aktif), biru (Bersedia), abu-abu (Selesai)
- Butang "Lihat Butiran" pada setiap kad (buat papar modal ringkasan)

**Todo List:**
1. Tambah data mock `DEWAN` dengan medan: nama, subjek, masaMula, masaTamat, calonDaftar, calonHadir, pengawas, status
2. Render grid kad dewan menggunakan Bootstrap card
3. Implementasi countdown timer dengan `setInterval(updateTimers, 1000)`
4. Render progress bar kehadiran calon
5. Tambah modal "Butiran Dewan" yang papar maklumat lengkap

**Relevant Context:**
- Countdown dikira dari `masaTamat` mock (masa tetap, bukan masa sebenar) — gunakan offset dari masa semasa untuk simulasi
- Status "Aktif" = dewan sedang berlangsung peperiksaan
- Minimum 2 dewan "Aktif", 2 "Bersedia", 1 "Selesai" dalam data mock

**Status:** [ ] pending

---

### Sub-Task 4 — Halaman Senarai Pengawas

**Intent:** Bina halaman senarai pengawas dengan kad profil, bar beban tugasan, dan jadual ringkasan tugasan setiap pengawas.

**Expected Outcomes:**
- Grid kad pengawas (sekurang-kurangnya 9 pengawas)
- Setiap kad: nama, ID, jabatan, bilangan tugasan, progress bar beban kerja
- Badge status: Aktif/Tidak Aktif
- Jadual ringkasan di bawah grid: nama, jabatan, tugasan, % beban, status
- Filter bar: carian nama, filter jabatan (dropdown)

**Todo List:**
1. Render grid kad pengawas menggunakan data `PENGAWAS`
2. Tambah progress bar beban kerja (merah jika > 70%, kuning > 50%, hijau lain)
3. Render jadual ringkasan pengawas
4. Implementasi filter carian nama + filter jabatan

**Relevant Context:**
- Data `PENGAWAS` perlu konsisten dengan data `JADUAL` (ID pengawas dirujuk dalam jadual)
- Gunakan data yang sama dari Sub-Task 2

**Status:** [ ] pending

---

### Sub-Task 5 — Halaman Pengurusan Insiden

**Intent:** Bina halaman log insiden dengan jadual insiden masa nyata, butang log insiden baharu, modal borang log insiden, dan ringkasan statistik insiden mengikut kategori.

**Expected Outcomes:**
- 4 stat mini: Jumlah Insiden, Belum Selesai, Dalam Semakan, Selesai
- Jadual insiden: nombor, tarikh/masa, dewan, kategori, penerangan ringkas, dilaporkan oleh, status, tindakan
- Butang "+ Log Insiden Baharu" buka modal borang
- Modal borang: pilih dewan, kategori insiden (dropdown), penerangan, nama pelapor
- Selepas submit modal, tambah baris baharu ke jadual (JS push + re-render)
- Badge warna mengikut kategori: merah (Kecurangan), jingga (Kesihatan), kuning (Teknikal), biru (Kehadiran), abu (Lain)

**Todo List:**
1. Tambah data mock `INSIDEN` (sekurang-kurangnya 8 rekod)
2. Render stat mini insiden
3. Render jadual insiden
4. Bina modal borang log insiden dengan Bootstrap modal
5. Implementasi fungsi `logInsiden()` — push data baru ke array `INSIDEN` dan re-render jadual
6. Tambah butang "Selesai" pada setiap baris untuk tukar status insiden

**Relevant Context:**
- Ini adalah modul paling interaktif — pastikan modal Bootstrap berfungsi dengan betul
- Status insiden: "Baru", "Dalam Semakan", "Selesai"
- Kategori: Kecurangan, Masalah Kesihatan, Teknikal, Calon Lewat/Tidak Hadir, Lain-lain

**Status:** [ ] pending

---

### Sub-Task 6 — Halaman Jadual Peperiksaan

**Intent:** Bina halaman jadual peperiksaan dengan jadual lengkap, carian & penapis, dan modal tambah peperiksaan baru.

**Expected Outcomes:**
- Jadual: Bil, Kod Subjek, Nama Subjek, Tarikh, Hari, Masa, Dewan, Pengawas Ketua, Pengawas 2, Pelajar, Status, Tindakan
- Filter bar: carian teks, filter hari, filter sesi (pagi/petang), filter status
- Butang "+ Tambah Peperiksaan" buka modal borang
- Modal borang: semua medan jadual, dropdown bilik dan pengawas dari data mock
- Selepas submit, tambah baris baharu ke jadual

**Todo List:**
1. Render jadual menggunakan data `JADUAL`
2. Implementasi filter: carian, hari, sesi, status
3. Bina modal borang tambah peperiksaan
4. Implementasi fungsi `tambahJadual()` — push dan re-render

**Relevant Context:**
- Gunakan data `JADUAL` yang sama dari Sub-Task 2
- Status badge: "Sahih" (hijau), "Konflik" (merah), "Draf" (abu-abu)
- Kod subjek papar dalam `<code>` tag dengan background sedikit

**Status:** [ ] pending

---

### Sub-Task 7 — Halaman Laporan & Carta

**Intent:** Bina halaman laporan dengan statistik ringkasan sesi peperiksaan, carta Chart.js (donat dan bar), dan jadual beban pengawas untuk cetakan.

**Expected Outcomes:**
- 4 stat cards laporan: Subjek Diperiksa, Jumlah Pelajar, Sesi Pagi, Sesi Petang
- Carta donat Chart.js: agihan status peperiksaan (Sahih/Konflik/Draf)
- Carta bar Chart.js: bilangan peperiksaan mengikut jabatan/program
- Jadual beban pengawas dengan progress bar
- Butang "Cetak Laporan" (panggil `window.print()`)

**Todo List:**
1. Render 4 stat cards laporan
2. Render carta donat Chart.js — kira agihan status dari `JADUAL`
3. Render carta bar Chart.js — kira bilangan peperiksaan mengikut hari
4. Render jadual beban pengawas
5. Implementasi butang cetak dengan `window.print()`

**Relevant Context:**
- Chart.js dark mode: gunakan `color: '#ccc'` untuk label dan `borderColor: '#333'` untuk grid
- Carta menggunakan data yang dikira secara dinamik dari array `JADUAL` dan `PENGAWAS`
- Canvas Chart.js perlu ada `height` yang ditetapkan untuk elak carta terlalu besar

**Status:** [ ] pending

---

## Data Mock Summary

Semua data mock berikut perlu ada dalam satu blok `<script>` yang dikongsi:

| Pembolehubah | Medan Utama |
|---|---|
| `DEWAN` | id, nama, subjek, masaMula, masaTamat, calonDaftar, calonHadir, pengawasKetua, pengawas2, status |
| `PENGAWAS` | id, nama, jabatan, tugasan, status |
| `JADUAL` | kod, subjek, tarikh, hari, masa, dewan, pk, p2, pelajar, status |
| `INSIDEN` | id, masa, dewan, kategori, penerangan, pelapor, status |

---

## Fail Output

- **Fail:** `dashboard-pengawasan.html`
- **Teknologi:** HTML5, Bootstrap 5.3 (CDN), Bootstrap Icons 1.11 (CDN), Chart.js 4 (CDN), Vanilla JS
- **Tema:** Dark mode sepenuhnya (`data-bs-theme="dark"`)
- **Navigasi:** SPA sidebar — 6 halaman dalam satu fail

---

## Status Keseluruhan

- [x] Sub-Task 1 — Struktur Asas HTML & Dark Mode Layout
- [x] Sub-Task 2 — Halaman Dashboard (Ringkasan Statistik)
- [x] Sub-Task 3 — Halaman Pemantauan Dewan Masa Nyata
- [x] Sub-Task 4 — Halaman Senarai Pengawas
- [x] Sub-Task 5 — Halaman Pengurusan Insiden
- [x] Sub-Task 6 — Halaman Jadual Peperiksaan
- [x] Sub-Task 7 — Halaman Laporan & Carta

## Nota Implementasi

- Semua 7 sub-task diselesaikan dalam satu fail: `dashboard-pengawasan.html`
- Data mock dikongsi dalam satu blok `<script>` — arrays `DEWAN`, `PENGAWAS`, `JADUAL`, `INSIDEN`
- Countdown timer menggunakan `setInterval` dengan nilai simulasi (D1: 65 minit, D2: 43 minit)
- Chart.js instances disimpan dalam `chartInstances{}` untuk destroy sebelum re-render
