---
title: "ThemeConfigForm-requirement"
status: draft
doc-id: "mb-3d2f1a03"
updated: "2026-11-25"
---
# ThemeConfigForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `settings.ThemeConfigForm` |
| Purpose | Settings → Theme: the public website's header style (ticket 33, F-06; DESIGN.md ThemeConfigForm) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels (hosted in the Theme tab) |
| Layout | `layouts.AdminLayout` (opened from the Theme tab) |
| Route | None — opened from `settings.SettingsForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Header style selector | `m3.DropdownMenu` (`dd_header_style`) | Standard, Centered, Minimal (DESIGN.md ThemeConfigForm) | `outlined-select` | — | `theme.header_style` |
| Live preview card | `m3.Card` (`card_theme_preview`, outlined) | Live preview of the public website header (DESIGN.md ThemeConfigForm) | `surface-card` | mobile | — |
| Preview title row | `m3.Text` (`text_preview_title`) | The preview's site name row, in the chosen arrangement | `body-medium` | — | — |
| Colours note | `m3.Text` (`text_theme_note`) | Colours come only from the eight palettes (Brand Palette tab); typography is Roboto only. Free colour pickers and font choices are not offered, because they would break the palette system, contrast and the single type family (DESIGN.md ThemeConfigForm) | `body-small` | — | `theme.note` |
| Save button | `m3.Button` (`btn_save_theme`, filled) | Saves the header style | `primary-button` | — | `theme.save` |
| Saved confirmation | `m3.Snackbar` (Python class, in code) | Confirms the save | — | — | `theme.saved` |
| Help icon | `components.HelpTooltipComponent` (help entry `settings.theme`) | The feature's tooltip and help video | — | — | `help.settings.theme.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Header style | `theme_config` | `header_style` |
| Updated at | `theme_config` | `updated_at` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the form | `form_show` → `load_theme` | `settings_service` read (`server_settings/settings_service`) | The current header style arrives |
| Save | `btn_save_theme_click` | `settings_service` save | `theme_config.header_style` is stored |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the form always shows its selector and preview |
| Heading | None — not applicable |
| Body text | None — not applicable |
| Primary action | None — not applicable |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per `access-matrix.yaml` `settings_tabs.Theme`.

## 7. Text

| Phrase key | English text |
|---|---|
| `theme.header_style` | Header style |
| `theme.header_style.standard` | Standard |
| `theme.header_style.centered` | Centered |
| `theme.header_style.minimal` | Minimal |
| `theme.note` | Colours come from your palette and type is Roboto, so your site stays consistent everywhere. |
| `theme.save` | Save |
| `theme.saved` | Header style saved. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Choosing your header style | `master_template` help entry `settings.theme` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management | The public website's header configuration |
| UF-44 Trying the demo apps | The demo client changes the header; the nightly reset restores it |
