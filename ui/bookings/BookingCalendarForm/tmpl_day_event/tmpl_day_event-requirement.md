---
title: "tmpl_day_event-requirement"
status: draft
doc-id: "mb-c95ee6e2"
updated: "2026-11-25"
---
# tmpl_day_event — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `bookings.BookingCalendarForm.tmpl_day_event` |
| Purpose | One booking in the day view (ticket 24, F-11; spec-calendar-feature section 4.3) |
| Screen type and width | Card in a grid — 320 px, embedded |
| Layout | Item template of the day view's `rp_day_events` |
| Route | None — an item template |

## 2. Elements

Per `spec-calendar-feature.md` section 4.3:

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Event card | `m3.InteractiveCard` (`card_event`, outlined) | The spec's booking card; the status colour as its accent bar, the status word in the card | `surface-card` | mobile | — |
| Event title | `m3.Heading` (`hdg_event_title`) | Customer name, or session name with places taken | `headline-small` | — | — |
| Event time | `m3.Text` (`text_event_time`) | Start–end time; for a visit, the travel time before and after is shown as well | `body-small` | — | — |
| Event detail | `m3.Text` (`text_event_detail`) | Service, provider, meeting type, address or resource, payment status (paid, deposit paid, pay on the day, invoiced) | `body-small` | — | — |
| Event notes | `m3.Text` (`text_event_notes`) | The customer's notes or special requests, shortened to two lines | `body-small` | — | — |
| Blocked card variant | the same card with the blocked token | A blocked period appears in the list as a card with the blocked token and the reason (spec-calendar section 4.3) | — | mobile | — |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The event's item | the period's items | the calendar item's structure: status, start, end, travel_before/after, title, service_name, provider_name, meeting_type, address, resource_name, payment_status, notes |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Open the booking | the card's click | None | `bookings.BookingViewerForm` opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the card always shows its booking or block |
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
| `bookings.payment.paid` | Paid |
| `bookings.payment.deposit_paid` | Deposit paid |
| `bookings.payment.pay_on_day` | Pay on the day |
| `bookings.payment.invoiced` | Invoiced |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The calendar controller's video covers the views |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-11 Admin daily operations | The day view's bookings and blocked periods |
