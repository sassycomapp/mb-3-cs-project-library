---
title: "mb-3-cs-b15-app1-global-proposals"
document: Global-move proposal — Step 15, run app1
state: Reviewed 2026-11-07
doc-id: "mb-550c77a6"
updated: "2026-11-07"
---

# Global-Move Proposal — Step 15 (run app1)

Every decision in `project-library-global` that this review found applies to this application alone, for David to approve moving into `mb-3-cs-project-library`.

**Proposal 1 — the summary tasks' period-keyed upsert (R5/D6).** `adr-data-patterns` Pattern 4's example writes each summary row with `add_row`; this application's approved pattern is the period-keyed upsert inside `@tables.in_transaction` (design doc §23.6): a re-run updates or skips instead of duplicating. **David's decision (2026-11-07): write-back to global — `adr-data-access-patterns` is corrected after Step 15 by Claude, so the upsert reaches every application with the next release; no local ADR is added.** The correction rides the post-Step-15 document round.

**Nothing else.** Every other finding this review made against the global corpus is a correction that serves every application and is already assigned to the post-Step-15 correction round by David: the webhook ADR's hourly wording (5-minute reconciliation), `spec-component-properties`' SidesheetContent entry, the notification-engine specification's `/platform-notification` path, the three security-global documents' Brevo wording, and now `adr-data-access-patterns`' upsert correction (Proposal 1). No global decision is used by this application alone, and no local ADR arises from this step.
