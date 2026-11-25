---
title: "HomePageMinimalistTemplate-requirement"
status: draft
doc-id: "mb-4ec6276b"
updated: "2026-11-25"
---
# HomePageMinimalistTemplate — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `public_pages.HomePage.HomePageMinimalistTemplate` |
| Purpose | The Minimalist homepage variant: Hero (concise) → Intro (very brief) → Trust strip (compact) → CTA band → Footer; hides the carousel, full services grid, full testimonials and video, uses fewer text blocks and more whitespace (ticket 34, F-07; DESIGN.md Template Differentiation Matrix) |
| Screen type and width | Public page — 900 px (a section order of the home page) |
| Layout | Hosted by `public_pages.HomePage` |
| Route | None — a local template of the home page |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Template container | Anvil `ColumnPanel` (`col_template_minimalist`) | The variant's section order | — | mobile | — |
| The sections | the shared section inventory's components (the shown ones) | The variant hides the carousel, full services grid, full testimonials and video (DESIGN.md) | as the shared inventory | mobile | as the shared inventory |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Shown sections | `config` | `value` of `home_config` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| None of its own | — | — | The variant renders |

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
| UF-17 Homepage customisation | The clean, minimal variant |
