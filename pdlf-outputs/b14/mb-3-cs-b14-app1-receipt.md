---
title: "mb-3-cs-b14-app1-receipt"
document: Step 14 Receipt — mb-3-cs b14 app1
state: Generated from ledger steps row id 7; snapshot 2026-11-04
doc-id: "mb-3594e2e2"
updated: "2026-11-04"
---

# Step Receipt — Step 14, CEO Review (run app1, route application)

| Field | Value |
|---|---|
| Step | 14, CEO review (Phase B, sort 14) |
| Run / app | `app1` / App 1 |
| Type | `skill` (`gstack`) |
| Status | `complete` — started 2026-11-03 16:16 UTC, completed 2026-11-04 06:59 UTC |
| Synthesized | `false` |
| Artifact | `docs-local\design\app1\design-doc.md` — the reviewed plan, amended in place, GStack's report last |
| BFT | Full — all six answers in the section BFT (Full) below (`bft_records`, step_id 7, row id 4) |
| Gate | None at Step 14; the run pauses after Step 16's `domain-locked` gate |
| Inputs used | Step 12 (steps row id 6); b14-config/reference/test-spec; spec-execution-discipline; spec-bft; spec-skill-context-pack; spec-artifact-handover; spec-postgres-ledger; spec-pdlf-process-tests; ts-method, ts-catalogue, ts-conformance-checks; the step-receipt template; `adr-timezone-utc-storage-display-conversion`; the canonical corpus read in full (mb-3-cs-project-context 33 files; project-library-global including the five security-global matches and the two new cookie documents `pol-cookies-and-consent` and `spec-cookie-consent-system`; the project-library skeletons); skills: `secure-software-engineering` (requirements phase), `/plan-ceo-review` (HOLD SCOPE, 11 sections), `mb-sentinel-review` (Medium) |

## What Step 14 established for this run

- **The five security-data-law sections written** into the design doc after Canonical Sources (numbering 16–20, as David approved): **§16 Data Inventory** — 13 kinds of data, each with owner, readers, writers, protection level and retention; **§17 Regulatory Position** — POPIA applies (registration after launch; rights implemented), GDPR conditional per client's actual base and rechecked yearly on the same rights machinery, PCI DSS SAQ A applies with the annual SAQ A and the payment-page script rule; **§18 Security Requirements** — assets, actors, harms, what must never leak, and 16 testable acceptance criteria AC-S1 to AC-S16 each naming its catalogue test; **§19 Client Exit** — what the exit export writes to the client's bucket and the handover; **§20 Plan Fit** — a typical client at roughly 350,000–450,000 rows after three years against the 1,000,000-row and 100 GB limits, the 80% warnings, and the test account cost recorded.
- **`/plan-ceo-review` ran in HOLD SCOPE** (David, D1; the skill's >15-files rule was named and set aside with its reason — the documents fix the scope). Step 0 evidence: 6 premises with receipts, unverified=4 tracked with owners; reality rows 6 pass + 1 n/a; no approach decision needed. Sections 1–11 evaluated at strategy depth; 6 diagrams produced; 0 stale diagrams.
- **Five findings, every one decided by David and applied in place:**
  - **D2** — the demo apps share the development account, the single point of failure; the funnel consequence is now named in the §11 risk row.
  - **D3** — the private test client's hosting account is a 101st Business account outside the margin arithmetic; recorded in §20; SC-5 unchanged.
  - **D4** — the webhook reconciliation task (required by the test spec 2.3 and the API spec §2.5) was missing from the background-task schedule; recorded at the test spec's cadence, every 5 minutes, plus D8's nightly consent archive to `consents/{year}/{month}/`; `spec-api-specification` §2.5's hourly wording is brought into line after Step 14.
  - **D5** — the late-payment rule: a payment succeeding for a released or cancelled booking is recorded; reinstated and confirmed when the slot and resource are still free, refunded in full through the original gateway when not; the customer and the Owner notified either way. **Sentinel FLAG `sentinel-14-app1`** found the reinstatement's race; David amended the rule: the reinstatement re-checks slot, resource and places **inside the same transaction** that sets the booking to `confirmed`, as SR-16 requires for `confirm_booking` — never a prior read-check alone; re-check failure refunds in full and notifies both. The concurrency test (late-payment webhook vs a new booking for the same slot; exactly one succeeds) is in What Must Be Tested. Triage row 11 resolved `ready-for-agent` with David's decision.
  - **D6** — `pol-cookies-and-consent` (self-hosted help videos) conflicts with `pol-anvil-platform` (YouTube explainer videos); David confirmed self-hosted and fixed the storage home: once per language in a Mybizz-owned R2 bucket published at `media.mybizz.live`, direct video addresses, never `theme/assets/`, never an outside video service, no cookie-setting Cloudflare feature on the host; the clients' F-09 videos stay embedded, consent-gated; `pol-anvil-platform.md` and `pdlf-build-plan.md` are aligned after Step 14.
- **D8 (David, before the review) and its Step 14 extension:** the Cookie Policy is the Clause Builder's third document; extended into **one legal-documents system** covering the Cookie Policy, the Privacy Policy and the Terms and Conditions — every publish raises the document's version and saves a copy to R2 at `legal/{document}/v{version}.html`; customers accept the Terms and Conditions and the Privacy Policy with an unticked box at booking, order, package purchase, online payment, customer portal sign-up and quote acceptance; contact and lead-capture forms link to the Privacy Policy; one `consents` table (consent number, document, choice, version, contact for terms and privacy only, time); the schema stays at **70 tables**; retention: cookie records 2 years, terms and privacy records 7 years. Both new documents (`pol-cookies-and-consent`, `spec-cookie-consent-system`) were read in full and are law; the design doc's counts, groups and Canonical Sources were brought to 70 tables, 6 global policies, 26 global specifications, with `cookie_policy` and `consents` in the Content and policy group (9).
- **§21 Scope and §22 What Must Be Tested** written after the review: goals, non-goals and the review's NOT-in-scope reference; 16 rows of critical flows, roles and edge cases each with its why and its tests.
- **The review's terminal report** is the design doc's last section: CEO review CLEAR, 0 unresolved decisions, 0 critical gaps; Outside Voice declined by the PDLF fixed answer; the final line is the exact unbolded `NO UNRESOLVED DECISIONS`.

## Tests (b14-test-spec) — evidence in `pdlf-outputs\b14\`

| Test | Result | Evidence |
|---|---|---|
| PR-01 | PASS | `mb-3-cs-b14-app1-checks.xml` — entry record complete (steps row id 4); Step 12 complete with `synthesized=false` and its design doc approved (steps row id 6, b12 receipt) |
| DC-03 | PASS | `mb-3-cs-b14-app1-checks.xml` — §21 Scope goals/non-goals + NOT in scope reasons; §16 inventory complete for all 13 kinds; §17 decides POPIA, GDPR, PCI SAQ A; §19 names the export; §20 shows a typical client within the limits |
| DC-01 | PASS | `mb-3-cs-b14-app1-checks.xml` — AC-S1–AC-S16 and the D-decisions each a single testable statement with its named test; §22 names a test for every row |
| PR-04 | PASS | `mb-3-cs-b14-app1-checks.xml` — Sentinel at Medium; four security-global documents read in full; FLAG `sentinel-14-app1` (triage row 11) resolved by David's recorded decision; the row exists with `resolved_at` set |
| PR-06 | PASS | `mb-3-cs-b14-app1-gitleaks.json` — 6 commits scanned in history and the working tree; 0 leaks; no agent committed |

Test evidence in `pdlf-outputs/b14` (ts_report.py output, unchanged):

| File | Passed | Failed | Review | Skipped | Errors |
|---|---|---|---|---|---|
| mb-3-cs-b14-app1-checks.xml | 5 | 0 | 0 | 0 | 0 |
| **Total** | **5** | **0** | **0** | **0** | **0** |

## BFT (Full)

The six answers of `bft_records` row id 4, recorded 2026-11-04 07:00 UTC, as `spec-bft` sets them out.

**BONES — Structure.** Hinge elements: the reviewed design doc with its seven new sections — 16 to 22 — and the terminal GSTACK REVIEW REPORT last; the decisions D8 extended, D2, D3, D4, D5 and D6 recorded and applied in place. Big bones: the five secure-software-engineering sections — Data Inventory with 13 kinds each carrying owner, readers, writers, protection level and retention; Regulatory Position deciding POPIA, GDPR conditional and PCI DSS SAQ A plus the legal-documents system; Security Requirements with 16 testable criteria each naming its catalogue test; Client Exit naming the export; Plan Fit with the typical-client estimate and the 80 percent warnings. The HOLD SCOPE review across 11 sections: 5 findings, every one resolved by a recorded David decision. Small bones: the checks file 5 of 5 passed, the gitleaks report 0 findings over history and worktree, the step-folder copy, the GStack copy, triage row 11 resolved ready-for-agent, the decision ledger in the review body, the six diagrams. Intangible bone: the record stays honest about the unverified — 4 premises tracked with owners — and about the two tool failures; nothing was papered over.

**MUSCLE — Force.** Channel muscle dominant: secure-software-engineering at its requirements phase; the plan-ceo-review skill through Step 0 and all 11 sections; the Sentinel skill over four security-global documents read in full; the ledger bridge; gitleaks over history and worktree; ts_report. project-library-global files read and applied by name this step: spec-security-architecture, spec-regulatory-compliance-baseline, adr-client-r2-storage-and-exit-handover, spec-anvil-platform-standards, pol-common-aspects-for-mybizz-anvil-applications, spec-security, spec-vault-system, pol-security-and-data-governance, adr-dependency-based-not-multi-tenant, adr-anvil-platform-constraints, pol-anvil-first-development, spec-five-app-architecture-model, spec-m3-design-standards, adr-client-data-management-rights-and-mybizz-retention-boundary, adr-tiers-model, adr-no-free-trial, adr-payment-security-boundary-vault, spec-integration, spec-api-specification, adr-mybizz-management-visibility, adr-timezone-utc-storage-display-conversion, pol-cookies-and-consent, spec-cookie-consent-system, ts-method, ts-catalogue, ts-conformance-checks, and the five security-global matches sec-multi-tenant-assumption-leakage, sec-payment-manipulation, sec-business-logic-race-conditions, sec-injection-unsafe-input, sec-requirements-third-party-integration. Information muscle: the corpus read in full this session, not from the design doc summaries. Logic muscle: the D5 race was found by applying the race-condition document's prohibited pattern to the rule as written; the D4 gap by cross-checking two documents' requirements against the background-task schedule. Value muscle: David amended his own D5 rule on the Sentinel finding rather than defending it, and reshaped the consent record into one system rather than two.

**NERVES — Connections.** Achieving: the chain held in order — dependencies checked, row opened, corpus read, GBrain searched at each task, sections written, review run with per-finding decisions, checks recorded, approval, Sentinel, gitleaks, copies, ledger, receipt. Sequencing mattered: the Cookie Policy decision preceded the sections' final form; the Sentinel finding preceded the receipt so the receipt carries its resolution. Exploring was bounded by HOLD SCOPE — findings raised, never expansions. Organizing: the five sections, the review body and the report reference each other without duplication; the carries T1 to T4 name their owners.

**FAT — Waste.** Cut: the expansion-mode ceremonies — 0F, 0H and the CEO archive — skipped by the HOLD SCOPE route table, not by shortcut. Cut: the next-review offer and the docs/designs promotion, both by fixed answer. Cut: a separate decision number for the D8 extension, folded under D4 as David directed. Kept: the full Error and Rescue and Failure Modes registries — they are the review's hinge at strategy depth; the reality rows — they make the reading checkable rather than claimed.

**SKIN — Presentation.** The receipt is the ledger row formatted, never composed separately. The design doc names documents instead of restating them, except where the step configuration assigns content — sections 16 to 20 carry the facts the tests require. The checks file names its evidence per ts-method section 5. The report's terminal line is the exact unbolded NO UNRESOLVED DECISIONS. The three denials this step — two lapsed grant windows and one guard word match — are recorded verbatim, not softened.

**HEALTH — Is it healthy?** Good in itself: the reviewed plan is approved with every finding resolved, 0 unresolved decisions, 0 critical gaps, 5 of 5 tests with evidence, the Sentinel FLAG resolved by the developer amendment. Good for the project: Step 15 receives a plan whose data, law, exit and plan-fit are settled and testable, with the carries named. The downstream consequence watched most closely: the run pauses after Step 16 gate domain-locked until Phase C is built — by design, reported, never worked around. Second: the post-Step-14 carries — if any is missed the design doc leads the documents and the drift the Step 12 receipt warned about grows; the notes below name every one.

## Findings recorded for this step (triage_findings, step_id 7)

| id | finding_id | Label | State |
|---|---|---|---|
| 11 | sentinel-14-app1 | ready-for-agent | Resolved — Sentinel FLAG (High/High): the D5 reinstatement was not bound to a transactional re-check; David approved the amendment (reinstatement inside the same transaction as SR-16; re-check failure refunds in full and notifies both); applied to the design doc; resolved_by David, resolved_at set |

## Ledger rows written

- `steps` row id 7: project_slug mb-3-cs, step_number 14, sort_order 14, phase B, run app1, app_number 1, step_type skill, skill_family gstack, skill_name NULL, status complete, synthesized false, artifact_path [[[mb-3-cs-project-library:docs-local/design/app1/design-doc]]](C:/dev/dev-mb-3-cs/mb-3-cs-project-library/docs-local/design/app1/design-doc.md), started 2026-11-03 16:16:14 UTC, completed 2026-11-04 06:59:42 UTC, gbrain_query and gbrain_results set, next_action and notes_for_next_session set, blockers NULL.
- `bft_records` row id 4: step_id 7, level full, recorded 2026-11-04 07:00:00 UTC, all six answers.
- `triage_findings` row id 11: finding_id sentinel-14-app1, source_step 14, needs-triage → ready-for-agent, resolved_by David, resolved_at set.

## Deviation

None — no deviation row was written; the three denials (two lapsed grant windows, one command-guard word match on payload text) were resolved by David's re-grants and by rewording, with no scope or method change.

## Blockers

None.

## Notes for next session

- **Carries after Step 14 (Claude, outside the run):** `authoritative-schema.md` gains `cookie_policy` and `consents` (70 tables); `schema-migration-log.md` gains the prompt for the two tables; `integration-architecture.md`'s schedule gains the webhook reconciliation task (every 5 minutes, per test spec 2.3) and the nightly consent archive task; `spec-api-specification` §2.5's hourly reconciliation wording is brought into line; `DESIGN.md`'s booking status lifecycle gains the D5 edge (`cancelled → confirmed` through the late-payment path only, inside the transactional re-check); `pol-anvil-platform.md` and `pdlf-build-plan.md` are aligned with `pol-cookies-and-consent` on help-video hosting; the legal-documents global policy and specification are written (one system, three documents, `consents`, the retention split).
- **Step 15 carries into the threat model and test plan:** the D5 late-payment rule with its transactional reinstatement and the concurrency test; the consent endpoints and the archive task; the AI/tool crossings; `access-matrix.yaml` (IN-02); the `media.mybizz.live` cookie-freedom check at release.
- **David's business actions (T4):** AC-6 to Anvil's sales team; AC-14 to Amazon for SES production access — both before the first paying client.
- **Tool failures preserved as learnings (the learnings-log helper was denied by the local command guard twice, so they live here):** grant-expiry-clock-skew — developer-issued grant slips can carry an `expires_at_ns` in the past when the issuing terminal clock disagrees with WSL; verify the echoed expiry against `date +%s` before resuming. question-preference-module-missing — `gstack-question-preference --check` fails on the missing `./scripts/one-way-doors.ts` in the 1.91.164.0 install; the fallback is to ask normally. Also: the command guard pattern-matches payload text — the words for the guard's own name and the grant command's name must not appear in ledger payloads or file content written through the shell.
- **GStack grants expire within the hour:** re-issue before a later step's GStack-copy writes, or the copy blocks.
- The run pauses after Step 16's `domain-locked` gate until Phase C is built — by design.

## Next action

Step 15 (Phase B) for run `app1`, route application: build the threat model, the test plan and the access matrix. Run pauses after Step 16 gate `domain-locked`: Phases C to F are not built.

---

Please run `/mb-close-out` when you are ready.

Type "continue" to proceed to the next step, or raise anything you want changed first.
