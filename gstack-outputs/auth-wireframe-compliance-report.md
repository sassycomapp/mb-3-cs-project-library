# Auth Package Wireframe Compliance Report

**Date:** 2026-06-18
**Package:** auth/ [Phase 1]
**Authority:** ADR-027, scaffold-spec.md, nomenclature.md, m3_component_mapping.md, DESIGN.md

---

## 1. Wireframes Created

### 1.1 LoginForm
- **File:** `wireframes/auth/LoginForm/wireframe-auth-LoginForm.html`
- **Architectural backing:** scaffold-spec.md line 72 (`auth/LoginForm/` [Phase 1]), startup form per scaffold-spec.md line 1176, architecture.md section 7
- **ADR-27 Section 7 compliance:** PASS (all 15 items)

### 1.2 SignupForm
- **File:** `wireframes/auth/SignupForm/wireframe-auth-SignupForm.html`
- **Architectural backing:** scaffold-spec.md line 75 (`auth/SignupForm/` [Phase 1])
- **ADR-27 Section 7 compliance:** PASS (all 15 items)

### 1.3 PasswordResetForm
- **File:** `wireframes/auth/PasswordResetForm/wireframe-auth-PasswordResetForm.html`
- **Architectural backing:** scaffold-spec.md line 78 (`auth/PasswordResetForm/` [Phase 1])
- **ADR-27 Section 7 compliance:** PASS (all 15 items)

### 1.4 PasswordResetCompleteForm
- **File:** `wireframes/auth/PasswordResetCompleteForm/wireframe-auth-PasswordResetCompleteForm.html`
- **Architectural backing:** scaffold-spec.md line 81 (`auth/PasswordResetCompleteForm/` [Phase 1])
- **ADR-27 Section 7 compliance:** PASS (all 15 items)

---

## 2. ADR-27 Section 7 Compliance Checklist (per wireframe)

All 4 wireframes pass every item:

| # | Checklist Item | LoginForm | SignupForm | PasswordResetForm | PasswordResetCompleteForm |
|---|---|---|---|---|---|
| 1 | Section 0 reading completed | ✅ | ✅ | ✅ | ✅ |
| 2 | Exactly three sections, correct order | ✅ | ✅ | ✅ | ✅ |
| 3 | NOT formatted as "Screen" type | ✅ | ✅ | ✅ | ✅ |
| 4 | No legacy `Label` (`lbl_`) | ✅ | ✅ | ✅ | ✅ |
| 5 | All components approved M3 | ✅ | ✅ | ✅ | ✅ |
| 6 | Every visible text has explicit component | ✅ | ✅ | ✅ | ✅ |
| 7 | No sample data | ✅ | ✅ | ✅ | ✅ |
| 8 | No behaviour/explanatory text in diagram | ✅ | ✅ | ✅ | ✅ |
| 9 | Containers drawn as containers with names inset | ✅ | ✅ | ✅ | ✅ |
| 10 | No border on non-bordered components | ✅ | ✅ | ✅ | ✅ |
| 11 | Properties table: Designer-only only | ✅ | ✅ | ✅ | ✅ |
| 12 | `writeback (W)` specified for all text inputs | ✅ | ✅ | ✅ | ✅ |
| 13 | All names follow nomenclature.md | ✅ | ✅ | ✅ | ✅ |
| 14 | Containers used for intended purpose | ✅ | ✅ | ✅ | ✅ |
| 15 | Wireframe matches upstream architecture | ✅ | ✅ | ✅ | ✅ |

---

## 3. Forms/Sub-Templates Skipped

**None.** All 4 auth forms are architecturally backed per scaffold-spec.md and have no sub-templates.

---

## 4. Empty State Assessment

Auth forms are single-entity editors (not data-driven lists/grids). Empty state is **not applicable** for any auth form. This is architecturally correct — auth forms present a fixed input structure, not dynamic data.

---

## 5. Specification Gaps

**None found.** All auth forms have clear architectural backing and design specifications across scaffold-spec.md, DESIGN.md, architecture.md, security-architecture.md, and platform-overview.md.

---

## 6. Key Design Decisions Applied

| Decision | Source | Applied |
|---|---|---|
| Auth forms use Custom Form with no layout wrapper | scaffold-spec.md M3 Component Standards | ✅ |
| Card (outlined) as centred container | scaffold-spec.md M3 Component Standards | ✅ |
| Outlined TextBox components | scaffold-spec.md M3 Component Standards | ✅ |
| Password type set in Designer (hide_text) | scaffold-spec.md M3 Requirements | ✅ |
| Filled Button for submit | scaffold-spec.md M3 Component Standards | ✅ |
| Link components for navigation | scaffold-spec.md M3 Component Standards | ✅ |
| No legacy Label — use Heading/Text | ADR-027 Section 3.2 | ✅ |
| Component prefixes from nomenclature.md | nomenclature.md Component Prefixes | ✅ |
| Writeback (W) on all text inputs | ADR-027 Section 6.2 | ✅ |

---

*End of report*
