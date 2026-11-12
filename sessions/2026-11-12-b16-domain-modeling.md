---
title: "2026-11-12-b16-domain-modeling"
doc-id: "mb-370a3fce"
updated: "2026-11-12"
---
# Step 16 domain modeling for mb-3-cs run app1 — proving run, Phase B closed at domain-locked

OpenCode session, 2026-11-12. PDLF Step 16 (domain modeling) for mb-3-cs, run app1, carried out as a proving run — following b16-reference/config/build-plan/test-spec exactly and reporting every place the step's documents fell short.

## What was done and the evidence

- **The interview** (`grill-with-docs` = grilling with domain-modeling, started from the project library): seven rounds to an empty frontier; 127 terms settled and written into `docs-local/GLOSSARY.md` as they settled, each marked common or trade; the shared names (Client, Customer, Owner, Visitor, Roles) kept their spec-nomenclature meanings. Evidence: the glossary itself; the round-by-round questions and answers in the session.
- **Two ADRs** written with David's approval: `adr-local/adr-common-feature-reads-trade-data.md` (features marked common by need may read trade tables; the one-direction rule binds packages and table links — settling OQ-1) and `adr-local/adr-plan-summary-display-text.md` (the plan summary's amounts are Paddle display text, not money records).
- **The plan brought into agreement** with the glossary: OQ-1/OQ-2 removed from Open Questions (§12 now "None"); §14.9 brought up to date for F-37 (F-37 common feature, plan_summary common table, Common 50→51); §23.7 corrected to the seven management endpoints (the live global API spec names seven — the design doc said six); §17's stale "70 tables" wording corrected; the whole design doc's wording aligned to the glossary under David's authorisation (Terms and Conditions, team members, meeting type, booking).
- **The domain map**: scoped with mb-archify-scope (Scope Card in `output/mb-3-cs/mb-3-cs-b16-app1-domain-map-scope.md` + machine header), drawn with archify into `pdlf-outputs/b16/mb-3-cs-b16-app1-domain-map.html` — 11 nodes, every sublabel marked common/trade, no edge crossing another; visual pass by David.
- **Checks**: DC-09 passed (127 unique terms, no tenant wording, every entity owned and placed); Sentinel CLEAR at Low/Medium (triage_findings id 13, four security-global documents read in full); gitleaks clean over 25 commits; Phase B close-out all green (`pdlf-outputs/b16/mb-3-cs-b16-app1-phase-b-close.md`); the gate `domain-locked` passed by David's `pdlf-gate.sh` (gates row id 1). Step Receipt: `pdlf-outputs/b16/mb-3-cs-b16-app1-receipt.md`; ledger row id 9 complete.

## Decisions and standards David set

- F-13 keeps its feature name "Session packages"; the glossary term is Package. The client's document is the Terms and Conditions; Mybizz's own terms are the Client service agreement; never "Terms of Service".
- Contact, provider and crm-outreach marked common per §14.9; deals stay trade. The referral kit's email banner is "added to campaign emails (sequences and broadcasts)".
- The approved-change authorisations: F-37 carried into the access matrix (PlanAndBilling tab, get_plan_summary, update_referral_kit), the test plan (§20) and the threat model (B5, seven endpoints); the whole design doc aligned to the glossary; the b11 receipt corrected at source (the tool's own table pasted from stdout by script, never retyped).
- Scope Card: approved with two corrections, re-scoped after the first authoring attempt, two layout corrections on his visual review.
- Three proving-run findings recorded on the receipt: PDLF has no rule for changes authorised after a later step's approval; the architecture renderer has no README with a layout budget; PDLF is to gain the rule that after one failed Archify repair the agent runs mb-archify-scope's recovery and presents a revised Scope Card.

## Quirks

- The architecture renderer pins every routed edge to its target side's centre port (one edge per side) and infers horizontal sides whenever dx ≠ 0 — centre-aligned nodes need no routing at all. Seven learnings recorded in `output/mb-3-cs/learnings/2026-11-12-domain-map.md`.
- DC-09 caught a duplicate Customer definition in the glossary mid-step; removed.
- Archify 3.2.0 is available (installed 2.16.0 unchanged — upgrade is David's decision outside a run).

## Documents written or changed

- New: `docs-local/GLOSSARY.md` (filled), `adr-local/adr-common-feature-reads-trade-data.md`, `adr-local/adr-plan-summary-display-text.md`, `output/mb-3-cs/mb-3-cs-b16-app1-domain-map-scope.md`, `output/mb-3-cs/mb-3-cs-b16-app1-domain-map-header.json`, `output/mb-3-cs/learnings/2026-11-12-domain-map.md`, `pdlf-outputs/b16/` (receipt, checks.xml, phase-b-close.md, domain-map.html + .json + visual-check sidecar, glossary and ADR copies, gitleaks.json).
- Changed: `docs-local/design/app1/design-doc.md` (§12, §14.9, §17, §23.7, wording alignment), `docs-local/design/app1/access-matrix.yaml` (+PlanAndBilling, two functions, seven endpoints), `docs-local/design/app1/test-plan.md` (§20 F-37), `docs-local/design/app1/threat-model.md` (B5, actor row), `pdlf-outputs/b11/mb-3-cs-b11-app1-receipt.md` (the approved Tests-table correction), `~/.gstack/.../dev-p-main-design-pdlf-app1.md` (the GStack copy, byte-identical to design-doc.md).

## What is open

- The run pauses by design after the gate: Phase C begins at Step 17 when built; Step 17 reads GLOSSARY.md and adr-local first.
- David's business actions: AC-6 (Anvil hosting conditions) and AC-14 (SES production access), both before the first paying client.
- The three proving-run findings are PDLF-improvement items for David.
- Other-document alignments David makes outside the run: "delivery mode"→meeting type; "staff"/"user" wording; "newsletter"→broadcast; three schema descriptions.
