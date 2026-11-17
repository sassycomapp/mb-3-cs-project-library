---
title: "50 — F-36 Demo apps"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-f8170b8e"
updated: "2026-11-17"
---

## Description

**Feature:** F-36 Demo apps (UF-44; ADR `no-free-trial`; SR-58; FR-36). The demo flag inside an app (the two demo instances are configured copies, not separate code).
**Depends on:** 03 (`demo_service`), 29 (email redirect), 23 (test-mode payments), 43 (the sample data the demo shows).

### Forms

- None new. The demo behaviour is configuration (`config.demo_mode`) plus the flag's enforcement across the seams.

### Server functions

- `demo_service` completion (ticket 03's module): email delivered only to the signed-in demo prospect (at the `email_service` seam), payments on the gateways' test keys with the test card shown on screen, the nightly reset removing every demo sign-in and restoring the sample state (idempotent)

### Tables

- `users` (`is_demo_visitor`), `sample_rows`, `config` (`demo_mode`)

### Acceptance tests

- `SR-58`; UF-44; the demo reset's idempotence; a prospect uses every feature; every email goes to the prospect's own address
- The Register button opens the registration flow (UF-01)

### Security criteria

- AC-S14 — the demo apps' separation from every paying client; demo accounts are removed nightly
- B10 — demo mode never disables the Vault, rate limits or audit trails

## Comments
