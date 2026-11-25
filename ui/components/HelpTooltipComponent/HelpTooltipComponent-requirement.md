---
title: "HelpTooltipComponent-requirement"
status: draft
doc-id: "mb-9afdfcd2"
updated: "2026-11-25"
---
# HelpTooltipComponent — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `components.HelpTooltipComponent` |
| Purpose | The in-app help control placed beside a feature's heading or field on every admin screen and every onboarding step: a help icon that opens the feature's tooltip and, from it, the feature's help video (ticket 10, F-25) |
| Screen type and width | Custom component (embedded); its size is the help icon's touch target |
| Layout | A component form built from theme components; `help_key` is set in code by the host |
| Route | None — a component |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Help icon | `m3.IconButton` (`ibtn_help`, icon `mi:help_outline`, standard appearance) | A small help icon button beside the feature's heading or field; its accessible label is the feature's plain-language tooltip text (custom-component-requirements-matrix, HelpTooltipComponent) | `icon-button` | — | set from the feature's help entry (`help.<feature>.tooltip`) |
| Persistent rich tooltip | `m3.RichTooltip` (Python class, created in code, persistent) | `m3.RichTooltip` shows the text and one action, held open on click; the theme component for a tooltip with an action (RichTooltip.md) | — | — | the feature's tooltip text is the tooltip's `text` |
| Watch-the-video action | the `RichTooltip`'s `action_text` + `on_action` | The action opens the video dialog | — | — | `help.tooltip.watch_video` |
| Video dialog | `m3.Dialog` (`dlg_help_video`) | The theme's dialog holds the feature's help video through `components.VideoDisplayComponent` in the app language; closing returns focus to the icon | — | — | `help.video.title` |
| Hidden when tooltips are off | component `visible` set in code | The component is hidden when the user has switched tooltips off (`users.preferences.tooltips`) | — | — | — |

## 3. Data

| Element | Table | Column |
|---|---|---|
| `help_key` | — | The feature's help entry: text and video address per language, held in `master_template` (ticket 10) |
| Tooltips preference | `users` | `preferences.tooltips` (via the signed-in user, never a client-supplied id — ticket 10, B1) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Show the tooltip | the icon's focus/hover, or click for the persistent tooltip | None | The feature's plain-language tooltip text appears |
| Watch the video | the `RichTooltip` action (`on_action`) | None | The video dialog opens with the feature's help video in the app language |
| Close the video | `dlg_help_video` close | None | Focus returns to the help icon |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the component is one icon; a missing help entry shows the icon hidden (defensive: `help_key` with no entry hides the component) |
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
| Customer | none |
| SignedOut | none |

The component is placed on admin screens and onboarding steps, which are team-member screens (SR-41: self-service help on every screen).

## 7. Text

| Phrase key | English text |
|---|---|
| `help.tooltip.watch_video` | Watch the video |
| `help.video.title` | How this works |
| `help.<feature>.tooltip` | The feature's plain-language tooltip text, one per feature, held with the help entry |

## 8. Help Videos

| Process | Help video |
|---|---|
| Every feature it is placed on | The feature's own help entry: text and one video per language in `master_template`; the pop-up plays the version in the app language, and the English version with subtitles when that language's version does not yet exist (spec-translation section 9) |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-02 Owner onboarding | A help video on every onboarding step |
| UF-30 Using the AI helper | The helper's tooltip opens its help video |
| UF-34 Help | Self-service help before a ticket: SR-41 |
| Every admin flow | The tooltip beside each feature's heading or field |
