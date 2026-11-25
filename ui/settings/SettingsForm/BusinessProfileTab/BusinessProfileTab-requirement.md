---
title: "BusinessProfileTab-requirement"
status: draft
doc-id: "mb-814f572d"
updated: "2026-11-25"
---
# BusinessProfileTab — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `settings.SettingsForm.BusinessProfileTab` |
| Purpose | The Settings → Business Profile tab: the business's name, description, contact details, timezone, app language, entity type, logo, social links, accreditations and the video addresses (ticket 18, F-05) |
| Screen type and width | Settings panel inside `settings.SettingsForm` (1000 px) |
| Layout | Hosted by `settings.SettingsForm` |
| Route | None — a local tab of the hub |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Tab panel container | Anvil `ColumnPanel` (`col_profile_tab`) | The tab's one column of fields | — | mobile | — |
| Fields grid | Anvil `GridPanel` (`grid_profile_fields`) | The fields in two columns that stack on a phone | — | mobile | — |
| Business name field | `m3.TextBox` (`txt_business_name`, outlined) | Required, 2–100 characters (the onboarding plan's validation) | `outlined-input` | — | `profile.name` |
| Description field | `m3.TextArea` (`txt_description`, outlined) | The business description; the AI helper writes website text from it | `outlined-input` | — | `profile.description` |
| Tagline field | `m3.TextBox` (`txt_tagline`, outlined) | The tagline | `outlined-input` | — | `profile.tagline` |
| Contact email field | `m3.TextBox` (`txt_contact_email`, type `email`, outlined) | Required, valid email | `outlined-input` | — | `profile.contact_email` |
| Phone field | `m3.TextBox` (`txt_phone`, type `tel`, outlined) | Required, E.164 format | `outlined-input` | — | `profile.phone` |
| Address fields | `m3.TextBox` (`txt_address_line_1`, `txt_address_line_2`, `txt_city`, `txt_country`, `txt_postal_code`) | The address shown on invoices | `outlined-input` | — | `profile.address_1`, `profile.address_2`, `profile.city`, `profile.country`, `profile.postal_code` |
| Timezone selector | `m3.DropdownMenu` (`dd_timezone`) | Required, valid IANA timezone; the value the datetimes display in (adr-timezone-utc-storage-display-conversion) | `outlined-select` | — | `profile.timezone` |
| App language selector | `m3.DropdownMenu` (`dd_app_language`) | The languages whose translation is complete (F-25); a change reloads the application in the new language | `outlined-select` | — | `profile.app_language` |
| Entity type selector | `m3.RadioGroupPanel` (`rg_entity`) with `m3.RadioButton` (`rb_individual`, `rb_business`) | `individual` / `business` | — | mobile | `profile.entity.individual`, `profile.entity.business` |
| Logo upload | `m3.FileLoader` (`fu_logo`) | The admin-area and invoice logo (`business_profile.logo`) | — | — | `profile.logo` |
| Public logo upload | `m3.FileLoader` (`fu_business_logo`) | The public website's logo (`business_profile.business_logo`) | — | — | `profile.business_logo` |
| Social link fields | `m3.TextBox` (`txt_social_facebook`, `txt_social_instagram`, `txt_social_linkedin`, `txt_social_x`) | The footer's social links | `outlined-input` | — | `profile.social.facebook`, `profile.social.instagram`, `profile.social.linkedin`, `profile.social.x` |
| Accreditations field | `m3.TextBox` (`txt_accreditations`) | The footer's accreditations | `outlined-input` | — | `profile.accreditations` |
| Home video address field | `m3.TextBox` (`txt_home_video_url`) | The homepage video; empty hides the section; the type is detected on save | `outlined-input` | — | `profile.home_video` |
| Services video address field | `m3.TextBox` (`txt_services_video_url`) | The services page video; empty hides the section | `outlined-input` | — | `profile.services_video` |
| Google Maps address field | `m3.TextBox` (`txt_maps_url`) | The map on Contact and in the footer; empty hides the map | `outlined-input` | — | `profile.maps` |
| AI helper trigger | `components.AIHelperComponent` (trigger) | The bio/profile writer assist drafts the description and the first website text from the profile (F-26; onboarding step 3) | — | — | `ai.open` |
| Save button | `m3.Button` (`btn_save_profile`, filled) | The tab's one filled action | `primary-button` | — | `profile.save` |
| Saved confirmation | `m3.Snackbar` (Python class, in code) | Confirms the save | — | — | `profile.saved` |
| Help icon | `components.HelpTooltipComponent` (help entry `settings.business_profile`) | The tab's tooltip and help video | — | — | `help.settings.business_profile.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| All fields | `business_profile` | `business_name`, `description`, `tagline`, `contact_email`, `phone`, `address_line_1`, `address_line_2`, `city`, `country`, `postal_code`, `timezone`, `app_language`, `entity_type`, `logo`, `business_logo`, `social_facebook`, `social_instagram`, `social_linkedin`, `social_x`, `accreditations`, `home_video_url`, `home_video_type`, `services_video_url`, `services_video_type`, `google_maps_embed_url` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the tab | `form_show` → `load_profile` | `settings_service` read (`server_settings/settings_service`) | The profile's values fill the fields |
| Save | `btn_save_profile_click` | `settings_service` save (`server_settings/settings_service`) | The profile is saved; validations enforced (name 2–100, valid email, E.164 phone, valid IANA timezone); the video types are detected on save; the snackbar confirms |
| AI text | the AI trigger | `request(assist="bio", ...)` (`server_ai/ai_request_layer`) | The description and website text drafts appear for acceptance |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the tab always shows its fields (provisioning creates the row) |
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

Per `access-matrix.yaml` `settings_tabs.BusinessProfile`.

## 7. Text

| Phrase key | English text |
|---|---|
| `profile.name` | Business name |
| `profile.description` | Description |
| `profile.tagline` | Tagline |
| `profile.contact_email` | Contact email |
| `profile.phone` | Phone |
| `profile.address_1` | Address line 1 |
| `profile.address_2` | Address line 2 |
| `profile.city` | City |
| `profile.country` | Country |
| `profile.postal_code` | Postal code |
| `profile.timezone` | Time zone |
| `profile.app_language` | App language |
| `profile.entity.individual` | Individual |
| `profile.entity.business` | Business |
| `profile.logo` | Logo (admin and invoices) |
| `profile.business_logo` | Logo (public website) |
| `profile.social.facebook` | Facebook page |
| `profile.social.instagram` | Instagram |
| `profile.social.linkedin` | LinkedIn |
| `profile.social.x` | X |
| `profile.accreditations` | Accreditations |
| `profile.home_video` | Homepage video address |
| `profile.services_video` | Services page video address |
| `profile.maps` | Google Maps embed address |
| `profile.save` | Save |
| `profile.saved` | Business profile saved. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Filling in the business profile | `master_template` help entry `settings.business_profile` — played in the app language |
| Choosing the app language | `master_template` help entry `settings.app_language` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-02 Owner onboarding | The wizard's step 3 links here; the AI helper offers the first website text from the profile |
| UF-16 Content management | The profile feeds the public pages' fixed content |
| UF-44 Trying the demo apps | The demo Owner edits the profile; the nightly reset restores it |
