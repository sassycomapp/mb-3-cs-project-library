Recommended final sequence:

1. /design-consultation - Refresh design authority
- Deep-update `DESIGN.md` as the canonical design reference before any further downstream review, because it is explicitly the design authority for downstream design and UI decisions. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abcb2a0a-867a-4582-be40-02477949ce72/DESIGN-2.md)
- Remove stale custom-component assumptions from `DESIGN.md`, specifically:
  - remove `OnboardingPaletteForm`,
  - add `ClauseBuilder`,
  - downgrade `PaletteSettingsForm` from shared to local/form-specific. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/40ff1e55-c402-46e8-a381-3d89b38a96b6/sanity-audit-final-report-4.md)
- Align the design authority to the reconciled planning state:
  - separate `OnboardingForm` from `SettingsForm`,
  - preserve the 11-tab Settings architecture,
  - reflect RBAC-governed tab visibility,
  - reflect “Return to Onboarding” behavior,
  - keep legal acknowledgement on onboarding rather than as a mutable settings concern. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/6b405cdc-e5d4-4a05-bb85-3079a235354d/platform-overview-6.md)
- Ensure the design authority explicitly covers the current implementation-facing artefacts:
  - `OnboardingForm`,
  - `SettingsForm`,
  - `PolicyEditorForm`,
  - `ClauseBuilder`,
  - `PrivacyPolicyPage`,
  - `TermsConditionsPage`. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/87573d9b-ca99-44b3-8354-484c38fd9664/plan-tune-verification-report-5.md)
- Normalize any stale inventory, naming, component purpose, or template references that still predate the reconciled audit outcome. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abcb2a0a-867a-4582-be40-02477949ce72/DESIGN-2.md)

2. /build - Complete missing planning artefacts in the workspace
- Create the missing `wireframe-onboarding-OnboardingForm.html`, which remains the one high-priority missing wireframe in the reconciled planning set. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/40ff1e55-c402-46e8-a381-3d89b38a96b6/sanity-audit-final-report-4.md)
- Ensure the onboarding wireframe shows the approved 6-step onboarding flow and its navigation relationship with Settings tabs. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/40ff1e55-c402-46e8-a381-3d89b38a96b6/sanity-audit-final-report-4.md)
- Verify `wireframe-settings-PolicyEditorForm.html` exists, is current, and is aligned with ClauseBuilder-based policy authoring, because the latest mapping confirms it belongs in the implementation set and in Stage 4.2 planning. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/87573d9b-ca99-44b3-8354-484c38fd9664/plan-tune-verification-report-5.md)
- Verify the ClauseBuilder wireframe remains aligned with the current shared-component decision and policy-authoring behavior. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/40ff1e55-c402-46e8-a381-3d89b38a96b6/sanity-audit-final-report-4.md)
- Normalize repo-side naming/ownership of onboarding, policy-editor, and policy-page artefacts so later reviews inspect a stable workspace state. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/87573d9b-ca99-44b3-8354-484c38fd9664/plan-tune-verification-report-5.md)

3. /plan-design-review - Validate design against the reconciled plan
- Re-review the plan, not the live site, against the refreshed `DESIGN.md`, current wireframes, and reconciled planning docs, because this command is specifically for plan-level design completeness review. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
- Confirm there are no remaining contradictions in:
  - onboarding versus Settings boundary,
  - ClauseBuilder placement,
  - policy editing flow,
  - custom component inventory,
  - public versus management UI design consistency. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/6b405cdc-e5d4-4a05-bb85-3079a235354d/platform-overview-6.md)
- Confirm information architecture, interaction states, responsive/accessibility expectations, and unresolved design decisions are all closed at plan level before implementation proceeds. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
- Force any remaining design ambiguity back into documentation/todo updates before engineering lock-in. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)

4. /plan-CEO-review - Validate product coherence
- Reconfirm the current V1 is still the narrowest viable wedge after the planning corrections and design refresh. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/6b405cdc-e5d4-4a05-bb85-3079a235354d/platform-overview-6.md)
- Revalidate that the onboarding/settings split, policy editing capability, and Settings tab scope still make product sense and do not introduce accidental scope creep. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
- Remove or defer any feature that remains technically valid but product-noisy for current scope. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
- Lock the product posture as hold scope, selective expansion, or scope reduction before engineering is finalized. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)

5. /plan-ENG-review - Validate execution readiness
- Lock the technical execution model after product and design are revalidated, because this command is the architecture/data-flow/system-boundary lock before implementation. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
- Confirm implementation order and dependencies for:
  - onboarding,
  - tabbed Settings,
  - system currency,
  - legal acknowledgement,
  - ClauseBuilder,
  - policy editor,
  - public policy pages,
  - RBAC boundaries,
  - Vault-related configuration boundaries,
  - anything still merely contextual for `Mybizzmanagement`. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/87573d9b-ca99-44b3-8354-484c38fd9664/plan-tune-verification-report-5.md)
- Confirm the engineering plan and test-plan implications are still current after the refreshed design and artefact work. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
- Ensure the resulting implementation sequence is explicit enough for developer execution in the live Anvil app. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/87573d9b-ca99-44b3-8354-484c38fd9664/plan-tune-verification-report-5.md)

***6. Developer tasks - Update the live Anvil app from the stabilized repo/docs handoff***
- Update live Data Tables in the Anvil app:
  - verify `businessprofile` onboarding/settings fields,
  - verify `systemcurrency`,
  - verify `onboardingstatus`,
  - verify `legalackaccepted`,
  - create or verify `privacypolicy`,
  - create or verify `tandc`. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/40ff1e55-c402-46e8-a381-3d89b38a96b6/sanity-audit-final-report-4.md)
- Update live Anvil form/package structure:
  - create/verify standalone `OnboardingForm`,
  - create/verify tabbed `SettingsForm`,
  - create/verify policy editor host screen,
  - create/verify `PrivacyPolicyPage`,
  - create/verify `TermsConditionsPage`. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/87573d9b-ca99-44b3-8354-484c38fd9664/plan-tune-verification-report-5.md)
- Update live component structure:
  - implement `ClauseBuilder` as a shared custom component,
  - keep `PaletteSettingsForm` local/form-specific,
  - remove obsolete onboarding-palette remnants. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abcb2a0a-867a-4582-be40-02477949ce72/DESIGN-2.md)
- Update live navigation, logic, and permissions:
  - preserve the approved plain-Link/lambda navigation pattern,
  - enforce Settings tab RBAC,
  - enforce onboarding resumability,
  - enforce system-currency immutability,
  - keep payment gateway configuration as a Settings-only concern. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/6b405cdc-e5d4-4a05-bb85-3079a235354d/platform-overview-6.md)
- Ensure these live-app changes result in the updated Anvil application structure and therefore an updated `anvil.yaml`, which later repo-facing review steps should then inspect. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/6b405cdc-e5d4-4a05-bb85-3079a235354d/platform-overview-6.md)

7. /build - Capture post-developer workspace updates
- Pull in and normalize the live-app-derived repo changes after developer execution so the workspace reflects the real implemented structure, including updated form/package/layout metadata and the latest `anvil.yaml`. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/6b405cdc-e5d4-4a05-bb85-3079a235354d/platform-overview-6.md)
- Use `/build` to reconcile any workspace code/docs that must now reflect the actual implemented Anvil structure rather than the prior planning-only expectation. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
- Update implementation-facing files where needed so the repo becomes the current source of truth after the developer pass:
  - build-plan references,
  - todo references,
  - form/component file mappings,
  - any implementation notes tied to actual Anvil artefacts. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/87573d9b-ca99-44b3-8354-484c38fd9664/plan-tune-verification-report-5.md)
- Verify that the planned forms, components, and tables are now represented consistently across docs and code-facing workspace files. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/40ff1e55-c402-46e8-a381-3d89b38a96b6/sanity-audit-final-report-4.md)

8. /plan-tune - Final tightening pass after live-app structure is reflected back into the workspace
- Run `/plan-tune` only after the workspace has been updated from the developer’s live Anvil changes, so it can tighten the final truth rather than a pre-implementation approximation. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
- Reconcile the full planning set against the now-updated repo/workspace state:
  - `DESIGN.md`,
  - build-plan,
  - todo,
  - wireframes,
  - implementation mappings,
  - component inventory,
  - actual form/package structure,
  - `anvil.yaml`-reflected structure. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abcb2a0a-867a-4582-be40-02477949ce72/DESIGN-2.md)
- Remove any last stale references, loose edges, or mismatches between plan and actual implementation structure. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/87573d9b-ca99-44b3-8354-484c38fd9664/plan-tune-verification-report-5.md)
- Produce a true implementation-ready or planning-closed state only after that reconciliation is complete. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)

9. /review - Audit the changed files after implementation structure exists
- Run `/review` after the live-app work and post-developer `/build` pass, because only then do you have the actual changed file set for staff-engineer review. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
- Audit changed files for broken invariants, trust-boundary issues, race conditions, missing edge-case handling, and implementation drift from the locked plan. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
- Pay particular attention to:
  - onboarding/state-transition handling,
  - RBAC gating,
  - Settings tab ownership,
  - policy editing save/publish behavior,
  - navigation consistency,
  - Data Table interaction boundaries. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/6b405cdc-e5d4-4a05-bb85-3079a235354d/platform-overview-6.md)
- Auto-fix obvious issues if that is how you are running review, and surface the ambiguous ones for decision. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)

10. /qa - Test the implemented app after review
- Run `/qa` after `/review`, because the GStack pipeline explicitly places QA after review and before ship. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
- Use QA to test the changed forms/pages/components that now exist in the app, especially:
  - onboarding flow,
  - Settings tabs,
  - ClauseBuilder flow,
  - policy editor,
  - Privacy Policy page,
  - Terms & Conditions page,
  - navigation and role-based visibility. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/40ff1e55-c402-46e8-a381-3d89b38a96b6/sanity-audit-final-report-4.md)
- Validate real browser behavior against the implemented app state rather than only doc assumptions. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
- Ensure any bugs found are either fixed in-loop or converted into explicit follow-up work before moving on. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)

11. /document-release - Cross-check documentation against the latest diff
- Run `/document-release` after implementation review and QA so documentation is reconciled against the actual latest diff, which is what this command is intended to do. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
- Update anything that drifted:
  - file paths,
  - component lists,
  - command lists,
  - project structure references,
  - implementation handoff references,
  - any documentation that still describes pre-implementation assumptions. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
- Ensure the repo documentation finally matches both the locked plan and the implemented Anvil structure. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abcb2a0a-867a-4582-be40-02477949ce72/DESIGN-2.md)

12. Optional final close-out commands
- `/plan-devex-review` if you want a final maintainability and handoff-quality audit before broader implementation continues, because this command is specifically for developer-experience review of the plan/workflow/setup quality. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
- `/ship` only after `/review` and `/qa` have passed and you are actually ready to push/open a PR. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)

## Notes on command usage

- `/scaffold` should not appear anywhere in the sequence because it is not a valid slash command in `slash-commands-gstack.md`. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
- Live Anvil app work is a developer task, not a GStack “live app skill,” because the valid GStack commands operate on the workspace pipeline and repo-side flow rather than replacing developer execution inside the live Anvil editor. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/6b405cdc-e5d4-4a05-bb85-3079a235354d/platform-overview-6.md)
- `/build` can appear more than once where appropriate, because it is the main coding skill and can be used at more than one stage of the workflow when the workspace needs to be updated before later review/tuning steps. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
- Your requested inversion is correct: the “complete missing planning artefacts” `/build` should come before the final `/plan-tune`, and a later `/build` is also appropriate after developer live-app work so the workspace catches up before final tune/review/qa. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/87573d9b-ca99-44b3-8354-484c38fd9664/plan-tune-verification-report-5.md)

## Short execution view

1. `/design-consultation` - refresh design authority. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/abcb2a0a-867a-4582-be40-02477949ce72/DESIGN-2.md)
2. `/build` - create/verify remaining planning artefacts. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/40ff1e55-c402-46e8-a381-3d89b38a96b6/sanity-audit-final-report-4.md)
3. `/plan-design-review` - validate plan-level design completeness. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
4. `/plan-CEO-review` - validate V1 product coherence. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
5. `/plan-ENG-review` - lock execution readiness. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
6. Developer tasks - update live Anvil app and Data Tables. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/6b405cdc-e5d4-4a05-bb85-3079a235354d/platform-overview-6.md)
7. `/build` - capture post-developer workspace/anvil.yaml-aligned changes. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/6b405cdc-e5d4-4a05-bb85-3079a235354d/platform-overview-6.md)
8. `/plan-tune` - final tightening against actual updated workspace state. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
9. `/review` - audit changed files. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
10. `/qa` - browser-test changed app flows. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
11. `/document-release` - sync docs to latest diff. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)
12. `/ship` - only when ready to open PR. [ppl-ai-file-upload.s3.amazonaws](https://ppl-ai-file-upload.s3.amazonaws.com/web/direct-files/attachments/50806054/9dbded3d-49a4-43a8-92b2-95aa61c7ed74/slash-commands-gstack.md)