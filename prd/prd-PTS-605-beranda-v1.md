# PRD — Beranda (CRM Bidan)

**Epic:** PTS-605 — `[CRM][Bidan] Beranda (Dashboard)` · **Task PRD:** PTS-617
**Versi:** v1 · **Tanggal:** 17 Sep 2026 · **Penyusun:** Jo (AI Assistant) untuk Dhani Prisantika
**Prototype acuan:** `docs/prototype/CRMBidan_17sep2026.html` (desain final)
**Hasil grill:** 8 pertanyaan dijawab pemilik produk (7 opsi A/B + 1 gugur karena desain) — ringkasan di §5.

> **Fokus dokumen ini:** satu layar **Beranda** yang menjawab *"apa yang harus saya kerjakan hari ini?"* — ringkasan angka praktik, jalan pintas ke tugas utama, peringatan pasien yang lewat jadwal, daftar kunjungan mendatang, dan pasien yang mendekati persalinan.
> **Catatan penting:** spec "varian 2" pada deskripsi epic (menyebut **sapaan waktu** & pertanyaan pemandu) **tidak dipakai lagi** — desain final di prototype 17 Sep tidak memuatnya. Dokumen ini mengikuti **desain terbaru**.

---

## 1. Overview & Scope Boundary

**Yang dibangun:** halaman **Beranda** CRM Bidan sebagai layar pertama setelah bidan masuk — memuat blok statistik, aksi cepat, banner pasien telat, blok Pengingat Kunjungan, dan blok Mendekati Persalinan, dengan modal ringkasan pasien sebagai jalan cepat ke profil.

**Untuk siapa:** bidan pengelola praktik mandiri (PMB) yang memakai modul CRM Bidan dan mengelola puluhan pasien hamil/nifas/anak sekaligus.

**Masalah:**
1. **Tidak ada satu layar yang menjawab tugas hari ini** — jadwal tersebar di halaman Jadwal Kunjungan dan Kantong Persalinan; bidan harus membuka dua tempat berbeda untuk tahu siapa yang harus ditindaklanjuti.
2. **Pasien yang lewat jadwal kontrol tidak menonjol** — risiko pasien hilang dari pemantauan (ANC, KB, nifas) karena tidak ada peringatan di layar pertama.
3. **Pasien mendekati persalinan (HPL ≤ 3 minggu) susah dipantau cepat** — sekarang harus membuka Kantong Persalinan dan menelusuri per pasien.
4. **Tidak ada gambaran beban kerja bulan berjalan** — jumlah kunjungan dan jumlah ibu hamil aterm hanya bisa dilihat dengan menghitung manual.
5. **Prototype masih membawa alat uji UX** (pemilih varian desain 1/2/3 dan toggle "Preview kosong") yang membingungkan saat dipakai untuk demo.

**Di dalam cakupan:**
- **Header statistik** — judul **"Ringkasan hari ini"** + 2 kartu: **Kunjungan bulan ini** dan **Kehamilan > 38 minggu**. **Tanpa** sapaan waktu/tanggal.
- **Aksi Cepat (4 tombol)** — Periksa pasien baru · Daftar Pasien · Kantong Persalinan · Pelaporan.
- **Banner pasien telat** — *"N pasien lewat jadwal kunjungan"* → modal **"Lewat Jadwal Kunjungan"**.
- **Blok Pengingat Kunjungan** — maksimal 6 pasien, badge `Hari ini` / `N hari lagi`, tautan "Lihat semua" ke halaman Jadwal Kunjungan.
- **Blok Mendekati Persalinan** — maksimal 6 pasien dengan **HPL ≤ 21 hari**, tautan "Lihat semua" ke Kantong Persalinan.
- **Modal ringkasan pasien** — dipicu dari Beranda, memuat jalan ke **"Lihat profil pasien"**.
- **Empty state** untuk kedua blok, dan **menghapus alat uji UX** dari prototype.

**Di luar cakupan:**
- **Notifikasi push / pengingat otomatis** — ditunda untuk CRM Bidan berbasis web; penggantinya **badge pada menu** dan daftar harian di Beranda (keputusan lama, tetap berlaku).
- **Kartu ke-3 (pemasukan/transaksi kasir bulan ini)** — angka keuangan tetap di halaman **Keuangan**.
- **Grafik tren, analitik, dan laporan periodik** — bukan Beranda.
- **Filter/segmentasi lanjutan dan pengaturan urutan blok oleh pengguna** (drag-and-drop) — di luar rilis ini.
- **Pengiriman pesan WhatsApp langsung dari Beranda** — pengiriman tetap dari halaman Jadwal Kunjungan / Kantong Persalinan (naskah: PTS-386).
- **Perubahan isi** halaman Jadwal Kunjungan, Kantong Persalinan, Daftar Pasien, dan Laporan Puskesmas — sudah diatur di epic masing-masing.

---

## 2. User Stories

| ID | User Story | Prioritas |
|---|---|---|
| US-01 | Sebagai bidan, saya ingin **melihat ringkasan angka penting praktik** (kunjungan bulan ini & ibu hamil di atas 38 minggu) supaya saya tahu beban kerja saya sekilas tanpa menghitung manual. | Must Have |
| US-02 | Sebagai bidan, saya ingin **jalan pintas ke empat tugas utama** (periksa pasien baru, daftar pasien, kantong persalinan, pelaporan) langsung dari Beranda, supaya saya tidak perlu menelusuri menu. | Must Have |
| US-03 | Sebagai bidan, saya ingin **diberi tahu saat ada pasien yang lewat jadwal kunjungan**, supaya saya bisa segera menindaklanjuti pasien yang berisiko hilang dari pemantauan. | Must Have |
| US-04 | Sebagai bidan, saya ingin **melihat daftar kunjungan mendatang dan pasien yang mendekati persalinan langsung di Beranda**, supaya tidak ada pasien yang terlewat. | Must Have |
| US-05 | Sebagai bidan, saya ingin **melihat ringkasan singkat pasien saat namanya diklik**, supaya saya bisa memutuskan tindak lanjut tanpa berpindah halaman. | Should Have |

## 3. Functional Requirements

| ID | Requirement |
|---|---|
| FR-01 | **Header Beranda** memuat judul **"Ringkasan hari ini"** dan **2 kartu statistik**. Tidak ada sapaan waktu ("Selamat pagi/siang/malam"), tidak ada pertanyaan pemandu, dan tidak ada tanggal di header. |
| FR-02 | Kartu **"Kunjungan bulan ini"** menampilkan jumlah **catatan layanan** pasien pada **bulan berjalan** dari **semua** jenis layanan: ANC, Nifas, KB, Wellness, **Persalinan**, dan **Imunisasi**. Dihitung **per catatan layanan** (bukan per pasien), sehingga satu pasien dengan 2 kunjungan dihitung 2. |
| FR-03 | Kartu **"Kehamilan > 38 minggu"** menampilkan jumlah pasien berstatus **hamil** dengan **usia kehamilan di atas 38 minggu**. |
| FR-04 | Blok **"Aksi Cepat"** memuat 4 tombol berurutan: **Periksa pasien baru** → Pasien Baru · **Daftar Pasien** → Daftar Pasien · **Kantong Persalinan** → Kantong Persalinan · **Pelaporan** → **Laporan Puskesmas**. Label "Periksa pasien lama" tidak dipakai. |
| FR-05 | Aksi cepat ditampilkan sebagai **grid 2 kolom** (ikon + label), dengan target sentuh minimal **44×44 px** per tombol. |
| FR-06 | **Banner peringatan** menampilkan teks **"N pasien lewat jadwal kunjungan"** dan **hanya tampil bila** ada minimal 1 pasien dengan jadwal berikutnya **sudah lewat**. Bila tidak ada pasien telat, banner tidak ditampilkan sama sekali (bukan banner kosong). |
| FR-07 | **Klik banner** membuka modal **"Lewat Jadwal Kunjungan"** berisi daftar pasien telat; setiap baris memuat **nama pasien**, **tanggal jadwalnya**, dan badge **`Lewat N hari`**. Klik satu baris membuka **modal ringkasan pasien** tanpa meninggalkan Beranda. |
| FR-08 | Blok **"Pengingat Kunjungan"** menampilkan maksimal **6 pasien** yang jadwal berikutnya **hari ini atau akan datang**, diurutkan dari yang **paling mendesak**; tiap baris memuat badge **`Hari ini`** atau **`N hari lagi`**. |
| FR-09 | Tautan **"Lihat semua"** pada blok Pengingat Kunjungan menuju halaman **Jadwal Kunjungan**. |
| FR-10 | Blok **"Mendekati Persalinan"** menampilkan maksimal **6 pasien hamil** dengan **HPL ≤ 21 hari** (termasuk HPL yang sudah lewat), diurutkan dari HPL terdekat; tiap baris memuat informasi HPL dan penanda status (`HPL N hari lagi` / `HPL lewat`). |
| FR-11 | Tautan **"Lihat semua"** pada blok Mendekati Persalinan menuju halaman **Kantong Persalinan**. |
| FR-12 | **Klik nama pasien** (pada kedua blok maupun modal pasien telat) membuka **modal ringkasan pasien**: nama pasien, usia kehamilan/HPL bila hamil, serta jadwal kunjungan berikutnya — dengan tombol **"Lihat profil pasien"** menuju halaman profil pasien. |
| FR-13 | **Empty state:** blok Pengingat Kunjungan menampilkan *"Belum ada jadwal kunjungan mendatang."* dan blok Mendekati Persalinan menampilkan *"Belum ada pasien dengan HPL dalam 3 minggu ke depan."* |
| FR-14 | Data Beranda **dihitung ulang setiap halaman dibuka** dari data pasien & jadwal terkini — bukan angka yang disimpan/cache dari sesi sebelumnya. |
| FR-15 | Urutan blok Beranda: **header statistik → Aksi Cepat → banner (kondisional) → Pengingat Kunjungan → Mendekati Persalinan**. |
| FR-16 | **Alat uji UX dihapus** dari prototype: pemilih **"Desain Beranda" 1/2/3** dan toggle **"Preview kosong"** tidak lagi tampil; hanya ada **satu** desain Beranda. |
| FR-17 | Beranda **tidak menampilkan data keuangan** (nominal pemasukan/tagihan) dan tidak menautkan ke halaman Kasir. |
| FR-18 | Seluruh teks Beranda berbahasa Indonesia dengan satuan **"hari"**/**"minggu"** (bukan "hr"/"mgu"), dan istilah mengikuti PRD terkait (`kunjungan`, `jadwal kunjungan`, `HPL`, `Kantong Persalinan`). |
---

## 4. Acceptance Criteria

**US-01 — Ringkasan angka praktik**
- **Given** bidan membuka Beranda, **When** halaman tampil, **Then** terlihat judul **"Ringkasan hari ini"** dengan 2 kartu: **"Kunjungan bulan ini"** dan **"Kehamilan > 38 minggu"**.
- **Given** pasien A punya 2 catatan layanan di bulan ini (ANC dan Nifas) dan pasien B punya 1 catatan KB, **When** Beranda dibuka, **Then** kartu "Kunjungan bulan ini" bernilai **3**.
- **Given** ada 1 pasien bersalin tercatat di bulan ini, **When** Beranda dibuka, **Then** persalinan tersebut **ikut dihitung** pada kartu "Kunjungan bulan ini".
- **Given** catatan layanan pasien bertanggal bulan lalu, **When** Beranda dibuka, **Then** catatan itu **tidak** dihitung pada bulan berjalan.
- **Given** ada 2 pasien hamil dengan usia kehamilan 39 dan 36 minggu, **When** Beranda dibuka, **Then** kartu "Kehamilan > 38 minggu" bernilai **1**.
- **Given** Beranda dibuka, **Then** **tidak ada** sapaan waktu, tanggal, kotak "Mau lakukan apa hari ini?", maupun kartu pemasukan.

**US-02 — Aksi cepat**
- **Given** Beranda dibuka, **Then** blok **"Aksi Cepat"** menampilkan 4 tombol: **Periksa pasien baru**, **Daftar Pasien**, **Kantong Persalinan**, **Pelaporan**.
- **Given** bidan menekan **Pelaporan**, **Then** terbuka halaman **Laporan Puskesmas**.
- **Given** bidan menekan **Daftar Pasien**, **Then** terbuka halaman Daftar Pasien (dengan pencarian) — bukan halaman berlabel "Periksa pasien lama".

**US-03 — Peringatan pasien lewat jadwal**
- **Given** ada 3 pasien dengan jadwal kunjungan yang sudah lewat, **When** Beranda dibuka, **Then** muncul banner **"3 pasien lewat jadwal kunjungan"**.
- **Given** tidak ada pasien dengan jadwal lewat, **When** Beranda dibuka, **Then** banner **tidak muncul** dan tidak menyisakan ruang kosong.
- **Given** banner diklik, **Then** terbuka modal **"Lewat Jadwal Kunjungan"** berisi daftar pasien telat dengan tanggal jadwal dan badge `Lewat N hari`.
- **Given** modal pasien telat terbuka, **When** salah satu nama pasien dipilih, **Then** terbuka **modal ringkasan pasien** dan posisi tetap di Beranda.

**US-04 — Daftar kunjungan & mendekati persalinan**
- **Given** ada 9 pasien dengan jadwal kunjungan mendatang, **When** Beranda dibuka, **Then** blok **Pengingat Kunjungan** menampilkan **6 pasien paling mendesak** dan tombol **"Lihat semua"**.
- **Given** pasien dengan jadwal hari ini, **Then** badge pada barisnya bertuliskan **`Hari ini`**; pasien dengan jadwal 3 hari lagi berbadge **`3 hari lagi`**.
- **Given** "Lihat semua" pada blok Pengingat Kunjungan diklik, **Then** terbuka halaman **Jadwal Kunjungan**.
- **Given** ada 8 pasien hamil dengan HPL ≤ 21 hari, **When** Beranda dibuka, **Then** blok **Mendekati Persalinan** menampilkan **6 pasien dengan HPL terdekat**; "Lihat semua" membuka **Kantong Persalinan**.
- **Given** tidak ada pasien dengan HPL ≤ 21 hari, **Then** muncul teks *"Belum ada pasien dengan HPL dalam 3 minggu ke depan."*
- **Given** tidak ada jadwal kunjungan mendatang, **Then** muncul teks *"Belum ada jadwal kunjungan mendatang."*

**US-05 — Ringkasan singkat pasien**
- **Given** bidan menekan nama pasien di Beranda, **Then** terbuka **modal ringkasan pasien** berisi nama, usia kehamilan/HPL (bila hamil), dan jadwal berikutnya.
- **Given** modal ringkasan terbuka, **When** bidan menekan **"Lihat profil pasien"**, **Then** terbuka halaman profil pasien.
- **Given** bidan menutup modal tanpa memilih aksi, **Then** kembali ke Beranda **tanpa** perubahan data pasien maupun jadwal.

---

## 5. Keputusan Grill (8 butir)

| No. | Topik | Keputusan |
|---|---|---|
| 1 | Sapaan waktu di header | **Tidak dipakai** — desain terbaru memakai judul "Ringkasan hari ini" + 2 kartu statistik; spec sapaan di deskripsi epic dicabut. |
| 2 | Definisi "Kunjungan bulan ini" | **Semua layanan** di bulan berjalan (ANC, Nifas, KB, Wellness, **Persalinan**, **Imunisasi**) — bukan hanya transaksi berbayar. |
| 3 | Label aksi cepat ke-2 | **"Daftar Pasien"** (ikut desain); label "Periksa pasien lama" tidak dipakai. |
| 4 | Perilaku klik banner | Buka **modal "Lewat Jadwal Kunjungan"** (tidak langsung pindah halaman). |
| 5 | Jumlah item per blok | **Maksimal 6 item** + tautan "Lihat semua". |
| 6 | Alat uji UX di prototype | **Dihapus total** (pemilih varian 1/2/3 & toggle "Preview kosong") — varian 1 & 3 tidak relevan lagi. |
| 7 | Kartu statistik | **Tetap 2 kartu**; kartu pemasukan/transaksi kasir tidak ditambahkan di rilis ini. |
| 8 | Klik nama pasien | **Modal ringkasan pasien** + tombol "Lihat profil pasien" (bukan langsung ke halaman profil). |

---

## 6. State & Edge Cases

| No. | Keadaan | Perilaku yang diharapkan |
|---|---|---|
| 1 | Praktik baru — belum ada pasien | Kedua kartu bernilai **0**; kedua blok menampilkan empty state; banner telat tidak tampil. |
| 2 | Semua pasien lewat jadwal | Banner tampil dengan N = jumlah seluruh pasien telat; blok Pengingat Kunjungan menampilkan empty state. |
| 3 | Pasien punya 2 catatan layanan di bulan yang sama | Dihitung **2** pada kartu "Kunjungan bulan ini" (per catatan, bukan per pasien). |
| 4 | Pergantian bulan (mis. 1 Okt) | Angka "Kunjungan bulan ini" **reset** menghitung bulan berjalan saat halaman dibuka. |
| 5 | HPL sudah lewat (mis. lewat 4 hari) | Pasien **tetap tampil** di blok Mendekati Persalinan (paling atas) dengan penanda `HPL lewat` — bukan disembunyikan. |
| 6 | Pasien hamil tanpa data HPHT/HPL yang valid | Tidak masuk blok Mendekati Persalinan; kartu statistik tetap dihitung dari status kehamilan yang berlaku. |
| 7 | Jadwal kunjungan diubah/dihapus dari halaman Jadwal | Beranda menampilkan kondisi terbaru saat halaman dibuka ulang (bukan angka lama). |
| 8 | Modal "Lewat Jadwal Kunjungan" dibuka saat daftar telat panjang (>10 pasien) | Modal dapat di-scroll; tidak memotong daftar dan tidak menutup otomatis. |
| 9 | Nama pasien sangat panjang / nama anak tanpa nama orang tua | Baris tetap satu baris dengan pemotongan teks (ellipsis); badge tetap terbaca. |
---

## 7. Non-Functional Requirements

| ID | Requirement |
|---|---|
| NFR-01 | **Performa:** Beranda tampil dalam **< 2 detik** pada perangkat kelas menengah; seluruh angka statistik dihitung dari data yang sudah dimuat (tanpa permintaan jaringan tambahan saat halaman dibuka). |
| NFR-02 | **Aksesibilitas:** setiap kartu & aksi punya label teks (bukan hanya ikon); status pada badge disampaikan lewat **teks** (`Hari ini`, `N hari lagi`, `HPL lewat`), bukan hanya warna; target sentuh minimal **44×44 px**; kontras teks memenuhi WCAG AA. |
| NFR-03 | **Konsistensi bahasa & format:** Bahasa Indonesia; satuan **"hari"**/**"minggu"** (bukan "hr"/"mgu"); istilah mengikuti PRD terkait — `kunjungan`, `jadwal kunjungan`, `HPL`, `Kantong Persalinan`, `Pasien Baru`; format tanggal Indonesia. |
| NFR-04 | **Privasi:** Beranda hanya menampilkan **nama pasien** dan informasi jadwal/kehamilan yang diperlukan; tidak menampilkan nomor HP penuh maupun catatan medis pada layar daftar. |

---

## 8. Dependencies & Open Items

**Ketergantungan:**
- **PTS-367 (Login & Manajemen Data Pasien)** — master data pasien yang menjadi sumber seluruh hitungan Beranda.
- **PTS-374 (Kantong Persalinan)** — data status hamil, HPHT/HPL, dan hari persalinan perkiraan; tujuan tombol "Lihat semua" blok Mendekati Persalinan.
- **PTS-376 (Pengingat & Jadwal Kunjungan)** — data jadwal kunjungan berikutnya & perhitungan selisih hari (`Hari ini` / `N hari lagi` / `Lewat N hari`); tujuan tombol "Lihat semua" blok Pengingat Kunjungan.
- **PTS-604 (Pengaturan & Akun)** — nama praktik pada header aplikasi; keluar akun dari halaman Lainnya.
- **PTS-605 task lain** — PTS-609 (Wireframe & Research), PTS-613 (Hifi Design), PTS-617 (PRD ini), PTS-621 (Handover), PTS-625 (Development), PTS-629 (Announcement).
- **PrimaCare** — skema akun & data pasien harus tetap sejalan (keputusan 14 Sep: CRM Bidan berdiri bersama PrimaCare).

**Open items:**
1. Perlu tidaknya **kartu ke-3** (mis. **Transaksi kasir bulan ini**) setelah Kasir & Harga Layanan (PTS-602) stabil.
2. Perlu tidaknya tautan **"Lihat semua"** juga di dalam modal "Lewat Jadwal Kunjungan" (menuju halaman Jadwal Kunjungan).
3. Kapan **notifikasi** diaktifkan (saat ini ditunda); rencana sementara: **badge pada menu** + daftar harian di Beranda.
4. Perlu tidaknya **urutan blok yang bisa diatur bidan** bila jumlah blok Beranda bertambah.

**Iterasi berikutnya (di luar PRD ini):**
- Kartu pemasukan/transaksi kasir · grafik tren kunjungan (mingguan/bulanan) · pengingat otomatis (push/WhatsApp terjadwal) · ringkasan imunisasi anak di Beranda · pencarian cepat pasien langsung dari Beranda.

## 9. Referensi

- Prototype acuan (desain final): `docs/prototype/CRMBidan_17sep2026.html` · live: <https://dhaniprisan.github.io/crm-bidan/>
- Keputusan grill Beranda: `features/crm-bidan/context/ADR-beranda.md`
- Catatan produk & pelajaran implementasi: `features/crm-bidan/context/CONTEXT.md`
- PRD terkait: **PTS-367** (`prd-PTS-367-v1.md`) · **PTS-374** (`prd-PTS-374-v1.md`) · **PTS-376** (`prd-PTS-376-wa-reminder-v3.md`) · **PTS-602** (`prd-PTS-602-kasir-harga-layanan-v1.md`) · **PTS-604** (`prd-PTS-604-pengaturan-akun-v1.md`)
- Epic produk: **PTS-605** — `[CRM][Bidan] Beranda (Dashboard)` · O5-03 · Level S3
- Copy deck naskah WA (untuk konteks istilah): `docs/copy-deck-wa-ringkasan-invoice.md`
- Roadmap inisiatif produk: `features/crm-bidan/context/` (blok O1–O16)

## 10. Riwayat Revisi

| Versi | Tanggal | Perubahan |
|---|---|---|
| **v1** | 17 Sep 2026 | Versi pertama: **5 user story · 18 FR · 22 AC · 9 edge case · 4 NFR**. Disusun dari **desain final prototype 17 Sep** + hasil grill 8 butir. Perubahan penting dari spec epic lama: **sapaan waktu & pertanyaan pemandu dicabut**, label aksi cepat ke-2 menjadi **"Daftar Pasien"**, definisi **"Kunjungan bulan ini" diperluas ke semua layanan (termasuk persalinan & imunisasi)**, blok dibatasi **6 item**, dan **alat uji UX dihapus** dari prototype. |
