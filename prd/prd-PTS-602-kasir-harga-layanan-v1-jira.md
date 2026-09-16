## 1. Overview & Scope Boundary


**Yang dibangun:** penutup kunjungan (kasir) untuk praktik mandiri bidan — bidan mencatat layanan yang diberikan beserta biayanya, menutup kunjungan, lalu pasien menerima **struk pembayaran (PDF)** dan **ringkasan pembayaran via WhatsApp**. Harga layanan diatur sekali di halaman **Pengaturan → Harga Layanan** (daftar SKU per kategori) sehingga biaya otomatis terisi saat mencatat kunjungan.

**Untuk siapa:** bidan pemilik praktik mandiri (PMB) yang mengelola transaksi harian sendiri.

**Masalah:**
1. Kunjungan sudah tercatat di rekam medis, tetapi **pembayaran tidak tercatat sama sekali** — tidak ada metode bayar, tidak ada bukti bayar, tidak ada nomor transaksi.
2. Biaya diketik manual setiap kunjungan sehingga **rawan tidak konsisten** dan tidak ada daftar harga baku.
3. Pasien tidak menerima **bukti pembayaran**, padahal bidan kerap diminta rincian biaya setelah pulang.

**Di dalam cakupan:**
- **Layar Kasir** sebagai penutup kunjungan, dapat dibuka dari **6 kartu layanan** (ANC · Persalinan · Nifas · KB · Layanan Lain · Imunisasi): pilih layanan dari SKU aktif, biaya terisi otomatis dari harga SKU (tetap bisa diubah), aksi **Selesai** / **Lewati**.
- **Empat field pembayaran baru:** metode bayar (Tunai · Transfer · QRIS), status lunas (default "Lunas"), jumlah dibayar, dan **nomor struk otomatis** (`<kode klinik>-<yyyymmdd>-<nomor urut>`).
- **Struk pembayaran PDF** yang dapat diunduh/dibagikan, plus **kirim ringkasan pembayaran via WhatsApp** (teks invoice) ke nomor pasien.
- **Halaman Pengaturan → Harga Layanan:** kelola SKU & harga per kategori (tambah, ubah harga, aktif/nonaktif, hapus).
- Transaksi menjadi **sumber data rekap pemasukan** di halaman Keuangan.

**Di luar cakupan (rilis ini):**
- **Multi-layanan dalam satu transaksi** (lebih dari satu item + subtotal) — dicatat sebagai **iterasi berikutnya**.
- **Diskon / potongan harga**.
- **Pelunasan bertahap / cicilan:** field status lunas tetap disimpan (default "Lunas") sebagai kesiapan, tetapi **belum ada alur mengubah status**.
- **Sinkronisasi master harga & transaksi ke PrimaCare** — rilis ini **mandiri di CRM Bidan**.
- **Cetak struk ke printer thermal** dan **kirim PDF sebagai lampiran otomatis** (deep link `wa.me` tidak dapat melampirkan file → PDF dibagikan lewat share sheet perangkat).
- **Invoice "belum lunas"** — naskah sudah disiapkan, dipakai setelah alur pelunasan ada.

---

## 2. User Stories


| ID | User Story | Prioritas |
|---|---|---|
| US-01 | Sebagai bidan, saya ingin **menutup kunjungan dengan mencatat layanan & biayanya**, supaya pembayaran pasien tercatat rapi. | Must Have |
| US-02 | Sebagai bidan, saya ingin **biaya terisi otomatis dari daftar harga yang saya atur**, supaya tidak mengetik ulang dan harga tetap konsisten. | Must Have |
| US-03 | Sebagai bidan, saya ingin **mengatur daftar SKU & harga tiap kategori layanan**, supaya harga bisa berubah tanpa mengubah alur pencatatan. | Must Have |
| US-04 | Sebagai bidan, saya ingin **membuka dan mengirim struk pembayaran** (PDF + ringkasan WA), supaya pasien punya bukti bayar. | Must Have |
| US-05 | Sebagai bidan, saya ingin **nomor struk dibuat otomatis**, supaya transaksi mudah dilacak saat pasien menanyakan ulang. | Must Have |
| US-06 | Sebagai bidan, saya ingin **melewati kasir tanpa transaksi**, supaya kunjungan yang tidak berbayar tetap bisa ditutup tanpa mengotori data keuangan. | Should Have |

## 3. Functional Requirements


### Layar Kasir (penutup kunjungan)

| ID | Requirement |
|---|---|
| FR-01 | Layar Kasir dapat dibuka dari **layar ringkasan sukses setelah catatan layanan disimpan** melalui tombol **"Lanjut ke Kasir"**, berlaku untuk **6 kartu layanan**: ANC, Persalinan, Nifas, KB, Layanan Lain, dan Imunisasi. |
| FR-02 | Header layar memuat label **"Kasir"**, nama pasien (untuk imunisasi memakai **nama anak**), dan deskripsi *"Pilih layanan yang diberikan & konfirmasi biayanya sebelum menyelesaikan kunjungan."* |
| FR-03 | Layar menampilkan peringatan: **"Transaksi tidak bisa diedit lagi setelah disimpan. Pastikan layanan & biaya sudah benar."** |
| FR-04 | Field **Layanan** berupa pemilih berisi **SKU aktif sesuai kategori layanan pasien**, dengan format opsi `<nama layanan> — Rp <harga>`, disertai keterangan *"Harga otomatis terisi dari pengaturan, tetap bisa diubah manual."* |
| FR-05 | Field **Biaya (Rp)** terisi otomatis dari harga SKU terpilih, tetap dapat diubah manual, dan tidak menerima nilai negatif. |
| FR-06 | Field **Metode bayar** (baru) berupa pemilih **Tunai · Transfer · QRIS** dan wajib diisi sebelum transaksi disimpan. |
| FR-07 | Field **Jumlah dibayar (Rp)** (baru) terisi otomatis sebesar total biaya. Bila diubah, layar menampilkan informasi **kembalian** (bila lebih besar) atau **selisih kurang** (bila lebih kecil); keduanya **tidak memblokir** penyimpanan karena pelunasan bertahap belum termasuk cakupan. |
| FR-08 | Field **Status lunas** (baru) tersimpan dengan nilai default **"Lunas"**; belum ada alur untuk mengubah status pada rilis ini. |
| FR-09 | **Nomor struk** (baru) dibuat otomatis dengan format **`<kode klinik>-<yyyymmdd>-<nomor urut 3 digit>`** (contoh `PMB001-20260916-003`), urut per klinik per hari, dan tidak dapat diedit bidan. |
| FR-10 | Aksi **"Selesai"** menyimpan transaksi lalu membuka **layar struk**; aksi **"Lewati"** menutup kunjungan tanpa transaksi dan mengembalikan bidan ke tab layanan pasien, tanpa menambah data keuangan. |

### Struk & pengiriman

| ID | Requirement |
|---|---|
| FR-11 | Layar struk menampilkan: nama praktik & lokasi, judul **"Struk Pembayaran"**, **nomor struk**, tanggal & waktu transaksi, nama pasien (dan No. RM bila ada), rincian layanan, **total biaya**, **metode bayar**, **jumlah dibayar**, serta kembalian bila ada. |
| FR-12 | Struk dapat **diunduh/dibagikan sebagai file PDF** (berbagi memakai share sheet perangkat sehingga bidan dapat memilih WhatsApp sendiri). |
| FR-13 | Layar struk menyediakan tombol **"Kirim WA"** yang membuka WhatsApp berisi **teks ringkasan pembayaran** (nomor struk, rincian, total, metode bayar) ke **No. HP pasien**; tombol tidak tampil bila nomor HP kosong. |
| FR-14 | Struk dapat dibuka ulang dari **riwayat transaksi pada Profil Pasien**, sehingga bidan dapat mengirim ulang kapan saja. |

### Harga Layanan (pengaturan)

| ID | Requirement |
|---|---|
| FR-15 | Halaman **Pengaturan → Harga Layanan** memuat pemilih **kategori layanan**: ANC · Persalinan · Nifas · KB · Imunisasi · Layanan Lain, dengan deskripsi *"Atur SKU & harga default tiap layanan. Harga ini jadi pilihan di halaman Kasir…"* |
| FR-16 | Bidan dapat **menambah SKU** (nama layanan + harga) ke kategori yang sedang dipilih. |
| FR-17 | Daftar SKU per kategori menampilkan nama & harga, dengan aksi **ubah harga langsung di daftar**, **aktif/nonaktifkan SKU**, dan **hapus SKU** disertai konfirmasi *"Hapus SKU?"*. |
| FR-18 | Halaman menampilkan catatan: *"SKU yang aktif akan muncul sebagai pilihan di halaman Kasir saat mencatat kunjungan untuk kategori ini."* |
| FR-19 | Bila kategori belum punya SKU, tampil keadaan kosong *"Belum ada SKU untuk kategori ini. Tambahkan lewat kolom di atas."* |
| FR-20 | SKU nonaktif tidak muncul sebagai pilihan di layar Kasir, namun transaksi lama yang memakainya tetap menampilkan nama & biaya yang tersimpan. |

### Data & keterkaitan modul

| ID | Requirement |
|---|---|
| FR-21 | Transaksi tersimpan **per pasien dan per kunjungan**, memuat seluruh field pembayaran (metode, jumlah dibayar, status lunas, nomor struk) beserta layanan & biaya. |
| FR-22 | Data transaksi menjadi **sumber rekap pemasukan** di halaman Keuangan (epic PTS-603) dan dasar perhitungan laporan. |
| FR-23 | Master SKU/harga dan transaksi **dikelola mandiri di CRM Bidan** pada rilis ini; belum ada sinkronisasi harga/transaksi ke PrimaCare. |

---

## 4. Acceptance Criteria


**US-01 — Mencatat layanan & biaya saat menutup kunjungan**
- **Given** bidan baru menyimpan catatan layanan (mis. ANC), **When** bidan menekan **"Lanjut ke Kasir"**, **Then** layar Kasir terbuka dengan nama pasien dan pilihan layanan sesuai kategori.
- **Given** layar Kasir terbuka, **When** bidan memilih layanan lalu menekan **"Selesai"**, **Then** transaksi tersimpan dan **layar struk** ditampilkan beserta nomor struk.
- **Given** bidan menekan **"Lewati"**, **Then** kunjungan ditutup tanpa transaksi, bidan kembali ke tab layanan pasien, dan **tidak ada data baru** di halaman Keuangan.

**US-02 — Biaya otomatis dari daftar harga**
- **Given** SKU "ANC Reguler" berharga Rp 50.000, **When** bidan memilih SKU tersebut, **Then** field Biaya terisi **Rp 50.000** dan tetap dapat diubah manual.
- **Given** bidan mengubah nilai biaya, **When** transaksi disimpan, **Then** struk memakai **nilai biaya hasil perubahan**, bukan harga SKU.

**US-03 — Mengatur SKU & harga**
- **Given** bidan membuka Pengaturan → Harga Layanan dan memilih kategori **KB**, **When** bidan menambahkan "KB Suntik 1 Bulan" dengan harga Rp 100.000, **Then** SKU muncul di daftar kategori KB dan tersedia sebagai pilihan di layar Kasir.
- **Given** sebuah SKU dinonaktifkan, **Then** SKU tersebut **tidak muncul** di layar Kasir, tetapi tetap tampil di daftar Harga Layanan dengan penanda nonaktif.
- **Given** kategori belum punya SKU, **Then** tampil keadaan kosong *"Belum ada SKU untuk kategori ini…"*.

**US-04 — Struk & pengiriman**
- **Given** transaksi selesai, **When** bidan menekan **unduh/bagikan**, **Then** **PDF struk** terunduh/terkirim berisi nama praktik, nomor struk, tanggal, rincian, total, metode bayar, dan jumlah dibayar.
- **Given** pasien punya No. HP, **When** bidan menekan **"Kirim WA"**, **Then** WhatsApp terbuka dengan **teks ringkasan pembayaran** sudah terisi ke nomor pasien.
- **Given** pasien tanpa No. HP, **Then** tombol "Kirim WA" **tidak ditampilkan** sementara unduh PDF tetap tersedia.

**US-05 — Nomor struk otomatis**
- **Given** tiga transaksi tersimpan pada hari yang sama, **Then** nomor struk berurutan (`…-001`, `…-002`, `…-003`) dengan kode klinik di depan, dan bidan **tidak dapat mengedit** nomor tersebut.

**US-06 — Melewati kasir**
- **Given** bidan memilih **"Lewati"**, **Then** kunjungan tetap tercatat pada rekam medis pasien, transaksi tidak dibuat, dan halaman Keuangan tidak berubah.

## 5. Struk Pembayaran & Naskah Pesan


### 5.1 Susunan struk PDF

```
PMB Ratna Sejahtera
Jl. Raya Sragen No. 12, Sragen · 0851-5697-4057

STRUK PEMBAYARAN
No. Struk : PMB001-20260916-003
Tanggal   : Rabu, 16 September 2026

Pasien    : Siti Aminah
No. RM    : RM-000123

Layanan                Biaya
ANC Reguler            Rp 50.000
--------------------------------
TOTAL                  Rp 50.000
Metode bayar           Tunai
Jumlah dibayar         Rp 50.000
Kembalian              Rp 0

Terima kasih atas kepercayaan Anda. 🙏
```

*(Kop memakai nama praktik + lokasi + No. HP dari pengaturan klinik; bila pasien tidak punya nomor RM, baris tersebut tidak ditampilkan.)*

### 5.2 Naskah teks WA — pembayaran lunas

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

### 5.3 Naskah teks WA — kirim ulang struk

```
Salam sehat, Ibu Siti.
Berikut kami kirimkan kembali rincian pembayaran kunjungan Ibu pada Rabu, 16 September 2026:
🧾 No. Struk: PMB001-20260916-003
💰 Total: Rp 50.000 (Tunai)

Bila ada yang ingin ditanyakan, silakan balas pesan ini ya.

PMB Ratna Sejahtera
📍 https://maps.app.goo.gl/pmb-ratna-sejahtera
```

### 5.4 Naskah teks WA — belum lunas *(disiapkan untuk iterasi berikutnya, belum dipakai)*

```
Salam sehat, Ibu Siti.
Kami mencatat pembayaran kunjungan Ibu pada Rabu, 16 September 2026:
🧾 No. Struk: PMB001-20260916-003
💰 Total: Rp 150.000
✅ Dibayar: Rp 100.000
⏳ Sisa: Rp 50.000

Mohon informasi bila ingin dilunasi ya. Terima kasih 🙏

PMB Ratna Sejahtera
```

**Catatan naskah:** nama pasien mengikuti aturan sapaan yang berlaku di modul pengingat (dewasa "Ibu"; di bawah 18 tahun tanpa "Ibu"); untuk imunisasi anak, struk & pesan ditujukan ke orang tua dengan menyebut nama anak; PDF **tidak** dilampirkan otomatis oleh tombol WA karena deep link `wa.me` tidak mendukung lampiran file.

---

## 6. State & Edge Cases


| No. | Keadaan | Perilaku yang diharapkan |
|---|---|---|
| 1 | Kategori layanan **belum punya SKU aktif** | Pemilih layanan kosong dengan ajakan mengatur harga layanan; tombol "Selesai" tidak aktif sampai layanan dipilih. |
| 2 | **SKU gratis** (harga Rp 0, mis. Edukasi KB) | Transaksi tetap dapat disimpan; struk dan pesan WA menampilkan **Rp 0**. |
| 3 | **Biaya diubah manual** oleh bidan | Nilai hasil perubahan yang disimpan; harga SKU di pengaturan **tidak ikut berubah**. |
| 4 | **Jumlah dibayar lebih besar** dari biaya | Layar & struk menampilkan **kembalian**; transaksi tetap tersimpan. |
| 5 | **Jumlah dibayar lebih kecil** dari biaya | Layar menampilkan **selisih kurang** sebagai informasi; transaksi tetap tersimpan dengan status **Lunas** (pelunasan bertahap di luar cakupan rilis ini). |
| 6 | Pasien **tanpa No. HP** | Tombol "Kirim WA" tidak ditampilkan; unduh/bagikan PDF tetap tersedia. |
| 7 | **Imunisasi anak** | Nama pada struk = nama anak; pengiriman WA ke orang tua (No. HP anak, fallback No. HP orang tua). |
| 8 | Dua transaksi dibuat pada waktu berdekatan | Nomor urut struk tetap **unik** (dihitung per klinik per hari), tidak ada nomor kembar. |
| 9 | **Gagal membuat PDF** | Muncul pesan kegagalan dan tombol coba lagi; **data transaksi tidak hilang**. |
| 10 | SKU dihapus setelah dipakai transaksi lama | Riwayat transaksi lama tetap menampilkan nama & biaya yang tersimpan. |

**Keterbatasan yang diketahui (bukan cacat):** transaksi yang sudah disimpan **tidak dapat diedit** (sesuai peringatan di layar Kasir); koreksi dilakukan dengan mencatat kunjungan baru bila memang diperlukan.

---

## 7. Non-Functional Requirements


| ID | Requirement |
|---|---|
| NFR-01 | **Privasi isi struk & pesan:** hanya memuat data transaksi (nomor struk, nama pasien, layanan, biaya, metode bayar) — **tidak memuat data medis atau diagnosis**. |
| NFR-02 | **Format angka & tanggal:** mata uang memakai format Indonesia (`Rp 50.000`), tanggal memakai nama hari & nama bulan penuh, waktu memakai zona WIB. |
| NFR-03 | **Aksesibilitas & kejelasan status:** tombol aksi utama minimal tinggi 44 px; status nonaktif SKU ditandai teks (tidak hanya warna); peringatan transaksi tidak dapat diedit selalu tampil sebelum tombol Selesai. |

---

## 8. Dependencies & Open Items


**Ketergantungan:**
- **Alur pencatatan layanan (epic PTS-375)** sebagai titik masuk Kasir (tombol "Lanjut ke Kasir" pada layar ringkasan sukses).
- **Halaman Keuangan (epic PTS-603)** mengonsumsi data transaksi ini sebagai rekap pemasukan.
- **Pengaturan klinik:** nama praktik, lokasi, **kode klinik**, dan No. HP praktik (dipakai untuk kop struk & format nomor struk).
- **Nomor RM pasien** (standar PrimaCare) untuk identitas pada struk — bila belum tersedia, baris No. RM tidak ditampilkan.

**Open items:**
1. Kop struk: perlu logo klinik? ukuran & tata letak final diserahkan ke UX.
2. Perlukah **tanda tangan bidan / cap** pada struk PDF.
3. Apakah unduh ulang PDF dibatasi rentang waktu (mis. 3 bulan terakhir) atau bebas untuk seluruh riwayat.
4. Apakah tombol "Kirim WA" juga disediakan pada riwayat transaksi lama (FR-14 saat ini membuka struk, pengiriman ulang dilakukan dari layar struk).

**Iterasi berikutnya (di luar PRD ini):** multi-layanan per transaksi + subtotal · diskon/potongan · pelunasan bertahap (status lunas & naskah "belum lunas" sudah disiapkan) · sinkronisasi harga & transaksi ke PrimaCare · cetak struk ke printer thermal.
