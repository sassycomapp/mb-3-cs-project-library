---
title: "20 — F-17 CRM and contacts"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-3f94590b"
updated: "2026-11-17"
---

## Description

**Feature:** F-17 CRM and contacts (UF-21, UF-22; ADRs `single-contacts-table`, `lead-capture-simultaneous-creation`; SR-32; FR-17). Client package `crm/`, server package `server_customers/` (ADR `crm-package-replaces-marketing`).
**Depends on:** 16 (auth), 03 (foundation).

### Forms

- `crm.ContactListForm`, `crm.ContactEditorForm`, `crm.ContactViewerForm` (statistics, company, timeline, deals, quotes, active campaigns, packages, open invoices, quick actions)
- `crm.ContactImportForm` — CSV import with column mapping (the same mapping as onboarding step 9)
- `crm.CompanyListForm`, `crm.CompanyViewerForm`
- `crm.SegmentManagerForm` — prebuilt and custom segments, the rule builder with "all of"/"any of", preview and count (UF-22)

### Server functions

- `server_customers/contact_service` — `get_contact` with the restricted projection (`internal_notes` only for the roles the access matrix allows), create-or-match by email, timeline events
- `server_customers/company_service`
- `server_customers/contact_import_export` — `import_contacts` (existing contacts with the same email are updated, not duplicated); CSV export
- `server_customers/segment_service` — rule evaluation (pure module beside it), the nightly `contact_count` recalculation
- The `contact_counter` increment inside `@tables.in_transaction`

### Tables

- `contacts`, `contact_counter`, `contact_events`, `companies`, `segments`

### Acceptance tests

- `IN-02`, `IN-03` — access and row scoping; `IN-04` — the `internal_notes` restricted projection
- `SR-32`, `SR-08a` (import); UF-21, UF-22

### Security criteria

- B1, B2 — role decorators per the access matrix; Admin may edit the listed writable fields only (`writable_fields.contacts`)
- AC-S2 — no role reaches another person's records beyond the matrix's scopes
- AC-S12 — timeline events and import writes audited where significant

### Notes

- Existing forms at these paths are rebuilt as HTML templates at the same module path; the R6 divergence inventory touches nothing here.

## Comments
