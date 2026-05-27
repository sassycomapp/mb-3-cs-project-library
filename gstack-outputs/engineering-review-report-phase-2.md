# Mybizz CS — Phase 2 Engineering Review Report

**Date:** 2026-05-26  
**Review Type:** Engineering Review (Phase 2 Transactional Core)  
**Branch:** N/A (project-library documentation review)  
**Reviewer:** Build Agent (gstack)

---

## Executive Summary

**Verdict:** ✅ **APPROVED** — All architectural decisions validated, ready for implementation

Phase 2 Engineering Review completed for Transactional Core. 3 key decisions made and approved:

1. ✅ **Payment webhook idempotency** — Full state machine implementation (3-4 hours)
2. ✅ **Booking state machine** — Full state machine with explicit transitions (4-5 hours)
3. ✅ **Invoice PDF generation** — Anvil built-in PDF (2-3 hours)

**Phase Scope:** 6 Stages (Services, Availability, Bookings, Payments, Invoicing, Time Tracking)  
**Files to Implement:** ~12-15 files  
**Estimated Implementation Time:** 25-35 hours

---

## Issues Found & Decisions Made

### Issue 1: Payment Webhook Idempotency ✅ DECIDED

**Finding:** [P1] (confidence: 9/10) `server_payments/stripe_service.py` — No webhook idempotency pattern specified

**Problem:**
Payment webhooks can be delivered multiple times (network retries, gateway retries). Without idempotency:
- Double charges to customers
- Duplicate bookings created
- Invoice status inconsistencies
- Race conditions in state transitions

**Decision:** **Full idempotency with state machine** (3-4 hours)

**Rationale:** Payment processing is a one-way door decision — once customers are charged, you cannot undo it without refunds and reputation damage. Idempotency is non-negotiable for payment systems.

**Implementation Plan:**
- Store webhook ID in `webhook_log` table with `processed` flag
- Check for duplicate webhook IDs before processing
- Implement idempotent payment state machine:
  - `PENDING` → `PROCESSING` → `COMPLETED` or `FAILED`
  - Atomic state transitions with database locks
  - Retry logic with exponential backoff for transient failures
- Webhook verification with signature validation (TODO 10)

---

### Issue 2: Booking State Machine ✅ DECIDED

**Finding:** [P2] (confidence: 9/10) `server_bookings/service.py` — Booking state transitions not specified

**Problem:**
Booking flow has multiple states (PENDING → CONFIRMED → COMPLETED/CANCELLED) but no explicit state machine:
- No validation of valid state transitions
- No prevention of invalid state changes (e.g., CANCELLED → CONFIRMED)
- No audit trail of state changes
- No business rule enforcement at state transition points

**Decision:** **Full state machine with explicit transitions** (4-5 hours)

**Rationale:** Booking management is core to the business. A state machine ensures data integrity, prevents invalid states, and provides clear audit trail for debugging and compliance.

**Implementation Plan:**
- Define booking state machine:
  ```
  PENDING → CONFIRMED → COMPLETED
           ↓          ↓
        CANCELLED  CANCELLED
  ```
- Implement state transition validation:
  - `PENDING` → `CONFIRMED` (only after payment)
  - `CONFIRMED` → `COMPLETED` (only after service delivery)
  - Any state → `CANCELLED` (with reason)
  - No other transitions allowed
- Add `booking_state_history` table for audit trail
- Enforce transitions in server functions with guards
- Add business rule validation at each transition (e.g., cancellation policy)

---

### Issue 3: Invoice PDF Generation ✅ DECIDED

**Finding:** [P2] (confidence: 8/10) `server_payments/invoice_service.py` — PDF generation method not specified

**Problem:**
Build-plan.md requires "Invoice PDF generation functional" but doesn't specify implementation approach:
- Anvil built-in PDF (fastest, least flexible)
- External library (most control, adds dependency)
- HTML-to-PDF via browser (middle ground)

**Decision:** **Anvil built-in PDF generation** (2-3 hours)

**Rationale:** Anvil built-in PDF is sufficient for V1 invoice requirements. External libraries add dependency management overhead and complexity. Can be upgraded to external library in V2 if needed.

**Implementation Plan:**
- Use Anvil's built-in PDF generation for invoices
- Template-based invoice design with M3 styling
- Include: business logo, invoice details, line items, totals, payment status
- Email PDF via Brevo SMTP (configured in Phase 0)
- Store PDF URL in `invoice.pdf_url` field

---

## Architecture Review Summary

**Issues Found:** 3  
**Issues Resolved:** 3 (all decisions approved)  
**Overall Architecture Rating:** 8/10 → 9/10 (after fixes)

### Section 2: Code Quality Review

**Phase 2 Deliverables Analysis:**

| Stage | Files | Complexity | DRY Violations | Error Handling |
|-------|-------|------------|----------------|----------------|
| 2.1 Services | 3 forms + 1 service | Low | ✅ None | ✅ Complete |
| 2.2 Availability | 2 tables + 1 form | Low | ✅ None | ✅ Complete |
| 2.3 Bookings | 4 forms + 1 service | Medium | ✅ None | ✅ Complete |
| 2.4 Payments | 2 gateways + webhooks | High | ⚠️ Gateway duplication | ✅ Complete |
| 2.5 Invoicing | 3 forms + 1 service | Medium | ✅ None | ✅ Complete |
| 2.6 Time Tracking | 1 form + 1 service | Low | ✅ None | ✅ Complete |

**Recommendations:**
- ✅ **No DRY violations** — Each service has clear responsibility
- ⚠️ **Gateway duplication** — Consider abstracting common payment logic to `server_payments/payment_service.py`
- ✅ **Complete error handling** — All stages include error handling in completion criteria

---

## Section 3: Test Review

**Test Coverage Requirements for Phase 2:**

### Stage 2.1: Service Catalogue Tests
```python
tests/test_service_service.py:
test_service_creation_duration_pricing
test_service_creation_unit_pricing
test_service_category_management
test_service_provider_assignment
test_service_public_listing
test_service_detail_page
```

### Stage 2.2: Availability Management Tests
```python
tests/test_availability_service.py:
test_availability_rules_creation
test_availability_exceptions_creation
test_availability_conflict_detection
test_availability_buffer_time
test_availability_advance_window
```

### Stage 2.3: Booking System Tests
```python
tests/test_booking_service.py:
test_booking_creation_flow
test_booking_status_transitions
test_recurring_appointments
test_booking_conflict_detection
test_contact_auto_creation
test_booking_metadata_schemas
```

### Stage 2.4: Payment Integration Tests
```python
tests/test_payment_service.py:
test_stripe_payment_processing
test_paystack_payment_processing
test_webhook_idempotency
test_webhook_signature_verification
test_payment_state_machine
test_external_service_failure_retry
```

### Stage 2.5: Invoicing System Tests
```python
tests/test_invoice_service.py:
test_invoice_generation_on_booking
test_manual_invoice_creation
test_invoice_pdf_generation
test_invoice_email_delivery
test_invoice_status_transitions
test_multi_currency_display
```

### Stage 2.6: Time Tracking Tests
```python
tests/test_time_service.py:
test_time_entry_creation
test_staff_attribution
test_billable_vs_non_billable
test_time_entry_linking_to_bookings
```

**Total Test Cases:** ~30 test functions

---

## Section 4: Performance Review

**Performance Considerations for Phase 2:**

| Concern | Impact | Mitigation |
|---------|--------|------------|
| **Webhook processing** | High | Idempotency + async processing |
| **Booking conflicts** | High | Database-level locking |
| **PDF generation** | Medium | Anvil built-in (fast) |
| **Payment retries** | Medium | Exponential backoff |
| **Calendar queries** | Low | Indexed datetime fields |

**Recommendations:**
- ✅ **Webhook processing** — Use idempotency to prevent duplicate processing
- ✅ **Booking conflicts** — Implement database-level locking with `SELECT ... FOR UPDATE`
- ✅ **PDF generation** — Anvil built-in is sufficient for V1
- ✅ **Payment retries** — Exponential backoff with jitter
- ✅ **Calendar queries** — Add indexes on `bookings.start_datetime`, `bookings.resource_id`

---

## Engineering Decisions Summary

| Decision | Status | Time Estimate |
|----------|--------|---------------|
| Payment webhook idempotency | ✅ Approved | 3-4 hours |
| Booking state machine | ✅ Approved | 4-5 hours |
| Invoice PDF generation | ✅ Approved | 2-3 hours |
| Gateway abstraction (recommended) | ⏸️ Pending | 1-2 hours |
| Database indexes (recommended) | ⏸️ Pending | 30 min |

**Total Estimated Time:** 25-35 hours

---

## Test Coverage Diagram

```
Phase 2 Code Paths:
├── Service Catalogue (2.1)
│   ├── Service creation (duration/unit pricing)
│   ├── Category management
│   ├── Provider assignment
│   └── Public listing
│
├── Availability Management (2.2)
│   ├── Rules creation
│   ├── Exceptions creation
│   ├── Conflict detection
│   └── Buffer time calculation
│
├── Booking System (2.3)
│   ├── Booking creation flow
│   ├── State transitions (PENDING→CONFIRMED→COMPLETED/CANCELLED)
│   ├── Recurring appointments
│   ├── Conflict detection
│   └── Contact auto-creation
│
├── Payment Integration (2.4)
│   ├── Stripe processing
│   ├── Paystack processing
│   ├── Webhook handling (idempotent)
│   ├── Signature verification
│   └── Retry logic
│
├── Invoicing System (2.5)
│   ├── Auto-generation on booking
│   ├── Manual creation
│   ├── PDF generation
│   ├── Email delivery
│   └── Status transitions
│
└── Time Tracking (2.6)
    ├── Time entry creation
    ├── Staff attribution
    ├── Billable vs non-billable
    └── Linking to bookings
```

**Every branch in this diagram needs a test.**

---

## Next Steps

**Before Implementation:**
1. ✅ Payment webhook idempotency — Full state machine
2. ✅ Booking state machine — Full state machine
3. ✅ Invoice PDF generation — Anvil built-in

**Implementation Order:**
1. Stage 2.1: Service Catalogue (4-6 hours)
2. Stage 2.2: Availability Management (3-4 hours)
3. Stage 2.3: Booking System (6-8 hours)
4. Stage 2.4: Payment Integration (8-10 hours)
5. Stage 2.5: Invoicing System (4-6 hours)
6. Stage 2.6: Time Tracking (2-3 hours)

**Total Estimated Time:** 27-37 hours

---

## Sign-Off

**Engineering Review Completed:** 2026-05-26  
**Review Status:** ✅ APPROVED  
**Issues Found:** 3 (all resolved)  
**Files to Implement:** ~12-15 files  
**Estimated Implementation Time:** 25-35 hours  
**Next Review:** Phase 3 Engineering Review (CRM + Email)

---

*End of file — Phase 2 Engineering Review Report*
