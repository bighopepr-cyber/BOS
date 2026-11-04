# Business Process Specification (BPS)

| Atribut | Nilai |
|---|---|
| Document Code | DOC-005 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Business Process Owner |
| Related Documents | `brs.md`, `frs.md`, `use-case-specification.md`, `business-rules-specification.md`, `../../docs/MTS.md` |

## 1. Tujuan dan Cakupan

Menjelaskan alur lintas fungsi yang tersurat atau secara langsung dibutuhkan oleh capability MTS. Diagram adalah alur konseptual, bukan persetujuan atas semua state, SLA, role, maupun kebijakan bisnis.

## 2. Prinsip Proses

- Backend memvalidasi permission, data scope, dan business state.
- Perubahan atomik diproses dalam database transaction.
- Proses lanjutan dapat asynchronous; status dan retry harus dapat diamati.
- Perubahan finance/inventory tidak boleh menghapus jejak transaksi asal.
- Workflow hanya diberlakukan bila dikonfigurasi dan disetujui.

## 3. Proses

### BPS-CORE-001 — Akses ke data bisnis

**Trace:** SYS-AUTH-001/002, FUN-AUTH-001/002.

```mermaid
flowchart TD
  A[Request pengguna] --> B{Autentikasi valid?}
  B -- Tidak --> X[401]
  B -- Ya --> C{Permission tersedia?}
  C -- Tidak --> Y[403]
  C -- Ya --> D{Data scope dan record diizinkan?}
  D -- Tidak --> Y
  D -- Ya --> E{Business state mengizinkan?}
  E -- Tidak --> Z[Penolakan domain]
  E -- Ya --> F[Validasi dan jalankan use case]
  F --> G[Transaksi dan audit bila relevan]
  G --> H[Respons API]
```

Scope dilaksanakan backend. Strategi penyamaran record di luar scope pada response (misalnya 403/404) perlu konsistensi API dan review security.

### BPS-WF-001 — Persetujuan generik

**Trace:** FUN-WF-001, UC-WF-001.

```mermaid
flowchart TD
  A[Pemohon mengirim permohonan] --> B[Validasi domain, permission, scope]
  B --> C{Valid?}
  C -- Tidak --> R[Error validasi; tidak ada transisi]
  C -- Ya --> D[Transaksi: simpan objek dan workflow instance]
  D --> E[Tentukan approver menurut versi workflow]
  E --> F[Notifikasi asynchronous bila aktif]
  F --> G[Approver meninjau]
  G --> H{Keputusan}
  H -- Setujui --> I[Perbarui status dan history]
  H -- Tolak --> I
  H -- Revisi --> J[Status revisi dan history]
  J --> A
```

Conditional routing, delegasi, eskalasi, withdrawal, dan SLA menunggu konfigurasi/policy. Penolakan tidak menghapus riwayat.

### BPS-FIN-001 — Validasi dan posting jurnal

**Trace:** FUN-FIN-001, UC-FIN-001.

```mermaid
flowchart TD
  A[Draft journal] --> B[Validasi akun, periode, nilai dan referensi]
  B --> C{Debit = kredit?}
  C -- Tidak --> X[Tolak; journal tidak diposting]
  C -- Ya --> D{Periode terbuka dan izin valid?}
  D -- Tidak --> Y[Tolak dengan alasan]
  D -- Ya --> E{Approval diperlukan?}
  E -- Ya --> F[Workflow approval]
  F --> G{Disetujui?}
  G -- Tidak --> H[Simpan hasil/history sesuai aturan]
  G -- Ya --> I[Posting dalam transaksi dan audit]
  E -- Tidak --> I
  I --> J[Journal posted; tidak dapat diedit langsung]
  J --> K[Koreksi berikutnya melalui reversal/adjustment]
```

### BPS-INV-001 — Transfer stok

**Trace:** FUN-INV-001/002, UC-INV-001.

```mermaid
flowchart LR
  A[Buat transfer] --> B[Validasi item, lokasi, scope, saldo]
  B --> C{Valid?}
  C -- Tidak --> X[Tolak tanpa mutasi saldo]
  C -- Ya --> D[Status siap dikirim]
  D --> E[Dispatch; catat stock movement keluar]
  E --> F[Transit]
  F --> G[Receipt; catat stock movement masuk]
  G --> H[Rekonsiliasi jumlah dan status]
  H --> I[Audit dan saldo konsisten]
```

Aturan selisih, backorder, costing, reservation, lot/serial, dan cancellation harus ditetapkan pada pack Inventory. Tidak boleh ada saldo yang diubah langsung tanpa movement.

### BPS-POS-001 — Checkout dan dampak transaksi

**Trace:** FUN-POS-001, BUS-POS-001.

```mermaid
flowchart TD
  A[Cart] --> B[Validasi item, harga, discount, tax, shift]
  B --> C{Valid?}
  C -- Tidak --> X[Tolak checkout]
  C -- Ya --> D[Submit dengan idempotency key]
  D --> E[Proses payment dan rekam status]
  E --> F{Payment berhasil/terkonfirmasi?}
  F -- Tidak --> G[Catat gagal/pending dan recovery]
  F -- Ya --> H[Commit transaksi POS]
  H --> I[Kontrak stock movement Inventory]
  H --> J[Kontrak posting/rekap Finance]
  I --> K[Receipt dan audit]
  J --> K
```

Atomicity lintas modul, payment timeout, dan sinkronisasi inventory/finance harus ditetapkan pada desain integrasi; diagram tidak mengasumsikan transaksi database lintas provider.

### BPS-OPS-001 — Update sistem

**Trace:** FUN-ADM-003.

```mermaid
flowchart TD
  A[Validasi versi dan paket] --> B[Backup dan verifikasi]
  B --> C[Maintenance mode bila diperlukan]
  C --> D[Deploy aplikasi dan migration]
  D --> E[Clear cache dan restart worker]
  E --> F[Health check]
  F --> G{Sehat?}
  G -- Ya --> H[Catat versi dan hasil]
  G -- Tidak --> I[Recovery sesuai runbook; validasi data]
```

## 4. Kontrol Lintas Proses

- Setiap penolakan/konflik harus menghasilkan error yang konsisten, tanpa mutasi parsial.
- Operasi kritis memerlukan audit yang tidak memuat credential/secret.
- Event/notifikasi setelah commit harus punya strategi kegagalan yang ditentukan DOC-020.
- Proses asynchronous tidak boleh dianggap selesai sebelum statusnya terkonfirmasi.

## 5. Batasan dan Open Issues

Role/approval authority, nominal threshold, SLA/escalation, accounting close, refund/return rules, payment provider, stock discrepancy, inventory valuation, backup policy dan recovery objectives belum diatur oleh sumber yang tersedia.

| ID | Isu | Status |
|---|---|---|
| BPS-OPEN-001 | Detail state machine untuk proses domain belum ditetapkan. | TBC |
| BPS-OPEN-002 | Konsistensi lintas modul POS-payment-inventory-finance memerlukan keputusan integrasi. | ADR Required |
| BPS-OPEN-003 | Approval matrix dan SLA organisasi belum tersedia. | TBD |

## References

- `../../docs/MTS.md`, §§3.2, 5.4–5.7, 6.2, 6.6–6.7, 15–16.
- `frs.md` DOC-003; `use-case-specification.md` DOC-004.

## Change History

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Alur proses konseptual untuk akses, workflow, finance, inventory, POS, operations | Documentation Agent |
