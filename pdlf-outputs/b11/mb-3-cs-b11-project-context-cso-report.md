---
title: "mb-3-cs-b11-project-context-cso-report"
doc-id: "mb-f3912f6e"
updated: "2026-11-01"
---
partial — default
Status: partial. Some required coverage is missing; findings below cover only the assessed part.
Ran: snapshot-inputs (assessed); history-inputs (partial). Missing: history-inputs (partial); application-model (not\_assessed); attack-surface (not\_assessed); secrets (not\_assessed); dependencies (not\_assessed); ci-cd (not\_assessed); infrastructure (not\_assessed); integrations (not\_assessed); llm-agentic-mcp (not\_assessed); skill-supply-chain (not\_assessed); owasp-2025 (not\_assessed); stride (not\_assessed); data-classification (not\_assessed).
Reason: application-model: not assessed; attack-surface: not assessed; secrets: not assessed; dependencies: not assessed; ci-cd: not assessed; infrastructure: not assessed; integrations: not assessed; llm-agentic-mcp: not assessed; skill-supply-chain: not assessed; owasp-2025: not assessed; stride: not assessed; data-classification: not assessed; Application model was not completed; History truncated: patches were withheld because the newest commit's patch or the patch log exceeds the 1048576-byte history limit; only commit metadata was retained; Assessment has not been submitted.
Next: start a new /cso audit after resolving the reasons above; run `gstack-cso doctor --repo <repo>` to check prerequisites.
Run: 1793375457984-d14053cdf98a91b0. Mode: daily.
Timing: started 2026-10-30T15:50:57.948Z; deadline 2026-10-30T16:00:57.948Z; terminal 2026-10-30T15:52:38.587Z; elapsed 100639 ms.
Material gaps: application-model: not assessed; attack-surface: not assessed; secrets: not assessed; dependencies: not assessed; ci-cd: not assessed; infrastructure: not assessed; integrations: not assessed; llm-agentic-mcp: not assessed; skill-supply-chain: not assessed; owasp-2025: not assessed; stride: not assessed; data-classification: not assessed; Application model was not completed; History truncated: patches were withheld because the newest commit's patch or the patch log exceeds the 1048576-byte history limit; only commit metadata was retained; Assessment has not been submitted.

Application model:
- Actors: none.
- Assets: none.
- Entrypoints: none.
- Tenant boundaries: none.
- Sensitive operations: none.
- Security invariants: none.

No supported findings in the assessed scope.

Snapshot transformations:
- AGENTS.md: excluded: credential or execution configuration
- CLAUDE.md: excluded: credential or execution configuration
- agents.md: excluded: credential or execution configuration
- [[[mb-3-cs-project-context:docs-local/mb-3-cs-test-specification]]](C:/dev/dev-mb-3-cs/mb-3-cs-project-context/docs-local/mb-3-cs-test-specification.md): secret spans redacted
- memory/retro-2026-06-26.json: secret spans redacted
- wireframe-refresh-completion-report-2026-06-28: secret spans redacted
- judgement-trail: secret spans redacted
- questions-to-raise-with-anvil: secret spans redacted

Coverage:
- snapshot-inputs: assessed; fail-closed captured source inventory.
  Scope: default. Evidence: 582 sanitized execution inputs captured; 3 explicit non-executable exclusions; 0 unread in-scope inputs; 0 tracked deletions. Gaps: none. Exclusions: AGENTS.md: excluded: credential or execution configuration; CLAUDE.md: excluded: credential or execution configuration; agents.md: excluded: credential or execution configuration.
- history-inputs: partial; bounded helper-retained Git history.
  Scope: default. Evidence: Captured metadata for at most 100 from d0164e420cf65c897f26cd0606d8bc19159462e7. Gaps: History truncated: patches were withheld because the newest commit's patch or the patch log exceeds the 1048576-byte history limit; only commit metadata was retained. Exclusions: none.
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
