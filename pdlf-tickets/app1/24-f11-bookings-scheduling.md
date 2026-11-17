---
title: "24 — F-11 Bookings and scheduling"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-3e14d048"
updated: "2026-11-17"
---

## Description

**Feature:** F-11 Bookings and scheduling (UF-05–UF-08, UF-11; `spec-calendar-feature.md` in full; DESIGN.md Booking Design; SR-15–SR-22; FR-11). Trade feature — the product's core.
**Depends on:** 15 (PayPalButtonComponent), 21 (services), 23 (payments), 09 (AI helper on the calendar), 20 (contacts).

### Forms

- `bookings.BookingCalendarForm` — the controller and its view forms and templates per `spec-calendar-feature.md` §3 (month, week, day; Today/previous/next; filter by provider or resource; refresh every minute)
- `bookings.BookingListForm`, `bookings.BookingFlowForm` (the customer flow, steps 1–7, three round trips ending in one `confirm_booking`), `bookings.BookingEditorForm`, `bookings.BookingViewerForm`, `bookings.AvailabilitySettingsForm` (business hours, provider schedules, blocked dates, exceptions)

### Server functions

- `server_bookings/booking_service` (renamed from `server_bookings/service.py` at this step, per the divergence inventory) — `confirm_booking` with the atomic re-check of slot, resource, places and price inside `@tables.in_transaction` (SR-16); the D5 late-payment reinstatement inside the same transaction; `get_booking_context`, `get_my_bookings`
- `server_bookings/availability_service` — `get_available_slots`; the slot calculation as a pure module beside it
- `server_bookings/calendar_service` — `get_calendar_items` (Staff: own)
- `server_bookings/intake_service` (renamed from `metadata_service.py` at this step)
- `server_bookings/booking_tasks` — reminders (24h, 1h), the 5-minute payment-hold expiry
- `server_bookings/time_service` — assessed at this build step: fold into `server_time/time_entry_service` or keep, decided here (divergence inventory)

### Tables

- `bookings`, `availability_rules`, `availability_exceptions`, `recurring_series`, `services`, `resources`, `group_sessions`, `booking_metadata_schemas`, `contacts`, `customer_packages`, `payments`

### Acceptance tests

- Test-plan §1 (E2E booking flow, 3-second gate, data checks), §2 (contention — exactly one succeeds), §3 (hold and expiry), §4 (D5 late-payment race with 23), §19 (lifecycle state machine)
- `IN-05`, `DM-01`, `SR-15`–`SR-22`, `BR-01`, `BR-06`; UF-05–UF-08, UF-11

### Security criteria

- B9 — every contended write in a transaction: confirm, hold expiry, places, draw-down (AC-S10)
- B1 — Staff see and change only their own bookings (`get_calendar_items` own; `writable_fields.bookings`)
- AC-S2 — customer functions derive the customer from the signed-in user, never a client-supplied id
- Group-session `places_taken` and package draw-down updated in the same call as each booking and cancellation

## Comments
