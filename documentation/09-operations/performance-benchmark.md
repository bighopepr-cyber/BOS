# Performance Benchmark Specification

| Atribut | Nilai |
|---|---|
| Document Code | DOC-039 |
| Document Version | 0.1 |
| Document Status | DRAFT |
| Created Date | 2026-11-04 |
| Last Updated | 2026-11-04 |
| Document Owner | TBD — Performance/Test Owner |
| Related Documents | `../08-testing/software-testing-specification.md`, `../06-security-infrastructure/infrastructure-deployment.md`, `../01-requirements/srs.md`, `../../docs/MTS.md` |

## Tujuan

Menetapkan cara mengukur performance target MTS tanpa memperlakukannya sebagai jaminan sebelum environment dan workload didefinisikan.

## Baseline Targets

| Scenario | MTS target awal | Status |
|---|---|---|
| Simple API read | p95 <500 ms | Target baseline, test environment TBD |
| Simple API write | p95 <800 ms | Target baseline, test environment TBD |
| Initial dashboard | <3 seconds | Data/infrastructure profile TBD |
| Large report | asynchronous processing | “Large” threshold TBD |
| Large list | pagination | Required |

## Test Profile Required

Sebelum run, define hardware CPU/RAM/storage/network; OS/container/runtime/DB/cache versions; dataset size/shape; warm/cold cache; concurrent users/ramp/steady duration; mix/read-write; endpoint/data scope; queue/worker capacity; external stubs; acceptable error rate; sampling and percentile method. No claim is valid without profile and repeatable script.

## Scenarios and Evidence

Measure auth/session, paginated lists, representative writes, dashboard queries, report export, import, queue backlog, file upload/download, concurrent document numbering, POS idempotent checkout and finance/inventory transactions. Capture p50/p95/p99, throughput, error rate, CPU/memory, DB waits/locks, cache/queue metrics, dataset/release, run date and anomalies. Protect sampled payloads from sensitive data.

## Acceptance and Tuning

Compare to approved SLO/workload. Identify bottleneck before scaling; MTS recommends vertical scaling first, then worker split/read replica/external object storage/dedicated reporting as needed. Do not introduce microservices as default optimization. Benchmark final thresholds and capacity owner approval TBD.

## Open Issues

Target concurrency/volume, hardware, SLO/availability, data profiles, report threshold, load tool, traffic patterns, duration, error budget and release gate TBD.

## References / Change History

MTS §§17.1–17.2, 18; DOC-002, DOC-028, DOC-030.

| Versi | Tanggal | Perubahan | Penulis |
|---|---|---|---|
| 0.1 | 2026-11-04 | Benchmark protocol based on provisional MTS targets | Documentation Agent |
