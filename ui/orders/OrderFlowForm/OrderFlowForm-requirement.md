---
title: "OrderFlowForm-requirement"
status: draft
doc-id: "mb-babbe78c"
updated: "2026-11-25"
---
# OrderFlowForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `orders.OrderFlowForm` |
| Purpose | The customer order for delivered work: details, brief (text and attachments), payment by the service's payment option; submits once (ticket 26, F-12; UF-09) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.PublicLayout` (entered from a `unit`-priced service's public page) |
| Route | Opened from the public service page |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Flow container | Anvil `ColumnPanel` (`col_order_flow`) | The order's one flow | — | mobile | — |
| Service summary card | `m3.Card` (`card_service_summary`, elevated) | The service's name, price and description | `elevated-card` | mobile | — |
| Details fields | `m3.TextBox` (`txt_customer_name`, type `email` `txt_customer_email`, type `tel` `txt_customer_phone`) | The customer's details; signed-in customers are pre-filled | `outlined-input` | mobile | `booking.flow.name`, `booking.flow.email`, `booking.flow.phone` |
| Brief text field | `m3.TextArea` (`txt_brief`) | The work's brief (UF-09 step 2) | `outlined-input` | — | `orders.brief` |
| Brief attachments | `m3.FileLoader` (`fu_brief_files`, `multiple=True`) | The brief's attachments (`orders.brief_files` → `files`) | — | — | `orders.brief_files` |
| Payment panel | `m3.Card` (`card_payment`, outlined) | The service's payment option: in full, deposit, or invoice with payment terms (UF-09 step 3) | `surface-card` | mobile | — |
| Stripe fields | the gateway's Elements (locale = the app language) | The card payment surface | — | mobile | — |
| PayPal surface | `components.PayPalButtonComponent` | The PayPal option's buttons | — | mobile | — |
| Terms acceptance box | `m3.Checkbox` (`cb_terms`, never ticked) + two `m3.Link` | The unticked acceptance box with both documents linked; the confirm button stays disabled until ticked (DESIGN.md Terms Acceptance Box) | — | — | `consent.terms.checkbox` |
| Confirm button | `m3.Button` (`btn_confirm_order`, filled) | Submits once (UF-09: the order is placed and paid) | `primary-button` | — | `orders.confirm` |
| Confirmation card | `m3.Card` (`card_order_confirmation`, elevated) | The order number and the confirmation email note | `elevated-card` | mobile | — |
| Loading indicator | `m3.LoadingIndicator` (`load_order`, hidden, contained) | During the submit | — | — | — |
| Retry message | `m3.Text` (`text_order_retry`, hidden) | The timeout's retry message; `order_flow_state` is preserved | `body-small` | — | `booking.flow.timeout` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The order | `orders` | `order_number`, `contact_id`, `service_id`, `brief`, `brief_files`, `status` (`ordered`), `total_price`, `payment_option`, `payment_status`, `ordered_at` |
| Files | `files` | `path`, `file` |
| Flow state | `shared/globals.py` | `order_flow_state` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the flow | `form_show` → `load_order_context` | `order_service` read (`server_orders/order_service`) | The service's public details arrive |
| Submit | `btn_confirm_order_click` | `order_service` create | The order is created (with the contact created or matched); the status is `ordered`; the confirmation email is sent; payment is taken per the option |
| Accept terms | `cb_terms_change` | None | The confirm button enables; the `consents` rows are written with the contact |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Package icon (`mi:inventory_2`) |
| Heading | `orders.flow.unavailable.title` |
| Body text | `orders.flow.unavailable.body` — when the service is not orderable |
| Primary action | `public.nav.services` |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | all |
| SignedOut | all |

The flow is public (UF-09).

## 7. Text

| Phrase key | English text |
|---|---|
| `orders.brief` | Tell us about the work |
| `orders.brief_files` | Attach files |
| `orders.confirm` | Confirm & Pay |
| `orders.confirm_no_pay` | Confirm Order |
| `orders.flow.unavailable.title` | This service can't be ordered online |
| `orders.flow.unavailable.body` | Contact the business to order this service. |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The order flow is a public customer flow; no in-app help video is attached |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-09 Ordering delivered work | The whole flow: read, order, brief, pay, confirmation |
| UF-12 Managing an order | The client manages the order afterwards |
| UF-20 Lead capture | The source is carried into the order |
