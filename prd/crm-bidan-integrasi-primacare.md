# Catatan Integrasi — CRM Bidan ↔ PrimaCare (Skema Pondasi Bersama)

**Ditetapkan:** Boss (Dhani), 14 Sep 2026 · **Status:** keputusan utama sudah final
**Constraint:** CRM Bidan secara skema pondasi berdiri bersama PrimaCare → **skema user & data pasien harus match PrimaCare**.
**Catatan ini sudah ditanam di deskripsi 9 epic:** PTS-367 · 374 · 375 · 376 · 395 · 602 · 603 · 604 · 605

---

## 1. Keputusan (jawaban Boss, 14 Sep 2026)

| # | Pertanyaan | Keputusan |
|:--|:--|:--|
| 1 | Auth — akun sama atau terpisah? | **Akun PrimaCare yang sama.** Saat akun dibuat melalui **admin/onboarding PrimaCare**, akun tersebut diperlakukan sebagai **superadmin** → bisa mengakses **CRM Bidan dan PrimaCare**. Tidak ada master user terpisah. |
| 2 | Kode klinik | **Sama dengan PrimaCare** (satu identitas klinik/praktik). |
| 3 | Nomor RM | **Menggunakan standar PrimaCare.** |
| 4–6 | Kepemilikan field obstetri · master layanan/harga · batas produk | **Cakupan integrasi = sampai level data pasien dan riwayat pemeriksaan di rekam medis.** Field yang belum ada di PrimaCare **tetap disimpan**, namun **belum ditampilkan pada form pemeriksaan di PrimaCare**. |

### Keputusan lanjutan (14 Sep 2026, batch 2) — khusus PRD PTS-367

| # | Hal | Keputusan |
|:--|:--|:--|
| 1 | Registrasi/daftar akun di CRM Bidan | Link **"Belum punya akun? Daftar" → WhatsApp PrimaCare (sales/onboarding)**: https://api.whatsapp.com/send/?phone=6285124402922&text=Halo+PrimaCare%2C+saya+tertarik+untuk+mengetahui+lebih+lanjut+mengenai+layanan+Rekam+Medis+Elektronik+%28RME%29+PrimaCare.+Mohon+informasi+mengenai+fitur%2C+implementasi%2C+dan+cara+bermitranya.+Terima+kasih.&type=phone_number&app_absent=0 — **bukan** registrasi mandiri di aplikasi |
| 2 | Kode klinik di form login | **Tetap ditampilkan** (nilai mengikuti klinik pada akun PrimaCare) |
| 3 | Cakupan data pasien | Bidan **dapat melihat seluruh pasien klinik** (akses level superadmin) |
| 4 | Hapus data pasien | Tidak ada hard delete → aksi **"Arsipkan / Lepas dari daftar pasien"** |
| 5 | Keluarga Terhubung & "Ibu dari pasien" | **Masuk sisi CRM Bidan** (relasi keluarga dikelola di CRM) |
| 6 | Pasien duplikat | **Merge dilakukan di PrimaCare** |
| 7 | Mode offline | **Harus online** — tidak ada mode offline |
| 8 | Lokasi PRD | **PRD di-inject ke PTS-370** (Task "PRD", parent PTS-367) |

### Batas integrasi (tegas)
- **Termasuk:** data pasien (identitas + No RM) dan **riwayat pemeriksaan di rekam medis**.
- **Belum termasuk:** modul kasir/billing, master layanan & harga, dan pembatasan produk lain. Jadi CRM Bidan **tidak** (untuk sekarang) menyatukan billing dengan Kasir PrimaCare.
- **Field klinis baru** (mis. HPHT, HPL, G/P/A, TB, TBJ, Leopold, presentasi janin, catatan diagnosa) → **disimpan di sistem**, tapi **tidak muncul di form pemeriksaan PrimaCare** dulu. Artinya: CRM Bidan boleh menyimpan lebih kaya dari yang ditampilkan PrimaCare.

---

## 2. Referensi PrimaCare (dari dokumen internal, untuk konteks)

**User/Personil**
- 8 role: Dokter · Perawat · **Bidan** · Farmasi Staff · Frontdesk · Kasir · Administrasi · Finance Staff
- **Owner/Superadmin** = akun spesial · ada Permission Override per user + audit trail (PRD di PTS-28)
- Akun bidan CRM Bidan = dibuat via onboarding PrimaCare → **superadmin**

**Master Pasien**
- Nama pasien (depan + belakang) · Jenis kelamin · Nomor identitas (tipe + nomor) · Tempat lahir · Tanggal lahir (dd/mm/yyyy + auto umur) · Nomor handphone · **No. Rekam Medis** · Status
- Ada tab **"Riwayat Penggabungan"** → mekanisme merge/dedupe pasien sudah ada (PRD Rekam Medis & Pendaftaran, PTS-280)

**Kunjungan (encounter)**
- Waktu · Pasien · **Jenis Kunjungan** · **Layanan** · **Dokter & Poli** · Status
- Detail: Data Pengantar (tipe, nama, No HP) · Data PJ (hubungan, No identitas, nama, No HP, alamat) · Poli & Dokter · Pembayaran

**Lain-lain**
- PrimaCare punya modul Kasir (fitur kasir-diskon) — **di luar cakupan integrasi saat ini**
- PrimaCare memakai terminologi & kode **SATUSEHAT** (ICD-10 v2010, ICD-11 imunisasi, modul ANC SATUSEHAT)

---

## 3. Dampak per area CRM Bidan

| Area CRM Bidan (prototype) | Keputusan / tindakan |
|:--|:--|
| Login (email/No HP + password) | Pakai **akun PrimaCare**; bidan masuk sebagai superadmin → bisa akses kedua app. Password & reset password mengikuti mekanisme PrimaCare. |
| "Kode klinik" (cth PMB-RATNA) | **Sama dengan PrimaCare** — tidak membuat kode klinik sendiri. |
| Daftarkan pasien baru | Identitas pasien memakai **master + standar No RM PrimaCare**. Duplikat ditangani lewat mekanisme **merge/penggabungan** yang sudah ada. |
| Field obstetri (HPHT, HPL, status kehamilan, G/P/A, riwayat alergi/penyakit) | Diperlakukan sebagai **field baru**: disimpan, **belum tampil** di form pemeriksaan PrimaCare. |
| Kategori pasien (Hamil / Tidak Hamil / Anak / Laki-laki) | Turunan klinis di sisi CRM Bidan (tidak mengubah master pasien PrimaCare). |
| Keluarga Terhubung / "Ibu dari pasien" | Tetap di sisi CRM Bidan untuk sekarang (di luar cakupan integrasi). |
| Catatan kunjungan (ANC/persalinan/nifas/KB/imunisasi) | Menjadi bagian **riwayat pemeriksaan di rekam medis** → ikut tampil di riwayat pasien PrimaCare. |
| Kasir & Harga Layanan (PTS-602) | **Di luar cakupan integrasi** — tetap jalan sebagai fitur CRM Bidan sendiri (tidak menyatu dengan Kasir PrimaCare). |
| Laporan Puskesmas | Sumber data = catatan kunjungan/riwayat pemeriksaan yang sama. |
| Field baru (TB, TBJ, Leopold, presentasi/bagian terendah janin, catatan diagnosa) | Disimpan di CRM Bidan; belum ditampilkan di PrimaCare. |

---

## 4. Konsekuensi ke penulisan PRD (mulai PTS-367, due 15 Sep)

Setiap PRD epic CRM Bidan wajib memuat bagian **"Integrasi dengan PrimaCare"** berisi:
1. **Akun & akses** — akun PrimaCare (superadmin) → bisa akses CRM Bidan + PrimaCare; tidak ada user master terpisah
2. **Klinik** — kode klinik mengikuti PrimaCare
3. **Pasien** — identitas & No RM mengikuti standar PrimaCare; penanganan duplikat via merge
4. **Rekam medis** — catatan kunjungan CRM Bidan masuk ke riwayat pemeriksaan pasien
5. **Field baru** — disimpan, belum ditampilkan di PrimaCare (jelaskan agar tidak dianggap hilang)
6. **Di luar cakupan** — kasir/billing, master layanan & harga, relasi keluarga (belum diintegrasikan)

---

## 5. Yang masih dibutuhkan untuk tech ticket

- **Detail teknis skema PrimaCare**: nama kolom/tabel, tipe data, endpoint API, dan standar penomoran RM — belum tersedia di mesin ini.
- Setelah tersedia → lanjutkan **field-by-field mapping** (field CRM Bidan → kolom PrimaCare, wajib/opsional, tipe) supaya tech ticket bisa mengacu langsung, bukan deskriptif.
