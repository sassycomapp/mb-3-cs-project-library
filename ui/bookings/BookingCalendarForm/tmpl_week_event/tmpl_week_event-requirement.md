---
title: "tmpl_week_event-requirement"
status: draft
doc-id: "mb-5bb95f7e"
updated: "2026-11-25"
---
# tmpl_week_event — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `bookings.BookingCalendarForm.tmpl_week_event` |
| Purpose | One booking in the week view (ticket 24, F-11; spec-calendar-feature section 4.2) |
| Screen type and width | Card in a grid — 320 px, embedded |
| Layout | Item template of the day column's `rp_day_events` |
| Route | None — an item template |

## 2. Elements

Per `spec-calendar-feature.md` section 4.2:

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Event card | `m3.InteractiveCard` (`card_event`, outlined) | The spec's booking card; the status colour as its accent bar on the card's left edge, the status word in the card (spec-calendar section 7.2) | `surface-card` | mobile | — |
| Event title | `m3.Heading` (`hdg_event_title`) | Customer name, or session name with places taken ("Yoga — 8/12") | `headline-small` | — | — |
| Event time | `m3.Text` (`text_event_time`) | Start–end time | `body-small` | — | — |
| Event detail | `m3.Text` (`text_event_detail`) | One of: the address for a visit at the customer's location, the resource name, "Video" or "Phone"; a recurring booking shows the repeat icon | `body-small` | — | `calendar.detail.video`, `calendar.detail.phone` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The event's item | the period's items | the calendar item's structure (spec-calendar section 7.2): status, start, end, title, meeting_type, address, resource_name, places_taken, is_recurring |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Open the booking | the card's click | None | `bookings.BookingViewerForm` opens (spec-calendar section 6) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the card always shows its booking |
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
| `calendar.detail.video` | Video |
| `calendar.detail.phone` | Phone |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The calendar controller's video covers the views |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-11 Admin daily operations | The week view's bookings |
