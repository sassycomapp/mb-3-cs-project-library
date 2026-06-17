# Design Review — Mybizz CS V1

**Date:** 2026-06-16
**Mode:** Plan-mode audit — reviewing design documents, not a live site
**Inputs:** 15+ design documents, ADRs 14/15/27/37, DESIGN.md, build-plan.md, archived wireframes
**Status:** DONE_WITH_CONCERNS

---

## Section 0: MUI Findings Report

**The user requested a thorough search for all MUI references. Here are the findings.**

### What MUI Is in This Project

MUI (Material-UI) is used **exclusively as an aesthetic reference** — not as a component library, not as a framework, not as code. The MUI dashboard template (`https://mui.com/material-ui/getting-started/templates/dashboard/`) was selected as the visual aesthetic reference for the Mybizz CS design system.

### How MUI Is Applied

The MUI aesthetic is applied as a CSS overlay on top of Material 3 (M3). It modifies 9 specific visual properties:

| Property | MUI Value Adopted | M3 Default |
|----------|-------------------|------------|
| Surface distinction | Two-level (background vs surface-container-low) | Single surface |
| Card shadow | Soft dual-layer `hsla(220,30%,5%,0.07)` | M3 elevation system |
| Card border radius | 8px (tightened) | 12px |
| Typography heading weight | 600 | 400 |
| Dividers | outline-variant at 40% opacity | Solid |
| State hover opacity | 0.08 | M3 default |
| State pressed/focused opacity | 0.12 | M3 default |
| App bar | surface token, hairline bottom border | M3 default |
| Left nav | surface-container-low background | M3 default |

### What MUI Does NOT Affect

The following are explicitly preserved from M3 and NOT changed by the MUI overlay:
- Button radius: 100px (pill)
- Dialog radius: 28px
- Input radius: 4px
- Navigation item radius: 100px
- Data grid radius: 24px
- Font family: Roboto
- Spacing rhythm: 4px base grid
- M3 color role structure

### ADR Decision (ADR-037)

ADR-037 evaluated three approaches:
1. Strict M3 theme
2. M3 with custom CSS overrides
3. MUI design styles without React

**Decision:** Approach 2 adopted. MUI patterns studied for inspiration but not directly implemented. Anvil's component model differs from React — MUI's `sx` prop pattern does not map cleanly.

### Files Referencing MUI

| File | Reference Type |
|------|---------------|
| `docs/DESIGN.md` (lines 1127-1180) | Authoritative — MUI Aesthetic Implementation section |
| `adr/37-ui-customization-approach.md` | ADR decision — MUI evaluated and scoped |
| `docs/spec-material-3-theme.md` | References MUI overlay |
| `docs/ui-standards.md` | References MUI overlay |
| `Prompts/6 step run.md` | GStack workflow search terms |
| `workspace/gstack/prompt-design-doc-cleanup.md` | Cleanup alignment reference |
| `Obsolete/DESIGN_md_additions.md` | Obsolete copy of MUI section |

### Verdict

MUI is correctly scoped as an aesthetic-only overlay. No MUI React components, no `sx` props, no MUI imports exist or are planned. The 9 aesthetic variables are documented in DESIGN.md and applied via CSS custom properties. **No action needed.**

---

## Section 1: Empty States

### Current State

Empty states have **minimal coverage** across the design documents:

| Document | Empty State Coverage |
|----------|---------------------|
| `design-direction.md` | List Page: "Label with helpful message" — one line, no pattern |
| `build-plan.md` | Stages 1.3, 1.4, 2.3, 3.1 have empty state completion criteria (added by CEO remediation) |
| `DESIGN.md` | No empty state specifications |
| `custom-component-requirements-matrix.md` | No empty state specs for any of the 5 custom components |
| `m3_component_mapping.md` | No empty state mapping |

### Gaps

| Screen | Empty State Needed | Status |
|--------|-------------------|--------|
| Dashboard (no metrics yet) | Welcome message + onboarding link | Specified in build-plan.md Stage 1.3 |
| Contact list (no contacts) | "No contacts yet" + "Add Contact" CTA | Specified in build-plan.md Stage 3.1 |
| Booking calendar (no bookings) | "No bookings yet" + "Create Booking" CTA | Specified in build-plan.md Stage 2.3 |
| Invoice list (no invoices) | Empty state pattern | **NOT SPECIFIED** |
| Task list (no tasks) | Empty state pattern | **NOT SPECIFIED** |
| Campaign list (no campaigns) | Empty state pattern | **NOT SPECIFIED** |
| Blog list (no posts) | Empty state pattern | **NOT SPECIFIED** |
| Landing page list (no pages) | Empty state pattern | **NOT SPECIFIED** |
| Service list (no services) | Empty state pattern | **NOT SPECIFIED** |
| Segment list (no segments) | Empty state pattern | **NOT SPECIFIED** |
| Activity feed (no activity) | Empty state pattern | **NOT SPECIFIED** |
| Vault (no secrets) | Empty state pattern | **NOT SPECIFIED** |
| Analytics dashboard (no data) | Empty state pattern | **NOT SPECIFIED** |

### Recommendation

The build-plan.md additions from the CEO remediation cover 4 key screens. The remaining 9 data-driven screens need empty state completion criteria added to their respective build plan stages. Each empty state should include:
1. A warm message (not just "No items found")
2. A primary action button (the most likely next step)
3. Optional: a brief explanation of what will appear here

---

## Section 2: Error States and Validation Feedback

### Current State

Error states have **partial coverage**:

| Document | Error State Coverage |
|----------|---------------------|
| `design-direction.md` | Validation errors: 2px red border, red label, 12px message below, red icon, disabled submit |
| `DESIGN.md` | Input states: Error state defined (border: Error, bg: Surface Variant, text: Error) |
| `build-plan.md` | Stages 1.3, 2.3, 3.1 have error state completion criteria |
| `test-specifications.md` | 7 error path scenarios documented |

### Gaps

| Error Type | Status |
|-----------|--------|
| Form validation (inline) | Defined in design-direction.md |
| Server call failure (retry pattern) | Specified in build-plan.md Stages 1.3, 2.3, 3.1 |
| Payment failure | Specified in test-specifications.md |
| Webhook failure | Specified in ADR-038 + test-specifications.md |
| Network timeout | Specified in build-plan.md Stage 2.3 (30s timeout) |
| Application-level error pages (404, 500, 403) | **NOT SPECIFIED** in current docs — found only in archived Obsolete wireframes |
| Booking-specific errors (slot taken, provider unavailable) | **NOT SPECIFIED** in design docs |
| Vault access errors (TOTP failure, key not found) | **NOT SPECIFIED** in design docs |

### Orphaned Finding: ErrorLayout

The archived wireframes contain an `ErrorLayout` specification:
- Full-viewport centred layout, no chrome (no sidebar, no app bar)
- Error code in Display typography (57px), heading in Headline Large (32px), message in Body Large (16px)
- Max-width container: 480px
- Single "Go Home" filled button, navigates based on auth state
- Supports 404, 500, 403 error types

**This decision exists ONLY in the archived wireframes and is NOT in any current authoritative document.** It should be added to DESIGN.md or design-direction.md.

### Recommendation

1. Add ErrorLayout specification to DESIGN.md (from archived wireframes)
2. Add booking-specific error states to build-plan.md Stage 2.3
3. Add Vault error states to build-plan.md Stage 1.5

---

## Section 3: Loading States

### Current State

Loading states have **the best coverage** of the three state types, but still incomplete:

| Document | Loading State Coverage |
|----------|----------------------|
| `design-direction.md` | Button spinner, table skeleton (3-5 rows), card skeleton (pulse), form disable+spinner |
| `build-plan.md` | Stages 1.3, 1.4, 2.3, 3.1 have loading state completion criteria |
| `m3_component_mapping.md` | LinearProgressIndicator, CircularProgressIndicator mapped |
| `DESIGN.md` | No loading state specifications |

### Gaps

| Loading Pattern | Status |
|----------------|--------|
| Button spinner | Defined in design-direction.md |
| Table skeleton rows | Defined in design-direction.md |
| Card skeleton | Defined in design-direction.md |
| Form disable + spinner | Defined in design-direction.md |
| Dashboard metric cards | Specified in build-plan.md Stage 1.3 |
| Booking calendar skeleton | Specified in build-plan.md Stage 2.3 |
| Contact list skeleton | Specified in build-plan.md Stage 3.1 |
| Server call progress indicator | **NOT SPECIFIED** — ADR-034 notes 100-500ms latency per call |
| Booking flow step transitions | **NOT SPECIFIED** — 3 round-trips, each needs loading feedback |
| Page transition loading | **NOT SPECIFIED** — content panel swaps need loading indicator |

### Recommendation

1. Add loading state specs to DESIGN.md (from design-direction.md)
2. Add booking flow step transition loading to build-plan.md Stage 2.3
3. Add content panel swap loading indicator to ADR-035 or build-plan.md Stage 1.3

---

## Section 4: Booking Flow — 7 Steps with Full State Coverage

### Current State

The booking flow is the most complex user flow in the application. It has 7 steps:

1. Select service
2. Select provider
3. Select date/time
4. Select meeting type
5. Fill intake form
6. Process payment
7. Confirmation

### What's Defined

| Aspect | Status | Source |
|--------|--------|--------|
| 3-round-trip budget | Defined | build-plan.md Stage 2.3 |
| Server functions per round-trip | Defined | build-plan.md Stage 2.3 |
| globals.py booking_flow_state | Defined | docs/globals-contract.md |
| Performance gate (3 seconds) | Defined | build-plan.md Stage 2.3 |
| Mid-payment navigation (15-min hold) | Defined | build-plan.md Stage 2.3 |
| Slow connection timeout (30s) | Defined | build-plan.md Stage 2.3 |
| Empty state | Defined | build-plan.md Stage 2.3 |
| Loading state | Defined | build-plan.md Stage 2.3 |
| Error state | Defined | build-plan.md Stage 2.3 |

### What's NOT Defined

| Aspect | Status |
|--------|--------|
| Per-step UI layout (what the user sees at each step) | **NOT SPECIFIED** — no wireframe exists for the booking flow |
| Step transition animation/feedback | **NOT SPECIFIED** |
| Back navigation between steps | **NOT SPECIFIED** — globals.booking_flow_state preserves state, but what happens visually? |
| Provider unavailable state | **NOT SPECIFIED** |
| Slot taken (double-booking) error state | **NOT SPECIFIED** |
| Payment processing state (between confirm and result) | **NOT SPECIFIED** |
| Booking confirmation screen layout | **NOT SPECIFIED** |
| Calendar view architecture (month/week/day) | **Found only in archived wireframes** — 4 template subforms with color-coded events |
| Booking status lifecycle (Confirmed/Pending/Cancelled) | **Found only in archived wireframes** — badge colors, calendar color coding |
| Booking list columns and row actions | **Found only in archived wireframes** — Booking #, Customer, Service, Date, Time, Status, Actions |

### Orphaned Finding: Booking Calendar Architecture

The archived wireframes contain a complete booking calendar specification:
- 4 template subforms: `tmpl_day_event`, `tmpl_month_day_cell`, `tmpl_week_event`, `tmpl_week_timeslot`
- Event color coding: Confirmed = green (`#388e3c`), Pending = yellow (`#f9a825`), Cancelled = red (`#c62828`)
- View toggle: SegmentedButton (Month/Week/Day)
- Empty timeslot: thin spacer line (`height: 1px`, `background: #E7E0EC`)
- Week timeslot: 60px time label + 1fr content

**This exists ONLY in the archived wireframes.** It should be promoted to DESIGN.md or a booking-specific design spec.

### Recommendation

1. Create a booking flow design spec document covering all 7 steps with per-step layouts
2. Promote the booking calendar template architecture from archived wireframes to DESIGN.md
3. Define booking status badge colors and calendar event color coding in DESIGN.md
4. Specify booking list columns and row actions in DESIGN.md

---

## Section 5: Onboarding Form States

### Current State

The onboarding form is well-specified in the onboarding implementation plan:

| Aspect | Status | Source |
|--------|--------|--------|
| 6-step flow | Defined | onboarding-implementation-plan.md |
| Resumable (ADR-023) | Defined | ADR-023 |
| Revisitable (ADR-026) | Defined | ADR-026 |
| System Currency immutability | Defined | ADR-016 |
| Legal Acknowledgement | Defined | ADR-018 |
| Edge case: browser close mid-onboarding | Defined | build-plan.md Stage 1.4 |

### Orphaned Finding: Two Legal Checkboxes

The archived wireframes specify TWO separate legal acknowledgement checkboxes:
1. `cb_legal_ack` — "I understand I must configure my own Privacy Policy and Terms & Conditions..."
2. `cb_data_governance_ack` — "I understand that Mybizz retains certain records for legal and compliance purposes..."

The "Confirm Acknowledgement" button is enabled only when BOTH are ticked.

**Current docs describe only ONE checkbox.** The `data_governance_ack_accepted` field is in the authoritative schema but the UI requirement for a separate checkbox is only in the archived wireframes.

### Orphaned Finding: Step Enable/Disable Logic

The archived wireframes specify per-step Continue button logic:
- Steps 2-3 (Business Profile, Palette): Continue always enabled (optional skip)
- Step 4 (System Currency): Continue enabled only after currency confirmed
- Step 5 (Legal): Continue enabled only after acknowledgement confirmed
- Step 6 (Completion): "Go to Dashboard" sets `onboarding_status = "completed"`

### Recommendation

1. Update DESIGN.md to specify two legal acknowledgement checkboxes
2. Document step enable/disable logic in the onboarding implementation plan or DESIGN.md

---

## Section 6: Vault Form

### Current State

| Aspect | Status | Source |
|--------|--------|--------|
| TOTP step-up on every open | Defined | ADR-003, build-plan.md Stage 1.5 |
| No grace period | Defined | ADR-003 |
| Email notification on every access | Defined | build-plan.md Stage 1.5 |
| Secret masking in get_payment_config() | Defined | build-plan.md Stage 1.5 |
| TOTP recovery path | Defined | ADR-034 Constraint 11 |

### What's NOT Defined

| Aspect | Status |
|--------|--------|
| Vault form UI layout | **NOT SPECIFIED** — no wireframe exists |
| TOTP entry screen layout | **NOT SPECIFIED** |
| Secret entry form layout | **NOT SPECIFIED** |
| Secret display (masked vs revealed) | **NOT SPECIFIED** |
| Vault empty state (no secrets stored) | **NOT SPECIFIED** |
| Vault error state (TOTP failure) | **NOT SPECIFIED** |
| Vault loading state (encryption/decryption) | **NOT SPECIFIED** |

### Recommendation

1. Create a Vault form wireframe covering: TOTP entry, secret list, secret entry, empty state, error state

---

## Section 7: Cross-Document Gaps

| Gap | Files Affected | Severity |
|-----|---------------|----------|
| DESIGN.md Component and Role Map missing Alerts & Feedback and Data Display sections | DESIGN.md vs design-direction.md | Moderate |
| Hardcoded hex values in design-direction.md contradict token-only policy | design-direction.md, DESIGN.md | Moderate |
| ADR-014 lists Switch/RadioGroup/CheckBoxGroup as blocked but they're Fully supported in native M3 | ADR-014 vs m3_component_mapping.md | Low |
| 5 custom components lack interaction specs, wireframes, and state definitions | custom-component-requirements-matrix.md | Moderate |
| Partially supported components (FAB, Snackbar, Dialog, Chips, List, App Bar) lack concrete composition patterns | m3_component_mapping.md | Moderate |
| Time Picker is Not Supported with no workaround | m3_component_mapping.md | Low |
| Loading state specs only in design-direction.md, marked as Phase 2 work | design-direction.md | Moderate |
| AdminLayout sidebar nav groups + conditional visibility not in current docs | DESIGN.md, design-direction.md | High |
| Booking calendar template architecture (4 subforms) not in current docs | DESIGN.md, design-direction.md | High |
| Booking status lifecycle + status-based actions not in current docs | DESIGN.md, design-direction.md | High |

---

## Section 8: Priority Action List

### P1 — Must Fix Before Wireframe Production

1. **Add ErrorLayout to DESIGN.md** — promote from archived wireframes
2. **Add AdminLayout sidebar navigation groups and conditional visibility to DESIGN.md** — promote from archived wireframes
3. **Add booking calendar template architecture to DESIGN.md** — promote from archived wireframes (4 subforms, color coding, view toggle)
4. **Add booking status lifecycle to DESIGN.md** — promote from archived wireframes (Confirmed/Pending/Cancelled, badge colors, calendar colors)
5. **Add booking list columns and row actions to DESIGN.md** — promote from archived wireframes

### P2 — Should Fix Before Wireframe Production

6. **Add empty state patterns to DESIGN.md** — for the 9 remaining data-driven screens
7. **Add Vault form UI specification to DESIGN.md** — TOTP entry, secret list, secret entry
8. **Add two legal acknowledgement checkboxes to onboarding spec** — promote from archived wireframes
9. **Add per-step enable/disable logic to onboarding spec** — promote from archived wireframes
10. **Add booking flow per-step UI layout specification** — create new or promote from archived wireframes

### P3 — Fix During Wireframe Production

11. **Add loading state specs to DESIGN.md** — from design-direction.md
12. **Add composition patterns for partially supported components** — FAB, Snackbar, Dialog, Chips
13. **Resolve hardcoded hex values in design-direction.md** — align with token-only policy
14. **Add MetricCard/ActivityFeed programmatic property specs** — promote from archived wireframes
15. **Add ClauseBuilder interaction pattern (list-hide/form-replace)** — promote from archived wireframes

---

## Section 9: Readiness Verdict

| Concern | Status | Blocking? |
|---------|--------|-----------|
| MUI scope | Correctly scoped as aesthetic overlay | No |
| Empty states | 4 of 13 screens specified | Blocks wireframe production for remaining 9 |
| Error states | Partial — input validation defined, app-level error pages missing | Blocks wireframe production |
| Loading states | Partial — patterns defined in design-direction.md, not in DESIGN.md | Blocks wireframe production |
| Booking flow | Round-trip budget defined, per-step UI not specified | Blocks wireframe production |
| Onboarding flow | Well-specified, 2 minor gaps (dual checkboxes, step logic) | Minor blocker |
| Vault form | No UI specification at all | Blocks wireframe production |
| Orphaned decisions | 3 High-severity findings in archived wireframes | Must promote before wireframe production |
| Cross-document gaps | 10 gaps identified | Should resolve before wireframe production |

**The design documentation is comprehensive for the design system (tokens, palettes, components, roles) but has significant gaps in screen-level specifications (empty states, error states, loading states, booking flow, Vault form).** The archived wireframes contain orphaned decisions that should be promoted to authoritative documents before wireframe production begins.

---

*End of Design Review — 2026-06-16*
