# Mybizz CS — CEO Review Task List

**Date:** 2026-05-10  
**Review Mode:** HOLD SCOPE  
**Critical Gaps:** 1  
**Total Issues:** 17  
**Status:** All tasks documented and ready for implementation

---

## Phase 0 Tasks (Priority — Must Complete Before Phase 1)

### TODO 1: Add Vault Error Handling (P1, Critical)

**Status:** ⏳ Implementation guide created, pending implementation  
**Location:** `server_code/server_shared/vault_service.py`

**What:** Add rescue block for `KeyNotFoundError` in `get_vault_secret()`  
**Why:** Prevent 500 errors when secret not found (CRITICAL GAP from Section 2)  
**Pros:** Better user experience, no 500 errors, proper error handling  
**Cons:** Minimal — 5 lines of code  
**Context:** Critical gap identified in Section 2 (Error & Rescue Map)  
**Effort:** S (human) → S (CC)  
**Priority:** P1  
**Depends on:** None  
**Blocked by:** None  

**Implementation:**
- Follow `docs/phased-implementation/phase-0-implementation.md`
- Create `server_code/server_shared/exceptions.py` with custom exception classes
- Update `vault_service.py` to rescue `KeyNotFoundError`
- Add user-friendly error message: "Vault secret not configured. Please contact the business owner."
- Write unit test for missing secret scenario

**Testing:** Add unit test for `get_vault_secret()` with missing secret

---

### TODO 2: Define Test Coverage Requirements (P1, Pre-Implementation)

**Status:** ⏳ Implementation guide created, pending implementation  
**Location:** `tests/` directory

**What:** Specify unit, integration, E2E test coverage requirements  
**Why:** Ensure critical paths are tested before launch (Section 6 finding)  
**Pros:** Confidence in launch, fewer bugs, maintainable code  
**Cons:** ~2 days of testing work  
**Context:** Section 6 review identified test strategy gap  
**Effort:** M (human) → S (CC)  
**Priority:** P1  
**Depends on:** None  
**Blocked by:** None  

**Implementation:**
- Follow `docs/phased-implementation/phase-0-implementation.md`
- Create `pytest.ini` with test configuration
- Create `tests/conftest.py` with shared fixtures
- Unit tests: 100% coverage for pure logic functions
- Integration tests: All server functions with mock data
- E2E tests: Critical flows (auth, booking, payment, email)
- Test data cleanup procedures
- Mock external services (Brevo, payment gateways)

**Testing:** Verify test coverage with coverage.py or similar

---

### TODO 3: Define Observability Requirements (P2, Pre-Implementation)

**Status:** ⏳ Implementation guide created, pending implementation  
**Location:** `server_code/server_shared/logging_config.py`

**What:** Specify logging, metrics, alerting requirements  
**Why:** Ensure platform health can be monitored (Section 8 finding)  
**Pros:** Operational visibility, faster incident response  
**Cons:** ~1 day of implementation work  
**Context:** Section 8 review identified observability gap  
**Effort:** M (human) → S (CC)  
**Priority:** P2  
**Depends on:** None  
**Blocked by:** None  

**Implementation:**
- Follow `docs/phased-implementation/phase-0-implementation.md`
- Structured logging at entry/exit for all server functions
- Metrics: bookings created, payments processed, errors, latency
- Alerts: error rates, API failures, rate limits
- Admin dashboard for platform health
- Log retention: 2 years for audit

**Testing:** Verify logs are captured, metrics are accurate, alerts fire

---

### TODO 4: Define Deployment Procedures (P2, Pre-Implementation)

**Status:** ⏳ Implementation guide created, pending implementation  
**Location:** `docs/deployment-procedures.md`

**What:** Specify migration, feature flag, rollback procedures  
**Why:** Ensure safe deployments with rollback capability (Section 9 finding)  
**Pros:** Reduced deployment risk, faster recovery  
**Cons:** ~1 day of documentation and implementation  
**Context:** Section 9 review identified deployment gap  
**Effort:** M (human) → S (CC)  
**Priority:** P2  
**Depends on:** None  
**Blocked by:** None  

**Implementation:**
- Follow `docs/phased-implementation/phase-0-implementation.md`
- Backward-compatible DB migrations (expand-and-contract pattern)
- Feature flags for gradual rollout (10%, 50%, 100%)
- Rollback procedures (Git revert, feature flag disable)
- Post-deploy verification (smoke tests, monitoring)
- Environment parity (staging matches production)

**Testing:** Test rollback procedure in staging

---

## Phase 1+ Tasks (Implementation Edge Cases — Address During Development)

### TODO 5: Add Form Debounce (P2)

**Status:** ⏳ Pending  
**What:** Add debounce to form submissions to prevent double-click  
**Why:** Prevent duplicate submissions (Section 4 finding)  
**Pros:** Better UX, no duplicate records  
**Cons:** Minimal — add JavaScript debounce  
**Context:** Section 4 review identified form submission edge case  
**Effort:** S (human) → S (CC)  
**Priority:** P2  
**Depends on:** None  
**Blocked by:** None  

**Implementation:**
- Add JavaScript debounce (300ms) to all form submit handlers
- Test with rapid double-click — should only submit once
- Reference: CEO review Section 4 (Data Flow)

---

### TODO 6: Add Empty State Messages (P2)

**Status:** ⏳ Pending  
**What:** Add empty state messages to list views  
**Why:** Better UX when no data (Section 4 finding)  
**Pros:** Clear user feedback, no confusion  
**Cons:** Minimal — add UI component  
**Context:** Section 4 review identified zero results edge case  
**Effort:** S (human) → S (CC)  
**Priority:** P2  
**Depends on:** None  
**Blocked by:** None  

**Implementation:**
- Create empty state component
- Add to all list forms (contacts, bookings, invoices, etc.)
- Include helpful call-to-action (e.g., "No contacts yet. Create one or import from CSV.")
- Test empty state displays correctly
- Reference: CEO review Section 4 (Data Flow)

---

### TODO 7: Add Partial Failure Handling (P2)

**Status:** ⏳ Pending  
**What:** Add handling for partial job failures  
**Why:** Better error handling for background jobs (Section 4 finding)  
**Pros:** Partial success, better error reporting  
**Cons:** Moderate — need job tracking  
**Context:** Section 4 review identified partial failure edge case  
**Effort:** M (human) → S (CC)  
**Priority:** P2  
**Depends on:** None  
**Blocked by:** None  

**Implementation:**
- Track job progress
- Log partial failures
- Notify admin of partial success
- Allow retry of failed items
- Test partial failure handling works
- Reference: CEO review Section 4 (Data Flow)

---

### TODO 8: Add Database Indexes (P2)

**Status:** ⏳ Pending  
**What:** Add indexes for critical queries  
**Why:** Improve performance (Section 7 finding)  
**Pros:** Faster queries, better UX  
**Cons:** Minimal — add indexes  
**Context:** Section 7 review identified index coverage gap  
**Effort:** S (human) → S (CC)  
**Priority:** P2  
**Depends on:** None  
**Blocked by:** None  

**Implementation:**
- Add index on `contacts.email`
- Add index on `bookings.contact_id`
- Add index on `bookings.service_id`
- Add index on `bookings.start_datetime` (for calendar queries)
- Verify query performance with `EXPLAIN`
- Reference: CEO review Section 7 (Performance)

---

### TODO 9: Add Pagination to Large Lists (P2)

**Status:** ⏳ Pending  
**What:** Add pagination to contact list, booking list  
**Why:** Prevent memory issues with large datasets (Section 7 finding)  
**Pros:** Better performance, consistent UX  
**Cons:** Minimal — add pagination UI  
**Context:** Section 7 review identified pagination gap  
**Effort:** S (human) → S (CC)  
**Priority:** P2  
**Depends on:** None  
**Blocked by:** None  

**Implementation:**
- 50 items per page
- Page navigation controls
- Total count display
- Test pagination works with 10,000+ items
- Reference: CEO review Section 7 (Performance)

---

### TODO 10: Verify Webhook Signature Verification (P1)

**Status:** ⏳ Pending  
**What:** Audit webhook signature verification implementation  
**Why:** Security requirement (Section 3 finding)  
**Pros:** Prevent unauthorized webhook calls  
**Cons:** Review existing code  
**Context:** Section 3 review identified webhook signature verification gap  
**Effort:** S (human) → S (CC)  
**Priority:** P1  
**Depends on:** None  
**Blocked by:** None  

**Implementation:**
- Verify Stripe webhook signature verification
- Verify Paystack webhook signature verification
- Test with invalid signatures (should be rejected)
- Reference: CEO review Section 3 (Security)

---

### TODO 11: Verify RBAC Decorator Coverage (P1)

**Status:** ⏳ Pending  
**What:** Audit RBAC decorator coverage on all server functions  
**Why:** Security requirement (Section 3 finding)  
**Pros:** Prevent unauthorized access  
**Cons:** Review existing code  
**Context:** Section 3 review identified RBAC coverage gap  
**Effort:** S (human) → S (CC)  
**Priority:** P1  
**Depends on:** None  
**Blocked by:** None  

**Implementation:**
- Review all server functions
- Verify RBAC decorators on data-modifying functions
- Verify RBAC decorators on sensitive data access
- Test access with different roles
- Reference: CEO review Section 3 (Security)

---

### TODO 12: Add External Service Failure Handling (P2)

**Status:** ⏳ Pending  
**What:** Add handling for Brevo API, payment gateway failures  
**Why:** Better error handling (Section 5 finding)  
**Pros:** Better UX, no silent failures  
**Cons:** Moderate — add retry logic, error messages  
**Context:** Section 5 review identified external service failure gap  
**Effort:** M (human) → S (CC)  
**Priority:** P2  
**Depends on:** None  
**Blocked by:** None  

**Implementation:**
- Retry logic with backoff (3 retries, exponential backoff)
- User-friendly error messages
- Admin notifications for failures
- Test with failing external services
- Reference: CEO review Section 5 (Code Quality)

---

### TODO 13: Add Backup/Restore Procedures (P2)

**Status:** ⏳ Pending  
**What:** Define and implement backup/restore procedures  
**Why:** Data protection (Section 5 finding)  
**Pros:** Data safety, faster recovery  
**Cons:** Moderate — implement backup system  
**Context:** Section 5 review identified backup/restore gap  
**Effort:** M (human) → S (CC)  
**Priority:** P2  
**Depends on:** None  
**Blocked by:** None  

**Implementation:**
- Automated backups (daily)
- Backup verification
- Restore procedures documented
- Test restore procedure in staging
- Reference: CEO review Section 5 (Code Quality)

---

### TODO 14: Verify Accessibility Requirements (P2)

**Status:** ⏳ Pending  
**What:** Verify keyboard navigation, screen reader support, contrast ratios  
**Why:** Accessibility compliance (Section 11 finding)  
**Pros:** Inclusive UX, compliance  
**Cons:** Moderate — audit and fix  
**Context:** Section 11 review identified accessibility gap  
**Effort:** M (human) → S (CC)  
**Priority:** P2  
**Depends on:** None  
**Blocked by:** None  

**Implementation:**
- Keyboard navigation audit
- Screen reader testing (NVDA or VoiceOver)
- Contrast ratio verification (≥4.5:1)
- Fix accessibility issues
- Test with screen reader, keyboard-only navigation
- Reference: CEO review Section 11 (Design)

---

### TODO 15: Verify Responsive Breakpoints (P2)

**Status:** ⏳ Pending  
**What:** Verify mobile, tablet, desktop breakpoints  
**Why:** Responsive design (Section 11 finding)  
**Pros:** Consistent UX across devices  
**Cons:** Moderate — verify and fix  
**Context:** Section 11 review identified responsive breakpoints gap  
**Effort:** S (human) → S (CC)  
**Priority:** P2  
**Depends on:** None  
**Blocked by:** None  

**Implementation:**
- Test on mobile (320px, 375px, 414px)
- Test on tablet (768px, 1024px)
- Test on desktop (1280px, 1920px)
- Fix responsive issues
- Test on multiple devices
- Reference: CEO review Section 11 (Design)

---

## Multi-Currency Implementation (ADR-012)

### TODO 16: Implement System Currency Setting (P1)

**Status:** ⏳ ADR-012 created, pending implementation  
**What:** Add `system_currency` to `business_profile` table  
**Why:** Single source of truth for all internal operations  
**Pros:** Clear accounting, flexible local pricing  
**Cons:** Currency conversion logic TBD  
**Context:** ADR-012 created, follows same pattern as ADR-005 (timezone)  
**Effort:** M (human) → M (CC)  
**Priority:** P1  
**Depends on:** None  
**Blocked by:** Currency conversion strategy TBD  

**Implementation:**
- Add `system_currency` field to `business_profile` table
- Implement onboarding flow to set system_currency
- Make system_currency immutable after first transaction
- Add optional `local_currency_price` and `local_currency` to `services` table
- Implement currency conversion logic (TODO: resolve price conversion strategy)
- Reference: `adr/012-system-currency-setting.md`

**TODO:** Price conversion needs to be resolved
- Option A: Fixed exchange rates (manual update) — Recommended for V1
- Option B: API-based rates (real-time) — V2 enhancement
- Option C: No conversion (system currency only) — Simplest

---

## Timezone Implementation (ADR-005)

### TODO 17: Apply UTC Storage Pattern (P1)

**Status:** ⏳ ADR-005 applied, pending implementation  
**What:** Add `timezone` to `business_profile`, store all datetimes in UTC  
**Why:** Global client support with consistent UTC storage  
**Pros:** Accurate time display, no timezone bugs  
**Cons:** Need timezone conversion at display time  
**Context:** ADR-005 applied to core design  
**Effort:** M (human) → M (CC)  
**Priority:** P1  
**Depends on:** None  
**Blocked by:** None  

**Implementation:**
- Add `timezone` field to `business_profile` table (IANA string)
- Ensure all datetimes stored in UTC
- Implement timezone conversion at display time in server functions
- Create helper function for timezone conversion
- Reference: `adr/05-timezone-utc-storage-display-conversion.md`

---

## Summary

**Total Tasks:** 17 (15 CEO review + 2 multi-currency/timezone)  
**Priority P1:** 6 tasks (critical gaps + pre-implementation + multi-currency)  
**Priority P2:** 11 tasks (implementation edge cases)  

**Estimated Effort:**
- P1 tasks: ~4 days total (including multi-currency/timezone)
- P2 tasks: ~5 days total
- **Total:** ~9 days before full implementation

---

## Implementation Order

1. **Phase 0** — TODO 1-4 (critical pre-implementation)
2. **Multi-Currency** — TODO 16 (ADR-012)
3. **Timezone** — TODO 17 (ADR-005)
4. **Phase 1** — Foundation (Auth, RBAC, Vault, Navigation, Settings)
5. **TODO 5-15** — Implementation edge cases (address during development)

---

## Developer Instructions

**For AI Developer:**
- Read `07-programmatic-task-list.md` for all programmatic tasks
- Follow implementation order in programmatic task list
- AI creates all code, tests, schema, configuration
- AI does NOT create forms, modules, or edit YAML files

**For Human Developer:**
- Read `09-developer-task-list.md` for all manual Designer tasks
- Follow implementation order in developer task list
- Human does drag-and-drop UI design in Anvil Designer
- Human creates packages, forms, modules in Project Manager
- Human sets component properties in Designer

**Documentation References:**
- `04-ceo-review-report.md` — CEO review findings
- `05-ceo-review-task-list.md` — This file (task list)
- `07-programmatic-task-list.md` — AI programmatic tasks
- `09-developer-task-list.md` — Human Developer tasks
- `docs/phased-implementation/phase-0-implementation.md` — Phase 0 guide
- `adr/005-timezone-utc-storage-display-conversion.md` — Timezone pattern
- `adr/012-system-currency-setting.md` — System currency pattern
- `authoritative-schema.md` — Complete 36-table schema
- `scaffold-spec.md` — Scaffold specifications
- `scaffold-workflow.md` — Scaffold workflow instructions

---

*End of file — 05-ceo-review-task-list.md (Updated 2026-05-10)*
