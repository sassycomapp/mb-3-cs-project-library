# MM1 Assessment — Post-Clarification

**Date:** 2026-05-03
**Status:** Assessment complete, sanitisation deferred

---

## Updated Classification Changes

### REVISE → REMOVE

1. **mm1-12 (Mybizz Planning) Part 2 — Rules File Inventory** — Confirmed obsolete. Path aliases (`@devref`, `@agentref`, `@backup`) are from a prior workspace setup. The inventory no longer reflects the actual rules file set.
2. **mm1-16 (Policy Security) CCPA section (§2.3)** — Confirmed speculative, inherited from an earlier iteration. CCPA thresholds ($25M revenue, 50K consumers) are out of scope for this project.
3. **mm1-16 code examples** (`sm_security.py`, `encryption_master_key`) — Confirmed obsolete. Contradicts established Vault/encryption architecture.
4. **ADR-007 and ADR-008** — Confirmed obsolete. Carried over from a previous iteration.

### KEEP → REVISE

1. **mm1-21 (Build Plan) §1.6 AdminLayout Navigation** — `nav_media → MediaLibraryForm` is an orphaned reference. No Media Library table exists in the 47-table schema. To be validated and decided.
2. **mm1-18 (Project Scaffold)** — `client_code/routes/` package (`auth_routes.py`, `public_routes.py`, `error404.py`, `helpers.py`) does not appear in mm1-01 (Arch Spec A) §2.1 client packages. To be validated against live codebase and reconciled.
3. **mm1-08 (Dev Plan) and mm1-21 (Build Plan) — Expenses references** — Expense tracking is confirmed out of scope. References to Expenses in navigation, schema, and build plan must be removed.

---

## Risks and Ambiguities — Resolved

### 1. Phase Sequence Contradiction
**Status:** Resolved — all obsolete.
All three build sequences (mm1-08, mm1-12, mm1-21) are obsolete. A new build sequence will be decided following fresh planning. No existing sequence should be treated as authoritative.

### 2. Stale Zoho References
**Status:** Resolved — Zoho retired.
Zoho is wholly replaced by Brevo. All Zoho mentions in mm1-18 (Project Scaffold) and elsewhere are stale and must be removed.

### 3. Feature Scope Creep
**Status:** Resolved — all obsolete.
Knowledge Base, Ticket System, Reviews, Events, and Event Registrations are carry-overs from an earlier iteration. All are confirmed out of scope. Remove from schema, scaffold, and any build plan references.

### 4. Dual Contact Tables (contacts + customers)
**Status:** Resolved — intent clarified.
A single `contacts` table is the intended design to support the CRM. All customers are contacts but not all contacts are customers. The `customers` table is legacy and should be removed or merged into `contacts`.

### 5. mm1-16 Code Examples
**Status:** Resolved — obsolete.
Security policy document contains Python code that contradicts the established architecture (wrong module names, wrong secret names). Likely from an earlier iteration. Should be redesigned against current Vault/encryption patterns.

### 6. cs-refactor-notes.md
**Status:** Resolved — confirmed outdated.
Located at `C:\_mb2-cs-app\project-library\open-tasks\cs-refactor-notes.md`. Confirmed outdated. Not to be treated as authoritative.

---

## Net Assessment

**MM1 is not currently fit to support AM1 technical specification work** in its present state. The document set contains:

- **Substantial valid architectural content:** mm1-01 through mm1-04 (Architectural Spec A–D), mm1-06 (Conceptual Design), mm1-09 (Dev Policy), mm1-14 (Platform Overview), mm1-15 (Policy Nomenclature), mm1-19 (Database), mm1-20 (Database Schema), mm1-23 (User Flows)
- **Significant obsolete content** that must be cleaned before AM1 can safely derive from it
- **No canonical build sequence** — all three candidates (mm1-08, mm1-12, mm1-21) are obsolete
- **Schema drift** — 47 tables includes features confirmed out of scope (Knowledge Base, Tickets, Reviews, Events, Event Registrations, Expenses)

---

## Recommended Next Planning Step

A **MM1 sanitisation pass** before any new build planning:

1. **Strip obsolete features from schema** — Remove Knowledge Base (kb_articles, kb_categories), Tickets (tickets, ticket_messages), Reviews (reviews), Events (events, event_registrations), Expenses (expenses) from mm1-19/mm1-20. This reduces the table count from 47 to approximately 40.
2. **Resolve contacts/customers duality** — Mark `customers` table for removal or document its residual purpose if any remains. Single `contacts` table is the confirmed intent.
3. **Remove Zoho references** — Sweep mm1-18 and any other MM1 files for Zoho mentions.
4. **Remove orphaned nav items** — `nav_media`, Expenses from mm1-21 nav spec.
5. **Redesign mm1-16** — Replace obsolete code examples with current Vault/encryption patterns from mm1-01/mm1-02. Remove CCPA section.
6. **Flag mm1-08, mm1-12, mm1-21 as obsolete** — Mark all three build plans as superseded. New planning will produce a single canonical sequence.
7. **Validate `client_code/routes/`** — Check against live `anvil.yaml` to confirm whether this package exists or is stale.
8. **Remove ADR-007 and ADR-008** — Confirmed obsolete, carried over from previous iteration.

After sanitisation, the MM1 set will be a clean baseline from which new phase planning and AM1 spec work can proceed.

---

## Compliance Statement

- No files were edited during this assessment
- No files were created during this assessment
- No todos were changed during this assessment
- No devlog was changed during this assessment
- No ADRs were changed during this assessment
