---
title: "LandingPageEditorForm-requirement"
status: draft
doc-id: "mb-01dd1311"
updated: "2026-11-25"
---
# LandingPageEditorForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `landing_pages.LandingPageEditorForm` |
| Purpose | Create and configure a landing page: template, basic settings, content editing on the page itself, preview, publish (ticket 30, F-08; UF-18) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `landing_pages.LandingPageListForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_landing_editor`) | "New landing page" / the page's title, `headline-large` | `page-title` | — | `landing.new` |
| Template selector | `m3.ButtonGroup` (`bgrp_template`, connected, single) | Lead Capture, Event Registration or Video Sales Letter (UF-18 step 1) | — | mobile | `landing.template.lead`, `landing.template.event`, `landing.template.vsl` |
| Title field | `m3.TextBox` (`txt_landing_title`, outlined) | The page's title | `outlined-input` | — | `landing.page_title` |
| Address field | `m3.TextBox` (`txt_landing_slug`, outlined) | Generated, unique (UF-18 step 2) | `outlined-input` | — | `landing.slug` |
| Status selector | `m3.DropdownMenu` (`dd_landing_status`) | Draft or published | `outlined-select` | — | `landing.status` |
| Content note | `m3.Text` (`text_landing_content_note`) | The content is edited on the page itself in edit mode (UF-18 step 3; F-06) | `body-medium` | — | `landing.content_note` |
| Preview button | `m3.Button` (`btn_preview_landing`, outlined) | Preview on the blank layout — no header or footer (UF-18 step 4) | `secondary-button` | — | `landing.preview` |
| Publish button | `m3.Button` (`btn_publish_landing`, filled) | Publish → validated, saved, the shareable address shown (UF-18 step 5) | `primary-button` | — | `landing.publish` |
| Share address display | `m3.TextBox` (`txt_landing_url`, outlined, read-only) | The shareable address, with a copy button | `outlined-input` | — | `landing.url` |
| Copy button | `m3.IconButton` (`ibtn_copy_landing_url`, icon `mi:content_copy`) | Copies the address | `icon-button` | — | `plan.kit.copy` |
| Share actions | `m3.Button` (`btn_send_test_email`, text), `m3.Button` (`btn_share_landing`, text), `m3.Button` (`btn_embed_landing`, text), `m3.Button` (`btn_add_to_campaign`, text) | Copy the address, send a test email, share to social media, embed, add to an email campaign (UF-18 step 6) | `tertiary-button` | — | `landing.share.test`, `landing.share.social`, `landing.share.embed`, `landing.share.campaign` |
| Analytics card | `m3.Card` (`card_landing_analytics`, outlined) | Views, conversions, conversion rate, referrer, device (UF-18 analytics) | `surface-card` | mobile | — |
| Edit mode host | `components.EditModeComponent` (on the published page) | The content is edited in edit mode on the page itself | — | — | `editmode.button` |
| Help icon | `components.HelpTooltipComponent` (help entry `landing.editor`) | The feature's tooltip and help video | — | — | `help.landing.editor.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Settings | `landing_pages` | `title`, `slug`, `template_type`, `config`, `status` |
| Analytics | `landing_pages` | `views_count`, `conversions_count` |
| Content | `landing_pages.config` | the editable elements (written by edit mode) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the page | `form_show` → `load_landing` | `landing_page_service` read (`server_marketing/landing_page_service`) | The settings and analytics arrive |
| Publish | `btn_publish_landing_click` | `landing_page_service` publish | Validated, saved; the shareable address is shown (UF-18) |
| Preview | `btn_preview_landing_click` | None | The page opens on the blank layout |
| Share | the share actions | None | The address is copied, the test sent, the campaign chosen |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the editor always shows its fields |
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

Per the access matrix; edit mode Owner/Manager only.

## 7. Text

| Phrase key | English text |
|---|---|
| `landing.new` | New landing page |
| `landing.page_title` | Title |
| `landing.slug` | Address (slug) |
| `landing.status` | Status |
| `landing.content_note` | You edit this page's content on the page itself, in edit mode. |
| `landing.preview` | Preview |
| `landing.publish` | Publish |
| `landing.published` | Your landing page is live at {url}. |
| `landing.share.test` | Send a test email |
| `landing.share.social` | Share |
| `landing.share.embed` | Embed |
| `landing.share.campaign` | Add to a campaign |

## 8. Help Videos

| Process | Help video |
|---|---|
| Creating a landing page | `master_template` help entry `landing.editor` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-18 Landing page creation | The whole flow: template, settings, preview, publish, share |
| UF-16 Content management and edit mode | The content edited in edit mode |
| UF-20 Lead capture | The published page captures leads |
