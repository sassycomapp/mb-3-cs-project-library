---
title: "mb-3-cs-c17-app1-schema-diff"
updated: "2026-11-17"
doc-id: "mb-0047a810"
---
# Step 17 — Schema Difference List (run app1)

Every difference between `docs-standard-local/authoritative-schema.md` (71 tables) and the `db_schema` of `mb-3-cs-app1/anvil.yaml` (44 tables), sorted by kind as the Step 17 configuration sets out: additions, renames, removals, type changes. Every rename and removal carries its `docs-local/schema-migration-log.md` row. Generated from the two documents by comparison, table by table and column by column.

| Kind | Count |
|---|---|
| Added tables | 27 |
| Added columns on existing tables | 161 |
| Renames (all logged) | 8 |
| Removals (all logged) | 38 |
| Type changes | 0 |
| **Total** | 234 |

## 1. Additions — new tables (27)

Created by the migration prompts P1–P5, P8–P10, P12, P13 and P13a. Every column of each new table is an addition; the full column list is the authoritative schema's own section for the table.

- `sample_rows`: `table_name` (string), `row_id` (string), `created_at` (datetime)
- `resources`: `name` (string), `resource_type` (string), `description` (string), `is_active` (bool), `created_at` (datetime), `updated_at` (datetime), `metadata` (simpleObject)
- `group_sessions`: `service_id` (link -> services), `provider_id` (link -> users), `resource_id` (link -> resources), `start_datetime` (datetime), `end_datetime` (datetime), `place_limit` (number), `places_taken` (number), `location` (string), `call_link` (string), `status` (string), `created_at` (datetime), `updated_at` (datetime), `metadata` (simpleObject)
- `recurring_series`: `contact_id` (link -> contacts), `service_id` (link -> services), `provider_id` (link -> users), `pattern` (string), `start_date` (date), `end_date` (date), `status` (string), `created_at` (datetime), `updated_at` (datetime), `metadata` (simpleObject)
- `orders`: `order_number` (string), `contact_id` (link -> contacts), `service_id` (link -> services), `provider_id` (link -> users), `brief` (string), `brief_files` (links -> files), `status` (string), `total_price` (number), `payment_option` (string), `payment_status` (string), `delivery_message` (string), `delivery_files` (links -> files), `ordered_at` (datetime), `started_at` (datetime), `delivered_at` (datetime), `created_at` (datetime), `updated_at` (datetime), `metadata` (simpleObject)
- `packages`: `name` (string), `description` (string), `service_id` (link -> services), `session_count` (number), `price` (number), `expiry_days` (number), `is_active` (bool), `created_at` (datetime), `updated_at` (datetime), `metadata` (simpleObject)
- `customer_packages`: `package_id` (link -> packages), `contact_id` (link -> contacts), `sessions_total` (number), `sessions_remaining` (number), `purchased_at` (datetime), `expires_at` (datetime), `expiry_notice_sent_at` (datetime), `status` (string), `created_at` (datetime), `updated_at` (datetime), `metadata` (simpleObject)
- `payments`: `kind` (string), `invoice_id` (link -> invoice), `booking_id` (link -> bookings), `order_id` (link -> orders), `customer_package_id` (link -> customer_packages), `contact_id` (link -> contacts), `method` (string), `amount` (number), `currency` (string), `status` (string), `gateway_reference` (string), `gateway_fee` (number), `refund_of` (link -> payments), `recorded_by` (link -> users), `received_at` (datetime), `notes` (string), `created_at` (datetime), `updated_at` (datetime), `metadata` (simpleObject)
- `expenses`: `expense_id` (string), `expense_date` (date), `supplier` (string), `category_id` (link -> expense_categories), `description` (string), `amount` (number), `tax_amount` (number), `currency` (string), `payment_method` (string), `receipt_r2_key` (string), `status` (string), `recorded_by` (link -> users), `notes` (string), `created_at` (datetime), `updated_at` (datetime), `metadata` (simpleObject)
- `expense_categories`: `name` (string), `description` (string), `is_active` (bool), `created_at` (datetime), `updated_at` (datetime)
- `automation_rules`: `name` (string), `trigger_event` (string), `conditions` (simpleObject), `actions` (simpleObject), `is_active` (bool), `last_run_at` (datetime), `created_at` (datetime), `updated_at` (datetime)
- `automation_runs`: `automation_rule_id` (link -> automation_rules), `trigger_reference` (string), `contact_id` (link -> contacts), `result` (string), `detail` (simpleObject), `ran_at` (datetime), `created_at` (datetime)
- `companies`: `company_id` (string), `name` (string), `website` (string), `phone` (string), `email` (string), `address` (simpleObject), `tax_number` (string), `industry` (string), `notes` (string), `created_at` (datetime), `updated_at` (datetime), `metadata` (simpleObject)
- `deals`: `deal_id` (string), `title` (string), `contact_id` (link -> contacts), `company_id` (link -> companies), `owner_id` (link -> users), `stage` (string), `value` (number), `expected_close_date` (date), `quote_id` (link -> quotes), `lost_reason` (string), `won_at` (datetime), `lost_at` (datetime), `created_at` (datetime), `updated_at` (datetime), `metadata` (simpleObject)
- `quotes`: `quote_id` (string), `quote_number` (string), `contact_id` (link -> contacts), `company_id` (link -> companies), `deal_id` (link -> deals), `issue_date` (datetime), `valid_until` (datetime), `subtotal` (number), `tax_rate` (number), `tax_amount` (number), `total_amount` (number), `currency` (string), `status` (string), `accept_token` (string), `accepted_at` (datetime), `declined_at` (datetime), `invoice_id` (link -> invoice), `pdf_url` (string), `notes` (string), `created_at` (datetime), `updated_at` (datetime), `metadata` (simpleObject)
- `quote_items`: `quote_id` (link -> quotes), `service_id` (link -> services), `description` (string), `quantity` (number), `unit_price` (number), `amount` (number), `sort_order` (number), `created_at` (datetime), `updated_at` (datetime), `metadata` (simpleObject)
- `email_addresses`: `address` (string), `kind` (string), `display_name` (string), `member_ids` (links -> users), `signature` (string), `auto_reply_text` (string), `auto_reply_from` (datetime), `auto_reply_until` (datetime), `is_active` (bool), `created_at` (datetime), `updated_at` (datetime), `metadata` (simpleObject)
- `email_messages`: `address_id` (link -> email_addresses), `folder` (string), `direction` (string), `from_address` (string), `from_name` (string), `to_addresses` (simpleObject), `cc_addresses` (simpleObject), `subject` (string), `body_text` (string), `body_html` (string), `attachments` (simpleObject), `message_id` (string), `in_reply_to` (string), `thread_id` (string), `contact_id` (link -> contacts), `is_read` (bool), `is_starred` (bool), `spam_verdict` (string), `virus_verdict` (string), `dmarc_verdict` (string), `r2_key` (string), `received_at` (datetime), `sent_at` (datetime), `deleted_at` (datetime), `created_at` (datetime), `updated_at` (datetime), `metadata` (simpleObject)
- `email_suppressions`: `email` (string), `reason` (string), `scope` (string), `created_at` (datetime)
- `cookie_policy`: `category` (string), `title` (string), `clause` (string), `sort_order` (number), `is_seeded` (bool), `is_active` (bool), `feature_key` (string), `created_at` (datetime), `updated_at` (datetime), `metadata` (simpleObject)
- `consents`: `consent_id` (string), `document` (string), `choice` (string), `policy_version` (number), `contact_id` (link -> contacts), `created_at` (datetime)
- `reviews`: `contact_id` (link -> contacts), `booking_id` (link -> bookings), `provider_id` (link -> users), `service_id` (link -> services), `request_token` (string), `rating` (number), `comment` (string), `status` (string), `show_on_homepage` (bool), `requested_at` (datetime), `submitted_at` (datetime), `approved_at` (datetime), `created_at` (datetime), `updated_at` (datetime), `metadata` (simpleObject)
- `testimonials`: `author_name` (string), `text` (string), `rating` (number), `photo` (media), `show_on_homepage` (bool), `sort_order` (number), `created_at` (datetime), `updated_at` (datetime), `metadata` (simpleObject)
- `plan_summary`: `plan` (string), `renews_on` (date), `currency` (string), `lines` (simpleObject), `total` (string), `pending` (simpleObject), `reserve` (number), `referral_link` (string), `created_at` (datetime), `updated_at` (datetime)
- `notifications`: `user_id` (link -> users), `source` (string), `type` (string), `priority` (string), `title` (string), `body` (string), `data` (simpleObject), `is_read` (bool), `created_at` (datetime), `expires_at` (datetime), `idempotency_key` (string), `updated_at` (datetime)
- `user_preferences`: `user_id` (link -> users), `channel` (string), `type` (string), `enabled` (bool), `quiet_from` (string), `quiet_to` (string), `freq_cap` (number), `created_at` (datetime), `updated_at` (datetime)
- `delivery_logs`: `notification_id` (link -> notifications), `channel` (string), `status` (string), `attempts` (number), `error` (string), `created_at` (datetime), `updated_at` (datetime)

## 2. Additions — columns on existing tables (161)

Added by prompts P6, P7, P11, P13a and the folded amendment of 2026-11-06 (`created_at`, `updated_at`, `metadata`).

### `availability_exceptions`

| Column | Type |
|---|---|
| `created_at` | datetime |
| `updated_at` | datetime |

### `availability_rules`

| Column | Type |
|---|---|
| `created_at` | datetime |
| `updated_at` | datetime |

### `blog_categories`

| Column | Type |
|---|---|
| `created_at` | datetime |
| `updated_at` | datetime |

### `blog_comments`

| Column | Type |
|---|---|
| `metadata` | simpleObject |

### `blog_posts`

| Column | Type |
|---|---|
| `metadata` | simpleObject |
| `scheduled_at` | datetime |
| `share_captions` | simpleObject |
| `tags` | simpleObject |

### `booking_metadata_schemas`

| Column | Type |
|---|---|
| `created_at` | datetime |
| `updated_at` | datetime |

### `bookings`

| Column | Type |
|---|---|
| `call_link` | string |
| `cancelled_at` | datetime |
| `customer_address` | simpleObject |
| `customer_package_id` | link -> customer_packages |
| `deposit_amount` | number |
| `group_session_id` | link -> group_sessions |
| `hold_expires_at` | datetime |
| `meeting_type` | string |
| `payment_option` | string |
| `places` | number |
| `recurring_series_id` | link -> recurring_series |
| `reminder_1h_sent_at` | datetime |
| `reminder_24h_sent_at` | datetime |
| `resource_id` | link -> resources |
| `source` | string |
| `travel_after_minutes` | number |
| `travel_before_minutes` | number |

### `business_profile`

| Column | Type |
|---|---|
| `app_language` | string |
| `cancellation_window_hours` | number |
| `default_payment_terms_days` | number |
| `home_video_type` | string |
| `home_video_url` | string |
| `metadata` | simpleObject |
| `referral_badge_on` | bool |
| `referral_banner_in_emails` | bool |
| `referral_popup_on` | bool |
| `services_video_type` | string |
| `services_video_url` | string |
| `tax_label` | string |
| `tax_rate` | number |
| `tax_registration_number` | string |
| `trade` | string |

### `campaign_metrics`

| Column | Type |
|---|---|
| `emails_sent` | number |
| `unsubscribes` | number |
| `updated_at` | datetime |

### `contact_campaigns`

| Column | Type |
|---|---|
| `created_at` | datetime |
| `updated_at` | datetime |

### `contact_counter`

| Column | Type |
|---|---|
| `created_at` | datetime |
| `updated_at` | datetime |

### `contact_events`

| Column | Type |
|---|---|
| `created_at` | datetime |

### `contact_submissions`

| Column | Type |
|---|---|
| `created_at` | datetime |
| `metadata` | simpleObject |
| `updated_at` | datetime |

### `contacts`

| Column | Type |
|---|---|
| `address` | simpleObject |
| `company_id` | link -> companies |
| `email_opt_out` | bool |
| `metadata` | simpleObject |

### `daily_summary`

| Column | Type |
|---|---|
| `updated_at` | datetime |

### `email_campaigns`

| Column | Type |
|---|---|
| `from_address_id` | link -> email_addresses |
| `metadata` | simpleObject |
| `segment_id` | link -> segments |
| `unsubscribes` | number |
| `updated_at` | datetime |

### `email_config`

| Column | Type |
|---|---|
| `created_at` | datetime |
| `default_from_address_id` | link -> email_addresses |
| `dkim_verified` | bool |
| `dns_verified` | bool |
| `domain` | string |
| `mail_from_verified` | bool |
| `mx_verified` | bool |
| `ses_tenant` | string |
| `updated_at` | datetime |
| `verified_at` | datetime |

### `email_log`

| Column | Type |
|---|---|
| `created_at` | datetime |
| `ses_message_id` | string |
| `updated_at` | datetime |

### `email_templates`

| Column | Type |
|---|---|
| `created_at` | datetime |
| `language` | string |
| `updated_at` | datetime |

### `error_log`

| Column | Type |
|---|---|
| `updated_at` | datetime |

### `files`

| Column | Type |
|---|---|
| `created_at` | datetime |
| `metadata` | simpleObject |
| `updated_at` | datetime |

### `invoice`

| Column | Type |
|---|---|
| `amount_paid` | number |
| `booking_id` | link -> bookings |
| `customer_package_id` | link -> customer_packages |
| `metadata` | simpleObject |
| `order_id` | link -> orders |
| `overdue_reminder_sent_at` | datetime |
| `payment_terms_days` | number |
| `sent_at` | datetime |
| `subtotal` | number |
| `tax_amount` | number |
| `tax_rate` | number |
| `updated_at` | datetime |

### `invoice_items`

| Column | Type |
|---|---|
| `created_at` | datetime |
| `metadata` | simpleObject |
| `sort_order` | number |
| `time_entry_id` | link -> time_entries |
| `updated_at` | datetime |

### `landing_pages`

| Column | Type |
|---|---|
| `metadata` | simpleObject |

### `lead_captures`

| Column | Type |
|---|---|
| `created_at` | datetime |
| `updated_at` | datetime |

### `leads`

| Column | Type |
|---|---|
| `created_at` | datetime |
| `metadata` | simpleObject |
| `updated_at` | datetime |

### `pages`

| Column | Type |
|---|---|
| `metadata` | simpleObject |
| `updated_by` | link -> users |

### `payment_config`

| Column | Type |
|---|---|
| `created_at` | datetime |
| `paypal_client_id` | string |
| `paypal_connected` | bool |
| `paypal_connected_at` | datetime |
| `paypal_test_mode` | bool |
| `updated_at` | datetime |

### `privacy_policy`

| Column | Type |
|---|---|
| `metadata` | simpleObject |

### `provider_performance`

| Column | Type |
|---|---|
| `updated_at` | datetime |

### `rate_limits`

| Column | Type |
|---|---|
| `created_at` | datetime |
| `updated_at` | datetime |

### `segments`

| Column | Type |
|---|---|
| `updated_at` | datetime |

### `service_performance`

| Column | Type |
|---|---|
| `completed_count` | number |
| `updated_at` | datetime |

### `services`

| Column | Type |
|---|---|
| `deposit_amount` | number |
| `hourly_rate` | number |
| `intake_schema_id` | link -> booking_metadata_schemas |
| `is_group` | bool |
| `meeting_types` | simpleObject |
| `metadata` | simpleObject |
| `payment_option` | string |
| `payment_terms_days` | number |
| `providers` | links -> users |
| `required_resources` | links -> resources |
| `slug` | string |
| `travel_buffer_minutes` | number |
| `updated_at` | datetime |
| `video_type` | string |

### `t_and_c`

| Column | Type |
|---|---|
| `metadata` | simpleObject |

### `tasks`

| Column | Type |
|---|---|
| `assigned_to` | link -> users |
| `metadata` | simpleObject |
| `source` | string |
| `updated_at` | datetime |

### `theme_config`

| Column | Type |
|---|---|
| `created_at` | datetime |

### `time_entries`

| Column | Type |
|---|---|
| `invoice_id` | link -> invoice |
| `metadata` | simpleObject |
| `updated_at` | datetime |

### `users`

| Column | Type |
|---|---|
| `contact_id` | link -> contacts |
| `full_name` | string |
| `is_demo_visitor` | bool |
| `is_provider` | bool |
| `metadata` | simpleObject |
| `photo` | media |
| `preferences` | simpleObject |
| `specialisations` | string |
| `updated_at` | datetime |

### `webhook_log`

| Column | Type |
|---|---|
| `created_at` | datetime |
| `event_id` | string |
| `processed_at` | datetime |
| `r2_key` | string |
| `updated_at` | datetime |

## 3. Renames (8) — all carried by the migration log

Applied **by David, by hand in the Data Tables view (Part A), before any prompt**, except the two `created_date` replacements, which prompt P7 carries (added and removed in the same prompt; nothing reads `created_date`). A rename keeps the column's data.

| Table | Old name | New name | Applied by |
|---|---|---|---|
| `error_log` | `tomestamp` | `timestamp` | Part A |
| `blog_comments` | `comment` | `comment_text` | Part A |
| `availability_rules` | `resource_id` | `provider_id` | Part A |
| `availability_exceptions` | `resource_id` | `provider_id` | Part A |
| `bookings` | `resource_id` | `provider_id` | Part A |
| `time_entries` | `staff_id` | `provider_id` | Part A |
| `email_campaigns` | `created_date` | `created_at` | P7 |
| `segments` | `created_date` | `created_at` | P7 |

The `bookings` case is the one where the old name is reused: `resource_id` is first renamed `provider_id` (Part A, its data kept), then a **new** `resource_id` (link to `resources`) is added by prompt P6.

## 4. Removals (38) — all carried by the migration log

Applied through `anvil.yaml` and Resolve, each checked by name by David before it is applied (a removal deletes that column's data). P14 runs **last**. Every row below names its log row.

### `blog_categories` (log row: P14)

| Column | Current type |
|---|---|
| `client_id` | link -> users |

### `blog_comments` (log row: P14)

| Column | Current type |
|---|---|
| `instance_id` | link -> users |

### `blog_posts` (log row: P14)

| Column | Current type |
|---|---|
| `client_id` | link -> users |

### `bookings` (log row: P14)

| Column | Current type |
|---|---|
| `payment_id` | string |

### `campaign_metrics` (log row: P14)

| Column | Current type |
|---|---|
| `instance_id` | link -> users |

### `contact_submissions` (log row: P14)

| Column | Current type |
|---|---|
| `client_id` | link -> users |

### `contacts` (log row: P14)

| Column | Current type |
|---|---|
| `instance_id` | link -> users |

### `daily_summary` (log row: P14)

| Column | Current type |
|---|---|
| `instance_id` | link -> users |

### `email_campaigns` (log row: P14)

| Column | Current type |
|---|---|
| `instance_id` | link -> users |

### `email_config` (log row: P14)

| Column | Current type |
|---|---|
| `configured` | bool |
| `configured_at` | datetime |
| `from_email` | string |
| `from_name` | string |
| `sender_domain_verified` | bool |

### `email_templates` (log row: P14)

| Column | Current type |
|---|---|
| `client_id` | link -> users |

### `health_log` (log row: P14)

| Column | Current type |
|---|---|
| `instance_id` | link -> users |

### `invoice` (log row: P14)

| Column | Current type |
|---|---|
| `payment_id` | string |
| `payment_method` | string |

### `landing_pages` (log row: P14)

| Column | Current type |
|---|---|
| `client_id` | link -> users |
| `instance_id` | link -> users |

### `lead_captures` (log row: P14)

| Column | Current type |
|---|---|
| `instance_id` | link -> users |

### `leads` (log row: P14)

| Column | Current type |
|---|---|
| `instance_id` | link -> users |

### `payment_config` (log row: the 2026-11-10 row (by hand, Part A))

| Column | Current type |
|---|---|
| `active_gateway` | string |
| `paystack_connected` | bool |
| `paystack_connected_at` | datetime |

### `privacy_policy` (log row: P14)

| Column | Current type |
|---|---|
| `instance_id` | link -> users |

### `provider_performance` (log row: P14)

| Column | Current type |
|---|---|
| `instance_id` | link -> users |

### `segments` (log row: P14)

| Column | Current type |
|---|---|
| `instance_id` | link -> users |

### `service_performance` (log row: P14)

| Column | Current type |
|---|---|
| `instance_id` | link -> users |

### `services` (log row: P14)

| Column | Current type |
|---|---|
| `meeting_type` | string |
| `provider_id` | link -> users |

### `t_and_c` (log row: P14)

| Column | Current type |
|---|---|
| `instance_id` | link -> users |

### `tasks` (log row: P14)

| Column | Current type |
|---|---|
| `instance_id` | link -> users |

### `theme_config` (log row: P14)

| Column | Current type |
|---|---|
| `accent_color` | string |
| `font_family` | string |
| `primary_color` | string |

### `webhook_log` (log row: P14)

| Column | Current type |
|---|---|
| `payload` | simpleObject |
| `signature` | string |

## 5. Type changes

None. The one candidate, `bookings.resource_id` (the app's link to `users` against the authoritative's link to `resources`), is resolved by the migration log as a **rename** (`resource_id` to `provider_id`, Part A) followed by the P6 addition of the new `resource_id` (link to `resources`) — recorded in sections 2 and 3, not here.

## 6. Order of application

Per `schema-migration-log.md`: Part A (the six renames by hand, and the `payment_config` removals by hand) first; then prompts P1–P13 in order, then P13a; then P14 (the removals) **last**; then the Part C check — `db_schema` against the authoritative schema, table by table and column by column.

*End of the Step 17 schema difference list for run app1.*
