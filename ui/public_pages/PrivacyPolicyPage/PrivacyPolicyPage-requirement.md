---
title: "PrivacyPolicyPage-requirement"
status: draft
doc-id: "mb-20103fa6"
updated: "2026-11-25"
---
# PrivacyPolicyPage — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `public_pages.PrivacyPolicyPage` |
| Purpose | The public page that renders the Owner's Privacy Policy: the clauses grouped by category in order (ticket 33, F-22; DESIGN.md PrivacyPolicyPage) |
| Screen type and width | Public page — 900 px, full width: content sections |
| Layout | `layouts.PublicLayout` |
| Route | `/privacy-policy` — Anvil Routing; `Route.meta` sets the share tags |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page container | Anvil `ColumnPanel` (`col_privacy`) | The page's one content column | — | mobile | — |
| Page title | `m3.Heading` (`hdg_privacy_title`) | "Privacy Policy", `headline-large` | `page-title` | — | `public.policy.privacy.title` |
| Version display | `m3.Text` (`text_privacy_version`) | The published version (`config.legal_versions`) | `body-small` | — | `public.policy.version` |
| Category section | `m3.Heading` (`hdg_policy_category_*`) | The clauses grouped by category, in category order then `sort_order` (DESIGN.md PrivacyPolicyPage) | `headline-small` | — | — |
| Clause card | `m3.Card` (`card_policy_clause_*`, outlined) with `m3.Heading` + `m3.Text` | The clause's title and text | `surface-card` | mobile | — |
| Footer | `components.FooterComponent` | The shared footer, with the policy links | — | — | — |

Only active clauses (`is_active = True`) are displayed (DESIGN.md PrivacyPolicyPage). Comfortable density and generous whitespace, per the public tone.

## 3. Data

| Element | Table | Column |
|---|---|---|
| Clauses | `privacy_policy` | `category`, `title`, `clause`, `sort_order`, `is_active` |
| Version | `config` | `value` of `legal_versions` (`privacy_policy`) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the page | the route handler | `page_content_service` public read | The published clauses arrive; `Route.meta` sets the share tags |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Document icon (`mi:description`) |
| Heading | `public.policy.empty.title` |
| Body text | `public.policy.empty.body` — before the document is published |
| Primary action | None — the message says the document is being prepared |

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
| `public.policy.privacy.title` | Privacy Policy |
| `public.policy.version` | Version {version} |
| `public.policy.empty.title` | Coming soon |
| `public.policy.empty.body` | This document is being prepared. |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The public page explains itself |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-28 Clause Builder | The published document rendered here |
| UF-19 Contact form submission | The Privacy Policy the contact form links to |
| UF-41 Customer accepts a quote | The acceptance box links here |
| Cookie choice first | The banner's More info opens the Cookie Policy page, not this one |
