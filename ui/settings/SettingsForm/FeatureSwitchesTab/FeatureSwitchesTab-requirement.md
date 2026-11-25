---
title: "FeatureSwitchesTab-requirement"
status: draft
doc-id: "mb-c8d7cdee"
updated: "2026-11-25"
---
# FeatureSwitchesTab — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `settings.SettingsForm.FeatureSwitchesTab` |
| Purpose | The Settings → Feature Switches tab: `marketing_enabled`, `blog_enabled` and blog comments (ticket 18, F-05) |
| Screen type and width | Settings panel inside `settings.SettingsForm` (1000 px) |
| Layout | Hosted by `settings.SettingsForm` |
| Route | None — a local tab of the hub |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Tab panel container | Anvil `ColumnPanel` (`col_switches_tab`) | The tab's one column | — | mobile | — |
| Marketing switch | `m3.Switch` (`sw_marketing`) | Shows or hides the marketing menu items (Campaigns, Broadcasts, Segments, Automations) in AdminLayout | — | — | `switches.marketing` |
| Marketing hint | `m3.Text` (`text_marketing_hint`) | What the switch shows and hides | `body-small` | — | `switches.marketing.hint` |
| Blog switch | `m3.Switch` (`sw_blog`) | Shows or hides the public Blog page and the Blog menu item | — | — | `switches.blog` |
| Blog hint | `m3.Text` (`text_blog_hint`) | What the switch shows and hides | `body-small` | — | `switches.blog.hint` |
| Comments switch | `m3.Switch` (`sw_comments`) | Blog comments on or off (`business_profile.enable_comments`) | — | — | `switches.comments` |
| Comments hint | `m3.Text` (`text_comments_hint`) | What the switch does | `body-small` | — | `switches.comments.hint` |
| Saved confirmation | `m3.Snackbar` (Python class, in code) | Confirms each change | — | — | `switches.saved` |
| Help icon | `components.HelpTooltipComponent` (help entry `settings.switches`) | The tab's tooltip and help video | — | — | `help.settings.switches.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Marketing switch | `config` | `value` of `marketing_enabled` |
| Blog switch | `config` | `value` of `blog_enabled` |
| Comments switch | `business_profile` | `enable_comments` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the tab | `form_show` → `load_switches` | `settings_service` read (`server_settings/settings_service`) | The switches' states arrive |
| Switch marketing | `sw_marketing_change` | `settings_service` save | `config.marketing_enabled` is stored; AdminLayout shows or hides the marketing menu items at once |
| Switch blog | `sw_blog_change` | `settings_service` save | `config.blog_enabled` is stored; PublicLayout shows or hides the Blog link |
| Switch comments | `sw_comments_change` | `settings_service` save | `business_profile.enable_comments` is stored |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the tab always shows its switches |
| Heading | None — not applicable |
| Body text | None — not applicable |
| Primary action | None — not applicable |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | none |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per `access-matrix.yaml` `settings_tabs.FeatureSwitches`.

## 7. Text

| Phrase key | English text |
|---|---|
| `switches.marketing` | Email marketing |
| `switches.marketing.hint` | Shows Campaigns, Broadcasts, Segments and Automations in the menu. |
| `switches.blog` | Blog |
| `switches.blog.hint` | Shows the Blog page and menu item on your website. |
| `switches.comments` | Blog comments |
| `switches.comments.hint` | Visitors can comment on your posts; you approve each comment. |
| `switches.saved` | Feature switches saved. |

## 8. Help Videos

| Process | Help video |
|---|---|
| The feature switches | `master_template` help entry `settings.switches` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-16 Content management | The Blog switch gates the blog's public page and menu |
| UF-23 / UF-24 Campaigns and broadcasts | The marketing switch gates the campaigns and broadcasts |
| UF-27 Reviews | Not this tab — comments only |
