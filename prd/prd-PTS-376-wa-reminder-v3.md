# PRD — WA Reminder (Pengingat Kunjungan, Semua Layanan)

**Epic:** PTS-376 — `[CRM][Bidan] WA Follow-up 1-Click` · **Task PRD:** PTS-386
**Versi:** v3.3 · **Tanggal:** 15 Sep 2026 · **Penyusun:** Jo (AI Assistant) untuk Dhani Prisantika
**Prototype acuan:** `docs/prototype/CRMBidan_15sep2026.html`

> **Fokus dokumen ini:** pengingat kunjungan via WhatsApp untuk **seluruh jenis layanan** (ANC · Persalinan · Nifas · KB · Layanan Lain · Imunisasi) — bukan hanya kontrol kehamilan. Dasarnya: setiap pencatatan layanan sudah meminta **tanggal kunjungan berikutnya**, jadi pengingat harus bisa dipakai untuk semua jenis jadwal itu.
>
> Perilaku pengingat yang **sudah berjalan di prototype** (lima pintu masuk, isi modal pengingat, pengiriman `wa.me`) **tidak berubah** dan tidak diulang di dokumen ini — rinciannya di `features/crm-bidan/context/CONTEXT.md` §15.
>
> Naskah pesan **ringkasan pemeriksaan** & **invoice/struk** ada di dokumen terpisah (`docs/copy-deck-wa-ringkasan-invoice.md`).

---

## 1. Overview & Scope Boundary

**Yang dibangun:** satu pengingat kunjungan yang berfungsi untuk **semua jenis layanan** — bidan melihat **berapa pasien yang perlu diingatkan** (badge di menu), mendapat **daftar harian** lintas layanan, dan mengirim pengingat **satu klik** dengan naskah yang otomatis menyesuaikan **jenis layanan** dan **kondisi jadwal** (lewat / hari ini / akan datang).

**Untuk siapa:** bidan pengelola praktik mandiri (PMB) yang memakai modul CRM Bidan.

**Masalah:**
1. Jadwal kunjungan berikutnya **dicatat di semua layanan**, tapi pengingat yang jalan sekarang **hanya menjangkau jadwal ANC** — pengingat imunisasi bahkan **tidak pernah dikirim** meski tanggalnya sudah dicatat.
2. Angka di menu **tidak ada**, jadi bidan tidak tahu berapa pasien yang perlu dihubungi tanpa membuka halaman satu per satu.
3. Naskah pesan masih **satu nada, khusus kehamilan** — tidak cocok untuk KB, nifas, atau imunisasi anak.

**Di dalam cakupan:**
- **Badge angka** di menu Jadwal Kunjungan = jumlah pasien yang perlu diingatkan (semua jenis jadwal: hari ini + lewat).
- **Daftar harian lintas layanan** di Beranda + **halaman Jadwal Kunjungan menampilkan semua kategori pasien** (tidak lagi hanya pasien hamil), urut paling mendesak, dengan **label jenis layanan** pada tiap baris.
- **Imunisasi masuk sebagai sumber jadwal** (menyusul 5 sumber yang sudah ada).
- **Modal pengingat menampilkan semua jadwal** pasien bila lebih dari satu — bidan memilih mana yang diingatkan (jadwal terdekat terpilih default).
- **Naskah pengingat per jenis layanan × kondisi jadwal** (modular), termasuk aturan sapaan (pasien <18 tahun, pesan anak dialamatkan ke orang tua) dan blok penutup berisi **nama praktik/klinik + tautan lokasi Google Maps** (diambil dari profil klinik).

**Di luar cakupan:**
- **Push notification** — ditunda: CRM Bidan berbasis **web** (butuh PWA/service worker + domain HTTPS). Kebutuhan "siapa yang perlu dihubungi hari ini" dijawab badge + daftar harian.
- **WhatsApp Business API** (kirim otomatis/terjadwal, status "terkirim") — rilis ini memakai deep link `wa.me`.
- **Riwayat follow-up / penanda "sudah diingatkan"** — pengiriman tidak dicatat.
- **Kirim massal serentak** — daftar diproses satu per satu.
- **Preview teks di dalam CRM** sebelum membuka WhatsApp.
- **Naskah ringkasan pemeriksaan & invoice/struk** — dokumen & PRD terpisah.

---

## 2. User Stories

| ID | User Story | Prioritas |
|---|---|---|
| US-01 | Sebagai bidan, saya ingin **melihat angka pasien yang perlu diingatkan hari ini langsung dari menu**, supaya tidak ada follow-up yang terlewat. | Must Have |
| US-02 | Sebagai bidan, saya ingin **pengingat berlaku untuk semua layanan** (ANC, KB, nifas, persalinan, layanan lain, imunisasi anak), supaya pasien tidak hanya diingatkan untuk kontrol kehamilan. | Must Have |
| US-03 | Sebagai bidan, saya ingin **tahu ini pengingat untuk layanan apa** dan **bisa memilih jadwal mana** bila pasien punya lebih dari satu jadwal, supaya pesannya tepat. | Must Have |
| US-04 | Sebagai bidan, saya ingin **mengirim pengingat dalam satu klik tanpa mengetik**, supaya follow-up cepat dan seragam. | Must Have |
| US-05 | Sebagai orang tua, saya ingin **menerima pengingat imunisasi anak** dengan nama anak dan jenis imunisasi berikutnya, supaya jadwal imunisasi tidak terlewat. | Must Have |

## 3. Functional Requirements

| ID | Requirement |
|---|---|
| FR-01 | Menu **Jadwal Kunjungan** menampilkan **badge angka** = jumlah pasien yang perlu diingatkan (jadwal **hari ini** + **sudah lewat**) dari **semua jenis layanan**: ANC · Persalinan · Nifas · KB · Layanan Lain · Imunisasi. Angka dihitung dari **data jadwal**, bukan riwayat pengiriman WA. |
| FR-02 | Badge **tidak ditampilkan** bila tidak ada pasien yang perlu diingatkan. |
| FR-03 | Beranda menampilkan **daftar harian lintas layanan** (hari ini + lewat), **urut paling mendesak**, dan setiap baris memuat **label jenis layanan** (mis. `ANC`, `KB`, `Imunisasi`) + nama pasien/anak. |
| FR-04 | **Halaman Jadwal Kunjungan menampilkan semua kategori pasien** yang punya jadwal berikutnya — **tidak lagi hanya pasien hamil** — dengan urutan paling mendesak dan label jenis layanan pada tiap kartu. |
| FR-05 | **Imunisasi menjadi sumber jadwal**: jadwal berikutnya diambil dari catatan imunisasi anak (tanggal kunjungan berikutnya + antigen yang akan diberikan), sehingga masuk perhitungan badge, daftar harian, dan halaman Jadwal. |
| FR-06 | Bila pasien punya **lebih dari satu jadwal**, modal pengingat menampilkan **semua jadwal** tersebut (jenis layanan + tanggal + status urgensi) dan bidan **memilih satu** yang akan diingatkan; **jadwal terdekat terpilih sebagai default**. |
| FR-07 | Teks pengingat disusun dari **lima bagian tetap**: (1) salam pembuka + kalimat doa singkat, (2) kalimat pengantar yang menyebut **nama praktik** dan **jenis layanan**, (3) **blok jadwal** (`📅 Hari, Tanggal` — tanpa jam), (4) ajakan konfirmasi kehadiran atau penjadwalan ulang, (5) penutup. Naskah lengkap per jenis & kondisi ada di §5. |
| FR-08 | Nilai dinamis pada pesan: **nama pasien** (atau **nama anak** untuk imunisasi) · **hari & tanggal format panjang Indonesia** (mis. `Sabtu, 20 September 2026`) · **nama layanan/antigen**. **Jam tidak disertakan** karena data jadwal hanya menyimpan tanggal. |
| FR-09 | **Aturan sapaan:** pasien dewasa memakai `Ibu <nama pasien>`; pasien **di bawah 18 tahun** tanpa sapaan "Ibu" (memakai nama saja dengan bentuk "kamu/-mu"); pesan untuk **anak (imunisasi) dialamatkan ke orang tua** — `Salam sehat, Ibu <nama orang tua>.` sambil menyebut nama anak. |
| FR-10 | **Ajakan (bagian 4):** varian "hari ini" dan "akan datang" memakai kalimat *"Agar kami dapat mempersiapkan pelayanan terbaik, mohon berkenan membalas pesan ini untuk konfirmasi kehadiran Ibu."* · varian "lewat" memakai ajakan **penjadwalan ulang**. |
| FR-11 | **Blok penutup (bagian 5):** `Kami tunggu kedatangannya dengan senang hati. 🙏` + **nama praktik/klinik** + baris **`📍 <tautan lokasi Google Maps>`**. **Nama bidan tidak dicantumkan.** Nama praktik & tautan lokasi diambil dari profil klinik; fallback nama praktik `Praktik Mandiri Bidan`; bila tautan lokasi kosong → baris `📍` tidak ditampilkan. Varian "lewat" memakai penutup `Kami tunggu kabar baiknya. 🙏`. |
| FR-12 | **Nomor tujuan per jenis:** layanan pasien → **No. HP pasien**; **imunisasi anak → No. HP anak, fallback No. HP orang tua** (kolom No. HP tetap wajib pada data pasien). |

---

## 4. Acceptance Criteria

**US-01 — Angka pasien yang perlu diingatkan terlihat dari menu**
- **Given** ada 2 pasien dengan jadwal kontrol **hari ini** dan 1 pasien yang **sudah lewat**, **When** bidan membuka aplikasi, **Then** menu Jadwal Kunjungan menampilkan **badge angka 3** dan daftar harian memuat ketiganya dengan yang paling lama lewat di urutan atas.
- **Given** tidak ada jadwal hari ini maupun yang lewat, **When** aplikasi dibuka, **Then** **badge tidak ditampilkan**.
- **Given** bidan mengubah tanggal jadwal dari modal pengingat lalu menyimpan, **Then** angka badge dan urutan daftar **ikut menyesuaikan**.

**US-02 — Pengingat berlaku untuk semua layanan**
- **Given** pasien punya jadwal **KB** (`tanggalKontrolBerikutnya`) dan **hari ini** adalah tanggalnya, **Then** pasien muncul di daftar harian dengan label **`KB`** dan dapat diingatkan.
- **Given** anak punya jadwal **imunisasi berikutnya**, **Then** anak muncul di daftar harian & perhitungan badge (sebelumnya tidak muncul sama sekali), dan pengingat dikirim ke **No. HP anak → fallback No. HP orang tua**.
- **Given** halaman Jadwal Kunjungan dibuka, **Then** pasien **non-hamil** yang punya jadwal (mis. KB/nifas/imunisasi) **tetap tampil** di daftar.

**US-03 — Tahu jenis layanan & bisa memilih jadwal**
- **Given** satu pasien punya **dua jadwal** (mis. KB dan imunisasi anak), **When** modal pengingat dibuka, **Then** **kedua jadwal ditampilkan** dengan jenis + tanggal + status, dan **jadwal terdekat terpilih default**.
- **When** bidan memilih jadwal lain lalu menekan Kirim WA, **Then** naskah pesan memakai **jenis layanan jadwal yang dipilih** dan tanggal jadwal tersebut.
- **Given** hanya ada satu jadwal, **Then** modal menampilkan satu jadwal tanpa pemilihan.

**US-04 — Kirim pengingat satu klik**
- **Given** daftar harian menampilkan pasien, **When** bidan menekan nama lalu tombol **Kirim WA**, **Then** WhatsApp terbuka dengan nomor tujuan dan **teks pengingat sudah terisi**; teks boleh disunting bidan dan penyuntingan tidak mengubah naskah untuk pengiriman berikutnya.
- **Given** profil klinik sudah diisi nama praktik & tautan lokasi, **When** pengingat dikirim, **Then** blok penutup memuat **nama praktik + tautan lokasi Google Maps** dan **tidak memuat nama bidan**.
- **Given** jadwal kunjungan memiliki tanggal, **When** pengingat dikirim, **Then** pesan memuat baris `📅 Hari, Tanggal` dengan nama hari + nama bulan penuh, dan **tidak menyertakan jam**.

**US-05 — Pengingat imunisasi anak**
- **Given** anak bernama "Dinda" punya jadwal imunisasi **DPT-HB-Hib 1** dengan orang tua "Siti", **When** pengingat dikirim, **Then** pesan memakai varian **imunisasi** yang menyebut **nama anak**, **jenis imunisasi berikutnya**, dan tanggal, disapa ke **orang tua**.

## 5. Naskah Pesan Pengingat

### 5.1 Struktur naskah (lima bagian tetap)

Setiap pengingat disusun dari lima bagian tetap, sehingga bidan tinggal mengirim tanpa mengetik apa pun:

| Bagian | Isi | Menyesuaikan |
|---|---|---|
| 1. Salam pembuka | `Salam sehat, Ibu <nama pasien>.` + satu kalimat doa singkat | jenis layanan |
| 2. Kalimat pengantar | `Kami dari <nama praktik> ingin mengingatkan terkait <nama layanan> Ibu yang akan datang pada:` | jenis layanan + kondisi jadwal |
| 3. Blok jadwal | `📅 Hari, Tanggal: <hari>, <tanggal>` | kondisi jadwal |
| 4. Ajakan | Konfirmasi kehadiran (hari ini / akan datang) atau penjadwalan ulang (lewat) | kondisi jadwal |
| 5. Penutup | `Kami tunggu kedatangannya dengan senang hati. 🙏` + nama praktik + `📍 <tautan lokasi>` | tetap |

### 5.2 Bagian yang menyesuaikan jenis layanan

| Jenis layanan | Kalimat doa (bagian 1) | Nama layanan pada kalimat pengantar (bagian 2) |
|---|---|---|
| ANC — kontrol kehamilan | Semoga Ibu dan calon buah hati selalu dalam keadaan baik. | jadwal pemeriksaan kehamilan (ANC) |
| Persalinan — kontrol setelah bersalin | Semoga Ibu dan buah hati selalu dalam keadaan baik. | jadwal kontrol setelah persalinan |
| Nifas — kunjungan nifas | Semoga Ibu dan buah hati selalu dalam keadaan baik. | jadwal kunjungan nifas |
| KB — kontrol ulang | Semoga Ibu selalu dalam keadaan baik. | jadwal kontrol KB ulang |
| Layanan Lain (wellness) | Semoga Ibu selalu dalam keadaan baik. | jadwal layanan |
| Imunisasi anak *(ke orang tua)* | Semoga <nama anak> selalu sehat dan tumbuh kembangnya baik. | jadwal imunisasi <nama anak> (<antigen>) |

### 5.3 Blok jadwal (bagian 3) & ajakan (bagian 4) menurut kondisi

| Kondisi | Kalimat pengantar | Blok jadwal | Ajakan |
|---|---|---|---|
| Akan datang | `…yang akan datang pada:` | `📅 Hari, Tanggal: Sabtu, 20 September 2026` | `Agar kami dapat mempersiapkan pelayanan terbaik, mohon berkenan membalas pesan ini untuk konfirmasi kehadiran Ibu.` |
| Hari ini | `…yang dijadwalkan hari ini:` | `📅 Hari, Tanggal: Rabu, 16 September 2026` | sama dengan "akan datang" |
| Lewat | `…yang sudah lewat pada:` | `📅 Hari, Tanggal: Rabu, 12 Agustus 2026` | `Mohon berkenan membalas pesan ini agar kami dapat menjadwalkan ulang kunjungan Ibu.` |

### 5.4 Sapaan khusus

**Pasien dewasa** → `Salam sehat, Ibu <nama pasien>.` dan penutup ajakan memakai kata "Ibu".

**Pasien di bawah 18 tahun** → sapaan "Ibu" tidak dipakai; memakai nama saja dengan bentuk "kamu/-mu":

```
Salam sehat, Dinda.
Semoga kamu dan calon buah hati selalu dalam keadaan baik.

Kami dari PMB Ratna Sejahtera ingin mengingatkan terkait jadwal pemeriksaan kehamilan (ANC) yang akan datang pada:
📅 Hari, Tanggal: Sabtu, 20 September 2026

Agar kami dapat mempersiapkan pelayanan terbaik, mohon berkenan membalas pesan ini untuk konfirmasi kehadiranmu.
```

**Pesan untuk anak (imunisasi)** → dikirim ke **orang tua**, disapa sebagai orang tua namun menyebut nama anak:

`Salam sehat, Ibu <nama orang tua>.` … `terkait jadwal imunisasi <nama anak> (<antigen>) …`

### 5.5 Blok penutup (bagian 5)

```
Kami tunggu kedatangannya dengan senang hati. 🙏

<nama praktik/klinik>
📍 <tautan lokasi Google Maps>
```
*(nama praktik & tautan lokasi diambil dari **profil klinik**; **nama bidan tidak dicantumkan**. Fallback bila nama praktik kosong: `Praktik Mandiri Bidan`. Bila tautan lokasi kosong → baris `📍` tidak ditampilkan. Untuk varian "lewat", kalimat penutup memakai `Kami tunggu kabar baiknya. 🙏`)*

### 5.6 Contoh pesan lengkap

**a. ANC — akan datang**
```
Salam sehat, Ibu Siti.
Semoga Ibu dan calon buah hati selalu dalam keadaan baik.

Kami dari PMB Ratna Sejahtera ingin mengingatkan terkait jadwal pemeriksaan kehamilan (ANC) Ibu yang akan datang pada:
📅 Hari, Tanggal: Sabtu, 20 September 2026

Agar kami dapat mempersiapkan pelayanan terbaik, mohon berkenan membalas pesan ini untuk konfirmasi kehadiran Ibu.

Kami tunggu kedatangannya dengan senang hati. 🙏

PMB Ratna Sejahtera
📍 https://maps.app.goo.gl/pmb-ratna-sejahtera
```

**b. KB — hari ini**
```
Salam sehat, Ibu Siti.
Semoga Ibu selalu dalam keadaan baik.

Kami dari PMB Ratna Sejahtera ingin mengingatkan terkait jadwal kontrol KB ulang Ibu yang dijadwalkan hari ini:
📅 Hari, Tanggal: Rabu, 16 September 2026

Agar kami dapat mempersiapkan pelayanan terbaik, mohon berkenan membalas pesan ini untuk konfirmasi kehadiran Ibu.

Kami tunggu kedatangannya dengan senang hati. 🙏

PMB Ratna Sejahtera
📍 https://maps.app.goo.gl/pmb-ratna-sejahtera
```

**c. Imunisasi anak — akan datang (dikirim ke orang tua)**
```
Salam sehat, Ibu Siti.
Semoga Dinda selalu sehat dan tumbuh kembangnya baik.

Kami dari PMB Ratna Sejahtera ingin mengingatkan terkait jadwal imunisasi Dinda (DPT-HB-Hib 1) yang akan datang pada:
📅 Hari, Tanggal: Sabtu, 14 November 2026

Agar kami dapat mempersiapkan pelayanan terbaik, mohon berkenan membalas pesan ini untuk konfirmasi kehadiran Ibu.

Kami tunggu kedatangannya dengan senang hati. 🙏

PMB Ratna Sejahtera
📍 https://maps.app.goo.gl/pmb-ratna-sejahtera
```

**d. Nifas — sudah lewat**
```
Salam sehat, Ibu Siti.
Semoga Ibu dan buah hati selalu dalam keadaan baik.

Kami dari PMB Ratna Sejahtera ingin mengingatkan terkait jadwal kunjungan nifas Ibu yang sudah lewat pada:
📅 Hari, Tanggal: Rabu, 12 Agustus 2026

Mohon berkenan membalas pesan ini agar kami dapat menjadwalkan ulang kunjungan Ibu.

Kami tunggu kabar baiknya. 🙏

PMB Ratna Sejahtera
📍 https://maps.app.goo.gl/pmb-ratna-sejahtera
```

**Catatan:** tanggal memakai **nama bulan penuh** (bukan singkatan) agar mudah dibaca · **jam tidak disertakan** — data jadwal hanya menyimpan tanggal (keputusan produk) · naskah mengikuti **jadwal yang dipilih bidan** bila pasien punya beberapa jadwal.

## 6. State & Edge Cases

| No. | Keadaan | Perilaku yang diharapkan |
|---|---|---|
| 1 | Pasien punya **lebih dari satu jadwal** (mis. KB + imunisasi anak) | Modal menampilkan semua jadwal; **jadwal terdekat terpilih default**; naskah mengikuti pilihan bidan. |
| 2 | **Imunisasi anak tanpa No. HP anak** | Pengingat dikirim ke **No. HP orang tua** yang terhubung. |
| 3 | Anak **belum punya jadwal imunisasi** (tanpa catatan imunisasi) | Tidak muncul di daftar pengingat; bidan diarahkan mencatat layanan dulu dari kartu "Rencana kunjungan". |
| 4 | Jadwal **sudah lewat sangat lama** | Tetap muncul di urutan paling atas; tidak ada pembatasan waktu. |
| 5 | Nomor HP berformat `+62…` / memakai spasi | Dinormalisasi otomatis (hanya digit, `08…` → `628…`). |
| 6 | Nomor tidak valid / terlalu pendek | Pengingat tidak diblokir — WhatsApp tetap dibuka agar bidan memilih kontak manual. |
| 7 | WhatsApp tidak terpasang / tidak dapat dibuka | Muncul pesan informatif dan bidan bisa mencoba lagi; tidak ada data yang hilang. |
| 8 | Nama praktik belum diisi | Penutup memakai fallback **`Praktik Mandiri Bidan`**; pengingat tetap terkirim. |
| 9 | Pasien hamil **tanpa HPHT** | Muncul di daftar dengan label "HPHT belum diisi"; tetap dapat diingatkan bila punya jadwal. |
| 10 | **Tautan lokasi klinik belum diisi** | Baris lokasi tidak ditampilkan; blok penutup hanya memuat nama praktik. |

**Keterbatasan yang diketahui (bukan cacat):** pengiriman **tidak dicatat**, jadi pasien tetap muncul di daftar sampai jadwalnya diperbarui/dikunjungi.

---

## 7. Non-Functional Requirements

| ID | Requirement |
|---|---|
| NFR-01 | **Privasi isi pesan:** pengingat hanya memuat nama pasien/anak, jenis layanan, dan tanggal — **tidak memuat data medis/klinis** (hasil lab, diagnosis, keluhan). |
| NFR-02 | **Aksesibilitas badge & label:** badge dan label jenis layanan punya makna teks (tidak hanya warna); target sentuh menu minimal 44×44 px. |
| NFR-03 | **Konsistensi bahasa & format:** Bahasa Indonesia; format tanggal Indonesia dengan nama hari; sapaan "Ibu" untuk dewasa; tanpa penyebutan jam. |

---

## 8. Dependencies & Open Items

**Ketergantungan:**
- **Data jadwal per layanan** harus terisi dari langkah "Jadwal Kunjungan Berikutnya" pada pencatatan (epic PTS-375) — 6 alur: ANC, persalinan, nifas, KB, layanan lain, imunisasi.
- **Data keluarga terhubung** (orang tua ↔ anak) untuk nomor tujuan imunisasi.
- **Profil klinik**: **nama praktik/klinik** + **tautan lokasi Google Maps** — sumber blok penutup (tanpa nama bidan).

**Open items:**
1. Bentuk **badge pada menu** — angka saja, atau angka + titik penanda (keputusan UX).
2. Format **label jenis layanan** pada kartu daftar (singkatan `KB`/`ANC`/`Imunisasi` atau label lebih panjang).
3. Perlu tidaknya **masking nomor** pada modal pengingat.

**Iterasi berikutnya (di luar PRD ini):**
- WhatsApp Business API (kirim otomatis/terjadwal + status terkirim) · riwayat follow-up/anti-dobel · preview teks di CRM · **naskah ringkasan pemeriksaan & invoice/struk** (dokumen terpisah, PRD menyusul).

## 9. Referensi

- Prototype acuan: `docs/prototype/CRMBidan_15sep2026.html`
- Keputusan produk & hasil grill: `features/crm-bidan/context/CONTEXT.md` (§15 = audit flow jadwal per layanan)
- Naskah ringkasan & invoice: `docs/copy-deck-wa-ringkasan-invoice.md` + `.pdf`
- PRD terkait: PTS-370 (Login & Manajemen Data Pasien) · PTS-382 (Kantong Persalinan)

## 10. Riwayat Revisi

| Versi | Tanggal | Perubahan |
|---|---|---|
| v1–v1.3 | 15 Sep 2026 | Draft, finalisasi copy, push notification ditunda, naskah ringkasan & invoice dimasukkan lalu dikeluarkan. |
| v2 | 15 Sep 2026 | Disederhanakan: 22 FR → 9 FR, fokus control ANC. |
| **v3.3** | 15 Sep 2026 | **Baris `⏰ Waktu` dicabut** — keputusan produk: jadwal hanya memakai tanggal, jadi naskah cukup memuat `📅 Hari, Tanggal`. FR-13 (jam kunjungan), edge case "jam belum diisi", dan dependency field jam dihapus; struktur lima bagian tetap tidak berubah. |
| **v3.2** | 15 Sep 2026 | **Naskah pengingat diperbarui** (permintaan pemilik produk): struktur **lima bagian tetap** (salam + doa · kalimat pengantar menyebut nama praktik & jenis layanan · blok jadwal `📅 Hari, Tanggal` + `⏰ Waktu` · ajakan konfirmasi/penjadwalan ulang · penutup `Kami tunggu kedatangannya dengan senang hati. 🙏` + nama praktik + `📍` tautan lokasi). Tambahan: **FR-13 (jam kunjungan)**, **edge case 11 (jam belum diisi)**, tanggal memakai **nama bulan penuh**, doa menyesuaikan jenis layanan. |
| **v3.1** | 15 Sep 2026 | Blok penutup diubah (permintaan pemilik produk): **nama bidan dihapus**, penutup berisi **nama praktik/klinik + tautan lokasi Google Maps** (dari profil klinik) + FR-11 & edge case 10 diperbarui. |
| **v3** | 15 Sep 2026 | **Cakupan diperluas ke SEMUA jenis layanan** (permintaan pemilik produk: *"reminder ini untuk keseluruhan layanan karena kalau kamu melakukan pencatatan pemeriksaan kamu bisa jadwalkan kunjungan selanjutnya"*): (a) badge & daftar harian menghitung **semua jenis jadwal**; (b) **imunisasi masuk sumber jadwal** (sebelumnya tidak pernah dikirim pengingatnya) dengan nomor tujuan anak → fallback orang tua; (c) **halaman Jadwal Kunjungan menampilkan semua kategori pasien** (keputusan A); (d) **modal menampilkan semua jadwal** bila lebih dari satu & bidan memilih (keputusan B); (e) **naskah per jenis layanan × kondisi jadwal** (6 × 3) + varian pasien <18 tahun + pesan anak ke orang tua (keputusan C). Hasil: **5 US · 12 FR · 12 butir AC · 9 edge case · 3 NFR**. |

