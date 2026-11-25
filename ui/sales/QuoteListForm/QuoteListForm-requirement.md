---
title: "QuoteListForm-requirement"
status: draft
doc-id: "mb-2bd93691"
updated: "2026-11-25"
---
# QuoteListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `sales.QuoteListForm` |
| Purpose | The quotes list with their statuses (ticket 38, F-35; UF-40) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Sales & Operations → Quotes) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_quotes`) | "Quotes", `headline-large` | `page-title` | — | `quotes.title` |
| New Quote button | `m3.Button` (`btn_new_quote`, filled) | Opens QuoteEditorForm in create mode (DESIGN.md empty states: "+ New Quote") | `primary-button` | — | `quotes.new` |
| Status filter | `m3.DropdownMenu` (`dd_quote_status`) | Draft, Sent, Accepted, Declined, Expired | `outlined-select` | — | `quotes.filter.status` |
| Search field | `m3.SearchBar` (`srch_quotes`) | Search by number or customer | — | — | `quotes.search` |
| Quotes grid | Anvil `DataGrid` (`dg_quotes`) | Number, customer, issue date, valid until, total, status | — | mobile | — |
| Quote row | Anvil `DataRowPanel` (`drp_quote_*`) | One quote per row | — | — | — |
| Status badge | `m3.Chip` (`chip_quote_status_*`, type `input`, removable=False) | The quote status badges (DESIGN.md Sales Design: draft On Surface Variant, sent pending, accepted success, declined error, expired On Surface Variant) | — | — | — | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `sales.quote_list`) | The feature's tooltip and help video | — | — | `help.sales.quote_list.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Grid | `quotes` | `quote_number`, `contact_id`, `issue_date`, `valid_until`, `total_amount`, `status` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_quotes` | `quote_service` list read (`server_sales/quote_service`) | The grid arrives |
| Filter / search | `dd_quote_status_change`, `srch_quotes_search` | the same read | The grid reloads |
| New quote | `btn_new_quote_click` | None | `sales.QuoteEditorForm` opens |
| Open a quote | the row click | None | `sales.QuoteEditorForm` opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Document icon (`mi:description`) |
| Heading | `quotes.empty.title` (DESIGN.md: "No quotes yet") |
| Body text | `quotes.empty.body` (DESIGN.md: "Quotes are accepted online and become invoices.") |
| Primary action | `quotes.new` (+ New Quote) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix.

## 7. Text

| Phrase key | English text |
|---|---|
| `quotes.title` | Quotes |
| `quotes.new` | + New Quote |
| `quotes.filter.status` | Status |
| `quotes.search` | Search quotes |
| `quotes.status.draft` | Draft |
| `quotes.status.sent` | Sent |
| `quotes.status.accepted` | Accepted |
| `quotes.status.declined` | Declined |
| `quotes.status.expired` | Expired |
| `quotes.empty.title` | No quotes yet |
| `quotes.empty.body` | Quotes are accepted online and become invoices. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Managing quotes | `master_template` help entry `sales.quote_list` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-40 Deals, companies and quotes | The quotes list |
| UF-41 Customer accepts a quote | The quote's state after acceptance |
