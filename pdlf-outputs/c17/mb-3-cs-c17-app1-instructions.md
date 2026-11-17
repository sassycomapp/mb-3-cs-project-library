---
title: "mb-3-cs-c17-app1-instructions"
updated: "2026-11-17"
doc-id: "mb-0854e03b"
---
# Step 17 — David's Instructions (run app1): Part A, by hand

Written with the `wizard` skill. The interactive walker is
`/tmp/opencode/mb-3-cs-c17-app1-part-a-wizard.sh` (nine stages, one task per
screen, confirm at every stage; run it with `bash /tmp/opencode/mb-3-cs-c17-app1-part-a-wizard.sh`
in a WSL terminal). This document is the durable record of the same instructions.

**When:** now, before any prompt (the migration log's Part A). Everything here
happens in the Anvil editor, Data Tables view, of `mb-3-cs-app1`. A rename made
by hand in the Data Tables view keeps the column's data; a rename made in
`anvil.yaml` would apply as a removal and an addition and delete it. That is why
this part is by hand.

## Stage 1 — Open the Anvil editor

1. Sign in at anvil.works and open the app `mb-3-cs-app1` (Edit app).
2. Open the **Data Tables** view (the table icon in the editor's sidebar).

## Stage 2 — Rename 1/6: `error_log`

| Table | Old column | New column |
|---|---|---|
| `error_log` | `tomestamp` | `timestamp` |

## Stage 3 — Rename 2/6: `blog_comments`

| Table | Old column | New column |
|---|---|---|
| `blog_comments` | `comment` | `comment_text` |

## Stage 4 — Rename 3/6: `availability_rules`

| Table | Old column | New column |
|---|---|---|
| `availability_rules` | `resource_id` | `provider_id` |

The name `resource_id` reappears later in this table as a link to `resources` —
through the `anvil.yaml` write (prompt P6), not by hand now.

## Stage 5 — Rename 4/6: `availability_exceptions`

| Table | Old column | New column |
|---|---|---|
| `availability_exceptions` | `resource_id` | `provider_id` |

## Stage 6 — Rename 5/6: `bookings`

| Table | Old column | New column |
|---|---|---|
| `bookings` | `resource_id` | `provider_id` |

Do **not** create a new `resource_id` column now. A new `resource_id` (link to
`resources`) arrives later through prompt P6, after `anvil.yaml` is written.
Renaming only renames the column and keeps its data.

## Stage 7 — Rename 6/6: `time_entries`

| Table | Old column | New column |
|---|---|---|
| `time_entries` | `staff_id` | `provider_id` |

## Stage 8 — Delete three columns from `payment_config`

Per the migration log's 2026-11-10 row, these are deleted **by hand (Part A)**,
before any prompt. App 1 holds development data only; the data in these columns
is not needed. Stripe is the card gateway and PayPal is optional; a customer
chooses between them at checkout.

1. Delete `active_gateway`
2. Delete `paystack_connected`
3. Delete `paystack_connected_at`

Each deletion is confirmed by name inside the wizard before the next runs.

## Stage 9 — Pull to local

Anvil has committed the editor's work to GitHub. Pull it to the local copy so
the agent can check `anvil.yaml` against the renames:

```bash
cd /mnt/c/dev/dev-mb-3-cs/mb-3-cs-app1 && git pull
```

After the pull, the agent checks that `anvil.yaml` now shows every rename
(reference item 11), then writes the whole authoritative schema into
`anvil.yaml` (section 5).

---

# Step 17 — David's Instructions (run app1): Part B, applying the schema

The five instructions given for applying the written schema (reference item 16).
The wizard was not authored: David applied the schema directly, as recorded in
**Execution** below.

## Instruction 1 — Commit and push

```bash
cd /mnt/c/dev/dev-mb-3-cs/mb-3-cs-app1 && git pull && gcp
```

This commits the schema write and the two test copies (`tests/ts-config.yaml`,
`tests/component-inventory.yaml`). Pushed as `1c40fc3 "Update"`.

## Instruction 2 — Open the editor's Data Tables view

Open `mb-3-cs-app1` in the Anvil editor; the schema-mismatch notice and the
**Resolve** button appear in the Data Tables view.

## Instruction 3 — Resolve: apply the additions

The 27 new tables and the 164 added columns on existing tables, applied as
Resolve presents them. Additions carry no data risk.

## Instruction 4 — Resolve: the 37 removals, checked by name

Every removal below is to be verified by name against Resolve's list before it
is applied. A removal deletes that column's data. **Any difference Resolve
shows that is not on this list is a deviation: stop and report it.** There are
no type changes. The three `payment_config` columns are already gone by hand;
if Resolve still shows them, that is a difference the list does not hold.

| Table | Columns to remove |
|---|---|
| `blog_categories` | `client_id` |
| `blog_comments` | `instance_id` |
| `blog_posts` | `client_id` |
| `bookings` | `payment_id` |
| `campaign_metrics` | `instance_id` |
| `contact_submissions` | `client_id` |
| `contacts` | `instance_id` |
| `daily_summary` | `instance_id` |
| `email_campaigns` | `instance_id`, `created_date` (its `created_at` is added in the same Resolve, per P7) |
| `email_config` | `configured`, `configured_at`, `from_email`, `from_name`, `sender_domain_verified` |
| `email_templates` | `client_id` |
| `health_log` | `instance_id` |
| `invoice` | `payment_id`, `payment_method` |
| `landing_pages` | `client_id`, `instance_id` |
| `lead_captures` | `instance_id` |
| `leads` | `instance_id` |
| `privacy_policy` | `instance_id` |
| `provider_performance` | `instance_id` |
| `segments` | `instance_id`, `created_date` (its `created_at` is added in the same Resolve, per P7) |
| `service_performance` | `instance_id` |
| `services` | `meeting_type`, `provider_id` |
| `t_and_c` | `instance_id` |
| `tasks` | `instance_id` |
| `theme_config` | `primary_color`, `accent_color`, `font_family` |
| `webhook_log` | `payload`, `signature` |

## Instruction 5 — Confirm

The Data Tables view reports no schema mismatch.

## Execution record

David applied the schema directly: `git pull && gcp` (push `1c40fc3`), Sync Git
Remotes, then Resolve with Anvil's "The schema of the source code is correct"
option. **The by-name check of the 37 removals was not done before applying**
— a deviation from reference item 16, David's decision, recorded in the
ledger's `deviations` table (row id 4).

**Verified after the fact**, by the best means available:

1. The committed `db_schema` equals the authoritative schema — the comparison
   is empty, table by table and column by column (71 tables both sides, 0
   differences of every kind).
2. The columns present in the pre-write commit (`5924469`) and absent in the
   committed schema (`1c40fc3`) are exactly the 37 removals above, name by
   name — no removal outside the list took place. The columns added on
   existing tables are the difference list's 161, plus `config.created_at`
   (from the §1.3 correction) and the two `created_at` halves of the P7
   rename rows — all on the list.
3. The live tables equal `db_schema` by Anvil's own comparison: Resolve was
   applied with "The schema of the source code is correct" and the Data Tables
   view reports no schema mismatch. A removal outside the 37 would leave the
   live tables missing a column `db_schema` holds and the banner would show.

Why the check ran after Resolve rather than before: David chose to apply
directly and ordered the after-the-fact verification instead; the verification
above is that order carried out. The risk the by-name check guards — a removal
outside the list deleting data — is excluded by items 2 and 3.

*End of the Step 17 Part B instructions and execution record for run app1.*

*End of the Step 17 Part A instructions for run app1.*
