---
title: "ReviewListForm-requirement"
status: draft
doc-id: "mb-d2954304"
updated: "2026-11-25"
---
# ReviewListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `reviews.ReviewListForm` |
| Purpose | Approve or decline reviews; the awaiting-approval queue (ticket 37, F-23; UF-27) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Customers & Marketing → Reviews) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_reviews`) | "Reviews", `headline-large` | `page-title` | — | `reviews.title` |
| Status filter | `m3.ButtonGroup` (`bgrp_review_status`, connected, single: Awaiting/Approved/All) | The queue first | — | mobile | `reviews.filter.awaiting`, `reviews.filter.approved`, `reviews.filter.all` |
| Reviews grid | Anvil `DataGrid` (`dg_reviews`) | Customer, service, provider, rating, comment, status, date | — | mobile | — |
| Review row | Anvil `DataRowPanel` (`drp_review_*`) | One review per row | — | — | — |
| Rating display | `components.StarRatingComponent` (display mode) | The review's stars with the text (ticket 08) | — | — | — |
| Status badge | `m3.Chip` (`chip_review_status_*`, type `input`, removable=False) | Requested, Awaiting approval, Approved, Declined | — | — | — |
| Approve button | `m3.Button` (`btn_approve_review_*`, filled) | Approves; approved reviews become available as homepage testimonials | `primary-button` | — | `comments.approve` |
| Decline button | `m3.Button` (`btn_decline_review_*`, outlined) | Declines | `secondary-button` | — | `reviews.decline` | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `reviews.list`) | The feature's tooltip and help video | — | — | `help.reviews.list.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Grid | `reviews` | `contact_id`, `booking_id`, `provider_id`, `service_id`, `rating`, `comment`, `status`, `show_on_homepage`, `submitted_at` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_reviews` | `review_service` list read (`server_reviews/review_service`) | The grid arrives |
| Approve | `btn_approve_review_*_click` | `review_service` approve | The review becomes approved and available for the homepage |
| Decline | `btn_decline_review_*_click` | `review_service` decline | The review is declined; it never shows publicly |
| Open a review | the row click | None | `reviews.ReviewViewerForm` opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Star icon (`mi:star`) |
| Heading | `reviews.empty.title` (DESIGN.md: "No reviews yet") |
| Body text | `reviews.empty.body` (DESIGN.md: "Customers are invited to review after each completed appointment. Approved reviews can appear on your homepage." — no action, per DESIGN.md) |
| Primary action | None — the empty state explains how reviews arrive (DESIGN.md) |

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
| `reviews.title` | Reviews |
| `reviews.filter.awaiting` | Awaiting approval |
| `reviews.filter.approved` | Approved |
| `reviews.filter.all` | All |
| `reviews.decline` | Decline |
| `reviews.status.requested` | Requested |
| `reviews.status.awaiting` | Awaiting approval |
| `reviews.status.approved` | Approved |
| `reviews.status.declined` | Declined |
| `reviews.empty.title` | No reviews yet |
| `reviews.empty.body` | Customers are invited to review after each completed appointment. Approved reviews can appear on your homepage. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Approving reviews | `master_template` help entry `reviews.list` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-27 Reviews and testimonials | The approval half of the flow |
| UF-17 Homepage customisation | Approved reviews are offered in the homepage editor |
