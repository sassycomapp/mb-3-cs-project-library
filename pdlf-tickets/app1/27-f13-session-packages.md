---
title: "27 — F-13 Session packages"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-6ff16e8b"
updated: "2026-11-17"
---

## Description

**Feature:** F-13 Session packages (UF-10; SR-24; FR-13). Trade feature.
**Depends on:** 15 (PayPalButtonComponent), 21 (services), 23 (payments), 20 (contacts).

### Forms

- `packages.PackageListForm`, `packages.PackageEditorForm`

### Server functions

- `server_packages/package_service` — purchase (creates the `customer_packages` row), the session draw-down and return inside `@tables.in_transaction` (changed in the same call as each booking and cancellation)
- `server_packages/package_tasks` — expiry notices sent before expiry; expiry lapsing unused sessions

### Tables

- `packages`, `customer_packages`, `bookings` (draw-down link), `payments`

### Acceptance tests

- `SR-24`; UF-10; the customer-package lifecycle (test-plan §19); the boundary values of package balance and expiry window (§22 edge cases)

### Security criteria

- B9 — draw-down and return are transactional; a booking and its draw-down cannot half-land
- AC-S2 — a customer sees only their own packages (portal view is ticket 39)

## Comments
