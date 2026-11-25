---
title: "BlankLayout-requirement"
status: draft
doc-id: "mb-029fba4a"
updated: "2026-11-25"
---
# BlankLayout — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `layouts.BlankLayout` |
| Purpose | The bare shell with no header and no navigation: sign-in, landing pages, error pages and public previews (ticket 01) |
| Screen type and width | Layout (shell); it hosts sign-in screens (400 px centred card), landing pages and errors, each of which sets its own width |
| Layout | `m3.Layouts.NavigationDrawerLayout` with `show_sidesheet=False`; the navigation drawer holds nothing |
| Route | None — the forms it hosts name BlankLayout as their layout |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| The shell | `m3.Layouts.NavigationDrawerLayout` | The theme's drawer layout with everything hidden — the neutral base (DESIGN.md ErrorLayout: "BlankLayout" as the no-chrome base) | — | — | — |
| Content slot | the layout's `content` slot | The hosted form fills the whole viewport | — | — | — |
| Landing-page edit-mode host | `components.EditModeComponent` (hidden by default) | Placed by the landing pages (ticket 11: every landing page template carries it); visible only to a signed-in Owner or Manager | — | — | `editmode.button` |

The layout adds no chrome of its own: no app bar, no navigation, no footer. A landing page hosted here is chrome-free by design (DESIGN.md F-08).

## 3. Data

| Element | Table | Column |
|---|---|---|
| Edit-mode host visibility | `users` | `role` (Owner, Manager) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| None | — | — | The layout hosts; its hosted forms carry every action |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the layout is a shell |
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

The layout itself carries no restriction; each hosted form carries its own (sign-in screens are SignedOut-first, landing pages are public).

## 7. Text

| Phrase key | English text |
|---|---|
| (none) | The layout holds no visible text of its own |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The layout hosts no process of its own |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-02 Owner onboarding | The shell of the screens before sign-in (SetPassword, Two-factor enrolment) |
| UF-05–UF-10 Booking and orders | Not this layout — the public flow pages sit on PublicLayout |
| UF-18 Landing page creation | The shell of the published landing page and its preview |
| UF-44 Trying the demo apps | The shell of the demo apps' sign-in screens |
| Error recovery | The shell of `layouts.ErrorLayout`'s error screens |
