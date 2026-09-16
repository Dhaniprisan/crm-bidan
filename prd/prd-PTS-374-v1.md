## 1. Metadata

| Item | Keterangan |
|:--|:--|
| Feature | Kantong Persalinan (Perkiraan Kelahiran) |
| Product | CRM Bidan (mobile-first, praktik mandiri bidan) |
| Author | Dhani Prisantika |
| Date | 15 Sep 2026 |
| Version | v1 |
| Priority | High |
| Status | Draft |
| Primary User | Bidan praktik mandiri (superadmin klinik) |
| Module | Kantong Persalinan |
| Epic | PTS-374 — [CRM][Bidan] Kantong Persalinan |
| PRD ticket | PTS-382 |
| Kode referensi | O3-02 · Level S1 · Platform: Mobile web |
| Desain | Prototype CRMBidan_14sep2026 · halaman `/kantong-bidan` dan `/kantong-bidan/:year/:month` |

## 2. Overview & Scope Boundary

Kantong Persalinan adalah **alat operasional bidan** untuk melihat berapa pasien yang diperkirakan melahirkan pada tiap bulan, sehingga bidan dapat menyiapkan jadwal, perlengkapan, dan pendampingan persalinan. Fitur ini **bukan** kanal komunikasi ke pasien: tidak ada pengiriman pesan otomatis, tidak ada aksi tandai-bersalin, dan tidak ada export. Semua aksi di layar bersifat melihat dan berpindah (navigasi).

Perhitungan perkiraan kelahiran bertumpu pada data kehamilan yang lahir di epic Login & Daftar Pasien (PTS-367/PRD PTS-370): HPHT, HPL, usia kehamilan, dan trimester. Kantong Persalinan adalah **konsumen data**, bukan pembuat data pasien.

**IN SCOPE**
- Grid perkiraan kelahiran 12 bulan per tahun (jumlah pasien per bulan, bulan berjalan disorot, bulan kosong ditandai).
- Pemilih tahun dan aksi "Hari Ini".
- Detail bulan: baris pemilih Jan–Des, daftar kartu pasien pada bulan tersebut, urut HPL terdekat.
- Navigasi dari kartu pasien ke Profil Pasien.
- Penanda pasien yang sudah lewat HPL namun belum ada catatan persalinan.
- Penanda pasien hamil yang belum memiliki HPHT (HPL belum dapat dihitung).
- Aturan HPL aktif: HPL hasil USG dipakai bila terisi, jika tidak memakai HPL dari HPHT.
- Aturan status sudah bersalin yang diturunkan dari catatan Kartu Persalinan.

**OUT OF SCOPE**
- Checklist kantong / kesiapan persalinan (item perlengkapan persalinan) — tidak dikerjakan.
- Pengiriman WA/pengingat otomatis ke pasien (mekanisme WA 1-click ada di epic PTS-376).
- Export/unduh daftar per bulan (kebutuhan pelaporan ke puskesmas ada di epic PTS-603 — Laporan Puskesmas).
- Aksi menandai pasien sudah bersalin dari layar Kantong; pencatatan persalinan dilakukan pada Kartu Persalinan (epic PTS-375).
- Section khusus "pasien tanpa HPHT" dan section khusus "pasien lewat HPL" di halaman Kantong.
- Kalender jadwal kunjungan (ada di epic PTS-375 — Jadwal Kunjungan).
- Pengingat jadwal kontrol (epic PTS-605 — Beranda).

## 3. Background & Problem Statement

**Kondisi saat ini.** Bidan praktik mandiri memperkirakan waktu persalinan pasien dengan menghitung manual dari HPHT (roda kehamilan atau kalkulator), lalu mencatatnya di buku KIA maupun buku catatan pribadi. Tidak ada satu tempat untuk melihat sebaran perkiraan kelahiran pada bulan-bulan mendatang.

**Masalah.**
1. Bidan tidak punya gambaran beban kerja per bulan — berapa pasien yang diperkirakan lahiran bulan ini, bulan depan, dan seterusnya.
2. Persiapan persalinan (jadwal jaga, perlengkapan, pendampingan, transportasi) dilakukan mendadak karena tidak ada daftar yang bisa dilihat lebih awal.
3. Pasien yang HPL-nya sudah lewat namun belum ada catatan persalinan mudah terlewat dari pantauan.
4. Pasien hamil yang data HPHT-nya belum lengkap "menghilang" dari perkiraan tanpa disadari, sehingga tidak ikut direncanakan.

**Mengapa penting.** Kantong Persalinan memberi bidan satu kendali perencanaan: sebaran persalinan per bulan, daftar nama yang bisa langsung dibuka, serta penanda untuk pasien yang perlu perhatian. Data ini juga menjaga kesinambungan asuhan karena pasien yang sama tetap dapat ditelusuri lintas bulan.

## 4. User Stories

| ID | User Story | Ringkasan Acceptance Criteria | Priority |
|:--|:--|:--|:--|
| US-01 | Sebagai bidan, saya ingin melihat jumlah pasien yang diperkirakan melahirkan tiap bulan dalam satu tahun, agar saya bisa menyiapkan jadwal dan perlengkapan persalinan | Grid 12 bulan menampilkan jumlah pasien per bulan, bulan kosong ditandai "–", bulan berjalan disorot | Must Have |
| US-02 | Sebagai bidan, saya ingin berpindah tahun dan kembali ke bulan berjalan, agar bisa merencanakan lintas tahun tanpa mencari manual | Pemilih tahun menampilkan jumlah pasien per tahun; aksi "Hari Ini" membuka bulan berjalan | Should Have |
| US-03 | Sebagai bidan, saya ingin membuka satu bulan dan melihat daftar pasiennya, agar saya tahu siapa saja yang diperkirakan lahir pada bulan itu | Detail bulan menampilkan jumlah pasien, kartu pasien urut HPL terdekat, dan pemilih bulan Jan–Des | Must Have |
| US-04 | Sebagai bidan, saya ingin membuka data pasien dari daftar bulan tersebut, agar saya bisa langsung menindaklanjuti pasiennya | Menekan kartu pasien membuka Profil Pasien | Must Have |
| US-05 | Sebagai bidan, saya ingin tahu pasien yang sudah lewat HPL namun belum ada catatan persalinan, agar tidak ada pasien yang terlewat dipantau | Kartu pasien menampilkan penanda "Lewat HPL N hr" | Must Have |
| US-06 | Sebagai bidan, saya ingin pasien yang sudah bersalin otomatis tidak lagi masuk perkiraan kelahiran, agar perencanaan tetap akurat | Pasien dengan catatan Kartu Persalinan dianggap sudah bersalin; status kehamilan menjadi tidak hamil dan keluar dari Kantong Persalinan | Must Have |
| US-07 | Sebagai bidan, saya ingin HPL memakai hasil USG bila tersedia dan tetap menyimpan HPL dari HPHT, agar perkiraan kelahiran lebih akurat | HPL aktif = HPL USG bila terisi; HPL HPHT tetap tersimpan dan tampil sebagai pembanding | Should Have |
| US-08 | Sebagai bidan, saya ingin tahu pasien hamil yang HPHT-nya belum lengkap, agar datanya bisa dilengkapi dan pasien ikut masuk perencanaan | Kartu pasien di Daftar Pasien menampilkan penanda "HPL belum ada" beserta ajakan melengkapi HPHT | Should Have |

**Prioritas (MoSCoW):** Must Have = wajib pada rilis pertama · Should Have = penting, dapat menyusul · Could Have = opsional · Won't Have = tidak pada rilis ini.

## 5. Functional Requirements

### Akses & Navigasi

| ID | Requirement | Priority | MoSCoW |
|:--|:--|:--|:--|
| FR-01 | Sistem menyediakan menu Kantong Persalinan yang dapat dibuka setelah bidan login (dari navigasi utama dan aksi cepat Beranda) | High | Must |
| FR-02 | Sistem menampilkan tautan "Kantong Persalinan" pada section "Mendekati Persalinan" di Beranda | Medium | Should |
| FR-03 | Sistem membatasi data Kantong Persalinan pada pasien dari klinik yang sama dengan akun bidan | High | Must |

### Grid Perkiraan Kelahiran

| ID | Requirement | Priority | MoSCoW |
|:--|:--|:--|:--|
| FR-04 | Sistem menampilkan header: label "Perkiraan Kelahiran", judul "Kantong Persalinan", dan deskripsi kegunaan fitur | Medium | Should |
| FR-05 | Sistem menampilkan jumlah pasien yang diperkirakan lahir pada tahun yang dipilih | High | Must |
| FR-06 | Sistem menampilkan grid 12 bulan (3 kolom × 4 baris) berisi jumlah pasien yang diperkirakan lahir pada tiap bulan | High | Must |
| FR-07 | Sistem menandai bulan tanpa perkiraan kelahiran dengan tanda "–" | Medium | Should |
| FR-08 | Sistem menyorot bulan berjalan sebagai penanda posisi saat ini | Medium | Should |
| FR-09 | Sistem menyediakan pemilih tahun (tahun sebelumnya dan tahun berikutnya) | High | Must |
| FR-10 | Sistem menyediakan aksi "Hari Ini" untuk langsung membuka bulan berjalan | Medium | Should |

### Detail Bulan

| ID | Requirement | Priority | MoSCoW |
|:--|:--|:--|:--|
| FR-11 | Sistem menampilkan judul "<Nama Bulan> <Tahun>" dan jumlah pasien yang diperkirakan lahir pada bulan tersebut | High | Must |
| FR-12 | Sistem menyediakan baris pemilih Januari–Desember berisi jumlah pasien per bulan untuk berpindah bulan tanpa kembali ke grid | Medium | Should |
| FR-13 | Sistem menampilkan kartu pasien berisi nama, HPL aktif, badge trimester (TM 1/2/3), dan progress bar usia kehamilan | High | Must |
| FR-14 | Sistem mengurutkan kartu pasien berdasarkan HPL terdekat | Medium | Should |
| FR-15 | Sistem membuka Profil Pasien ketika kartu pasien ditekan | High | Must |
| FR-16 | Sistem menampilkan keadaan kosong "Belum ada pasien dengan taksiran persalinan di bulan ini." bila tidak ada pasien pada bulan tersebut | Medium | Should |
| FR-17 | Sistem menyediakan aksi kembali ke halaman Kantong Persalinan | Medium | Should |

### Data & Perhitungan

| ID | Requirement | Priority | MoSCoW |
|:--|:--|:--|:--|
| FR-18 | Sistem hanya menghitung pasien dengan status kehamilan hamil | High | Must |
| FR-19 | Sistem memakai HPL aktif, yaitu HPL hasil USG bila terisi dan HPL dari HPHT (HPHT + 280 hari) bila tidak | High | Must |
| FR-20 | Sistem menurunkan usia kehamilan dan trimester dari HPL aktif | High | Must |
| FR-21 | Sistem menampilkan penanda "Lewat HPL N hr" pada kartu pasien bila HPL aktif sudah terlewati dan pasien belum memiliki catatan persalinan | High | Must |
| FR-22 | Sistem mengecualikan pasien dari perhitungan bila HPHT maupun HPL belum ada, serta menampilkan penanda "HPL belum ada" dan ajakan melengkapi HPHT pada kartu pasien di Daftar Pasien | High | Must |
| FR-23 | Sistem menganggap pasien sudah bersalin bila terdapat catatan Kartu Persalinan: status kehamilan otomatis menjadi tidak hamil sehingga pasien keluar dari perhitungan Kantong Persalinan dan dari daftar "Mendekati Persalinan" | High | Must |

### Data Pendukung & Sinkronisasi

| ID | Requirement | Priority | MoSCoW |
|:--|:--|:--|:--|
| FR-24 | Sistem menyimpan HPL hasil USG sebagai field data obstetri pada modul CRM Bidan (disimpan, belum ditampilkan pada form pemeriksaan PrimaCare) | Medium | Should |
| FR-25 | Sistem menampilkan HPL dari HPHT dan HPL hasil USG sebagai pembanding pada Profil Pasien beserta penanda acuan yang dipakai | Medium | Should |
| FR-26 | Sistem menyinkronkan perubahan status kehamilan menjadi tidak hamil ke PrimaCare | Medium | Should |
## 6. Acceptance Criteria

### US-01 — Kalender perkiraan kelahiran

1. **Given** bidan membuka menu Kantong Persalinan, **When** halaman dimuat, **Then** sistem menampilkan grid 12 bulan pada tahun berjalan beserta jumlah pasien yang diperkirakan lahir pada tiap bulan.
2. **Given** grid perkiraan kelahiran terbuka, **When** suatu bulan tidak memiliki pasien dengan perkiraan kelahiran, **Then** kartu bulan tersebut menampilkan tanda "–".
3. **Given** grid perkiraan kelahiran terbuka, **When** bulan berjalan ditampilkan, **Then** kartu bulan tersebut diberi penanda khusus sebagai posisi saat ini.
4. **Given** grid perkiraan kelahiran terbuka, **When** bidan melihat keterangan tahun, **Then** sistem menampilkan total jumlah pasien yang diperkirakan lahir pada tahun tersebut.

### US-02 — Pindah tahun & kembali ke bulan berjalan

1. **Given** grid perkiraan kelahiran tahun berjalan terbuka, **When** bidan menekan pemilih tahun sebelumnya, **Then** sistem menampilkan grid tahun sebelumnya beserta jumlah pasiennya.
2. **Given** bidan berada pada tahun lain, **When** menekan aksi "Hari Ini", **Then** sistem membuka detail bulan berjalan pada tahun berjalan.
3. **Given** suatu tahun tidak memiliki pasien dengan perkiraan kelahiran, **When** grid tahun tersebut dimuat, **Then** seluruh kartu bulan menampilkan "–" dan keterangan tahun menampilkan total 0.

### US-03 — Daftar pasien pada satu bulan

1. **Given** bidan menekan salah satu kartu bulan, **When** halaman detail bulan dimuat, **Then** sistem menampilkan judul bulan beserta jumlah pasien yang diperkirakan lahir pada bulan tersebut.
2. **Given** detail bulan terbuka, **When** daftar pasien dimuat, **Then** setiap kartu pasien menampilkan nama, HPL aktif, badge trimester, dan progress bar usia kehamilan.
3. **Given** detail bulan terbuka, **When** bidan memilih bulan lain pada baris pemilih Januari–Desember, **Then** sistem berpindah bulan tanpa kembali ke grid.
4. **Given** detail bulan terbuka, **When** bidan menekan aksi kembali, **Then** sistem menampilkan kembali grid Kantong Persalinan.

### US-04 — Membuka profil pasien dari daftar bulan

1. **Given** detail bulan menampilkan kartu pasien, **When** kartu tersebut ditekan, **Then** sistem membuka Profil Pasien pasien tersebut.
2. **Given** bulan belum memiliki pasien, **When** detail bulan dimuat, **Then** sistem menampilkan keadaan kosong "Belum ada pasien dengan taksiran persalinan di bulan ini." tanpa kartu pasien.

### US-05 — Penanda pasien yang lewat HPL

1. **Given** pasien belum memiliki catatan Kartu Persalinan dan HPL aktifnya sudah terlewati, **When** kartu pasien ditampilkan, **Then** sistem menampilkan penanda "Lewat HPL N hr" dengan N adalah jumlah hari sejak HPL.
2. **Given** pasien belum memiliki catatan Kartu Persalinan dan HPL aktifnya belum terlewati, **When** kartu pasien ditampilkan, **Then** sistem tidak menampilkan penanda "Lewat HPL".
3. **Given** pasien sudah memiliki catatan Kartu Persalinan, **When** daftar perkiraan kelahiran dihitung, **Then** pasien tersebut tidak lagi ditampilkan.

### US-06 — Status sudah bersalin otomatis

1. **Given** bidan menyimpan Kartu Persalinan untuk seorang pasien, **When** penyimpanan berhasil, **Then** status kehamilan pasien berubah menjadi tidak hamil.
2. **Given** status kehamilan pasien sudah menjadi tidak hamil, **When** bidan membuka Kantong Persalinan, **Then** pasien tersebut tidak lagi dihitung pada bulan HPL-nya maupun pada daftar "Mendekati Persalinan" di Beranda.
3. **Given** status kehamilan pasien berubah menjadi tidak hamil, **When** Profil Pasien dibuka, **Then** tab layanan tetap memuat Persalinan, Nifas, dan KB sesuai kategori pasien "Ibu (Tidak Hamil)".

### US-07 — HPL aktif dari hasil USG

1. **Given** pasien memiliki HPHT dan HPL hasil USG, **When** perkiraan kelahiran dihitung, **Then** sistem memakai HPL hasil USG sebagai HPL aktif dan menempatkan pasien pada bulan HPL tersebut.
2. **Given** pasien hanya memiliki HPHT, **When** perkiraan kelahiran dihitung, **Then** sistem memakai HPL dari HPHT (HPHT + 280 hari).
3. **Given** pasien memiliki kedua nilai HPL, **When** tab Ringkasan Profil Pasien dibuka, **Then** kedua nilai ditampilkan sebagai pembanding beserta penanda acuan yang dipakai.

### US-08 — Penanda pasien hamil tanpa HPHT

1. **Given** pasien berstatus hamil namun HPHT belum diisi, **When** kartu pasien tampil pada Daftar Pasien, **Then** sistem menampilkan penanda "HPL belum ada" beserta ajakan melengkapi HPHT.
2. **Given** pasien berstatus hamil tanpa HPHT, **When** Kantong Persalinan dihitung, **Then** pasien tersebut tidak diikutkan pada perhitungan bulan mana pun.

## 7. State & Edge Cases

| Kondisi | Perilaku yang diharapkan |
|:--|:--|
| Tahun tanpa pasien dengan perkiraan kelahiran | Grid tetap tampil; semua kartu bulan "–"; keterangan tahun menampilkan total 0 |
| Bulan tanpa pasien | Detail bulan menampilkan keadaan kosong, pemilih bulan tetap dapat dipakai |
| Pasien hamil tanpa HPHT dan tanpa HPL USG | Tidak diikutkan perhitungan; ditandai "HPL belum ada" di Daftar Pasien + ajakan melengkapi HPHT |
| Pasien hamil dengan HPL USG terisi | HPL USG menjadi acuan; pasien ditempatkan pada bulan HPL USG |
| HPL aktif sudah lewat dan belum ada catatan persalinan | Kartu pasien menampilkan "Lewat HPL N hr"; pasien tetap berada pada bulan HPL-nya |
| HPL aktif sudah lewat dan sudah ada catatan persalinan | Pasien tidak lagi dihitung (status kehamilan menjadi tidak hamil) |
| Kartu Persalinan dicatat lebih awal dari HPL (mis. persalinan preterm) | Pasien keluar dari Kantong Persalinan sejak catatan tersimpan; data persalinan tetap tercatat pada Profil Pasien |
| Tipe klinik/kategori akun bukan Bidan | Modul CRM Bidan tidak ditampilkan (mengikuti aturan akses epic PTS-367) |
| Layanan CRM Bidan/PrimaCare tidak tersedia | Daftar tidak dapat dimuat; tampil pesan kesalahan dengan aksi coba lagi (tidak ada draft di layar baca) |
| Perhitungan tanggal berganti hari (mis. lewat tengah malam) | Usia kehamilan, trimester, dan hitungan "Lewat HPL N hr" mengikuti tanggal hari ini saat halaman dimuat |
| Pasien non aktif (diarsipkan) | Tidak muncul pada Kantong Persalinan meskipun data kehamilannya ada |

## 8. Kepemilikan Data & Penempatan Field

| Data | Sumber / Penempatan Catatan | Catatan |
|:--|:--|:--|
| Identitas pasien (nama, jenis kelamin, tanggal lahir, NIK, kontak, alamat) | Master pasien PrimaCare | Dibaca oleh Kantong Persalinan, tidak diubah dari layar ini |
| Nomor RM | Standar PrimaCare | Tidak dibuat ulang oleh CRM Bidan |
| Kode klinik & akun pengguna | PrimaCare (onboarding) | Menentukan cakupan data yang tampil |
| Status pasien (aktif / non aktif) | PrimaCare (hasil arsip) | Pasien non aktif tidak muncul pada Kantong Persalinan |
| HPHT, HPL (dari HPHT) | Modul obstetri di sisi CRM Bidan | Disimpan, belum ditampilkan pada form pemeriksaan PrimaCare |
| HPL hasil USG | Modul obstetri di sisi CRM Bidan (field baru) | Menjadi HPL aktif bila terisi |
| Status kehamilan (hamil / tidak hamil) | Modul CRM Bidan, tersinkron ke PrimaCare | Berubah otomatis saat Kartu Persalinan tercatat |
| Badge trimester, usia kehamilan, progress bar, "Lewat HPL N hr" | Nilai turunan (tidak disimpan) | Dihitung dari HPL aktif & tanggal hari ini |
| Catatan Kartu Persalinan | Rekam medis (epic PTS-375) | Menjadi penentu status sudah bersalin; tampil pada list RM sebagai catatan |

## 9. Non-Functional Requirements

| ID | Requirement | MoSCoW |
|:--|:--|:--|
| NFR-01 | Aplikasi memerlukan koneksi internet (tidak ada mode offline); layar Kantong bersifat baca data | Must |
| NFR-02 | Seluruh data hanya dapat diakses setelah bidan login, dan dibatasi pada pasien dari klinik akun tersebut | Must |
| NFR-03 | Tampilan mobile-first; grid 12 bulan tetap terbaca pada layar kecil (3 kolom) | Must |
| NFR-04 | Perhitungan usia kehamilan, trimester, dan hitungan hari ke HPL mengikuti tanggal hari ini, tanpa cache nilai lama | Must |
| NFR-05 | Tidak ada aksi destruktif pada layar Kantong Persalinan (tanpa hapus/arsip/tandai bersalin) | Must |
| NFR-06 | Perubahan status kehamilan akibat pencatatan persalinan harus tercatat (jejak audit) dan tidak boleh hilang saat sinkronisasi gagal | Should |
| NFR-07 | Riwayat jumlah pasien per bulan tetap akurat untuk tahun-tahun sebelumnya (data historis tidak dihitung ulang dari status sekarang) | Should |

## 10. Dependencies & Open Items

**Dependency ke tim PrimaCare**
- Skema pasien, kode klinik, dan nomor RM (mengikuti standar PrimaCare).
- Mekanisme autentikasi & akses modul berdasarkan tipe klinik/kategori akun.
- Endpoint pembacaan data pasien untuk kebutuhan perhitungan perkiraan kelahiran.
- Sinkronisasi perubahan status kehamilan (tidak hamil) dan penampilannya pada PrimaCare.

**Dependency lintas epic CRM Bidan**
- PTS-367 (PTS-370): sumber data HPHT, HPL, status kehamilan, dan kategori pasien.
- PTS-375: pencatatan Kartu Persalinan yang memicu status sudah bersalin, serta relasi dengan Jadwal Kunjungan.
- PTS-605: section "Mendekati Persalinan" dan tautan ke Kantong Persalinan di Beranda.
- PTS-376: mekanisme WA 1-click untuk tindak lanjut pasien (di luar layar Kantong).

**Open Items (UX)**
- Label halaman: memakai istilah "Kantong Persalinan" dengan label "Perkiraan Kelahiran" (mengikuti prototype) atau perlu penyesuaian istilah.
- Warna/penanda khusus untuk badge "Lewat HPL N hr" dan badge "HPL belum ada" — menunggu keputusan visual UX (high-fidelity).
- Penempatan field "HPL hasil USG" pada form (di dalam accordion data lain atau sejajar HPHT).

## 11. Lampiran

- Prototype acuan: `docs/prototype/CRMBidan_14sep2026.html` — halaman `/kantong-bidan` dan `/kantong-bidan/:year/:month`.
- Rekap prototype & keputusan scope: `~/.hermes/states/bidan-crm-mockup-rekap.md`.
- Keputusan grill & constraint: `features/crm-bidan/context/CONTEXT.md` (bagian Kantong Persalinan).
- Catatan integrasi PrimaCare: `docs/crm-bidan-integrasi-primacare.md`.
- Panduan interview bidan: `~/.hermes/states/bidan-crm-interview-guide.md` (butir Kantong Bidan & Jadwal).
- Rumus HPL/usia kehamilan/trimester: PRD PTS-370 — section "Perhitungan HPL, Usia Kehamilan & Trimester".

**Riwayat Revisi**

| Versi | Tanggal | Perubahan |
|:--|:--|:--|
| v1 | 15 Sep 2026 | Draft awal berdasarkan grill 8 butir (nilai fitur, checklist, aksi layar, pasien lewat HPL, HPL USG, pasien tanpa HPHT, isi kartu, export) |
| v1.1 | 15 Sep 2026 | Atas permintaan Boss: section **Before vs After** dan **Integrasi dengan PrimaCare** dihapus dari PRD (fitur ini bagian dari aplikasi secara keseluruhan, bukan permukaan integrasi baru). Section setelahnya dinomori ulang (User Stories 3 → NFR 8). Berlaku di Jira PTS-382 |

**Status inject (14 Sep 2026, 19:45 WIB):** PRD **PTS-374 versi ramping section 1–10** sudah di-inject ke **PTS-382** (Task "PRD", child PTS-374) — verified dari Jira: **56 node · 24 heading · 10 tabel (79 baris) · 10 orderedList (34 butir) · 2 bulletList · 0 baris pemisah `:--`**, panjang teks **19.196 karakter** (limit ~31.800, sisa ~12,6 rb → masih lega untuk edit manual Boss). Yang **tidak** di-inject (internal): Metadata, Dependencies & Open Items, Lampiran + Riwayat Revisi — ada di file lokal `prd-PTS-374-v1.md`. File versi Jira: `prd-PTS-374-v1-jira.md`.
Perbaikan injector: regex `isSep` di `features/crm-bidan/jira/prd-to-jira.mjs` diperlebar jadi `/^\|[\s:|-]+\|$/` (sebelumnya hanya cocok untuk tabel 1 kolom → baris `:--` masuk jadi data). Sudah dicatat di `jira-adf-automation` → `references/prd-markdown-to-adf.md`.

## 13. Revisi PRD PTS-374 setelah inject (14 Sep 2026, 20:0x WIB)
Permintaan Boss: **"Before vs After nggak perlu… ini fitur emang bagian dari whole apps-nya, terus integrasi dengan Primacare juga kayaknya gaperlu dimasukin"** → dua section dibuang dari **PTS-382** (surgical edit, bukan re-inject): "Before vs After" (2 node) + "Integrasi dengan PrimaCare" (4 node), lalu section setelahnya dinomori ulang. Hasil verified: **50 node · teks 16.533 · 8 section** → 1. Overview & Scope Boundary · 2. Background & Problem Statement · 3. User Stories · 4. Functional Requirements · 5. Acceptance Criteria · 6. State & Edge Cases · 7. Kepemilikan Data & Penempatan Field · 8. Non-Functional Requirements. Konten inti utuh (26 FR · 8 US · 7 NFR).
Markdown lokal disinkronkan: draft (`prd-PTS-374-v1.md`) + versi Jira (`prd-PTS-374-v1-jira.md`) sudah tanpa kedua section itu, Riwayat Revisi ditambah baris **v1.1**. Dicatat di skill `crm-bidan-epic-briefs`: section Integrasi PrimaCare & Before vs After **ditawarkan, bukan wajib**, untuk PRD fitur yang bagian dari aplikasi keseluruhan.

## 14. Guidance UX Kantong Persalinan (15 Sep 2026)
Artefak: `docs/guidance-ux-kantong-persalinan.html` + `.pdf` (**9 halaman A4**) — render ulang pakai `bash docs/scripts/render-guidance-ux.sh guidance-ux-kantong-persalinan` (script juga menghitung baris per halaman untuk deteksi halaman meluber).
Isi: cover (6 area berubah · 5 keputusan UX · 8 bagian tetap) → satu halaman per perubahan (mockup "sekarang vs usulan" + aturan) → halaman keputusan UX (tabel opsi + rekomendasi) → halaman "yang tidak berubah" + referensi.
6 perubahan yang diminta ke UX: (1) badge **"Lewat HPL N hr"** di kartu detail bulan — token rose, sejajar badge TM; (2) penanda **"HPL belum ada"** + tautan "Lengkapi HPHT" di kartu Daftar Pasien (usulan amber); (3) field **HPL hasil USG** opsional di form + label HPL otomatis dipertegas; (4) tab Ringkasan menampilkan **dua HPL** (aktif + pembanding) dengan penanda acuan; (5) umpan balik **"sudah bersalin"** (banner saat simpan Kartu Persalinan + chip di header profil); (6) route diselaraskan ke `/kantong-persalinan` & checklist Kantong 10 item **jangan dihidupkan**.
5 keputusan menunggu UX: nada badge HPL-belum-ada · penempatan field HPL USG · format dua HPL · bentuk umpan balik sudah bersalin · perlu-tidaknya chip "Sudah bersalin". Semua warna memakai token prototype yang sudah ada (rose/amber/sage/plum/biru) — tanpa palet baru.
Catatan QA render: `@page{size:A4}` + `.page{padding:13mm 12mm}`; mockup 2 ponsel 63mm + `.notes{flex:1 1 100%}` supaya catatan turun ke bawah (sebelumnya 3 kolom → tiap halaman meluber jadi 16 halaman Letter). Sampul dipadatkan lewat kelas `.cover`.

## 15. Prep PRD WA Follow-up (PTS-376) — 15 Sep 2026
Epic **PTS-376** `[CRM][Bidan] WA Follow-up 1-Click (Ringkasan & Edukasi)` · O3-04 · S2 · start 7 Sep · due 21 Sep. PRD task = **PTS-386** (Task "PRD", child PTS-376, masih kosong). Deskripsi epic sudah memuat 3 bagian: ringkasan layanan via WA, edukasi per trimester, dan "catatan untuk tim UX".

**Bukti prototype (verified dari source, bukan asumsi):**
- Helper WA: `Ii(noHp, teks)` → `https://wa.me/<nomor ternormalisasi>?text=<encoded>`; `Li()` membuka `window.open`. Normalisasi 08xx → 628xx.
- **Modal Pengingat Jadwal sudah ADA** · komponen `Ri`, dipicu dari **Profil Pasien → tab Ringkasan** (klik kartu jadwal). Teks: `Halo <nama>, ini pengingat jadwal kontrol pada <tanggal>. Ditunggu kedatangannya ya 🙏 — <nama bidan||Bidan>` lalu dikirim via `Li(e.noHp, teks)`. Modal ini juga bisa **mengubah tanggal jadwal kontrol**, plus aksi lihat profil & catat layanan.
- **Halaman Jadwal Kunjungan (`/kunjungan`) belum punya aksi WA sama sekali** (0 kemunculan "Kirim"/"WA") — sesuai catatan epic.
- Tombol "Kirim ringkasan ke pasien (WA)" hanya dirender bila `noHp` tersedia (komponen `Aa({noHp,text,label})`), bersanding "Lanjut ke Kasir" di layar ringkasan sukses ("Berhasil dicatat").
- Generator ringkasan per layanan: `xa` (ANC), `Sa` (nifas), `Ca` (persalinan), + KB/imunisasi/wellness; diakhiri tanda tangan `— <bidan||Bidan>`.
- Edukasi: 6 materi kurasi (TM1 2 · TM2 2 · TM3 2), filter Semua/TM1/TM2/TM3 + pencarian; modal "Kirim ke WhatsApp" berisi input **"Nomor WhatsApp tujuan"** (wajib, placeholder 08xxxxxxxxxx) → format `Edukasi kehamilan — <judul>` + poin `•` + tanda tangan.
- **Belum ada di prototype (masuk epic, perlu dirancang UX):** (a) aksi pengingat WA dari halaman Jadwal Kunjungan, (b) riwayat follow-up per pasien (kapan terakhir WA dikirim + materi/ringkasan apa).

**Grill untuk PRD PTS-376:** Q1 kanal pengiriman (wa.me manual vs WA API) · Q2 penempatan pengingat jadwal · Q3 riwayat follow-up · Q4 sumber nomor (anak/orang tua) · Q5 pasien tanpa No. HP · Q6 preview/edit teks sebelum kirim · Q7 materi kurasi tetap · Q8 tanda tangan & identitas klinik.

### Prototype v15 Sep 2026 (581.174 bytes · md5 `4d405437dd700f3bd2eb19cbfbb504a8`) — ACUAN BARU
Disimpan: `docs/prototype/CRMBidan_15sep2026.html` (dari Boss, 15 Sep). Menggantikan v14 Sep sebagai acuan audit.
**Yang baru dibanding v14 Sep:**
- **Beranda dirombak**: header "Ringkasan hari ini" + KPI (Kunjungan bulan ini · Kehamilan >38 minggu) · **Aksi Cepat** (4 kartu: Periksa pasien baru · Daftar Pasien · Kantong Persalinan · Pelaporan) · **banner rose "N pasien lewat jadwal kontrol"** → modal **"Lewat Jadwal Kontrol"** (daftar pasien lewat, klik → modal pengingat WA) · section **"Pengingat Kunjungan"** (maks 6, "Lihat semua" → /kunjungan; klik → modal WA) · **"Mendekati Persalinan"** (HPL ≤21 hari, "Lihat semua" → /kantong-bidan). Switcher "Desain Beranda" **diganti toggle "Preview kosong"**.
- **Jadwal Kunjungan**: juga punya banner "N pasien lewat jadwal kontrol" + daftar mendatang; pasien tanpa HPHT diberi label **"HPHT belum diisi"**.
- **Kantong Persalinan**: guidance UX kita **sudah masuk** — label "HPL dari HPHT (otomatis)", **"HPL hasil USG (opsional)"**, **"HPL hasil USG (acuan)"**, penanda **"HPL belum ada"** + **"Lengkapi HPHT →"**, badge **"Lewat HPL N hr"**.
- **Profil Pasien**: **Kelola Kehamilan** (modal form: Status kehamilan Hamil/Tidak hamil; **Kehamilan Berakhir** → Alasan (Keguguran/Lainnya) + Tanggal kejadian + Catatan; Gravida/Para/Abortus; HPHT; HPL dari HPHT), **Riwayat Kehamilan** (daftar outcome), CTA **"+ Kehamilan baru"** untuk kategori tidak-hamil, **"Kode Pasien"** di modal Detail Lengkap Pasien (kandidat No RM klinik), golongan darah, riwayat penyakit keluarga, "Kategori pasien".
- Login: brand **"PrimaCare Untuk Bidan"**, login pakai password + "Lupa Password?" (reset via kode klinik + email).

**Status WA di v15 Sep (tetap, tidak bertambah):** "Kirim WA" 3x · "Kirim ringkasan ke pasien (WA)" 1x · "Buka WhatsApp" 1x · modal edukasi "Nomor WhatsApp tujuan" 1x → total tetap **9 titik**; template pengingat tidak berubah. **Kasir (`oo`) masih belum punya layar struk/invoice & belum ada kirim WA** → permintaan Boss (kirim invoice via WA) tetap jadi scope baru.
**Efek ke Q2:** UX sudah menyediakan **daftar** pasien lewat jadwal (banner + modal) di Beranda & Jadwal — jadi opsi "aksi massal" sebagian sudah terwujud sebagai daftar klik-satu-satu, bukan kirim serentak.

**Jawaban grill:**
- **Q1 = A (Deep link `wa.me`) + teks predefined.** Bidan kirim manual dari WhatsApp-nya, tapi **teks pesan sudah disiapkan sistem** untuk pasien (tanpa mengetik). Konsekuensi yang harus tampung di PRD: sistem **tidak bisa memverifikasi pesan terkirim** (hanya "WA dibuka") → memengaruhi definisi riwayat follow-up (Q3) dan larangan klaim "terkirim otomatis" di AC.
- **Q2 ditutup = A (cukup 4 pintu yang sudah ada).** Setelah v15 Sep: pengingat WA tersedia via (1) Beranda section "Pengingat Kunjungan", (2) banner "N pasien lewat jadwal kontrol" → modal "Lewat Jadwal Kontrol" (Beranda & Jadwal Kunjungan), (3) kartu pasien di halaman Jadwal Kunjungan, (4) kartu jadwal di Profil Pasien — semuanya membuka modal pengingat (`Ri`) yang sama. Fokus PRD = **merapikan perilaku**, bukan bikin entry point baru.
- **Arahan Boss (15 Sep): "kita fokus ke wa reminder dulu ya jo, lanjut dulu"** → grill & PRD difokuskan ke **WA Reminder (pengingat jadwal kontrol via WA)** dulu; bagian ringkasan layanan & edukasi per trimester menyusul di iterasi berikutnya.
- **Klarifikasi "yg pasien baru" = ditunda** oleh Boss (bukan prioritas sekarang).

**Antrian grill lanjutan (fokus WA reminder):** isi & aturan teks (Q3) · bisa-diedit atau read-only (Q4) · pasien tanpa No. HP + fallback nomor anak/orang tua (Q5) · cakupan jenis jadwal + imunisasi anak (Q6) · anti-dobel ingat (riwayat "sudah diingatkan") (Q7) · pengingat manual vs terjadwal-otomatis (Q8) · identitas pengirim/tanda tangan (Q9).

- **Q3 = B + C (template per jenis jadwal × per status urgensi).** Pendekatan yang dipakai: **template modular** — blok **pembuka (per status: lewat / hari ini / akan datang)** + blok **inti (per jenis: ANC · KB · Nifas · Persalinan · Layanan Lain)** + blok **penutup** (tanda tangan). 8 blok teks untuk direview → menghasilkan 15 kombinasi otomatis. Varian "Layanan Lain" pakai kalimat generik ("jadwal kontrol"). Teks dimuat di PRD sebagai tabel jenis × status.
- **Q4 = B (teks bisa diedit bidan di WhatsApp).** Teks predefined tetap terisi otomatis, tapi **tidak ada preview di CRM** — bidan langsung diarahkan ke WhatsApp dan bebas mengubah/menambah sebelum kirim. Konsekuensi yang harus masuk PRD: (a) sistem **tidak mengunci teks** (secara teknis memang tak bisa dicegah dengan `wa.me`), (b) AC tidak boleh menjamin isi pesan persis sesuai template, (c) alur jadi benar-benar 1 klik (buka WA) — bukan 2 langkah.
- **Q5 = nomor HP sudah WAJIB → kasus "tanpa No. HP" dianggap tidak ada.** Verified di prototype v15 Sep: form pasien (`ca`) memvalidasi `if(!o.noHp.trim()){l('No. HP wajib diisi.'); return}` — field bernama **"No. HP / WhatsApp"**. Jadi tombol "Kirim WA" selalu punya nomor tujuan; aturan "tombol hilang" tidak perlu dibuat.
  - **Edge case yang tetap perlu diatur di PRD:** (a) **bayi** yang dibuat otomatis dari Kartu Persalinan — tidak mungkin punya HP sendiri, jadi nomor tujuan harus diwarisi dari **No. HP ibu**; komponen tombol imunisasi di prototype sudah memakai fallback `noHp anak || noHp orang tua`; (b) data lama/hasil impor yang belum punya nomor (kebijakan migrasi).
  - UX nit: pastikan label "No. HP / WhatsApp" diberi **tanda wajib** (validasi sudah ada, tapi penanda visual belum tentu).
- **Q6 = A (pengingat fokus ANC dulu).** Ruang lingkup rilis ini: **pengingat WA untuk jadwal kontrol ANC (pasien hamil)** — sesuai perilaku halaman Jadwal Kunjungan yang sudah memfilter `e.hamil`. **Tidak diperluas** ke imunisasi anak, KB, Nifas, Persalinan, Layanan Lain; modal pengingat yang sudah generik untuk 5 jenis **dibiarkan apa adanya** (tidak diubah, tidak masuk AC rilis ini). Konsekuensi: kartu jadwal **tidak wajib** diberi label jenis jadwal, dan nomor tujuan selalu No. HP pasien (fallback No. HP ibu cukup dicatat sebagai catatan, bukan AC).
- **Q7 = A (tidak dicatat / tanpa log).** Tidak ada penanda "sudah diingatkan" dan tidak ada riwayat follow-up; bidan boleh mengirim pengingat berkali-kali ke pasien yang sama. Konsekuensi: (a) tidak ada mekanisme anti-dobel, (b) catatan epic soal "riwayat follow-up per pasien" **tidak diambil** untuk rilis ini, (c) badge/daftar harian tidak boleh bergantung pada log WA (lihat Q8).
- **Q8 = C (badge angka di menu + daftar harian otomatis).** Menu Jadwal diberi **badge angka** jumlah pasien yang perlu diingatkan, dan Beranda menampilkan **daftar harian otomatis** (hari ini + yang lewat). Karena Q7 = A, angka badge & daftar **diturunkan dari data jadwal** (tanggal kontrol vs hari ini) — **bukan** dari riwayat pengiriman WA. Sebagian sudah ada di v15 Sep (banner merah "N pasien lewat jadwal kontrol" + section "Pengingat Kunjungan"); yang perlu ditambah = **badge di menu**.
- **Q9 = B (tanda tangan nama bidan + nama praktik/klinik).** Penutup pesan memuat nama bidan dan nama praktik/klinik, mis. `— Ratna, PMB Ratna Sejahtera`. Nama klinik diambil dari profil akun/klinik (hasil onboarding), bukan diketik ulang per pesan.

**GRILL WA REMINDER SELESAI (Q1–Q9).** Ringkasan keputusan: **wa.me deep link 1-klik** (Q1) · **cukup 4 pintu pengingat yang ada** (Q2) · **template modular per jenis × status urgensi** (Q3) · **teks bisa diedit di WA, tanpa preview di CRM** (Q4) · **No. HP wajib → nomor selalu tersedia** (Q5) · **cakupan = jadwal kontrol ANC saja** (Q6) · **tanpa log/anti-dobel** (Q7) · **badge menu + daftar harian otomatis** (Q8) · **tanda tangan bidan + klinik** (Q9).

### PRD PTS-376 (WA Reminder) — v1.1 final + inject ke PTS-386 (15 Sep 2026)
File: `features/crm-bidan/prd/prd-PTS-376-wa-reminder-v1.md` (PRD lengkap 11 section) + `prd-PTS-376-wa-reminder-v1-jira.md` (versi inject, 9 section).
**Keputusan copy final (review Boss):** (1) **jam kontrol tidak disertakan** — data jadwal hanya tanggal, jadi rilis ini **tanggal saja** (tanpa tambah input jam); (2) **nama hari ditambahkan** (`Sabtu, 20 Sep 2026`); (3) **sapaan memakai nama tanpa gelar**, pasien **<18 th tanpa sapaan "Ibu"** (pakai "kamu/-mu"); (4) **ajakan konfirmasi kehadiran** ditambahkan di varian "akan datang" & "hari ini" (*"Mohon balas pesan ini untuk konfirmasi kehadiran ya."*); (5) blok inti **dilebur** ke blok isi → teks jadi 2 blok modular; (6) **fallback nama praktik = `Praktik Mandiri Bidan`** (default sistem, bukan "Bidan").
**Struktur PRD:** 9 US · **22 FR** (grup A–E) · 21 butir AC · 14 edge case · 7 NFR · tabel copy 4.1 (5 baris contoh pesan).
**Hasil inject ke PTS-386** (verified dari Jira): **79 node level-1 · 1.475 node total · 22.638 karakter teks · 15 heading · 11 tabel · 0 baris sampah `:--`** · updated 15:05 WIB. Cek konten kunci: US-01..09 ✓ · FR-01 & FR-22 ✓ · nama hari ✓ · ajakan konfirmasi ✓ · sapaan <18 ✓ · fallback praktik ✓ · wa.me ✓ · badge ✓ · "tanpa riwayat" ✓.
**Belum dikerjakan:** tech ticket PF untuk WA Reminder (usulan 1 epic + 5 story) — menunggu approval Boss. Tech ticket PF Kantong Persalinan (1 epic + 5 story) juga masih menunggu.

**Temuan tambahan untuk Q2 (halaman Jadwal Kunjungan):**
- Halaman `/kunjungan` ADA dan sudah berupa daftar pengingat: eyebrow "Pengingat", judul "Jadwal Kunjungan", deskripsi "Pantau kontrol ANC seluruh pasien, diurutkan dari yang paling mendesak", badge urgensi dari `daysToNextVisit` ("Lewat N hr" / "Hari ini" / "N hr lagi"), plus empty state "Belum Punya Jadwal Kontrol".
- Di halaman itu **tidak ada aksi WA sama sekali** (0 kemunculan "Kirim"/"WA") — pengingat WA hanya ada di Profil Pasien (modal `Ri`).
- Ada juga alur terpisah "Jadwal Kunjungan Berikutnya" (atur/tunda jadwal kontrol per pasien, `onLewati`/`onSimpan`). Fokus jadwal saat ini = kontrol ANC.

### Inventaris lengkap titik "Kirim WA" di prototype v14 Sep (audit 15 Sep 2026)
Helper: `Fi(noHp)` normalisasi `08xx` → `62xx` (kosong → null) · `Ii(noHp, teks)` → `https://wa.me/<nomor>?text=…` (nomor kosong → `wa.me/?text=` = pilih kontak manual) · `Li(noHp, teks)` → `window.open`.
Komponen tombol: `Aa({noHp, text, label='Kirim WA'})` — pill sage 12% ukuran xs, **hanya dirender bila `noHp` ada** (jika tidak → tombol hilang tanpa penjelasan).

| # | Layar | Tombol | Teks terkirim | Nomor |
|---|---|---|---|---|
| 1 | Profil → tab **ANC** (baris riwayat) | Kirim WA (pill) | `xa()` — Ringkasan kunjungan ANC: tanggal, TD, BB, LiLA, TFU, DJJ, dst | `pasien.noHp` |
| 2 | Profil → tab **Persalinan** | Kirim WA (pill) | `Ca()` — Ringkasan persalinan (+ komplikasi/catatan) | `pasien.noHp` |
| 3 | Profil → tab **Nifas** | Kirim WA (pill) | `Sa()` — Ringkasan nifas: tipe kunjungan, tanggal bersalin, dst | `pasien.noHp` |
| 4 | Profil → tab **KB** | Kirim WA (pill) | `wa()` — Ringkasan KB: layanan, efek samping, jadwal kontrol/ulang, biaya | `pasien.noHp` |
| 5 | Profil → tab **Layanan Lain (Wellness)** | Kirim WA (pill) | `Ta()` — Ringkasan wellness | `pasien.noHp` |
| 6 | Profil → **Kartu Imunisasi Anak** (per baris imunisasi) | Kirim WA (pill) | `Da(anak, catatan, bidan)` — ringkasan imunisasi | **`anak.noHp || orangtua.noHp`** (fallback!) |
| 7 | **Modal "Jadwal Kunjungan / Pengingat"** (dari Profil, klik kartu jadwal) | Kirim WA (full-width sage) + Periksa | `Halo <nama>, ini pengingat jadwal kontrol pada <tanggal>. Ditunggu kedatangannya ya 🙏 — <bidan>` | `pasien.noHp` |
| 8 | **Layar ringkasan sukses** catat layanan (3 alur: ANC · Nifas · Persalinan — komponen `Ka`) | "Kirim ringkasan ke pasien (WA)" + "Lanjut ke Kasir" | ringkasan yang sama dengan baris riwayat | `pasien.noHp` |
| 9 | **Halaman Edukasi** (`/edukasi`, kartu materi) | Kirim WA (pill) → modal "Kirim ke WhatsApp" | `Edukasi kehamilan — <judul>` + poin `•` + `— <bidan>` | **input manual "Nomor WhatsApp tujuan" (wajib, 08xxxxxxxxxx)** |

**Modal pengingat (Ri) dirender di 3 pintu** — semua memakai modal yang sama, jadi tombol "Kirim WA" sudah tersedia di ketiganya:
1. **Beranda** (komponen `Vi`; section "Pengingat Jadwal Kunjungan" — `Gi({list,onSelect})`, ada tautan "Lihat semua" → `/kunjungan`). Klik item daftar → modal pengingat. Daftar Beranda memuat **semua pasien** yang punya jadwal berikutnya.
2. **Halaman Jadwal Kunjungan `/kunjungan`** (`_o`) — klik kartu pasien → modal pengingat (bukan langsung ke profil).
3. **Profil Pasien** → tab Ringkasan, klik kartu jadwal → modal pengingat.

**Cakupan pengingat = 5 jenis jadwal** (bukan cuma ANC): `jadwalSumber` memilih tanggal terdekat dari `visits.nextVisitDate` (ANC) · `kb.tanggalKontrolBerikutnya` · `nifas.nextVisitDate` · `persalinan.nextVisitDate` · `wellness.nextVisitDate`. Modal juga bisa mengubah tanggal jadwal sesuai `jenis`+`field` sumbernya.

**Gap yang benar-benar ada (hasil koreksi audit):**
- **Halaman Jadwal Kunjungan hanya menampilkan pasien HAMIL** (`filter(e=>e.hamil)`) → jadwal KB/Nifas/Layanan Lain tidak muncul di daftar itu, meski Beranda menampilkannya (daftar Beranda = semua pasien).
- **Jadwal imunisasi anak tidak masuk daftar pengingat** (jadwal imunisasi dasar dibuat otomatis saat Kartu Persalinan disimpan, tapi tidak ada di `jadwalSumber`).
- **Riwayat follow-up (log WA) belum ada** — 0 kemunculan istilah apa pun ("terakhir dihubungi"/"riwayat WA").
- **Kasir (`Ja`) belum punya layar struk/invoice dan belum ada kirim via WA** — hanya select Layanan + Biaya + tombol "Selesai"/"Lewati"; setelah "Selesai" langsung kembali ke `/pasien/<id>?tab=<layanan>`. Keputusan Boss: **kirim invoice via WA perlu ditambahkan** (scope baru di epic ini, sekaligus butuh desain struk/kwitansi).
- Beranda punya 3 varian desain (switcher 1/2/3, tersembunyi di mobile); halaman Jadwal & Profil hanya 1 varian.
