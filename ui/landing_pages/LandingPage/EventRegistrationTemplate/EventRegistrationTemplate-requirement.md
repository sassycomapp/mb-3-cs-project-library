---
title: "EventRegistrationTemplate-requirement"
status: draft
doc-id: "mb-c6fc764c"
updated: "2026-11-25"
---
# EventRegistrationTemplate — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `landing_pages.LandingPage.EventRegistrationTemplate` |
| Purpose | The Event Registration landing page template: the group session's details and the registration form (ticket 30, F-08; UF-18; the glossary's Event Registration) |
| Screen type and width | Public page — 900 px (a section set of the landing page) |
| Layout | Hosted by `landing_pages.LandingPage` |
| Route | None — a local template of the landing page |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Hero | `components.ParallaxComponent` | The template's hero with the session's name | — | mobile | — |
| Headline | `m3.Heading` (`hdg_event_hero`) | The session's name and headline (client content) | `headline-large` | — | — |
| Details section | `m3.Card` (`card_event_details`, outlined) | The session's date, time, location or call link, and what attendees get | `surface-card` | mobile | — |
| Session picker | `m3.DropdownMenu` (`dd_event_session`) | The group session to register for (with places left) | `outlined-select` | — | `event.session` |
| Registration form | `m3.Card` (`card_registration_form`, elevated) | The registration fields from `lead_captures.form_fields` | `elevated-card` | mobile | — |
| Submit button | `m3.Button` (`btn_event_submit`, filled) | The registration's primary action | `primary-button` | — | the capture's own label |
| Privacy Policy link | `m3.Link` (`lnk_event_privacy`) | Beside the submit button (F-22) | `tertiary-button` | — | `consent.terms.link_privacy` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Content | `landing_pages` | `config` |
| The session | `group_sessions` | `start_datetime`, `place_limit`, `places_taken`, `location`, `call_link` |
| Registration fields | `lead_captures` | `form_fields` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Submit | `btn_event_submit_click` | the capture write (ticket 31) | The lead and contact are created together; the session's places are drawn by the booking that follows |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Group icon (`mi:groups`) |
| Heading | `event.full.title` |
| Body text | `event.full.body` — when the chosen session is full (UF-07 step 4: when a session is full, it shows as full and cannot be booked) |
| Primary action | None — the message says the session is full |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | all |
| SignedOut | all |

## 7. Text

| Phrase key | English text |
|---|---|
| `event.session` | Choose a session |
| `event.full.title` | This session is full |
| `event.full.body` | Every place is taken. Choose another session, or ask the business about the next one. |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The editor's video covers the set-up |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-07 Group session booking | The registration half on the public site |
| UF-18 Landing page creation | The template chosen in the editor |
| UF-20 Lead capture via landing page | The event-registration capture kind |
