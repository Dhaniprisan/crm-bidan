# Copy Deck — WA: Ringkasan Pemeriksaan & Invoice

**Untuk:** epic PTS-376 (WA Follow-up 1-Click) — bagian **ringkasan layanan** + **invoice**
**Versi:** v1 · **Tanggal:** 15 Sep 2026 · **Penyusun:** Jo (AI Assistant) untuk Dhani Prisantika
**Acuan copy eksisting:** generator ringkasan di prototype `CRMBidan_15sep2026.html` (6 jenis layanan) — dirapikan & disaring pada dokumen ini.

---

## 1. Aturan umum (berlaku untuk semua pesan)

| No. | Aturan |
|---|---|
| A1 | **Kanal:** deep link `wa.me` — pesan terisi otomatis, bidan masih boleh menyunting di WhatsApp sebelum kirim. |
| A2 | **Tanda tangan seragam:** `— <nama bidan>, <nama praktik/klinik>`. Bila nama praktik kosong → `Praktik Mandiri Bidan`. |
| A3 | **Format tanggal:** Indonesia + nama hari untuk jadwal mendatang (mis. `Sabtu, 20 Sep 2026`); tanggal kejadian lampau tanpa nama hari (`12 Agu 2026`). **Jam tidak disertakan** (data kunjungan hanya tanggal). |
| A4 | **Sapaan:** nama pasien tanpa gelar (`Halo Siti,` pada pengingat; pada ringkasan langsung menyebut nama di judul). Sapaan **"Ibu"** untuk pasien dewasa; pasien **<16 tahun** memakai bentuk "kamu/-mu". |
| A5 | **Baris kosong dikosongkan otomatis:** field yang tidak diisi bidan **tidak dikirim** (tidak ada baris "-" atau "Tidak ada"). |
| A6 | **Bahasa:** hangat, singkat, tanpa istilah medis rumit; istilah yang dipakai harus sama dengan yang tertulis di aplikasi. |
| A7 | **Kalimat privasi** (wajib di semua ringkasan): *"Pesan ini berisi data pemeriksaan Ibu. Mohon tidak dibagikan ke pihak lain."* |
| A8 | **Penyaring data sensitif** (lihat bagian 2) — hasil skrining menular dan angka laboratorium **tidak dikirim**, diganti kalimat arahan. |

---

## 2. Penyaring data sensitif — apa yang DIBUANG

**Prinsip:** pesan ke WhatsApp memuat informasi yang **berguna untuk pasien** dan **tidak berisiko** bila terbaca orang lain (nomor tertukar, HP dipinjam, layar terlihat). Hasil skrining menular dan angka laboratorium **tidak dikirim** — pasien bisa menanyakannya langsung ke bidan.

| Field pada copy lama | Keputusan | Alasan |
|---|---|---|
| Triple Eliminasi (HIV / Sifilis / HepB) | **DIBUANG** | Skrining menular — risiko stigma & privasi tertinggi bila nomor/HP salah. |
| Hb, protein urine, gula darah | **DIBUANG** | Angka laboratorium → dipindah ke kalimat arahan. |
| Kondisi lokhia, involusi, suhu (nifas) | **DIPERTAHANKAN** | Bagian pemulihan pasien sendiri, berguna untuk pemantauan mandiri. |
| Tanda bahaya (nifas) | **DIPERTAHANKAN (wajib)** | Informasi keselamatan — justru harus sampai ke pasien. |
| APGAR, perkiraan perdarahan (persalinan) | **DIBUANG** | Detail klinis yang berpotensi membuat cemas tanpa bisa dijelaskan lewat teks. |
| Partus / Abortus (nifas) | **DIBUANG** | Riwayat obstetri sensitif; tidak menambah nilai bagi pasien. |
| Berat/panjang bayi, IMD, Vit K1, HB0 | **DIPERTAHANKAN** | Informasi kelahiran yang dinanti keluarga. |
| Metode KB, jadwal ulang KB | **DIPERTAHANKAN** | Inti tujuan pesan (pengingat kontrol KB). |
| Keluhan & catatan bidan | **DIPERTAHANKAN, tapi dibatasi** | Bagian personal bidan; bidan boleh menghapus sebelum kirim (pesan bisa diedit di WA). |
| Biaya / total pembayaran | **DIPERTAHANKAN** | Banyak bidan memakai WA untuk konfirmasi pembayaran. Bidan bisa hapus bila tidak ingin menampilkan harga. |
| Kalimat arahan pengganti hasil lab | **DITAMBAH** | *"Hasil pemeriksaan laboratorium dapat ditanyakan langsung ke bidan ya."* |

---

## 3. Ringkasan Pemeriksaan — copy per layanan

### 3.1 Kunjungan ANC (kehamilan)

```
Ringkasan Pemeriksaan Kehamilan — PMB Ratna Sejahtera

Halo Siti, ini ringkasan pemeriksaan Ibu hari ini.

Tanggal: Sabtu, 12 Sep 2026
Usia kehamilan: 36 minggu 4 hari (Trimester 3)
Tekanan darah: 110/70 mmHg
Berat badan: 62 kg
LiLA: 23,5 cm
TFU: 30 cm
DJJ: 140 bpm
Imunisasi TT/Td: sudah lengkap
Tablet Tambah Darah: diberikan
USG: dilakukan — posisi janin kepala, air ketuban cukup

Keluhan: pegal di pinggang
Catatan bidan: perbanyak istirahat, kurangi aktivitas berat

Hasil pemeriksaan laboratorium dapat ditanyakan langsung ke bidan ya.
Jadwal kontrol berikutnya: Sabtu, 26 Sep 2026

Layanan: ANC Reguler
Biaya: Rp 50.000

Pesan ini berisi data pemeriksaan Ibu. Mohon tidak dibagikan ke pihak lain.

— Ratna, PMB Ratna Sejahtera
```

**Perubahan dari copy lama:** Triple Eliminasi, Hb, protein urine, dan gula darah **dihapus**; **ditambahkan** usia kehamilan + trimester, kalimat arahan lab, kalimat privasi, dan nama praktik pada tanda tangan.

### 3.2 Kunjungan Nifas

```
Ringkasan Kunjungan Nifas — PMB Ratna Sejahtera

Halo Siti, ini ringkasan kunjungan nifas Ibu hari ini ya.

Tanggal: Senin, 14 Sep 2026
Tipe kunjungan: Kunjungan Nifas 2 (KF2)
Tanggal bersalin: 8 Sep 2026
Tekanan darah: 110/70 mmHg
Suhu tubuh: 36,8 °C
Kondisi lokhia: normal (rubra)
Involusi uterus: baik
Kondisi ASI/menyusui: lancar, bayi menyusu kuat

Keluhan: belum ada
Catatan bidan: tambah asupan protein & cairan ya, Bu

Yang perlu segera dihubungi bidan: perdarahan banyak, demam tinggi,
nyeri kepala hebat, atau pandangan kabur.

Jadwal kunjungan berikutnya: Senin, 21 Sep 2026

Layanan: Kunjungan Nifas
Biaya: Rp 75.000

Pesan ini berisi data pemeriksaan Ibu. Mohon tidak dibagikan ke pihak lain.

— Ratna, PMB Ratna Sejahtera
```

**Perubahan dari copy lama:** Partus/Abortus **dihapus**; kalimat **tanda bahaya** dirapikan jadi kalimat lengkap yang bisa dipahami; ditambah kalimat privasi & nama praktik.

### 3.3 Persalinan

```
Ringkasan Persalinan — PMB Ratna Sejahtera

Selamat ya Bu Siti! Ini ringkasan persalinan Ibu.

Tanggal: Selasa, 8 Sep 2026
Jenis persalinan: Persalinan normal
Tempat: PMB Ratna Sejahtera
Penolong: Bidan Ratna
Kondisi ibu: baik
Kondisi bayi: baik

Data bayi:
Bayi 1: perempuan, 3.100 g, 49 cm
Tanda perawatan: IMD, Vit K1, salep mata, HB0

Bayi sudah otomatis terdaftar sebagai pasien & punya jadwal imunisasi dasar
di aplikasi. Imunisasi pertama (HB0) sebaiknya segera setelah lahir ya.

Catatan bidan: ibu dan bayi sehat, kontrol nifas 6 hari setelah bersalin.

Biaya: Rp 1.500.000

Pesan ini berisi data pemeriksaan Ibu. Mohon tidak dibagikan ke pihak lain.

— Ratna, PMB Ratna Sejahtera
```

**Perubahan dari copy lama:** APGAR & perkiraan perdarahan **dihapus**; ditambah ucapan selamat, ringkasan data bayi yang bisa dipahami, dan penjelasan otomatisnya pendaftaran bayi + jadwal imunisasi.

### 3.4 Layanan KB

```
Ringkasan Layanan KB — PMB Ratna Sejahtera

Halo Siti, ini catatan layanan KB Ibu ya.

Tanggal: Kamis, 10 Sep 2026
Metode: KB Suntik 3 bulan
Tekanan darah: 110/70 mmHg
Berat badan: 58 kg
Keluhan efek samping: tidak ada
Catatan bidan: tidak ada keluhan, lanjutkan sesuai jadwal berikutnya.

Jadwal kontrol/ulang berikutnya: Kamis, 10 Des 2026

Layanan: KB Suntik 3 bulan
Biaya: Rp 100.000

Pesan ini berisi data pemeriksaan Ibu. Mohon tidak dibagikan ke pihak lain.

— Ratna, PMB Ratna Sejahtera
```

**Perubahan dari copy lama:** judul diseragamkan (dulu *"Info KB"* → **"Ringkasan Layanan KB"**) + kalimat privasi & nama praktik.

### 3.5 Layanan Lain (Wellness)

```
Ringkasan Layanan — PMB Ratna Sejahtera

Halo Siti, ini catatan layanan Ibu hari ini ya.

Tanggal: Senin, 14 Sep 2026
Layanan: Pemeriksaan kesehatan umum
Catatan bidan: kondisi baik, disarankan kontrol rutin 6 bulan lagi.

Layanan: Pemeriksaan kesehatan umum
Biaya: Rp 75.000

Pesan ini berisi data pemeriksaan Ibu. Mohon tidak dibagikan ke pihak lain.

— Ratna, PMB Ratna Sejahtera
```

### 3.6 Imunisasi Anak (dikirim ke nomor orang tua)

```
Ringkasan Imunisasi — PMB Ratna Sejahtera

Halo Ibu Siti, ini ringkasan imunisasi Dinda hari ini ya.

Tanggal kunjungan: Senin, 14 Sep 2026
Nama anak: Salma Nuraini
Imunisasi diberikan: BCG, Polio 1
Berat/Tinggi badan: 4,2 kg / 54 cm
Gejala setelah imunisasi: demam ringan (reda dalam 1 hari)
Catatan bidan: kompres hangat bila demam, cukupkan ASI.

Imunisasi selanjutnya: DPT-HB-Hib 1 — Sabtu, 14 Nov 2026

Layanan: Imunisasi Dasar
Biaya: Rp 150.000

Pesan ini berisi data kesehatan anak Ibu. Mohon tidak dibagikan ke pihak lain.

— Ratna, PMB Ratna Sejahtera
```

**Catatan:** pesan anak **dialamatkan ke orang tua** ("Halo Ibu Siti"), bukan ke anak. Nomor tujuan = No. HP anak bila ada, jika tidak → **No. HP orang tua** (perilaku yang sudah dipakai prototype).

---

## 4. Invoice / Struk Pembayaran via WA (copy baru)

### 4.1 Aturan
- **Nomor struk:** `<kode klinik>-<YYYYMMDD>-<nomor urut 3 digit>` (mis. `PMB001-20260916-003`) — dibuat otomatis saat transaksi disimpan, urut per klinik per hari.
- **Kapan dikirim:** dari halaman **Kasir**, setelah bidan menekan **Selesai** (transaksi tersimpan) — tombol *Kirim struk ke WhatsApp*.
- **Bisa dikirim ulang** kapan pun dari riwayat kunjungan (tanpa batas waktu).
- **Tanpa lampiran file** — `wa.me` hanya bisa membawa teks; struk dikirim sebagai teks WhatsApp.
- **Boleh disunting bidan** di WhatsApp (sama seperti pengingat).
- Bila klinik tidak ingin menampilkan harga, bidan dapat menghapus baris biaya sebelum mengirim.
- **Satu layanan per transaksi** — rincian struk memuat satu layanan (bukan daftar panjang).
- **Pembayaran selalu lunas** pada rilis ini; baris "Dibayar / Sisa" hanya disiapkan untuk iterasi pelunasan bertahap (belum dipakai).
- **Tanda tangan pesan** memakai nama praktik + tautan lokasi Google Maps (tanpa nama bidan), sama seperti pesan pengingat.

### 4.2 Struk — pembayaran LUNAS (dipakai)

```
Salam sehat, Ibu Siti.
Terima kasih sudah berkunjung ke PMB Ratna Sejahtera hari ini.

Berikut rincian pembayaran kunjungan Ibu:
🧾 No. Struk: PMB001-20260916-003
📅 Tanggal: Rabu, 16 September 2026
💉 Layanan: ANC Reguler
💰 Total: Rp 50.000
💳 Metode bayar: Tunai

Struk lengkap (PDF) bisa kami kirimkan bila diperlukan ya.

Semoga Ibu dan calon buah hati selalu sehat. 🙏

PMB Ratna Sejahtera
📍 https://maps.app.goo.gl/pmb-ratna-sejahtera
```

### 4.3 Struk — BELUM LUNAS (belum dipakai)

> **Belum dipakai pada rilis ini.** Transaksi CRM Bidan selalu lunas (PRD PTS-614 · FR-07). Naskah ini disiapkan untuk iterasi **pelunasan bertahap/cicilan** berikutnya.

```
Salam sehat, Ibu Siti.
Kami mencatat pembayaran kunjungan Ibu pada Rabu, 16 September 2026:
🧾 No. Struk: PMB001-20260916-003
💰 Total: Rp 150.000
✅ Dibayar: Rp 100.000
⏳ Sisa: Rp 50.000

Mohon informasi bila ingin dilunasi ya. Terima kasih 🙏

PMB Ratna Sejahtera
📍 https://maps.app.goo.gl/pmb-ratna-sejahtera
```

### 4.4 Struk — kirim ulang (versi ringkas)

```
Salam sehat, Ibu Siti.
Berikut kami kirimkan kembali rincian pembayaran kunjungan Ibu pada Rabu, 16 September 2026:
🧾 No. Struk: PMB001-20260916-003
💰 Total: Rp 50.000 (Tunai)

Bila ada yang ingin ditanyakan, silakan balas pesan ini ya.

PMB Ratna Sejahtera
📍 https://maps.app.goo.gl/pmb-ratna-sejahtera
```

## 5. Catatan untuk spek & teknis

| No. | Catatan |
|---|---|
| C1 | **Halaman Kasir perlu 3 field baru** agar struk bisa lengkap: **metode bayar** (Tunai · Transfer · QRIS), **status lunas** (default "Lunas"), dan **nomor struk**. **Jumlah dibayar & kembalian tidak dipakai** karena transaksi selalu lunas (PRD PTS-614 · FR-07). |
| C2 | ~~**Halaman struk belum ada** di prototype~~ **Sudah ada** — layar struk + unduh PDF + kirim WA sudah tersedia di prototype 17 Sep 2026. — perlu dibuat layar konfirmasi setelah "Selesai" yang memuat tombol *Kirim struk ke WhatsApp* + *Lihat struk*. |
| C3 | **Rincian multi-layanan** mengikuti layanan yang dicatat pada transaksi; bila hanya satu layanan, baris "Rincian" tidak perlu (langsung "Layanan: …"). |
| C4 | **Ringkasan pemeriksaan** dikirim dari: (a) tombol pada baris riwayat layanan di Profil Pasien, (b) tombol pada layar ringkasan sukses setelah mencatat layanan. |
| C5 | **Semua pesan memakai template baru** pada dokumen ini — copy lama di prototype (dengan data lab & skrining menular) **diganti**. |
| C6 | Pesan ringkasan **tidak memuat jam** (data kunjungan hanya tanggal). |

## 6. Riwayat Revisi

| Versi | Tanggal | Perubahan |
|---|---|---|
| v2 | 17 Sep 2026 | Disamakan dengan keputusan Kasir & Harga Layanan (PRD PTS-614): nomor struk `<kode klinik>-<yyyymmdd>-<3 digit>`, satu layanan per transaksi, pembayaran selalu lunas (baris Dibayar/Sisa jadi naskah iterasi berikutnya), tanda tangan pesan memakai nama praktik + tautan lokasi, field kasir 4 → 3, layar struk ditandai sudah ada. Naskah 4.2–4.4 disamakan dengan PRD 5.2–5.4. |
| v1 | 15 Sep 2026 | Copy deck awal: penyaring data sensitif (10 keputusan field), 6 copy ringkasan (ANC, nifas, persalinan, KB, wellness, imunisasi), 3 copy invoice (lunas, belum lunas, kirim ulang), catatan spek & teknis. |

