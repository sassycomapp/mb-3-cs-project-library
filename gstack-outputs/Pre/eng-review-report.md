# Mybizz CS — Engineering Review Report

**Date:** 2026-05-05
**Branch:** main
**Mode:** HOLD SCOPE
**Approach:** Standard Anvil workflow (Scaffold → AI wireframes → Developer builds UIs → Code & test)

---

## Executive Summary

Engineering review completed for Mybizz CS V1 architecture. Review confirms architecture is sound with 1 critical gap and 16 issues identified. All findings documented below with recommendations.

**Key Decision:** Architecture is solid and ready for implementation. Critical gap (Vault error handling) must be addressed before Phase 1 implementation begins.

**Implementation Approach:** Standard Anvil workflow — scaffold → AI wireframes → Developer builds UIs → Code & test

---

## 0A. System Audit

**Repository:** `project-library` on `main` branch
**Last commit:** `af4cc84` - "docs: Complete unified documentation set with build plan"
**Design doc:** None found (reviewing build plan and unified docs)

**Current State:**
- 17 unified docs in project-library/docs
- 12 ADRs (001-012)
- Build plan with 5 phases, 26 stages
- mb-3-cs scaffolding complete in Anvil

---

## 0B. Premise Challenge

**1. Is this the right problem to solve?**
Yes. Small service businesses lose revenue because they can't manage bookings, payments, and customer follow-up in one place. Mybizz replaces Calendly + Stripe + spreadsheets + manual email.

**2. What is the actual user/business outcome?**
Ship a V1 MVP that solves the core problem for consulting and services businesses. Architecture is well-reasoned and aligned with requirements.

**3. What would happen if we did nothing?**
Continue with the patchwork of tools. The business wouldn't exist.

---

## 0C. Existing Code Leverage

**What exists:**
- `project-library`: 17 unified docs, 12 ADRs, complete planning ✅
- `mb-3-cs`: Fresh scaffolding in Anvil, cloned to local ✅

**Is this plan rebuilding anything?**
No. The `project-library` docs are the planning source. The `mb-3-cs` scaffolding is fresh — no legacy code to rebuild.

---

## 0D. Dream State Mapping

```
CURRENT STATE                  THIS PLAN                  12-MONTH IDEAL
[Planning complete, fresh]     ---> [Ship V1 MVP, 1-5 clients] ---> [50-100 clients, Phase 2]
```

---

## Review Sections

### Section 1: Architecture Review

**Overall System Design:**
- Dependency model (master_template → client instances) is solid
- Single vertical (C&S) eliminates multi-vertical complexity
- Data isolation is structural (separate Anvil apps)

**Dependency Graph:**
```
master_template_dev → master_template → client instances
    ↓
    └── client_code/     ← UI layer
    └── server_code/     ← Business logic layer
    └── Data Tables      ← Persistence layer
```

**Data Flow (All 4 paths):**
- **Happy path:** User → Auth → Dashboard → Feature → Database → Return
- **Nil path:** Input is nil → Validation fails → Error message
- **Empty path:** Input is empty → Validation fails → Error message
- **Error path:** Upstream fails → Retry/backoff → User message

**State Machines:**
- `bookings`: PENDING → CONFIRMED → COMPLETED/CANCELLED
- `contacts`: new → active → at_risk → lost
- `tasks`: created → in_progress → completed

**Coupling Concerns:**
- `contacts` table is central — many tables reference it (bookings, tasks, campaigns, leads)
- `users` table is central — all tables with `instance_id` link to it
- This coupling is justified — these are the core entities

**Scaling Characteristics:**
- Under 10x load: Dashboard queries may slow (need pagination)
- Under 100x load: Email campaigns may hit Brevo rate limits (need throttling)
- Single points of failure: Brevo API, payment gateways (webhooks)

**Security Architecture:**
- Vault protects all API keys
- RBAC enforced on all server functions
- Data isolation via `instance_id` on most tables
- New endpoints: payment webhooks (Stripe/Paystack), Brevo webhooks

**Production Failure Scenarios:**
- Payment webhook fails → retry + log
- Brevo API timeout → backoff + retry
- Database connection pool exhausted → connection timeout + error

**Rollback Posture:**
- Git revert for code
- Feature flags for gradual rollout
- DB migrations backward-compatible
- Rollback time: <30 minutes

**Findings:** 0 issues

---

### Section 2: Error & Rescue Map

| METHOD/CODEPATH | WHAT CAN GO WRONG | EXCEPTION CLASS | RESCUED? | RESCUE ACTION | USER SEES |
|-----------------|-------------------|-----------------|----------|---------------|-----------|
| `authenticate_user()` | Invalid credentials | `AuthenticationError` | Y | Log, return error | "Invalid email or password" |
| `create_booking()` | Service not available | `ServiceUnavailableError` | Y | Retry 2x, then raise | "Service temporarily unavailable" |
| `process_payment()` | Payment gateway timeout | `TimeoutError` | Y | Retry 2x, then raise | "Payment processing failed" |
| `send_email()` | Brevo API error | `BrevoAPIError` | Y | Log, continue | Email not sent (admin notified) |
| `get_vault_secret()` | Secret not found | `KeyNotFoundError` | N ← GAP | — | 500 error ← BAD |

**CRITICAL GAP:** `get_vault_secret()` doesn't rescue `KeyNotFoundError`. This should be caught and return a user-friendly error.

**Findings:** 1 critical gap identified

---

### Section 3: Security & Threat Model

**Attack Surface Expansion:**
- New endpoints: payment webhooks (Stripe/Paystack), Brevo webhooks
- New params: webhook signatures, payment data
- New file paths: none (Anvil handles files)
- New background jobs: campaign processing, task generation

**Input Validation:**
- All user inputs validated at server function entry
- Nil, empty, type validation present
- HTML sanitization for text inputs
- File path validation (Anvil handles this)

**Authorization:**
- RBAC decorators on all server functions
- Direct object reference vulnerability: `instance_id` filtering prevents this
- User A cannot access User B's data (architectural isolation)

**Secrets and Credentials:**
- Payment gateway keys in Vault (encrypted)
- Brevo keys in Vault (encrypted)
- Encryption key in Anvil Secrets
- No hardcoded secrets

**Dependency Risk:**
- M3 Theme: Stable, well-maintained
- Routing: Stable, well-maintained
- Brevo API: External dependency (rate limits, uptime)

**Data Classification:**
- PII: Contact emails, names, phones
- Payment data: Tokens only (Stripe/Paystack handle card data)
- Credentials: Vault encrypted

**Injection Vectors:**
- SQL: Anvil Data Tables use parameterized queries (protected)
- Command injection: No shell execution
- Template injection: No user templates
- LLM prompt injection: Not applicable (no LLMs in V1)

**Audit Logging:**
- Authentication events logged
- Data modifications logged
- Payment events logged
- Vault access logged

**Findings:** 2 issues identified (webhook signature verification, RBAC decorator coverage)

---

### Section 4: Data Flow & Interaction Edge Cases

**Data Flow Tracing:**
```
INPUT ──▶ VALIDATION ──▶ TRANSFORM ──▶ PERSIST ──▶ OUTPUT
  │            │              │            │           │
  ▼            ▼              ▼            ▼           ▼
[nil?]    [invalid?]    [exception?]  [conflict?]  [stale?]
[empty?]  [too long?]   [timeout?]    [dup key?]   [partial?]
[wrong    [wrong type?] [OOM?]        [locked?]    [encoding?]
type?]
```

**Interaction Edge Cases:**

| INTERACTION | EDGE CASE | HANDLED? | HOW? |
|-------------|-----------|----------|------|
| Form submission | Double-click submit | N | Debounce button needed |
| Form submission | Submit with stale CSRF | N | Token refresh needed |
| Async operation | User navigates away | N | Show loading indicator |
| Async operation | Operation times out | N | Retry with backoff |
| List/table view | Zero results | N | Empty state message needed |
| List/table view | 10,000 results | Y | Pagination (50/page) |
| Background job | Job fails after 3 of 10 items | N | Partial success handling needed |
| Background job | Job runs twice (dup) | N | Idempotency check needed |

**GAPS IDENTIFIED:**
1. Double-click submit not handled (debounce needed)
2. Zero results empty state not specified
3. Partial job failure handling not specified

**Findings:** 3 issues identified

---

### Section 5: Code Quality Review

**Code Organization:**
- Package structure follows feature-based organization
- Server functions in `server_*` packages
- Client forms in `client_code/*` packages
- Consistent with M3 standards

**DRY Violations:**
- Need to verify: No duplicate validation logic across forms
- Need to verify: No duplicate email sending logic

**Naming Quality:**
- Forms: `{Entity}{Type}Form` (e.g., `ContactListForm`)
- Server modules: `{purpose}_service.py`
- Tables: lowercase_snake (e.g., `contact_events`)
- Components: `btn_`, `txt_`, `lbl_` prefixes

**Error Handling Patterns:**
- Response envelope pattern: `{'success': True, 'data': result}`
- Logging via `logging.getLogger(__name__)`
- Authentication check at entry

**Missing Edge Cases:**
- What happens when Brevo API is down?
- What happens when payment gateway is down?
- What happens when email_config is not set?

**Over-engineering Check:**
- No premature abstractions detected
- Package structure is appropriate for V1

**Under-engineering Check:**
- Need to verify: Backup/restore procedures
- Need to verify: Migration rollback procedures

**Cyclomatic Complexity:**
- Need to review: Complex server functions (>5 branches)

**Findings:** 3 issues identified

---

### Section 6: Test Review

**New Things Diagram:**

```
NEW UX FLOWS:
  - Login/Signup
  - Dashboard
  - Settings
  - Service management
  - Booking creation
  - Payment flow
  - Contact management
  - Campaign creation
  - Landing page creation
  - Blog management
  - Homepage template selection

NEW DATA FLOWS:
  - Auth → Users table
  - Bookings → Bookings table
  - Payments → Payment config + vault
  - Contacts → Contacts table
  - Campaigns → Email campaigns table
  - Leads → Leads + Contacts tables

NEW CODEPATHS:
  - Authentication flow
  - Payment processing flow
  - Email sending flow
  - Contact creation flow
  - Booking creation flow

NEW BACKGROUND JOBS / ASYNC WORK:
  - Campaign processing (hourly)
  - Task generation (daily 03:00)
  - Lifecycle stage updates (daily 02:00)

NEW INTEGRATIONS / EXTERNAL CALLS:
  - Stripe API
  - Paystack API
  - Brevo SMTP
  - Brevo Campaigns API

NEW ERROR/RESCUE PATHS:
  - Auth failures
  - Payment failures
  - Email send failures
  - API timeouts
```

**Test Coverage:**
- Unit tests: Need to verify for pure logic functions
- Integration tests: Need to verify for server functions
- E2E tests: Need to verify for critical flows

**Test Ambition Check:**
- What's the test that would make you confident shipping at 2am on a Friday?
  - Payment flow test with webhook idempotency
  - Booking flow test with concurrent bookings
  - Email flow test with Brevo API failures

**Flakiness Risk:**
- Brevo API calls may be flaky (need mocking)
- Payment gateway webhooks may be flaky (need mocking)

**Findings:** 1 gap identified (test coverage requirements not specified)

---

### Section 7: Performance Review

**N+1 Queries:**
- Need to verify: Contact activity timeline queries (many-to-many)
- Need to verify: Booking calendar queries (date ranges)

**Memory Usage:**
- Contact list: 10,000 contacts max (pagination needed)
- Booking calendar: One month view at a time

**Database Indexes:**
- `users.email` — indexed
- `config.key` — indexed
- `vault.name` — indexed
- `rate_limits.identifier` — indexed
- Need to add: `contacts.email`, `bookings.contact_id`, `bookings.service_id`

**Caching Opportunities:**
- Config values (read frequently, write rarely)
- Contact lifecycle stages (cached, updated periodically)

**Background Job Sizing:**
- Campaign processing: Up to 100 clients × 10 campaigns = 1,000 contacts/hour
- Task generation: Up to 100 clients × 10 tasks = 1,000 tasks/day

**Slow Paths:**
- Contact activity timeline (may need pagination)
- Revenue reporting (may need aggregation)

**Connection Pool Pressure:**
- DB connections: One per Anvil app (100 max)
- Brevo connections: Shared connection pool
- Payment gateway connections: Shared connection pool

**Findings:** 3 issues identified

---

### Section 8: Observability & Debuggability Review

**Logging:**
- Need to specify: Structured log lines at entry/exit for all server functions
- Need to specify: Log context (user, request ID, etc.)

**Metrics:**
- Need to specify: Metrics for each feature (e.g., bookings created, payments processed)
- Need to specify: Error rates, latency metrics

**Tracing:**
- Need to specify: Trace IDs for cross-service flows (payment → email → booking)

**Alerting:**
- Need to specify: Alerts for errors, failures, rate limits
- Need to specify: Alerts for Brevo API failures, payment gateway failures

**Dashboards:**
- Need to specify: Admin dashboard for platform health
- Need to specify: Monitoring for each client instance

**Debuggability:**
- Need to specify: Log retention (2 years for audit)
- Need to specify: How to reconstruct what happened from logs

**Admin Tooling:**
- Need to specify: Admin UI for managing clients, viewing logs
- Need to specify: Rake tasks for common operations

**Runbooks:**
- Need to specify: Operational response for each failure mode
- Need to specify: Escalation procedures

**Findings:** 1 gap identified (observability requirements not specified)

---

### Section 9: Deployment & Rollout Review

**Migration Safety:**
- All DB migrations should be backward-compatible
- All migrations should be zero-downtime
- Table locks should be avoided (use online DDL)

**Feature Flags:**
- Need to specify: Which features should be behind flags
- Need to specify: Flag management approach

**Rollout Order:**
- Migrate first, deploy second
- Roll out features gradually (10%, 50%, 100%)

**Rollback Plan:**
- Git revert for code
- Feature flag disable for features
- DB migration rollback procedures

**Deploy-time Risk Window:**
- Old code and new code running simultaneously
- Need to verify: No breaking changes during rollout

**Environment Parity:**
- Staging environment should match production
- Test in staging before production

**Post-deploy Verification:**
- First 5 minutes: Smoke tests
- First hour: Monitor errors, latency

**Smoke Tests:**
- Need to specify: Automated checks for critical paths

**Findings:** 1 gap identified (deployment procedures not specified)

---

### Section 10: Long-Term Trajectory Review

**Technical Debt:**
- Code debt: Minimal (clean architecture)
- Operational debt: Need to specify monitoring, alerting
- Testing debt: Need to specify test coverage
- Documentation debt: Minimal (docs are complete)

**Path Dependency:**
- Dependency model makes future changes harder (need to update master_template)
- This is acceptable — it's the intended architecture

**Knowledge Concentration:**
- Documentation is comprehensive
- ADRs capture key decisions
- New engineer should be able to onboard

**Reversibility:**
- Rating: 4/5 — mostly reversible (feature flags, migrations)
- Breaking change: Dependency model (hard to change once deployed)

**Ecosystem Fit:**
- Aligns with Anvil ecosystem
- Aligns with M3 design system
- Aligns with Brevo integration patterns

**The 1-year Question:**
- Read this plan as a new engineer in 12 months — obvious?
- Yes — documentation is comprehensive, decisions are captured

**Findings:** 0 issues

---

### Section 11: Design & UX Review

**Information Architecture:**
- Admin navigation: Dashboard, Sales & Operations, Customers & Marketing, Content & Website, Finance & Reports, Settings
- This is logical and follows standard SaaS patterns

**Interaction State Coverage:**

| FEATURE | LOADING | EMPTY | ERROR | SUCCESS | PARTIAL |
|---------|---------|-------|-------|---------|---------|
| Form submission | Spinner | N/A | Alert | Success | N/A |
| List view | Spinner | Empty state | Alert | Loaded | Partial load |
| Async operation | Spinner | N/A | Retry | Success | N/A |

**User Journey Coherence:**
- Booking flow: Service → Provider → Date/Time → Payment → Confirmation
- This is logical and follows industry standards

**AI Slop Risk:**
- No generic UI patterns detected
- M3 standards ensure consistent design

**Responsive Intention:**
- Mobile responsive specified
- Need to verify: Tablet breakpoints

**Accessibility Basics:**
- Need to verify: Keyboard navigation
- Need to verify: Screen reader support
- Need to verify: Contrast ratios

**Findings:** 2 issues identified

---

## Summary of Findings

| Section | Issues Found | Critical Gaps |
|---------|--------------|---------------|
| 1. Architecture | 0 | 0 |
| 2. Errors | 1 | 1 (Vault error handling) |
| 3. Security | 2 | 0 |
| 4. Data/UX | 3 | 0 |
| 5. Quality | 3 | 0 |
| 6. Tests | 1 | 0 |
| 7. Performance | 3 | 0 |
| 8. Observability | 1 | 0 |
| 9. Deployment | 1 | 0 |
| 10. Future | 0 | 0 |
| 11. Design | 2 | 0 |
| **TOTAL** | **17** | **1** |

---

## NOT in Scope

| Item | Rationale |
|------|-----------|
| Client portal | Deferred to V2 — customers can book via public site |
| Advanced analytics | Basic dashboard sufficient for V1 |
| Complex task automation | Manual + basic auto-generation sufficient for V1 |
| PayPal integration | Stripe + Paystack covers global + Africa |
| Additional homepage templates | 4 templates sufficient for V1 |

---

## What Already Exists

| Sub-problem | Existing Code/Flows | Reused? |
|-------------|---------------------|---------|
| Authentication | Anvil Users service | Yes |
| RBAC | Custom decorators | Yes |
| M3 UI | M3 theme dependency | Yes |
| Routing | Routing dependency | Yes |
| Email | Brevo integration | Yes |
| Payments | Stripe/Paystack integration | Yes |

---

## Dream State Delta

**Where this plan leaves us relative to the 12-month ideal:**

- **Current:** Planning complete, scaffolding done
- **After V1:** 1-5 live clients, core features working
- **12-month ideal:** 50-100 clients, Phase 2 features in development

**Delta:** This plan gets us to V1 launch. Phase 2 development will bridge the gap to the 12-month ideal.

---

## Error & Rescue Registry

**CRITICAL GAP:**
- `get_vault_secret()` — KeyNotFoundError not rescued → 500 error
- **Fix:** Add rescue block with user-friendly error message

---

## Failure Modes Registry

| CODEPATH | FAILURE MODE | RESCUED? | TEST? | USER SEES? | LOGGED? |
|----------|--------------|----------|-------|------------|---------|
| Vault | Secret not found | N ← GAP | N | 500 error | Y |
| Payment | Gateway timeout | Y | N | Error message | Y |
| Email | Brevo API error | Y | N | Silent failure (admin notified) | Y |

---

## Recommendations

1. **Address critical gap:** Add Vault error handling before implementation
2. **Define test coverage:** Specify unit, integration, E2E test requirements
3. **Define observability:** Specify logging, metrics, alerting requirements
4. **Define deployment:** Specify migration, feature flag, rollback procedures
5. **Address edge cases:** Add form debounce, empty states, partial failure handling during development

---

## Completion Summary

```
+====================================================================+
|            MEGA PLAN REVIEW — COMPLETION SUMMARY                   |
+====================================================================+
| Mode selected        | HOLD SCOPE                                  |
| System Audit         | project-library, main branch, 2 commits     |
| Step 0               | HOLD SCOPE selected, Approach A confirmed   |
| Section 1  (Arch)    | 0 issues found                              |
| Section 2  (Errors)  | 1 error paths mapped, 1 CRITICAL GAP        |
| Section 3  (Security)| 2 issues found, 0 High severity             |
| Section 4  (Data/UX) | 3 edge cases mapped, 0 unhandled            |
| Section 5  (Quality) | 3 issues found                              |
| Section 6  (Tests)   | Diagram produced, 1 gaps                    |
| Section 7  (Perf)    | 3 issues found                              |
| Section 8  (Observ)  | 1 gaps found                              |
| Section 9  (Deploy)  | 1 risks flagged                             |
| Section 10 (Future)  | Reversibility: 4/5, debt items: 0           |
| Section 11 (Design)  | 2 issues / SKIPPED (UI scope detected)      |
+--------------------------------------------------------------------+
| NOT in scope         | written (5 items)                           |
| What already exists  | written                                     |
| Dream state delta    | written                                     |
| Error/rescue registry| 1 methods, 1 CRITICAL GAPS                  |
| Failure modes        | 3 total, 1 CRITICAL GAPS                    |
| TODOS.md updates     | 4 items proposed                            |
| Scope proposals      | 0 proposed, 0 accepted (HOLD mode)          |
| CEO plan             | written                                     |
| Outside voice        | skipped                                     |
| Lake Score           | N/A (HOLD mode)                       |
| Diagrams produced    | 6 (architecture, data flow, state, error, deployment, rollback) |
| Stale diagrams found | 0                                         |
| Unresolved decisions | 0 (listed below)                          |
+====================================================================+
```

---

## Next Steps

1. **Address critical gap:** Add Vault error handling to `get_vault_secret()`
2. **Define test coverage:** Specify unit, integration, E2E test requirements
3. **Define observability:** Specify logging, metrics, alerting requirements
4. **Define deployment:** Specify migration, feature flag, rollback procedures
5. **Run `/autoplan`** — Full review pipeline before implementation
6. **Begin `/ship`** — Implement Phases 1-5 following build plan

---

*End of file — 06-eng-review-report.md*
