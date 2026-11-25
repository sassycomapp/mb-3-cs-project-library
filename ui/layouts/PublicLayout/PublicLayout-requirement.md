---
title: "PublicLayout-requirement"
status: draft
doc-id: "mb-b20b4518"
updated: "2026-11-25"
---
# PublicLayout — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `layouts.PublicLayout` |
| Purpose | The public website's shell: the header with the site menu, the FooterComponent slot, and the cookie banner's home on every public page (ticket 01, F-06) |
| Screen type and width | Layout (shell) built on the theme's `m3.Layouts.NavigationDrawerLayout`; it hosts every public page, each of which is a Public page at 900 px (spec-ui-standards section 7) |
| Layout | `m3.Layouts.NavigationDrawerLayout` with the navigation drawer closed on desktop (the header links navigate) and the drawer available on a phone |
| Route | None — the routed public pages (`@router.route` of Anvil's Routing dependency) name PublicLayout as their layout |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| The shell | `m3.Layouts.NavigationDrawerLayout` | The theme's drawer layout; the public header is its app bar | — | — | — |
| Header title | `m3.Text` (`text_site_title`, bold) | The business's name from `business_profile.business_name`; set in code (client content, not a phrase key) | `title-medium` | — | — |
| Home link | `m3.Link` (`lnk_home`) | The header's site links are Links that route to the public addresses (architecture reference section 9: public pages use the Routing dependency) | `app-bar-link` | — | `public.nav.home` |
| About link | `m3.Link` (`lnk_about`) | Header link to `/about` | `app-bar-link` | — | `public.nav.about` |
| Services link | `m3.Link` (`lnk_services`) | Header link to `/services` | `app-bar-link` | — | `public.nav.services` |
| Blog link | `m3.Link` (`lnk_blog`) | Header link to `/blog`; visible only while `config.blog_enabled` | `app-bar-link` | — | `public.nav.blog` |
| Contact link | `m3.Link` (`lnk_contact`) | Header link to `/contact` | `app-bar-link` | — | `public.nav.contact` |
| Header links row | Anvil `FlowPanel` (`flow_header_links`) | The header links in one row that wraps on a phone | — | mobile | — |
| Help videos menu | `m3.IconButtonMenu` (`ibm_help_videos`, icon `mi:help_outline`) | The always-available help control; on a public page it lists the processes of the page's admin counterpart for signed-in team members and is hidden otherwise | `icon-button` | — | `layout.help.videos` |
| Cookie banner container | Anvil `ColumnPanel` (`col_cookie_banner`, hidden) | Sits at the bottom of the screen and never blocks the page (DESIGN.md Cookie Banner); built at ticket 41, hosted here and on the public pages | — | mobile | — |
| Banner text | `m3.Text` (`text_cookie_banner`) | The banner's message, from the translation module | `body-small` | — | `consent.banner.text` |
| Accept all button | `m3.Button` (`btn_accept_all`, filled) | The banner's buttons are the same size and style (DESIGN.md Cookie Banner) | `primary-button` | — | `consent.banner.accept_all` |
| Refuse all button | `m3.Button` (`btn_refuse_all`, outlined) | Same size and style as Accept all | `secondary-button` | — | `consent.banner.refuse_all` |
| More info link | `m3.Link` (`lnk_cookie_more`) | A text link to the Cookie Policy page | — | — | `consent.banner.more_info` |
| Footer slot | the layout's content bottom | `components.FooterComponent` sits at the bottom of every public page (ticket 05) | — | — | — |
| Content slot | the layout's `content` slot | The routed public page fills it | — | — | — |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Site title | `business_profile` | `business_name` (via `get_public_business_profile`) |
| Blog link visibility | `config` | `value` of `blog_enabled` (via `get_layout_context`) |
| Cookie banner visibility | `consents` | absence of a current `mb_consent` choice, or a consent older than 12 months or older than the published Cookie Policy version (ticket 41; DESIGN.md Cookie Banner) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Navigate to a public page | `lnk_<page>_click` | — | Anvil's Routing dependency opens the page's route; the header stays |
| Accept cookies | `btn_accept_all_click` | `record_consent` (`server_settings/policy_service`) | A `consents` row records the choice; the banner hides; embedded maps and videos may then load |
| Refuse cookies | `btn_refuse_all_click` | `record_consent` (`server_settings/policy_service`) | A `consents` row records the refusal; placeholders stay until the visitor accepts (spec-legal-documents-and-consent) |
| Open Cookie Policy | `lnk_cookie_more_click` | — | The Cookie Policy page opens |
| Show the cookie banner again | the footer's Cookie settings link (FooterComponent) | `record_consent` | The banner opens again and shows the visitor's consent number |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the layout is a shell; it always has the header and footer |
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

Every public page is reachable signed out; edit mode inside the pages is Owner/Manager only (the pages' own requirements).

## 7. Text

| Phrase key | English text |
|---|---|
| `public.nav.home` | Home |
| `public.nav.about` | About |
| `public.nav.services` | Services |
| `public.nav.blog` | Blog |
| `public.nav.contact` | Contact |
| `consent.banner.text` | We use cookies to show maps and videos. You choose what we may set. |
| `consent.banner.accept_all` | Accept all |
| `consent.banner.refuse_all` | Refuse all |
| `consent.banner.more_info` | More info |
| `layout.help.videos` | Help videos |

## 8. Help Videos

| Process | Help video |
|---|---|
| The cookie choice | `master_template` help entry `consent.banner` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-05–UF-10 Booking, orders and packages | The shell every public page sits on, from which the customer reaches the booking and order flows |
| UF-16 Content management and edit mode | The shell of the pages the Owner edits in edit mode |
| UF-19 Contact form submission | The shell of the Contact page |
| UF-20 Lead capture | The shell of the landing pages' links back into the site |
| UF-44 Trying the demo apps | The shell of the demo apps' public site |
| Cookie choice first | The banner shows on a visitor's first visit before any embedded map or video loads |
