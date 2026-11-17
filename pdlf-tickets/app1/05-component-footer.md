---
title: "05 — Shared component: FooterComponent"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-1fe96fdf"
updated: "2026-11-17"
---

## Description

**Feature:** F-06 (public website footer); shared custom component 2 of 11 (custom-component-requirements-matrix).
**Depends on:** 01 (PublicLayout hosts it), 02 (phrase keys).

### Forms

- `components.FooterComponent` — logo, accreditations, social links, contact email and phone, policy links, the Cookie settings link, optional Google Maps embed; identical on every public page

### Server functions

- `get_public_business_profile` (server_settings/page_content_service) — the intentionally public reader of the footer's fields

### Tables

- `business_profile` (read, public subset)

### Acceptance tests

- `ST-11`, `DC-13` — component standards and accessibility
- The map loads only after the visitor accepts cookies; before that a placeholder names Google Maps with a Load button (pol-legal-documents-and-consent)

### Security criteria

- Public read only — the reader returns the public subset and nothing else (B1; the access matrix's intentionally-public list)
- No colour values; roles only

## Comments
