# Mybizz CS — Revised Anvil Work List

**Date:** 2026-06-03
**Source:** Eng-review alignment report + component classification correction
**Purpose:** Exact items to create/edit in Anvil Designer to bring mb-3-cs in line with reconciled planning state
**Scaffold-spec:** `docs/scaffold-spec.md` (v2 — updated with corrected component classification)

---

## CLIENT CODE — 6 new forms to CREATE

| # | Package | Form | Type | Phase |
|---|---------|------|------|-------|
| 1 DONE| `onboarding` | `OnboardingForm` | Form | 1 |
| 2 DONE| `bookings` | `BookingViewerForm` | Form | 2 |
| 3 Done| `landing_pages` | `LandingPageListForm` | Form | 3 |
| 4 Done| `landing_pages` | `LandingPageEditorForm` | Form | 3 |
| 5 Done| `settings` | `PolicyEditorForm` | Form | 4 |
| 6 Done| `blog` | `CommentModerationForm` | Form | 4 |

---

## CLIENT CODE — 5 shared custom components to CREATE

| # | Package | Component | Used in | Phase |
|---|---------|-----------|---------|-------|
| 7 Done| `components` | `ClauseBuilder` | PolicyEditorForm (privacy_policy + t_and_c) | 4 |
| 8 Done| `components` | `FooterComponent` | All public pages (6+) | 4 |
| 9 Done | `components` | `VideoDisplayComponent` | BlogPostForm, HomePage | 4 |
| 10 Done| `components` | `TimeLapseCarouselComponent` | HomePage, ServicesForm | 4 |
| 11 Done| `components` | `ParallaxComponent` | HomePage, multiple public pages | 4 |

---

## SERVER CODE — 3 modules to CREATE

| # | Package | Module | Purpose | Phase |
|---|---------|--------|---------|-------|
| 1 Done| `server_blog` | `comment_service` | Blog comment moderation | 4 |
| 2 Done| `server_shared` | `policy_service` | Policy clause CRUD (privacy_policy, t_and_c) | 4 |
| 3 Done| `server_shared` | `export_service` | GDPR/POPIA data export | 5 |


## DATA TABLES — 5 to CREATE

### 1. `privacy_policy` [Phase 4]

| Column | Anvil Type | Notes |
|--------|-----------|-------|
| `instance_id` | link_single → users | Client identifier |
| `category` | string | e.g., "Data Collection", "Cookies", "Third Parties" |
| `title` | string | Clause heading |
| `clause` | string | Body text |
| `sort_order` | number | Display order within category |
| `is_seeded` | bool | True if from master template |
| `is_active` | bool | Whether clause is published |
| `created_at` | datetime | UTC |
| `updated_at` | datetime | UTC |

### 2. `t_and_c` [Phase 4]

| Column | Anvil Type | Notes |
|--------|-----------|-------|
| `instance_id` | link_single → users | Client identifier |
| `category` | string | e.g., "Service Terms", "Payment Terms", "Liability" |
| `title` | string | Clause heading |
| `clause` | string | Body text |
| `sort_order` | number | Display order within category |
| `is_seeded` | bool | True if from master template |
| `is_active` | bool | Whether clause is published |
| `created_at` | datetime | UTC |
| `updated_at` | datetime | UTC |

### 3. `blog_comments` [Phase 4]

| Column | Anvil Type | Notes |
|--------|-----------|-------|
| `instance_id` | link_single → users | Client identifier |
| `post_id` | link_single → blog_posts | Parent post |
| `commenter_name` | string | Required |
| `commenter_email` | string | Required |
| `comment_text` | string | Required |
| `status` | string | "pending", "approved", "spam", "deleted" |
| `created_at` | datetime | UTC |
| `updated_at` | datetime | UTC |

### 4. `invoice_items` [Phase 2]

| Column | Anvil Type | Notes |
|--------|-----------|-------|
| `invoice_id` | link_single → invoice | Parent invoice |
| `description` | string | Line item description |
| `quantity` | number | Quantity |
| `unit_price` | number | Price per unit |
| `amount` | number | quantity x unit_price |

### 5. `time_entries` [Phase 2 — deferred to V2]

| Column | Anvil Type | Notes |
|--------|-----------|-------|
| `entry_id` | string | Unique ID |
| `contact_id` | link_single → contacts | Client contact |
| `staff_id` | link_single → users | Provider |
| `service_id` | link_single → services | Related service |
| `booking_id` | link_single → bookings | Optional |
| `start_time` | datetime | UTC |
| `end_time` | datetime | UTC |
| `duration_hours` | number | Calculated |
| `hourly_rate` | number | Rate |
| `total_amount` | number | duration x hourly_rate |
| `description` | string | Notes |
| `is_billable` | bool | Billable flag |
| `is_invoiced` | bool | Invoiced flag |
| `created_at` | datetime | UTC |


## DATA TABLES — 2 to EDIT

### 1. `business_profile` — ADD 18 columns

**Onboarding fields (11):**

| # | Column | Anvil Type | Notes |
|---|--------|-----------|-------|
| 1 | `onboarding_status` | string | "in_progress" or "completed" |
| 2 | `onboarding_completed` | bool | Overall completion flag |
| 3 | `onboarding_completed_at` | datetime | UTC |
| 4 | `entity_type` | string | "Individual" or "Business" |
| 5 | `palette` | string | Selected brand palette identifier |
| 6 | `legal_compliance_enabled` | bool | Irreversible once True |
| 7 | `legal_compliance_enabled_at` | datetime | UTC |
| 8 | `legal_ack_accepted` | bool | Legal responsibility acknowledgement |
| 9 | `legal_ack_accepted_at` | datetime | UTC |
| 10 | `data_governance_ack_accepted` | bool | Data governance acknowledgement |
| 11 | `data_governance_ack_accepted_at` | datetime | UTC |

**Design fields (7):**

| # | Column | Anvil Type | Notes |
|---|--------|-----------|-------|
| 12 | `palette_updated_at` | datetime | When palette last changed |
| 13 | `blog_list_view` | string | "tiled" or "list" |
| 14 | `enable_comments` | bool | Blog comments toggle |
| 15 | `homepage_template` | string | "classic", "services", "booking", "minimalist" |
| 16 | `business_logo` | media | Alternative logo for public pages |
| 17 | `accreditations` | string | Business accreditations text |
| 18 | `google_maps_embed_url` | string | Google Maps embed URL |
---
********** START HERE *******************
---
### 2. `blog_posts` — ADD 5 columns

| # | Column | Anvil Type | Notes |
|---|--------|-----------|-------|
| 1 | `template` | string | "classic", "two_column", "full_width" |
| 2 | `meta_title` | string | SEO title override |
| 3 | `meta_description` | string | SEO meta description |
| 4 | `keywords` | string | SEO keywords |
| 5 | `pullquote` | string | Featured pullquote for full-width template |

---

## LAYOUT VARIANTS — 13 to build INTO parent forms (not separate)

| # | Variant | Build into | Approach |
|---|---------|-----------|----------|
| 1 | PaletteSettingsForm | SettingsForm Palette tab | Content section directly in the tab |
| 2 | BlogListTiledTemplate | BlogListForm | Swappable content panel, toggled by view mode |
| 3 | BlogListListTemplate | BlogListForm | Swappable content panel, toggled by view mode |
| 4 | BlogPostClassicTemplate | BlogPostForm | Swappable content panel, selected by `post.template` |
| 5 | BlogPostTwoColumnTemplate | BlogPostForm | Swappable content panel, selected by `post.template` |
| 6 | BlogPostFullWidthTemplate | BlogPostForm | Swappable content panel, selected by `post.template` |
| 7 | HomePageClassicTemplate | HomePage | Swappable content panel, selected by `homepage_template` |
| 8 | HomePageServicesTemplate | HomePage | Swappable content panel, selected by `homepage_template` |
| 9 | HomePageBookingTemplate | HomePage | Swappable content panel, selected by `homepage_template` |
| 10 | HomePageMinimalistTemplate | HomePage | Swappable content panel, selected by `homepage_template` |
| 11 | LeadCaptureTemplate | LandingPage | Swappable content panel, selected by `template_type` |
| 12 | EventRegistrationTemplate | LandingPage | Swappable content panel, selected by `template_type` |
| 13 | VslTemplate | LandingPage | Swappable content panel, selected by `template_type` |

---

## SUMMARY

| Action | Count |
|--------|-------|
| New client forms | 6 |
| New shared custom components | 5 |
| New server modules | 3 |
| New tables | 5 |
| Tables to edit | 2 (+23 columns) |
| Layout variants (build into parent forms) | 13 |
| **Total new items to create in Anvil** | **19** (6 forms + 5 components + 3 modules + 5 tables) |

---

*End of revised work list — 2026-06-03*
