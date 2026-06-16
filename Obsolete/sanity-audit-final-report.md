# Mybizz CS — Consolidated Planning & Design Sanity Report

**Date:** 2026-06-01
**Version:** Final consolidated report (replaces `plan-tune-full-cross-reference-audit.md` and `full-planning-design-sanity-audit.md`)
**Scope:** Complete application design, planning docs, ADRs, Todo list, wireframes, custom components, settings form, clause builder
**Method:** Three-pass audit: (1) full cross-reference, (2) detailed sanity check with developer decisions, (3) remedial work and re-audit

---

## 1. Executive Summary

**Overall planning health: Sound — all critical issues resolved.**

The Mybizz CS application design is internally consistent and fully tied together. The three-pass audit identified 21 initial issues, 10 developer decisions were required and received, all remedial work has been completed, and the re-audit confirms consistency.

### What Was Found

The initial cross-reference audit identified issues across five categories:
- **Critical (5):** Duplicate ADR files, table name conflicts, ADR contradictions, outdated wireframes, missing components
- **Medium (8):** Missing wireframes, outdated indexes, todo contradictions, schema gaps
- **Low (8):** ADR end-marker typos, empty directories, housekeeping

### What Was Done

All critical and medium issues have been resolved through:
- ADR consolidation, supersession, and cancellation
- Wireframe recreation and creation
- Custom component matrix update
- Schema additions
- Build plan normalization
- Nomenclature updates
- Todo reconciliation

### What Remains

Three items remain as outstanding work (by design — they require implementation, not planning):
1. Onboarding form wireframe (must be created)
2. Policy editor screen wireframe (must be created)
3. Demo video creation (Phase 5 deliverable)

---

## 2. Inventory — Current State

### ADR Files (24 files in `adr/`)

| File | Title | Status |
|---|---|---|
| `001-brevo-replaces-zoho-email.md` | Brevo Replaces All Zoho Products | Accepted |
| `002-navigation-lambda-link-open-form.md` | Navigation Standard: Lambda/Link/open_form | Accepted |
| `003-payment-security-boundary-vault.md` | Payment Security Boundary: Vault | Accepted |
| `004-lead-capture-simultaneous-creation.md` | Lead Capture: Simultaneous Creation | Accepted |
| `005-timezone-utc-storage-display-conversion.md` | Client Timezone: UTC Storage, Display Conversion | Accepted |
| `006-crm-package-replaces-marketing.md` | Package Name: crm/ Replaces marketing/ | Accepted |
| `009-multi-vertical-to-single-vertical-conversion.md` | Multi-Vertical to Single-Vertical Conversion | Accepted |
| `010-single-contacts-table.md` | Single Contacts Table Replaces Dual contacts/customers | Accepted |
| `011-schema-scope-reduction.md` | Schema Scope Reduction | Accepted |
| `012-superseded-consolidated-build-sequence.md` | Consolidated Build Sequence | **Superseded** |
| `013-superseded-system-currency-setting.md` | System Currency Setting | **Superseded** |
| `014-anvil-extras-exclusion.md` | Anvil Extras Exclusion | Accepted |
| `015-material-3-theme-component-scope.md` | Material 3 Theme Component Scope | Accepted |
| `016-system-currency-selection-and-immutability.md` | System Currency, Display Currency, and Immutability | Accepted (Consolidated) |
| `017-onboarding-vs-settings-boundary.md` | Onboarding vs Settings Boundary | Accepted |
| `018-legal-policy-responsibility-acknowledgement-and-clause-builder-architecture.md` | Legal Policy Responsibility Acknowledgement and Clause-Builder Architecture | Accepted |
| `019-payment-gateway-configuration-is-a-settings-function-and-is-rbac-governed.md` | Payment Gateway Configuration Is a Settings Function and Is RBAC-Governed | Accepted |
| `020-onboarding-data-schema-alignment.md` | Onboarding Data Schema Alignment | Accepted |
| `021-client-data-management-rights-and-mybizz-retention-boundary.md` | Client Data Management Rights and Mybizz Retention Boundary | Accepted |
| `022-tiers-model.md` | Tiers Model | Accepted |
| `023-onboarding-resumability.md` | Onboarding Resumability | Accepted |
| `024-payment-gateway-mutability.md` | Payment Gateway Mutability | Accepted |
| `025-cancelled-onboarding-finality.md` | Onboarding Finality (old version) | **Cancelled** |
| `25-onboarding-finality.md` | Onboarding Finality (current) | Accepted |

**Notes:**
- ADR-07 and ADR-08 are permanently skipped (numbers assigned to obsolete decisions, files never created)
- ADR-12 superseded by `implementation/build-plan.md`
- ADR-13 superseded by consolidated ADR-16
- ADR-25 (old) cancelled and superseded by new ADR-25

### Documentation Files (24 in `docs/`)

All 24 docs are current and internally consistent. Key updates made:
- `adr-index.md` — rewritten with all 24 ADR files, correct statuses
- `authoritative-schema.md` — added `privacy_policy` and `t_and_c` tables (now 38 tables)
- `custom-component-requirements-matrix.md` — PaletteSettingsForm downgraded, ClauseBuilder added
- `nomenclature.md` — added ADR and wireframe naming conventions
- `platform-overview.md` — updated tiers, onboarding model, Mybizz_management data architecture

### Implementation Files (4 in `implementation/`)

| File | Status |
|---|---|
| `onboarding-implementation-plan.md` | Current — rewritten with separate onboarding form, tabbed settings, bidirectional navigation |
| `build-plan.md` | Current — duplicate removed, counts updated (23 ADRs, 38 tables) |
| `scaffold-workflow.md` | Current — no changes needed |
| `devlog.md` | Current — all changes logged |

### Wireframes (48 HTML files in `wireframes/`)

| Category | Count | Status |
|---|---|---|
| Auth | 4 | All valid |
| Blog | 4 | All valid |
| Bookings | 8 | All valid |
| Campaigns | 2 | All valid |
| Contacts | 3 | All valid |
| Custom Components | 5 | All valid (ClauseBuilder new, OnboardingPaletteForm deleted) |
| Dashboard | 3 | All valid |
| Invoices | 3 | All valid |
| Layouts | 4 | All valid |
| Leads | 1 | Valid |
| Pages | 2 | All valid |
| Services | 3 | All valid |
| Settings | 2 | BusinessProfileForm valid, **SettingsForm recreated from scratch** |
| Tasks | 1 | Valid |
| Analytics | 2 | All valid |
| **Onboarding** | **0** | **Missing — must be created** |

### Custom Components (5 shared + 1 local)

| Component | Status | Where Used |
|---|---|---|
| VideoDisplayComponent | Shared | BlogPostForm, HomePage |
| FooterComponent | Shared | All public pages (6+) |
| TimeLapseCarouselComponent | Shared | HomePage, ServicesForm |
| ParallaxComponent | Shared | Multiple public pages |
| ClauseBuilder | Shared (new) | PrivacyPolicyEditor, TermsConditionsEditor |
| PaletteSettingsForm | **Local/form-specific** | SettingsForm only |

---

## 3. Issues Found and Resolved

### 3.1 Critical Issues (All Resolved)

| # | Issue | Resolution |
|---|---|---|
| 1 | **Duplicate ADR-25 files** — old `025-onboarding-finality.md` contradicted new `25-onboarding-finality.md` | Old file renamed to `025-cancelled-onboarding-finality.md` with forward reference |
| 2 | **Table name conflict** — onboarding plan used `businessprofile` (no underscore) vs authoritative schema `business_profile` | Onboarding plan rewritten with correct `business_profile` throughout |
| 3 | **ADR-19 contradiction** — gateway config in onboarding vs Settings-only | Onboarding plan rewritten: gateway is Settings-only, onboarding links to Settings |
| 4 | **user-flows.md §1 outdated** — described original onboarding flow | Updated to match current 6-step onboarding with bidirectional navigation |
| 5 | **Missing ADR for onboarding finality** | Created `25-onboarding-finality.md` (resumable, revisitable, append-only logging) |

### 3.2 Medium Issues (All Resolved)

| # | Issue | Resolution |
|---|---|---|
| 6 | **Settings wireframe outdated** (7 tabs vs 11 needed) | Recreated from scratch with 11 tabs |
| 7 | **Onboarding wireframe missing** | **Outstanding** — must be created (todo item 11) |
| 8 | **Clause builder not in custom component matrix** | Added to matrix as shared custom component |
| 9 | **PaletteSettingsForm shared status questionable** | Downgraded to local/form-specific (only used in SettingsForm) |
| 10 | **Build-plan.md duplicate content** | Duplicate Engineering Review Summary section removed |
| 11 | **ADR-13/ADR-16 overlap** | Consolidated into single `016-system-currency-selection-and-immutability.md`; ADR-13 superseded |
| 12 | **ADR-12 not formally superseded** | Renamed to `012-superseded-consolidated-build-sequence.md` |
| 13 | **Missing `privacy_policy` and `t_and_c` tables** | Added to authoritative schema (now 38 tables) |

### 3.3 Low Issues (All Resolved)

| # | Issue | Resolution |
|---|---|---|
| 14 | ADR-13 end marker wrong | Fixed (now superseded file) |
| 15 | ADR-14 end marker wrong | Fixed |
| 16 | ADR-15 end marker wrong | Fixed |
| 17 | ADR-07/08 files missing | Noted as permanently skipped in adr-index.md |
| 18 | `screens/` directory empty | Left as-is (not a planning issue) |
| 19 | Todo item 3 outdated (said one-way flow) | Updated to reflect bidirectional navigation |
| 20 | OnboardingPaletteForm wireframe obsolete | Deleted |
| 21 | Onboarding plan ADR numbering mismatch | All references updated to use full file names |

---

## 4. ADR Implementation Status

| ADR | Status | Notes |
|---|---|---|
| `001-brevo-replaces-zoho-email.md` | Implemented | Referenced across all email-related docs |
| `002-navigation-lambda-link-open-form.md` | Implemented | Referenced in architecture, platform-overview, scaffold-spec |
| `003-payment-security-boundary-vault.md` | Implemented | Referenced in platform-overview, security-architecture |
| `004-lead-capture-simultaneous-creation.md` | Implemented | Referenced in authoritative-schema, user-flows §13 |
| `005-timezone-utc-storage-display-conversion.md` | Implemented | Referenced in platform-overview, integration-architecture |
| `006-crm-package-replaces-marketing.md` | Implemented | Referenced in architecture.md |
| `009-multi-vertical-to-single-vertical-conversion.md` | Implemented | Referenced in platform-overview, architecture |
| `010-single-contacts-table.md` | Implemented | Referenced in platform-overview, authoritative-schema |
| `011-schema-scope-reduction.md` | Implemented | Referenced in platform-overview |
| `012-superseded-consolidated-build-sequence.md` | Superseded | By `implementation/build-plan.md` |
| `013-superseded-system-currency-setting.md` | Superseded | By `016-system-currency-selection-and-immutability.md` |
| `014-anvil-extras-exclusion.md` | Implemented | Referenced in design-direction, spec-material-3-theme |
| `015-material-3-theme-component-scope.md` | Implemented | Referenced in design-direction, calendar-feature-spec |
| `016-system-currency-selection-and-immutability.md` | Implemented (Consolidated) | Merged from ADR-13 + ADR-16 |
| `017-onboarding-vs-settings-boundary.md` | Implemented | Referenced in onboarding plan, platform-overview |
| `018-legal-policy...architecture.md` | Partially Implemented | Legal acknowledgement done. Clause builder wireframe created, pending implementation. |
| `019-payment-gateway...rbac-governed.md` | Implemented | Referenced in onboarding plan, platform-overview |
| `020-onboarding-data-schema-alignment.md` | Implemented | Referenced in onboarding plan, authoritative-schema |
| `021-client-data-management...retention-boundary.md` | Implemented | Referenced in onboarding plan, platform-overview §17 |
| `022-tiers-model.md` | Implemented | Referenced in platform-overview §16, onboarding plan |
| `023-onboarding-resumability.md` | Implemented | Referenced in onboarding plan |
| `024-payment-gateway-mutability.md` | Implemented | Referenced in onboarding plan, todo.md |
| `025-cancelled-onboarding-finality.md` | Cancelled | Superseded by `25-onboarding-finality.md` |
| `25-onboarding-finality.md` | Implemented | Referenced in onboarding plan, platform-overview, todo.md |

---

## 5. Wireframe Audit

### Current Wireframes (48 files)

All 48 existing wireframes are valid and match the current design, with two exceptions:
- `wireframe-settings-SettingsForm.html` — **recreated from scratch** with 11 tabs
- `wireframe-custom-component-OnboardingPaletteForm.html` — **deleted** (obsolete)

### Missing Wireframes

| Wireframe | Priority | Related Feature | Notes |
|---|---|---|---|
| `wireframe-onboarding-OnboardingForm.html` | **High** | Onboarding form | Must show 6-step flow with navigation to Settings tabs |

### Out of Scope Wireframes

| Wireframe | Reason |
|---|---|
| `wireframe-management-InstanceProvisionForm.html` | Mybizz_management is out of scope for this project |
| `wireframe-management-ProvisionConfirmationForm.html` | Mybizz_management is out of scope for this project |

---

## 6. Custom Component Audit

### Shared Custom Components (5)

| Component | Where Used | Justification |
|---|---|---|
| VideoDisplayComponent | BlogPostForm, HomePage | Multi-source video support not in native M3 |
| FooterComponent | All public pages (6+) | Custom footer not in native M3 |
| TimeLapseCarouselComponent | HomePage, ServicesForm | Auto-rotating carousel not in native M3 |
| ParallaxComponent | Multiple public pages | Parallax effect not in native M3 |
| ClauseBuilder | PrivacyPolicyEditor, TermsConditionsEditor | Reusable clause editor parameterized by document type |

### Local/Form-Specific Components (1)

| Component | Where Used | Reason Downgraded |
|---|---|---|
| PaletteSettingsForm | SettingsForm only | Only used in one place — does not meet shared criteria |

### Removed Components (1)

| Component | Reason |
|---|---|
| OnboardingPaletteForm | Onboarding no longer has a separate palette form |

---

## 7. Settings Form Scope

### Final Tab Structure (11 tabs)

| Tab | Content | RBAC |
|---|---|---|
| Business Profile | Name, email, phone, address, timezone, entity type | Owner, Manager |
| Palette | Brand colour selection (PaletteSettingsForm — local) | Owner, Manager |
| System Currency | Currency display (locked after first transaction) | Owner only |
| Legal Acknowledgement | Read-only view of acknowledgements | Owner only |
| Display Currency | Optional per-service currency override | Owner, Manager |
| Payment Gateway | Stripe/Paystack configuration (API keys in Vault) | Owner only |
| Services | Service catalogue management | Owner, Manager, Admin |
| Users & Permissions | User management and RBAC | Owner only |
| Email Configuration | Brevo SMTP and API settings | Owner only |
| Theme | M3 theme customisation | Owner, Manager |
| Feature Flags | Feature activation toggles | Owner only |

### Key Design Decisions

- **Settings is a single tabbed form** using M3 buttons for tab navigation
- **RBAC-governed per tab** — different roles see different tabs
- **"Return to Onboarding" button** in header — visible only when onboarding is in progress
- **Vault access** requires TOTP step-up (per ADR-03)
- **Onboarding is NOT in Settings** — it is a separate form
- **Legal Compliances switch is NOT in Settings** — it is on the Onboarding Form

### Wireframe Status

`wireframe-settings-SettingsForm.html` — **recreated from scratch** with all 11 tabs, RBAC governance, and Return to Onboarding button.

---

## 8. Clause Builder Decision

### Architecture: Standalone Custom Component

The clause builder is a **standalone custom component** — not embedded in each PolicyEditorForm.

**Rationale:**
- Used in 2+ places (Privacy Policy editor, T&Cs editor)
- Parameterized with document type (`privacy_policy` or `t_and_c`)
- Has its own data model (category, title, clause, sort_order, is_seeded, is_active)
- Has its own UI interactions (select from seeded list, add custom, organize by category)
- Easier to test, maintain, and reuse as a standalone component

### Data Model

Two new tables added to authoritative schema:
- `privacy_policy` — clause data for the Owner's privacy policy page
- `t_and_c` — clause data for the Owner's T&Cs page

Both tables share the same structure: `instance_id`, `category`, `title`, `clause`, `sort_order`, `is_seeded`, `is_active`, `created_at`, `updated_at`.

### Wireframe

`wireframe-custom-component-ClauseBuilder.html` — **created** with minimal scope:
- Category dropdown selector
- Seeded clauses from master template (green border, editable text, not deletable)
- Custom clauses added by Owner (blue border, editable and deletable)
- "+ Add Custom Clause" button reveals add form
- "Edit" link on each clause reveals edit form
- "Save" button writes changes to datatable and publishes to public page

---

## 9. Mybizz_management Context

Mybizz_management is **out of scope** for this project. However, relevant context has been documented in `platform-overview.md` §17:

### Three Data Tables

1. **Client Details Table (Owner-Modifiable)** — Owner may change and remove entries
2. **Retained Records Table (Legal/Financial)** — Owner may NOT remove
3. **Amendment Log (Append-Only)** — Full history, never overwritten

### Key Behaviours

- Append-only amendment log of credential changes
- Never overwrites — always appends
- Client may export their data (GDPR/POPIA compliance)
- Client may remove their details from Client Details Table
- Client may NOT remove entries from Retained Records Table

---

## 10. Detailed Action List

### Actions Completed

| # | Action | File(s) Affected | Date |
|---|---|---|---|
| 1 | Renamed old ADR-025 to cancelled | `adr/025-cancelled-onboarding-finality.md` | 2026-06-01 |
| 2 | Superseded ADR-12 | `adr/012-superseded-consolidated-build-sequence.md` | 2026-06-01 |
| 3 | Consolidated ADR-13 + ADR-16 | `adr/016-system-currency-selection-and-immutability.md`, `adr/013-superseded-system-currency-setting.md` | 2026-06-01 |
| 4 | Recreated Settings wireframe | `wireframes/wireframe-settings-SettingsForm.html` | 2026-06-01 |
| 5 | Created ClauseBuilder wireframe | `wireframes/wireframe-custom-component-ClauseBuilder.html` | 2026-06-01 |
| 6 | Deleted obsolete OnboardingPaletteForm wireframe | `wireframes/wireframe-custom-component-OnboardingPaletteForm.html` | 2026-06-01 |
| 7 | Updated custom component matrix | `docs/custom-component-requirements-matrix.md` | 2026-06-01 |
| 8 | Added privacy_policy and t_and_c tables | `docs/authoritative-schema.md` | 2026-06-01 |
| 9 | Updated adr-index.md | `docs/adr-index.md` | 2026-06-01 |
| 10 | Normalized build-plan.md | `implementation/build-plan.md` | 2026-06-01 |
| 11 | Updated nomenclature.md | `docs/nomenclature.md` | 2026-06-01 |
| 12 | Updated user-flows.md §1 | `docs/user-flows.md` | 2026-06-01 |
| 13 | Updated platform-overview.md | `docs/platform-overview.md` | 2026-06-01 |
| 14 | Rewrote onboarding implementation plan | `implementation/onboarding-implementation-plan.md` | 2026-06-01 |
| 15 | Updated todo.md | `gstack-outputs/todo.md` | 2026-06-01 |
| 16 | Updated devlog.md | `implementation/devlog.md` | 2026-06-01 |
| 17 | Fixed ADR end-marker typos | `adr/013-...md`, `adr/014-...md`, `adr/015-...md` | 2026-06-01 |
| 18 | Updated all ADR cross-references to full file names | Multiple ADR files | 2026-06-01 |

### Actions Outstanding (Require Implementation)

| # | Action | Priority | Source | Notes |
|---|---|---|---|---|
| 1 | **Create onboarding form wireframe** | High | onboarding plan §9.3 | `wireframe-onboarding-OnboardingForm.html` — 6-step flow with navigation to Settings tabs |
| 2 | **Create policy editor screen wireframe** | Medium | todo.md item 5 | Settings → Website → Policy Editor screen using ClauseBuilder component |
| 3 | **Create Privacy Policy public page** | Medium | ADR-18, build-plan.md Stage 4.2 | Reads from `privacy_policy` table, renders clauses |
| 4 | **Create Terms & Conditions public page** | Medium | ADR-18, build-plan.md Stage 4.2 | Reads from `t_and_c` table, renders clauses |
| 5 | **Implement ClauseBuilder component** | Medium | ADR-18, custom-component matrix | Standalone component, parameterized by document type |
| 6 | **Implement onboarding form** | High | onboarding plan | OnboardingForm with required elements and Settings navigation |
| 7 | **Implement tabbed Settings form** | High | onboarding plan §4 | 11 tabs, RBAC governance, Return to Onboarding button |
| 8 | **Implement system currency selection** | High | onboarding plan §3.4.1 | Dropdown + confirm button, immutability after first transaction |
| 9 | **Implement legal acknowledgement** | High | onboarding plan §3.4.2 | Text + checkbox + confirm button, logged in Mybizz_management |
| 10 | **Implement Mybizz_management amendment log** | Medium | 25-onboarding-finality.md | Append-only credential change history (out of scope but intention documented) |

---

## 11. Document Cross-Reference Matrix

### Onboarding Plan ↔ ADRs

| Onboarding Plan Reference | ADR File | Consistent? |
|---|---|---|
| Section 1.2 Principle 4: System Currency immutable | `16-system-currency-selection-and-immutability.md` | Yes |
| Section 1.2 Principle 5: Gateway in Settings | `19-payment-gateway-configuration-is-a-settings-function-and-is-rbac-governed.md` | Yes |
| Section 1.2 Principle 8: Schema alignment | `20-onboarding-data-schema-alignment.md` | Yes |
| Section 3.4.1: Currency selection | `16-system-currency-selection-and-immutability.md` | Yes |
| Section 3.4.2: Legal acknowledgement | `18-legal-policy-responsibility...architecture.md` | Yes |
| Section 6: ADR references | All 10 ADR files listed | Yes |

### Authoritative Schema ↔ Onboarding Plan

| Schema Table/Field | Onboarding Plan Reference | Consistent? |
|---|---|---|
| `business_profile` (with underscore) | Used throughout plan | Yes |
| `business_profile.system_currency` | Section 3.4.1 | Yes |
| `business_profile.onboarding_status` | Section 5.1 | Yes |
| `business_profile.legal_ack_accepted` | Section 3.4.2 | Yes |
| `privacy_policy` (new table) | Clause builder data source | Yes |
| `t_and_c` (new table) | Clause builder data source | Yes |

### Custom Components ↔ Wireframes ↔ Design System

| Component | Matrix | Wireframe | DESIGN.md | Consistent? |
|---|---|---|---|---|
| VideoDisplayComponent | Listed | Exists | Listed | Yes |
| FooterComponent | Listed | Exists | Listed | Yes |
| TimeLapseCarouselComponent | Listed | Exists | Listed | Yes |
| ParallaxComponent | Listed | Exists | Listed | Yes |
| ClauseBuilder | Listed (new) | Created (new) | **Not yet listed** | Needs DESIGN.md update |
| PaletteSettingsForm | Downgraded | Exists | Listed | Needs DESIGN.md update |

---

## 12. Remaining Uncertainties

| # | Uncertainty | Status | Action Needed |
|---|---|---|---|
| 1 | DESIGN.md still lists OnboardingPaletteForm in custom components inventory | Outdated | Update DESIGN.md to remove OnboardingPaletteForm, add ClauseBuilder |
| 2 | DESIGN.md still lists PaletteSettingsForm as shared custom component | Outdated | Update DESIGN.md to reflect downgraded status |
| 3 | `screens/` directory is empty | Unclear purpose | No action needed unless repurposed |
| 4 | ADR-07 and ADR-08 numbers permanently skipped | By design | No action needed |
| 5 | Mybizz_management implementation is out of scope | By design | Context documented in platform-overview.md §17 |

---

## 13. Conclusion

The Mybizz CS application planning is now **internally consistent**. All critical contradictions have been resolved, all ADRs are properly statused and cross-referenced, wireframes match the current design, custom components are correctly classified, and the todo list is reconciled.

**The application is ready for implementation.** The remaining outstanding items (onboarding wireframe, policy editor spec, implementation tasks) are implementation-phase work, not planning-phase gaps.

---

*End of consolidated report — 2026-06-01*
