---
title: "45 — F-28 Accounts"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-3581d99d"
updated: "2026-11-17"
---

## Description

**Feature:** F-28 Accounts (UF-32; SR-46; FR-28). Common feature reading trade tables through its own server functions (ADR `common-feature-reads-trade-data`).
**Depends on:** 23 (invoices and payments feed the reports), 03 (foundation), 04 (R2 for receipts).

### Forms

- `accounts.ExpenseListForm`, `accounts.ExpenseEditorForm` (with the receipt photo), `accounts.AccountsForm` — profit and loss by month, outstanding invoices, tax collected and paid, customer statements (PDF), export

### Server functions

- `server_accounts/expense_service` — `record_expense` (Owner/Manager); void, never deleted; the receipt image to the client's bucket at `expenses/receipts/` with the key in `expenses.receipt_r2_key`
- `server_accounts/accounts_report_service` — the reports from `invoice`, `payments`, `expenses`
- `server_accounts/accounting_export_service` — the Wave, Xero and QuickBooks exports: one ZIP of the period's full books, in that package's import format, into `exports/`

### Tables

- `expenses`, `expense_categories`, `invoice`, `payments`, `contacts`

### Acceptance tests

- `SR-46`; UF-32; `IN-10` — the R2 receipt writes; `IN-14` — every export writes an audit row
- A void expense is left out of every report and export

### Security criteria

- B1 — Owner/Manager only (access matrix)
- B6 — the bucket writes through `r2_service` only
- AC-S16 — exports audited; the accounting export is part of the exit export's formats

## Comments
