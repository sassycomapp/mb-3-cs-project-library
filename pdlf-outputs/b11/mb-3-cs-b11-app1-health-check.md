---
title: "mb-3-cs-b11-app1-health-check"
doc-id: "mb-f4e2ff84"
updated: "2026-10-31"
---
# Health Check — mb-3-cs, Step 11 self-audit (run app1)

Run per `sop-health-check`, 2026-10-31. One line per check, failures with reasons.

| # | Check | Result | Detail |
|---|---|---|---|
| 1 | Ledger reachable, seven tables exist | PASS | Database `pdlf` on port 5433 answers; tables: gates, deviations, isc, bft_records, triage_findings, projects, steps |
| 2 | Business row matches the disk | PASS | All five recorded paths exist; GBrain sources `mb-3-cs-project-library` (51 pages, federated) and `mb-3-cs-project-context` (303 pages, federated) are registered |
| 3 | Each app folder is a linked Anvil app | PASS | app1, app2, app3, app5: each a git repository, origin `https://github.com/sassycomapp/mb-3-cs-app{n}`, each holds `anvil.yaml` |
| 4 | Each app folder is up to date | PASS | After `git fetch origin`, all four report `master...origin/master` with no behind marker |
| 5 | Each app folder keeps secrets out | PASS | Each `.gitignore` holds the baseline: 2 env-exclusion lines and 3 cache-exclusion lines (env file, env pattern, credentials-equivalents, `__pycache__`, `.pytest_cache`, `.venv`) |
| 6 | Project library is sound | PASS | Git repository linked to `https://github.com/sassycomapp/mb-3-cs-project-library`; all 26 template folders present (none missing); `pdlf-tickets\` present; `pdlf-outputs\` holds one folder per step (51 folders) |
| 7 | Project library registered and committed | PASS | Registered as the federated GBrain source; pre-commit hook installed (mb-doc-check present); on the `commit-all` list and in `mb-doc-check.sources` |
| 8 | Matt Pocock configuration is right | PASS | `docs\agents\domain.md` points to `docs-local\GLOSSARY.md` and `adr-local\`, as the fixed answers require |
| 9 | Anvil command-line tool answers | PASS | `anvil --version` → 0.7.7 |

ALL PASS
