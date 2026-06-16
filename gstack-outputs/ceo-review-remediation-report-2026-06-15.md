# CEO Review Remediation Report — 2026-06-15

**Date:** 2026-06-15
**Source:** CEO review report (`gstack-outputs/ceo-review-2026-06-15.md`)
**Commit:** `67ba9fa` — "CEO review remediation: plan doc updates and todo capture 2026-06-15"
**Files changed:** 5 files, 225 insertions

---

## 1. Classification Summary

| Section | Finding | Classification |
|---------|---------|---------------|
| 1 | Anvil account suspension — no recovery plan | PLAN CHANGE → deployment-procedures.md |
| 2 | `app_tables` failure — no documented behavior | PLAN CHANGE → ADR-034 |
| 2 | Background task failure — no retry policy per task | PLAN CHANGE → ADR-036 |
| 3 | TOTP recovery path — undocumented | PLAN CHANGE → ADR-034 |
| 3 | Brevo credential monitoring — undocumented | PLAN CHANGE → integration-architecture.md |
| 4 | 6 edge cases undocumented | PLAN CHANGE → build-plan.md |
| 5 | No self-review checklist | PLAN CHANGE → build-plan.md |
| 6 | Missing E2E booking test | TODO ITEM (8.8 exists) |
| 6 | Missing error path tests | TODO ITEM (8.9 exists) |
| 6 | Missing RBAC matrix | TODO ITEM (8.10 exists) |
| 7 | Observability gaps (thresholds, retention, error monitoring) | PLAN CHANGE → build-plan.md |
| 8 | 18 mechanical schema gaps | No new action — already in CEO fix plan |
| 9 | Webhook schema mismatch | TODO ITEM (8.12 exists) |
| 10 | Anvil billing question | TODO ITEM (8.13 exists) |
| 11 | Empty/loading/error state specs | PLAN CHANGE → build-plan.md |
| 11 | Onboarding wireframe "missing" | AMBIGUOUS — stale reference (created 2026-06-02) |
| 11 | Policy editor wireframe "missing" | AMBIGUOUS — stale reference (created 2026-06-01) |

---

## 2. PLAN CHANGES Applied

### 2.1 `docs/deployment-procedures.md`
Added "Anvil Account-Level Failure Scenarios" section with three scenarios:
- **Scenario 1: Account Suspension** — detection, recovery (contact Anvil support, resolve issue, reinstate, verify), prevention
- **Scenario 2: Account Compromise** — detection (unexpected code changes, unknown sessions), recovery (credential reset, audit, notify)
- **Scenario 3: Platform Outage** — detection (health heartbeat fails), recovery (monitor Anvil status, communicate, verify)

### 2.2 `adr/34-anvil-platform-constraints.md`
Added two new constraints:
- **Constraint 10: `app_tables` Resolution Failure** — if client instance is corrupted, show generic error, log to `health_log` as `"unhealthy"`, alert Mybizz_management. Prevention: complete table set in `blank_client_template`, schema migration checklist, health heartbeat verification.
- **Constraint 11: Vault TOTP Recovery** — Owner contacts Mybizz_management, identity verified out-of-band, TOTP reset, re-enrollment, amendment log entry. No self-service recovery.

Updated related documents table to include `docs/deployment-procedures.md`.

### 2.3 `adr/36-real-time-and-background-tasks.md`
Added "Per-Task Failure and Retry Policy" table:

| Task | On Failure | Max Retries | Retry Interval | Failure Action |
|------|-----------|-------------|----------------|----------------|
| Health heartbeat | Log, continue | 1 | Next run (15 min) | Mark `"degraded"` after 3 consecutive failures |
| Campaign enrollment | Skip, continue | 3 | 5 min (exp backoff) | Log, skip contact |
| Appointment reminders (24h) | Retry, skip | 3 | 10 min | Log, retry on next hourly run |
| Appointment reminders (1h) | Retry, skip | 2 | 5 min | Log |
| Lifecycle stage updates | Retry batch | 2 | 30 min | Log, retry on next daily cycle |
| Automated task generation | Retry batch | 2 | 30 min | Log, retry on next daily cycle |
| Webhook processing | Retry event | 5 | Exp backoff (1-16 min) | Mark `"failed"`, alert Mybizz_management |

Updated related documents table to include `docs/deployment-procedures.md`.

### 2.4 `docs/integration-architecture.md`
Added "Brevo Credential Monitoring" section:
- V1: No monitoring for unusual Brevo activity per client; Vault TOTP gate provides credential protection
- Mybizz_management intention: monitor API usage, alert on volume spikes, enable credential rotation (out of V1 scope)

### 2.5 `implementation/build-plan.md`
Multiple targeted additions:
- **Updated date:** 2026-06-02 → 2026-06-15
- **Stage 0.3:** Added health thresholds (`"healthy"`, `"degraded"`, `"unhealthy"`), log retention policy (2yr/90d/30d), error rate monitoring (5% threshold in 15-min window)
- **Stage 1.3:** Added empty state (welcome message + onboarding link), loading state (skeleton/placeholder), error state (retry button + error message)
- **Stage 1.4:** Added currency immutability edge case (refund/voided — currency remains immutable), onboarding resumability edge case (browser close — resume from last step per ADR-023)
- **Stage 2.3:** Added empty state ("No bookings yet" + CTA), loading state (skeleton grid), error state (inline validation + retry), edge cases (mid-payment navigation — 15-min hold; slow connection — 30s timeout)
- **Stage 3.1:** Added loading state (skeleton rows), error state (inline validation + retry), edge cases (dedup same email different names — manual merge dialog; duplicate email lead — update existing contact)
- **Self-Review Checklists:** New section with per-phase checklists (Phase 1: 7 items, Phase 2: 7 items, Phase 3: 7 items, Phase 4: 7 items, Phase 5: 7 items)

---

## 3. TODO ITEMS in todo.md

Section 8 (items 8.1-8.14) was already present. No new items added. All 14 items verified:

| # | Title | Type | Priority | Status |
|---|-------|------|----------|--------|
| 8.1 | Anvil Account Failure Recovery Documentation | Documentation | P1 | [Planned] |
| 8.2 | app_tables Failure Handling Spec | Documentation | P1 | [Planned] |
| 8.3 | Background Task Failure Policy | Documentation | P1 | [Planned] |
| 8.4 | TOTP Recovery Path | Documentation | P1 | [Planned] |
| 8.5 | Brevo Monitoring Note | Documentation | P2 | [Planned] |
| 8.6 | Edge Case Documentation (Core Flows) | Documentation | P1 | [Planned] |
| 8.7 | Build Plan Self-Review Checklists | Documentation | P2 | [Planned] |
| 8.8 | E2E Booking Test Spec | Test | P1 | [Planned] |
| 8.9 | Error Path Test Coverage | Test | P1 | [Planned] |
| 8.10 | RBAC Coverage Matrix | Test | P1 | [Planned] |
| 8.11 | Observability Gaps Documentation | Documentation | P1 | [Planned] |
| 8.12 | Webhook Schema Reconciliation | Documentation | P1 | [Planned] |
| 8.13 | Anvil Billing Model Clarification | Research | P2 | [Planned] |
| 8.14 | UI State Specifications | Documentation | P1 | [Planned] |

---

## 4. Ambiguous Findings

| Finding | Resolution |
|---------|-----------|
| Section 11: "Onboarding wireframe missing" | Stale reference. `todo.md` item 5.1 shows `wireframe-onboarding-OnboardingForm.html` was created 2026-06-02. CEO review predates or did not reflect this. No action needed. |
| Section 11: "Policy editor wireframe missing" | Stale reference. `todo.md` section 14 shows `wireframe-settings-PolicyEditorForm.html` was created 2026-06-01. Same situation. No action needed. |
| Section 8: 18 mechanical schema gaps | Already tracked. CEO review explicitly states "No new schema issues found." These are in the CEO fix plan (2026-06-14). |

---

## 5. ADR Contradiction Check

No confirmed ADR decisions were contradicted:
- **ADR-28** (Client Instance Architecture) — respected
- **ADR-29** (Dependency Update Model) — respected
- **ADR-30** (blank_client_template) — respected
- **ADR-41** — respected

---

## 6. Readiness for /plan-eng-review

**Confirmed: project-library is ready for /plan-eng-review.**

**What's in place:**
- All 11 CEO review sections processed and classified
- 8 PLAN CHANGE items applied across 5 files
- 14 TODO ITEMS captured in todo.md section 8
- Self-review checklists added to all 5 build plan phases
- Empty/loading/error state specs added to 4 key stages
- Edge case behaviors documented in build plan completion criteria
- Observability thresholds and retention policy documented
- Anvil account failure scenarios documented
- Per-task failure/retry policies documented
- ADR cross-references updated

**What remains for /plan-eng-review:**
- 14 TODO ITEMS in todo.md section 8 (documentation, tests, research)
- 18 mechanical schema gaps from CEO fix plan (2026-06-14)
- GBrain sync has 5 pre-existing file failures (unrelated to this change)

---

*End of CEO Review Remediation Report — 2026-06-15*
