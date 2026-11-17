---
title: "52 — F-33 Hosting"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-a136aefd"
updated: "2026-11-17"
---

## Description

**Feature:** F-33 Hosting (architecture reference §4, §12 F-33; common aspects 2 and 10; BRD AC-5, AC-6, AC-7, AC-8, AC-12; ADR `tiers-model`; FR-33; SR-52). The last ticket: it proves the application fits the client instance's limits.
**Depends on:** 47 (the usage report and heartbeat carry the figures).

### Forms

- None.

### Server functions

- `server_management/usage_service` completion — the memory, storage, email-volume and heavy-job figures the daily usage report carries (the report itself is ticket 47)
- The plan-fit proofs: every search bounded (never whole-table iteration), every aggregate pre-computed, every operation expected to exceed 22 seconds a background task, the cold data in R2 with index rows only

### Tables

- `health_log`, `rate_limits` (the limits that keep a client small)

### Acceptance tests

- `DC-07` — expensive functions bounded; `IN-11` — background tasks; `ST-09` — bounded searches
- `SR-52` — the usage report's figures; the at-risk alert thresholds are App 5's side
- The 15-minute heartbeat degrades to every 30 minutes before anything else is skipped

### Security criteria

- B8 — resource exhaustion: rate limits on every endpoint including background-triggering ones; unbounded queries and exports refused
- The per-container load is counted per client, never summed across clients (common aspect 2)

### Notes

- The two open platform unknowns are proved in this run, not researched: an app scheduling its dependency's background task and serving its dependency's HTTP endpoints (PRD OQ-2 — proved in App 1 before Step 31 relies on it), and `boto3` in the release image (proved on the test client instance before the first release).

## Comments
