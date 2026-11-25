---
title: "MyInvoicesForm-requirement"
status: draft
doc-id: "mb-f32a7882"
updated: "2026-11-25"
---
# MyInvoicesForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `portal.MyInvoicesForm` |
| Purpose | The customer's invoices: view, download PDF, pay online (ticket 39, F-24; UF-29 step 6) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.CustomerLayout` |
| Route | None — opened by the portal routing |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_my_invoices`) | "My Invoices", `headline-large` | `page-title` | — | `portal.nav.invoices` |
| Invoices grid | Anvil `DataGrid` (`dg_my_invoices`) | Number, issue date, due date, total, amount paid, status | — | mobile | — |
| Invoice row | Anvil `DataRowPanel` (`drp_my_invoice_*`) | One invoice per row | — | — | — |
| Status badge | `m3.Chip` (`chip_my_invoice_status_*`, type `input`, removable=False) | The invoice's status | — | — | — |
| View button | `m3.Button` (`btn_view_my_invoice_*`, outlined) | Opens the invoice document (`invoice.pdf_url`) | `secondary-button` | — | `invoices.download` |
| Pay online button | `m3.Button` (`btn_pay_my_invoice_*`, filled) | Pays online (UF-29 step 6; the gateway's surface with the app language as locale) | `primary-button` | — | `portal.invoices.pay` |
| PayPal surface | `components.PayPalButtonComponent` | The PayPal option on the pay step | — | mobile | — |
| Help icon | `components.HelpTooltipComponent` (help entry `portal.invoices`) | The feature's tooltip and help video | — | — | `help.portal.invoices.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Invoices | `invoice` | the signed-in customer's rows via `get_my_invoices` (own only) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the invoices | `form_show` → `load_my_invoices` | `get_my_invoices` (`server_payments/invoice_service`; own only) | The grid arrives |
| View an invoice | `btn_view_my_invoice_*_click` | `invoice_service` PDF read | The PDF opens |
| Pay online | `btn_pay_my_invoice_*_click` | the invoice payment path (server-side amounts; webhooks confirm) | The payment is taken; the invoice is marked paid when the webhook lands |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Receipt icon (`mi:receipt_long`) |
| Heading | `portal.invoices.empty.title` |
| Body text | `portal.invoices.empty.body` |
| Primary action | None — invoices arrive from the business |

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
| `portal.invoices.pay` | Pay online |
| `portal.invoices.empty.title` | No invoices yet |
| `portal.invoices.empty.body` | Invoices the business sends you appear here. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Paying your invoices | `master_template` help entry `portal.invoices` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-29 Customer portal | My Invoices |
| UF-41 Customer accepts a quote | The acceptance's invoice appears here with its payment link |
