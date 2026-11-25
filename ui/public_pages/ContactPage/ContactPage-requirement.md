---
title: "ContactPage-requirement"
status: draft
doc-id: "mb-ab9e43aa"
updated: "2026-11-25"
---
# ContactPage — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `public_pages.ContactPage` |
| Purpose | The public Contact page: the enquiry form into the CRM, the business's details and the optional Google Maps embed (ticket 33, F-06; UF-19) |
| Screen type and width | Public page — 900 px, full width: content sections |
| Layout | `layouts.PublicLayout` |
| Route | `/contact` — Anvil Routing; `Route.meta` sets the share tags |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page container | Anvil `ColumnPanel` (`col_contact`) | The page's one content column | — | mobile | — |
| Details section | `m3.Card` (`card_contact_details`, outlined) | The business's contact email and phone, from `business_profile` | `surface-card` | mobile | — |
| Map section | Anvil `ColumnPanel` (`col_contact_map`) | The Google Maps embed; the map loads only after the visitor accepts cookies (ticket 05's placeholder pattern) | — | mobile | — |
| Map placeholder | `m3.Card` (`card_map_consent`, outlined) with `m3.Button` (`btn_map_load`, filled) | Before consent: names Google Maps with a Load button | `surface-card` | mobile | `footer.map.placeholder` |
| Enquiry form panel | `m3.Card` (`card_enquiry_form`, elevated) | The contact form's fields | `elevated-card` | mobile | — |
| Name field | `m3.TextBox` (`txt_enquiry_name`, outlined) | Required (UF-19 fields) | `outlined-input` | — | `enquiry.name` |
| Email field | `m3.TextBox` (`txt_enquiry_email`, type `email`, outlined) | Required | `outlined-input` | — | `booking.flow.email` |
| Phone field | `m3.TextBox` (`txt_enquiry_phone`, type `tel`, outlined) | Optional | `outlined-input` | — | `booking.flow.phone` |
| Message field | `m3.TextArea` (`txt_enquiry_message`, outlined, `character_limit=1000`) | Required, up to 1,000 characters (UF-19) | `outlined-input` | — | `enquiry.message` |
| Submit button | `m3.Button` (`btn_send_enquiry`, filled) | The form's primary action; the Privacy Policy is linked beside it (F-22) | `primary-button` | — | `enquiry.send` |
| Privacy Policy link | `m3.Link` (`lnk_enquiry_privacy`) | Beside the submit button | `tertiary-button` | — | `consent.terms.link_privacy` |
| Thank-you message | `m3.Text` (`text_enquiry_thanks`, hidden) | The form resets and shows a thank-you message (UF-19 step 5) | `body-medium` | — | `enquiry.thanks` |
| Error text | `m3.Text` (`text_enquiry_error`, hidden) | Says what to fix (spec-ui-standards section 13.5) | `body-small` | — | `enquiry.error` |
| Footer | `components.FooterComponent` | The shared footer | — | — | — |
| Edit mode host | `components.EditModeComponent` | Placed once on the page (ticket 11) | — | — | `editmode.button` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The submission | `contact_submissions` | `name`, `email`, `phone`, `message`, `status` (`new`), `submitted_date` |
| The contact | `contacts` | created or updated by the same email (UF-19 step 2) |
| The enquiry event | `contact_events` | `event_type` (`enquiry`) on the contact's timeline |
| The follow-up task | `tasks` | created by the submission handling (UF-19 step 3) |
| Page content | `pages` | `components` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Submit the enquiry | `btn_send_enquiry_click` | the public contact-form write (ticket 33: one bounded, rate-limited public write) | Saves the submission, creates or updates the contact, adds the enquiry to the timeline, creates the follow-up task, notifies the Owner and sends the auto-reply if configured (UF-19 steps 1–4); the form resets and shows the thank-you |
| Load the map | `btn_map_load_click` | `record_consent` (`server_settings/policy_service`) | The choice is recorded; the embed loads |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the page renders its content and form |
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
| Customer | all |
| SignedOut | all |

The form is a public, rate-limited write (10/min unauthenticated per IP; ticket 33, B8).

## 7. Text

| Phrase key | English text |
|---|---|
| `enquiry.name` | Name |
| `enquiry.message` | Your message |
| `enquiry.send` | Send message |
| `enquiry.thanks` | Thank you! We've got your message and will be in touch. |
| `enquiry.error.name` | Tell us your name. |
| `enquiry.error.email` | That email address doesn't look right. |
| `enquiry.error.message` | Write your message (up to 1,000 characters). |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The public form explains itself |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-19 Contact form submission | The whole flow |
| UF-21 Contact management | The contact created or updated here |
| UF-26 Tasks and automation | The follow-up task created here |
