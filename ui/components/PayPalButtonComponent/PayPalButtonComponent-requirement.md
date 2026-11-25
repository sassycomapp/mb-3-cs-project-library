---
title: "PayPalButtonComponent-requirement"
status: draft
doc-id: "mb-04321063"
updated: "2026-11-25"
---
# PayPalButtonComponent — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `components.PayPalButtonComponent` |
| Purpose | The PayPal checkout surface: loads PayPal's JavaScript SDK and renders its buttons, used by BookingFlowForm, OrderFlowForm and the pay page; the server creates and captures the order (ticket 15, F-15) |
| Screen type and width | Custom component (embedded in the payment step of the flows); its width is its host's |
| Layout | A component form; the PayPal SDK is loaded through `anvil.js` (approved use: a third-party library behind a custom component's interface, spec-ui-standards section 3.7) |
| Route | None — a component |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Component container | Anvil `ColumnPanel` (`col_paypal`) | Anvil canonical container; holds the PayPal buttons | — | mobile | — |
| PayPal buttons | PayPal's SDK buttons (rendered into the container through `anvil.js`) | No native component loads PayPal's JavaScript SDK and its buttons (custom-component-requirements-matrix, PayPalButtonComponent); the buttons take the buyer's approval, the server holds the money logic | — | — | — |
| Locale | the SDK's `locale` parameter | The app language (spec-translation section 8) | — | — | — |
| Unavailable notice | `m3.Text` (`text_paypal_unavailable`, hidden) | PayPal is unavailable when the system currency is ISK (SR-26); the host shows the notice instead of the buttons | `body-small` | — | `payment.paypal.unavailable` |
| Loading indicator | `m3.LoadingIndicator` (`load_paypal`, hidden, contained) | Shown while the SDK loads and while the capture runs | — | — | — |

## 3. Data

| Element | Table | Column |
|---|---|---|
| PayPal client id | `payment_config` | `paypal_client_id` (public) |
| Test mode | `payment_config` | `paypal_test_mode` |
| The order's amount | the host's record | `bookings.total_price`, `orders.total_price` — read from the database by the server, never a parameter (ticket 15, B3) |
| The payment row | `payments` | `method` (`paypal`), `status`, `gateway_reference`, `gateway_fee`, `amount` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Create the order | the SDK's `createOrder` callback → component call | `create_paypal_order` (behind `payment_service`, created at ticket 04) | The server creates the PayPal order with the amount from the database; a repeated capture records one payment via `PayPal-Request-Id` (ticket 15) |
| Capture the order | the SDK's `onApprove` callback → component call | `capture_paypal_order` (behind `payment_service`) | The server captures and records the payment; the host continues its flow |
| Buyer closes the browser | the webhook path (ticket 04's handlers) | the gateway webhook handlers | The closed-browser order is captured by the webhook (test 4.20) |
| PayPal unavailable | host code | None | The notice replaces the buttons; the buyer pays another way |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the buttons render when PayPal is connected; otherwise the notice shows |
| Heading | None — not applicable |
| Body text | `payment.paypal.unavailable` when ISK or not connected |
| Primary action | None — not applicable |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | all |
| SignedOut | all |

The buttons are on the public flows' payment steps and on the pay page; every buyer reaches them (SC-08: the customer plane stays the customer plane — the buttons call only the public payment flow).

## 7. Text

| Phrase key | English text |
|---|---|
| `payment.paypal.unavailable` | PayPal is not available in your currency. Choose another way to pay. |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The PayPal buttons are the gateway's own interface; no help video is attached |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-05 Services appointment booking | The payment step's PayPal option |
| UF-09 Ordering delivered work | The order's PayPal payment |
| UF-10 Buying a session package | The package purchase's PayPal payment |
| UF-38 Error recovery | A failed or abandoned approval leaves the order unpaid; the hold or the flow's retry path applies |
