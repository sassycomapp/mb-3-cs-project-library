---
title: "StarRatingComponent-requirement"
status: draft
doc-id: "mb-42565efd"
updated: "2026-11-25"
---
# StarRatingComponent — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `components.StarRatingComponent` |
| Purpose | The star rating for reviews and testimonials, in input mode (one to five stars, pointer and keyboard) and display mode (read-only stars plus the rating as text) (ticket 08, F-23) |
| Screen type and width | Custom component (embedded in ReviewSubmitForm, ReviewViewerForm, the homepage testimonials and My Reviews); its width is its host's |
| Layout | A component form built from theme components; filled stars Primary, empty stars Outline, tokens only |
| Route | None — a component |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Component container | Anvil `FlowPanel` (`flow_stars`) | Anvil canonical container; the stars in one row that never rearranges | — | never | — |
| The five stars (input mode) | `m3.ToggleIconButton` (`tib_star_1` … `tib_star_5`, icon `mi:star`) | The theme's toggle icon button carries a selected state and a click event — the theme component for a star that is on or off; keyboard-reachable (ticket 08: selectable by pointer and keyboard) | selected: filled `Primary`; empty: `Outline` | — | `stars.value.1` … `stars.value.5` (accessible label per value) |
| The five stars (display mode) | `m3.IconButton` (`ico_star_1` … `ico_star_5`, decorative) | Read-only stars are decorative icon buttons with no click event (`ico_` per nomenclature 1.6) | filled `Primary`; empty `Outline` | — | — |
| Rating text (display mode) | `m3.Text` (`text_rating`) | The rating also given as text, e.g. "4 out of 5" — no state is shown by colour alone (ticket 08) | `body-small` | — | `stars.display_text` |

Properties: `rating` (number, 1–5), `mode` (`input` / `display`), set by the host in code.

## 3. Data

| Element | Table | Column |
|---|---|---|
| `rating` | the host's record | `reviews.rating`, `testimonials.rating` (1 to 5) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Set a star (input mode) | `tib_star_<n>_click` | None — the host saves with its own form's submit | The component raises its `change` event with the value; the host stores it on its record |
| Display a rating | host code | None | The stars fill to the rating and the text states it ("4 out of 5") |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — with no rating in display mode the stars show empty and the text says no rating yet |
| Heading | None — not applicable |
| Body text | `stars.display_text` states "Not rated yet" when the rating is empty |
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

Input mode is reachable by whoever the host form shows it to (the customer submitting a review; the Owner typing a testimonial).

## 7. Text

| Phrase key | English text |
|---|---|
| `stars.value.1` | 1 star |
| `stars.value.2` | 2 stars |
| `stars.value.3` | 3 stars |
| `stars.value.4` | 4 stars |
| `stars.value.5` | 5 stars |
| `stars.display_text` | {count} out of 5 |
| `stars.not_rated` | Not rated yet |

## 8. Help Videos

| Process | Help video |
|---|---|
| Leaving a review | `master_template` help entry `reviews.submit` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-27 Reviews and testimonials | The customer's star rating on ReviewSubmitForm; the rating display on ReviewViewerForm and My Reviews |
| UF-25 Homepage customisation | The homepage testimonials' rating display |
