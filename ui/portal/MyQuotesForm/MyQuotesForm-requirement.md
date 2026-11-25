---
title: "MyQuotesForm-requirement"
status: draft
doc-id: "mb-624a4920"
updated: "2026-11-25"
---
# MyQuotesForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `portal.MyQuotesForm` |
| Purpose | The customer's quotes: view, download PDF, accept or decline (ticket 39, F-24; UF-29 step 5, UF-41) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.CustomerLayout` |
| Route | None — opened by the portal routing |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_my_quotes`) | "My Quotes", `headline-large` | `page-title` | — | `portal.nav.quotes` |
| Quotes grid | Anvil `DataGrid` (`dg_my_quotes`) | Number, issue date, valid until, total, status | — | mobile | — |
| Quote row | Anvil `DataRowPanel` (`drp_my_quote_*`) | One quote per row | — | — | — |
| Status badge | `m3.Chip` (`chip_my_quote_status_*`, type `input`, removable=False) | The quote's status | — | — | — |
| View button | `m3.Button` (`btn_view_my_quote_*`, outlined) | Opens the quote document (`quotes.pdf_url`) | `secondary-button` | — | `invoices.download` |
| Accept button | `m3.Button` (`btn_accept_my_quote_*`, filled) | Opens `sales.QuoteAcceptPage` (UF-41 step 1) | `primary-button` | — | `quote.accept` |
| Decline button | `m3.Button` (`btn_decline_my_quote_*`, outlined) | Opens the same page's decline action | `secondary-button` | — | `quote.decline` |
| Help icon | `components.HelpTooltipComponent` (help entry `portal.quotes`) | The feature's tooltip and help video | — | — | `help.portal.quotes.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Quotes | `quotes` | the signed-in customer's rows |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the quotes | `form_show` → `load_my_quotes` | the portal's quote read (own only) | The grid arrives |
| View a quote | `btn_view_my_quote_*_click` | `quote_service` PDF read | The PDF opens |
| Accept or decline | `btn_accept_my_quote_*_click` / `btn_decline_my_quote_*_click` | None | The acceptance page opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Document icon (`mi:description`) |
| Heading | `portal.quotes.empty.title` |
| Body text | `portal.quotes.empty.body` |
| Primary action | None — quotes arrive from the business |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | none |
| Customer | all |
| SignedOut | none |

## 7. Text

| Phrase key | English text |
|---|---|
| `portal.quotes.empty.title` | No quotes yet |
| `portal.quotes.empty.body` | Quotes the business sends you appear here. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Your quotes | `master_template` help entry `portal.quotes` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-41 Customer accepts a quote | My Quotes as the entry (UF-41 step 1) |
| UF-29 Customer portal | My Quotes |
