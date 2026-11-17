---
title: "15 — Shared component: PayPalButtonComponent"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-a6702c0e"
updated: "2026-11-17"
---

## Description

**Feature:** F-15 (PayPal checkout surface); shared custom component 11 of the matrix's shared list, and the last ticket of order 1.
**Depends on:** 04 (the `payment_service` seam and the PayPal adapter), 02 (phrase keys).

### Forms

- `components.PayPalButtonComponent` — loads PayPal's JavaScript SDK and renders its buttons; used by `bookings.BookingFlowForm`, `orders.OrderFlowForm` and the pay page; takes the app language as `locale`

### Server functions

- The PayPal order lifecycle the buttons drive, behind `payment_service` (created at ticket 04): `create_paypal_order`, `capture_paypal_order` (server-created and server-captured only — the browser never sets an amount; a repeated capture records one payment via `PayPal-Request-Id`)

### Tables

- `payments`, `payment_config` (read)

### Acceptance tests

- `IN-06` — server-side amounts; a repeated capture records one payment; test-plan §6
- `SR-26` — PayPal unavailable for ISK
- Test 4.20 (spec-test-specification) — PayPal checkout

### Security criteria

- B3 — the PayPal order is created and captured only by the server; every amount read from the database (AC-S10)
- Card data never touches the instance (AC-S3); PayPal is a hosted-buttons surface (SAQ A)

## Comments
