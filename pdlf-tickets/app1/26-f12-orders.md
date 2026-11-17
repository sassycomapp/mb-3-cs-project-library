---
title: "26 — F-12 Orders for delivered work"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-49cadba9"
updated: "2026-11-17"
---

## Description

**Feature:** F-12 Orders for delivered work (UF-09, UF-12; SR-23; FR-12). Trade feature.
**Depends on:** 15 (PayPalButtonComponent), 21 (services), 23 (payments), 20 (contacts).

### Forms

- `orders.OrderFlowForm` — the customer order: details, brief (text and attachments), payment by the service's payment option; submits once
- `orders.OrderListForm`, `orders.OrderViewerForm` — status changes, delivery files and message

### Server functions

- `server_orders/order_service` — create (with the contact created or matched), status transitions (`ordered` → `in_progress` → `delivered`; `cancelled`), delivery with files and message, balance invoicing

### Tables

- `orders`, `files` (brief and delivery files), `services`, `contacts`, `payments`, `invoice` (balance)

### Acceptance tests

- `SR-23`; UF-09, UF-12; the order lifecycle (test-plan §19); `IN-02` (Staff own)

### Security criteria

- B1, AC-S2 — row scoping per the access matrix
- B9 — the status transitions and any balance invoicing are transactional where contended
- Rows are never deleted; the status changes (schema Rules)

## Comments
