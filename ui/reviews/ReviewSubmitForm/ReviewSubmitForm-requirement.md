---
title: "ReviewSubmitForm-requirement"
status: draft
doc-id: "mb-91c228a5"
updated: "2026-11-25"
---
# ReviewSubmitForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `reviews.ReviewSubmitForm` |
| Purpose | The public review page at `/review/{token}`: star rating (1–5) and comment (ticket 37, F-23; UF-27) |
| Screen type and width | Public page — 900 px (the review page; a single card, comfortably laid out) |
| Layout | `layouts.BlankLayout` (the token page is a standalone public page) |
| Route | `/review/{token}` — Anvil Routing (`reviews.request_token`); the token page is intentionally public (access matrix) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page container | Anvil `ColumnPanel` (`col_review_submit`) | The page's one content column | — | mobile | — |
| Invitation card | `m3.Card` (`card_review_invite`, elevated) | The invitation: who is asking and about which appointment | `elevated-card` | mobile | — |
| Business name | `m3.Text` (`text_review_business`) | The business's name from `business_profile` | `title-medium` | — | — |
| Rating input | `components.StarRatingComponent` (input mode) | One to five stars, selectable by pointer and keyboard (ticket 08) | — | mobile | — |
| Comment field | `m3.TextArea` (`txt_review_comment`) | The comment | `outlined-input` | mobile | `reviews.submit.comment` |
| Submit button | `m3.Button` (`btn_submit_review`, filled) | Submits the review | `primary-button` | — | `reviews.submit.send` |
| Thank-you card | `m3.Card` (`card_review_thanks`, elevated, hidden) | The confirmation after submission | `elevated-card` | mobile | — |
| Expired card | `m3.Card` (`card_review_expired`, outlined, hidden) | A used or unknown token's state | `surface-card` | mobile | — |
| Cookie banner | the public banner | Not on the blank layout; the review page sets no cookies of its own beyond the necessary first-party `mb_consent` handled at the platform level | — | — | — |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The review | `reviews` | `request_token`, `rating`, `comment`, `status` (`awaiting_approval` after submission), `submitted_at` |
| The booking | `bookings` | the review's booking (AC-S2: a customer's review is linked to their own booking) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the page | the route handler | `review_service` token read (`server_reviews/review_service`) | The invitation arrives; a used token shows the expired card |
| Submit | `btn_submit_review_click` | `review_service` submit | The review is stored as `awaiting_approval`; the thank-you card shows |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Star icon (`mi:star`) |
| Heading | `reviews.submit.expired.title` |
| Body text | `reviews.submit.expired.body` — the token has been used or is unknown |
| Primary action | None — the message asks the customer to contact the business |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | all |
| SignedOut | all |

The token page is intentionally public.

## 7. Text

| Phrase key | English text |
|---|---|
| `reviews.submit.title` | How was your appointment? |
| `reviews.submit.hint` | {business} would love your honest review. |
| `reviews.submit.comment` | Tell us about it |
| `reviews.submit.send` | Send review |
| `reviews.submit.thanks.title` | Thank you! |
| `reviews.submit.thanks.body` | Your review is with {business} now. They'll publish it once they've read it. |
| `reviews.submit.expired.title` | This review link has been used |
| `reviews.submit.expired.body` | Review links work once. Contact the business if you'd like to change your review. |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The public form explains itself; the star component carries the accessible labels |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-27 Reviews and testimonials | The customer's half: rating and comment after the completed appointment |
