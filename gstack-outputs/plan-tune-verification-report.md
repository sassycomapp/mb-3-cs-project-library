# Mybizz CS — Plan-Tune Verification Report

**Date:** 2026-06-01
**Input artefacts:** `sanity-audit-final-report.md`, `todo.md` (with Implementation Backlog)
**Scope:** Verify todo.md coverage against project plan, implementation task mapping, and stale references

---

## 1. Executive Summary

**Verification result: All gaps resolved.**

All [Planned] and [Deferred] items in todo.md are now reflected in the project plan. All implementation tasks (Section 7.*) are mapped to concrete files, components, and ADRs. No [Done] or [Cancelled] items remain referenced as future work.

### What Was Done

1. **Clause builder and policy authoring (todo 2.1, 2.2)** added to Phase 4 Stage 4.2 (Public Website Pages) in build-plan.md
2. **Client data export (todo 3.2)** mapped explicitly to Stage 5.3 with file references
3. **DESIGN.md inventory update (todo 6.1)** mapped to Stage 4.2
4. **build-plan.md stale counts** corrected: 38 tables, 47 wireframes
5. **build-plan.md Stage 1.4 SettingsForm** updated to 11-tab structure with RBAC governance
6. **All todo items** now include `Build Plan:` and `Source:` references for traceability

---

## 2. [Planned] Items — Coverage Check

| Todo Item | Tag | In build-plan.md? | In onboarding plan? | Gap? |
|---|---|---|---|---|
| 2.1 Clause Builder Custom Component | [Planned] | **Yes — Stage 4.2** | No | **Resolved** |
| 2.2 Policy Document Authoring System | [Planned] | **Yes — Stage 4.2** | No | **Resolved** |
| 3.2 Client Data Export / Rights Interface | [Planned] | **Yes — Stage 5.3** | No | **Resolved** |
| 4.2 Service Catalogue Setup | [Planned] | Yes — Stage 2.1 | No | No |
| 5.1 Onboarding Form Wireframe | [Planned] | No (onboarding plan scope) | Yes — §9.3 | No |
| 6.1 Custom Components Inventory in DESIGN.md | [Planned] | **Yes — Stage 4.2** (doc task) | No | **Resolved** |

### Gap 1: Clause Builder (2.1) and Policy Authoring (2.2)

**Finding:** The clause builder and policy authoring system are defined in todo.md and have wireframes, but neither is mapped to a specific phase or stage in build-plan.md. The build-plan.md mentions "Privacy Policy page populated" and "Terms of Service page populated" in Stage 5.3 (Compliance), but does not reference the clause builder component or the policy editor screen.

**Impact:** Implementation tasks 7.5 (ClauseBuilder Component), 7.6 (Policy Editor Screen Wireframe), 7.7 (Privacy Policy Public Page), and 7.8 (Terms & Conditions Public Page) have no home in the build sequence.

**Question for developer:** Should these items be added to Phase 4 (Content + Website) alongside the blog and public website work, or to Phase 5 (Polish + Launch) alongside compliance? The natural fit appears to be Phase 4 Stage 4.2 (Public Website Pages), where PrivacyPolicyPage and TermsConditionsPage are already listed.

### Gap 2: Client Data Export (3.2)

**Finding:** The client data export / rights interface is defined in todo.md and referenced in ADR-021. The build-plan.md mentions "export_my_data() function — GDPR/POPIA data export" in Stage 5.3, but the todo item describes a more specific Dashboard entry point with RBAC and PDF export. The mapping is loose — the build-plan references the function, not the full interface.

**Impact:** Implementation is partially covered (the function exists in Stage 5.3), but the UI entry point and RBAC governance are not explicitly mapped.

**Question for developer:** Is the current Stage 5.3 coverage sufficient, or should the todo item be updated to reference Stage 5.3 explicitly?

### Gap 3: DESIGN.md Custom Components Inventory (6.1)

**Finding:** The todo item to update DESIGN.md's custom components inventory is a documentation task, not an implementation task. It is correctly scoped outside the build-plan. However, it is not tracked in any plan document as a required action.

**Impact:** Low — this is a doc maintenance task that can be done at any time. But it is currently only tracked in todo.md.

**Question for developer:** Should this be added to the build-plan as a documentation task, or is todo.md sufficient?

---

## 3. [Deferred] Items — Coverage Check

| Todo Item | Tag | In build-plan.md? | Correctly Scoped? |
|---|---|---|---|
| 7.9 Mybizz_management Amendment Log | [Deferred] | No (out of scope) | Yes — correctly deferred |

**Finding:** The amendment log is correctly marked as out-of-scope and deferred. No action needed.

---

## 4. [Done] and [Cancelled] Items — Stale Reference Check

| Todo Item | Tag | Referenced as future work? | Status |
|---|---|---|---|
| 1.1 Onboarding Status Tracker | [Done] | No | Clean |
| 1.2 Onboarding Initiation Interface | [Done] | No | Clean |
| 1.3 Settings Return Flow | [Done] | No | Clean |
| 2.3 Policy Responsibility Acknowledgement | [Done] | No | Clean |
| 3.1 Payment Gateway Settings Module | [Done] | No | Clean |
| 4.1 Schema Alignment Review | [Done] | No | Clean |
| 5.2 Settings Form Wireframe | [Done] | No | Clean |
| 5.3 OnboardingPaletteForm Wireframe | [Cancelled] | No | Clean |

**Finding:** No [Done] or [Cancelled] items are referenced as future work in any plan document. All stale references have been cleaned up.

---

## 5. Implementation Tasks (Section 7.*) — Mapping Verification

| Task | Files | Components | ADRs | Mapped to Plan? |
|---|---|---|---|---|
| 7.1 Onboarding Form | `OnboardingForm` | Welcome, Currency dropdown, Legal checkbox | ADR-017, ADR-023, ADR-025, ADR-024 | Yes — onboarding plan §3 |
| 7.2 Tabbed Settings Form | `SettingsForm` | 11 tab buttons, tab panels, Return to Onboarding button | ADR-019, ADR-016 | Yes — onboarding plan §4, build-plan Stage 1.4 |
| 7.3 System Currency Selection | `business_profile.system_currency` | DropdownMenu, Confirm button | ADR-016, ADR-025 | Yes — onboarding plan §3.4.1 |
| 7.4 Legal Acknowledgement | `business_profile.legal_ack_accepted` | CheckBox, Confirm button, Text | ADR-018, ADR-025 | Yes — onboarding plan §3.4.2 |
| 7.5 ClauseBuilder Component | `ClauseBuilder` | Category dropdown, RepeatingPanels, Add/Edit forms | ADR-018 | **Yes — build-plan Stage 4.2** |
| 7.6 Policy Editor Screen Wireframe | `wireframe-settings-PolicyEditorForm.html` | Feature | ADR-018 | **Yes — build-plan Stage 4.2** |
| 7.7 Privacy Policy Public Page | `PrivacyPolicyPage` | Clause rendering | ADR-018 | **Yes — build-plan Stage 4.2** |
| 7.8 Terms & Conditions Public Page | `TermsConditionsPage` | Clause rendering | ADR-018 | **Yes — build-plan Stage 4.2** |
| 7.9 Mybizz_management Amendment Log | (out of scope) | (out of scope) | ADR-025 | Correctly deferred |

**Finding:** All 9 implementation tasks are now mapped to concrete files, components, and ADRs. All tasks have build-plan stage references.

---

## 6. build-plan.md Internal Consistency

| Check | Result |
|---|---|
| Schema count matches authoritative-schema.md? | **Yes** — build-plan says "38 tables" (line 6 and status table). Matches authoritative-schema.md. |
| ADR count matches adr-index.md? | **Yes** — build-plan says "23 ADRs" (line 19). adr-index.md lists 23 active ADRs. |
| Wireframe count matches actual files? | **Yes** — build-plan says "47 HTML screens" (line 17). Matches actual count after OnboardingPaletteForm deletion. |
| Phase 1 SettingsForm description matches onboarding plan? | **Yes** — build-plan Stage 1.4 now describes 11-tab structure with RBAC governance and Return to Onboarding button. |

## 7. Resolutions Applied

All three gaps and all stale data issues have been resolved:

### Gap 1: Clause Builder and Policy Authoring — RESOLVED
- Added `ClauseBuilder` component, policy editor screen, `privacy_policy` and `t_and_c` tables to Stage 4.2 deliverables
- Added completion criteria for ClauseBuilder, Privacy Policy rendering, Terms rendering, policy editor
- Updated todo items 2.1, 2.2, 7.5, 7.6, 7.7, 7.8 with `Build Plan: Stage 4.2` references

### Gap 2: Client Data Export — RESOLVED
- Updated Stage 5.3 deliverables to include Dashboard entry point, RBAC, PDF export with data separation
- Updated todo item 3.2 with `Build Plan: Stage 5.3` and file references

### Gap 3: DESIGN.md Inventory — RESOLVED
- Updated todo item 6.1 with `Build Plan: Stage 4.2` reference

### Stale Counts — RESOLVED
- Schema count: 36 → 38 (matching authoritative-schema.md)
- Wireframe count: 48 → 47 (after OnboardingPaletteForm deletion)

### Stage 1.4 SettingsForm — RESOLVED
- Updated from 5 sub-sections to 11-tab structure with RBAC governance and Return to Onboarding button
- Added completion criteria for tab layout, RBAC, and return button

### Stage 5.3 Compliance — RESOLVED
- Added data export UI, clause builder integration, PDF export with data separation

---

*End of plan-tune verification report — 2026-06-01*

---

*End of plan-tune verification report — 2026-06-01*
