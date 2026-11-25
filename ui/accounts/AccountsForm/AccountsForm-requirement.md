---
title: "AccountsForm-requirement"
status: draft
doc-id: "mb-f5b18e6d"
updated: "2026-11-25"
---
# AccountsForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `accounts.AccountsForm` |
| Purpose | Tabs Profit and Loss, Outstanding, Tax, Statements, Export; figures in tables with the period selector at the top; Export offers one period and three buttons (ticket 45, F-28; UF-32; DESIGN.md Accounts Design) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Finance & Reports → Accounts) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_accounts`) | "Accounts", `headline-large` | `page-title` | — | `accounts.title` |
| Period selector | `m3.DatePicker` (`dp_accounts_from`), `m3.DatePicker` (`dp_accounts_to`) | The period, last month by default (UF-32 Export step 1) | `outlined-date` | mobile | `data.history.from`, `data.history.to` |
| Tabs | `m3.Tabs` (`tabs_accounts`, type `primary`) | Profit and Loss, Outstanding, Tax, Statements, Export (DESIGN.md Accounts Design) | — | — | one key per tab, section 7 |
| Profit and Loss panel | `m3.Card` (`card_pnl`, outlined) with the table | Payments less refunds, against expenses and gateway fees, by month (architecture reference F-28) | `surface-card` | mobile | — |
| P&L table | Anvil `DataGrid` (`dg_pnl`) | Month, income, expenses, profit | — | mobile | — |
| Outstanding panel | `m3.Card` (`card_outstanding`, outlined) with the table | The outstanding invoices | `surface-card` | mobile | — |
| Outstanding table | Anvil `DataGrid` (`dg_outstanding`) | Invoice, customer, due date, total, paid | — | mobile | — |
| Tax panel | `m3.Card` (`card_tax`, outlined) with the table | Tax collected (invoices) and tax paid (expenses) | `surface-card` | mobile | — |
| Tax table | Anvil `DataGrid` (`dg_tax`) | Period, collected, paid | — | mobile | — |
| Statements panel | `m3.Card` (`card_statements`, outlined) | The customer statements as PDF (architecture reference F-28) | `surface-card` | mobile | — |
| Statement row | `m3.ListItem` (`li_statement_*`) | One customer per row with its statement download | — | — | — |
| Export panel | `m3.Card` (`card_export`, outlined) | One period, three buttons: Download for Wave, Download for Xero, Download for QuickBooks (DESIGN.md Accounts Design) | `surface-card` | mobile | — |
| Export buttons | `m3.Button` (`btn_export_wave`, filled), `m3.Button` (`btn_export_xero`, outlined), `m3.Button` (`btn_export_quickbooks`, outlined) | The three accounting exports; each produces one ZIP of the period's full books, in that package's import format | `primary-button` | mobile | `accounts.export.wave`, `accounts.export.xero`, `accounts.export.quickbooks` |
| Export progress | `m3.LoadingIndicator` (`load_accounts_export`, hidden) | The export runs as a background task | — | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `accounts.reports`) | The feature's tooltip and help video | — | — | `help.accounts.reports.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Reports | `invoice`, `payments`, `expenses` | the reports computed in server code (architecture reference F-28) |
| Statements | `invoice`, `contacts` | the customer's figures |
| Exports | the accounting export ZIPs | kept in the bucket's `exports/` folder (`integration-architecture.md`, Accounting Export) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the figures | `form_show` → `load_accounts` | `accounts_report_service` read (`server_accounts/accounts_report_service`) | The tab's figures arrive |
| Change the period | the date pickers | the same read | The figures reload |
| Download a statement | `li_statement_*_click` | `accounts_report_service` statement | The customer's statement PDF is produced |
| Export for Wave / Xero / QuickBooks | `btn_export_*_click` | `accounting_export_service` (`server_accounts/accounting_export_service`) | One ZIP of the period's full books, in that package's import format, into `exports/` and downloaded; the export is audited (AC-S16); sample data is never exported (F-28) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Chart icon (`mi:monitoring`) |
| Heading | `accounts.empty.title` |
| Body text | `accounts.empty.body` |
| Primary action | `analytics.export.csv` |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix.

## 7. Text

| Phrase key | English text |
|---|---|
| `accounts.title` | Accounts |
| `accounts.tab.pnl` | Profit and Loss |
| `accounts.tab.outstanding` | Outstanding |
| `accounts.tab.tax` | Tax |
| `accounts.tab.statements` | Statements |
| `accounts.tab.export` | Export |
| `accounts.export.wave` | Download for Wave |
| `accounts.export.xero` | Download for Xero |
| `accounts.export.quickbooks` | Download for QuickBooks |
| `accounts.export.started` | Your export is being prepared. |
| `accounts.empty.title` | Nothing to report yet |
| `accounts.empty.body` | Your figures appear here as your business operates. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Your accounts at a glance | `master_template` help entry `accounts.reports` — played in the app language |
| Exporting for your accountant | `master_template` help entry `accounts.export` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-32 Expenses, accounts reports and export | The reports and export half |
