# ADR-13: System Currency Setting at Onboarding [SUPERSEDED]

**Status:** **Superseded** — consolidated into `016-system-currency-selection-and-immutability.md`
**Date:** 2026-05-10
**Authors:** Mybizz CS Team
Superseded by: `016-system-currency-selection-and-immutability.md` (2026-06-01) — consolidated ADR covering system currency, display currency, immutability, and conversion strategy

---

## Context

The system needs to support multi-currency operations while maintaining a single system of record for all internal calculations.

### Problem Statement

Small service businesses operate in different currencies globally. The system needs to:
1. Have a single system currency for all internal operations (accounting, reporting, analytics)
2. Allow clients to set prices in different currencies for specific services/locations
3. Handle currency conversion when needed

### Existing Design

From `authoritative-schema.md`:
- `business_profile.system_currency` — set at onboarding, immutable after first transaction
- `services.price` — price in system currency
- `services.display_currency` — optional display currency for customer-facing prices
- `invoice.currency` — transaction currency (may differ from system currency)

### Requirements

1. **System Currency (system-currency):**
   - Set during onboarding
   - Immutable after first transaction
   - Used for all internal operations (accounting, reporting, analytics)
   - Default currency for all services

2. **Local Currency (local-currency):**
   - Optional per-service override
   - Set for specific sales items at specific locations
   - Only used for selling that specific item at that currency
   - Does not affect internal calculations

3. **Currency Conversion:**
   - TODO: Price conversion needs to be resolved
   - Options:
     - Fixed exchange rates (manual update)
     - API-based rates (real-time)
     - No conversion (services priced in system currency only)

---

## Decision

### System Currency Architecture

```
ONBOARDING → system_currency set (immutable)
    ↓
ALL INTERNAL OPERATIONS use system_currency
    ↓
SERVICE PRICING:
  - Default: price in system_currency
  - Optional: local_currency override per service/location
    ↓
CUSTOMER SEES: local_currency price (if set)
    ↓
INTERNAL: system_currency calculation
```

### Schema Changes

**business_profile table:**
```
| Field | Type | Notes |
|-------|------|-------|
| system_currency | string | ISO 4217 (e.g., "ZAR", "USD") — set at onboarding, immutable after first transaction |
```

**services table:**
```
| Field | Type | Notes |
|-------|------|-------|
| price | number | Price in system_currency |
| local_currency_price | number | Optional override price in local currency |
| local_currency | string | ISO 4217 (e.g., "USD") — only if local_currency_price set |
| location_id | link → locations | Optional — if set, price applies to this location only |
```

**invoice table:**
```
| Field | Type | Notes |
|-------|------|-------|
| currency | string | ISO 4217 — transaction currency |
| total_amount | number | Amount in transaction currency |
| system_currency_amount | number | Amount converted to system_currency (for reporting) |
```

### Currency Conversion Strategy

**TODO: Price conversion needs to be resolved**

**Option A: Fixed Exchange Rates (Recommended for V1)**
- Manual entry of exchange rates by business
- Rates updated periodically (weekly/monthly)
- Pros: Simple, no external dependencies
- Cons: Manual work, rates may be outdated

**Option B: API-Based Rates**
- Integrate with currency API (e.g., exchangerate.host)
- Real-time rates
- Pros: Accurate, automated
- Cons: External dependency, API costs

**Option C: No Conversion (System Currency Only)**
- All services priced in system_currency
- Customers pay in system_currency
- Pros: Simplest, no conversion logic
- Cons: Limited flexibility

**Recommendation:** Implement Option A (Fixed Exchange Rates) for V1, with Option B as V2 enhancement.

---

## Consequences

### Positive
- Single source of truth for accounting
- Flexibility for local pricing
- Clear separation of display vs. internal currency

### Negative
- Currency conversion complexity (TODO: needs resolution)
- Need to handle exchange rate updates
- Potential customer confusion if displayed price differs from charged price

### Neutral
- System currency immutability simplifies accounting
- Local currency is optional, not required

---

## Alternatives Considered

### Alternative 1: Multi-Currency System
- Allow multiple system currencies
- Cons: Complex accounting, reporting becomes difficult

### Alternative 2: Dynamic Currency Conversion
- Convert all prices at time of transaction
- Cons: Exchange rate volatility, customer disputes

### Alternative 3: No Local Currency Support
- All services priced in system_currency only
- Pros: Simplest
- Cons: Limited global flexibility

---

## Implementation Notes

### Onboarding Flow
1. Business selects system_currency during onboarding
2. system_currency stored in business_profile
3. After first transaction, system_currency becomes immutable

### Service Pricing Flow
1. Default: service priced in system_currency
2. Optional: Set local_currency_price for specific location
3. System displays local_currency_price to customers at that location
4. All internal calculations use system_currency

### Invoice Generation Flow
1. Customer sees local_currency_price (if set)
2. Invoice generated in local_currency
3. System_currency_amount calculated and stored for reporting
4. Accounting uses system_currency_amount

---

## Related Documents

- `authoritative-schema.md` — Database schema
- `docs/phased-implementation/phase-0-implementation.md` — Phase 0 implementation guide
- `adr/05-timezone-utc-storage-display-conversion.md` — Timezone/UTC storage pattern (similar approach)

---

*End of ADR-013*
