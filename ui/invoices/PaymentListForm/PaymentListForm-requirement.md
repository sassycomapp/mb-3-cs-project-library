---
title: "PaymentListForm-requirement"
status: draft
doc-id: "mb-6c16d498"
updated: "2026-11-25"
---
# PaymentListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `invoices.PaymentListForm` |
| Purpose | Every payment and refund in one list, with its gateway reference and fee (ticket 23, F-15, F-28) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Finance & Reports → Payments) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_payments`) | "Payments", `headline-large` | `page-title` | — | `payments.title` |
| Kind filter | `m3.DropdownMenu` (`dd_kind_filter`) | All, Payments, Refunds | `outlined-select` | — | `payments.filter.kind` |
| Method filter | `m3.DropdownMenu` (`dd_method_filter`, `allow_none=True`) | Stripe, PayPal, Cash, Bank transfer, Other | `outlined-select` | — | `payments.filter.method` |
| Search field | `m3.SearchBar` (`srch_payments`) | Search by customer or reference | — | — | `payments.search` |
| Payments grid | Anvil `DataGrid` (`dg_payments`) | Date, kind, customer, invoice or booking, method, amount, fee, status | — | mobile | — |
| Payment row | Anvil `DataRowPanel` (`drp_payment_*`) | One payment or refund per row | — | — | — |
| Status badge | `m3.Chip` (`chip_payment_status_*`, type `input`, removable=False) | Pending, Succeeded, Failed as outlined pills with the word | — | — | — | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `payments.list`) | The feature's tooltip and help video | — | — | `help.payments.list.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Grid | `payments` | `kind`, `contact_id`, `invoice_id`, `booking_id`, `order_id`, `customer_package_id`, `method`, `amount`, `currency`, `status`, `gateway_reference`, `gateway_fee`, `received_at` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_payments` | `invoice_service` payments read (`server_payments/invoice_service`) | The grid arrives |
| Filter | `dd_kind_filter_change`, `dd_method_filter_change` | the same read | The grid reloads |
| Search | `srch_payments_search` | the same read | The grid reloads |
| Open the related record | the row click | None | The invoice, booking or order opens in its viewer |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Payment icon (`mi:payments`) |
| Heading | `payments.empty.title` |
| Body text | `payments.empty.body` |
| Primary action | None — payments arrive from bookings, orders and invoices |

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
| `payments.title` | Payments |
| `payments.filter.kind` | Kind |
| `payments.filter.method` | Method |
| `payments.search` | Search payments |
| `payments.kind.payment` | Payment |
| `payments.kind.refund` | Refund |
| `payments.method.stripe` | Stripe |
| `payments.method.paypal` | PayPal |
| `payments.method.cash` | Cash |
| `payments.method.bank_transfer` | Bank transfer |
| `payments.method.other` | Other |
| `payments.status.pending` | Pending |
| `payments.status.succeeded` | Succeeded |
| `payments.status.failed` | Failed |
| `payments.empty.title` | No payments yet |
| `payments.empty.body` | Payments appear here when bookings, orders and invoices are paid. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Reading the payments list | `master_template` help entry `payments.list` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-14 Invoices, payment terms and refunds | Every payment and refund, by any gateway or by hand |
| UF-15 Payment webhooks | Gateway payments appear with their references |
| UF-32 Expenses and accounts | The gateway fees shown here feed the accounts reports (F-28) |
