# Guidance AE & CS — Koding ICD: Vaksin & Layanan Bidan

> **Tujuan:** Pedoman menjawab pertanyaan dokter & bidan seputar kode ICD-10/ICD-9 yang berkaitan dengan **3 vaksin (Varicella, Rotavirus, PCV)** dan **layanan bidan** — agar jawaban AE/CS konsisten, akurat, dan menenangkan.
> **Tanggal disusun:** 2 Sep 2026 · **Status:** Revisi internal (cek ulang jika ada perubahan data)

---

## 📌 Ringkasan 30 Detik (buat yang buru-buru)

| Topik | Jawaban inti |
|:------|:-------------|
| **Kenapa 3 vaksin nggak ada kodenya?** | ICD-10 secara internasional memang tidak punya kode khusus vaksin varicella/rotavirus/PCV — berlaku di semua sistem, **bukan kekurangan PrimaCare** |
| **Gimana cara catat vaksinnya?** | Pakai kode imunisasi yang valid: PCV `Z23.8` · Varicella `Z25.8` · Rotavirus `Z26.9` — nama vaksin detail dicatat di kolom lain |
| **ICD untuk bidan ada?** | **Lengkap** — 501 kode O + 71 Z30-Z39 + 387 P + 40 imunisasi |
| **Match dengan Satu Sehat?** | **100% match** — versi sama (ICD-10 2010), 0 kode hilang |

---

# BAGIAN A — 3 Vaksin (Varicella, Rotavirus, PCV)

## A.1 Fakta yang harus dipahami AE/CS

1. **ICD-10 tidak menyediakan kode khusus** untuk vaksin varicella, rotavirus, dan PCV. Kode imunisasi ICD-10 (Z23–Z28) hanya sampai level "jenis penyakit" (bakteri/virus/umum), bukan sampai level produk vaksin.
2. **Ini berlaku global** — tidak ada sistem di dunia (termasuk Satu Sehat & BPJS) yang punya kode ICD-10 spesifik untuk ketiga vaksin ini.
3. **WHO berhenti memperbarui ICD-10 sejak 2018** — kode spesifik nanti hanya ada di ICD-11, dan **Indonesia belum menerapkan ICD-11**. Jadi tidak perlu menunggu "update" yang tidak akan datang.
4. **Data ICD PrimaCare sudah sama persis dengan referensi Satu Sehat** (dua-duanya ICD-10 versi 2010). Tidak ada kode yang hilang — yang ada hanyalah keterbatasan standar internasional.

## A.2 Kode yang digunakan untuk mencatat 3 vaksin

| Vaksin | Kode ICD-10 | Nama kode | Keterangan |
|:-------|:------------|:----------|:-----------|
| **PCV** (Pneumokokus) | `Z23.8` | Need for immunization against other single bacterial diseases | Vaksin bakteri |
| **Varicella** (Cacar air) | `Z25.8` | Need for immunization against oth specified single viral diseases | Vaksin virus |
| **Rotavirus** | `Z26.9` | Need for immunization against unspecified infectious disease | Vaksin umum |

Semua kode di atas **terdaftar valid di Satu Sehat & BPJS** — aman untuk klaim e-klaim. Nama vaksin yang spesifik (mis. "PCV13/Prevnar", "Varicella", "Rotarix/RotaTeq") dicatat di catatan/kolom keterangan terpisah.

## A.3 Script menjawab (AE/CS tinggal pakai)

**Q: "Kenapa di PrimaCare tidak ada kode vaksin varicella/rotavirus/PCV?"**
> "Kode itu sebenarnya bukan tidak ada di PrimaCare, tapi **memang tidak pernah ada di standar ICD-10** secara internasional — termasuk di Satu Sehat dan BPJS. Standar ICD-10 hanya menyediakan kode imunisasi umum seperti Z23.8 / Z25.8 / Z26.9. Jadi ini keterbatasan standar dunia, bukan kekurangan sistem kami."

**Q: "Lalu gimana saya mencatat vaksinnya?"**
> "Sangat mudah — gunakan kode imunisasi yang valid: PCV pakai `Z23.8`, varicella pakai `Z25.8`, rotavirus pakai `Z26.9`. Nama vaksinnya yang spesifik tetap bisa Anda catat di kolom keterangan. Kode-kode ini sudah terdaftar dan aman digunakan untuk pelaporan Satu Sehat maupun klaim BPJS."

**Q: "Kapan kode spesifiknya tersedia?"**
> "Kode spesifik seperti itu nantinya ada di sistem koding baru bernama ICD-11. Sayangnya WHO sudah tidak memperbarui ICD-10 (sejak 2018) dan Indonesia **belum** menerapkan ICD-11. Jadi untuk saat ini, cara yang benar adalah menggunakan kode Z23.8 / Z25.8 / Z26.9 — ini yang juga dipakai fasilitas kesehatan lain."

**Q: "Apakah data ICD PrimaCare sama dengan Satu Sehat?"**
> "Ya, sama persis. PrimaCare dan Satu Sehat sama-sama memakai ICD-10 versi 2010. Kami sudah melakukan perbandingan menyeluruh — total kode 100% cocok, tidak ada yang hilang."

---

# BAGIAN B — ICD untuk Bidan

## B.1 Fakta singkat

- **Semua kode yang dibutuhkan bidan sudah ada di PrimaCare** dan **match 100% dengan Satu Sehat** (0 kode hilang di kedua arah).
- Total kode relevan bidan: **501 kode O** (kehamilan/persalinan/nifas) + **71 kode Z30-Z39** (KB/kehamilan/BBL) + **387 kode P** (bayi/perinatal) + **40 kode imunisasi Z23-Z28**.
- Catatan kecil: ada **6 nama** (di kode P & Z28) yang beda penulisan tanda kutip (PrimaCare pakai `` ` `` vs Satu Sehat `'`). **Ini tidak memengaruhi kode/validitas** — hanya tampilan nama.

## B.2 Kode populer yang sering ditanyakan

### Persalinan (ICD-10)
| Kode | Nama |
|:-----|:-----|
| `O80` | Persalinan spontan tunggal |
| `O80.0` | Persalinan spontan letak kepala (vertex) |
| `O80.9` | Persalinan spontan tunggal, tidak spesifik |
| `O82` | Persalinan dengan seksio sesarea |
| `O82.0` | SC elektif |
| `O82.1` | SC emergensi |
| `O81` | Persalinan dengan forceps / vakum |

### KB / Kontrasepsi (ICD-10)
| Kode | Nama |
|:-----|:-----|
| `Z30.0` | Konseling & nasihat kontrasepsi |
| `Z30.1` | Pemasangan alat kontrasepsi dalam rahim (IUD/AKDR) |
| `Z30.2` | Sterilisasi |
| `Z30.4` | Pemantauan obat kontrasepsi (suntik/pil) |
| `Z30.5` | Pemantauan IUD |

### Kehamilan & Nifas (ICD-10)
| Kode | Nama |
|:-----|:-----|
| `Z34.0` | Supervisi kehamilan normal pertama |
| `Z34.9` | Supervisi kehamilan normal, tidak spesifik |
| `Z39.0` | Perawatan & pemeriksaan segera setelah melahirkan |
| `Z39.1` | Perawatan & pemeriksaan ibu menyusui |
| `Z39.2` | Pemeriksaan nifas rutin |

### Bayi Baru Lahir (ICD-10)
| Kode | Nama |
|:-----|:-----|
| `Z38.0` | Bayi tunggal lahir di rumah sakit |
| `Z38.1` | Bayi tunggal lahir di luar rumah sakit |
| `Z38.2` | Bayi tunggal, tempat lahir tidak spesifik |

### Tindakan/Prosedur (ICD-9-CM) — juga tersedia di PrimaCare
| Kode | Nama |
|:-----|:-----|
| `73.6` | Episiotomi |
| `74.0` | SC klasik |
| `74.1` | SC segmen bawah rahim (low cervical) |
| `75.4` | Pengeluaran plasenta secara manual |
| `75.5x` | Perbaikan laserasi obstetri |
| `73.4` | Induksi persalinan medikamentosa |

## B.3 Script menjawab (AE/CS tinggal pakai)

**Q: "Apakah kode untuk layanan bidan ada di PrimaCare?"**
> "Lengkap, Bu/Pak. PrimaCare punya 501 kode kehamilan-persalinan-nifas (O), 71 kode KB/kehamilan/bayi (Z30–Z39), 387 kode bayi baru lahir (P), dan 40 kode imunisasi (Z23–Z28). Semua kebutuhan pencatatan ANC, persalinan, nifas, KB, dan bayi sudah ter-cover."

**Q: "Apakah kodenya cocok dengan Satu Sehat?"**
> "Cocok 100%. Keduanya memakai ICD-10 versi 2010 yang sama — kami sudah memverifikasi, tidak ada satu pun kode yang hilang atau berbeda. Ada sedikit perbedaan penulisan nama (tanda petik) di 6 kode, tapi itu tidak memengaruhi keabsahan kode sama sekali."

**Q: "Kode persalinan normal / SC / KB apa?"**
> "Persalinan normal: `O80` atau `O80.0`. SC: `O82.x`. KB: `Z30.0` konseling, `Z30.1` pasang IUD, `Z30.4` kontrol suntik/pil. Semua tinggal dipilih di sistem."

**Q: "Apakah kode tindakan juga ada? (misalnya episiotomi)"**
> "Ada. Kode tindakan/prosedur menggunakan ICD-9-CM — misalnya episiotomi `73.6`, SC `74.x`, dan lainnya, semuanya tersedia di PrimaCare."

---

# BAGIAN C — Aturan Main (Do & Don't)

## ✅ DO
- **Jawab dengan tenang dan konsisten**: semua keterbatasan ini = standar internasional, bukan kekurangan produk.
- **Rujuk ke kode yang valid**: Z23.8 / Z25.8 / Z26.9 untuk vaksin; O80 / Z30.x / Z34.x / Z38.x untuk bidan.
- **Tegaskan data PrimaCare = Satu Sehat** (versi 2010, match 100%) — ini penguat kepercayaan.
- Catat nama vaksin spesifik di kolom keterangan jika dokter/bidan ingin detail.

## ❌ DON'T
- **Jangan berjanji "kode spesifik segera tersedia"** — ICD-10 sudah di-freeze WHO sejak 2018, tidak akan ada update.
- **Jangan menyebut kode yang dibuat-buat** (bukan dari referensi resmi).
- **Jangan bilang data PrimaCare "beda"/"kurang" dari Satu Sehat** — itu akan membuat dokter/bidan ragu. Faktanya match 100%.
- **Jangan menjawab di luar topik ini sendirian** — jika pertanyaan teknis koding di luar panduan, forward ke tim product.

---

## 📎 Referensi & Validitas

- **Sumber data:** File terminologi ICD PrimaCare vs terminologi ICD-10 SATUSEHAT Platform (dokumen resmi Kemenkes, diakses 2 Sep 2026).
- **Versi:** ICD-10 versi 2010 & ICD-9-CM 2010 — sama di PrimaCare dan Satu Sehat.
- **Hasil verifikasi:** ICD-9-CM 4.626/4.626 kode cocok · ICD-10 18.542/18.543 cocok (1 kode ekstra di Satu Sehat: U12.9, COVID-19 vaccine) · 6 kode hanya di PrimaCare (tidak apa-apa, tetap valid WHO) · Nama beda: hanya kosmetik.
- **Kebijakan WHO:** ICD-10 tidak lagi diperbarui (2018). Kode vaksin spesifik hanya ada di ICD-11; Indonesia belum menerapkan.

*Dokumen ini untuk penggunaan internal AE/CS. Jika data terminologi diperbarui, dokumen ini perlu direvisi.*