# PRD — Pengaturan & Akun (CRM Bidan)

**Epic:** PTS-604 — `[CRM][Bidan] Pengaturan & Akun` · **Task PRD:** PTS-616
**Versi:** v1 · **Tanggal:** 16 Sep 2026 · **Penyusun:** Jo (AI Assistant) untuk Dhani Prisantika
**Prototype acuan:** `docs/prototype/CRMBidan_16sep2026.html`
**Hasil grill:** 8/8 pertanyaan dijawab oleh pemilik produk (semua opsi A) — ringkasan di §5.

> **Fokus dokumen ini:** identitas praktik & akun bidan pada aplikasi CRM Bidan — data yang tampil di aplikasi **dan di pesan ke pasien**, akses ke menu pengaturan lain, serta keluar akun.

---

## 1. Overview & Scope Boundary

**Yang dibangun:** satu halaman **Pengaturan Klinik** yang menjadi sumber tunggal identitas praktik (nama praktik, lokasi, tautan lokasi, kontak) + halaman **Lainnya** sebagai pintu ke pengaturan lain dan keluar akun, dengan **identitas akun PrimaCare (kode klinik & email) tampil read-only**.

**Untuk siapa:** bidan pengelola praktik mandiri (PMB) yang memakai modul CRM Bidan.

**Masalah:**
1. **Tautan lokasi praktik belum ada** di data praktik, padahal blok penutup pesan pengingat WhatsApp sudah membutuhkannya (keputusan PTS-386) — akibatnya baris lokasi tidak pernah bisa tampil.
2. **Nama bidan & nama praktik tertukar pemakaian**: pesan pengingat memakai nama **bidan** dengan fallback `"— Bidan"`, padahal keputusan produk sudah **mencabut nama bidan** dari pesan.
3. Ada **menu "Layanan Wellness" yang buntu** — menu tampil di daftar, tetapi halamannya tidak ada.
4. **Identitas akun tidak terlihat** di pengaturan, dan kartu "Masuk sebagai" menampilkan data yang salah (email sebagai nama, kode klinik sebagai nama praktik).
5. Kontak praktik **digabung dalam satu field** "No. HP / Email" sehingga sulit dibedakan fungsinya.

**Di dalam cakupan:**
- Field **Pengaturan Klinik**: Nama bidan (wajib) · Nama praktik/klinik (wajib) · Lokasi praktik · **Tautan lokasi praktik** (baru) · **No. HP / WhatsApp praktik** (baru, opsional).
- **Identitas akun read-only**: Kode klinik + Email akun dengan keterangan *"dikelola lewat PrimaCare"*.
- **Kartu "Masuk sebagai"** yang benar (nama bidan + nama praktik) dan **Keluar akun** dengan konfirmasi.
- **Menghapus menu "Layanan Wellness"** sehingga tidak ada tautan buntu; katalog wellness tetap dikelola lewat **Harga Layanan**.
- **Pemakaian identitas**: nama praktik di header aplikasi, penutup pesan pengingat, dan kop struk; nama bidan hanya pada kartu akun & sapaan internal.

**Di luar cakupan:**
- **Ganti password & ubah email** — diarahkan ke **"Lupa password"** pada layar login (kode klinik + email) dan admin PrimaCare.
- **Logo/foto praktik & tanda tangan bidan** — header aplikasi dan kop struk tetap memakai nama praktik (teks).
- **Jam praktik, tipe praktik, multi-pengguna/peran (role), notifikasi pengingat melengkapi identitas, pengaturan tarif pajak/kas** — tidak dibahas di rilis ini.
- **Pendaftaran akun baru** (onboarding) dan **login** — sudah diatur pada epic PTS-367 dan alur onboarding PrimaCare.

---

## 2. User Stories

| ID | User Story | Prioritas |
|---|---|---|
| US-01 | Sebagai bidan, saya ingin **mengelola identitas praktik** (nama bidan, nama praktik, lokasi, tautan lokasi Google Maps, No. HP/WhatsApp praktik) supaya data yang tampil pada pesan dan dokumen untuk pasien selalu benar. | Must Have |
| US-02 | Sebagai bidan, saya ingin **melihat identitas akun** (kode klinik & email) yang sedang saya pakai, supaya saya tahu akun mana yang aktif tanpa harus keluar-masuk aplikasi. | Must Have |
| US-03 | Sebagai bidan, saya ingin **melihat siapa yang sedang masuk dan bisa keluar akun dari satu tempat**, supaya perangkat praktik tetap aman ketika dipakai bergantian. | Must Have |
| US-04 | Sebagai bidan, saya ingin **tautan lokasi praktik otomatis ikut terkirim** pada penutup pesan pengingat, supaya pasien mudah menemukan lokasi praktik. | Must Have |
| US-05 | Sebagai bidan, saya ingin **tidak menemui menu yang buntu**, supaya katalog layanan wellness tetap bisa dikelola meski menu khususnya dihapus. | Must Have |

## 3. Functional Requirements

| ID | Requirement |
|---|---|
| FR-01 | **Pengaturan Klinik** memuat field: **Nama bidan** (wajib) · **Nama praktik/klinik** (wajib) · **Lokasi praktik** (teks, mis. `Bandung, Jawa Barat`) · **Tautan lokasi praktik** (baru) · **No. HP / WhatsApp praktik** (baru). Field gabungan lama "No. HP / Email" **dihapus**. |
| FR-02 | **Tautan lokasi praktik** diisi dengan **menempel tautan** dari Google Maps, divalidasi ringan: harus diawali `http` dan mengandung `google`/`maps`. Bila tidak memenuhi syarat → **pesan galat** dan data tidak tersimpan. |
| FR-03 | Field **Tautan lokasi** dan **No. HP / WhatsApp praktik** bersifat **opsional** — pengaturan tetap bisa disimpan tanpa keduanya. |
| FR-04 | **Identitas akun read-only**: halaman menampilkan **Kode klinik** dan **Email akun** sebagai data yang tidak dapat diubah dari CRM Bidan, dengan keterangan *"dikelola lewat PrimaCare"*. |
| FR-05 | **Simpan** menyimpan identitas praktik ke sesi berjalan dan menampilkan **notifikasi toast "Tersimpan"**. Tidak ada tombol tambahan (pratinjau/pesan uji) pada rilis ini. |
| FR-06 | **Nama praktik** dipakai pada **header aplikasi**, **blok penutup pesan pengingat WhatsApp**, dan **kop struk pembayaran**. Bila nama praktik kosong, dipakai fallback **`Praktik Mandiri Bidan`**. |
| FR-07 | **Nama bidan tidak dicantumkan** pada pesan ke pasien maupun struk; nama bidan dipakai hanya pada **kartu "Masuk sebagai"** dan sapaan internal aplikasi. |
| FR-08 | **Tautan lokasi praktik** otomatis dipakai sebagai baris `📍 <tautan>` pada penutup pesan pengingat. Bila kosong → **baris lokasi tidak ditampilkan** dan pengingat tetap terkirim. |
| FR-09 | Menu **"Layanan Wellness" dihapus** dari daftar menu (sidebar & halaman Lainnya) sehingga tidak ada tautan menuju halaman yang tidak tersedia. |
| FR-10 | Halaman **"Lainnya"** memuat kartu menu: **Laporan Puskesmas · Keuangan · Harga Layanan · Pengaturan Klinik**, katalog layanan wellness & komplementer dikelola melalui **Harga Layanan** per kategori. |
| FR-11 | Kartu **"Masuk sebagai"** menampilkan **nama bidan** dan **nama praktik/klinik** yang sedang aktif (bukan email atau kode klinik). |
| FR-12 | Aksi **"Keluar"** menampilkan konfirmasi, lalu menghapus sesi dan mengembalikan pengguna ke layar **Login**. Data pasien & catatan layanan tidak terhapus. |

---

## 4. Acceptance Criteria

**US-01 — Kelola identitas praktik**
- **Given** bidan membuka Pengaturan Klinik, **When** halaman tampil, **Then** tersedia field Nama bidan, Nama praktik/klinik, Lokasi praktik, **Tautan lokasi praktik**, dan **No. HP / WhatsApp praktik**.
- **Given** nama bidan atau nama praktik/klinik dikosongkan, **When** bidan menekan Simpan, **Then** muncul pesan galat pada field tersebut dan data **tidak** tersimpan.
- **Given** tautan lokasi diisi `bit.ly/lokasi-praktik`, **When** bidan menekan Simpan, **Then** muncul pesan galat bahwa tautan harus berupa tautan Google Maps.
- **Given** tautan lokasi diisi `https://maps.app.goo.gl/pmb-ratna-sejahtera`, **When** bidan menekan Simpan, **Then** data tersimpan dan muncul toast *"Tersimpan"*.

**US-02 — Identitas akun terlihat**
- **Given** bidan membuka Pengaturan Klinik, **Then** **Kode klinik** dan **Email akun** tampil sebagai data read-only dengan keterangan *"dikelola lewat PrimaCare"*.
- **When** bidan mencoba mengubah kode klinik atau email, **Then** kedua field **tidak dapat disunting** dan tidak ada aksi simpan untuk keduanya.
- **Given** sesi tidak memuat data akun, **Then** kedua field menampilkan tanda `-` tanpa membuat halaman gagal tampil.

**US-03 — Masuk sebagai & keluar akun**
- **Given** bidan sudah masuk, **When** membuka halaman Lainnya, **Then** kartu "Masuk sebagai" menampilkan **nama bidan** dan **nama praktik/klinik** yang benar.
- **When** bidan menekan **Keluar**, **Then** muncul dialog konfirmasi; setelah dikonfirmasi sesi terhapus dan pengguna kembali ke layar Login.
- **When** bidan membatalkan dialog konfirmasi, **Then** tetap berada di halaman Lainnya tanpa perubahan.

**US-04 — Tautan lokasi terkirim ke pasien**
- **Given** tautan lokasi praktik sudah diisi, **When** bidan mengirim pengingat kunjungan, **Then** blok penutup memuat **nama praktik** dan baris `📍 <tautan lokasi>` serta **tidak memuat nama bidan**.
- **Given** tautan lokasi belum diisi, **When** pengingat dikirim, **Then** baris `📍` **tidak ditampilkan** dan pesan tetap terkirim.
- **Given** tautan lokasi diubah, **When** pengingat berikutnya dikirim, **Then** pesan memakai **tautan yang baru** tanpa perlu login ulang.

**US-05 — Tidak ada menu buntu**
- **Given** menu aplikasi dibuka, **Then** **tidak ada** item "Layanan Wellness" yang mengarah ke halaman tidak tersedia.
- **Given** bidan ingin mengatur layanan wellness/komplementer, **When** membuka **Harga Layanan**, **Then** kategori wellness tetap dapat dikelola beserta harganya.

## 5. Keputusan Grill (8 butir — semua opsi A)

| No. | Topik | Keputusan |
|---|---|---|
| 1 | Identitas akun | Kode klinik & email ditampilkan **read-only** ("dikelola lewat PrimaCare"). |
| 2 | Field kontak | "No. HP / Email" → **"No. HP / WhatsApp praktik"** (opsional). |
| 3 | Tautan lokasi | **Input tempel tautan** + validasi ringan; kosong → baris 📍 tidak tampil. |
| 4 | Menu "Layanan Wellness" | **Dihapus**; katalog wellness lewat **Harga Layanan**. |
| 5 | Ganti password & ubah email | **Di luar cakupan** (Lupa password / admin PrimaCare). |
| 6 | Logo praktik | **Di luar cakupan** (pakai nama praktik teks). |
| 7 | Umpan balik simpan | **Toast "Tersimpan"** saja. |
| 8 | Nama bidan vs praktik | **Nama praktik** di header/pesan/struk; **nama bidan** hanya di kartu akun & internal. |

## 6. State & Edge Cases

| No. | Keadaan | Perilaku yang diharapkan |
|---|---|---|
| 1 | Tautan lokasi tidak memenuhi format | Pesan galat pada field; data tidak tersimpan sampai diperbaiki. |
| 2 | Nama praktik kosong | Fallback `Praktik Mandiri Bidan` dipakai pada header, pesan, dan struk. |
| 3 | Nama bidan kosong | Field wajib — pesan galat, tidak tersimpan. |
| 4 | No. HP / WhatsApp praktik kosong | Tidak memblokir penyimpanan; nomor tidak ditampilkan di aplikasi. |
| 5 | Sesi tidak memuat data akun (sesi lama) | Kode klinik & email tampil `-`; halaman tetap dapat disimpan. |
| 6 | Bidan menekan Keluar saat ada perubahan belum disimpan | Konfirmasi keluar berlaku; perubahan yang belum disimpan hilang (dipastikan lewat dialog). |
| 7 | Tautan lokasi diperbarui | Pesan pengingat berikutnya memakai tautan baru; tidak ada proses login ulang. |
| 8 | Menu Layanan Wellness diakses lewat tautan lama (bookmark) | Halaman tidak ditemukan; navigasi kembali ke halaman Lainnya (tidak blank). |

**Keterbatasan yang diketahui (bukan cacat):** perubahan identitas praktik **tidak menulis ulang** pesan yang sudah terkirim; hanya memengaruhi pengiriman berikutnya.

---

## 7. Non-Functional Requirements

| ID | Requirement |
|---|---|
| NFR-01 | **Privasi:** halaman hanya menampilkan data praktik & akun; tidak menampilkan data pasien maupun data medis. |
| NFR-02 | **Aksesibilitas:** setiap field memiliki label teks; pesan galat tidak hanya berupa warna; target sentuh tombol minimal 44×44 px. |
| NFR-03 | **Konsistensi bahasa & format:** Bahasa Indonesia; keterangan identitas akun memakai kalimat baku *"dikelola lewat PrimaCare"*; format nomor telepon mengikuti standar Indonesia. |

---

## 8. Dependencies & Open Items

**Ketergantungan:**
- **PTS-386 (Pengingat Kunjungan)** — memakai nama praktik + tautan lokasi pada blok penutup pesan; field tautan lokasi berasal dari epic ini.
- **Kasir & Harga Layanan (PTS-602)** — kop struk memakai nama praktik; katalog wellness dikelola di Harga Layanan.
- **PTS-367 (Login & Manajemen Data Pasien)** — alur login/logout; layar login memuat **Lupa password** (kode klinik + email).
- **PrimaCare** — sumber **kode klinik**, **akun**, dan **email** (read-only di CRM Bidan).

**Open items:**
1. Perlu tidaknya **jam praktik** pada identitas praktik (untuk struk/pesan).
2. Apakah **No. HP / WhatsApp praktik** nantinya dipakai sebagai pengirim/CC pesan otomatis saat WhatsApp Business API tersedia.
3. Perlu tidaknya **penanda praktik sudah melengkapi identitas** (mis. pengingat di Beranda bila nama praktik/tautan lokasi kosong).

**Iterasi berikutnya (di luar PRD ini):**
- Ganti password mandiri · ubah email · logo praktik & tanda tangan · multi-pengguna/peran · pengaturan jam praktik · pengaturan template struk.

## 9. Referensi

- Prototype acuan: `docs/prototype/CRMBidan_16sep2026.html`
- Naskah pengingat (penutup: nama praktik + tautan lokasi): PRD **PTS-386** §5.5 · PRD lokal `features/crm-bidan/prd/prd-PTS-376-wa-reminder-v3.md`
- Hasil grill & keputusan produk: `/tmp/grill-pengaturan-604.md` + `features/crm-bidan/context/CONTEXT.md`
- Naskah ringkasan & invoice: `docs/copy-deck-wa-ringkasan-invoice.md`
- Epic terkait: **PTS-367** (Login & Manajemen Data Pasien) · **PTS-602** (Kasir & Harga Layanan) · **PTS-376** (Pengingat & Jadwal Kunjungan)

## 10. Riwayat Revisi

| Versi | Tanggal | Perubahan |
|---|---|---|
| **v1** | 16 Sep 2026 | Versi pertama: 5 user story · 12 FR · 15 AC · 8 edge case · 3 NFR. Berdasarkan hasil grill 8/8 (semua opsi A) atas kondisi prototype 16 Sep: field **Tautan lokasi praktik** ditambahkan, field kontak dipecah, identitas akun read-only, menu "Layanan Wellness" dihapus, dan pemakaian **nama praktik** diseragamkan pada header/pesan/struk. |
