---
title: "22 — F-16 Dual currency"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-926ed6a9"
updated: "2026-11-18"
---

## Description

**Feature:** F-16 Dual currency (architecture reference §12 F-16; ADR `system-currency-selection-and-immutability`; Step 12 decision D3; FR-16; SR-31).
**Depends on:** 18 (the CurrencyTab), 21 (services — the display currency's setting surface).

### Forms

- None new. `settings.SettingsForm`'s `CurrencyTab` (built at ticket 18) is completed here: system currency select-and-confirm while no booking or invoice exists, read-only once locked; display currency per service and business-wide, display-only.

### Server functions

- `settings_service` currency functions — the system-currency lock check (locked once the first booking or invoice exists); no conversion column is built (D3)

### Tables

- `business_profile.system_currency`, `services.display_currency`, `config`

### Acceptance tests

- `SR-31`; `LG-04` — every stored amount is whole cents of the system currency; the display currency is display-only
- The lock: once a booking or invoice exists, the system currency cannot change; the lock's full proof lands with tickets 23 and 24's data checks (the currency check appears in their confirmation tests)

### Security criteria

- AC-S10 pattern — the lock is server-side; a client-supplied currency never changes a stored amount
- The plan summary is the one exception to the money rules, and it is not this feature's (ADR `plan-summary-display-text`; ticket 48)

## Comments
