# System Administration Manual

| Atribut | Nilai |
|---|---|
| Document Code | DOC-033 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — System Administrator |
| Related Documents | `deployment-manual.md`, `monitoring-logging.md`, `troubleshooting-guide.md`, `../06-security-infrastructure/backup-disaster-recovery.md`, `../../docs/MTS.md` |

## Tujuan dan Access

Panduan operasi administrative BOS. Pisahkan application administrator dari host/DB operator sesuai RACI yang TBD; akses least privilege dan backend permission enforcement berlaku.

## Tugas Admin Aplikasi

- Kelola organization settings, locale/timezone/currency setelah kebijakan dikonfirmasi.
- Kelola user activation/suspension, role/permission dan scope; lakukan access review.
- Kelola module settings, workflow versions, document numbering, notification/storage/email configuration sesuai permission.
- Pantau queue depth, failed jobs, scheduler, storage, app/DB connectivity, error events dan last backup.
- Jangan mengubah transaksi posted/ledger atau saldo stok di luar domain workflow; gunakan reversal/adjustment atau stock movement.

## Tugas Infrastruktur

- Pantau application version, DB/Redis, disk/storage, worker/scheduler, logs dan backup.
- Update mengikuti release procedure, backup dahulu, verifikasi migration dan restart workers.
- Restore hanya melalui prosedur yang disetujui dan dicatat.
- Secrets/keys diakses oleh operator yang ditunjuk; jangan salin ke tiket/log.

## Routine and Incident Actions

Sebelum perubahan: identifikasi scope, approver, backup/impact, maintenance need. Setelah: health check, queue/scheduler, representative API, file access, audit and result record. Untuk incident, preserve evidence, restrict affected credentials, follow incident response TBD, avoid destructive cleanup.

## Open Issues

RACI/role catalog, review cadence, operational dashboards, alert thresholds, retention, support access, incident contacts, emergency access/break-glass and administrative audit review TBD.

## References / Change History

MTS §§5.3–5.7, 16; DOC-027/028/029/038.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Application/host administration procedures outline | Documentation Agent |
