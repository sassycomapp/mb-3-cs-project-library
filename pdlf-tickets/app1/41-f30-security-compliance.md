---
title: "41 — F-30 Security and compliance"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-de9d8adb"
updated: "2026-11-17"
---

## Description

**Feature:** F-30 Security and compliance (`spec-regulatory-compliance-baseline`; `spec-security-architecture`; SR-49; FR-30; pol-legal-documents-and-consent).
**Depends on:** 33 (the public pages host the banner), 40 (the legal documents exist to reference), 03 (rate limits), 16 (auth).

### Forms

- None new. The cookie banner is built here onto the public pages and PublicLayout: Accept all, Refuse all and More info before any embedded-content cookie; placeholders on refusal; the Cookie settings link (FooterComponent, ticket 05)

### Server functions

- `record_consent` (server_settings/policy_service) — the public, rate-limited consent write; server-derived fields only; one `consents` row per choice, acceptance or shown notice with the published version
- The consent archive task — rows older than 30 days to `consents/{document}/{year}/{month}/` in the client's bucket; cookie records deleted at 2 years, acceptance records at 7 years
- Data-subject rights: export of a person's data, correction, erasure on request with transactions kept for the 7-year tax period (IN-14)
- Rate-limit enforcement completion across the public endpoints

### Tables

- `consents`, `rate_limits`, `activity_log`, `contacts` (the terms and privacy acceptance rows carry the contact)

### Acceptance tests

- spec-legal-documents-and-consent §12 — the banner and consent tests; test-plan §15
- `ST-02` on the endpoints; `IN-14` — data-subject rights and retention; `IN-16` — rate limits
- `mb_consent` is a first-party necessary cookie (12 months, no personal data)

### Security criteria

- B8 — the consent write is rate-limited and bounded; a forged or spammed consent write changes nothing of consequence (the record holds no identity for cookie rows)
- AC-S8, AC-S12, AC-S16 — every export writes an audit row; the full-data export runs as a background task
- AC-S14 — structural isolation proofs (with the scanners: ST-06, SC-14)

## Comments
