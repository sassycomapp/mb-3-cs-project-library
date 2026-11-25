---
title: "MyBookingsForm-requirement"
status: draft
doc-id: "mb-2588bf4d"
updated: "2026-11-25"
---
# MyBookingsForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `portal.MyBookingsForm` |
| Purpose | The customer's bookings: view, reschedule or cancel within the allowed window; book again (ticket 39, F-24; UF-29 step 3) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.CustomerLayout` |
| Route | None — opened by the portal routing |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_my_bookings`) | "My Bookings", `headline-large` | `page-title` | — | `portal.nav.bookings` |
| Book again button | `m3.Button` (`btn_my_book_again`, filled) | The primary action | `primary-button` | — | `portal.book_again` |
| Bookings grid | Anvil `DataGrid` (`dg_my_bookings`) | Service, date, time, status, price | — | mobile | — |
| Booking row | Anvil `DataRowPanel` (`drp_my_booking_*`) | One booking per row | — | — | — |
| Status badge | `m3.Chip` (`chip_my_booking_status_*`, type `input`, removable=False) | The booking's status | — | — | — |
| Row actions | `m3.IconButton` (`ibtn_reschedule_*`, `ibtn_cancel_my_booking_*`) | Reschedule and cancel within the allowed window (`business_profile.cancellation_window_hours`) | `icon-button` | — | `bookings.reschedule`, `bookings.row.cancel` |
| Help icon | `components.HelpTooltipComponent` (help entry `portal.bookings`) | The feature's tooltip and help video | — | — | `help.portal.bookings.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Bookings | `bookings` | the signed-in customer's rows via `get_my_bookings` (own only) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the bookings | `form_show` → `load_my_bookings` | `get_my_bookings` (`server_bookings/booking_service`; own only) | The grid arrives |
| Reschedule | `ibtn_reschedule_*_click` | the booking reschedule path (customer side) | The booking moves within the window |
| Cancel | `ibtn_cancel_my_booking_*_click` | the booking cancellation path | The booking is cancelled; a package session is returned where the window allows (UF-10 step 4) |
| Book again | `btn_my_book_again_click` | None | The booking flow opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Calendar icon (`mi:event`) |
| Heading | `portal.bookings.empty.title` |
| Body text | `portal.bookings.empty.body` |
| Primary action | `portal.book_again` |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | none |
| Customer | all |
| SignedOut | none |

Per the access matrix.

## 7. Text

| Phrase key | English text |
|---|---|
| `portal.bookings.empty.title` | No bookings yet |
| `portal.bookings.empty.body` | Book your next appointment to see it here. |
| `portal.bookings.cancel_window` | You can cancel up to {hours} hours before. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Managing your bookings | `master_template` help entry `portal.bookings` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-29 Customer portal | My Bookings |
| UF-10 Buying a session package | The cancel-within-window session return |
