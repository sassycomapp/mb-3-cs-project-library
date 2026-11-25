---
title: "OrderViewerForm-requirement"
status: draft
doc-id: "mb-2fa612ad"
updated: "2026-11-25"
---
# OrderViewerForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `orders.OrderViewerForm` |
| Purpose | The order's record: brief, status changes, delivery files and message (ticket 26, F-12; UF-12) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `orders.OrderListForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_order_viewer`) | The order number, `headline-large` | `page-title` | — | — |
| Status badge | `m3.Chip` (`chip_order_status`, type `input`, removable=False) | The order's status | — | — | — |
| Details panel | `m3.Card` (`card_order_details`, outlined) | Customer, service, brief, brief files, total, payment status | `surface-card` | mobile | — |
| Brief files row | Anvil `FlowPanel` (`flow_brief_files`) | The customer's attachments as downloads | — | mobile | — |
| Download button | `m3.IconButton` (`ibtn_download_brief_*`, icon `mi:download`) | Downloads a brief file | `icon-button` | — | `orders.file.download` |
| Start work button | `m3.Button` (`btn_start_order`, outlined) | Sets the order to `in_progress`; the customer is emailed (UF-12 step 2) | `secondary-button` | — | `orders.start` |
| Delivery panel | `m3.Card` (`card_delivery`, outlined) | The delivery files and message | `surface-card` | mobile | — |
| Delivery message field | `m3.TextArea` (`txt_delivery_message`) | The delivery's message (UF-12 step 3) | `outlined-input` | — | `orders.delivery_message` |
| Delivery files field | `m3.FileLoader` (`fu_delivery_files`, `multiple=True`) | The delivery's files (`orders.delivery_files`) | — | — | `orders.delivery_files` |
| Deliver button | `m3.Button` (`btn_deliver_order`, filled) | Attaches the files and message, sets `delivered`; the customer is emailed with the delivery | `primary-button` | — | `orders.deliver` |
| Invoice balance button | `m3.Button` (`btn_invoice_balance`, outlined) | Any balance due is invoiced (UF-12 step 4) | `secondary-button` | — | `orders.invoice_balance` |
| Cancel order button | `m3.Button` (`btn_cancel_order`, text) | The order is cancelled, never deleted | `tertiary-button` | — | `orders.cancel` |
| Help icon | `components.HelpTooltipComponent` (help entry `orders.viewer`) | The feature's tooltip and help video | — | — | `help.orders.viewer.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Details | `orders` | all the order's columns |
| Files | `files` | `path`, `file` (brief and delivery files) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the order | `form_show` → `load_order` | `order_service` read (`server_orders/order_service`) | The panels fill |
| Start work | `btn_start_order_click` | `order_service` status change | `in_progress`; the customer is emailed |
| Deliver | `btn_deliver_order_click` | `order_service` deliver | `delivered` with the files and message; the customer is emailed with the delivery |
| Invoice the balance | `btn_invoice_balance_click` | `order_service` balance invoicing | The balance invoice is created (UF-14) |
| Cancel | `btn_cancel_order_click` | `order_service` cancel | The order is cancelled; its history is kept |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Package icon (`mi:inventory_2`) |
| Heading | `orders.empty.title` |
| Body text | `orders.empty.body` — the not-found state |
| Primary action | None — back to the list |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

Staff own only.

## 7. Text

| Phrase key | English text |
|---|---|
| `orders.start` | Start work |
| `orders.deliver` | Deliver |
| `orders.delivery_message` | Message to the customer |
| `orders.delivery_files` | Delivery files |
| `orders.file.download` | Download |
| `orders.invoice_balance` | Invoice the balance |
| `orders.cancel` | Cancel order |

## 8. Help Videos

| Process | Help video |
|---|---|
| Delivering an order | `master_template` help entry `orders.viewer` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-12 Managing an order | The whole flow: review the brief, start, deliver, invoice the balance |
