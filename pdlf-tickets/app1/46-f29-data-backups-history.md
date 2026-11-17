---
title: "46 — F-29 Data, backups and history"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-e0e7e44d"
updated: "2026-11-17"
---

## Description

**Feature:** F-29 Data, backups and history (UF-33; `integration-architecture.md` Client Storage; ADR `client-r2-storage-and-exit-handover`; SR-47, SR-48; FR-29).
**Depends on:** 04 (`r2_service`), 03 (foundation), 17 (the Vault's keys).

### Forms

- None new. The Settings `DataTab` (built as a placeholder at ticket 18) is completed here: audit history older than 30 days, webhook records, past backups, restore status, Export all data as CSV.

### Server functions

- `server_storage/backup_tasks` — the nightly backup to `backups/daily/` and the weekly to `backups/weekly/` (the last 7 daily and 4 weekly)
- `server_storage/history_service` — the history views read from the bucket
- `server_storage/audit_archive_tasks` — audit rows older than 30 days to `audit/{year}/{month}/`
- The retry queue behind every bucket write; the Owner told if a failure persists
- The exit export (`export_service` completion): every data table as CSV, every email still in the instance with its attachments, the accounting exports (with ticket 45), `encryption_key` and the decrypted Vault, into `exports/` — proven on the test client instance before the first release

### Tables

- `activity_log`, `webhook_log`, `email_messages`, `health_log`, `error_log` (the retention rules per kind)

### Acceptance tests

- `IN-10`, `IN-14`, `RL-02` (restore drills), `RL-08` (the exit handover); UF-33; test-plan §12; `client-exit-handover.md` §4

### Security criteria

- B6 — one bucket per client in the client's own account; the token scoped to that bucket, in the Vault; bulk exports audited; backups proven by restore drills; the exit export confirmed before anything is deleted (AC-S16, AC-S14)
- The decrypted Vault holds live credentials — the exit email tells the Owner to change the gateway keys once the instance is closed

## Comments
