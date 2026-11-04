# Deployment Manual

| Atribut | Nilai |
|---|---|
| Document Code | DOC-032 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Deployment Operator |
| Related Documents | `../06-security-infrastructure/infrastructure-deployment.md`, `../06-security-infrastructure/backup-disaster-recovery.md`, `release-management.md`, `../04-backend/backend-technical-specification.md`, `../../docs/MTS.md` |

## Tujuan dan Prasyarat

Panduan deployment konseptual self-hosted BOS. MTS belum menentukan distro/version, image, domain, host sizing, secret provider, storage backend, atau commands final; instruksi ini adalah urutan kerja, bukan copy-paste production runbook.

Prasyarat: Linux host sesuai matriks kompatibilitas yang disetujui; Docker/Compose dan resource memenuhi profil TBD; domain/TLS, secret/config, PostgreSQL, Redis, persistent storage, backup target, operator berwenang, release artifact tervalidasi.

## Instalasi Awal

1. Hardening host/network; batasi akses administrasi dan service ports.
2. Siapkan deployment configuration dan secrets di luar repository; verifikasi tidak tercatat di logs.
3. Siapkan persistent DB/file volumes dan akses backup.
4. Jalankan service sesuai compose artifact yang telah direview: NGINX, frontend, backend, PostgreSQL, Redis, worker, scheduler.
5. Jalankan migration dan initial administrator/organization sesuai prosedur terkontrol; aktifkan installation lock.
6. Konfigurasi storage/email; verifikasi authorized file access dan outbound test yang disetujui.
7. Jalankan health/security smoke check, migration status, queue/scheduler status, backup verification, lalu catat versi dan operator.
8. Buka akses pengguna hanya setelah approver operasional menerima evidence.

## Update

Validate release/version/compatibility → backup dan verifikasi restore artifact → maintenance mode bila perlu → deploy kompatibel → migration → clear cache/restart worker → health, API, queue, scheduler, file, smoke test → record hasil. Bila gagal, hentikan traffic/operasi terdampak dan gunakan prosedur data-aware recovery; jangan mengandalkan `down` migration semata.

## Rollback/Recovery

Ikuti DOC-029. Tetapkan recovery point, data writes setelah backup, versi kode/skema, file/object consistency dan approver sebelum restore. Commands spesifik TBD setelah teknologi/tool ditetapkan.

## Checklist Operasional

- TLS berlaku dan service non-public tidak terbuka.
- DB/Redis/file persistence dan disk capacity terverifikasi.
- Secret tidak ada di repo/log; application key tersedia aman.
- Migration version dan release identifier tercatat.
- Queue workers dan scheduler sehat.
- Backup baru dan restore procedure diuji.
- Admin account bootstrap diamankan; installer terkunci.

## Open Issues

OS/Docker versions, commands, secrets, TLS, domain, image provenance, host sizing, maintenance window, deployment automation, smoke-test endpoints, on-call/approval, rollback compatibility.

## References / Change History

MTS §§15–17; DOC-028/029/037/038.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Deployment runbook outline, pending environment-specific commands | Documentation Agent |
