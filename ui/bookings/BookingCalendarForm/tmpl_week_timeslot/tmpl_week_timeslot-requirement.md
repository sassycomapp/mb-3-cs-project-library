---
title: "tmpl_week_timeslot-requirement"
status: draft
doc-id: "mb-82fabeb1"
updated: "2026-11-25"
---
# tmpl_week_timeslot — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `bookings.BookingCalendarForm.tmpl_week_timeslot` |
| Purpose | One time row label in the week view's time column (ticket 24, F-11; spec-calendar-feature section 4.2) |
| Screen type and width | Card in a grid — 320 px, embedded |
| Layout | Item template of the week view's `rp_time_labels` |
| Route | None — an item template |

## 2. Elements

Per `spec-calendar-feature.md` section 4.2:

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Time label | `m3.Heading` (`hdg_time`) | The time label for one row, e.g. 07:00 | `headline-small` | — | — |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The slot's time | the period's items | the time rows the controller works out from the week's earliest start to latest end |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| None | — | — | The template is a label |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the label always shows its time |
| Heading | None — not applicable |
| Body text | None — not applicable |
| Primary action | None — not applicable |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

Same as the calendar.

## 7. Text

| Phrase key | English text |
|---|---|
| (none) | The label shows the slot's time |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The calendar controller's video covers the views |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-11 Admin daily operations | The week view's time rows |
