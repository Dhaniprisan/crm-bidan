# CRM Bidan — Prototype &amp; Dokumen

> **Satu URL:** <https://dhaniprisan.github.io/crm-bidan/> — prototype langsung terbuka, dan tombol **📄 Dokumen & Versi** di kanan bawah berisi PRD, guidance UX, copy deck, serta versi arsip.

Kumpulan **prototype interaktif** dan **dokumen produk** untuk modul **CRM Bidan** (PrimaCare) — aplikasi web untuk praktik mandiri bidan (PMB): manajemen pasien, Kartu Layanan (ANC · Persalinan · Nifas · KB · Imunisasi · Layanan Lain), Kantong Persalinan, pengingat kunjungan via WhatsApp, kasir & harga layanan.

> Prototype ini adalah **mockup statis** (satu file HTML berisi React + Tailwind yang sudah dibundel, data disimpan di `localStorage` browser). Tidak ada backend — dipakai untuk review alur & UX, bukan aplikasi produksi.


## 🌐 Lihat langsung (GitHub Pages)

| URL | Isi |
|---|---|
| **https://dhaniprisan.github.io/crm-bidan/** | **Prototype terbaru (17 Sep 2026)** — jadwal kunjungan mandiri + kasir & struk pembayaran |
| https://dhaniprisan.github.io/crm-bidan/docs.html | Daftar dokumen: prototype semua versi, guidance UX, copy deck, PRD |
| https://dhaniprisan.github.io/crm-bidan/prototype/CRMBidan_16sep2026.html | Prototype 16 Sep — jalur kasir & harga layanan |
| https://dhaniprisan.github.io/crm-bidan/prototype/CRMBidan_16sep2026-jadwal-mandiri.html | Prototype jalur jadwal mandiri (arsip, tanpa kasir) |
| https://dhaniprisan.github.io/crm-bidan/prototype/CRMBidan_15sep2026.html | Prototype versi sebelumnya |
| https://dhaniprisan.github.io/crm-bidan/prototype/CRMBidan_14sep2026.html | Prototype arsip |

**Versi 17 Sep 2026 = hasil gabungan dua jalur pengembangan:**
- **Pengaturan & Akun (PTS-604)** — halaman Pengaturan Klinik: field baru **Tautan lokasi Google Maps** (divalidasi) + **No. HP / WhatsApp praktik**, blok **Akun** (Kode klinik & Email) yang **read-only — "dikelola lewat PrimaCare"**; menu **"Layanan Wellness" dihapus**; **nama praktik** dipakai pada header aplikasi, kop struk, dan penutup pesan WhatsApp (nama bidan tidak lagi dicantumkan pada pesan ke pasien).
- **Jadwal kunjungan mandiri** — jadwal dicatat terpisah dari pelayanan sehingga satu pasien bisa punya lebih dari satu jadwal aktif; modal *Jadwalkan / Edit / Hapus jadwal*, preset agenda kontrol (ANC rutin, KB suntik/implan, imunisasi, nifas KF1–KF4), field agenda/catatan pada langkah jadwal, dan catatan tekanan darah pada pemeriksaan.
- **Kasir & Harga Layanan** — field metode bayar (Tunai/Transfer/QRIS) + jumlah dibayar & kembalian di layar Kasir, **layar struk pembayaran** (Unduh/Bagikan PDF + Kirim WA), **nomor struk otomatis** `PMB001-YYYYMMDD-NNN`, tab **Transaksi** di profil pasien, penanda **Aktif/Nonaktif** + penghitung SKU di Harga Layanan, serta nomor struk & metode bayar pada baris Keuangan.

Struktur repo: `index.html` = prototype terbaru (agar bisa dibuka langsung), `docs.html` = daftar dokumen, `prototype/` = semua versi, `guidance-ux/`, `copy-deck/`, `prd/` = dokumen.

## Isi repo

| Folder | Isi |
|---|---|
| `prototype/` | Versi prototype: **14 Sep**, **15 Sep**, **16 Sep 2026** (terbaru = `CRMBidan_16sep2026.html`) |
| `guidance-ux/` | Guidance perubahan UX per fitur (HTML + PDF): Kantong Persalinan, Field Baru Bidan, **Kasir & Harga Layanan** |
| `copy-deck/` | Naskah pesan WhatsApp: ringkasan pemeriksaan & invoice/struk (md + html + pdf) |
| `prd/` | PRD per epic: Login & Data Pasien (PTS-367), Kantong Persalinan (PTS-374), Pengingat Kunjungan (PTS-376), Kasir & Harga Layanan (PTS-602) + dokumen pendukung |

## Cara membuka prototype

Unduh file HTML-nya lalu buka di browser (Chrome/Safari), atau:

```bash
open prototype/CRMBidan_16sep2026.html      # macOS
xdg-open prototype/CRMBidan_16sep2026.html  # Linux
```

Login prototype: pilih salah satu akun contoh pada layar masuk (mode demo, tanpa backend).

## Perubahan pada versi 16 Sep 2026 — Kasir & Harga Layanan

Mengimplementasikan rekomendasi `guidance-ux/guidance-ux-kasir-harga-layanan.pdf` ke prototype:

**1. Layar Kasir (penutup kunjungan)** — berlaku untuk 6 kartu layanan (ANC · Persalinan · Nifas · KB · Layanan Lain · Imunisasi)
- Field baru: **Metode bayar** (Tunai · Transfer · QRIS, wajib) dan **Jumlah dibayar** (terisi otomatis = total biaya)
- Baris informasi **kembalian** / **kurang dari biaya** + chip **Lunas**
- Tombol **Selesai** non-aktif sampai layanan & metode bayar terisi

**2. Layar Struk Pembayaran (baru)** — muncul setelah "Selesai"
- Struk memuat kop praktik, **nomor struk**, tanggal, pasien, rincian layanan, total, metode bayar, jumlah dibayar, kembalian
- Aksi: **Unduh / Bagikan PDF** (print-to-PDF), **Kirim WA** (teks invoice), **Lihat profil pasien**
- Nomor struk otomatis: `PMB001-YYYYMMDD-NNN` (urut per hari)

**3. Riwayat transaksi di Profil Pasien (baru)** — tab **Transaksi**
- Daftar transaksi (layanan · biaya · tanggal · metode · nomor struk · chip Lunas)
- Tap baris → struk, tersedia **Unduh PDF** & **Kirim WA** untuk kirim ulang

**4. Pengaturan → Harga Layanan** — label **Aktif/Nonaktif**, penghitung SKU per kategori, keterangan masa berlaku harga

**5. Halaman Keuangan** — baris transaksi menampilkan **nomor struk** & **metode bayar**

## Catatan

- Prototype 16 Sep menyimpan field transaksi (`nomorStruk`, `metodeBayar`, `jumlahBayar`, `statusLunas`) pada catatan layanan pasien — cukup untuk review alur, belum untuk produksi.
- Tombol **Unduh / Bagikan PDF** memakai dialog print browser (Save as PDF) karena prototype tidak punya layanan pembuat PDF.
- Dokumen internal (catatan keputusan produk, feedback narasumber, rujukan standar Kemenkes) **tidak** dimasukkan ke repo ini.

---

Dikelola oleh **Dhani Prisantika** · PrimaCare · September 2026
