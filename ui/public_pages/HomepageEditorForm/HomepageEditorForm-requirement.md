---
title: "HomepageEditorForm-requirement"
status: draft
doc-id: "mb-848cad51"
updated: "2026-11-25"
---
# HomepageEditorForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `public_pages.HomepageEditorForm` |
| Purpose | Layout selection, hero, intro, services showcase, video section, testimonials, trust strip, final call to action — the shared section inventory, no Features element; auto-save every 30 seconds (ticket 34, F-07; UF-17) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Content & Website → Pages → Homepage) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_homepage_editor`) | "Homepage", `headline-large` | `page-title` | — | `homepage.title` |
| Layout selector | `m3.ButtonGroup` (`bgrp_homepage_layout`, connected, single) | Classic, Services, Booking, Minimalist (`business_profile.homepage_template`) | — | mobile | `homepage.layout.classic`, `homepage.layout.services`, `homepage.layout.booking`, `homepage.layout.minimalist` |
| Hero section editor | `m3.Card` (`card_hero_editor`, outlined) | Headline, subheadline, hero image, call to action (UF-17 step 2) | `surface-card` | mobile | — |
| Hero fields | `m3.TextBox` (`txt_hero_headline`), `m3.TextBox` (`txt_hero_subheadline`), `m3.FileLoader` (`fu_hero_image`), `m3.TextBox` (`txt_hero_cta`) | The hero's fields | `outlined-input` | mobile | `homepage.hero.*` |
| Services showcase editor | `m3.Card` (`card_showcase_editor`, outlined) | On or off, number shown (1–6), display style (UF-17 step 4) | `surface-card` | mobile | — |
| Showcase switch | `m3.Switch` (`sw_showcase`) | On or off | — | — | `homepage.showcase` |
| Showcase count field | `m3.TextBox` (`txt_showcase_count`, type `number`, outlined) | 1–6 | `outlined-input` | — | `homepage.showcase.count` |
| Video editor | `m3.Card` (`card_video_editor`, outlined) | A video address (YouTube, Vimeo, Dailymotion or direct); the section hides if empty (UF-17 step 6) | `surface-card` | mobile | — |
| Video address field | `m3.TextBox` (`txt_home_video_url`) | The address (`business_profile.home_video_url`) | `outlined-input` | — | `profile.home_video` |
| Testimonials editor | `m3.Card` (`card_testimonials_editor`, outlined) | Approved reviews are offered automatically; typed-in testimonials (name, text, rating, photo); up to 10 in total; choose which appear (UF-17 step 5) | `surface-card` | mobile | — |
| Testimonial row | Anvil `FlowPanel` (`flow_testimonial_*`) | One testimonial per row: name, text, rating, photo, show switch | — | mobile | — |
| Show switch | `m3.Switch` (`sw_testimonial_show_*`) | Sets `show_on_homepage`; at most 10 reviews and testimonials together (ticket 34) | — | — | `homepage.testimonial.show` |
| Add testimonial button | `m3.Button` (`btn_add_testimonial`, outlined) | Types in a testimonial (marked as entered by the business) | `secondary-button` | — | `homepage.testimonial.add` |
| Trust strip editor | `m3.Card` (`card_trust_editor`, outlined) | The trust data points | `surface-card` | mobile | — |
| CTA editor | `m3.Card` (`card_cta_editor`, outlined) | Headline, subheadline, button text and link (UF-17 step 7) | `surface-card` | mobile | — |
| CTA fields | `m3.TextBox` (`txt_cta_headline`), `m3.TextBox` (`txt_cta_subheadline`), `m3.TextBox` (`txt_cta_button_text`), `m3.TextBox` (`txt_cta_button_link`) | The CTA's fields | `outlined-input` | mobile | `homepage.cta.*` |
| Save button | `m3.Button` (`btn_save_homepage`, filled) | Saves now (auto-save runs too) | `primary-button` | — | `homepage.save` |
| Auto-save | Anvil `Timer` (`tmr_homepage_save`, 30 s) | Changes auto-save every 30 seconds (UF-17) | — | — | — |
| Saved notice | `m3.Text` (`text_homepage_saved`, hidden) | The auto-save's notice | `body-small` | — | `homepage.autosaved` |
| Preview link | `m3.Link` (`lnk_preview_homepage`) | Opens the public home page | `tertiary-button` | — | `services.view_public` |
| Help icon | `components.HelpTooltipComponent` (help entry `public_pages.homepage_editor`) | The feature's tooltip and help video | — | — | `help.public_pages.homepage_editor.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Section configuration | `config` | `value` of `home_config` (key = `home_config`) |
| Layout choice | `business_profile` | `homepage_template` |
| Video | `business_profile` | `home_video_url`, `home_video_type` (type detected on save) |
| Testimonials | `testimonials` | `author_name`, `text`, `rating`, `photo`, `show_on_homepage` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the editor | `form_show` → `load_homepage` | `settings_service` home-config functions (`server_settings/settings_service`) | The `home_config` and the testimonials arrive |
| Auto-save | `tmr_homepage_save_tick` | the home-config functions | The configuration is stored; the live site updates at once (UF-17 step 8: Preview → Save → live immediately) |
| Save | `btn_save_homepage_click` | the home-config functions | The configuration is stored |
| Add a testimonial | `btn_add_testimonial_click` | `testimonial_service` save (`server_reviews/testimonial_service`) | The typed-in testimonial is stored |
| Toggle a testimonial | `sw_testimonial_show_*_change` | `testimonial_service` save | `show_on_homepage` is set; the tenth is refused with a message when ten are already shown |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the editor always shows its sections |
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

The editor is Owner-only (access matrix `public_pages.HomepageEditorForm`; ticket 34 B1).

## 7. Text

| Phrase key | English text |
|---|---|
| `homepage.title` | Homepage |
| `homepage.layout.classic` | Classic |
| `homepage.layout.services` | Services |
| `homepage.layout.booking` | Booking |
| `homepage.layout.minimalist` | Minimalist |
| `homepage.hero.headline` | Headline |
| `homepage.hero.subheadline` | Subheadline |
| `homepage.hero.image` | Hero image |
| `homepage.hero.cta` | Button text |
| `homepage.showcase` | Services showcase |
| `homepage.showcase.count` | Services shown |
| `homepage.video.address` | Video address |
| `homepage.testimonial.show` | Show on the homepage |
| `homepage.testimonial.add` | + Add testimonial |
| `homepage.cta.headline` | Headline |
| `homepage.cta.subheadline` | Subheadline |
| `homepage.cta.button_text` | Button text |
| `homepage.cta.button_link` | Button link |
| `homepage.save` | Save |
| `homepage.autosaved` | Saved. |
| `homepage.testimonial.limit` | Ten testimonials and reviews show at most. Unhide one first. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Customising your homepage | `master_template` help entry `public_pages.homepage_editor` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-17 Homepage customisation | The whole flow: layout, sections, testimonials, save |
| UF-27 Reviews and testimonials | The typed-in testimonials are added here |
