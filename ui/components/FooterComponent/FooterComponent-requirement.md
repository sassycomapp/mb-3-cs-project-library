---
title: "FooterComponent-requirement"
status: draft
doc-id: "mb-fd1820bf"
updated: "2026-11-25"
---
# FooterComponent — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `components.FooterComponent` |
| Purpose | The shared footer on every public page: logo, accreditations, social links, contact details, the policy links, the Cookie settings link, the referral badge and the optional Google Maps embed that loads only after cookie consent (ticket 05, F-06, F-37) |
| Screen type and width | Custom component (embedded at the bottom of every public page inside PublicLayout); its width is its host's |
| Layout | A component form built from theme components; identical on every public page (custom-component-requirements-matrix, FooterComponent) |
| Route | None — a component |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Footer container | Anvil `ColumnPanel` (`col_footer`) | Anvil canonical container; the footer's one column | — | never | — |
| Business logo | Anvil `Image` (`img_logo`) | The approved Anvil Image shows the logo from `business_profile.business_logo`; hidden when none is set | — | — | — |
| Accreditations text | `m3.Text` (`text_accreditations`) | The theme's Text; hidden when `business_profile.accreditations` is empty | `body-small` | — | — |
| Contact line | `m3.Text` (`text_contact`) | The contact email and phone from `business_profile` | `body-small` | — | — |
| Social links row | Anvil `FlowPanel` (`flow_social`) | One row of icon links that wraps on a phone | — | mobile | — |
| Facebook / Instagram / LinkedIn / X links | `m3.IconButton` (`ibtn_facebook`, `ibtn_instagram`, `ibtn_linkedin`, `ibtn_x`) | The theme's icon buttons open the business's social addresses from `business_profile.social_*`; each carries a name for screen readers (spec-ui-standards section 13.4) | `icon-button` | — | `footer.social.facebook`, `footer.social.instagram`, `footer.social.linkedin`, `footer.social.x` |
| Privacy Policy link | `m3.Link` (`lnk_privacy`) | Links to the published Privacy Policy page | — | — | `footer.policy.privacy` |
| Terms and Conditions link | `m3.Link` (`lnk_terms`) | Links to the published Terms and Conditions page | — | — | `footer.policy.terms` |
| Cookie Policy link | `m3.Link` (`lnk_cookie_policy`) | Links to the published Cookie Policy page | — | — | `footer.policy.cookie` |
| Cookie settings link | `m3.Link` (`lnk_cookie_settings`) | Opens the cookie banner again and shows the visitor's consent number (DESIGN.md Cookie Banner) | — | — | `footer.policy.cookie_settings` |
| Referral badge | Anvil `Image` (`img_referral_badge`, hidden) | Shown only while `business_profile.referral_badge_on` is True (F-37); the badge carries the client's referral link | — | — | `footer.referral.badge` |
| Report-abuse line | `m3.Text` (`text_abuse`) | The fixed line "Report abuse: abuse@mybizz.live", linked as an email address, in the footer's smallest text style; cannot be removed or edited by the client (DESIGN.md, Shared Section Inventory 8) | `body-small` | — | — |
| Map container | Anvil `ColumnPanel` (`col_map`) | Holds the Google Maps embed; hidden when `business_profile.google_maps_embed_url` is empty | — | mobile | — |
| Map placeholder | `m3.Card` (`card_map_consent`, outlined) | Before consent, the placeholder names Google Maps with a Load button (ticket 05; pol-legal-documents-and-consent) | `surface-card` | — | — |
| Map placeholder text | `m3.Text` (`text_map_placeholder`) | Names Google Maps | `body-small` | — | `footer.map.placeholder` |
| Map load button | `m3.Button` (`btn_map_load`, filled) | Loads the embed only after the visitor accepts cookies | `primary-button` | — | `footer.map.load` |
| Map embed | Anvil `ColumnPanel` (`col_map_embed`, hidden) | Holds the Google Maps iframe after consent (DESIGN.md JavaScript usage: approved uses; the embed is client content) | — | — | — |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Logo | `business_profile` | `business_logo` |
| Accreditations | `business_profile` | `accreditations` |
| Contact line | `business_profile` | `contact_email`, `phone` |
| Social links | `business_profile` | `social_facebook`, `social_instagram`, `social_linkedin`, `social_x` |
| Policy links | the published documents' pages | routes of the public pages |
| Referral badge | `business_profile` | `referral_badge_on` |
| Report-abuse line | — | fixed text, phrase key `footer.abuse.line` |
| Map | `business_profile` | `google_maps_embed_url` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the component's content | `form_show` → `load_footer` | `get_public_business_profile` (`server_settings/page_content_service`) | The intentionally public reader returns the public subset only (ticket 05) |
| Open a social address | `ibtn_<platform>_click` | — | The business's social address opens in a new tab |
| Load the map | `btn_map_load_click` | `record_consent` (`server_settings/policy_service`) | A `consents` row records the choice if none exists; the embed then loads (the map loads only after the visitor accepts cookies) |
| Reopen the cookie banner | `lnk_cookie_settings_click` | `record_consent` | The banner opens again and shows the visitor's consent number |
| Open the referral link | `img_referral_badge_click` | — | Mybizz's registration opens with the client's referral code filled in (F-37) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the footer always shows its contact and policy links |
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

The footer is on every public page; its reader returns the public subset and nothing else (ticket 05, B1).

## 7. Text

| Phrase key | English text |
|---|---|
| `footer.social.facebook` | Facebook |
| `footer.social.instagram` | Instagram |
| `footer.social.linkedin` | LinkedIn |
| `footer.social.x` | X |
| `footer.policy.privacy` | Privacy Policy |
| `footer.policy.terms` | Terms and Conditions |
| `footer.policy.cookie` | Cookie Policy |
| `footer.policy.cookie_settings` | Cookie settings |
| `footer.abuse.line` | Report abuse: abuse@mybizz.live |
| `footer.map.placeholder` | This map is provided by Google Maps and sets cookies. |
| `footer.map.load` | Load |
| `footer.referral.badge` | Refer a business — you both gain a discount |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The footer holds no process of its own; the cookie choice's video lives on PublicLayout |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-19 Contact form submission | The footer's contact details and map on the Contact page |
| UF-16 Content management and edit mode | The footer is fixed content — edit mode does not edit it |
| Cookie choice first | The map loads only after consent; the placeholder names Google Maps with a Load button |
| UF-46 Sharing the referral kit | The badge shows on the public website only while `referral_badge_on` is set |
