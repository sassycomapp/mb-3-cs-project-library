General Agentic workflow
A session runs continuously until A new session is started

Stations can revisited and continued later if desired
When An agentic task is complete and it has closed out and I then perform one of the interjection tasks and then return to opencode, after an interjection, the commands that follow reference those files from disk, not from conversation history. 

The Non-agentic interjections at steps 3, 7, 8 and 14 are file-producing tasks — wireframes, Anvil UI code, database tables, and Uplink test files which all land on disk. 
Work done in Anvil.works itself, is outside of the terminal. Ensure that the anvil repo is synced back to local before continuing to the next step 9, so that the agent reads the current state of the codebase.
The context that was in the session history remains in the session history but the new work that was done during the interjection task is not included in the session history as it was not done during the session. The new work that was done during the interjection task will be on disk and That work and the affected files should be brought to the attention of the session if it is relevant context for the next agentic task in that session
Once a skill has run, The next prompt will be directly to the opencode agent unless a specific skill is nominated with the “/” command

1.  [GStack]: /plan-ceo-review
    └─ locks product scope and direction; source of truth for everything downstream

2.  [GStack]: /design-consultation ← rerun ONLY if step 1 changed scope significantly
    └─ produces DESIGN.md (typography, colour, spacing, motion)

3.  Update wireframes ← OpenCode natural language request (not a slash command)
    └─ structural layout only, not styled
    └─ reference docs/ and wireframes/ folders
    └─ always required after step 1 regardless of whether step 2 was rerun

4.  [GStack]: /design-shotgun ← applies DESIGN.md design system TO updated wireframes
    └─ produces 3 styled variants → you approve one before proceeding

5.  [GStack]: /design-html ← converts approved variant into polished production HTML
    └─ output IS based on your wireframes; serves as your Anvil build reference

6.  [GStack]: /design-review ← audits the production HTML with an 80-item visual checklist
    └─ enters a fix loop: locates issues, makes minimal CSS changes,
       commits atomically, re-navigates and verifies with screenshots
    ⚠ resolve all flagged issues before proceeding to step 7

7.  [Anvil]: Build UIs in Anvil.works ← manual translation of production HTML (step 5)
    into Anvil forms
    └─ update local repo when done

8.  [Anvil]: Build database in Anvil.works ← adjust data tables to match approved schema
    └─ update local repo when done

9.  [GStack]: /plan-eng-review ← now has real scaffolding, anvil.yaml and DB schema
    └─ locks technical architecture: data flow, system boundaries,
       state transitions, edge cases and failure modes

10. [GStack]: /plan-design-review ← reviews the plan (not the live site) for design
    completeness across 7 dimensions; patches the plan where needed
    ⚠ any issues from steps /plan-eng-review and /plan-design-review requiring Anvil changes must be resolved
      in Anvil and repo re-synced before proceeding to step 11

11. [GStack]: /build ← codes the app on top of existing Anvil scaffolding
    └─ Python server modules, event handlers, data bindings,
       business logic and navigation logic

12. [GStack]: /review ← paranoid code audit: security holes, race conditions, N+1 queries,
    broken invariants and edge case gaps
    └─ auto-fixes obvious issues; flags ambiguous ones for your decision

13. [GStack]: /qa ← spins up real Chromium via Playwright, reads git diff, tests changed
    pages; auto-generates regression tests for every bug it fixes

14. [Anvil]: Anvil Uplink integration tests ← OpenCode natural language request
    └─ tests server modules against real Anvil database
    └─ only add dedicated unit tests if server modules contain
       complex pure logic not covered by Uplink tests

15. [GStack]: /ship ← syncs with main, runs tests, audits test coverage,
    pushes branch and creates GitHub PR

16. [GStack]: /land-and-deploy ← merges PR, waits for CI and deploy to complete,
    runs canary health checks against production
    └─ Note: requires one-time /setup-deploy before first use

----

