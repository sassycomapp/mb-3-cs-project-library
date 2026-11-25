---
title: "InvoiceListForm-requirement"
status: draft
doc-id: "mb-478a2c6f"
updated: "2026-11-25"
---
# InvoiceListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `invoices.InvoiceListForm` |
| Purpose | Invoice discovery and status awareness: search, filter by status, row actions, and the create action (ticket 23, F-15; DESIGN.md InvoiceListForm Scope) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_invoices`) | "Invoices", `headline-large` | `page-title` | — | `invoices.title` |
| Search field | `m3.SearchBar` (`srch_invoices`) | Search by invoice number or customer name (DESIGN.md InvoiceListForm) | — | — | `invoices.search` |
| Status filter | `m3.DropdownMenu` (`dd_status_filter`) | All, Draft, Sent, Paid, Overdue, Cancelled (DESIGN.md InvoiceListForm) | `outlined-select` | — | `invoices.filter.status` |
| Create Invoice button | `m3.Button` (`btn_create_invoice`, filled) | "+ Create Invoice" opens InvoiceEditorForm in create mode (DESIGN.md InvoiceListForm) | `primary-button` | — | `invoices.create` |
| Invoice grid | Anvil `DataGrid` (`dg_invoices`) | Number, customer, issue date, due date, total, amount paid, status | — | mobile | — |
| Invoice row | Anvil `DataRowPanel` (`drp_invoice_*`) | One invoice per row | — | — | — |
| Status badge | `m3.Chip` (`chip_status_*`, type `input`, removable=False) | The invoice status as an outlined pill with its word | — | — | — |
| Row actions | `m3.IconButton` (`ibtn_view_*`, `ibtn_edit_*`) | View (all statuses), Edit (Draft only) (DESIGN.md InvoiceListForm) | `icon-button` | — | `invoices.row.view`, `invoices.row.edit` |
| Help icon | `components.HelpTooltipComponent` (help entry `invoices.list`) | The feature's tooltip and help video | — | — | `help.invoices.list.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Grid | `invoice` | `invoice_number`, `contact_id`, `invoice_date`, `due_date`, `total_amount`, `amount_paid`, `status`, `currency` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_invoices` | `invoice_service` list read (`server_payments/invoice_service`) | The grid arrives (server-side filtering) |
| Search | `srch_invoices_search` | the same read | The grid reloads |
| Filter by status | `dd_status_filter_change` | the same read | The grid reloads |
| View an invoice | `ibtn_view_*_click` | None | `invoices.InvoiceViewerForm` opens |
| Edit a draft | `ibtn_edit_*_click` | None | `invoices.InvoiceEditorForm` opens for the draft (Edit only for Draft) |
| Create an invoice | `btn_create_invoice_click` | None | `invoices.InvoiceEditorForm` opens in create mode |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Receipt icon (`mi:receipt_long`) |
| Heading | `invoices.empty.title` (DESIGN.md: "No invoices yet") |
| Body text | `invoices.empty.body` (DESIGN.md: "Invoices will appear here when you create them for your bookings.") |
| Primary action | `invoices.create` (+ Create Invoice) |

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
| `invoices.title` | Invoices |
| `invoices.search` | Search by number or customer |
| `invoices.filter.status` | Status |
| `invoices.create` | + Create Invoice |
| `invoices.row.view` | View |
| `invoices.row.edit` | Edit |
| `invoices.status.draft` | Draft |
| `invoices.status.sent` | Sent |
| `invoices.status.paid` | Paid |
| `invoices.status.overdue` | Overdue |
| `invoices.status.cancelled` | Cancelled |
| `invoices.empty.title` | No invoices yet |
| `invoices.empty.body` | Invoices will appear here when you create them for your bookings. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Finding and managing invoices | `master_template` help entry `invoices.list` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-14 Invoices, payment terms and refunds | The invoice list; row actions per status |
| UF-41 Customer accepts a quote | Accepted quotes' invoices appear here |
| UF-11 Admin daily operations | The morning routine's outstanding invoices |
