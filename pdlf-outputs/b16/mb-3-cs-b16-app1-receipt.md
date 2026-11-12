---
title: "mb-3-cs-b16-app1-receipt"
document: Step 16 Receipt — mb-3-cs b16 app1
state: Generated from ledger steps row id 9; snapshot 2026-11-12
doc-id: "mb-2aa6c3ea"
updated: "2026-11-12"
---

# Step Receipt — mb-3-cs — Step 16 — app1

Phase: B   |   Type: gate   |   Status: complete
Started: 2026-11-12 07:37:56+00   |   Completed: 2026-11-12 10:00:10+00

## What was invoked

Skill family: matt-skills
Skill: grill-with-docs (grilling with domain-modeling), with data-models, graphify (prior material), mb-archify-scope + archify (the domain map), mb-sentinel-review (Low/Medium), gitleaks, ts_report.py
Inputs used: Step 15 (the reviewed plan and its data design, the test plan, the access matrix, the threat model, the b15 receipt); the glossary skeleton and adr-local (three accepted ADRs); the app's graphify code graph as prior material; spec-nomenclature; the context pack in full; the Phase B receipts and evidence b11–b15.

## Output

Artifact: `docs-local/GLOSSARY.md` — the business's glossary: 127 terms, each defined once, marked common or trade, written as the interview settled them
Beside it: `adr-local/adr-common-feature-reads-trade-data.md` and `adr-local/adr-plan-summary-display-text.md` (written with David's approval); design-doc.md brought into agreement with the glossary (§14.9 updated for F-37, §23.7 corrected to the seven management endpoints, §17's table-count wording corrected, the wording aligned under David's authorisation, Open Questions emptied); the domain map `pdlf-outputs/b16/mb-3-cs-b16-app1-domain-map.html` with its Scope Card and machine header in `output/mb-3-cs/`; the Phase B close-out check `pdlf-outputs/b16/mb-3-cs-b16-app1-phase-b-close.md`; the step-folder copies (glossary, both ADRs, the map and its spec)
Synthesized (test-mode shortcut): false

## Tests

Test evidence in `pdlf-outputs/b16` (ts_report.py output, unchanged):

| File | Passed | Failed | Review | Skipped | Errors |
|---|---|---|---|---|---|
| mb-3-cs-b16-app1-checks.xml | 7 | 0 | 0 | 0 | 0 |
| **Total** | **7** | **0** | **0** | **0** | **0** |

Case names: PR-01, DC-09, PR-05, VISUAL-CHECK, PR-04, PR-06, PR-03.

## BFT checkpoint

Level applied: full
Bones:   Hinge: the 127-term glossary and the two ADRs. Big bones: the common/trade marking consistent across glossary and plan (24 term corrections to match §14.9; F-37 and plan_summary added, Common 50→51); OQ-1 and OQ-2 settled and removed; the domain map (11 nodes, marks in every sublabel, no edge crossing another). Small bones: the F-37 carry into access matrix, test plan and threat model; §23.7's seven endpoints; the wording alignment; the corrected b11 receipt; the copies and checks file. Intangible: every David decision recorded verbatim, nothing silently applied.
Muscle:  domain-modeling active discipline: 127 terms challenged and sharpened, the duplicate Customer caught by DC-09, relationships stress-tested, each term written as it settled; data-models both directions (every entity owned and placed; data-design wording checked against the glossary); grilling seven rounds to an empty frontier.
Nerves:  Backward from the gate: Phase C needed one vocabulary, produced and locked. Forward: Step 17 builds the 71 tables from authoritative-schema with every table access already decided (data-table-creation met at vocabulary level, Sentinel CLEAR).
Fat:     Cut: no prototype (no uncertain lifecycle), no research run, the referral-link URL kept out as implementation, the map's twelfth node cut by re-scope. Kept: the full 127 terms (DC-09 demands the whole vocabulary); the below-spine straight verticals (David's visual ruling).
Skin:    Glossary in the skill format with marks; the map with marks and David's corrections; the receipt generated from the ledger row; the gate block as configured.
Health:  In itself: 9/9 artifact checks, David's visual pass, gitleaks clean over 25 commits, Sentinel CLEAR, PR-05 across all Phase B, DC-09 pass. For the project: Phase C builds on a settled foundation; the three proving-run findings recorded.

## Gate status

Gate: domain-locked
Passed: true
Approved at: 2026-11-12 09:58:27.878332+00

## Deviation

None — no deviation row was written. Two transient command-guard timeouts and one guard stop were resolved by rerouting through the dedicated tools; no scope or method changed.

## David's decisions and authorisations recorded this step

- **The interview (seven rounds, 127 terms):** every round answered yes with refinements David dictated; the glossary carries them verbatim in intent. Q5: F-13 keeps its feature name Session packages in the concept and architecture documents; Avoid "session credit" not "credit". Q12: Meeting type's Avoid is "mode, channel, delivery mode" with no note. David added the party terms (Mybizz, the developer, Owner, User, Team member, Visitor, Prospect) and later Subscription (Plan's Avoid loses "subscription alone") and corrected the referral kit's email banner to "campaign emails (sequences and broadcasts)". Q49: the client's document is the Terms and Conditions; Mybizz's own terms are the Client service agreement; never "Terms of Service".
- **Q9 authorisation — F-37 carried into Step 15 outputs:** the access matrix gained the PlanAndBilling tab (Owner only) and `get_plan_summary`/`update_referral_kit`; the test plan gained §20 (F-37: the mock-up invoice, the referral kit, BR-10); the threat model's B5 and the App 5 actor row gained the plan-summary push and the seven endpoints. Recorded with the proving-run finding below.
- **Q9 extension — the whole reviewed plan aligned to the glossary:** design-doc.md wording aligned beyond the Architecture section under David's authorisation ("Terms of Service"→Terms and Conditions, "delivery mode"→meeting type, "unlimited staff"/"staff"→team members, "appointment"→booking where the record is meant, the later settled terms); the exact change list is in the step conversation.
- **M1:** §17's "the schema stays at 70 tables" corrected to the legal-documents-added-two-tables wording, the count left to authoritative-schema.md.
- **M3:** Email template and Domain added to the glossary (Domain with David's exact wording and Avoid).
- **Scope Card:** approved with two corrections (contact, provider, crm-outreach common per §14.9; §14.9 brought up to date for F-37); re-scoped after the first authoring attempt (customer-package to the deferred list, 11 nodes); two layout corrections on his visual review (provider and quote below the spine as straight verticals; every sublabel beginning with its mark); approved as written and re-approved after re-delivery.
- **M1/M2/M3 alignments David makes outside the run:** "delivery mode" for meeting type (concept, feature list, architecture reference, user flows, brand profile, BRD BR-1, PRD FR-10 and FR-11); "staff" for the whole team and "user" where a role is meant; "newsletter" for a broadcast (concept, feature list, architecture reference, integration architecture, user flows, schema); the schema's referral-banner note ("marketing emails and newsletters"); the schema's three descriptions (Contact form messages → Enquiries; Transactional email templates; the email-domain description).
- **PR-05 correction:** David approved correcting the b11 receipt at its source on condition — the tool's own table pasted unchanged from `ts_report.py` stdout by script (never retyped), per-test rows kept below, no evidence file changed; all four Phase B `--check` runs then passed.
- **The gate:** David's yes at the block, recorded by his `pdlf-gate.sh` (gates row id 1) — the agent never ran it and never wrote the row.

## Proving-run findings

1. **PDLF has no rule for a change authorised after a later step's approval.** Step 15 was approved, then David authorised changes covering F-37 and wording; Step 16 had no rule under which to edit Step 15's outputs (access matrix, test plan, threat model) or the whole reviewed plan rather than only its Architecture section. Each edit was made only on David's explicit authorisation in the step, recorded above. PDLF is to gain a rule for the authorised-change case.
2. **The architecture renderer has no README with a layout budget.** mb-archify-scope's Scope Card names `renderers/<type>/README.md` as the budget source and says "read it at Phase 5, never from memory"; `renderers/architecture/` holds only `grid.mjs` and `render-architecture.mjs` — there is no README to read. The b15 Scope Card's pointer was aspirational in the same way.
3. **The Archify recovery rule is missing from PDLF.** When Archify's validation failed on routing, the agent repaired geometry for two rounds before running mb-archify-scope's recovery; the skill's own rule is to re-scope on corridor failures and reshape the rows rather than hand-route. PDLF is to gain the rule: after one failed repair, the agent runs the recovery and presents a revised Scope Card.

## Ledger row written

- `steps` row id 9: project_slug mb-3-cs, step_number 16, sort_order 16, phase B, run app1, app_number 1, step_type gate, skill_family matt-skills, skill_name grill-with-docs, status complete, synthesized false, artifact_path `docs-local/GLOSSARY.md`, started 2026-11-12 07:37:56 UTC, completed 2026-11-12 10:00:10 UTC, inputs_used `{"inputs": ["Step 15"]}`, gbrain_query and gbrain_results set, next_action and notes_for_next_session set, blockers NULL.
- `gates` row id 1: project mb-3-cs, step_id 9, gate_name domain-locked, passed true, approved_at 2026-11-12 09:58:27.878332 UTC — written by David's `pdlf-gate.sh`.
- `bft_records` row id 6: step_id 9, level full, recorded 2026-11-12, all six answers.
- `triage_findings` row id 13: finding_id sentinel-16-app1, source_step 16, triage_label clear, resolved_at set (Sentinel CLEAR at Low/Medium; four documents read in full).

## Input notices carried

- No Step 12–15 input was synthesized or skipped (`synthesized=false` on every row; no skipped status) — no taint notice is due.
- **David's post-Step-15 authorisation (carried from the task):** after Step 15 was approved, David authorised changes to the reviewed plan and the project documents — Mybizz's price £119 a month or £1,190 a year; the database standby from the first client; no fixed client limit; part-time support from 20 clients; feature F-37 Plan, billing and referrals with the plan_summary table (71 tables), the Plan and billing tab in Settings (15 tabs), the referral kit, and requirements BR-30, US-44, US-45, FR-40, SR-65 and SR-66. The `pdlf-outputs/b15/` copies are Step 15's evidence and stay unchanged; the live documents are the current inputs.
- **Carried from the b15 receipt:** the post-Step-15 document corrections (Claude, outside the run) are done and verified: adr-webhook-architecture's 5-minute wording, SidesheetContent approved, the three security-global documents' SES wording, the notification spec's `/management/push-notification` path, adr-data-access-patterns' period-keyed upsert, and authoritative-schema.md mirroring the 2026-11-06 amendment. The design doc holds two GSTACK REVIEW REPORT sections — the LAST one is current. David's business actions remain open: AC-6 (Anvil hosting conditions) and AC-14 (SES production access), both before the first paying client.

## Close-out

Please run `/mb-close-out` when you are ready.

## Blockers

None.

## Notes for next session

- Phase B is closed and locked at `domain-locked`; Phase C begins at Step 17 when built — the run pauses here by design (Phases C–F are not built).
- Step 17 reads `docs-local/GLOSSARY.md` and `adr-local/` first, then the reviewed plan's data design; the domain map and its Scope Card are in `pdlf-outputs/b16/` and `output/mb-3-cs/`.
- The glossary is the business's one vocabulary: later runs extend it, never rewrite it.
- The three proving-run findings above are PDLF-improvement items for David, not app work.
- Archify 3.2.0 is available (normal severity); the installed 2.16.0 is unchanged — upgrading is David's decision outside a run.

## Next action

Step 17 (Phase C, design and plan) is the next step of run app1 — the run pauses here by design. When Phase C is built, Step 17 reads GLOSSARY.md and adr-local first.

---

Type "continue" to proceed to the next step, or raise anything you want changed first.
