---
title: "mb-3-cs-c17-app1-receipt"
document: Step 17 Receipt — mb-3-cs c17 app1
state: Generated from ledger steps row id 10; snapshot 2026-11-17
doc-id: "mb-e8cfe2e7"
updated: "2026-11-17"
---

# Step Receipt — mb-3-cs — Step 17 — app1

Phase: C   |   Type: skill   |   Status: complete
Started: 2026-11-17 10:28:31+00   |   Completed: 2026-11-17 11:56:34+00

## What was invoked

Skill family: anvil
Skill: none named singly — the step's skills and tools, each invoked with the
handover material and the context pack in full: `anvil-yaml` (the schema write
and ST-01), `secure-software-engineering` (the schema review against the threat
model), `wizard` (the Part A instructions), `mb-sentinel-review` (High), plus
`ts_scan.py`, `anvil --json validate`, and a verified comparison generator.
Inputs used: Step 16 (the glossary and adr-local — the vocabulary the schema
carries); Step 15 (the threat model, the access matrix and the reviewed plan —
the data design and the security standard); Steps 12 and 14 (the reviewed plan
and its data design in `design-doc.md`); the entry record of Step 03. The
authoritative schema and `schema-migration-log.md` are the project documents
this step checked and extended.

## Output

Artifact: `mb-3-cs-app1/anvil.yaml` — `db_schema` holds the whole authoritative
schema: 71 tables, 836 columns, every table `client: none` / `server: full`.
Beside it, in the step folder `pdlf-outputs/c17/`:

- `mb-3-cs-c17-app1-schema-diff.md` — the difference list: 234 differences
  (27 new tables; 161 added columns; 8 renames, all logged; 38 removals, all
  logged; 0 type changes), sorted by kind as the configuration sets out.
- `mb-3-cs-c17-app1-instructions.md` — Part A (the nine-stage wizard record:
  renames by hand, `payment_config` deletions, pull) and Part B (the five
  application instructions and the execution record).
- `mb-3-cs-c17-app1-authoritative-schema.md`, `mb-3-cs-c17-app1-anvil-db-schema.yaml`
  (the `db_schema` section, verified identical), `mb-3-cs-c17-app1-st01-validate.json`,
  `mb-3-cs-c17-app1-ts-scan.xml`, `mb-3-cs-c17-app1-checks.xml`.

Also written this step, by David's decisions recorded in the conversation: the
theme copies (`tests/component-inventory.yaml` byte-identical to the theme's;
the four theme keys of `tests/ts-config.yaml` aligned to the template's); the
migration log's 2026-11-17 application row; David's corrections at source — the
scanner's DD-13 now expects `activity_log` (project-library-global),
`authoritative-schema.md` §1.3 `config.created_at` moved into the column table,
and the design doc's Vault count corrected to 11.

Synthesized (test-mode shortcut): false

## Tests

Test evidence in `/mnt/c/dev/dev-mb-3-cs/mb-3-cs-project-library/pdlf-outputs/c17`

| File | Passed | Failed | Review | Skipped | Errors |
|---|---|---|---|---|---|
| mb-3-cs-c17-app1-checks.xml | 8 | 0 | 0 | 0 | 0 |
| mb-3-cs-c17-app1-ts-scan.xml | 37 | 1 | 2 | 5 | 0 |
| **Total** | **45** | **1** | **2** | **5** | **0** |

Not passed:
- REVIEW ts_scan::ST-07 No column holds a secret
- REVIEW ts_scan::ST-07 Times are datetimes in UTC
- FAIL ts_scan::ST-11 Forms are well-formed HTML templates

The two ST-07 review groups carry David's review note of 2026-11-17: the eight
flagged lines are confirmed correctly shaped (the two public page tokens, the
four `HH:MM` availability columns, the two referral on/off switches) — recorded
in the checks file's ST-07 case. The ST-11 failure is the app's 91 pre-PDLF
YAML form templates — outside Step 17's scope (the step's scanner tests are
ST-06 and ST-07); forms are rebuilt as HTML at the build steps. The checks
file's eight cases: PR-01, Theme-copies-current, ST-01, ST-06, ST-07,
Schema-match, Applied, PR-04 — all passed, each naming its evidence.

## BFT checkpoint

Level applied: light
Bones:   Hinge: `db_schema` equals the authoritative schema — 71 tables, 0
         differences of every kind — and the difference list traces all 234
         changes to their migration-log rows. Big bones: three parts — the
         authoritative schema verified against the data design and the threat
         model (secure-software-engineering; one finding, the Vault count,
         closed by David in the design doc); the difference list (27 new
         tables, 162 added columns, 8 renames, 38 removals, 0 type changes);
         the application — Part A by hand (6 renames, 3 `payment_config`
         deletions), the whole-schema write through the `anvil-yaml` skill, one
         Resolve pass. Small bones: the theme copies, both instruction records,
         the ST-01 and scanner evidence, the checks file, the migration-log
         application row, the deviations row, the ledger row. Intangible: the
         by-name check that ran after the direct Resolve instead of before —
         surfaced, recorded as deviation row 4, closed by computation (the
         37-column git diff and Anvil's no-mismatch), nothing unspoken left.
Health:  In itself: the app schema is exactly the law — ST-01 valid, ST-06 and
         ST-07 pass with the eight review items confirmed by David, and the
         live tables match by Anvil's own comparison. For the project: Phase C
         builds on a schema equal to its locked vocabulary and reviewed plan.
         The downstream consequence watched most: this schema is applied by
         hand to App 3 at Step 42 and later to up to 100 client instances —
         bounded by the migration discipline and the heartbeat's schema
         fingerprint. Carried: the ST-11 YAML-forms finding is pre-existing and
         belongs to the build steps; DD-04 stays skipped until Step 31/32 fills
         `core_tables`.

## Deviation

Description: Step 17 item 16 — the 37 schema removals were applied through
Resolve without the prescribed by-name check before applying. David applied
Resolve directly (push `1c40fc3`, then Sync Git Remotes, Resolve with Anvil's
"the schema of the source code is correct" option) and ordered verification
after the fact.
Options presented: A) author the Part B wizard and run the by-name check before
applying; B) apply Resolve directly and verify after the fact that the live
tables equal `db_schema` and no removal outside the 37 took place; C) stop the
step.
Decision: B — applied directly; the after-the-fact verification completed: the
committed `db_schema` equals the authoritative schema (0 differences), the
pre-write-to-committed column diff is exactly the 37 removals, and Anvil
reports no schema mismatch.

## Ledger row written

- `steps` row id 10: project_slug mb-3-cs, step_number 17, sort_order 17,
  phase C, run app1, app_number 1, step_type skill, skill_family anvil, status
  complete, synthesized false, artifact_path `mb-3-cs-app1/anvil.yaml`,
  started 2026-11-17 10:28:31 UTC, completed 2026-11-17 11:56:34 UTC,
  inputs_used naming Steps 16, 15, 12/14 and the entry record, gbrain_query and
  gbrain_results set, next_action and notes_for_next_session set, blockers NULL.
- `deviations` row id 4: the Part B application deviation, decision B, decided
  2026-11-17.
- `bft_records` row id 7: step_id 10, level light, bones and health answers,
  recorded 2026-11-17.
- `triage_findings` row id 14: finding_id sentinel-17-app1, source_step 17,
  triage_label clear, resolved_at set (Sentinel CLEAR at High; both matched
  documents read in full).
- `gates`: none — Step 17 has no gate.

## Input notices carried

- No Step 12–16 input was synthesized or skipped (`synthesized=false` on every
  row; no skipped status) — no taint notice is due.
- David's corrections made at source during this step, carried to every later
  reader: the test-suite scanner's DD-13 expects `activity_log` as the
  audit-trail table (project-library-global, 2026-11-17); `authoritative-schema.md`
  §1.3 carries `config.created_at` in the column table; the design doc states
  11 Vault secret names.

## Close-out

Please run `/mb-close-out` when you are ready.

## Blockers

None.

## Notes for next session

- The schema is applied and live: the Data Tables view reports no mismatch.
  The migration log's 2026-11-17 row records the application; the prompts
  P1–P14 and P13a are superseded as the application method by that row.
- Step 18 (Phase C, pull to local) is next; it reads the app schema as
  committed, the authoritative schema, the migration log and this receipt.
- ST-11's 91 YAML form templates are pre-existing (the app was partially built
  outside PDLF); the build steps rebuild forms as HTML. DD-04 stays skipped
  until Step 31/32 fills `ts-config` `core_tables`.
- The three canonical corrections made this step (scanner DD-13, the schema's
  §1.3, the design doc's Vault count) are David's changes at source — later
  steps read the corrected documents.

## Next action

Step 18 (Phase C, pull to local) is the next step of run app1.

---

Type "continue" to proceed to the next step, or raise anything you want changed first.
