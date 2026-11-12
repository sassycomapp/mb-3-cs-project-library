---
title: "mb-3-cs-b11-app1-receipt"
document: Step 11 Receipt — mb-3-cs b11 app1
state: Generated from ledger steps row id 5; snapshot 2026-10-31
doc-id: "mb-93e783f5"
updated: "2026-11-12"
---

# Step Receipt — Step 11, Init (run app1, route application)

| Field | Value |
|---|---|
| Step | 11, Init (Phase B, sort 11) |
| Run / app | `app1` / App 1 |
| Type | `skill` (`opencode`) |
| Status | `complete` — started 2026-10-30 07:59 UTC, completed 2026-10-31 09:48 UTC |
| Synthesized | `false` |
| Artifact | `pdlf-outputs\b11\mb-3-cs-b11-app1-receipt.md` — this file |
| BFT | Full — all six questions recorded (bft_records, step_id 5) |
| Gate | None at Step 11 |
| Inputs used | Entry record (steps row id 4); b11-config/reference/test-spec; spec-execution-discipline; spec-bft; spec-security §3; ts-method; sop-health-check; test-suite templates; ts_local.sh |

## What Step 11 established for this run

- **Project library** made from the project template in the emptied `mb-3-cs-project-library\`, committed and pushed to `sassycomapp/mb-3-cs-project-library`, registered as the federated GBrain source `mb-3-cs-project-library` (51 pages), on the commit tools with the hook installed — all by the developer's confirmed actions.
- **Repositories verified and aligned**: app1, app2, app3, app5 each a git repository linked to `sassycomapp/mb-3-cs-app{n}`, holding `anvil.yaml`; `.gitignore` baseline applied to all four; App 1 and App 5 given `tests\` suites and `scripts\` per template, with App 1's suite completed (conftest.py merged with the legacy fixtures by approved deviation).
- **Users service**: App 1's settings aligned to spec-security §3 (2FA required, email confirmation required, sign-up off); Apps 2/3/5 deferred by developer decision (App 2 at Step 42 release; App 3 and App 5 in their own runs).
- **Test environment proven**: PR-09 passed 4/4 (server connection, client connection, TEST_ user signs in with second factor, mail catcher receives email) after five defects in `ts_local.sh` were fixed at source under developer grants (data dir creation, stop hardening, key-format revert, package-named app link with dependency cloning at anvil.yaml tags, Maildir skeleton).
- **Projects row** completed: library path, GBrain source, app paths, App 1 and App 5 Anvil addresses (Apps 2 and 3 have no address by design).
- **Context locked read-only**: `mb-3-cs-project-context` linked, federated, on both commit tools; the library was empty before the template copy.

## Tests (b11-test-spec) — evidence in `pdlf-outputs\b11\`

| File | Passed | Failed | Review | Skipped | Errors |
|---|---|---|---|---|---|
| mb-3-cs-b11-app1-checks.xml | 7 | 0 | 0 | 0 | 0 |
| mb-3-cs-b11-app1-pr09.xml | 4 | 0 | 0 | 0 | 0 |
| **Total** | **11** | **0** | **0** | **0** | **0** |

| Test | Result | Evidence |
|---|---|---|
| PR-01 | PASS | `mb-3-cs-b11-app1-checks.xml` — projects row, entry record, context checks, template-made library |
| ST-01 | PASS | `mb-3-cs-b11-app1-validate.json` — valid true, 251/251 files, 0 invalid |
| ST-12 | PASS | anvil.yaml users service line read in session (2FA on, confirmation on, sign-up off, remember-me 30 min, embedding off, role column present) |
| ST-10 | PASS | `.gitignore` baselines verified; App 1 and App 5 test suites complete; App 3 holds no code |
| PR-09 | PASS | `mb-3-cs-b11-app1-pr09.xml` — 4 tests, 0 failures |
| PR-06 | PASS | gitleaks over five repositories: app1/app2/app3/app5 = 0 findings; project-context = 8 candidates with engineer verdicts recorded (triage row 8); no agent commits; `.env` untracked everywhere |
| PR-04 | PASS | Sentinel ran at High scrutiny — triage row 9 (`sentinel-11-app1`, clear), all three matched security documents read in full |
| PR-02 | PASS | `mb-3-cs-b11-app1-health-check.md` — nine SOP checks, ALL PASS |

## Findings recorded for this step (triage_findings, step_id 5)

| id | finding_id | Label | State |
|---|---|---|---|
| 7 | b11-st01-landing-pages-init-syntax | ready-for-agent | Resolved — corrupted line fixed, validation re-run 251/251 |
| 8 | b11-pr06-context-secret-scan | ready-for-human | Resolved — engineer verdicts: OpenRouter key rotated months ago (dead); AWS tokens benign expired links; stripe placeholders benign |
| 9 | sentinel-11-app1 | clear | Resolved — Sentinel CLEAR, no concern found |

## Developer decisions recorded in this step

- Code graph = code files only; the 107 machine YAML files left to the next build; **Step 12 reads forms, tables and services directly from `anvil.yaml` and `form_template.yaml`, not from the graph**.
- Users service work deferred for App 2 (release), App 3 and App 5 (own runs); App 5's `.env` and PR-09 deferred to its first feature run.
- The OpenRouter key was **rotated by David months ago** (not revoked today); history not rewritten.
- Stale old names in App 1's `AGENTS.md` and its reference copy are corrected by the engineer at the end of Step 11 — the agent must not change them.

## STOP

Step 11 is complete. The run proceeds to **Step 12 (Phase B)** on your "continue": reading the named context `mb-3-cs-project-context` and defining the application concept. The run will pause after Step 16's `domain-locked` gate until Phase C is built — by design.
