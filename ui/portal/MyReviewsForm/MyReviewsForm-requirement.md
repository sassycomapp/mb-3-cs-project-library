---
title: "MyReviewsForm-requirement"
status: draft
doc-id: "mb-96129520"
updated: "2026-11-25"
---
# MyReviewsForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `portal.MyReviewsForm` |
| Purpose | The customer's reviews: reviews written and pending review requests (ticket 39, F-24; UF-29 step 7) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.CustomerLayout` |
| Route | None — opened by the portal routing |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_my_reviews`) | "My Reviews", `headline-large` | `page-title` | — | `portal.nav.reviews` |
| Written reviews card | `m3.Card` (`card_written_reviews`, outlined) | The reviews the customer has written | `surface-card` | mobile | — |
| Review row | `m3.ListItem` (`li_my_review_*`) | Service, date, rating display, status | — | — | — |
| Rating display | `components.StarRatingComponent` (display mode) | The review's stars (ticket 08) | — | — | — |
| Pending requests card | `m3.Card` (`card_pending_reviews`, outlined) | The review requests waiting (`reviews.status = requested`) | `surface-card` | mobile | — |
| Request row | `m3.ListItem` (`li_review_request_*`) | Service, date; the action opens `reviews.ReviewSubmitForm` | — | — | — |
| Leave a review button | `m3.Button` (`btn_leave_review_*`, filled) | Opens the review submission | `primary-button` | — | `reviews.submit.send` |
| Help icon | `components.HelpTooltipComponent` (help entry `portal.reviews`) | The feature's tooltip and help video | — | — | `help.portal.reviews.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Reviews | `reviews` | the signed-in customer's rows |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the reviews | `form_show` → `load_my_reviews` | the portal's review read (own only) | The cards arrive |
| Leave a review | `btn_leave_review_*_click` | None | `reviews.ReviewSubmitForm` opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Star icon (`mi:star`) |
| Heading | `portal.reviews.empty.title` |
| Body text | `portal.reviews.empty.body` |
| Primary action | None — requests arrive after appointments |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | none |
| Customer | all |
| SignedOut | none |

## 7. Text

| Phrase key | English text |
|---|---|
| `portal.reviews.written` | Your reviews |
| `portal.reviews.pending` | Reviews waiting for you |
| `portal.reviews.empty.title` | No reviews yet |
| `portal.reviews.empty.body` | After your appointments you can review the service here. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Your reviews | `master_template` help entry `portal.reviews` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-27 Reviews and testimonials | The review also appears in the portal under My Reviews (UF-27 step 3) |
| UF-29 Customer portal | My Reviews |
