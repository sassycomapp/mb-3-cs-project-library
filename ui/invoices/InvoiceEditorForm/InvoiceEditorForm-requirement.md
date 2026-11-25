---
title: "InvoiceEditorForm-requirement"
status: draft
doc-id: "mb-116f5d35"
updated: "2026-11-25"
---
# InvoiceEditorForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `invoices.InvoiceEditorForm` |
| Purpose | Create and edit draft invoices: customer, dates, payment terms, currency, line items (typed, or pulled from time entries, an order or a package), and notes (ticket 23, F-15; DESIGN.md InvoiceEditorForm Scope) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `invoices.InvoiceListForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_invoice_editor`) | "New invoice" / the invoice number, `headline-large` | `page-title` | — | `invoices.new` |
| Details panel | `m3.Card` (`card_details`, outlined) | Customer, dates, terms, currency | `surface-card` | mobile | — |
| Customer selector | `m3.DropdownMenu` (`dd_customer`) | The contact the invoice is for | `outlined-select` | — | `invoices.customer` |
| Issue date field | `m3.DatePicker` (`dp_issue_date`, outlined) | The issue date | `outlined-date` | — | `invoices.issue_date` |
| Payment terms field | `m3.TextBox` (`txt_terms_days`, type `number`, outlined) | The days until due; 0 = due on receipt; the default comes from Settings → Invoicing | `outlined-input` | — | `invoicing.payment_terms` |
| Due date display | `m3.Text` (`text_due_date`) | The calculated due date | `body-medium` | — | `invoices.due_date` |
| Currency display | `m3.Text` (`text_currency`) | The system currency (read-only) | `body-medium` | — | `invoices.currency` |
| Lines panel | `m3.Card` (`card_lines`, outlined) | The line items | `surface-card` | mobile | — |
| Line items grid | Anvil `DataGrid` (`dg_items`) | Description, quantity, unit price, amount | — | mobile | — |
| Line item row | Anvil `DataRowPanel` (`drp_item_*`) | One line per row, editable for drafts | — | — | — |
| Pull-in menu | `m3.SplitButton` (`split_pull_in`, text "Add from") | Pull in time entries, an order balance or a package (DESIGN.md InvoiceEditorForm) | — | — | `invoices.pull_in` |
| Add line button | `m3.Button` (`btn_add_line`, outlined) | Adds a typed line | `secondary-button` | — | `invoices.line.add` |
| Tax summary | `m3.Text` (`text_tax_summary`) | Subtotal, tax (shown only when the rate is above 0, with the client's tax label), total | `body-medium` | — | `invoices.subtotal`, `invoices.tax_line`, `invoices.total` |
| Notes field | `m3.TextArea` (`txt_notes`, outlined) | The notes shown on the invoice | `outlined-input` | — | `invoices.notes` |
| Save Draft button | `m3.Button` (`btn_save_draft`, filled) | Sets status to `draft` (DESIGN.md InvoiceEditorForm) | `primary-button` | — | `invoices.save_draft` |
| Send Invoice button | `m3.Button` (`btn_send_invoice`, outlined) | Sets status to `sent` and triggers email delivery (DESIGN.md InvoiceEditorForm) | `secondary-button` | — | `invoices.send` |
| Cancel button | `m3.Button` (`btn_cancel_invoice`, text) | Returns to the list without saving (DESIGN.md InvoiceEditorForm) | `tertiary-button` | — | `editmode.bar.cancel` |
| Help icon | `components.HelpTooltipComponent` (help entry `invoices.editor`) | The feature's tooltip and help video | — | — | `help.invoices.editor.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Details | `invoice` | `contact_id`, `invoice_date`, `payment_terms_days`, `due_date`, `currency`, `notes` |
| Lines | `invoice_items` | `description`, `quantity`, `unit_price`, `amount`, `time_entry_id`, `sort_order` |
| Totals | `invoice` | `subtotal`, `tax_rate`, `tax_amount`, `total_amount` — fixed at issue |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the invoice | `form_show` → `load_invoice` | `invoice_service` read (`server_payments/invoice_service`) | Edit works only on drafts (status must be `draft`) |
| Save Draft | `btn_save_draft_click` | `create_invoice` (`server_payments/invoice_service`; Owner/Manager) | The invoice is stored with status `draft`; numbering through the `config` sequence keys inside a transaction; tax fixed at issue |
| Send Invoice | `btn_send_invoice_click` | `create_invoice` + the send path | The PDF is generated server-side (`invoice_pdf`, reportlab) and emailed with a payment link; status `sent` |
| Pull in time entries | `split_pull_in_item_click` | `invoice_service` read of uninvoiced entries | The entries become lines; they are marked invoiced in the same call that creates the lines (F-14) |
| Pull in an order balance / a package | `split_pull_in_item_click` | `invoice_service` read | The balance line is added |
| Add a typed line | `btn_add_line_click` | None | A blank line is added |
| Cancel | `btn_cancel_invoice_click` | None | The list opens without saving |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the editor always shows its fields |
| Heading | None — not applicable |
| Body text | None — not applicable |
| Primary action | None — not applicable |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix; writable fields per `writable_fields.invoice` (status, notes, payment_terms_days).

## 7. Text

| Phrase key | English text |
|---|---|
| `invoices.new` | New invoice |
| `invoices.customer` | Customer |
| `invoices.issue_date` | Invoice date |
| `invoices.due_date` | Due {date} |
| `invoices.currency` | Currency |
| `invoices.pull_in` | Add from |
| `invoices.pull_in.time` | Time entries |
| `invoices.pull_in.order` | Order balance |
| `invoices.pull_in.package` | Package |
| `invoices.line.add` | + Add line |
| `invoices.line.description` | Description |
| `invoices.line.quantity` | Qty |
| `invoices.line.unit_price` | Unit price |
| `invoices.line.amount` | Amount |
| `invoices.subtotal` | Subtotal |
| `invoices.tax_line` | {label} ({rate}%) |
| `invoices.total` | Total |
| `invoices.notes` | Notes |
| `invoices.save_draft` | Save Draft |
| `invoices.send` | Send Invoice |

## 8. Help Videos

| Process | Help video |
|---|---|
| Creating an invoice | `master_template` help entry `invoices.editor` — played in the app language |
| Pulling in time entries | `master_template` help entry `invoices.pull_in` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-14 Invoices, payment terms and refunds | Create manually; the due date from the terms; the tax at the client's rate |
| UF-13 Time tracking and invoicing hours | The pull-in of billable, uninvoiced entries |
| UF-41 Customer accepts a quote | Not this form — acceptance creates the invoice on the server |
