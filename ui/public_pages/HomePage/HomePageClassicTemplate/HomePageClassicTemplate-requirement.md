---
title: "HomePageClassicTemplate-requirement"
status: draft
doc-id: "mb-1dc3eae6"
updated: "2026-11-25"
---
# HomePageClassicTemplate — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `public_pages.HomePage.HomePageClassicTemplate` |
| Purpose | The Classic homepage variant: Hero → Intro → Services → Video → Testimonials → Trust strip → CTA band → Footer; all major sections visible by default (ticket 34, F-07; DESIGN.md Template Differentiation Matrix) |
| Screen type and width | Public page — 900 px (a section order of the home page) |
| Layout | Hosted by `public_pages.HomePage` |
| Route | None — a local template of the home page |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Template container | Anvil `ColumnPanel` (`col_template_classic`) | The variant's section order | — | mobile | — |
| The sections | the shared section inventory's components (as `public_pages.HomePage` lists them) | The variant states the order and the default visibility; it invents no new components (DESIGN.md Template Governance) | as the shared inventory | mobile | as the shared inventory |

## 3. Data

| Element | Table | Column |
|---|---|---|
| All sections | `config` | `value` of `home_config` (the same data the other variants read) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| None of its own | — | — | The variant renders; the sections carry the actions |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the variant renders its configured sections |
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
| None | The home page's video covers the variants |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-17 Homepage customisation | The Classic variant (default, balanced) |
