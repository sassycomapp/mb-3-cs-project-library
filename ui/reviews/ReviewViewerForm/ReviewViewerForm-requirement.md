---
title: "ReviewViewerForm-requirement"
status: draft
doc-id: "mb-42b4bb5b"
updated: "2026-11-25"
---
# ReviewViewerForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `reviews.ReviewViewerForm` |
| Purpose | A review's record: the rating, the comment, the booking and customer it came from, its status and homepage switch (ticket 37, F-23) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `reviews.ReviewListForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_review_viewer`) | The customer's name, `headline-large` | `page-title` | — | — |
| Rating display | `components.StarRatingComponent` (display mode) | The review's stars with the text (ticket 08) | — | — | — |
| Status badge | `m3.Chip` (`chip_review_status`, type `input`, removable=False) | The review's status | — | — | — |
| Comment card | `m3.Card` (`card_review_comment`, outlined) | The review's comment | `surface-card` | mobile | — |
| Booking card | `m3.Card` (`card_review_booking`, outlined) | The booking, service and provider the review is about; the click opens them | `surface-card` | mobile | — |
| Homepage switch | `m3.Switch` (`sw_show_on_homepage`) | Sets `reviews.show_on_homepage` (at most 10 reviews and testimonials together) | — | — | `homepage.testimonial.show` |
| Approve / Decline buttons | `m3.Button` (`btn_approve_review`, filled), `m3.Button` (`btn_decline_review`, outlined) | The status actions while awaiting approval | `primary-button` | — | `comments.approve` |
| Help icon | `components.HelpTooltipComponent` (help entry `reviews.viewer`) | The feature's tooltip and help video | — | — | `help.reviews.viewer.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Review | `reviews` | all the review's columns |
| Booking | `bookings` | the review's booking |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the review | `form_show` → `load_review` | `review_service` read (`server_reviews/review_service`) | The review arrives |
| Approve / Decline | `btn_approve_review_click` / `btn_decline_review_click` | `review_service` approve / decline | The status changes |
| Show on the homepage | `sw_show_on_homepage_change` | `review_service` save | `show_on_homepage` is set; the tenth is refused when ten are shown |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Star icon (`mi:star`) |
| Heading | `reviews.empty.title` |
| Body text | `reviews.empty.body` — the not-found state |
| Primary action | None |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix.

## 7. Text

| Phrase key | English text |
|---|---|

*(No phrase keys of its own beyond the shared keys listed above.)*


## 8. Help Videos

| Process | Help video |
|---|---|
| None | The reviews list's video covers the flow |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-27 Reviews and testimonials | The review's record |
| UF-17 Homepage customisation | The homepage switch |
