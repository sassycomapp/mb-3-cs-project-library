---
title: "39 — F-24 Customer portal"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-d1ee3609"
updated: "2026-11-17"
---

## Description

**Feature:** F-24 Customer portal (UF-29; scaffold-spec portal package and CustomerLayout menu; SR-40; FR-24). Common feature reading trade tables through its own server functions (ADR `common-feature-reads-trade-data`).
**Depends on:** 16 (customer sign-in), 24 (bookings), 27 (packages), 23 (invoices), 37 (reviews), 38 (quotes).

### Forms

- `portal.MyDashboardForm` — upcoming bookings, package balances, open quotes, invoices due
- `portal.MyBookingsForm`, `portal.MyPackagesForm`, `portal.MyQuotesForm`, `portal.MyInvoicesForm` (view, download PDF, pay online), `portal.MyReviewsForm`, `portal.AccountForm` (profile, password, email preferences, morning briefing switch)

### Server functions

- The portal's own customer functions: `get_my_bookings` (booking_service), `get_my_invoices` (invoice_service), and the package/quote/review reads — every one derives the customer from the signed-in user, never from a client-supplied id
- `CustomerLayout`'s menu (ticket 01) carries the portal navigation

### Tables

- `bookings`, `customer_packages`, `quotes`, `invoice`, `reviews`, `users`

### Acceptance tests

- `SR-40`; UF-29; `IN-02`, `IN-03`, `BR-03` — every view shows only the signed-in customer's own records; a Customer cannot call any back-office function (AC-S15)

### Security criteria

- B1, B2 — the Customer role's scopes exactly; row scoping by derivation from `anvil.users.get_user()`
- AC-S2 — the one direction the common-reads ADR allows, enforced own-records-only

## Comments
