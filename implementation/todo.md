# Todo

**Last Updated:** 2026-06-16 — Updated per build pass. CEO remediation audited, eng review processed, completable items executed.  
Items are reconciled against all ADRs, onboarding plan, design system, and wireframes.

---

## 1. Onboarding & Settings — Planning

### 1.1 Onboarding Status Tracker

- **Tag:** [Done]  
- **Status:** Resolved by onboarding implementation plan rewrite.  
- **Details:**  
  Onboarding is now a separate form with required elements.  
  `business_profile.onboarding_status` tracks state.  
  No separate onboarding checklist model is needed.  
  **ADR:** `25-onboarding-finality.md`

---

### 1.2 Onboarding Initiation Interface

- **Tag:** [Done]  
- **Status:** Resolved by onboarding implementation plan rewrite.  
- **Details:**  
  Mybizz_management provisions instances directly.  
  No token-based invitation links; `onboarding_tokens` table abandoned.  
  **ADR:** `25-onboarding-finality.md`

---

### 1.3 Settings Return Flow

- **Tag:** [Done]  
- **Status:** Bidirectional navigation defined.  
- **Details:**  
  - Settings form includes a “Return to Onboarding” button (visible when onboarding is in progress).  
  - Onboarding form includes buttons to jump to specific Settings tabs.  
  **ADRs:** `25-onboarding-finality.md`, onboarding implementation plan

---

## 2. Policy & Legal — Planning

### 2.1 Clause Builder Custom Component

- **Tag:** [Planned]  
- **Status:** Defined and in scope; architecture and wireframe complete. Mapped to build-plan Stage 4.2.  
- **Details:**  
  A reusable, owner-facing content editor used to build both the Privacy Policy and Terms & Conditions, operating on structured clause data.  
  **Architecture:**  
  - Standalone custom component (not embedded in each PolicyEditorForm).  
  - Parameterised by document type (`privacy_policy` or `t_and_c`).  
  **Core responsibilities:**  
  - Clause-level editing, not a single big text area.  
  - Selection from a master/template list.  
  - Editing clause text.  
  - Adding new custom clauses.  
  - Organising clauses by category and order.  
  - Reuse for both `privacy_policy` and `t_and_c` datasets.  
  **Data model:**  
  - Tables: `privacy_policy` and `t_and_c`.  
  - Fields: `category`, `title`, `clause`, `sort_order`, `is_seeded`, `is_active`.  
  - Master template seeds both documents with starter clauses.  
  **Wireframe:** `wireframes/wireframe-custom-component-ClauseBuilder.html`  
  **ADR:** `18-legal-policy-responsibility-acknowledgement-and-clause-builder-architecture.md`  
  **Build Plan:** Stage 4.2 (Public Website Pages)

---

### 2.2 Policy Document Authoring System

- **Tag:** [Planned]  
- **Status:** Defined; mapped to build-plan Stage 4.2.  
- **Details:**  
  Supports clause builder with a Policy-Editing screen in Settings → Website.  
  **Scope:**  
  - Category and clause listing with editing affordances.  
  - Clause builder integration for editing, adding, and organising clauses.  
  - Publish/update action tied to public policy pages.  
  - Version management for policy updates.  
  **Links:** clause builder (item 2.1), ADR-018.  
  **Build Plan:** Stage 4.2 (Public Website Pages)

---

### 2.3 Policy Responsibility Acknowledgement

- **Tag:** [Done]  
- **Status:** Resolved by onboarding implementation plan rewrite.  
- **Details:**  
  - Legal Acknowledgement is recorded on the Onboarding Form with checkbox + confirm.  
  - `legal_ack_accepted` and `data_governance_ack_accepted` stored in `business_profile` with timestamps.  
  - Acknowledgement logged in Mybizz_management amendment log.  
  **ADRs:** `18-legal-policy-responsibility-acknowledgement-and-clause-builder-architecture.md`, `25-onboarding-finality.md`

---

## 3. Payments & Data Rights — Planning

### 3.1 Payment Gateway Settings Module

- **Tag:** [Done]  
- **Status:** Resolved by ADR-019 and onboarding implementation plan rewrite.  
- **Details:**  
  - Payment gateway configuration is performed in Settings → Payment Gateway tab.  
  - Access is RBAC-governed (Owner only).  
  - Onboarding does not perform gateway configuration — it is post-onboarding Settings work.  
  **ADRs:** `19-payment-gateway-configuration-is-a-settings-function-and-is-rbac-governed.md`,  
  `24-payment-gateway-mutability.md`

---

### 3.2 Client Data Export / Rights Interface

- **Tag:** [Planned]  
- **Status:** Defined per ADR-021; mapped to build-plan Stage 5.3.  
- **Details:**  
  Dashboard entry point for Owner to export client data.  
  - RBAC: Owner only.  
  - Export as PDF with clear separation of:  
    - Mybizz-held records (contract, billing, support logs).  
    - Client-instance data (operational data in the client's own instance).  
  - Include source system labels in export.  
  - Part of GDPR/POPIA compliance.  
  **ADR:** `21-client-data-management-rights-and-mybizz-retention-boundary.md`  
  **Build Plan:** Stage 5.3 (Compliance Implementation)  
  **Files:** `DashboardForm` (entry point), `server_shared/export_service.py` (export logic)

---

## 4. Schema & Design System — Planning

### 4.1 Schema Alignment Review

- **Tag:** [Done]  
- **Status:** Resolved by plan-tune audit and onboarding plan rewrite.  
- **Details:**  
  - All onboarding fields align with `authoritative-schema.md`.  
  - `business_profile` (with underscore) used consistently.  
  - New onboarding fields added to the authoritative schema.  
  **ADR:** `20-onboarding-data-schema-alignment.md`

---

### 4.2 Service Catalogue Setup

- **Tag:** [Planned]  
- **Status:** Defined; mapped to build-plan Stage 2.1.  
- **Details:**  
  Clients may have multiple services (consulting or product).  
  **Scope:**  
  - Service catalogue is a Phase 2 (Transactional Core) deliverable.  
  - Services configured in Settings → Services after onboarding.  
  - Two pricing models: `duration` (time-based) and `unit` (deliverable-based).  
  - Per service: name, description, category, provider, pricing model, price, duration, images, video URL, meeting type.  
  **Build Plan:** Stage 2.1 (Service Catalogue)

---

## 5. Wireframes — Planning

### 5.1 Onboarding Form Wireframe

- **Tag:** [Done]  
- **Status:** Created 2026-06-02.  
- **Details:**  
  `wireframes/wireframe-onboarding-OnboardingForm.html` — 6-step onboarding flow:  
  1. Welcome and orientation  
  2. Business Profile → link to Settings → Business Profile tab  
  3. Palette Selection → link to Settings → Palette tab  
  4. System Currency → dropdown + confirm (required, immutable after first transaction)  
  5. Legal Acknowledgement → text + checkbox + confirm (required, logged in Mybizz_management)  
  6. Completion → onboarding complete  
  Includes progress indicator, bidirectional navigation buttons, and completion state.  
  **Source:** Onboarding implementation plan §3.3, §9.3, wireframe reconciliation audit  
  **Priority:** High  

---

### 5.2 Settings Form Wireframe

- **Tag:** [Done]  
- **Status:** Recreated from scratch (completed 2026-06-01).  
- **Details:**  
  `wireframes/wireframe-settings-SettingsForm.html` with 11 tabs:  
  Business Profile, Palette, System Currency, Legal Acknowledgement, Display Currency, Payment Gateway, Services, Users & Permissions, Email Configuration, Theme, Feature Flags.  
  Includes “Return to Onboarding” button (visible when onboarding in progress).

---

### 5.3 OnboardingPaletteForm Wireframe

- **Tag:** [Cancelled]  
- **Status:** Deleted 2026-06-01.  
- **Details:**  
  `wireframes/wireframe-custom-component-OnboardingPaletteForm.html` removed; component no longer exists in the design.

---

## 6. Design Inventory & Docs — Planning

### 6.1 Custom Components Inventory in DESIGN.md

- **Tag:** [Done]  
- **Status:** DESIGN.md reconciled 2026-06-02.  
- **Details:**  
  - `OnboardingPaletteForm` removed from custom components inventory.  
  - `PaletteSettingsForm` marked as local/form-specific (SettingsForm only).  
  - `ClauseBuilder` added as a shared custom component with summary of responsibilities.  
  **Build Plan:** Stage 4.2 (Public Website Pages) — documentation task  

---

## 7. Implementation Backlog

> These items correspond to “Actions Outstanding (Require Implementation)” in the sanity audit.  
> They are implementation-phase tasks and may be scheduled separately from planning items.

### 7.1 Onboarding Form Implementation

- **Tag:** [Planned]  
- **Type:** Feature  
- **Priority:** P1-high  
- **Source:** Sanity report Actions Outstanding #6; onboarding implementation plan; ADR-017, ADR-023, ADR-025, ADR-024.  
- **Task:**  
  Implement `OnboardingForm` as a 6-step, resumable onboarding flow:  
  1. Welcome and orientation  
  2. Business Profile → link to Settings → Business Profile tab  
  3. Palette Selection → link to Settings → Palette tab  
  4. System Currency selection → dropdown + confirm (immutable after first transaction)  
  5. Legal Acknowledgement → text + checkbox + confirm (logged as per ADR-018, ADR-025)  
  6. Completion → update `business_profile.onboarding_status` and mark onboarding complete.  

---

### 7.2 Tabbed Settings Form Implementation

- **Tag:** [Planned]  
- **Type:** Feature  
- **Priority:** P1-high  
- **Source:** Sanity report Actions Outstanding #7; settings scope; ADR-019, ADR-016.  
- **Task:**  
  Implement the tabbed `SettingsForm` with 11 tabs and RBAC-governed visibility:  
  - Tabs: Business Profile, Palette, System Currency, Legal Acknowledgement, Display Currency, Payment Gateway, Services, Users & Permissions, Email Configuration, Theme, Feature Flags.  
  - Implement “Return to Onboarding” button visible only when onboarding is in progress.  
  - Use Material 3 design system components.  

---

### 7.3 System Currency Selection (Immutable)

- **Tag:** [Planned]  
- **Type:** Feature  
- **Priority:** P1-high  
- **Source:** Sanity report Actions Outstanding #8; ADR-016, ADR-025.  
- **Task:**  
  Implement System Currency selection (Settings → System Currency):  
  - Require selection before first transaction.  
  - Provide confirm action that writes `business_profile.system_currency`.  
  - Enforce immutability after first transaction (guardrails and messaging).  

---

### 7.4 Legal Acknowledgement Implementation

- **Tag:** [Planned]  
- **Type:** Feature  
- **Priority:** P1-high  
- **Source:** Sanity report Actions Outstanding #9; ADR-018, ADR-025.  
- **Task:**  
  Implement the Legal Acknowledgement step on the Onboarding Form:  
  - Display legal responsibility text as per ADR-018.  
  - Checkbox + confirm button required to proceed.  
  - Persist `legal_ack_accepted` (and related fields) in `business_profile` with timestamps.  
  - Log acknowledgement in Mybizz_management amendment log.  

---

### 7.5 ClauseBuilder Component Implementation

- **Tag:** [Planned]  
- **Type:** Feature  
- **Priority:** P2-high  
- **Source:** Sanity report Actions Outstanding #5; ClauseBuilder decision; ADR-018.  
- **Build Plan:** Stage 4.2 (Public Website Pages)  
- **Files:** `wireframes/wireframe-custom-component-ClauseBuilder.html` (wireframe), component to be implemented  
- **Task:**  
  Implement standalone `ClauseBuilder` component, parameterised by document type (`privacy_policy`, `t_and_c`):  
  - Category selector.  
  - Seeded clauses (non-deletable, editable text, visually distinguished).  
  - Custom clauses (add, edit, delete).  
  - Ordering and activation (category + `sort_order` + `is_active`).  
  - Save action that writes back to the relevant table.

---

### 7.6 Policy Editor Screen Wireframe (Settings → Website)

- **Tag:** [Planned]  
- **Type:** Feature  
- **Priority:** P2-medium  
- **Source:** Sanity report Actions Outstanding #2; Todo item 2.2; wireframe reconciliation audit.  
- **Build Plan:** Stage 4.2 (Public Website Pages)  
- **Files:** `wireframe-settings-PolicyEditorForm.html` (created), `public_pages/PrivacyPolicyPage`, `public_pages/TermsConditionsPage`  
- **Task:**  
  Implement the Policy Editor screen in Settings → Website:  
  - Document type selector (Privacy Policy vs T&Cs)  
  - ClauseBuilder component embedded  
  - Publish/update button  
  - Version history display  
  - Integration with public policy pages  
  **Wireframe:** `wireframes/wireframe-settings-PolicyEditorForm.html`  
  **ADR:** `18-legal-policy-responsibility-acknowledgement-and-clause-builder-architecture.md`  

---

### 7.7 Privacy Policy Public Page Implementation

- **Tag:** [Planned]  
- **Type:** Feature  
- **Priority:** P2-medium  
- **Source:** Sanity report Actions Outstanding #3; ADR-018; build-plan Stage 4.2.  
- **Build Plan:** Stage 4.2 (Public Website Pages)  
- **Files:** `public_pages/PrivacyPolicyPage`  
- **Task:**  
  Implement the public Privacy Policy page that:  
  - Reads clauses from `privacy_policy` table.  
  - Renders clauses ordered by `sort_order`, grouped by category.  
  - Optionally distinguishes seeded vs custom clauses if required by ADR-018.  

---

### 7.8 Terms & Conditions Public Page Implementation

- **Tag:** [Planned]  
- **Type:** Feature  
- **Priority:** P2-medium  
- **Source:** Sanity report Actions Outstanding #4; ADR-018; build-plan Stage 4.2.  
- **Build Plan:** Stage 4.2 (Public Website Pages)  
- **Files:** `public_pages/TermsConditionsPage`  
- **Task:**  
  Implement the public Terms & Conditions page that:  
  - Reads clauses from `t_and_c` table.  
  - Renders clauses ordered by `sort_order`, grouped by category.  
  - Shares layout and styling with the Privacy Policy page where appropriate.  

---

### 7.9 Mybizz_management Amendment Log Implementation

- **Tag:** [Deferred]  
- **Type:** Feature (out-of-scope but documented intention)  
- **Priority:** P3-low  
- **Source:** Sanity report Actions Outstanding #10; Mybizz_management context; ADR-025.  
- **Task:**  
  Implement append-only amendment log for credential/legal changes in Mybizz_management:  
  - Maintain three tables: Client Details, Retained Records, Amendment Log.  
  - Enforce append-only behaviour for the log.  
  - Support export compatible with data rights interface (ADR-021).  
- **Note:**  
  Marked as deferred / out-of-scope for current project, but retained as a visible future task.

---

## 14. Policy Editor Screen Wireframe — COMPLETED

> **Status:** Created 2026-06-01.

`wireframes/wireframe-settings-PolicyEditorForm.html` — Policy Editor screen with embedded ClauseBuilder component, document type selector, publish button, and version history.

**Source:** Todo item 2.2, ADR-018, wireframe reconciliation audit  
**Build Plan:** Stage 4.2 (Public Website Pages)

---

*End of file — todo.md*

---

## 8. CEO Review 2026-06-15 — Captured Actions

### 8.1 Anvil Account Failure Recovery Documentation
- **Tag:** [Done]
- **Type:** Documentation
- **Priority:** P1
- **Source:** CEO review 2026-06-15 Section 1
- **Task:** Add account-level failure and recovery procedures to deployment-procedures.md
- **Deliverable:** `docs/deployment-procedures.md` — "Anvil Account-Level Failure Scenarios" section (3 scenarios: suspension, compromise, outage)

### 8.2 app_tables Failure Handling Spec
- **Tag:** [Done]
- **Type:** Documentation
- **Priority:** P1
- **Source:** CEO review 2026-06-15 Section 2
- **Task:** Define behavior when app_tables resolution fails in ADR-034
- **Deliverable:** `adr/34-anvil-platform-constraints.md` — Constraint 10: `app_tables` Resolution Failure

### 8.3 Background Task Failure Policy
- **Tag:** [Done]
- **Type:** Documentation
- **Priority:** P1
- **Source:** CEO review 2026-06-15 Section 2
- **Task:** Define retry/failure policy per background task in ADR-036
- **Deliverable:** `adr/36-real-time-and-background-tasks.md` — "Per-Task Failure and Retry Policy" table + "Task Priority for Graceful Degradation" section

### 8.4 TOTP Recovery Path
- **Tag:** [Done]
- **Type:** Documentation
- **Priority:** P1
- **Source:** CEO review 2026-06-15 Section 3
- **Task:** Document TOTP recovery via Mybizz_management
- **Deliverable:** `adr/34-anvil-platform-constraints.md` — Constraint 11: Vault TOTP Recovery

### 8.5 Brevo Monitoring Note
- **Tag:** [Done]
- **Type:** Documentation
- **Priority:** P2
- **Source:** CEO review 2026-06-15 Section 3
- **Task:** Add note on Brevo credential monitoring responsibility
- **Deliverable:** `docs/integration-architecture.md` — "Brevo Credential Monitoring" section

### 8.6 Edge Case Documentation (Core Flows)
- **Tag:** [Done]
- **Type:** Documentation
- **Priority:** P1
- **Source:** CEO review 2026-06-15 Section 4
- **Task:** Document booking, onboarding, dedup, currency, and landing page edge cases
- **Deliverable:** `implementation/build-plan.md` — Edge cases added to Stages 1.4, 2.3, 3.1 completion criteria

### 8.7 Build Plan Self-Review Checklists
- **Tag:** [Done]
- **Type:** Documentation
- **Priority:** P2
- **Source:** CEO review 2026-06-15 Section 5
- **Task:** Add self-review checklist to each build plan phase
- **Deliverable:** `implementation/build-plan.md` — "Self-Review Checklists" section (5 phases, 7 items each)

### 8.8 E2E Booking Test Spec
- **Tag:** [Done]
- **Type:** Test
- **Priority:** P1
- **Source:** CEO review 2026-06-15 Section 6
- **Task:** Define full booking flow E2E test
- **Deliverable:** `docs/test-specifications.md` — Section 1: E2E Booking Flow Test Spec (happy path + 3 edge cases)

### 8.9 Error Path Test Coverage
- **Tag:** [Done]
- **Type:** Test
- **Priority:** P1
- **Source:** CEO review 2026-06-15 Section 6
- **Task:** Add error-path test cases across modules
- **Deliverable:** `docs/test-specifications.md` — Section 2: Error Path Test Cases (7 scenarios: payment failure, webhook signature, dispatch failure, Brevo 429, Vault missing, dedup conflict, server timeout)

### 8.10 RBAC Coverage Matrix
- **Tag:** [Done]
- **Type:** Test
- **Priority:** P1
- **Source:** CEO review 2026-06-15 Section 6
- **Task:** Define RBAC test matrix
- **Deliverable:** `docs/test-specifications.md` — Section 3: RBAC Coverage Matrix (19 forms × 4 roles + 11 settings tabs × 4 roles)

### 8.11 Observability Gaps Documentation
- **Tag:** [Done]
- **Type:** Documentation
- **Priority:** P1
- **Source:** CEO review 2026-06-15 Section 7
- **Task:** Define thresholds, retention, and error monitoring
- **Deliverable:** `implementation/build-plan.md` — Stage 0.3 completion criteria (health thresholds, log retention policy, error rate monitoring)

### 8.12 Webhook Schema Reconciliation
- **Tag:** [Done]
- **Type:** Documentation
- **Priority:** P1
- **Source:** CEO review 2026-06-15 Section 9
- **Task:** Align webhook_log schema between ADR-038 and authoritative schema
- **Deliverable:** `adr/38-webhook-architecture.md` — Webhook Log Table reconciled with authoritative schema columns; code examples updated to use `gateway`/`event_type`/`verified`/`processed` instead of `source`/`status`/`idempotency_key`; hardening notes added (dispatch failure handling, idempotency in HTTP handler, reconciliation task)

### 8.13 Anvil Billing Model Clarification
- **Tag:** [Planned]
- **Type:** Research
- **Priority:** P2
- **Source:** CEO review 2026-06-15 Section 10
- **Task:** Resolve per-account vs per-app limits
- **Blocker:** Requires answer from Anvil support. Questions documented in `docs/questions-to-raise-with-anvil.md` Section 3-4. Graceful degradation design added to ADR-036 as mitigation.

### 8.14 UI State Specifications
- **Tag:** [Done]
- **Type:** Documentation
- **Priority:** P1
- **Source:** CEO review 2026-06-15 Section 11
- **Task:** Add empty/loading/error state specs to build plan
- **Deliverable:** `implementation/build-plan.md` — Empty/loading/error state specs added to Stages 1.3, 1.4, 2.3, 3.1 completion criteria

---

## 9. Engineering Review 2026-06-16 — Executed Items

### 9.1 Webhook Hardening Notes
- **Tag:** [Done]
- **Type:** Documentation
- **Priority:** P1
- **Source:** Engineering review 2026-06-16 Dilemma 2
- **Task:** Add dispatch failure handling, idempotency in HTTP handler, and reconciliation task to ADR-038
- **Deliverable:** `adr/38-webhook-architecture.md` — "Implementation Hardening Notes" section (3 patterns with code examples)

### 9.2 Booking Flow Round-Trip Budget
- **Tag:** [Done]
- **Type:** Documentation
- **Priority:** P1
- **Source:** Engineering review 2026-06-16 Dilemma 3
- **Task:** Document the 3-round-trip budget with specific server functions and performance gate
- **Deliverable:** `implementation/build-plan.md` — Stage 2.3 round-trip budget (3 calls: get_booking_context, get_available_slots, confirm_booking) + 3-second performance gate

### 9.3 globals.py Contract Document
- **Tag:** [Done]
- **Type:** Documentation
- **Priority:** P1
- **Source:** Engineering review 2026-06-16 Dilemma 4
- **Task:** Define a contract document listing every shared state variable in globals.py
- **Deliverable:** `docs/globals-contract.md` — 4 variables (current_contact, current_booking, booking_flow_state, current_invoice) with type, set-by, read-by, None-handling, and defensive loading pattern

### 9.4 Webhook Schema Reconciliation (Full)
- **Tag:** [Done]
- **Type:** Documentation
- **Priority:** P1
- **Source:** Engineering review 2026-06-16 Cross-Cutting Concern
- **Task:** Reconcile ADR-038 webhook_log columns with authoritative schema
- **Deliverable:** `adr/38-webhook-architecture.md` — Webhook Log Table updated to use authoritative schema column names (gateway, event_type, payload, signature, verified, processed, error_message, received_at); all code examples updated accordingly
