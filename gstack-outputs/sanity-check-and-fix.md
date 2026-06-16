# Sanity Audit and Remediation Report

**Date:** 2026-06-15
**Scope:** Full project-library — `adr/`, `docs/`, `gstack-outputs/`, `implementation/`
**Method:** Complete read of all 39 ADRs, 35 docs files, 22 gstack-outputs files, 5 implementation files. Cross-reference analysis against authoritative sources.
**Status:** All issues resolved

---

## 1. Executive Summary

A full sanity audit of the project-library identified 18 issues across four severity levels. All issues have been remediated:

| Severity | Found | Resolved |
|---|---|---|
| Critical | 5 | 5 |
| High | 5 | 5 |
| Medium | 5 | 5 |
| Low | 3 | 3 (noted, no action required) |

Key themes:
- Stale cross-references from ADR naming convention changes
- Hardcoded counts that contradicted authoritative sources
- One critical terminology error ("multi-tenant" in HANDOVER document)
- Stale consequence in ADR-29 contradicting confirmed architecture
- 18 historical/obsolete session files requiring cleanup

---

## 2. Issues Found and Resolved

### CRITICAL (5)

#### C1. ADR-41 end marker said "ADR-034"
- **File:** `adr/41-dependency-based-not-multi-tenant.md` (line 269)
- **Problem:** End marker read `*End of ADR-034*` instead of `*End of ADR-041*`. Copy-paste error.
- **Fix:** Changed to `*End of ADR-041*`

#### C2. ADR-26 title said "25"
- **File:** `adr/26-onboarding-finality.md` (line 1)
- **Problem:** Title read `# 25 — Onboarding Finality` but filename is `26-onboarding-finality.md`.
- **Fix:** Changed to `# 26 — Onboarding Finality`

#### C3. ADR-29 contradicted ADR-28 on form location
- **File:** `adr/29-dependency-update-model.md` (line 109)
- **Problem:** Consequences section stated "UI form updates do not propagate (forms live in client instance clone)" — directly contradicts ADR-028 Test B which proved forms live in master_template and propagate automatically.
- **Fix:** Changed to `✅ UI and form updates propagate automatically via master_template dependency (confirmed by Test B — see ADR-028)`

#### C4. HANDOVER document said "multi-tenant"
- **File:** `docs/HANDOVER-opencode-2026-06-13.md` (line 18)
- **Problem:** Stated "a multi-tenant SaaS business management platform". ADR-41 explicitly defines Mybizz CS as dependency-based, not multi-tenant.
- **Fix:** Changed to "a dependency-based SaaS business management platform"

#### C5. Table count inconsistency across documents
- **Files:** `docs/platform-overview.md` (lines 41, 230), `implementation/build-plan.md` (lines 6, 16)
- **Problem:** platform-overview.md cited "36 Data Tables" in two places; build-plan.md cited "38 tables". Authoritative count is 43 (per `docs/authoritative-schema.md`).
- **Fix:** Updated platform-overview.md to "43 Data Tables" in both locations. Updated build-plan.md to point to `docs/authoritative-schema.md` instead of citing a number.

---

### HIGH (5)

#### H1. Hardcoded ADR count in build-plan.md
- **File:** `implementation/build-plan.md` (line 19)
- **Problem:** Stated "23 ADRs written". Actual count is 39.
- **Fix:** Changed to "See docs/adr-index.md for current ADR count and status"

#### H2. Superseded ADR convention inconsistent
- **Files:** `adr/12-consolidated-build-sequence.md`, `adr/13-system-currency-setting.md`, `adr/25-cancelled-onboarding-finality.md`
- **Problem:** Superseded/cancelled status was annotated in multiple places (title, frontmatter, body). Convention: `[Superseded]` must be on line 2 only.
- **Fix:** Removed `[SUPERSEDED]`/`[CANCELLED]` from titles. Added `[Superseded]` or `[Cancelled]` on line 2. Removed Status and "Superseded by" lines from frontmatter. Removed `superseded` prefix from filenames.

#### H3. ADR-29 status said "pending"
- **File:** `adr/29-dependency-update-model.md` (line 4)
- **Problem:** Status read "Confirmed — branch propagation test pending". The dependency model has been tested and confirmed.
- **Fix:** Changed to "Confirmed"

#### H4. Multiple stale ADR counts existed
- **Problem:** build-plan.md, sanity-audit-final-report.md, and other files each had their own ADR counts, all different.
- **Fix:** Removed all hardcoded counts. `docs/adr-index.md` is now the single source of truth.

#### H5. Gstack-outputs contained obsolete and historical files
- **Problem:** 18 files in gstack-outputs were session prompts, historical reports, or replaced artifacts.
- **Fix:** 6 obsolete files moved to `Obsolete3/`. 12 historical reports moved to `gstack-outputs/historical/`. 4 current/relevant files remain in `gstack-outputs/`.

---

### MEDIUM (5)

#### M1. platform-overview.md referenced ADR-25 instead of ADR-26
- **File:** `docs/platform-overview.md` (lines 66, 68)
- **Problem:** Referenced `25-onboarding-finality.md` (the cancelled ADR) instead of `26-onboarding-finality.md` (the current decision).
- **Fix:** Updated both references to `26-onboarding-finality.md`

#### M2. build-plan.md table count stale
- **File:** `implementation/build-plan.md` (line 6)
- **Problem:** Cited "38 tables". Authoritative count is 43.
- **Fix:** Changed to "See docs/authoritative-schema.md for current table count"

#### M3. ADR filename convention inconsistency
- **Files:** 7 ADR files had leading zeros (`01-`, `02-`, etc.)
- **Problem:** Naming convention changed over time. Newer ADRs (28+) use no leading zeros. Older ones had `01-`, `02-`, etc.
- **Fix:** Renamed all 7 files to remove leading zeros. Updated all cross-references across 20+ files.

#### M3b. Superseded ADR filename prefix
- **Files:** `12-superseded-consolidated-build-sequence.md`, `13-superseded-system-currency-setting.md`
- **Problem:** Filenames contained `superseded` prefix. Convention is now `[Superseded]` on line 2.
- **Fix:** Renamed to `12-consolidated-build-sequence.md` and `13-system-currency-setting.md`.

#### M5. Cross-reference updates required
- **Files:** 20+ files across `adr/`, `docs/`, `implementation/`, `workspace/`
- **Problem:** Old ADR filename conventions (`01-`, `02-`, `002-`, `003-`) were referenced throughout.
- **Fix:** All active-file cross-references updated to current convention.

---

### LOW (3) — Noted, No Action Required

#### L1. ADR-07 and ADR-08 permanently skipped
- **Status:** Documented in `docs/adr-index.md`. Intentional.

#### L2. Historical files in Obsolete3/ and historical/ retain old conventions
- **Status:** Acceptable — these are archived records, not active references.

#### L3. `gstack-outputs/dev-plan.md` classification
- **Status:** Kept in place. Not moved — user directive: "Assumptions are not permitted."

---

## 3. Files Renamed

| Old Name | New Name |
|---|---|
| `01-brevo-replaces-zoho-email.md` | `1-brevo-replaces-zoho-email.md` |
| `02-navigation-lambda-link-open-form.md` | `2-navigation-lambda-link-open-form.md` |
| `03-payment-security-boundary-vault.md` | `3-payment-security-boundary-vault.md` |
| `04-lead-capture-simultaneous-creation.md` | `4-lead-capture-simultaneous-creation.md` |
| `05-timezone-utc-storage-display-conversion.md` | `5-timezone-utc-storage-display-conversion.md` |
| `06-crm-package-replaces-marketing.md` | `6-crm-package-replaces-marketing.md` |
| `09-multi-vertical-to-single-vertical-conversion.md` | `9-multi-vertical-to-single-vertical-conversion.md` |
| `12-superseded-consolidated-build-sequence.md` | `12-consolidated-build-sequence.md` |
| `13-superseded-system-currency-setting.md` | `13-system-currency-setting.md` |

---

## 4. Files Moved

### To `Obsolete3/` (6 files — redundant/replaced session prompts)
- `sanity-audit-final-report.md` — replaced by this audit
- `ceo-review-prompt-sanity-audit.md` — session prompt, used
- `plan-eng-review-prompt.md` — session prompt, used
- `build-batch-1-prompt.md` — session prompt, used
- `build-batch-2-anvil-spec.md` — session prompt, used
- `old-office-hours-handfoff.md` — historical, misspelled filename

### To `gstack-outputs/historical/` (12 files — session reports)
- `ceo-review-report.md`
- `ceo-review-fix-plan.md`
- `eng-review-alignment-report.md`
- `engineering-review-report-phase-1.md` through `phase-5.md`
- `plan-eng-review-report.md`
- `config-audit-final.md`
- `config-verification-tests.md`
- `gbrain-assessment.md`

### Remaining in `gstack-outputs/` (4 files — current/relevant)
- `design-reference-screens.html` — visual reference
- `dev-plan.md` — session planning sequence
- `revised-anvil-worklist.md` — has DONE markers, useful reference
- `scaffold-validation-report.md` — reference record

---

## 5. Cross-References Updated

All internal ADR cross-references updated across:

| Directory | Files Updated | Changes |
|---|---|---|
| `adr/` | 10 | Old filename refs → current convention |
| `docs/` | 12 | Old filename refs → current convention |
| `docs/adr-index.md` | 1 | Full filename table + notes section |
| `implementation/` | 2 | devlog.md, onboarding-implementation-plan.md |
| `workspace/` | 1 | gbrain-operating-guide.md |

---

## 6. Authoritative Sources Established

| Concern | Single Source of Truth |
|---|---|
| ADR count and status | `docs/adr-index.md` |
| Database table count | `docs/authoritative-schema.md` |
| Architecture terminology | `adr/41-dependency-based-not-multi-tenant.md` |
| Onboarding decision | `adr/26-onboarding-finality.md` |
| Client instance model | `adr/28-client-instance-architecture.md` |

No other file should cite hardcoded counts for ADRs or tables. All references should point to the authoritative source.

---

## 7. Conventions Established

### ADR Naming
- Format: `{number}-{kebab-case-name}.md`
- No leading zeros: `1-`, `2-`, ..., `10-`, `11-`, etc.
- No prefix annotations in filenames

### Superseded/Cancelled ADRs
- Line 1: Title (no status annotation)
- Line 2: `[Superseded]` or `[Cancelled]`
- Nowhere else in the file should "superseded" or "cancelled" appear as status

### Counts
- No file should hardcode ADR counts or table counts
- All references point to `docs/adr-index.md` and `docs/authoritative-schema.md`

---

*End of report*
