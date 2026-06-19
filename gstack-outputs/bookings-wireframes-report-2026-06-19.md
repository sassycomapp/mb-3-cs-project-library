# Bookings Wireframes Report — ADR-27 Compliance

**Date:** 2026-06-19
**Package:** bookings/ [Phase 2]
**Authority:** ADR-027, scaffold-spec.md, nomenclature.md, m3_component_mapping.md, DESIGN.md, build-plan.md, globals-contract.md, plan-design-review-2026-06-16.md

---

## 1. Wireframes Created

### 1.1 AvailabilitySettingsForm

- **File:** `wireframes/bookings/AvailabilitySettingsForm/wireframe-bookings-AvailabilitySettingsForm.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md lists `AvailabilitySettingsForm` in `bookings/` package [Phase 2]
  - `Clearly defined:` build-plan.md Stage 2.2: "Availability Management" with completion criteria
  - `Clearly defined:` DESIGN.md: not explicitly covered (spec gap flagged)
- **Form type:** Editor/settings form — weekly schedule configuration, booking rules, blocked dates
- **Components:** Page title, provider selector, weekly schedule card (7 day rows with Checkbox + DatePicker pairs), booking rules card (buffer time DropdownMenu, advance window DropdownMenu), blocked dates card (date picker, type dropdown, reason input, DataGrid, empty state)
- **Empty state:** 4 required components present: `ico_empty_state` (Icon "event_busy"), `hdg_empty_state` (Heading "No blocked dates"), `txt_empty_state` (Text), `btn_empty_action` (Button "Block a Date")
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.2 BookingCalendarForm

- **File:** `wireframes/bookings/BookingCalendarForm/wireframe-bookings-BookingCalendarForm.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md lists `BookingCalendarForm/` in `bookings/` package [Phase 2] with 4 sub-templates
  - `Clearly defined:` build-plan.md Stage 2.3: "Calendar views functional (month, week, day)"
  - `Clearly defined:` DESIGN.md Booking Calendar Architecture: 4 template subforms, event color coding, view toggle
- **Form type:** Calendar form — month/week/day views with SegmentedButton toggle
- **Components:** Header row (title, provider filter, add booking button, list view button), view controls (month/week/day buttons, prev/next/today navigation), month view panel (grid header + RepeatingPanel), week view panel (header + timeslots + events), day view panel (date heading + events RepeatingPanel), empty state
- **Empty state:** 4 required components present: `ico_empty_state` (Icon "event"), `hdg_empty_state` (Heading "No bookings yet"), `txt_empty_state` (Text), `btn_empty_action` (Button "Create Booking")
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.3 tmpl_day_event

- **File:** `wireframes/bookings/BookingCalendarForm/tmpl_day_event/wireframe-bookings-tmpl_day_event.html`
- **Architectural backing:**
  - `Clearly defined:` DESIGN.md Booking Calendar Architecture: "Day view (`tmpl_day_event`): Event card with 4px left border (color-coded by status), 8px border-radius, subtle shadow"
  - `Clearly defined:` scaffold-spec.md lists `tmpl_day_event/` under `BookingCalendarForm/`
- **Form type:** Template subform — event card for day view
- **Components:** InteractiveCard with event time, title, customer, meeting type, status text
- **Empty state:** Not applicable — template subform, not a standalone form
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.4 tmpl_month_day_cell

- **File:** `wireframes/bookings/BookingCalendarForm/tmpl_month_day_cell/wireframe-bookings-tmpl_month_day_cell.html`
- **Architectural backing:**
  - `Clearly defined:` DESIGN.md Booking Calendar Architecture: "Month view (`tmpl_month_day_cell`): Day cell in month grid. Today cell has `primary-container` background. Other-month cells have `opacity: 0.4`. Event chips are 9px font with status color coding."
  - `Clearly defined:` scaffold-spec.md lists `tmpl_month_day_cell/` under `BookingCalendarForm/`
- **Form type:** Template subform — day cell for month view grid
- **Components:** ColumnPanel with day number text + RepeatingPanel for event chips
- **Empty state:** Not applicable — template subform
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.5 tmpl_week_event

- **File:** `wireframes/bookings/BookingCalendarForm/tmpl_week_event/wireframe-bookings-tmpl_week_event.html`
- **Architectural backing:**
  - `Clearly defined:` DESIGN.md Booking Calendar Architecture: "Week view (`tmpl_week_event`): Event block with left border color coding (same palette as day view). Cancelled events have `opacity: 0.5`."
  - `Clearly defined:` scaffold-spec.md lists `tmpl_week_event/` under `BookingCalendarForm/`
- **Form type:** Template subform — event block for week view
- **Components:** InteractiveCard with event time, title, customer
- **Empty state:** Not applicable — template subform
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.6 tmpl_week_timeslot

- **File:** `wireframes/bookings/BookingCalendarForm/tmpl_week_timeslot/wireframe-bookings-tmpl_week_timeslot.html`
- **Architectural backing:**
  - `Clearly defined:` DESIGN.md Booking Calendar Architecture: "Week timeslot (`tmpl_week_timeslot`): Time row: 60px time label column + 1fr content. Empty timeslots show a thin spacer line."
  - `Clearly defined:` scaffold-spec.md lists `tmpl_week_timeslot/` under `BookingCalendarForm/`
- **Form type:** Template subform — time row for week view
- **Components:** FlowPanel with time label + content column (spacer + RepeatingPanel for events)
- **Empty state:** Not applicable — template subform
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.7 BookingEditorForm

- **File:** `wireframes/bookings/BookingEditorForm/wireframe-bookings-BookingEditorForm.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md lists `BookingEditorForm/` in `bookings/` package [Phase 2]
  - `Clearly defined:` build-plan.md Stage 2.3: "Booking creation flow working (service → provider → date/time → payment)"
  - `Clearly defined:` DESIGN.md 7-Step Booking Flow: 7 steps, 3 round-trips, per-step UI layout
  - `Clearly defined:` globals-contract.md: `booking_flow_state` dict with service, provider, date, time_slot, meeting_type, intake_data, cached_context
- **Form type:** Multi-step editor — 7 steps with progress indicator
- **Components:** Progress indicator, step label, 7 step panels (service selection with search + RepeatingPanel, provider selection with RepeatingPanel, date/time with DatePicker + time slot buttons, meeting type with RadioGroupPanel, intake form with dynamic fields, payment with order summary card, confirmation with success icon + details)
- **Empty state:** Not applicable — multi-step editor, not a data-driven list
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.8 BookingListForm

- **File:** `wireframes/bookings/BookingListForm/wireframe-bookings-BookingListForm.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md lists `BookingListForm/` in `bookings/` package [Phase 2]
  - `Clearly defined:` build-plan.md Stage 2.3: "Booking listing"
  - `Clearly defined:` DESIGN.md BookingListForm Columns: Booking #, Customer, Service, Date, Time, Status, Actions
- **Form type:** List form — DataGrid with search, filter, pagination
- **Components:** Page title, action bar (add booking filled button, calendar view outlined button), filter bar (search TextBox, status DropdownMenu, date range DropdownMenu), DataGrid (7 columns), pagination, empty state
- **Empty state:** 4 required components present: `ico_empty_state` (Icon "event"), `hdg_empty_state` (Heading "No bookings yet"), `txt_empty_state` (Text), `btn_empty_action` (Button "Create Booking")
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.9 BookingListForm-RowTemplate

- **File:** `wireframes/bookings/BookingListForm/BookingListForm-RowTemplate/wireframes-bookings-BookingListForm-RowTemplate.html`
- **Architectural backing:**
  - `Clearly defined:` DESIGN.md BookingListForm Columns: row structure with booking number, customer, service, date, time, status badge, action buttons
  - `Partially defined:` scaffold-spec.md does not explicitly list a RowTemplate sub-form for BookingListForm. DataGrids in Anvil render from column definitions. However, the task list explicitly includes this item.
- **Form type:** Row template — FlowPanel with 7 columns + action buttons
- **Components:** Booking number, customer name, service name, date, time, status badge, view/edit/delete IconButtons
- **Empty state:** Not applicable — row template, not a standalone form
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

### 1.10 BookingViewerForm

- **File:** `wireframes/bookings/BookingViewerForm/wireframe-bookings-BookingViewerForm.html`
- **Architectural backing:**
  - `Clearly defined:` scaffold-spec.md lists `BookingViewerForm/` in `bookings/` package [Phase 2] ★ NEW
  - `Clearly defined:` DESIGN.md Booking Design section: Status-based permitted actions table
  - `Clearly defined:` globals-contract.md: `current_booking` set by BookingListForm/BookingEditorForm, read by BookingViewerForm
- **Form type:** Viewer — read-only detail with status-dependent action buttons
- **Components:** Header row (title + status badge + back button), booking details card (service, provider, date, time, meeting type, booking type, price, payment status, special requests, created date), customer card (name, email, phone, view contact link), action buttons (edit, cancel, reschedule, view invoice — visibility controlled by status)
- **Empty state:** Not applicable — single-entity viewer. If `globals.current_booking` is None, redirect to BookingListForm.
- **ADR-27 Section 7 compliance:** ALL 15 items PASS

---

## 2. Forms/Sub-Templates Skipped

**None.** All 10 items in the task list received their own complete wireframe.

---

## 3. Specification Gaps

| Gap | Impact | Resolution |
|-----|--------|------------|
| AvailabilitySettingsForm not explicitly covered in DESIGN.md | Used build-plan.md Stage 2.2 + scaffold-spec.md as authority | Design derived from availability_rules and availability_exceptions table schemas |
| BookingEditorForm per-step UI layout not in DESIGN.md prior to design-remediation-report | DESIGN.md now includes 7-step layout per design-remediation-report-2026-06-16 | Used DESIGN.md as authority |
| BookingListForm-RowTemplate not explicitly listed in scaffold-spec.md sub-forms | DataGrid renders from column definitions; row template may be optional | Produced anyway per task list instruction (default to produce) |

---

## 4. Summary

| Item | Status |
|------|--------|
| Wireframes created | 10 (AvailabilitySettingsForm, BookingCalendarForm, tmpl_day_event, tmpl_month_day_cell, tmpl_week_event, tmpl_week_timeslot, BookingEditorForm, BookingListForm, BookingListForm-RowTemplate, BookingViewerForm) |
| Forms/sub-templates skipped | 0 |
| ADR-27 compliance | All 10 wireframes pass all 15 checklist items |
| Specification gaps | 3 (all non-blocking) |

---

*End of report — 2026-06-19*
