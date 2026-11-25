---
title: "AboutPage-requirement"
status: draft
doc-id: "mb-fb6b3b03"
updated: "2026-11-25"
---
# AboutPage — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `public_pages.AboutPage` |
| Purpose | The public About page: the business's story, credentials and team presence, editable in edit mode (ticket 33, F-06) |
| Screen type and width | Public page — 900 px, full width: content sections |
| Layout | `layouts.PublicLayout` |
| Route | `/about` — Anvil Routing; `Route.meta` sets the title, description and Open Graph tags |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page container | Anvil `ColumnPanel` (`col_about`) | The page's one content column | — | mobile | — |
| Hero | `components.ParallaxComponent` | The page's hero (custom-component-requirements-matrix: About) | — | mobile | — |
| Story sections | `m3.Heading` + `m3.Text` in `m3.Card` sections | The editable sections of the business's story | `headline-small`, `body-large`, `surface-card` | mobile | — |
| Accreditations section | `m3.Card` (`card_about_accreditations`, outlined) | The business's accreditations from `business_profile.accreditations` | `surface-card` | mobile | — |
| Footer | `components.FooterComponent` | The shared footer | — | — | — |
| Edit mode host | `components.EditModeComponent` | Placed once on the page (ticket 11) | — | — | `editmode.button` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Page content | `pages` | `components` (one entry per editable element, keyed by element id; written by edit mode, F-06) |
| Accreditations | `business_profile` | `accreditations` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the page | the route handler | `page_content_service` public read | The page's content arrives; `Route.meta` sets the share tags |
| Save (edit mode) | via EditModeComponent | `save_page_content` (`server_settings/page_content_service`) | The page's content is saved and published at once; the save is audited |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the page renders its content |
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
| (none) | The page's texts are the client's own content; the edit-mode keys live on the component |

## 8. Help Videos

| Process | Help video |
|---|---|
| Editing your About page | `master_template` help entry `public_pages.about` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management and edit mode | The page's edit mode |
