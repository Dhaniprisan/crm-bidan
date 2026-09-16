# PRD — CRM Bidan: Login & Manajemen Data Pasien

| | |
|:--|:--|
| **Feature Name** | CRM Bidan — Login & Manajemen Data Pasien (Login · Daftar Pasien · Cari Pasien Lama · Daftarkan Pasien Baru · Profil Pasien) |
| **Product** | CRM Bidan (berdiri bersama PrimaCare / PrimaKu) |
| **Author** | Product Manager (Dhani Prisantika) |
| **Date** | 14 September 2026 |
| **Version** | 1.0 — Draft |
| **Priority** | High |
| **Status** | Draft — Pending Review |
| **Primary User** | Bidan (praktik mandiri bidan / PMB), akun level superadmin |
| **Module** | CRM Bidan → Login & Data Pasien |
| **Epic** | PTS-367 — [CRM][Bidan] Login dan Daftar Pasien (daftar kontak) |
| **PRD Ticket** | PTS-370 |
| **Kode Referensi** | O3-01 · Level S1 |
| **Desain** | Prototype CRM Bidan v14 Sep 2026 (`docs/prototype/CRMBidan_14sep2026.html`) |

---

## 1. Overview

PRD ini mencakup modul masuk (login) dan pengelolaan data pasien pada aplikasi CRM Bidan — aplikasi mobile-first untuk bidan praktik mandiri (PMB) yang menggantikan pencatatan manual (buku KIA / buku catatan bidan).

Modul ini adalah **fondasi** bagi seluruh epic CRM Bidan lainnya: tanpa data pasien yang benar, fitur Kantong Persalinan, Jadwal & Catatan Kunjungan, WA Follow-up, Kasir, Keuangan, dan Beranda tidak dapat berjalan. Karena itu modul ini menjadi epic pertama yang dieksekusi.

### Scope Boundary

**IN SCOPE**
- Login, "Ingat saya", lupa password (mengikuti mekanisme PrimaCare), dan tautan pendaftaran akun baru ke WhatsApp PrimaCare
- Daftar pasien (daftar seluruh pasien klinik) dengan pencarian, filter, dan pengurutan
- Pencarian pasien lama untuk melanjutkan pencatatan layanan
- Pendaftaran pasien baru (identitas, data obstetri, kontak darurat)
- Profil pasien beserta tab layanan dan tab Ringkasan
- Relasi keluarga (Keluarga Terhubung / "Ibu dari pasien")
- Arsip pasien (menandai pasien non aktif)
- Integrasi data pasien dengan PrimaCare (kode klinik, nomor RM, status pasien)

**OUT OF SCOPE**
- Modul kasir, harga layanan, keuangan, dan laporan (epic terpisah: PTS-602, PTS-603)
- Penjadwalan kunjungan dan pengisian catatan layanan/ANC/persalinan/nifas/KB/imunisasi (PTS-375)
- WA follow-up & materi edukasi (PTS-376)
- Kantong Persalinan (PTS-374) dan Beranda (PTS-605)
- Penggabungan (merge) data pasien duplikat — dilakukan di PrimaCare
- Registrasi akun bidan baru — dilakukan melalui onboarding PrimaCare

---

## 2. Background & Problem Statement

**Kondisi saat ini.** Bidan praktik mandiri mencatat data pasien dan hasil pemeriksaan secara manual — di buku KIA, buku catatan pribadi, atau spreadsheet sederhana. Data pasien (identitas, HPHT, riwayat kehamilan) sering hanya diingat atau tersebar di beberapa catatan.

**Masalah.**
- Data pasien tidak terpusat sehingga sulit mencari pasien lama saat kunjungan ulang
- Jadwal kontrol berikutnya mudah terlewat karena tidak ada pengingat terstruktur
- Riwayat obstetri (kehamilan dan persalinan sebelumnya) tidak tercatat rapi, padahal dibutuhkan untuk menilai risiko
- Bidan tidak punya alat untuk melihat kondisi praktiknya secara ringkas (berapa pasien, siapa yang mendekati persalinan)
- Nomor rekam medis belum seragam sehingga data pasien PMB tidak menyatu dengan rekam medis elektronik

**Mengapa penting.**
- **Kesinambungan asuhan:** pasien yang sama harus dikenali lintas kunjungan, dan datanya tersambung dengan rekam medis elektronik PrimaCare
- **Keselamatan klinis:** riwayat obstetri dan data dasar (HPHT, HPL, G/P/A, golongan darah, riwayat alergi) memengaruhi keputusan asuhan dan rujukan
- **Efisiensi bidan:** pencarian pasien lama dan pendaftaran pasien baru harus bisa diselesaikan cepat, dari ponsel, saat bidan melayani pasien
- **Kepatuhan pencatatan:** data pasien dan riwayat pemeriksaan perlu tersimpan dalam satu sistem agar bisa ditelusuri dan dilaporkan

---

## 3. Before vs After

| Current State (Before) | Enhanced State (After) |
|:--|:--|
| Data pasien dicatat manual di buku KIA / buku pribadi bidan | Data pasien tersimpan di aplikasi, dapat dicari kembali kapan saja |
| Mencari pasien lama harus membuka-buka catatan kertas | Pencarian pasien berdasarkan nama dan/atau tanggal lahir, hasil langsung tampil |
| Riwayat kehamilan & persalinan sebelumnya hanya diingat | Riwayat obstetri tersimpan per pasien dan terlihat di profil pasien |
| Tidak ada nomor rekam medis yang seragam | Pasien langsung mendapat nomor RM sesuai standar PrimaCare |
| Jadwal kontrol dicatat di kertas, mudah terlewat | Jadwal kontrol berikutnya tersimpan per pasien (dasar fitur Jadwal Kunjungan) |
| Data pasien PMB terpisah dari rekam medis elektronik | Data pasien & riwayat pemeriksaan menyatu dengan rekam medis PrimaCare |
| Tidak ada kendali akses/data pasien tidak aktif | Pasien yang sudah tidak ditangani dapat diarsipkan (status non aktif) oleh superadmin |
| Bidan membuat akun sendiri / tidak ada jalur onboarding | Akun dibuat melalui onboarding PrimaCare (superadmin), pendaftaran dari aplikasi diarahkan ke WhatsApp PrimaCare |
| Tidak ada gambaran jumlah pasien yang dikelola | Daftar pasien menampilkan jumlah pasien aktif pada klinik tersebut |

---

## 4. Integrasi dengan PrimaCare

CRM Bidan **tidak memiliki master user maupun master pasien sendiri**. Seluruh identitas mengikuti PrimaCare.

1. **Akun & akses** — CRM Bidan memakai akun PrimaCare yang sama. Akun dibuat melalui admin/onboarding PrimaCare dan diperlakukan sebagai **superadmin**, sehingga dapat mengakses CRM Bidan dan PrimaCare.
2. **Klinik** — kode klinik mengikuti PrimaCare dan bersifat **read only** pada form login.
3. **Pasien** — identitas pasien dan **nomor RM mengikuti standar PrimaCare**. Penggabungan pasien duplikat dilakukan di PrimaCare.
4. **Rekam medis** — catatan kunjungan dari CRM Bidan tampil pada **riwayat pemeriksaan (list RM) sebagai catatan**, dan **tidak masuk ke daftar pendaftaran pasien** PrimaCare.
5. **Field baru (data obstetri)** — HPHT, HPL, status kehamilan, G/P/A, tinggi badan, TBJ, hasil pemeriksaan obstetrik, dan catatan diagnosa **disimpan pada modul terpisah di sisi CRM Bidan** yang mereferensikan pasien PrimaCare, dan **belum ditampilkan** pada form pemeriksaan PrimaCare.
6. **Status pasien** — status non aktif (hasil arsip) **juga terlihat di PrimaCare**.

Konsekuensi teknis: CRM Bidan memerlukan koneksi ke PrimaCare untuk membuat/membaca data pasien. Bila layanan PrimaCare tidak tersedia, data yang sedang diisi **disimpan sebagai draft di server CRM Bidan** (tanpa nomor RM) dan disinkronkan otomatis (retry) setelah layanan kembali normal.

## 5. User Stories

| ID | User Story | Ringkasan Acceptance Criteria | Priority |
|:--|:--|:--|:--|
| US-01 | Sebagai bidan, saya ingin masuk ke aplikasi menggunakan akun saya agar dapat mulai bekerja | Given bidan membuka aplikasi, When memasukkan kode klinik, email/No. HP, dan password yang benar, Then bidan masuk ke Beranda dan sesi tersimpan bila "Ingat saya" dicentang | Must Have |
| US-02 | Sebagai bidan, saya ingin mengatur ulang password saya agar tetap bisa mengakses akun | Given bidan menekan "Lupa password?", When mengikuti instruksi reset, Then bidan menerima instruksi reset melalui mekanisme PrimaCare dan dapat masuk kembali dengan password baru | Must Have |
| US-03 | Sebagai bidan baru, saya ingin tahu cara mendapatkan akun agar dapat mulai menggunakan aplikasi | Given bidan menekan "Belum punya akun? Daftar", When tautan dibuka, Then aplikasi membuka WhatsApp PrimaCare dengan pesan pengajuan kemitraan | Should Have |
| US-04 | Sebagai bidan, saya ingin melihat seluruh pasien klinik saya agar dapat memilih pasien yang akan dilayani | Given bidan membuka Daftar Pasien, When halaman dimuat, Then tampil seluruh pasien aktif klinik beserta jumlahnya, diurutkan sesuai pilihan | Must Have |
| US-05 | Sebagai bidan, saya ingin mencari dan menyaring pasien agar menemukan pasien dengan cepat | Given daftar pasien terbuka, When bidan mengetik nama atau memilih filter/urutan, Then daftar menampilkan hasil yang sesuai tanpa memuat ulang halaman | Must Have |
| US-06 | Sebagai bidan, saya ingin mencari pasien lama agar bisa langsung mencatat layanan berikutnya | Given bidan membuka Cari Pasien Lama, When memasukkan nama atau tanggal lahir, Then hasil pencarian tampil dan dapat dibuka untuk melanjutkan pencatatan layanan | Must Have |
| US-07 | Sebagai bidan, saya ingin mendaftarkan pasien baru agar datanya tercatat sejak kunjungan pertama | Given bidan membuka form pendaftaran, When mengisi field wajib dan menyimpan, Then pasien tersimpan dengan nomor RM standar PrimaCare dan muncul di Daftar Pasien | Must Have |
| US-08 | Sebagai bidan, saya ingin melihat profil pasien agar riwayat dan kondisi pasien mudah dipantau | Given bidan memilih pasien dari daftar, When profil terbuka, Then tampil identitas ringkas, usia kehamilan/HPL, jadwal kontrol berikutnya, dan tab layanan sesuai kategori pasien | Must Have |
| US-09 | Sebagai bidan, saya ingin menghubungkan pasien dengan keluarganya agar data ibu dan anak saling terhubung | Given bidan membuka tab relasi keluarga, When memilih "Cari Pasien Lain" atau "Diri Sendiri", Then pasien terhubung sebagai anggota keluarga dan dapat dibuka kartu imunisasinya | Should Have |
| US-10 | Sebagai superadmin, saya ingin mengarsipkan pasien yang sudah tidak ditangani agar daftar pasien tetap relevan | Given bidan membuka profil pasien, When memilih "Arsipkan / Lepas dari daftar pasien" dan mengonfirmasi, Then status pasien menjadi non aktif dan tampil di PrimaCare | Must Have |
| US-11 | Sebagai bidan, saya ingin data yang saya isi tidak hilang saat layanan PrimaCare bermasalah | Given data pasien diisi saat layanan PrimaCare tidak tersedia, When bidan menyimpan, Then data tersimpan sebagai draft di server CRM Bidan dan disinkronkan otomatis ketika layanan kembali normal | Must Have |

**Prioritas (MoSCoW):** Must Have = wajib pada rilis pertama · Should Have = penting, dapat menyusul · Could Have = opsional · Won't Have = tidak pada rilis ini

---

## 6. Functional Requirements

### Login & Akun

| ID | Requirement | Priority | MoSCoW |
|:--|:--|:--|:--|
| FR-01 | Form login menampilkan field Kode klinik, Email / No. HP, dan Password | High | Must |
| FR-02 | Field Kode klinik bersifat **read only** dan otomatis terisi dari klinik pada akun PrimaCare bidan | High | Must |
| FR-03 | Sistem menyediakan checkbox "Ingat saya" yang menyimpan sesi login pada perangkat | Medium | Should |
| FR-04 | Sistem menampilkan pesan kesalahan yang spesifik saat kredensial salah (mis. "Email/No. HP atau password tidak sesuai") tanpa mengungkap data akun | High | Must |
| FR-05 | Sistem menyediakan tautan "Lupa password?" yang mengarahkan bidan ke mekanisme reset password PrimaCare | High | Must |
| FR-06 | Sistem menyediakan tautan "Belum punya akun? Daftar" yang membuka WhatsApp PrimaCare (https://api.whatsapp.com/send/?phone=6285124402922&text=…) di aplikasi luar | Medium | Should |
| FR-07 | Sistem menolak akses seluruh halaman CRM Bidan bila bidan belum login, dan mengarahkan kembali ke halaman login | High | Must |
| FR-08 | Sistem menyediakan aksi keluar (logout) pada halaman "Lainnya" | High | Must |

### Daftar Pasien

| ID | Requirement | Priority | MoSCoW |
|:--|:--|:--|:--|
| FR-09 | Daftar Pasien menampilkan **seluruh pasien aktif pada klinik** yang sama dengan akun bidan | High | Must |
| FR-10 | Header Daftar Pasien menampilkan nama bidan, nama praktik/klinik, dan jumlah pasien terdaftar (mis. "14 pasien terdaftar") | Medium | Should |
| FR-11 | Sistem menyediakan pencarian berdasarkan nama pasien dengan placeholder "Cari nama pasien…" | High | Must |
| FR-12 | Sistem menyediakan filter: Semua, Hamil, Tidak Hamil, Anak, Laki-laki | High | Must |
| FR-13 | Sistem menyediakan filter status pasien: Aktif dan Non aktif | High | Must |
| FR-14 | Sistem menyediakan pengurutan: Nama, HPL terdekat, Usia | Medium | Should |
| FR-15 | Setiap kartu pasien menampilkan nama, usia (X th Y bln), HPL (atau "—" bila tidak ada), dan badge status (Ibu Hamil / Anak) | High | Must |
| FR-16 | Daftar Pasien menyediakan CTA "+ Daftarkan pasien" menuju form pendaftaran pasien baru | High | Must |
| FR-17 | Sistem menampilkan kondisi kosong yang informatif bila daftar pasien belum ada atau hasil pencarian tidak ditemukan | Medium | Should |

### Cari Pasien Lama

| ID | Requirement | Priority | MoSCoW |
|:--|:--|:--|:--|
| FR-18 | Halaman Cari Pasien menampilkan keterangan "Cari lalu lanjut catat layanan untuk pasien yang sudah terdaftar." | Low | Could |
| FR-19 | Sistem mencari pasien berdasarkan nama dan/atau tanggal lahir (tanggal lahir opsional) | High | Must |
| FR-20 | Hasil pencarian dapat dibuka untuk melanjutkan pencatatan layanan pasien tersebut | High | Must |
| FR-21 | Sistem menampilkan kondisi kosong pada dua keadaan: sebelum pencarian ("Ketik nama atau pilih tanggal lahir untuk mulai mencari.") dan hasil kosong ("Tidak ada pasien dengan nama itu.") | Medium | Should |

### Pendaftaran Pasien Baru

| ID | Requirement | Priority | MoSCoW |
|:--|:--|:--|:--|
| FR-22 | Field wajib pada form: Nama lengkap, Jenis kelamin, Status kehamilan, HPHT | High | Must |
| FR-23 | Field opsional: Tanggal lahir (usia dihitung otomatis), No. HP/WhatsApp, NIK, Alamat, Provinsi, Golongan darah, Riwayat alergi, Riwayat penyakit, Gravida (G), Para (P), Abortus (A), Kontak darurat (hubungan, nama, No. HP) | Medium | Should |
| FR-24 | Sistem menghitung dan menampilkan HPL secara otomatis dari HPHT; field HPL tidak diisi manual | High | Must |
| FR-25 | Sistem menyediakan field "Ibu dari pasien (opsional)" untuk menghubungkan pasien anak dengan data ibunya | Medium | Should |
| FR-26 | Sistem menampilkan riwayat obstetri (riwayat kehamilan & persalinan terdahulu, riwayat KB, riwayat pernikahan) sebagai daftar berulang yang dapat ditambah dan dihapus | Medium | Should |
| FR-27 | Saat menyimpan, sistem mengambil nomor RM sesuai standar PrimaCare dan menyimpan identitas pasien pada master pasien PrimaCare; data obstetri disimpan pada modul terpisah di sisi CRM Bidan | High | Must |
| FR-28 | Sistem memvalidasi duplikasi dasar (nama + tanggal lahir atau NIK) dan menampilkan peringatan sebelum menyimpan; penggabungan pasien dilakukan di PrimaCare | Medium | Should |
| FR-29 | Bila layanan PrimaCare tidak tersedia saat menyimpan, sistem menyimpan data sebagai draft (tanpa nomor RM) dan menandainya sebagai "menunggu sinkronisasi" | High | Must |
| FR-30 | Sistem melakukan percobaan sinkronisasi ulang secara otomatis untuk draft yang belum tersinkron, dan menampilkan statusnya di UI | High | Must |

### Profil Pasien

| ID | Requirement | Priority | MoSCoW |
|:--|:--|:--|:--|
| FR-31 | Profil pasien menampilkan header: nama pasien, aksi edit, identitas ringkas (G/P/A · golongan darah · alamat), dan tautan "Lihat detail lengkap" | High | Must |
| FR-32 | Profil menampilkan tab sesuai kategori pasien: Ringkasan, ANC, Persalinan, Nifas, KB, Imunisasi, Layanan Lain | High | Must |
| FR-33 | Tab Ringkasan menampilkan usia kehamilan (X mgu Y hr) + badge trimester + progress bar (0 / 12 / 27 / 40 mgu) untuk pasien hamil | High | Must |
| FR-34 | Tab Ringkasan menampilkan HPHT, HPL, dan hitungan hari menuju HPL | High | Must |
| FR-35 | Tab Ringkasan menampilkan jadwal kunjungan berikutnya, kontrol terakhir, dan badge jarak hari | High | Must |
| FR-36 | Tab Ringkasan menampilkan kontak pasien (No. HP, tanggal lahir, kontak darurat) dan riwayat alergi/penyakit bila diisi | Medium | Should |
| FR-37 | Profil menyediakan bagian relasi keluarga: "Anak & Keluarga Terhubung", aksi "Cari Pasien Lain" dan "Diri Sendiri", serta tautan ke Kartu Imunisasi anak | Medium | Should |
| FR-38 | Profil menyediakan aksi "Arsipkan / Lepas dari daftar pasien" yang mengubah status pasien menjadi **non aktif**, hanya tersedia untuk akun **superadmin**, dengan dialog konfirmasi | High | Must |
| FR-39 | Status non aktif membuat pasien tidak tampil pada daftar pasien default, namun tetap dapat ditemukan melalui filter status | High | Must |
| FR-40 | Perubahan status pasien (non aktif) ikut tersinkron dan **terlihat di PrimaCare** | High | Must |

## 7. Acceptance Criteria

### US-01 — Login
1. **Given** bidan belum login, **When** membuka aplikasi, **Then** sistem menampilkan halaman login dengan field Kode klinik, Email / No. HP, Password, checkbox "Ingat saya", tautan "Lupa password?", "Belum punya akun? Daftar", dan tombol Masuk.
2. **Given** halaman login terbuka, **When** bidang Kode klinik ditampilkan, **Then** nilainya terisi otomatis dari klinik pada akun PrimaCare dan tidak dapat diubah (read only).
3. **Given** bidan mengisi Email/No. HP dan Password yang benar, **When** menekan Masuk, **Then** sistem mengarahkan ke Beranda dan menampilkan nama bidan pada header.
4. **Given** bidan mencentang "Ingat saya" sebelum masuk, **When** login berhasil, **Then** sesi tersimpan sehingga bidan tidak perlu login ulang saat membuka aplikasi kembali.
5. **Given** bidan memasukkan password salah, **When** menekan Masuk, **Then** sistem menampilkan pesan "Email/No. HP atau password tidak sesuai" dan field password dikosongkan.
6. **Given** bidan belum login, **When** membuka tautan halaman dalam aplikasi secara langsung, **Then** sistem mengalihkan ke halaman login.

### US-02 — Lupa Password
1. **Given** halaman login terbuka, **When** bidan menekan "Lupa password?", **Then** sistem mengarahkan ke alur reset password PrimaCare (bukan form reset terpisah di CRM Bidan).
2. **Given** bidan menyelesaikan alur reset, **When** kembali ke aplikasi, **Then** bidan dapat masuk menggunakan password baru.

### US-03 — Tautan Pendaftaran Akun
1. **Given** halaman login terbuka, **When** bidan menekan "Belum punya akun? Daftar", **Then** sistem membuka WhatsApp PrimaCare (nomor 6285124402922) dengan pesan pengajuan kemitraan yang sudah terisi, di aplikasi WhatsApp.
2. **Given** bidan kembali dari WhatsApp, **When** kembali ke aplikasi, **Then** halaman login tetap pada kondisi semula.

### US-04 — Daftar Pasien
1. **Given** bidan sudah login, **When** membuka menu Pasien, **Then** sistem menampilkan seluruh **pasien aktif** pada klinik yang sama beserta jumlah pasien terdaftar di header.
2. **Given** daftar pasien terbuka, **When** halaman selesai dimuat, **Then** setiap kartu menampilkan nama, usia (X th Y bln), HPL (atau "—"), dan badge status (Ibu Hamil / Anak).
3. **Given** klinik belum memiliki pasien, **When** halaman dimuat, **Then** sistem menampilkan keadaan kosong beserta CTA "+ Daftarkan pasien".
4. **Given** daftar pasien tampil, **When** bidan menekan kartu pasien, **Then** sistem membuka Profil Pasien.

### US-05 — Pencarian, Filter, Urutan
1. **Given** daftar pasien terbuka, **When** bidan mengetik minimal 1 karakter pada "Cari nama pasien…", **Then** daftar tersaring sesuai nama tanpa memuat ulang halaman.
2. **Given** pencarian tidak menemukan hasil, **When** hasil kosong, **Then** sistem menampilkan pesan bahwa pasien tidak ditemukan dan tetap menampilkan CTA pendaftaran pasien.
3. **Given** daftar pasien terbuka, **When** bidan memilih filter Hamil, **Then** hanya pasien berstatus hamil yang ditampilkan; filter Anak menampilkan hanya pasien kategori anak.
4. **Given** daftar pasien terbuka, **When** bidan memilih filter status Non aktif, **Then** sistem menampilkan pasien berstatus non aktif.
5. **Given** daftar pasien terbuka, **When** bidan memilih urutan "HPL terdekat", **Then** pasien hamil diurutkan dari HPL paling dekat; urutan "Usia" mengurutkan dari usia termuda.

### US-06 — Cari Pasien Lama
1. **Given** bidan membuka Cari Pasien Lama, **When** halaman dimuat, **Then** tampil field pencarian nama dan pemilih tanggal lahir (opsional) beserta keterangan "Cari lalu lanjut catat layanan untuk pasien yang sudah terdaftar."
2. **Given** bidan mengetikkan nama pasien, **When** hasil ditemukan, **Then** daftar hasil menampilkan pasien beserta identitas pembeda (usia / tanggal lahir).
3. **Given** hasil pencarian tampil, **When** bidan memilih salah satu hasil, **Then** sistem membuka profil pasien untuk melanjutkan pencatatan layanan.
4. **Given** pencarian dijalankan, **When** tidak ada pasien yang cocok, **Then** sistem menampilkan "Tidak ada pasien dengan nama itu."

### US-07 — Daftarkan Pasien Baru
1. **Given** bidan membuka form pendaftaran, **When** halaman dimuat, **Then** tampil field wajib (Nama lengkap, Jenis kelamin, Status kehamilan, HPHT) dan field opsional (tanggal lahir, No. HP/WhatsApp, serta kelompok Identitas, Riwayat Kesehatan, dan Kontak Darurat).
2. **Given** bidan mengisi HPHT, **When** field HPHT terisi, **Then** sistem menghitung dan menampilkan HPL secara otomatis (field HPL tidak dapat diisi manual).
3. **Given** bidan memilih Status kehamilan "Hamil", **When** form ditampilkan, **Then** field HPHT menjadi wajib; bila Status kehamilan "Tidak Hamil", field HPHT tidak wajib.
4. **Given** bidan memilih Jenis kelamin dan mengisi tanggal lahir, **When** tanggal lahir terisi, **Then** sistem menampilkan usia pasien secara otomatis.
5. **Given** bidan mengisi Nama lengkap, Jenis kelamin, Status kehamilan, dan HPHT, **When** menekan Simpan dan layanan PrimaCare tersedia, **Then** sistem menyimpan pasien dengan nomor RM standar PrimaCare dan menampilkan pasien pada Daftar Pasien.
6. **Given** bidan belum mengisi salah satu field wajib, **When** menekan Simpan, **Then** sistem menandai field yang belum lengkap dan tidak menyimpan data.
7. **Given** nama dan tanggal lahir (atau NIK) pasien sama dengan pasien yang sudah ada, **When** bidan menekan Simpan, **Then** sistem menampilkan peringatan kemungkinan pasien ganda dengan pilihan melanjutkan atau membatalkan; penggabungan data dilakukan di PrimaCare.
8. **Given** bidan mengisi data pada kelompok opsional termasuk riwayat obstetri, **When** menekan tombol tambah riwayat, **Then** sistem menambahkan satu kartu riwayat baru yang dapat diubah atau dihapus.
9. **Given** bidan menekan Batal, **When** ada data yang sudah diisi, **Then** sistem meminta konfirmasi sebelum membuang data.

### US-08 — Profil Pasien
1. **Given** bidan membuka profil pasien hamil, **When** halaman dimuat, **Then** tampil nama pasien, aksi edit, identitas ringkas (G/P/A · golongan darah · alamat), dan tab Ringkasan, ANC, Persalinan, Nifas, KB, Imunisasi, Layanan Lain.
2. **Given** pasien hamil dengan HPHT terisi, **When** tab Ringkasan dibuka, **Then** tampil usia kehamilan (X mgu Y hr), badge trimester, progress bar kehamilan (skala 0 / 12 / 27 / 40 mgu), HPHT, HPL, dan hitungan hari menuju HPL.
3. **Given** pasien memiliki jadwal kontrol berikutnya, **When** tab Ringkasan dibuka, **Then** tampil tanggal kontrol terakhir, jadwal kontrol berikutnya, dan badge jarak hari.
4. **Given** pasien kategori anak, **When** profil dibuka, **Then** tab yang tampil hanya Ringkasan, Imunisasi, dan Layanan Lain (tanpa ANC/Persalinan/Nifas/KB).
5. **Given** pasien memiliki pasangan/keluarga terhubung, **When** bagian keluarga dibuka, **Then** sistem menampilkan daftar anggota keluarga terhubung dan dapat membuka Kartu Imunisasi anggota keluarga tersebut.

### US-09 — Relasi Keluarga
1. **Given** profil pasien terbuka, **When** bidan menekan "Cari Pasien Lain", **Then** sistem menampilkan pencarian pasien lain untuk dihubungkan sebagai anggota keluarga.
2. **Given** pencarian keluarga dijalankan, **When** bidan memilih pasien, **Then** pasien tersebut terhubung dan tampil pada bagian keluarga pasien.
3. **Given** pasien anak belum terhubung ke ibunya, **When** bidan mengisi field "Ibu dari pasien", **Then** relasi ibu–anak tersimpan dan tampil pada kedua profil.

### US-10 — Arsip Pasien (Non Aktif)
1. **Given** akun bidan berlevel superadmin, **When** membuka profil pasien, **Then** aksi "Arsipkan / Lepas dari daftar pasien" tersedia.
2. **Given** bidan menekan aksi arsip, **When** dialog konfirmasi muncul, **Then** sistem menjelaskan bahwa pasien akan berstatus non aktif (data tidak dihapus) dan meminta konfirmasi.
3. **Given** bidan mengonfirmasi, **When** proses berhasil, **Then** status pasien menjadi non aktif, pasien tidak tampil pada daftar pasien default, dan status tersebut tersinkron sehingga **terlihat di PrimaCare**.
4. **Given** pasien berstatus non aktif, **When** bidan memilih filter status "Non aktif", **Then** pasien tersebut tampil pada daftar.
5. **Given** proses arsip gagal karena layanan PrimaCare tidak tersedia, **When** bidan mengonfirmasi, **Then** sistem menampilkan pesan kegagalan dan status pasien tidak berubah.

### US-11 — Draft & Sinkronisasi
1. **Given** bidan mengisi form pendaftaran pasien, **When** sistem tidak dapat menghubungi layanan PrimaCare saat menyimpan, **Then** sistem menyimpan data sebagai draft di server CRM Bidan tanpa nomor RM dan menampilkan status "menunggu sinkronisasi".
2. **Given** terdapat draft yang belum tersinkron, **When** layanan PrimaCare kembali tersedia, **Then** sistem melakukan percobaan sinkronisasi otomatis tanpa perlu tindakan bidan.
3. **Given** draft berhasil tersinkron, **When** proses selesai, **Then** pasien tampil pada Daftar Pasien dengan nomor RM dan status sinkronisasi berubah menjadi berhasil.
4. **Given** draft gagal tersinkron berulang kali, **When** melewati jumlah percobaan yang ditentukan, **Then** sistem menandai draft sebagai perlu tindakan dan menampilkan informasi kepada bidan.

## 8. State & Edge Cases

| Kondisi | Perilaku yang diharapkan |
|:--|:--|
| Daftar pasien masih kosong (klinik baru) | Tampilkan keadaan kosong + penjelasan singkat + CTA "+ Daftarkan pasien" |
| Pencarian tidak menemukan hasil | Tampilkan "Pasien tidak ditemukan" + tetap sediakan CTA pendaftaran pasien baru |
| Pasien berstatus non aktif | Tidak tampil pada daftar default; hanya muncul bila filter status "Non aktif" dipilih |
| Pasien hamil tanpa HPHT | HPL dan usia kehamilan tidak dihitung; tampilkan "—" pada kartu pasien dan minta kelengkapan data pada profil |
| Pasien tanpa nomor RM (draft belum tersinkron) | Tampilkan penanda "menunggu sinkronisasi"; nomor RM tampil setelah sinkron berhasil |
| Layanan PrimaCare tidak tersedia saat menyimpan pasien | Simpan sebagai draft di server CRM Bidan; tampilkan notifikasi bahwa data akan disinkronkan otomatis |
| Layanan PrimaCare tidak tersedia saat mengarsipkan pasien | Batalkan perubahan status; tampilkan pesan bahwa status belum berubah karena layanan tidak tersedia |
| Kredensial login salah | Pesan kesalahan spesifik; field password dikosongkan; tidak mengungkap apakah akun terdaftar |
| Sesi habis / token kedaluwarsa | Arahkan ke halaman login dan tampilkan pesan sesi berakhir |
| Pasien anak tanpa relasi ke ibu | Izinkan penyimpanan; tampilkan ajakan melengkapi relasi ibu (opsional) pada profil |
| Data pasien sudah ada (nama + tanggal lahir atau NIK sama) | Tampilkan peringatan kemungkinan pasien ganda sebelum menyimpan; penggabungan dilakukan di PrimaCare |
| Nomor HP pasien kosong | Pendaftaran tetap dapat disimpan; fitur berbasis WhatsApp tidak ditampilkan untuk pasien tersebut |

---

## 9. Kepemilikan Data & Penempatan Field

| Data | Sumber / Penempatan | Catatan |
|:--|:--|:--|
| Kode klinik | PrimaCare (read only) | Ditampilkan pada form login, tidak dapat diubah dari CRM Bidan |
| Akun & hak akses bidan | PrimaCare (superadmin) | Satu akun untuk CRM Bidan & PrimaCare |
| Identitas pasien (nama, jenis kelamin, tanggal lahir, NIK, tempat lahir, No. HP, alamat) | **Master pasien PrimaCare** | CRM Bidan membaca & menulis melalui layanan PrimaCare |
| Nomor RM | **Standar PrimaCare** | Dibuat saat pasien disimpan |
| Status pasien (aktif / non aktif) | PrimaCare | Hasil aksi arsip juga terlihat di PrimaCare |
| Data obstetri & klinis (HPHT, HPL, status kehamilan, G, P, A, TB, TBJ, hasil pemeriksaan obstetrik, catatan diagnosa) | **Modul terpisah di sisi CRM Bidan** yang mereferensikan pasien PrimaCare | Disimpan, namun belum ditampilkan pada form pemeriksaan PrimaCare |
| Relasi keluarga ("Ibu dari pasien", Keluarga Terhubung) | Sisi CRM Bidan | Dikelola pada modul relasi CRM Bidan |
| Catatan kunjungan (epic PTS-375) | Sisi CRM Bidan → tampil pada riwayat pemeriksaan (list RM) PrimaCare sebagai catatan | Tidak membuat record pendaftaran pasien di PrimaCare |
| Riwayat alergi & riwayat penyakit | Sisi CRM Bidan (data klinis) | Belum ditampilkan di PrimaCare |

---

## 10. Non-Functional Requirements

| ID | Requirement | MoSCoW |
|:--|:--|:--|
| NFR-01 | Aplikasi membutuhkan koneksi internet (harus online) untuk membaca & menyimpan data pasien; tidak ada mode offline | Must |
| NFR-02 | Semua halaman CRM Bidan hanya dapat diakses setelah login; permintaan data selalu menyertakan kewenangan akun | Must |
| NFR-03 | Data pasien yang ditampilkan hanya pasien pada klinik yang sama dengan akun bidan | Must |
| NFR-04 | Aplikasi dioptimalkan untuk perangkat mobile (target utama) dan tetap dapat digunakan pada layar lebih besar | Must |
| NFR-05 | Waktu muat daftar pasien dan hasil pencarian ditampilkan tanpa memblokir interaksi (maksimal 3 detik pada koneksi normal) | Should |
| NFR-06 | Aksi arsip pasien dicatat sebagai jejak audit (siapa, kapan, pasien mana) | Should |
| NFR-07 | Data yang belum tersinkron tidak boleh hilang bila aplikasi ditutup atau di-refresh | Must |

---

## 11. Dependencies & Open Items

**Dependencies (tim PrimaCare)**
1. Detail teknis skema PrimaCare: nama tabel/kolom, endpoint API pembuatan pasien, format & sumber nomor RM, serta endpoint pembaruan status pasien.
2. Endpoint penulisan catatan ke riwayat pemeriksaan (list RM) pasien.
3. Mekanisme autentikasi & reset password PrimaCare yang akan dipakai CRM Bidan.
4. Konfirmasi penanganan pasien duplikat (deteksi & penggabungan) di sisi PrimaCare.

**Open Items (UX)**
1. Menambahkan **filter status pasien (Aktif / Non aktif)** pada daftar pasien — melengkapi filter yang sudah ada (Semua/Hamil/Tidak Hamil/Anak/Laki-laki).
2. Mengganti istilah **"pasien binaan"** pada deskripsi halaman daftar pasien, karena bidan mengelola **seluruh pasien klinik**.
3. Menghapus layar reset password tersendiri di CRM Bidan (digantikan mekanisme PrimaCare) dan mengganti tautan "Belum punya akun? Daftar" menjadi tautan WhatsApp PrimaCare.

---

## 12. Lampiran

| Dokumen | Lokasi |
|:--|:--|
| Prototype CRM Bidan v14 Sep 2026 | `docs/prototype/CRMBidan_14sep2026.html` |
| Feedback bidan (form pendaftaran & ANC) | `docs/feedback-bidan-form-pendaftaran-anc.md` + PDF `docs/feedback-bidan-untuk-ux.pdf` |
| Guidance UX field baru | `docs/guidance-ux-field-baru-bidan.pdf` |
| Catatan integrasi PrimaCare | `docs/crm-bidan-integrasi-primacare.md` |
| Himpunan keputusan & hasil grill | `features/crm-bidan/context/CONTEXT.md` |
| Interview bidan | `states/bidan-crm-interview-guide.md` (epic PTS-588) |

---

## 13. Riwayat Revisi

| Versi | Tanggal | Perubahan | Oleh |
|:--|:--|:--|:--|
| 1.0 | 14 Sep 2026 | Draft awal — disusun dari prototype v14 Sep, feedback bidan, dan keputusan integrasi PrimaCare | Product Manager |



