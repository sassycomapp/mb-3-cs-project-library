---
title: "22 — F-16 System and local currencies"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-926ed6a9"
updated: "2026-11-18"
---

## Description

**Feature:** F-16 System and local currencies (architecture reference §12 F-16; ADR `system-currency-selection-and-immutability`; Step 12 decision D3; FR-16; SR-31).
**Depends on:** 18 (the CurrencyTab), 21 (services — the local prices' setting surface).

### Forms

- None new. `settings.SettingsForm`'s `CurrencyTab` (built at ticket 18) is completed here: the system currency, read-only (chosen and fixed in onboarding); the Local Currencies list (Owner, Manager): turn a local currency on or off for a market (currency and countries), with the client's own rate and rounding, and the local prices of every package (service local prices are entered in the service editor, ticket 21).
- The public website's country selector, which sets the customer's currency.

### Server functions

- `settings_service` currency functions — the system currency, fixed once confirmed; local currencies on and off; local prices; `price_for(item, country)`, returning the customer's currency and price: the typed local price, or the system price at the client's rate, rounded, or the system price where the country has no local currency
- The payment service records `system_amount` on every payment and refund from the gateway's report (Stripe's balance transaction, PayPal's `seller_receivable_breakdown`), and the team member enters `system_amount` for a manual payment (`integration-architecture.md`, Currency)

### Tables

- `business_profile.system_currency`, `local_currencies`, `service_local_prices`, `package_local_prices`, `config`; `currency` on `bookings`, `orders` and `quotes`; `currency` and `system_amount` on `invoice` and `payments`

### Acceptance tests

- `SR-31`; `LG-04`; `DM-07` — the six points of [[[default:adr-global/adr-system-currency-selection-and-immutability|System Currency and Local Currencies]]](C:/dev/project-library-global/adr-global/adr-system-currency-selection-and-immutability.md)
- A customer in a market with a local currency sees, pays and is refunded in that currency at exactly the price shown; every other customer pays in the system currency
- Each payment's `system_amount` equals the gateway's pay-in amount; accounts, reports, the dashboard and contact totals use `system_amount` only
- The system currency cannot change once confirmed

### Security criteria

- AC-S10 pattern — prices and currencies are read on the server from the client's settings; a currency or price sent by the browser is never used
- The plan summary is the one exception to the money rules, and it is not this feature's (ADR `plan-summary-display-text`; ticket 48)

## Comments
