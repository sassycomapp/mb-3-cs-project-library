---
title: "HomePage-requirement"
status: draft
doc-id: "mb-843f7b1b"
updated: "2026-11-25"
---
# HomePage — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `public_pages.HomePage` |
| Purpose | The public home page: the four template variants loaded by `business_profile.homepage_template`, the shared section inventory, the footer and the cookie banner (tickets 33 and 34, F-06, F-07) |
| Screen type and width | Public page — 900 px, full width: content sections |
| Layout | `layouts.PublicLayout` |
| Route | `/` — Anvil Routing (`@router.route`); `Route.meta` sets the title, description and Open Graph tags |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page container | Anvil `ColumnPanel` (`col_home`) | The page's one content column | — | mobile | — |
| Template host | Anvil `ColumnPanel` (`col_template`) | The local template chosen by `business_profile.homepage_template` (Classic, Services, Booking, Minimalist) | — | mobile | — |
| Hero | `components.ParallaxComponent` | The shared section inventory's hero (DESIGN.md Shared Section Inventory 1) | — | mobile | — |
| Services showcase | `m3.Carousel` (`car_services`, layout `hero`) + the service card grid | The theme's Carousel in the hero layout plus the service card grid (DESIGN.md Shared Section Inventory 2); the carousel advances on a Timer calling `next()`, with a pause/play `m3.IconButton`; rotation stops for good when the visitor moves it, and does not start under reduced motion | `surface-card` | mobile | — |
| Carousel timer | Anvil `Timer` (`tmr_carousel`, 6 s) | Advances the carousel every 6 seconds (architecture reference F-09) | — | — | — |
| Pause button | `m3.IconButton` (`ibtn_carousel_pause`, icon `mi:pause`) | The pause/play control | `icon-button` | — | `public.carousel.pause` |
| Intro section | `m3.Heading` + `m3.Text` | Short text section with heading and body (Shared Section Inventory 3) | `headline-small`, `body-large` | mobile | — |
| Video story | `components.VideoDisplayComponent` | Bound to `business_profile.home_video_url`; hidden when no URL is configured (Shared Section Inventory 4) | — | mobile | — |
| Testimonials | `m3.Card` set (`card_testimonial_*`, outlined) with `components.StarRatingComponent` in display mode | Testimonial cards with quote text and author details (Shared Section Inventory 5) | `surface-card` | mobile | — |
| Trust strip | `m3.Card` (`card_trust`, outlined) with `m3.Text` | Trust data points using the theme's text and card patterns (Shared Section Inventory 6) | `surface-card` | mobile | — |
| CTA band | `m3.Card` (`card_cta`, elevated) with `m3.Heading`, `m3.Text`, `m3.Button` | Headline, explanatory text, primary button, optional secondary action (Shared Section Inventory 7) | `elevated-card` | mobile | `public.cta.button` |
| Footer | `components.FooterComponent` | The shared footer across all public pages (Shared Section Inventory 8) | — | — | — |
| Edit mode host | `components.EditModeComponent` | Placed once on the page; visible only to a signed-in Owner or Manager (ticket 11) | — | — | `editmode.button` |
| Section visibility | component `visible` in code | Optional sections render only when configured: video (an address is set), services carousel (active services exist) (architecture reference F-07) | — | — | — |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Template choice | `business_profile` | `homepage_template` |
| Section content | `config` | `value` of `home_config` (written by the HomepageEditorForm; UF-17) |
| Video | `business_profile` | `home_video_url`, `home_video_type` |
| Testimonials | `reviews`, `testimonials` | approved rows with `show_on_homepage` set (at most 10 together) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the page | the route handler | `page_content_service` public read (`server_settings/page_content_service`) | The page's content arrives; `Route.meta` sets the share tags |
| Advance the carousel | `tmr_carousel_tick` | None | `next()` advances; the visitor moving the carousel stops rotation for good |
| Open the CTA | `btn_cta_click` | None | The booking flow or contact page opens, with the source carried (UF-20 funnel) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the home page always shows its configured sections |
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

The page is public; edit mode is Owner/Manager only.

## 7. Text

| Phrase key | English text |
|---|---|
| `public.carousel.pause` | Pause the slideshow |
| `public.carousel.play` | Play the slideshow |
| `public.cta.button` | Book an appointment |

*(The remaining texts are the client's own content, not phrase keys.)*


## 8. Help Videos

| Process | Help video |
|---|---|
| Editing your home page | `master_template` help entry `public_pages.home` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-05 Services appointment booking | The page's CTAs lead into the booking flow |
| UF-16 Content management and edit mode | The Owner edits the page in edit mode |
| UF-17 Homepage customisation | The editor configures the sections; the templates render them |
| UF-20 Lead capture | The funnel's start |
