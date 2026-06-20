# Onboarding Wireframes Report — ADR-27 Compliance

**Date:** 2026-06-19
**Package:** onboarding/ [Phase 1]
**Authority:** ADR-027, scaffold-spec.md, nomenclature.md, m3_component_mapping.md, DESIGN.md

---

## 1. Wireframes Created

### 1.1 OnboardingForm

- **File:** `wireframes/onboarding/OnboardingForm/wireframe-onboarding-OnboardingForm.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md line 85–87 (`OnboardingForm/` in `onboarding/` package, Phase 1)
  - `Clearly defined:` DESIGN.md OnboardingForm section: 6-step wizard, resumability, revisitable, system currency immutability, legal acknowledgement
  - `Clearly defined:` ADR-016 (System Currency), ADR-018 (Legal Policy), ADR-023 (Onboarding Resumability), ADR-025 (Onboarding Finality)
- **Form type:** Onboarding wizard — 6-step flow with progress indicator
- **Components:** ColumnPanel (root), Heading, Text, Button (multiple), DropdownMenu, CheckBox, Link, ColumnPanel (info/warning/success boxes), custom CSS progress bar
- **Empty state:** N/A — onboarding wizard, not data-driven
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

---

## 2. Forms/Sub-Templates Skipped

**None.** The onboarding package contains only OnboardingForm.

---

## 3. Prior Existing Wireframe Diagnostic

### 3.1 OnboardingForm (old version)

**File:** `wireframes/onboarding/OnboardingForm/wireframe-onboarding-OnboardingForm.html`

**Violations found:**
1. **Section structure** — Had 8+ sections instead of required 3. ADR-27 Section 2 requires exactly three sections: Introduction, Wireframe Diagram, Properties Table.
2. **Explanatory text in wireframe** — Contained "Settings Navigation (Bidirectional)", "Behaviour Notes", "Data State Transitions", and "ADR & Document References" sections. ADR-27 Section 4.3 prohibits behaviour, data, layout logic, or explanatory text in the wireframe.
3. **Document references** — Implementation plan references belong in specification documents, not in wireframes.

**Resolution:** Wireframe recreated with only three sections. Explanatory content removed per ADR-27 Section 4.3.

---

## 4. Summary

| Item | Status |
|------|--------|
| Wireframes created | 1 (OnboardingForm) |
| Forms/sub-templates skipped | 0 |
| ADR-27 compliance | All 15 checklist items PASS |
| Specification gaps | None |
| Prior wireframes replaced | 1 (non-compliant) |

---

*End of report — 2026-06-19*
