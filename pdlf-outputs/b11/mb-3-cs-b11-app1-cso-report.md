---
title: "mb-3-cs-b11-app1-cso-report"
doc-id: "mb-690c1fc3"
updated: "2026-10-30"
---
not assessed — default
Status: not assessed. No security domain was assessed, so this report is not a clean result.
Ran: snapshot-inputs (assessed); history-inputs (assessed). Missing: application-model (not\_assessed); attack-surface (not\_assessed); secrets (not\_assessed); dependencies (not\_assessed); ci-cd (not\_assessed); infrastructure (not\_assessed); integrations (not\_assessed); llm-agentic-mcp (not\_assessed); skill-supply-chain (not\_assessed); owasp-2025 (not\_assessed); stride (not\_assessed); data-classification (not\_assessed).
Reason: application-model: not assessed; attack-surface: not assessed; secrets: not assessed; dependencies: not assessed; ci-cd: not assessed; infrastructure: not assessed; integrations: not assessed; llm-agentic-mcp: not assessed; skill-supply-chain: not assessed; owasp-2025: not assessed; stride: not assessed; data-classification: not assessed; Application model was not completed; Assessment has not been submitted.
Next: start a new /cso audit after resolving the reasons above; run `gstack-cso doctor --repo <repo>` to check prerequisites.
Run: 1793374934149-3f39bca41d286a8a. Mode: daily.
Timing: started 2026-10-30T15:42:14.134Z; deadline 2026-10-30T15:52:14.134Z; terminal 2026-10-30T15:48:32.241Z; elapsed 378107 ms.
Material gaps: application-model: not assessed; attack-surface: not assessed; secrets: not assessed; dependencies: not assessed; ci-cd: not assessed; infrastructure: not assessed; integrations: not assessed; llm-agentic-mcp: not assessed; skill-supply-chain: not assessed; owasp-2025: not assessed; stride: not assessed; data-classification: not assessed; Application model was not completed; Assessment has not been submitted.
Optional scanners without results: gitleaks (not\_assessed: docker is not installed in a trusted system executable directory).

Application model:
- Actors: none.
- Assets: none.
- Entrypoints: none.
- Tenant boundaries: none.
- Sensitive operations: none.
- Security invariants: none.

No findings: nothing was assessed.

Snapshot transformations:
- .anvil\_editor.yaml: secret spans redacted
- client\_code/bookings/BookingViewerForm/form\_template.html: secret spans redacted
- server\_code/server\_shared/encryption\_service.py: secret spans redacted
- tests/integration/test\_pr09\_environment.py: secret spans redacted

Coverage:
- snapshot-inputs: assessed; fail-closed captured source inventory.
  Scope: default. Evidence: 276 sanitized execution inputs captured; 0 explicit non-executable exclusions; 0 unread in-scope inputs; 0 tracked deletions. Gaps: none. Exclusions: none.
- history-inputs: assessed; bounded helper-retained Git history.
  Scope: default. Evidence: Captured at most 100 from 3131d273fce5a6a69616fd9071f6c57331495d02. Gaps: none. Exclusions: none.
- application-model: not\_assessed; pending investigation.
  Scope: default. Evidence: none. Gaps: Assessment has not been submitted. Exclusions: none.
- attack-surface: not\_assessed; pending investigation.
  Scope: default. Evidence: none. Gaps: Assessment has not been submitted. Exclusions: none.
- secrets: not\_assessed; pending investigation.
  Scope: default. Evidence: none. Gaps: Assessment has not been submitted. Exclusions: none.
- dependencies: not\_assessed; pending investigation.
  Scope: default. Evidence: none. Gaps: Assessment has not been submitted. Exclusions: none.
- ci-cd: not\_assessed; pending investigation.
  Scope: default. Evidence: none. Gaps: Assessment has not been submitted. Exclusions: none.
- infrastructure: not\_assessed; pending investigation.
  Scope: default. Evidence: none. Gaps: Assessment has not been submitted. Exclusions: none.
- integrations: not\_assessed; pending investigation.
  Scope: default. Evidence: none. Gaps: Assessment has not been submitted. Exclusions: none.
- llm-agentic-mcp: not\_assessed; pending investigation.
  Scope: default. Evidence: none. Gaps: Assessment has not been submitted. Exclusions: none.
- skill-supply-chain: not\_assessed; pending investigation.
  Scope: default. Evidence: none. Gaps: Assessment has not been submitted. Exclusions: none.
- owasp-2025: not\_assessed; pending investigation.
  Scope: default. Evidence: none. Gaps: Assessment has not been submitted. Exclusions: none.
- stride: not\_assessed; pending investigation.
  Scope: default. Evidence: none. Gaps: Assessment has not been submitted. Exclusions: none.
- data-classification: not\_assessed; pending investigation.
  Scope: default. Evidence: none. Gaps: Assessment has not been submitted. Exclusions: none.
- scanner:gitleaks: not\_assessed; qualified offline Docker scanner; candidate evidence only; tool gitleaks unavailable, freshness not reported, outcome not\_assessed.
  Scope: default. Evidence: 0 scanner candidates; plan 63d79a0d6dba895645bd9a9959a8e0e6473dfc0ecbff6d83a40dac81b8f46882; Immutable outcome: gitleaks-a21fa0a0e5dcb038-4ce1b9821df6f255. Gaps: docker is not installed in a trusted system executable directory. Exclusions: Snapshot transformations apply; inspect the snapshot manifest.; Historical revisions are not scanned by this directory pass..
