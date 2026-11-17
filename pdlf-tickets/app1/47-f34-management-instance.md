---
title: "47 — F-34 Mybizz management (instance side)"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-611caab5"
updated: "2026-11-17"
---

## Description

**Feature:** F-34 Mybizz management app — the instance side (architecture reference §1 App 5, §12 F-34; UF-36, UF-37; ADR `mybizz-management-visibility`; `spec-api-specification` management endpoints; SR-53, SR-53a, SR-54; FR-34). App 5 itself is built in its own run; this ticket is what a client instance exposes to it.
**Depends on:** 04 (the outbound App 5 route and retry queue), 03 (foundation).

### Forms

- None.

### Server functions / modules

- `server_management/management_endpoints` — the seven Bearer-token endpoints of `spec-api-specification` §3: health, status, suspend, unsuspend, push-notification, push-update-alert, plan-summary; each validates the instance's per-client Bearer token before any processing — an invalid token is HTTP 403 and nothing changes; the push path is the one canonical endpoint `POST /management/push-notification` (D4)
- `server_management/heartbeat_tasks` — the 15-minute heartbeat: status, release version, schema fingerprint, background-task status, error count, queued retries; degrades to every 30 minutes before anything else is skipped
- `server_management/usage_service` — the daily usage report: rows, storage, email volume, heavy-job activity, last team-member sign-in, last booking date; carries no personal data
- `server_support/help_service` — the Help delivery to App 5 (ticket 49 completes the feature's form side)

### Tables

- `health_log`, `error_log`, `plan_summary` (written only by App 5 through plan-summary), `notifications` (the platform push writes `source: platform`)

### Acceptance tests

- `IN-12`, `RL-05`, `SC-14`; `SR-53`, `SR-53a`; test-plan §17 rows 5–6 (the push lands once in the feed; a bad token is HTTP 403 and writes nothing)

### Security criteria

- B5 — one token per client, encrypted in the App 5 registry and the instance's Vault; rotation manual in V1 by re-provisioning, tracked (the residual is recorded, not silently assumed)
- AC-S14 — no client data beyond the endpoints' purpose; AC-S15 — nothing of App 5's credentials is in App 2 or App 3
- AC-S12 — heartbeats and the usage report carry no personal data beyond row ids

## Comments
