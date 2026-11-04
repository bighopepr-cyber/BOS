# Use Case Specification (UCS)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-004 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Business/System Analyst |
| Related Documents | `brs.md`, `srs.md`, `frs.md`, `business-process-specification.md`, `../00-governance/glossary.md` |

## 1. Tujuan dan Konvensi

Dokumen ini merinci use case representatif dari capability MTS untuk menurunkan interaksi, validasi, kontrol akses, dan test. Role berikut bersifat aktor konseptual, bukan katalog role final. Hak khusus dan persetujuan aktual tetap bergantung pada konfigurasi dan kebijakan pemilik.

## 2. Aktor

| Aktor | Deskripsi |
|---|---|
| Pengguna terautentikasi | User yang berhasil login; hak record/action tetap diperiksa backend. |
| Administrator organisasi | Pengguna yang diberi permission untuk konfigurasi organisasi dan user; nama role final TBD. |
| Pemohon | Pengguna yang mengajukan transaksi/proses yang memerlukan approval. |
| Approver | Pengguna yang ditugaskan/berwenang memutuskan workflow instance. |
| Operator modul | Pengguna dengan permission spesifik modul, contohnya HRM, Finance, Sales, Inventory atau POS. |
| System administrator | Operator deployment yang mengelola layanan, backup, queue, update dan health. |
| Sistem eksternal | Provider yang berinteraksi melalui API/webhook; provider dan kontrak TBD. |

## 3. Use Cases

### UC-CORE-001 — Autentikasi pengguna

- **Trace:** SYS-ID-001, FUN-ID-001; BUS-CORE-001.
- **Aktor:** Pengguna.
- **Prasyarat:** Akun tersedia dan tidak suspended; metode/session policy telah dikonfigurasi.
- **Pemicu:** Pengguna mengirim kredensial ke endpoint autentikasi.
- **Alur utama:** (1) Sistem menerima request; (2) membatasi percobaan sesuai konfigurasi; (3) memverifikasi kredensial; (4) membuat session/token sesuai strategi yang disetujui; (5) mencatat hasil login tanpa menyimpan password/token; (6) mengembalikan hasil dan konteks user minimum.
- **Alternatif/kegagalan:** Kredensial tidak valid atau akun suspended: gagal dengan respons generik yang tidak membocorkan status rahasia; throttling: respons rate limited. Rincian pesan dan durasi TBD.
- **Pascakondisi:** Session aktif atau tidak ada perubahan autentikasi; security event dapat diaudit.
- **Otorisasi/validasi:** Password hashing/session/CSRF/cookie mengikuti DOC-021; session vs token TBD.

### UC-CORE-002 — Memproses aksi dengan pemeriksaan scope

- **Trace:** SYS-AUTH-001/002, FUN-AUTH-001/002; BUS-ACCESS-001.
- **Aktor:** Pengguna terautentikasi.
- **Prasyarat:** User memiliki session valid.
- **Alur utama:** (1) API menerima resource/action; (2) backend memeriksa permission; (3) memeriksa data scope dan akses record; (4) memeriksa state bisnis; (5) mengeksekusi use case; (6) mencatat aktivitas sensitif.
- **Alternatif:** Tidak terautentikasi → 401; permission/scope/state ditolak → 403 atau error domain yang sesuai; record tidak ditemukan/di luar disclosure policy → penanganan 404/403 diputuskan konsisten per API.
- **Pascakondisi:** Data hanya dibaca/diubah bila semua pemeriksaan berhasil.

### UC-WF-001 — Mengajukan dan memutuskan permohonan

- **Trace:** SYS-WF-001, FUN-WF-001; BUS-WORK-001.
- **Aktor:** Pemohon, approver, engine workflow.
- **Prasyarat:** Workflow aktif/berversi dan pemohon berhak memulai proses; definisi proses spesifik telah disetujui.
- **Alur utama:** (1) Pemohon mengirim data; (2) backend memvalidasi domain dan scope; (3) transaksi membuat record permohonan dan workflow instance; (4) approver ditentukan menurut workflow; (5) notifikasi dikirim asynchronous bila diaktifkan; (6) approver meninjau dan menyetujui/menolak/meminta revisi; (7) workflow history dan status bisnis diperbarui secara konsisten.
- **Alternatif:** Validasi gagal, approver tidak dapat ditentukan, workflow tidak aktif, atau konflik perubahan: gagal tanpa transisi parsial; eskalasi/delegasi hanya bila terkonfigurasi.
- **Pascakondisi:** Status dan history dapat ditelusuri; aturan revisi/withdrawal spesifik TBD.

### UC-FIN-001 — Posting journal

- **Trace:** SYS-FIN-001, FUN-FIN-001; BUS-FIN-001.
- **Aktor:** Accountant/operator Finance; approver bila workflow diwajibkan.
- **Prasyarat:** User berhak; accounting period terbuka; journal memenuhi validasi.
- **Alur utama:** (1) User menyiapkan entry/lines; (2) sistem memvalidasi referensi, amount numeric, dan periode; (3) sistem memastikan total debit = total kredit; (4) backend memeriksa approval bila disyaratkan; (5) dalam transaksi database, entry diposting dan audit/reference sumber dicatat; (6) sistem menampilkan hasil.
- **Alternatif:** Debit/kredit tidak seimbang, periode terkunci, atau konflik concurrency: tidak diposting; error dapat ditindaklanjuti.
- **Pascakondisi:** Journal posted tidak dapat diedit langsung; koreksi melalui reversal/adjustment. Formula currency/rounding dan period policy TBD.

### UC-INV-001 — Transfer stok antar gudang

- **Trace:** SYS-INV-001, FUN-INV-001/002; BUS-INV-001.
- **Aktor:** Operator Inventory; approver bila dikonfigurasi.
- **Prasyarat:** Gudang/lokasi valid; user memiliki scope; aturan costing/lot/serial yang relevan diketahui.
- **Alur utama:** (1) User membuat transfer beserta item/kuantitas; (2) sistem memvalidasi saldo dan kebijakan sumber; (3) transfer masuk status siap dikirim; (4) pengiriman mencatat movement keluar; (5) penerimaan mencatat movement masuk/selisih sesuai aturan yang disetujui; (6) status dan audit diperbarui.
- **Alternatif:** Saldo kurang, lokasi tidak valid, duplicate request, atau selisih penerimaan: tolak/karantina sesuai aturan TBD; jangan mengubah saldo tanpa movement.
- **Pascakondisi:** Seluruh perubahan stock dapat direkonsiliasi dari stock movements.

### UC-POS-001 — Checkout POS

- **Trace:** SYS-POS-001, FUN-POS-001; BUS-POS-001.
- **Aktor:** Cashier; payment provider bila dipilih.
- **Prasyarat:** Terminal/shift aktif, catalog tersedia, user berwenang; metode payment dikonfigurasi.
- **Alur utama:** (1) Cashier membangun cart; (2) sistem memvalidasi item/harga/discount/tax terhadap konfigurasi yang disetujui; (3) cashier memilih payment; (4) backend memproses request dengan idempotency key untuk operasi berisiko duplikasi; (5) sistem mencatat transaksi/payment dan kontrak inventory/finance; (6) receipt dikeluarkan dan hasil dicatat.
- **Alternatif:** Payment gagal/timeout, duplicate key, shift ditutup, atau stock conflict: status dan recovery mengikuti DOC modul POS; jangan menganggap transaksi berhasil tanpa hasil payment yang tervalidasi.
- **Pascakondisi:** Transaksi dapat ditelusuri; return/refund dan cash closing mengikuti proses terpisah. Offline mode TBD.

### UC-ADM-001 — Memperbarui deployment

- **Trace:** FUN-ADM-003; BUS-OPS-001.
- **Aktor:** System administrator.
- **Prasyarat:** Versi/kompatibilitas diperiksa dan prosedur backup tersedia.
- **Alur utama:** (1) Validasi versi dan paket; (2) buat/verifikasi backup; (3) aktifkan maintenance bila diperlukan; (4) update aplikasi dan jalankan migration; (5) bersihkan cache/restart worker; (6) health check; (7) catat hasil.
- **Alternatif:** Migration/health check gagal: hentikan prosedur dan jalankan recovery terdokumentasi; rollback otomatis tidak diasumsikan aman.
- **Pascakondisi:** Versi dan hasil update tercatat; konsistensi data diverifikasi.

## 4. Use Case Belum Dirinci

Use case rinci untuk employee lifecycle, leave/payroll, lead conversion, sales order/invoice, project/timesheet, collaboration, LMS, reporting, data import, backup/restore, dan integrasi eksternal menjadi deliverable pack modul DOC-007. Semua harus mengacu pada FRS dan tidak menetapkan aturan bisnis tanpa sumber.

## 5. Open Issues

| ID | Isu | Status |
|---|---|---|
| UC-OPEN-001 | Matriks aktor-permission dan data scope aktual belum tersedia. | TBC |
| UC-OPEN-002 | Kebijakan status, approval, delegasi, revisi, SLA, dan eskalasi per proses belum disetujui. | TBD |
| UC-OPEN-003 | Perilaku error yang dapat mengungkap keberadaan record perlu disepakati lintas API. | ADR Required |
| UC-OPEN-004 | Payment provider, offline POS, dan recovery transaksi timeout belum ditentukan. | TBD |

## References

- `srs.md` DOC-002; `frs.md` DOC-003; `business-process-specification.md` DOC-005.
- `../../docs/MTS.md`, §§5, 6, 8–11, 15–18.
- `../00-governance/glossary.md`.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Use case representatif core, Finance, Inventory, POS, dan operations | Documentation Agent |
