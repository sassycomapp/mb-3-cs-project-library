---
title: "InvoiceViewerForm-requirement"
status: draft
doc-id: "mb-157a238b"
updated: "2026-11-25"
---
# InvoiceViewerForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `invoices.InvoiceViewerForm` |
| Purpose | The invoice's record with every architecturally defined field, and the status-appropriate actions: edit, send, record payment, refund, cancel, download PDF (ticket 23, F-15; DESIGN.md InvoiceViewerForm Completeness) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `invoices.InvoiceListForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_invoice_viewer`) | The invoice number, `headline-large` | `page-title` | — | — |
| Status badge | `m3.Chip` (`chip_status`, type `input`, removable=False) | The invoice number and status badge (DESIGN.md InvoiceViewerForm) | — | — | — |
| Bill To panel | `m3.Card` (`card_bill_to`, outlined) | Contact name, email, phone, address from the linked `contacts` record | `surface-card` | mobile | — |
| Dates panel | `m3.Card` (`card_dates`, outlined) | Invoice Date, Due Date, Payment Terms, Currency | `surface-card` | mobile | — |
| Line items panel | `m3.Card` (`card_items`, outlined) | The Line Items table: description, quantity, unit price, amount | `surface-card` | mobile | — |
| Items grid | Anvil `DataGrid` (`dg_items`) | The lines | — | mobile | — |
| Totals panel | `m3.Card` (`card_totals`, outlined) | Subtotal, Tax (shown only when the rate is above 0, with the client's tax label and registration number), Total | `surface-card` | mobile | — |
| Notes panel | `m3.Card` (`card_notes`, outlined) | The notes, if present on the invoice record | `surface-card` | mobile | — |
| Action row | Anvil `FlowPanel` (`flow_actions`) | The status-appropriate buttons (DESIGN.md's status table) | — | mobile | — |
| Edit button | `m3.Button` (`btn_edit`, outlined) | Draft only | `secondary-button` | — | `invoices.row.edit` |
| Send button | `m3.Button` (`btn_send`, outlined) | Draft only | `secondary-button` | — | `invoices.send` |
| Record Payment button | `m3.Button` (`btn_record_payment`, filled) | Sent, Overdue; the manual recording of cash, bank transfer or other (UF-14) | `primary-button` | — | `invoices.record_payment` |
| Refund button | `m3.Button` (`btn_refund`, outlined) | Paid; full or part refund (UF-14) | `secondary-button` | — | `invoices.refund` |
| Cancel Invoice button | `m3.Button` (`btn_cancel_invoice`, text) | Draft, Sent, Overdue | `tertiary-button` | — | `invoices.cancel` |
| Download PDF button | `m3.Button` (`btn_download_pdf`, outlined) | Every status; the PDF from R2 (`invoice.pdf_url`) | `secondary-button` | — | `invoices.download` |
| Record payment dialog | `m3.Dialog` (`dlg_record_payment`) | Method (cash, bank transfer, other), amount, notes | — | — | `invoices.record_payment.title` |
| Method selector | `m3.DropdownMenu` (`dd_payment_method`) | `cash` / `bank_transfer` / `other` (schema) | `outlined-select` | — | `invoices.payment_method` |
| Amount field | `m3.TextBox` (`txt_payment_amount`, type `number`, outlined) | The amount received | `outlined-input` | — | `invoices.payment_amount` |
| Refund dialog | `m3.Dialog` (`dlg_refund`) | Full or part; the refund is processed through the original gateway or recorded manually (UF-14) | — | — | `invoices.refund.title` |
| Refund amount field | `m3.TextBox` (`txt_refund_amount`, type `number`, outlined) | Never exceeds the payment (LG-04) | `outlined-input` | — | `invoices.refund_amount` |
| Help icon | `components.HelpTooltipComponent` (help entry `invoices.viewer`) | The feature's tooltip and help video | — | — | `help.invoices.viewer.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Bill To | `contacts` | `first_name`, `last_name`, `email`, `phone`, `address` (via `invoice.contact_id`) |
| Dates | `invoice` | `invoice_date`, `due_date`, `payment_terms_days`, `currency` |
| Items | `invoice_items` | `description`, `quantity`, `unit_price`, `amount` |
| Totals | `invoice` | `subtotal`, `tax_rate`, `tax_amount`, `total_amount`, `amount_paid` |
| Tax label and number | `business_profile` | `tax_label`, `tax_registration_number` |
| Notes | `invoice` | `notes` |
| PDF | `invoice` | `pdf_url` (the R2 key of the generated PDF) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the invoice | `form_show` → `load_invoice` | `invoice_service` read (`server_payments/invoice_service`) | The panels fill; the action buttons follow the status table |
| Record a payment | `btn_record_payment_click` → the dialog | `invoice_service` record payment (`server_payments/invoice_service`) | The manual payment is recorded; the invoice is marked paid when settled and a receipt is emailed (UF-14) |
| Refund | `btn_refund_click` → the dialog | `approve_refund` (`server_payments/refund_service`; step-up) | Full or part refund through the original gateway where the payment was online; never exceeds the payment; the customer is emailed |
| Cancel the invoice | `btn_cancel_invoice_click` | `invoice_service` cancel | The invoice is voided (its status becomes `cancelled`) |
| Download the PDF | `btn_download_pdf_click` | `invoice_service` PDF read | The PDF is downloaded |
| Send | `btn_send_click` | `invoice_service` send | The PDF is generated and emailed with a payment link |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Receipt icon (`mi:receipt_long`) |
| Heading | `invoices.empty.title` |
| Body text | `invoices.empty.body` — the not-found state |
| Primary action | None — back to the list |

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
| `invoices.bill_to` | Bill to |
| `invoices.record_payment` | Record Payment |
| `invoices.record_payment.title` | Record a payment |
| `invoices.payment_method` | Method |
| `invoices.payment_method.cash` | Cash |
| `invoices.payment_method.bank_transfer` | Bank transfer |
| `invoices.payment_method.other` | Other |
| `invoices.payment_amount` | Amount |
| `invoices.refund` | Refund |
| `invoices.refund.title` | Refund this payment |
| `invoices.refund_amount` | Refund amount |
| `invoices.cancel` | Cancel Invoice |
| `invoices.download` | Download PDF |
| `invoices.subtotal` | Subtotal |
| `invoices.tax_line` | {label} ({rate}%) |
| `invoices.total` | Total |
| `invoices.empty.title` | Invoice not found |
| `invoices.empty.body` | This invoice may have been removed. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Reading an invoice | `master_template` help entry `invoices.viewer` — played in the app language |
| Recording a payment by hand | `master_template` help entry `invoices.record_payment` |
| Refunding | `master_template` help entry `invoices.refund` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-14 Invoices, payment terms and refunds | The lifecycle actions: record payment, refund, cancel, PDF |
| UF-15 Payment webhooks | Online payments arrive through webhooks and appear here |
| UF-38 Error recovery | A failed payment leaves the invoice unpaid; retry with the same card or another method |
