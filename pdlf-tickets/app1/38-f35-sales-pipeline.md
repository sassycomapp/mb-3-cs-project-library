---
title: "38 — F-35 Sales pipeline, companies and quotes"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-c7ad56f8"
updated: "2026-11-17"
---

## Description

**Feature:** F-35 Sales pipeline, companies and quotes (UF-40, UF-41; DESIGN.md Sales Design — DealBoardForm, QuoteEditorForm, QuoteAcceptPage, quote status badges; SR-56, SR-57; FR-35). Trade feature.
**Depends on:** 20 (contacts and companies), 23 (acceptance creates the invoice), 02 (shared client).

### Forms

- `sales.DealBoardForm` — board by stage and list view; Staff see and move only their own deals
- `sales.DealEditorForm`
- `sales.QuoteListForm`, `sales.QuoteEditorForm` — lines typed or from services, the client's tax rate, validity date
- `sales.QuoteAcceptPage` — public at `/quote/{token}`: read, Accept or Decline, with the unticked acceptance box

### Server functions

- `server_sales/deal_service` — `list_deals` (Staff own), stage moves with the lost reason
- `server_sales/quote_service` — `accept_quote` (public token page): acceptance creates exactly one invoice, emails it with a payment link, moves a linked deal to won; decline notifies the business and leaves a linked deal open
- `server_sales/quote_pdf`, `server_sales/quote_tasks` (the nightly expiry)

### Tables

- `companies`, `deals`, `quotes`, `quote_items`, `invoice`, `contacts`

### Acceptance tests

- `SR-56`, `SR-57`; UF-40, UF-41; `IN-06` — quote acceptance creates one invoice, however often it is asked; the quote lifecycle (test-plan §19); DESIGN.md Sales Design's badges

### Security criteria

- B1, B3 — the acceptance is server-side; the invoice creation is idempotent by the quote's acceptance (AC-S10)
- AC-S2 — Staff own deals only; the token page is intentionally public
- The acceptance box records the Terms and Privacy consent (with ticket 41)

## Comments
