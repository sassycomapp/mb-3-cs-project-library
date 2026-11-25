---
title: "CookiePolicyPage-requirement"
status: draft
doc-id: "mb-35b4b35f"
updated: "2026-11-25"
---
# CookiePolicyPage — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `public_pages.CookiePolicyPage` |
| Purpose | The public page that renders the Owner's Cookie Policy, with the feature clauses tied to the features in use (ticket 33, F-22; DESIGN.md CookiePolicyPage) |
| Screen type and width | Public page — 900 px, full width: content sections |
| Layout | `layouts.PublicLayout` |
| Route | `/cookie-policy` — Anvil Routing; `Route.meta` sets the share tags |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page container | Anvil `ColumnPanel` (`col_cookie_policy`) | The page's one content column | — | mobile | — |
| Page title | `m3.Heading` (`hdg_cookie_title`) | "Cookie Policy", `headline-large` | `page-title` | — | `public.policy.cookie.title` |
| Version display | `m3.Text` (`text_cookie_version`) | The published version | `body-small` | — | `public.policy.version` |
| Category section | `m3.Heading` (`hdg_cookie_category_*`) | The clauses grouped by category, in order | `headline-small` | — | — |
| Clause card | `m3.Card` (`card_cookie_clause_*`, outlined) with `m3.Heading` + `m3.Text` | The clause's title and text | `surface-card` | mobile | — |
| Footer | `components.FooterComponent` | The shared footer, whose Cookie settings link opens the banner again | — | — | — |

Same rules as the other policy pages. A `cookie_policy` clause with a `feature_key` is active exactly while its feature is in use (schema; ticket 12).

## 3. Data

| Element | Table | Column |
|---|---|---|
| Clauses | `cookie_policy` | `category`, `title`, `clause`, `sort_order`, `is_active`, `feature_key` (`maps` / `video` / `payments`) |
| Version | `config` | `value` of `legal_versions` (`cookie_policy`) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the page | the route handler | `page_content_service` public read | The published clauses arrive |
| Reopen the banner | the footer's Cookie settings link | `record_consent` | The banner opens again and shows the visitor's consent number |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Cookie icon (`mi:cookie`) |
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
| `public.policy.cookie.title` | Cookie Policy |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The public page explains itself |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-28 Clause Builder | The published document rendered here |
| Cookie choice first | The banner's More info opens this page |
