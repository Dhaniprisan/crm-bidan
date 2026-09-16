# PRD — CRM Bidan: Kantong Persalinan (Perkiraan Kelahiran)

## 1. Overview & Scope Boundary

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

## 2. Background & Problem Statement

**Kondisi saat ini.** Bidan praktik mandiri memperkirakan waktu persalinan pasien dengan menghitung manual dari HPHT (roda kehamilan atau kalkulator), lalu mencatatnya di buku KIA maupun buku catatan pribadi. Tidak ada satu tempat untuk melihat sebaran perkiraan kelahiran pada bulan-bulan mendatang.

**Masalah.**
1. Bidan tidak punya gambaran beban kerja per bulan — berapa pasien yang diperkirakan lahiran bulan ini, bulan depan, dan seterusnya.
2. Persiapan persalinan (jadwal jaga, perlengkapan, pendampingan, transportasi) dilakukan mendadak karena tidak ada daftar yang bisa dilihat lebih awal.
3. Pasien yang HPL-nya sudah lewat namun belum ada catatan persalinan mudah terlewat dari pantauan.
4. Pasien hamil yang data HPHT-nya belum lengkap "menghilang" dari perkiraan tanpa disadari, sehingga tidak ikut direncanakan.

**Mengapa penting.** Kantong Persalinan memberi bidan satu kendali perencanaan: sebaran persalinan per bulan, daftar nama yang bisa langsung dibuka, serta penanda untuk pasien yang perlu perhatian. Data ini juga menjaga kesinambungan asuhan karena pasien yang sama tetap dapat ditelusuri lintas bulan.

## 3. User Stories

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

## 4. Functional Requirements

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
## 5. Acceptance Criteria

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

## 6. State & Edge Cases

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

## 7. Kepemilikan Data & Penempatan Field

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

## 8. Non-Functional Requirements

| ID | Requirement | MoSCoW |
|:--|:--|:--|
| NFR-01 | Aplikasi memerlukan koneksi internet (tidak ada mode offline); layar Kantong bersifat baca data | Must |
| NFR-02 | Seluruh data hanya dapat diakses setelah bidan login, dan dibatasi pada pasien dari klinik akun tersebut | Must |
| NFR-03 | Tampilan mobile-first; grid 12 bulan tetap terbaca pada layar kecil (3 kolom) | Must |
| NFR-04 | Perhitungan usia kehamilan, trimester, dan hitungan hari ke HPL mengikuti tanggal hari ini, tanpa cache nilai lama | Must |
| NFR-05 | Tidak ada aksi destruktif pada layar Kantong Persalinan (tanpa hapus/arsip/tandai bersalin) | Must |
| NFR-06 | Perubahan status kehamilan akibat pencatatan persalinan harus tercatat (jejak audit) dan tidak boleh hilang saat sinkronisasi gagal | Should |
| NFR-07 | Riwayat jumlah pasien per bulan tetap akurat untuk tahun-tahun sebelumnya (data historis tidak dihitung ulang dari status sekarang) | Should |
