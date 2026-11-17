---
title: "12 — Shared component: ClauseBuilder"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-f9fd83a8"
updated: "2026-11-17"
---

## Description

**Feature:** F-22 (the Clause Builder component); shared custom component 9 of 11.
**Depends on:** 02 (phrase keys), 04 (`r2_service` for the published copies).

### Forms

- `components.ClauseBuilder` — parameterised by document type (`privacy_policy`, `t_and_c`, `cookie_policy`); category selection; seeded clauses (editable, not deletable); custom clauses (editable and deletable); clause editing; drag reordering; switching clauses on or off; hosted by PolicyEditorForm (ticket 40)

### Server functions

- `publish_policy` (server_settings/policy_service — the module moves from `server_shared/` to `server_settings/` at this step, per the divergence inventory) — saves the document, publishes the public page immediately, raises the document's version and keeps the published text in R2 at `legal/{document}/v{version}.html`

### Tables

- `privacy_policy`, `t_and_c`, `cookie_policy` (the clause tables; `cookie_policy` adds `feature_key`), `config` (`legal_versions`), `activity_log`

### Acceptance tests

- `SR-38`, `SR-38a` — publication raises the version and writes the R2 copy
- spec-legal-documents-and-consent §12 — the publish tests; a `feature_key` clause is active exactly while its feature is on
- `IN-10` — the R2 write is queued on bucket failure

### Security criteria

- B1 — `publish_policy` is Owner-only (access matrix)
- B6 — the R2 write goes through `r2_service` only; the write is audited
- AC-S12 — the publication writes an audit row

## Comments
