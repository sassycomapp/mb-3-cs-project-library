---
title: "ParallaxComponent-requirement"
status: draft
doc-id: "mb-a5d2600b"
updated: "2026-11-25"
---
# ParallaxComponent — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `components.ParallaxComponent` |
| Purpose | The parallax hero of the homepage, About, Services and the landing pages: a background image moving at a different speed from the foreground on scroll (ticket 07, F-09) |
| Screen type and width | Custom component (embedded in a host's hero section); its width is its host's |
| Layout | A component form built from theme components and minimal `anvil.js` (never `eval`); disabled when the device requests reduced motion |
| Route | None — a component |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Component container | Anvil `ColumnPanel` (`col_parallax`) | Anvil canonical container; the parallax surface with the background image behind the foreground content | — | never | — |
| Foreground slot | Anvil `ColumnPanel` (`col_foreground`) | The host drops its hero heading, subheading and call to action here | — | mobile | — |
| Scroll effect | `anvil.js` scroll handler | Minimal browser code for the parallax offset; disabled when the device asks for reduced motion (custom-component-requirements-matrix, ParallaxComponent) | — | — | — |
| Contrast overlay | role with a token-derived overlay | Text over the image keeps WCAG AA contrast through a token-derived overlay (no colour value of its own) | `parallax-overlay` | — | — |

Properties of the component: `image` (the background address, set by the host), set in code; no other interface.

## 3. Data

| Element | Table | Column |
|---|---|---|
| Background image | the host's record | Homepage hero: `config` `home_config` (hero image); About, Services and landing pages: their content records |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Scroll effect | the `anvil.js` scroll handler | None | The background moves at a different speed from the foreground; no motion when the device asks for reduced motion |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — an empty image address hides the component (the host decides) |
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
| (none) | The component holds no visible text; its host supplies the hero text |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The component is presentation only |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management and edit mode | The hero is an editable element of the host page in edit mode |
| UF-25 Homepage customisation | The HomepageEditorForm sets the hero; the templates render it here |
| UF-20 Lead capture | The landing pages' heroes |
