---
title: "17 — F-02 The Vault"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-f3cebfe7"
updated: "2026-11-17"
---

## Description

**Feature:** F-02 The Vault (UF-04; `spec-vault-system`; ADR `payment-security-boundary-vault`; SR-06, SR-07; FR-02).
**Depends on:** 03 (the one `vault_service`), 16 (auth and step-up).

### Forms

- `settings.VaultForm` — Owner only; TOTP on every open with no remembered session; enrolment on first open from a QR code; masked values; the Reset Vault code path

### Server functions

- `server_settings/vault_totp_service` — the Vault's own authenticator: enrolment, code check, the 15-minute lock after 5 wrong codes
- `vault_service` completion — lockout state, the access email on every open, `activity_log` rows, the hard-fail guard before any broader handler on payment-config saves
- C3 completed here: the duplicated `vault_service.py`/`encryption_service.py` in `server_shared/` are deleted; only the one `vault_service` reads `encryption_key`

### Tables

- `vault`, `activity_log`, `rate_limits` (the Vault lock counter)

### Acceptance tests

- `IN-13`, `SC-03` — Vault access needs a fresh second factor and a fresh TOTP code; masked reads; the hard-fail guard
- Test-plan §8 (the five Vault steps); tests 4.18–4.19; test specification 2.5; UF-04
- `sop-vault-recovery.md` — both recovery paths (Owner-only reset by emailed link)

### Security criteria

- B10 — secrets lifecycle: masked reads, no round-trip, no logging of values; `encryption_key` never rotated, protected by the one-module rule (AC-S4)
- AC-S5 — every Vault view requires sign-in with the Owner's second factor in the last five minutes and a fresh TOTP code; every access emails the Owner; 5 wrong codes lock the Vault for 15 minutes
- AC-S7 — Vault access is a high-value action needing a factor given in the last five minutes

## Comments
