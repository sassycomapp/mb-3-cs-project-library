---
title: "03 — Server foundation: rbac and server_shared"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-7add45ee"
updated: "2026-11-17"
---

## Description

**Feature:** The server foundation every server function stands on (design doc §23.2 seams; F-01 enforcement point; F-02's one-module rule).
**Depends on:** None (can start immediately).

### Forms

- None.

### Server functions / modules

- `server_auth/rbac` — the role decorators (Owner, Manager, Admin, Staff, Customer) every callable wears; the enforcement point of F-01
- `server_shared/config` — read/write of `config` keys; sequence keys and feature switches; transactions inside
- `server_shared/constants`, `server_shared/exceptions`, `server_shared/logging_config`, `server_shared/validators`
- `server_shared/time_utils` — the one UTC ↔ business-time-zone converter; client code never converts
- `server_shared/retry_queue` — `enqueue(operation)`; one retry discipline for every outbound call that may fail transiently
- `server_shared/demo_service` — the demo-mode flag's behaviour (completed at ticket 50)
- `server_shared/export_service` — the export seam (completed at ticket 46)
- `server_settings/vault_service` — the ONE module that reads the `encryption_key` Anvil Secret; holds all encryption and decryption; masked reads `'***'`. The duplicated `vault_service.py` and `encryption_service.py`/`encryption.py` in `server_shared/` are deleted at ticket 17 (C3); this ticket creates the single surviving module.

### Tables

- `vault` (via `vault_service` only), `rate_limits` (the limiter helper every endpoint uses), `config`

### Acceptance tests

- `ST-02` — every callable carries its role decorator; the scanner over the foundation
- `ST-03` — only `vault_service` touches the secret; `IN-13` — masked reads
- `ST-08`, `LG-04`, `LG-05` — UTC storage, money in cents helpers, time boundaries
- `IN-16` — the rate-limit helper honours 10/min unauthenticated per IP, 100/min authenticated per user

### Security criteria

- B10 — secrets lifecycle: exactly one `anvil.secrets.get_secret()` call site; exception handlers in secret-reading functions log codes, never the exception object; secret fields omitted from every outbound payload (AC-S4)
- B8 — the rate-limit helper is the enforcement home of AC-S8
- B1 — every decorator names exactly its roles; a refused call returns `{'success': False}` and writes nothing (AC-S1)

### Notes

- Existing `server_shared` modules are brought to this shape; divergences (`utilities.py`, `metrics.py`) are assessed at the build step that touches them (R6 inventory), not renamed here.

## Comments
