---
title: "VslTemplate-requirement"
status: draft
doc-id: "mb-95796126"
updated: "2026-11-25"
---
# VslTemplate — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `landing_pages.LandingPage.VslTemplate` |
| Purpose | The Video Sales Letter landing page template: the video up front, then the offer and the capture form (ticket 30, F-08; UF-18, UF-20) |
| Screen type and width | Public page — 900 px (a section set of the landing page) |
| Layout | Hosted by `landing_pages.LandingPage` |
| Route | None — a local template of the landing page |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Video section | `components.VideoDisplayComponent` | The template's video, shown first (the VSL's point); consent-gated for outside services | — | mobile | — |
| Headline | `m3.Heading` (`hdg_vsl_hero`) | The offer's headline (client content) | `headline-large` | — | — |
| Body text | `m3.Text` (`text_vsl_body`) | The offer's text | `body-large` | — | — |
| Capture form | `m3.Card` (`card_vsl_form`, elevated) | The capture fields from `lead_captures.form_fields`, below the video | `elevated-card` | mobile | — |
| Submit button | `m3.Button` (`btn_vsl_submit`, filled) | The capture's primary action | `primary-button` | — | the capture's own label |
| Privacy Policy link | `m3.Link` (`lnk_vsl_privacy`) | Beside the submit button (F-22) | `tertiary-button` | — | `consent.terms.link_privacy` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Content | `landing_pages` | `config` |
| Video address | `landing_pages` | `config`'s video entry |
| Capture fields | `lead_captures` | `form_fields` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Submit | `btn_vsl_submit_click` | the capture write (ticket 31) | The lead and contact are created together |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the template renders its content; an empty video address hides the video section |
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

## 7. Text

| Phrase key | English text |
|---|---|

*(No phrase keys of its own: the texts are the client's own content.)*


## 8. Help Videos

| Process | Help video |
|---|---|
| None | The editor's video covers the set-up |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-20 Lead capture via landing page | The video-sales-letter capture kind |
| UF-18 Landing page creation | The template chosen in the editor |
