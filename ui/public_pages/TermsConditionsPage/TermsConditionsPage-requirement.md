---
title: "TermsConditionsPage-requirement"
status: draft
doc-id: "mb-19a9a902"
updated: "2026-11-25"
---
# TermsConditionsPage — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `public_pages.TermsConditionsPage` |
| Purpose | The public page that renders the Owner's Terms and Conditions, as `PrivacyPolicyPage` renders the Privacy Policy (ticket 33, F-22; DESIGN.md TermsConditionsPage) |
| Screen type and width | Public page — 900 px, full width: content sections |
| Layout | `layouts.PublicLayout` |
| Route | `/terms` — Anvil Routing; `Route.meta` sets the share tags |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page container | Anvil `ColumnPanel` (`col_terms`) | The page's one content column | — | mobile | — |
| Page title | `m3.Heading` (`hdg_terms_title`) | "Terms and Conditions", `headline-large` | `page-title` | — | `public.policy.terms.title` |
| Version display | `m3.Text` (`text_terms_version`) | The published version | `body-small` | — | `public.policy.version` |
| Category section | `m3.Heading` (`hdg_terms_category_*`) | The clauses grouped by category, in order | `headline-small` | — | — |
| Clause card | `m3.Card` (`card_terms_clause_*`, outlined) with `m3.Heading` + `m3.Text` | The clause's title and text | `surface-card` | mobile | — |
| Footer | `components.FooterComponent` | The shared footer | — | — | — |

Same rules as `PrivacyPolicyPage`: only active clauses, category order then `sort_order`, comfortable density (DESIGN.md).

## 3. Data

| Element | Table | Column |
|---|---|---|
| Clauses | `t_and_c` | `category`, `title`, `clause`, `sort_order`, `is_active` |
| Version | `config` | `value` of `legal_versions` (`t_and_c`) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the page | the route handler | `page_content_service` public read | The published clauses arrive |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Document icon (`mi:description`) |
| Heading | `public.policy.empty.title` |
| Body text | `public.policy.empty.body` |
| Primary action | None |

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
| `public.policy.terms.title` | Terms and Conditions |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The public page explains itself |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-28 Clause Builder | The published document rendered here |
| UF-05–UF-10 Bookings, orders and packages | The acceptance box links here |
| UF-41 Customer accepts a quote | The acceptance box links here |
