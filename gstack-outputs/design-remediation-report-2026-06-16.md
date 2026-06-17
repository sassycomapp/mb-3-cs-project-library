# Design Review Remediation Report — 2026-06-16

**Source:** plan-design-review-2026-06-16.md (P1 + P2 findings)
**Commit:** (pending)
**File changed:** `docs/DESIGN.md`

---

## Changes Applied

### 1. ErrorLayout Section (NEW)
**Location:** After Template and Layout Architecture section
**Content:** Full-viewport centred layout, no chrome, error code in Display type (57px), heading in Headline Large, message in Body Large, 480px max-width container, single "Go Home" filled button (navigates based on auth state), supports 404/500/403.

### 2. AdminLayout Section (NEW)
**Location:** After ErrorLayout section
**Content:** Sidebar navigation groups (Dashboard, Sales & Operations, Customers & Marketing, Content & Website, Finance & Reports, Settings, Vault, Sign Out), conditional visibility rules (marketing_enabled, blog_enabled, Owner-only), responsive behavior (260px fixed sidebar on desktop, hamburger drawer on mobile), plain Link + lambda navigation pattern.

### 3. Booking Design Section (NEW)
**Location:** After Invoice Design section
**Content:**
- Booking status lifecycle (Pending/Confirmed/Cancelled) with status badge colors
- Status-based permitted actions table (same pattern as Invoice)
- Booking calendar architecture: 4 template subforms (tmpl_day_event, tmpl_month_day_cell, tmpl_week_event, tmpl_week_timeslot)
- Event color coding (Confirmed=#388e3c green, Pending=#f9a825 yellow, Cancelled=#c62828 red)
- Calendar view toggle (SegmentedButton: Month/Week/Day)
- 7-step booking flow per-step UI layout with round-trip budget
- BookingListForm columns (Booking #, Customer, Service, Date, Time, Status, Actions)

### 4. Empty States Section (NEW)
**Location:** After Booking Design section
**Content:** General empty state pattern (icon + heading + body + primary action) with specifications for 10 screens: Invoice list, Task list, Campaign list, Blog list, Landing page list, Service list, Segment list, Activity feed, Vault, Analytics dashboard.

### 5. VaultForm Additions (APPENDED)
**Location:** End of existing VaultForm section
**Content:**
- TOTP entry screen layout (verification overlay, 6-digit input, verify button, success/failure states)
- Vault empty state (lock icon, "No secrets stored yet", "Add Your First Secret" button)
- Vault error state (TOTP server failure, encryption key missing)
- Vault loading state (encrypting spinner, decrypt spinner on reveal, delete spinner)

### 6. OnboardingForm Step 5 — Two Checkboxes (MODIFIED)
**Location:** OnboardingForm flow structure, step 5
**Change:** "text, checkbox, confirm button" → "two checkboxes plus confirm button"
**Added:** Full specification for `cb_legal_ack` and `cb_data_governance_ack` with exact checkbox text and enable/disable logic.

### 7. OnboardingForm Per-Step Continue Logic (ADDED)
**Location:** After OnboardingForm key design rules
**Content:** Table specifying Continue button behavior for all 6 steps: Steps 1-3 always enabled, Step 4 disabled until currency confirmed, Step 5 disabled until both checkboxes ticked, Step 6 "Go to Dashboard" sets onboarding_status to "completed".

---

## ADR Compliance

- **ADR-27 (Design Rules):** All additions follow the 9 design rules. No sample data in wireframe specs. All behaviour in specification documents.
- **ADR-41 (Dependency-based, not multi-tenant):** No tenant_id or client_id references introduced. All data access is per-client-instance.
- **ADR-003 (Vault):** TOTP entry screen and error states align with Vault security model.
- **ADR-016 (System Currency):** Step 4 enable/disable logic aligns with currency immutability.
- **ADR-018 (Legal Policy):** Two-checkbox specification aligns with legal acknowledgement requirements.
- **ADR-023 (Onboarding Resumability):** Per-step logic preserves resumability.
- **ADR-035 (Form Architecture):** Booking flow round-trip budget aligns with 3-call target.

---

*End of Remediation Report — 2026-06-16*
