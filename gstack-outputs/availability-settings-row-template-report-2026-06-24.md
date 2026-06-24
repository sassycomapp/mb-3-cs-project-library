# AvailabilitySettingsForm dg_blocked_dates Row Template — Production Report

**Date:** 2026-06-24
**Agent:** Build
**Authority:** ADR-027, authoritative-schema.md, build-plan.md, nomenclature.md, screen-bookings-AvailabilitySettingsForm.html

---

## 1. What Was Created

### AvailabilitySettingsFormRowTemplate

**File:** `wireframes/bookings/AvailabilitySettingsForm/AvailabilitySettingsFormRowTemplate/wireframe-bookings-AvailabilitySettingsFormRowTemplate.html`

**Purpose:** Row template for `dg_blocked_dates` DataGrid in AvailabilitySettingsForm. Defines the visual structure of each blocked date row.

**Architectural backing:**
- authoritative-schema.md: `availability_exceptions` table with fields: `exception_date`, `reason`, `is_available`
- build-plan.md Stage 2.2: Provider availability schedules, blocked dates
- screen-bookings-AvailabilitySettingsForm.html: Sample data showing Holiday and Personal type entries with delete action
- Parent wireframe line 163-164: `dg_blocked_dates` with columns: Date | Type | Reason | Actions

**Template structure:** DataRowPanel-based, with one component per DataGrid column slot.

**Component mapping (schema fields to wireframe columns):**

| Wireframe Column | Component | Schema Field | Notes |
|---|---|---|---|
| Date | `txt_exception_date` (Text) | `exception_date` | DD MMM YYYY format |
| Type | `txt_exception_type` (Text) | `reason` | Badge-style (Holiday/Personal/Closure/Other) |
| Reason | `txt_exception_reason` (Text) | `reason` | Descriptive text |
| Actions | `flow_actions` (FlowPanel) with `btn_delete` (IconButton) | N/A | Delete button only |

**File/folder naming:** No hyphens — PascalCase folder `AvailabilitySettingsFormRowTemplate`, file `wireframe-bookings-AvailabilitySettingsFormRowTemplate.html`. Follows Anvil naming convention (no hyphens in file or component names).

**No "Programmatic behaviour" section** — per ADR-27 Section 4.3, behavioral text belongs in spec documents, not wireframes.

---

## 2. ADR-27 Section 7 Compliance Checklist

| # | Checklist Item | Result |
|---|---|---|
| 1 | Section 0 reading completed | PASS |
| 2 | Document contains exactly three sections, in the correct order | PASS — Introduction, Wireframe Diagram, Properties Table |
| 3 | Document is NOT formatted as a "Screen" type document | PASS |
| 4 | No legacy `Label` (`lbl_`) component used anywhere | PASS |
| 5 | Every component is an approved M3 component | PASS — Text, IconButton, DataRowPanel, FlowPanel all approved |
| 6 | Every visible text element has its own explicit component | PASS |
| 7 | No sample data appears anywhere | PASS — placeholders used ([DD MMM YYYY], [reason text]) |
| 8 | No behaviour, data, layout logic, or explanatory text in wireframe diagram | PASS |
| 9 | Containers drawn as containers with names inset in the border | PASS |
| 10 | No border drawn around non-bordered component types | PASS |
| 11 | Properties table contains ONLY Designer-only properties | PASS |
| 12 | `writeback (W)` specified where needed | N/A — row template has no text input components |
| 13 | All component names follow `nomenclature.md` exactly | PASS |
| 14 | FlowPanel/ColumnPanel/DataRowPanel used only for intended structural purpose | PASS |
| 15 | Wireframe architecturally matches upstream | PASS — matches availability_exceptions schema, screen structure, build-plan Stage 2.2 |

**Result: ALL 15 items PASS**

---

## 3. Findings

### Parent wireframe reference update needed (noted, not edited)

The parent wireframe `wireframe-bookings-AvailabilitySettingsForm.html` line 163-164 defines:
```
DataGrid: dg_blocked_dates
[Columns: Date | Type | Reason | Actions]
```

This does not reference the row template by name. Consider adding an annotation like `[AvailabilitySettingsFormRowTemplate — DataRowPanel-based row template]` to the dg_blocked_dates component description in a follow-up pass.

### Parent wireframe uses hyphenated folder convention (noted)

The existing row template folders in this project use hyphens (e.g., `CommentModerationForm-RowTemplate/`, `BookingListForm-RowTemplate/`). This wireframe uses PascalCase without hyphens per the user's instruction that Anvil does not accept hyphens in file or component names. The existing hyphenated folders may need renaming in a follow-up pass.

### Screen already has sample data

The existing screen (`screen-bookings-AvailabilitySettingsForm.html` lines 304-336) already renders the DataGrid with 4 sample rows. The screen was not modified in this pass.

---

## 4. Summary

| Item | Status |
|---|---|
| Wireframe created | `wireframes/bookings/AvailabilitySettingsForm/AvailabilitySettingsFormRowTemplate/wireframe-bookings-AvailabilitySettingsFormRowTemplate.html` |
| Template type | DataRowPanel-based (correct for DataGrid row template) |
| Column coverage | 4 columns: Date, Type, Reason, Actions |
| Schema alignment | All fields from availability_exceptions schema mapped |
| Action buttons | 1 (delete) — matches screen implementation |
| ADR-27 compliance | ALL 15 items PASS |
| Parent wireframe reference | Noted as follow-up (not edited in this pass) |

---

*End of report*
