---
title: "PaletteSettingsForm-requirement"
status: draft
doc-id: "mb-262c60b2"
updated: "2026-11-25"
---
# PaletteSettingsForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `settings.SettingsForm.PaletteSettingsForm` |
| Purpose | The Brand Palette tab: each of the eight palettes with its samples and target fit; choosing previews live across the screen; Save stores the choice (ticket 18, F-31; DESIGN.md Palette Selector Requirements) |
| Screen type and width | Settings panel inside `settings.SettingsForm` (1000 px) |
| Layout | Hosted by `settings.SettingsForm`; a local component used only in this tab (custom-component-requirements-matrix, PaletteSettingsForm) |
| Route | None — a local tab of the hub |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Tab panel container | Anvil `ColumnPanel` (`col_palette_tab`) | The tab's one column | — | mobile | — |
| Palette grid | Anvil `GridPanel` (`grid_palettes`) | The eight palettes in a grid that stacks on a phone | — | mobile | — |
| Palette card | `m3.Card` (`card_palette_<name>`, clickable via `InteractiveCard`) | One card per palette; the card is clickable to preview | `surface-card` | mobile (320 px card in a grid) | `palette.<name>.label` |
| Primary swatch | Anvil `ColumnPanel` (`col_swatch_<name>`) | The palette's primary colour swatch, from the theme's tokens when that palette is applied | — | — | — |
| Sample filled button | `m3.Button` (`btn_sample_filled_<name>`) | The palette selector shows a sample filled button (DESIGN.md Palette Selector Requirements) | — | — | — |
| Sample outlined button | `m3.Button` (`btn_sample_outlined_<name>`) | A sample outlined button | — | — | — |
| Sample card | `m3.Card` (`card_sample_<name>`, outlined) | A sample card treatment | — | — | — |
| Sample heading and link | `m3.Heading` (`hdg_sample_<name>`) + `m3.Link` (`lnk_sample_<name>`) | A sample heading and link treatment | — | — | — |
| Target fit label | `m3.Text` (`text_fit_<name>`) | A clear label stating the target business fit (DESIGN.md) | `body-small` | — | `palette.<name>.fit` |
| Save button | `m3.Button` (`btn_save_palette`, filled) | Saves the chosen palette | `primary-button` | — | `palette.save` |
| Saved confirmation | `m3.Snackbar` (Python class, in code) | Confirms the save | — | — | `palette.saved` |
| Help icon | `components.HelpTooltipComponent` (help entry `settings.palette`) | The tab's tooltip and help video | — | — | `help.settings.palette.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Current palette | `business_profile` | `palette` (`default` / `professional_blue` / `warm_coral` / `calm_teal` / `sophisticated_purple` / `sky_blue` / `forest_green` / `desert_rose`) |
| Palette change time | `business_profile` | `palette_updated_at` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Preview a palette | `card_palette_<name>_click` | None (client-side) | The palette previews live across the screen through `m3x_theming.set_palette` (`m3-custom-theme`) before it is saved (custom-component-requirements-matrix) |
| Save | `btn_save_palette_click` | `settings_service` save (`server_settings/settings_service`) | `business_profile.palette` is stored with `palette_updated_at`; the theme's module sets the palette on the page, which changes the `--m3x-*` properties and nothing else (DESIGN.md Palette Governance) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the eight palettes always show |
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

Per `access-matrix.yaml` `settings_tabs.BrandPalette`.

## 7. Text

| Phrase key | English text |
|---|---|
| `palette.default.label` | Default |
| `palette.default.fit` | The plain Material 3 look |
| `palette.professional_blue.label` | Professional Blue |
| `palette.professional_blue.fit` | Consultants, clinics, therapists — trust and stability |
| `palette.warm_coral.label` | Warm Coral |
| `palette.warm_coral.fit` | Salons, spas, lifestyle, beauty |
| `palette.calm_teal.label` | Calm Teal |
| `palette.calm_teal.fit` | Fitness, wellness, health services |
| `palette.sophisticated_purple.label` | Sophisticated Purple |
| `palette.sophisticated_purple.fit` | Executive coaching, premium consulting |
| `palette.sky_blue.label` | Sky Blue |
| `palette.sky_blue.fit` | IT support, online tutors, remote services |
| `palette.forest_green.label` | Forest Green |
| `palette.forest_green.fit` | Gardeners, trades, outdoor services |
| `palette.desert_rose.label` | Desert Rose |
| `palette.desert_rose.fit` | Aestheticians, photographers, event planners |
| `palette.save` | Save |
| `palette.saved` | Palette saved. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Choosing the brand palette | `master_template` help entry `settings.palette` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-02 Owner onboarding | The wizard's step 4 links here with live preview |
| UF-44 Trying the demo apps | The demo client changes the palette; the nightly reset restores it |
