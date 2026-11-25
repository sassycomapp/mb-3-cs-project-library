---
title: "LandingPage-requirement"
status: draft
doc-id: "mb-4aa32f2d"
updated: "2026-11-25"
---
# LandingPage — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `landing_pages.LandingPage` |
| Purpose | The public landing page at `/landing/{slug}`, on the blank layout with no header or footer; its content is edited in edit mode (ticket 30, F-08; UF-18, UF-20) |
| Screen type and width | Public page — 900 px, full width: content sections |
| Layout | `layouts.BlankLayout` (DESIGN.md F-08: on a blank layout with no header or footer) |
| Route | `/landing/{slug}` — Anvil Routing (`@router.route`); the form reads its parameters from `routing_context.params` and sets title, description and Open Graph tags in `Route.meta` (spec-sm-syndication section 5; ticket 30) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page container | Anvil `ColumnPanel` (`col_landing`) | The page's one content column | — | mobile | — |
| Template host | Anvil `ColumnPanel` (`col_template`) | Holds the local template chosen by `landing_pages.template_type` | — | mobile | — |
| Parallax hero | `components.ParallaxComponent` | The landing page's hero (custom-component-requirements-matrix, ParallaxComponent: HomePage hero, About, Services, landing pages) | — | mobile | — |
| Content sections | `m3.Heading` + `m3.Text` in `m3.Card` sections | The template's headline, benefits, image or video, capture form (UF-20 step 2) | `headline-small`, `body-medium`, `surface-card` | mobile | the template's own texts, client content |
| Video section | `components.VideoDisplayComponent` | The VSL template's video; consent-gated | — | mobile | — |
| Capture form panel | `m3.Card` (`card_capture`, elevated) | The capture form's fields | `elevated-card` | mobile | — |
| Capture fields | `m3.TextBox` (`txt_lead_name`, type `email` `txt_lead_email`, type `tel` `txt_lead_phone`) | The form fields from `lead_captures.form_fields` | `outlined-input` | mobile | the capture's own labels (client content) |
| Submit button | `m3.Button` (`btn_submit_lead`, filled) | The capture's primary action; the Privacy Policy is linked beside it (F-22: the lead-capture forms link to the Privacy Policy beside the submit button) | `primary-button` | — | the capture's own submit label |
| Privacy Policy link | `m3.Link` (`lnk_lead_privacy`) | Beside the submit button | `tertiary-button` | — | `consent.terms.link_privacy` |
| Thank-you message | `m3.Text` (`text_lead_thanks`, hidden) | "A thank-you message is shown" after submission (UF-20 step 5) | `body-medium` | — | `landing.thanks` |
| Edit mode host | `components.EditModeComponent` | Placed once on every landing page template; visible only to a signed-in Owner or Manager (ticket 11) | — | — | `editmode.button` |
| Cookie banner | the layout's banner | The blank layout carries no banner; the public pages' banner does not host here — the landing page shows placeholders for embedded content until consent (pol-legal-documents-and-consent) | — | — | — |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The page's content | `landing_pages` | `config` (the template content), `title`, `template_type` |
| The capture's fields | `lead_captures` | `form_fields`, `offer_text` |
| The submitted lead | `leads` | `email`, `name`, `phone`, `source`, `landing_page_id`, `capture_id`, `converted_to_contact_id`, `status`, `captured_at` |
| The contact | `contacts` | created together with the lead in one step (UF-20 step 4) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the page | the route handler | `landing_page_service` public read (`server_marketing/landing_page_service`) | The page's content arrives; the view is counted; `Route.meta` sets the share tags |
| Submit the capture | `btn_submit_lead_click` | the public, rate-limited capture write (ticket 31's `lead_capture_service`) | The email is validated; the `leads` and `contacts` records are created together with `converted_to_contact_id` set at once; the conversion count is incremented; the contact is enrolled in the welcome sequence; the thank-you message shows (UF-20 step 4) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — a published landing page always shows its content |
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

The page is public (UF-20); the capture write is a public, rate-limited endpoint (ticket 31, B8).

## 7. Text

| Phrase key | English text |
|---|---|
| `landing.thanks` | Thank you! Check your email — we've sent you what you asked for. |
| `landing.capture.error_email` | That email address doesn't look right. |

*(The remaining texts are the client's own content, not phrase keys.)*


## 8. Help Videos

| Process | Help video |
|---|---|
| None | The public page is the visitor's; the editor's video covers the set-up |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-20 Lead capture via landing page | The whole flow: arrive, view, submit, thank-you, welcome email |
| UF-18 Landing page creation | The published page and its preview |
| UF-16 Content management and edit mode | The Owner edits the page in edit mode |
