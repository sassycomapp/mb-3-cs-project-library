---
title: "28 — F-14 Time tracking"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-e9a1f8d9"
updated: "2026-11-17"
---

## Description

**Feature:** F-14 Time tracking (UF-13; SR-25; FR-14). Trade feature.
**Depends on:** 23 (invoice lines), 21 (services' hourly rate), 16 (auth).

### Forms

- `time_tracking.TimeEntryListForm` — filter, select billable entries, create invoice
- `time_tracking.TimeEntryEditorForm` — manual entry and start/stop timer

### Server functions

- `server_time/time_entry_service` — entries with duration and total calculated on save; the billable-to-invoice step marks entries invoiced in the same call that creates the invoice lines, so they cannot be invoiced twice
- `server_bookings/time_service` — assessed at ticket 24; if kept, its functions fold under this feature's tests here

### Tables

- `time_entries`, `invoice`, `invoice_items`, `services`, `contacts`

### Acceptance tests

- `SR-25`; UF-13; `IN-02` (Staff own rows); reports: hours and value per provider, per customer and per service

### Security criteria

- B1, AC-S2 — Staff see and edit only their own entries (`writable_fields` pattern)
- The once-only invoicing invariant is enforced server-side in the same call (AC-S10 pattern)

## Comments
