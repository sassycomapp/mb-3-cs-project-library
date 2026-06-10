# Mybizz CS — Engineering Alignment Review

**Date:** 2026-06-03
**Scope:** Planning-to-scaffold reconciliation — project-library vs mb-3-cs
**Method:** Full cross-reference of reconciled planning state against live app repo scaffold

---

## 1. Files and paths reviewed

### Planning files reviewed (project-library)

| File | Status |
|---|---|
| `gstack-outputs/sanity-audit-final-report.md` | Read — 437 lines |
| `gstack-outputs/plan-tune-verification-report.md` | Read — 150 lines |
| `gstack-outputs/DESIGN.md` | Read — 884 lines |
| `gstack-outputs/design-review-report.md` | Read — 328 lines |
| `gstack-outputs/design-html-report.md` | Read — 336 lines |
| `gstack-outputs/design-consultation-plan-descriprive.md` | Read — 290 lines |
| `gstack-outputs/design-consultation-plan-workflows.md` | Read — 984 lines |
| `gstack-outputs/wireframe-reconciliation-audit.md` | Read — 248 lines |
| `gstack-outputs/todo.md` | Read — 378 lines |
| `docs/platform-overview.md` | Read — 341 lines |
| `docs/nomenclature.md` | Read — 152 lines |
| `docs/authoritative-schema.md` | Read — 692 lines |
| `implementation/build-plan.md` | Read — 781 lines |

### mb-3-cs repo areas reviewed

| Area | Coverage |
|---|---|
| `client_code/` (all 12 modules, ~50 forms) | Full directory tree + all `__init__.py` files read |
| `server_code/` (11 packages, 46 files) | Full directory tree + key files read |
| `theme/` (4 files) | Full inventory |
| `tests/` (2 files) | Full inventory |
| `anvil.yaml` | Read — 1382 lines, all 33 table definitions inspected |
| `.anvil_editor.yaml` | Inspected for form registry |
| `docs/` | Confirmed absent |

---

## 2. Alignment summary

**Partially aligned — material gaps exist.**

The planning state in project-library is internally consistent (per the sanity audit). The mb-3-cs scaffold provides a reasonable Phase 0/1 skeleton with correct package structure, M3 theme, routing dependency, and server_shared implementation. However, there are significant gaps between the reconciled planning state and the current repo reality:

- 3 planned forms/components are completely absent from the repo
- The SettingsForm exists but has no tab structure (planning requires 11 tabs)
- 4 database tables required by planning are missing from anvil.yaml
- 11 onboarding-related fields on `business_profile` are missing from anvil.yaml
- All planned shared custom components are absent from the repo
- One naming drift between planning and repo (`social_x` vs `social_twitter`)

The repo is at a Phase 0/early Phase 1 scaffold stage. Planning has moved significantly ahead of the scaffold, particularly around onboarding, settings architecture, policy/legal features, and custom components.

---

## 3. Planned items missing from mb-3-cs

### 3.1 Client-side forms/components

| Item | Expected source in planning | Current repo status | Required action |
|---|---|---|---|
| **OnboardingForm** | DESIGN.md §Forms and Pages, todo.md 7.1, build-plan Stage 1.4, ADR-017/023/025 | Completely absent — no file, no reference anywhere in client_code | Scaffold update — create `client_code/onboarding/OnboardingForm/` |
| **PolicyEditorForm** | DESIGN.md §Forms and Pages, todo.md 7.6, build-plan Stage 4.2, ADR-018 | Completely absent — no file, no reference anywhere in client_code | Scaffold update — create `client_code/settings/PolicyEditorForm/` (or a Website settings sub-section) |
| **ClauseBuilder** (shared custom component) | DESIGN.md §Custom Components, todo.md 7.5, build-plan Stage 4.2, ADR-018 | Completely absent — no custom components directory exists at all | Scaffold update — create custom components package and `ClauseBuilder` component |

### 3.2 Database tables (anvil.yaml)

| Item | Expected source in planning | Current repo status | Required action |
|---|---|---|---|
| `privacy_policy` table | Authoritative schema §5A.1, DESIGN.md schema expectations, build-plan Stage 4.2 | Not in anvil.yaml (33 tables present, this is missing) | Planning update — flag as required before Stage 4.2 implementation. Do NOT edit anvil.yaml directly. |
| `t_and_c` table | Authoritative schema §5A.2, DESIGN.md schema expectations, build-plan Stage 4.2 | Not in anvil.yaml | Planning update — flag as required before Stage 4.2 implementation. Do NOT edit anvil.yaml directly. |
| `invoice_items` table | Authoritative schema §6.2, build-plan Stage 2.5 | Not in anvil.yaml | Planning update — flag as required before Stage 2.5 implementation. Do NOT edit anvil.yaml directly. |
| `time_entries` table | Authoritative schema §6.3, build-plan Stage 2.6 (deferred to V2) | Not in anvil.yaml | Lower priority — deferred to V2 per CEO review. No immediate action. |

### 3.3 business_profile onboarding fields

| Item | Expected source in planning | Current repo status | Required action |
|---|---|---|---|
| `onboarding_status` | Authoritative schema §1.2, ADR-023/025 | Not in anvil.yaml business_profile | Planning update — flag as required before OnboardingForm implementation |
| `onboarding_completed` | Authoritative schema §1.2 | Not in anvil.yaml | Same |
| `onboarding_completed_at` | Authoritative schema §1.2 | Not in anvil.yaml | Same |
| `entity_type` | Authoritative schema §1.2 | Not in anvil.yaml | Same |
| `palette` | Authoritative schema §1.2, DESIGN.md schema expectations | Not in anvil.yaml | Same |
| `legal_compliance_enabled` | Authoritative schema §1.2 | Not in anvil.yaml | Same |
| `legal_compliance_enabled_at` | Authoritative schema §1.2 | Not in anvil.yaml | Same |
| `legal_ack_accepted` | Authoritative schema §1.2, ADR-018/025 | Not in anvil.yaml | Same |
| `legal_ack_accepted_at` | Authoritative schema §1.2 | Not in anvil.yaml | Same |
| `data_governance_ack_accepted` | Authoritative schema §1.2 | Not in anvil.yaml | Same |
| `data_governance_ack_accepted_at` | Authoritative schema §1.2 | Not in anvil.yaml | Same |

### 3.4 DESIGN.md expected schema additions not in anvil.yaml or authoritative schema

| Item | Expected source | Current repo status | Required action |
|---|---|---|---|
| `blog_comments` table | DESIGN.md schema expectations, design-consultation-plan-workflows.md | Not in anvil.yaml AND not in authoritative-schema.md | Planning update — add to authoritative-schema.md, then flag for anvil.yaml |

### 3.5 Planned shared custom components (all absent)

| Item | Expected source | Current repo status | Required action |
|---|---|---|---|
| VideoDisplayComponent | DESIGN.md custom components inventory | Absent | Scaffold update — defer to Phase 4 |
| FooterComponent | DESIGN.md custom components inventory | Absent | Scaffold update — defer to Phase 4 |
| TimeLapseCarouselComponent | DESIGN.md custom components inventory | Absent | Scaffold update — defer to Phase 4 |
| ParallaxComponent | DESIGN.md custom components inventory | Absent | Scaffold update — defer to Phase 4 |
| BlogListTiledTemplate | DESIGN.md custom components inventory | Absent | Scaffold update — defer to Phase 4 |
| BlogListListTemplate | DESIGN.md custom components inventory | Absent | Scaffold update — defer to Phase 4 |
| BlogPostClassicTemplate | DESIGN.md custom components inventory | Absent | Scaffold update — defer to Phase 4 |
| BlogPostTwoColumnTemplate | DESIGN.md custom components inventory | Absent | Scaffold update — defer to Phase 4 |
| BlogPostFullWidthTemplate | DESIGN.md custom components inventory | Absent | Scaffold update — defer to Phase 4 |
| HomePageClassicTemplate | DESIGN.md custom components inventory | Absent | Scaffold update — defer to Phase 4 |
| HomePageServicesTemplate | DESIGN.md custom components inventory | Absent | Scaffold update — defer to Phase 4 |
| HomePageBookingTemplate | DESIGN.md custom components inventory | Absent | Scaffold update — defer to Phase 4 |
| HomePageMinimalistTemplate | DESIGN.md custom components inventory | Absent | Scaffold update — defer to Phase 4 |
| LeadCaptureTemplate | DESIGN.md custom components inventory | Absent | Scaffold update — defer to Phase 3 |
| EventRegistrationTemplate | DESIGN.md custom components inventory | Absent | Scaffold update — defer to Phase 3 |
| VslTemplate | DESIGN.md custom components inventory | Absent | Scaffold update — defer to Phase 3 |

**Note:** The shared custom components are Phase 3/4 deliverables per the build-plan. Their absence is expected at the current Phase 0/1 stage. Only ClauseBuilder needs early attention because it is referenced in Stage 4.2 planning and has a completed wireframe.

---

## 4. Repo items not justified by planning

| Item | Where found | Why it is out of alignment | Required action |
|---|---|---|---|
| `files` table | `anvil.yaml` | Present in anvil.yaml but NOT listed in authoritative-schema.md. The authoritative schema has 37 named tables (doc header says 38 — counting discrepancy), and `files` is not among them. | Planning update — determine if `files` is intentional (Anvil system table?) or an undocumented addition. If intentional, add to authoritative-schema.md. |
| `social_x` field name | `anvil.yaml` business_profile table | Authoritative schema and DESIGN.md use `social_twitter`. anvil.yaml uses `social_x`. This is a naming drift. | Planning update — confirm whether the canonical name is `social_x` (repo reality) or `social_twitter` (planning). Update whichever is stale. |
| `auth/__init__.py` syntax error | `mb-3-cs/client_code/auth/__init__.py` line 5 | Line reads `authfrom routing import router` — missing newline between `auth` and `from`. This is a scaffold bug. | Scaffold update — fix the syntax error. |
| Separate settings sub-forms | `client_code/settings/` has 6 separate forms: EmailSetupForm, PaymentGatewayForm, ThemeConfigForm, UserManagementForm, VaultForm | Planning says SettingsForm is a SINGLE tabbed form with 11 tabs. The separate forms may be intended as tab content panels, but planning does not define them as standalone forms. The current structure risks treating them as independent navigable forms rather than tab content within SettingsForm. | Clarification needed — determine if these are intended as (a) standalone forms loaded into SettingsForm tabs, or (b) incorrectly scaffolded as separate forms. Planning update if (a), scaffold update if (b). |

---

## 5. Canonical artefact status

| Artefact | Status | Evidence |
|---|---|---|
| **OnboardingForm** | **Missing** | No file, no reference, no scaffold anywhere in mb-3-cs. Planning fully defines it (DESIGN.md, todo.md 7.1, build-plan Stage 1.4, ADR-017/023/025). Wireframe exists. |
| **SettingsForm** | **Present but misaligned** | Exists at `client_code/settings/SettingsForm/` as an empty stub. Has `content_panel` (ColumnPanel) and `navbar_links` (FlowPanel) only. No tab structure, no 11 tabs, no RBAC logic, no "Return to Onboarding" button. Planning requires 11-tab structure with RBAC-governed visibility. |
| **PolicyEditorForm** | **Missing** | No file, no reference anywhere in mb-3-cs. Planning defines it as Settings-hosted screen using ClauseBuilder (DESIGN.md, todo.md 7.6, build-plan Stage 4.2). Wireframe exists. |
| **ClauseBuilder** | **Missing** | No file, no reference, no custom components directory exists. Planning defines it as a shared custom component (DESIGN.md, todo.md 7.5, build-plan Stage 4.2, ADR-018). Wireframe exists. |
| **PrivacyPolicyPage** | **Present but misaligned** | Exists at `client_code/public_pages/PrivacyPolicyPage/` as an empty stub. Planning requires it to read from `privacy_policy` table and render clauses by category. The `privacy_policy` table does not exist in anvil.yaml. The page currently has no data-binding or rendering logic. |
| **TermsConditionsPage** | **Present but misaligned** | Exists at `client_code/public_pages/TermsConditionsPage/` as an empty stub. Planning requires it to read from `t_and_c` table and render clauses by category. The `t_and_c` table does not exist in anvil.yaml. The page currently has no data-binding or rendering logic. |

---

## 6. Settings and onboarding architecture status

**The repo does NOT currently reflect the reconciled architecture.** The following mismatches exist:

| Aspect | Planning requirement | Repo reality | Mismatch severity |
|---|---|---|---|
| Onboarding is separate from Settings | OnboardingForm is a standalone form, not a tab in Settings | No OnboardingForm exists at all | **High** — blocks Phase 1 Stage 1.4 |
| Settings is a single tabbed form | SettingsForm has 11 tabs with M3 button navigation | SettingsForm is an empty stub with no tabs | **High** — blocks Phase 1 Stage 1.4 |
| Settings has 11 tabs | Business Profile, Palette, System Currency, Legal Acknowledgement, Display Currency, Payment Gateway, Services, Users & Permissions, Email Configuration, Theme, Feature Flags | No tabs exist. 5 separate sub-forms exist (EmailSetupForm, PaymentGatewayForm, ThemeConfigForm, UserManagementForm, VaultForm) but they are not wired as tabs | **High** |
| Tab visibility is RBAC-governed | Different roles see different tabs | No RBAC logic exists in any form | **High** — but expected at current phase |
| Return to Onboarding behaviour | SettingsForm header has "Return to Onboarding" button visible when onboarding in progress | Not implemented | **Medium** — expected at current phase |
| Legal acknowledgement belongs to onboarding | Legal acknowledgement is a required step in OnboardingForm, not a mutable Settings config | No OnboardingForm exists; no legal acknowledgement logic anywhere | **Medium** — expected at current phase |
| Payment gateway configuration belongs to Settings | Payment gateway is a Settings tab (Owner only), not part of onboarding | PaymentGatewayForm exists as a separate form in settings/ — structurally compatible but not wired as a SettingsForm tab | **Low** — scaffold direction is correct, wiring needed |

---

## 7. Custom component status

| Component | Repo status | Planning status | Alignment |
|---|---|---|---|
| **ClauseBuilder** | Absent — no file, no directory, no reference | Shared custom component, wireframe complete, mapped to build-plan Stage 4.2 | **Missing** — but Stage 4.2 is not yet active |
| **PaletteSettingsForm** | Absent — no file, no reference | Local/form-specific component for SettingsForm Palette tab | **Missing** — but only needed when SettingsForm tabs are implemented |
| **OnboardingPaletteForm** | Correctly absent | Obsolete/deleted per sanity audit | **Aligned** — correctly not present |
| **VideoDisplayComponent** | Absent | Shared custom component, Phase 4 | **Missing** — expected at current phase |
| **FooterComponent** | Absent | Shared custom component, Phase 4 | **Missing** — expected at current phase |
| **TimeLapseCarouselComponent** | Absent | Shared custom component, Phase 4 | **Missing** — expected at current phase |
| **ParallaxComponent** | Absent | Shared custom component, Phase 4 | **Missing** — expected at current phase |
| **All blog/homepage templates** | Absent | Shared custom components, Phase 4 | **Missing** — expected at current phase |
| **All landing page templates** | Absent | Shared custom components, Phase 3 | **Missing** — expected at current phase |

**Key finding:** No custom components directory or package exists in mb-3-cs. The repo has no mechanism for shared custom components yet. This is expected at Phase 0/1 but will need to be scaffolded before Phase 3/4 work begins.

---

## 8. Naming and scaffold issues

### 8.1 Naming drift

| Issue | Location | Detail | Severity |
|---|---|---|---|
| `social_x` vs `social_twitter` | anvil.yaml business_profile vs authoritative-schema.md + DESIGN.md | anvil.yaml uses `social_x`; planning uses `social_twitter`. The repo name (`social_x`) is likely the newer/correct one (X rebrand), but planning has not been updated. | Medium |
| `files` table undocumented | anvil.yaml vs authoritative-schema.md | `files` table exists in anvil.yaml but is not listed in the authoritative schema. May be an Anvil system table or an undocumented addition. | Low |

### 8.2 Scaffold bugs

| Issue | Location | Detail | Severity |
|---|---|---|---|
| Syntax error in `auth/__init__.py` | `mb-3-cs/client_code/auth/__init__.py` line 5 | `authfrom routing import router` — missing newline. Will cause import errors. | Medium |
| Package `__init__.py` boilerplate | All package `__init__.py` files | All contain `say_hello()` placeholder and unnecessary imports. Not harmful but indicates untouched scaffold. | Low |

### 8.3 Scaffold structure mismatches

| Issue | Detail | Severity |
|---|---|---|
| No `client_code/onboarding/` package | Planning requires OnboardingForm in its own package | High |
| No `client_code/components/` or custom components package | Planning requires shared custom components (ClauseBuilder, etc.) | Medium — not needed until Phase 3/4 |
| No `client_code/website/` or `client_code/landing_pages/` package | Build-plan Stage 3.5 expects `landing_pages/` package with LandingPageListForm and LandingPageEditorForm | Low — Phase 3 |
| `DashboardAnalyticsForm` is the startup form | anvil.yaml startup_form points to `analytics.DashboardAnalyticsForm`. Planning says LoginForm should be the entry point for unauthenticated users. The analytics dashboard imports `plotly` which may not be installed. | Low — can be adjusted during Phase 1 |
| 5 separate settings sub-forms vs 1 tabbed form | EmailSetupForm, PaymentGatewayForm, ThemeConfigForm, UserManagementForm, VaultForm exist as standalone forms. Planning defines SettingsForm as a single tabbed form. The relationship between these is undefined. | Medium |

---

## 9. Required follow-up actions

### 9.1 Planning updates

| # | Action | Priority |
|---|---|---|
| P1 | Reconcile `social_x` (anvil.yaml) vs `social_twitter` (authoritative-schema.md, DESIGN.md) — determine canonical name and update the stale document | Medium |
| P2 | Add `blog_comments` table to authoritative-schema.md (currently in DESIGN.md expectations but missing from the authoritative schema) | Low |
| P3 | Clarify the status of the `files` table in anvil.yaml — add to authoritative-schema.md if intentional, flag for removal if not | Low |
| P4 | Document the relationship between the 5 existing settings sub-forms (EmailSetupForm, PaymentGatewayForm, etc.) and the planned 11-tab SettingsForm — are they tab content panels or incorrectly scaffolded standalone forms? | Medium |
| P5 | Verify authoritative-schema.md table count (header says 38, section headers sum to 37) — resolve the internal counting discrepancy | Low |

### 9.2 Scaffold updates

| # | Action | Priority | Build-plan dependency |
|---|---|---|---|
| S1 | Create `client_code/onboarding/OnboardingForm/` scaffold (empty stub) | High | Stage 1.4 |
| S2 | Refactor SettingsForm to include tab structure (11 tabs as M3 button navigation) | High | Stage 1.4 |
| S3 | Wire existing settings sub-forms as tab content panels within SettingsForm | High | Stage 1.4 |
| S4 | Create `client_code/settings/PolicyEditorForm/` scaffold (empty stub) | Medium | Stage 4.2 |
| S5 | Create custom components package and `ClauseBuilder` scaffold | Medium | Stage 4.2 |
| S6 | Fix syntax error in `client_code/auth/__init__.py` line 5 | Medium | Immediate |
| S7 | Add onboarding fields to `business_profile` in anvil.yaml (11 fields per authoritative schema) | High | Stage 1.4 — do NOT edit anvil.yaml directly; use Anvil IDE |
| S8 | Create `privacy_policy` and `t_and_c` tables in anvil.yaml | Medium | Stage 4.2 — do NOT edit anvil.yaml directly; use Anvil IDE |
| S9 | Create `invoice_items` table in anvil.yaml | Medium | Stage 2.5 — do NOT edit anvil.yaml directly; use Anvil IDE |

### 9.3 Implementation updates

| # | Action | Priority | Build-plan dependency |
|---|---|---|---|
| I1 | No implementation changes recommended at this time — repo is correctly at Phase 0/1 scaffold stage | N/A | N/A |

### 9.4 Todo updates

| # | Action | Priority |
|---|---|---|
| T1 | Add todo item: "Scaffold OnboardingForm" — mapped to build-plan Stage 1.4 | High |
| T2 | Add todo item: "Refactor SettingsForm to 11-tab structure" — mapped to build-plan Stage 1.4 | High |
| T3 | Add todo item: "Add onboarding fields to business_profile in anvil.yaml" — mapped to build-plan Stage 1.4 | High |
| T4 | Update todo.md item 7.7 (Privacy Policy Page) to note that `privacy_policy` table must be created in anvil.yaml first | Medium |
| T5 | Update todo.md item 7.8 (Terms & Conditions Page) to note that `t_and_c` table must be created in anvil.yaml first | Medium |

---

## 10. Final verdict

**Alignment ready = No**

**Blocking mismatches:**

1. **OnboardingForm is completely absent from mb-3-cs** — planning defines it as a core Phase 1 deliverable (Stage 1.4) with a 6-step flow, wireframe, and 4 ADRs. No scaffold, placeholder, or reference exists in the repo. Build work on onboarding cannot proceed without creating this form.

2. **SettingsForm has no tab structure** — planning requires 11 tabs with RBAC-governed visibility. The repo has an empty stub with no tabs, no RBAC logic, and no connection to the 5 existing settings sub-forms. Build work on settings cannot proceed without establishing the tab architecture.

3. **11 onboarding fields missing from business_profile in anvil.yaml** — the authoritative schema defines `onboarding_status`, `legal_ack_accepted`, `entity_type`, `palette`, and 7 other fields. None exist in the actual repo schema. Onboarding implementation depends on these fields.

4. **4 database tables missing from anvil.yaml** — `privacy_policy`, `t_and_c`, `invoice_items`, and `time_entries` are defined in the authoritative schema but absent from the repo. The policy/legal feature set (ClauseBuilder, PrivacyPolicyPage, TermsConditionsPage) depends on the first two.

5. **social_x vs social_twitter naming drift** — anvil.yaml uses `social_x`, planning uses `social_twitter`. This will cause code/schema mismatch if not reconciled before implementation references this field.

6. **No custom components mechanism exists** — the repo has no custom components directory or package. While most custom components are Phase 3/4, ClauseBuilder is Phase 4.2 and needs a component architecture to be established.

---

*End of eng-review alignment report — 2026-06-03*
