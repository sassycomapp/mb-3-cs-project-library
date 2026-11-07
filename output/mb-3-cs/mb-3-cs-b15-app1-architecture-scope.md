---
title: "mb-3-cs-b15-app1-architecture-scope"
doc-id: "mb-9222d44f"
updated: "2026-11-07"
---
# Scope Card — mb-3-cs-b15-app1-architecture

**intent:** Show the five-app system and every outside service with its credential home, so the reader trusts that client code never touches data or secrets and every cross-boundary call runs through one named seam. The load-bearing claim: a client instance holds only the schema, one startup module and its dependency — all logic and every credential path is server-side (design doc §6 constraint 3; §23.4).
**type:** architecture — components, boundaries and ownership; the default for "everything that exists and connects"; not a flow picture.
**quality_profile:** standard
**node inventory (10):** client-browser (public client code) · app1 (development workspace) · app2 master_template (all server logic and forms) · app3 blank_client_template (schema, startup, dependency) · app4 client instance (schema, startup, dependency — nothing else) · app5 management (registry, push, AI gateway) · gateways (Stripe/Paystack/PayPal) · ses (Amazon SES/SNS/S3) · r2 (the client's own bucket) · groq (reached only through App 5)
**main path:** client-browser → app4 → app2 → gateways/ses/r2
**deferred to cards:** the eleven seam names; the Vault's twelve secret names; the endpoint paths; the R2 folder table; the task schedule
**corridor plan:** lanes = the five apps, then the services band; each instance→service flow crosses one band at an empty row
**layout budget source:** renderers/architecture/README.md (read at authoring)
**probe declaration:** expected corridors — app4→app2 (dependency band), app2→{gateways,ses,r2,app5} (services band); abort if node overlap or an edge through a node survives the repair order
**decisions (verbatim):** "A) Approve card 1 (recommended)" — D9, 2026-11-07
**status:** approved
