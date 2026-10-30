---
title: "triage-labels"
doc-id: "mb-245ee9ed"
updated: "2026-10-30"
---
# Triage labels

The skills use five triage roles. This library uses the same five strings, which are also the values the PDLF ledger's `triage_findings` table accepts.

| Label in skills | Label in this library | Meaning |
|---|---|---|
| `needs-triage` | `needs-triage` | David needs to evaluate it |
| `needs-info` | `needs-info` | Waiting for more information |
| `ready-for-agent` | `ready-for-agent` | Fully specified, ready for an agent |
| `ready-for-human` | `ready-for-human` | Needs David's own work |
| `wontfix` | `wontfix` | Will not be done |
