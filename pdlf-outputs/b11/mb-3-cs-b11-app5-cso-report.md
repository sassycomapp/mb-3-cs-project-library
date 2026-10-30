---
title: "mb-3-cs-b11-app5-cso-report"
doc-id: "mb-80da077a"
updated: "2026-10-30"
---
not assessed — default
Status: not assessed. No security domain was assessed, so this report is not a clean result.
Ran: snapshot-inputs (assessed); history-inputs (assessed). Missing: application-model (not\_assessed); attack-surface (not\_assessed); secrets (not\_assessed); dependencies (not\_assessed); ci-cd (not\_assessed); infrastructure (not\_assessed); integrations (not\_assessed); llm-agentic-mcp (not\_assessed); skill-supply-chain (not\_assessed); owasp-2025 (not\_assessed); stride (not\_assessed); data-classification (not\_assessed).
Reason: application-model: not assessed; attack-surface: not assessed; secrets: not assessed; dependencies: not assessed; ci-cd: not assessed; infrastructure: not assessed; integrations: not assessed; llm-agentic-mcp: not assessed; skill-supply-chain: not assessed; owasp-2025: not assessed; stride: not assessed; data-classification: not assessed; Application model was not completed; Assessment has not been submitted.
Next: start a new /cso audit after resolving the reasons above; run `gstack-cso doctor --repo <repo>` to check prerequisites.
Run: 1793375418270-e031bbd533721c24. Mode: daily.
Timing: started 2026-10-30T15:50:18.243Z; deadline 2026-10-30T16:00:18.243Z; terminal 2026-10-30T15:50:56.686Z; elapsed 38443 ms.
Material gaps: application-model: not assessed; attack-surface: not assessed; secrets: not assessed; dependencies: not assessed; ci-cd: not assessed; infrastructure: not assessed; integrations: not assessed; llm-agentic-mcp: not assessed; skill-supply-chain: not assessed; owasp-2025: not assessed; stride: not assessed; data-classification: not assessed; Application model was not completed; Assessment has not been submitted.

Application model:
- Actors: none.
- Assets: none.
- Entrypoints: none.
- Tenant boundaries: none.
- Sensitive operations: none.
- Security invariants: none.

No findings: nothing was assessed.

Snapshot transformations:
- tests/integration/test\_pr09\_environment.py: secret spans redacted

Coverage:
- snapshot-inputs: assessed; fail-closed captured source inventory.
  Scope: default. Evidence: 24 sanitized execution inputs captured; 0 explicit non-executable exclusions; 0 unread in-scope inputs; 0 tracked deletions. Gaps: none. Exclusions: none.
- history-inputs: assessed; bounded helper-retained Git history.
  Scope: default. Evidence: Captured at most 100 from bae07ae26c6189e45b55f15f2c90b8010efeecad. Gaps: none. Exclusions: none.
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
