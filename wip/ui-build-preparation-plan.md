---
title: "ui-build-preparation-plan"
doc-id: "mb-deb563c7"
updated: "2026-10-01"
---
# UI Build Preparation Plan

**Purpose:** Final preparation steps before constructing UIs in Anvil.works using Anvil AI.
**Current state:** Three instructional documents exist in `ui-build-prep/`. Theme CSS is updated. Phase 1-3 realignment tasks are complete.
**Overarching principles:** Anvil Material 3 Theme compliance + MUI aesthetic overlay.

---

## Critical Sequence

The build follows a strict top-down authority chain. Each layer constrains the next:

```
Planning & Architecture docs (adr/, docs/)
        ↓
Wireframes (wireframes/) — must comply with planning docs
        ↓
Screens (screens/) — must comply with wireframes only
```

**Screens must contain ONLY what the wireframe declares.** Current observation: screens contain left sidebar, app bar, and other page-chrome elements that are NOT part of the wireframe's declared structure. This violates the authority chain. Screens must be rebuilt to match their wireframe exactly — nothing added, nothing removed.

This sequence is non-negotiable. Do not skip ahead.

---

## Execution Order

### Step 1 — Cross-check instructional docs against project library

**Skill:** `/investigate`
**Objective:** Systematic four-phase investigation of all three instructional documents against the current state of project library docs and ADRs. The Iron Law applies: no fixes without root cause.

**Phase 1 — Investigate (read everything):**
- Read `anvil-ui-build-standard.md`, `anvil-ui-build-prompt-template.md`, `anvil-ui-exception-review-procedure.md` in full.
- Read all ADRs in `adr/` that the Standard references (ADR-43, 44, 45, 15, 27, 37).
- Read `design-direction.md`, `screen-production-standard.md`, `spec-component-properties.md`, `m3_component_mapping.md`, `nomenclature.md`, `DESIGN.md`.
- Read `theme_mui_patch.css` and confirm it matches the Standard's MUI overlay table.
- Use GBrain to search for all cross-references between these files.

**Phase 2 — Analyze (find every mismatch):**
- The Standard references `adr-register.md` which has been deleted. All ADR references must point to `adr/` folder.
- Confirm the Standard's approved component list (§6) matches `adr/15-material-3-theme-component-scope.md`.
- Confirm the Standard's `drp_` prefix note (§4) matches `nomenclature.md`.
- Confirm the Standard's InteractiveCard appearances (§6) match `spec-component-properties.md`.
- Confirm the Standard's HTMLTemplate ban (§1, §7) matches `adr/45-htmltemplate-use.md`.
- Confirm the Standard's `wrap_on` guidance (§11) matches `adr/44-responsive-behaviour-mechanism.md`.
- Confirm the Standard's role assignment guidance (§6.0) matches `adr/43-role-property-assignment-mechanism.md`.
- Confirm the Standard's max-width table (§11) matches `screen-production-standard.md` and `design-direction.md`.
- Confirm the Standard's MUI overlay table (§3) matches `theme_mui_patch.css`.
- Confirm the Standard's dark mode note (§8) matches `DESIGN.md`.
- Confirm the Template's bootstrap section matches the deployed `theme_patched.css`.
- Check the Exception Review Procedure's steps against the Standard's sections.

**Phase 3 — Hypothesize (determine which document is authoritative for each mismatch):**
- For each mismatch, determine which document is the source of truth (ADR? Standard? design-direction.md?).
- Flag any case where the authority is unclear.

**Phase 4 — Implement (produce the discrepancy report):**
- List every discrepancy found, with the authoritative source identified.
- Propose a correction for each.
- If no discrepancies found, state this explicitly.

**Deliverable:** A comprehensive discrepancy report saved to `opencode-outputs/ui-build-audit-report-YYYY-MM-DD.md`.

**Dependency:** None. This is the first step.

---

### Step 2 — Resolve discrepancies through Q&A

**Skill:** `grill-with-docs`
**Objective:** For each discrepancy found in Step 1, conduct a relentless interview to determine the correct resolution. Produce ADRs for any new decisions that emerge.

**Process:**
1. Present each discrepancy as a question with the proposed fix.
2. `grill-with-docs` interviews the user on each one, challenging assumptions and forcing decisions.
3. For any decision that creates a new architectural precedent, produce an ADR in `adr/`.
4. Apply approved corrections to the three instructional documents.
5. Update any project library docs that were identified as incorrect during the audit.

**Deliverable:**
- Updated instructional documents with all discrepancies resolved.
- New ADRs (if any) for decisions made during the interview.
- Updated project library docs (if any) that were corrected.

**Dependency:** Step 1 complete.

---

### Step 3 — Finalize instructional documentation

**Objective:** Ensure the three instructional documents are in perfect harmony with the project library docs after Step 2 corrections.

**What to verify:**
- The Standard's "Remaining Open Items" table reflects current state.
- The Template's `{{EXCEPTION_NOTES}}` guidance still makes sense given the Standard's current coverage.
- The Exception Review Procedure's steps still align with the Standard's sections.
- All file paths referenced in the three documents are correct and current.
- All ADR references point to `adr/` folder with correct numbers.

**Deliverable:** Finalized versions of all three documents. Version bump the Standard if changes were made.

**Dependency:** Step 2 complete.

---

### Step 4 — Refresh wireframes for full compliance

**Skill:** `/design-consultation`
**Objective:** Ensure all wireframes in `wireframes/` are compliant with the finalized Standard and the project library docs.

**What to check per wireframe:**
- ADR-27 Section 7 compliance (3-section structure, no legacy Label, approved components only, no sample data, no behaviour text, containers drawn correctly, properties table correct).
- HTMLTemplate eliminated (per ADR-45).
- InteractiveCard appearances valid (no `tonal`).
- `drp_` prefix on DataRowPanel components.
- `mi:` prefix on icon references.
- Navigation items use `NavigationLink` in layout wireframes.
- `wrap_on` annotations where applicable.
- Wireframe structure matches the architectural and planning documentation.

**Deliverable:** List of wireframes requiring fixes, with fixes applied. If all wireframes are already compliant, explicit confirmation.

**Dependency:** Step 3 complete (Standard must be finalized before auditing against it).

---

### Step 5 — Refresh screens for full compliance

**Skill:** `/design-html`
**Objective:** Ensure all screens in `screens/` match their corresponding wireframes exactly. Rebuild screens that contain page chrome (sidebar, app bar, footer) not declared in the wireframe.

**Critical rule:** The wireframe is the sole structural authority for the screen. The screen must contain ONLY the components declared inside the wireframe's outermost container. Page chrome (sidebar, app bar, notification icons, footer, sign-out) belongs to the app's shared layout, not to the screen. If a screen currently renders these elements, they must be removed.

**What to check per screen:**
- Component structure matches wireframe (no components added or removed).
- No page chrome outside the wireframe's declared boundary.
- MUI aesthetic overlay variables present (all 9).
- No hardcoded colours.
- Empty state present where applicable (4 components).
- Material Icons imported.
- Professional Blue palette tokens used correctly.

**Deliverable:** List of screens requiring fixes, with fixes applied. If all screens are already compliant, explicit confirmation. All screens must be produced in both HTML and PNG formats.


**Dependency:** Step 4 complete (wireframes must be correct before screens can be verified against them).

---

### Step 6 — Deploy Standard to Anvil Assets

**Objective:** Copy the finalized `anvil-ui-build-standard.md` into the Anvil app's Assets so Anvil AI can read it during builds.

**Action:** Manual file replacement in the Anvil Editor. Copy contents of `project-library/ui-build-prep/anvil-ui-build-standard.md` into `mb-3-cs/Assets/anvil-ui-build-standard.md`.

**Verification:** Open the Anvil Editor, navigate to Assets, confirm the file is present and its content matches the project library version.

**Deliverable:** Confirmation that the file is deployed and readable.

**Dependency:** Step 3 complete (Standard must be finalized before deploying).

---

### Step 7 — Prepare first Anvil AI build prompt

**Objective:** Produce the first per-UI prompt for Anvil AI, following the scaffold structure from top to bottom.

**Process:**
1. Read `docs/scaffold-spec.md` to identify the first form in the build sequence.
2. Locate the corresponding wireframe in `wireframes/`.
3. Run the Exception Review Procedure on the wireframe + screenshot pair.
4. Fill in the Prompt Template with: form name, package, wireframe HTML, and exception notes.
5. Present the completed prompt for user review before sending to Anvil AI.

**Deliverable:** A ready-to-send prompt for the first UI build.

**Dependency:** Steps 3, 4, 5, and 6 complete. The Standard must be deployed, wireframes and screens must be compliant.

---

## Dependency Map

```
Step 1 (/investigate — audit)
  └→ Step 2 (grill-with-docs — resolve discrepancies)
       └→ Step 3 (finalize docs)
            ├→ Step 4 (/design-consultation — refresh wireframes)
            │    └→ Step 5 (/design-html — refresh screens)
            ├→ Step 6 (deploy Standard to Anvil)
            └→ Step 7 (prepare first prompt) ← depends on 4, 5, 6
```

Steps 4, 5, and 6 can run in parallel after Step 3 completes. Step 7 requires all three to be done.

---

## Risk Notes

- **ADR reference drift:** The Standard was written when ADRs lived in `adr-register.md`. That file has been deleted. All references must be updated to the current `adr/` folder with correct ADR numbers (43, 44, 45).
- **DataRowPanel gap:** The Standard notes `drp_` "still needs formal addition to nomenclature.md" — this was already done. The note should be removed.
- **Screen page chrome contamination:** Current screens contain sidebar, app bar, and other elements not declared in their wireframes. This violates the authority chain. Screens must be stripped down to match wireframes exactly.
- **Screen/wireframe drift:** If screens were produced against older wireframes, they may need regeneration rather than patching. Step 5 should flag this case.
- **Screen format:** All Screens must be produced in html and in png formats
- **Skill scope note:** `/design-consultation` is documented as creating DESIGN.md (design system documentation). The user has indicated it also builds wireframes. Confirm this usage before proceeding with Step 4.

---

*End of UI Build Preparation Plan*
