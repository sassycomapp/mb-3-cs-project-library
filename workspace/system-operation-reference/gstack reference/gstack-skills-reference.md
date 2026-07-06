# GStack Skills Reference

## What is this?

This is the complete catalog of all 59 GStack skills. Each skill entry shows what it does, what it needs, what it produces, and where output goes.

**How to invoke:** Type the skill name in OpenCode web UI chat (e.g., `/office-hours`)

**Where skills live:** `~/.config/opencode/skills/<name>/SKILL.md` (symlinked from `/mnt/c/mybizz/skills/`)

---

## Section 1: Skill Catalog

0. ### /autoplan
- **Usage:** Auto-review pipeline. Runs CEO, design, eng, and DX reviews sequentially with auto-decisions using 6 decision principles. One command, fully reviewed plan out.
- **Prerequisites:** A plan file or rough plan to review. Git repo with branch context.
- **Output:** Fully reviewed plan with taste decisions surfaced at a final approval gate.
- **Presentation:** Writes to plan file. Taste decisions presented via AskUserQuestion at final gate.

0. ### /benchmark
- **Usage:** Performance regression detection. Establishes baselines for page load times, Core Web Vitals, and resource sizes. Compares before/after on PRs.
- **Prerequisites:** A running web app URL. Browse daemon built and available.
- **Output:** Performance report with TTFB, FCP, LCP, DOM timings, bundle sizes, regression alerts.
- **Presentation:** Reports saved to `.gstack/benchmark-reports/`. Displayed inline.

0. ### /benchmark-models
- **Usage:** Cross-model benchmark. Runs the same prompt through multiple LLMs and compares latency, tokens, cost, and quality side-by-side.
- **Prerequisites:** At least one AI provider authed (Claude, GPT/Codex, or Gemini).
- **Output:** Comparison table showing fastest, cheapest, highest quality provider.
- **Presentation:** Optional save to `~/.gstack/benchmarks/`. Displayed inline.

0. ### /browse
- **Usage:** Fast headless Chromium for QA testing. Navigate pages, interact with elements, verify state, take screenshots, test responsive layouts.
- **Prerequisites:** Browse daemon binary built (one-time `./setup` with bun). URL to navigate.
- **Output:** Screenshots (PNG), annotated snapshots, accessibility trees, console logs, performance data.
- **Presentation:** Screenshots written to disk paths specified by user.

0. ### /canary
- **Usage:** Post-deploy visual monitoring. Watches the live app for console errors, performance regressions, and page failures after a deploy.
- **Prerequisites:** Deployed app URL. Browse daemon available.
- **Output:** Canary report with per-page health status (HEALTHY/DEGRADED/BROKEN), alert counts, screenshots.
- **Presentation:** Reports saved to `.gstack/canary-reports/`. Displayed inline.

### /careful
- **Usage:** Safety guardrails for destructive commands. Warns before `rm -rf`, `DROP TABLE`, force-push, `git reset --hard`.
- **Prerequisites:** Active session.
- **Output:** Warning prompts before destructive commands execute.
- **Presentation:** Inline warnings in chat. Session-scoped.

0. ### /codex
- **Usage:** OpenAI Codex CLI wrapper with three modes: Review (independent diff review), Challenge (adversarial mode), Consult (ask Codex anything).
- **Prerequisites:** Codex CLI installed. Auth configured. Git repo with diff for review mode.
- **Output:** Review report with pass/fail verdict, challenge findings, or consult response.
- **Presentation:** Output displayed inline.

0. Off ### /connect-chrome
- **Usage:** Launches GStack Browser — AI-controlled Chromium with sidebar extension. User watches every action in real time.
- **Prerequisites:** Browse daemon binary built.
- **Output:** Visible Chromium window with sidebar showing live activity feed.
- **Presentation:** Browser window opens on desktop.

### /context-restore
- **Usage:** Loads the most recent saved working context so you can pick up where you left off.
- **Prerequisites:** A previously saved context via `/context-save`.
- **Output:** Resumed context summary with title, branch, timestamp, status, remaining work.
- **Presentation:** Formatted summary displayed inline.

### /context-save
- **Usage:** Captures git state, decisions made, and remaining work so any future session can resume.
- **Prerequisites:** Active working session with git state to capture.
- **Output:** Saved context markdown file with frontmatter and sections.
- **Presentation:** Saved to `~/.gstack/projects/$SLUG/checkpoints/`. Confirmation displayed inline.

### /cso
- **Usage:** Chief Security Officer mode. Infrastructure-first security audit covering secrets, dependency supply chain, CI/CD, OWASP Top 10, STRIDE threat modeling.
- **Prerequisites:** Git repo with codebase to audit. No code changes made — report only.
- **Output:** Security Posture Report with findings, severity ratings, remediation plans.
- **Presentation:** Report displayed inline with attack surface map and risk matrix.

### /design-consultation
- **Usage:** Creates DESIGN.md as the project's design source of truth. Researches landscape, proposes complete design system (aesthetic, typography, color, layout, spacing, motion).
- **Prerequisites:** Product context (codebase, README, or `/office-hours` output).
- **Output:** DESIGN.md file with complete design system. Font+color preview page.
- **Presentation:** DESIGN.md saved to project root. Preview HTML opened in browser. Artifacts to `~/.gstack/projects/$SLUG/designs/`.

### /design-html
- **Usage:** Generates production-quality Pretext-native HTML/CSS where text reflows, heights are computed, and layouts are dynamic.
- **Prerequisites:** Design context (approved mockup, CEO plan, or user description). Browse daemon for preview.
- **Output:** Production HTML/CSS file with computed layout, responsive behavior, real text reflow.
- **Presentation:** HTML file saved to project. Opened in browser for preview.

### /design-review
- **Usage:** Designer's eye QA. Finds visual inconsistency, spacing issues, hierarchy problems, AI slop patterns, then iteratively fixes them with atomic commits.
- **Prerequisites:** Live site URL or feature branch with changes. Browse daemon. Clean git working tree.
- **Output:** Before/after screenshots, atomic fix commits, design audit findings with severity ratings.
- **Presentation:** Screenshots displayed inline. Fix commits in git history.

### /design-shotgun
- **Usage:** Standalone design exploration. Generates multiple AI design variants, opens them side-by-side in a comparison board, collects structured feedback.
- **Prerequisites:** Design brief (who, job-to-be-done, what exists, user flow, edge cases).
- **Output:** Multiple variant PNGs, comparison HTML board, approved.json with user's choice.
- **Presentation:** Comparison board opened in browser. Artifacts saved to `~/.gstack/projects/$SLUG/designs/`.

0. ### /devex-review
- **Usage:** Live developer experience audit. Uses browse tool to actually test the developer experience: navigates docs, tries getting started flow, times TTHW.
- **Prerequisites:** Developer-facing product (API, CLI, SDK, docs) with a URL to test.
- **Output:** DX scorecard with evidence (screenshots, timings).
- **Presentation:** Scorecard displayed inline with per-dimension scores and evidence screenshots.

### /diagram
- **Usage:** Turns an English description (or mermaid source) into a diagram triplet: mermaid source, an editable .excalidraw file, and rendered SVG+PNG.
- **Prerequisites:** English description of the diagram or mermaid source code.
- **Output:** Three files: mermaid source (.md), editable .excalidraw file, rendered SVG + PNG.
- **Presentation:** Files written to disk. Excalidraw file can be opened on excalidraw.com.

### /document-generate
- **Usage:** Generate missing documentation from scratch for a feature, module, or entire project. Uses the Diataxis framework (tutorial / how-to / reference / explanation).
- **Prerequisites:** Codebase to document.
- **Output:** Structured documentation files following Diataxis framework.
- **Presentation:** Documentation files written to project docs directory.

### /document-release
- **Usage:** Post-ship documentation update. Reads all project docs, cross-references the diff, updates README/ARCHITECTURE/CONTRIBUTING/CLAUDE.md, detects architecture diagram drift, polishes CHANGELOG.
- **Prerequisites:** Recently shipped code (merged PR or shipped feature). Existing project docs.
- **Output:** Updated documentation files, CHANGELOG polish, optional VERSION bump.
- **Presentation:** Files updated in-place.

### /freeze
- **Usage:** Restricts file edits to a specific directory for the session. Blocks Edit and Write outside the allowed path.
- **Prerequisites:** Active session. Directory path to restrict edits to.
- **Output:** Edit/Write blocks enforced via PreToolUse hook.
- **Presentation:** Inline block messages. Session-scoped.

0. ### /gstack-openclaw-ceo-review
- **Usage:** CEO/founder-mode plan review. Rethinks the problem, finds the 10-star product, challenges premises, expands or reduces scope. Four modes: Scope Expansion, Selective Expansion, Hold Scope, Scope Reduction.
- **Prerequisites:** A plan or proposal to review. No code changes made.
- **Output:** Review findings with scope expansion opportunities, risk analysis, edge case mapping.
- **Presentation:** Interactive review displayed inline with per-item opt-in/opt-out decisions.

0. ### /gstack-openclaw-investigate
- **Usage:** Systematic debugging with root cause investigation. Four phases: investigate, analyze, hypothesize, implement. Iron Law: no fixes without root cause.
- **Prerequisites:** Bug report, error, stack trace, or "something stopped working" report.
- **Output:** Root cause analysis, fix implementation, verification. Learning logged.
- **Presentation:** Investigation steps displayed inline. Fix committed atomically.

0. ### /gstack-openclaw-office-hours
- **Usage:** YC Office Hours mode. Six forcing questions that expose demand reality, status quo, desperate specificity, narrowest wedge, observation, and future-fit. Produces design docs, not code.
- **Prerequisites:** Product idea or design direction to explore. No code written.
- **Output:** Design document with problem statement, premise challenge, explored alternatives.
- **Presentation:** Design doc saved to `~/.gstack/projects/$SLUG/`. Displayed inline.

0. ### /gstack-openclaw-retro
- **Usage:** Weekly engineering retrospective. Analyzes commit history, work patterns, and code quality metrics with persistent history and trend tracking.
- **Prerequisites:** Git repo with commit history. Default last 7 days.
- **Output:** Retrospective report with per-person praise and growth areas, code quality metrics.
- **Presentation:** Report saved to `~/.gstack/projects/$SLUG/retros/`. Displayed inline.

### /gstack-upgrade
- **Usage:** Upgrades gstack to the latest version. Detects global vs vendored install, runs the upgrade, shows what's new.
- **Prerequisites:** Existing gstack installation.
- **Output:** Updated gstack installation. Changelog of what's new.
- **Presentation:** Upgrade output displayed inline.

### /guard
- **Usage:** Full safety mode combining destructive command warnings (`/careful`) with directory-scoped edits (`/freeze`). Two guardrails in one.
- **Prerequisites:** Active session.
- **Output:** Warning prompts before destructive commands + edit blocks outside allowed directory.
- **Presentation:** Inline warnings and blocks. Session-scoped.

0. ### /hackernews-frontpage
- **Usage:** Scrapes the Hacker News front page — titles, points, comment counts.
- **Prerequisites:** None (web access).
- **Output:** Structured data: title, points, comment count for each front page story.
- **Presentation:** Data displayed inline.

### /health
- **Usage:** Code quality dashboard. Wraps existing project tools (type checker, linter, test runner, dead code detector), computes a weighted composite 0-10 score.
- **Prerequisites:** Project with configured tooling (linter, type checker, tests).
- **Output:** Health score (0-10), per-tool results, trend data over time.
- **Presentation:** Dashboard displayed inline with scores per dimension.

### /investigate
- **Usage:** Systematic debugging with root cause investigation. Four phases: investigate, analyze, hypothesize, implement. Iron Law: no fixes without root cause.
- **Prerequisites:** Bug report, error, stack trace, or unexpected behavior.
- **Output:** Root cause analysis, fix implementation, verification. Learning logged.
- **Presentation:** Investigation steps displayed inline. Fix committed atomically.

0. ### /ios-clean
- **Usage:** Removes the DebugBridge SPM package and all `#if DEBUG` wiring from an iOS app.
- **Prerequisites:** iOS app with gstack DebugBridge installed.
- **Output:** Cleaned iOS project with DebugBridge removed.
- **Presentation:** Files modified in-place.

0. ### /ios-design-review
- **Usage:** Visual design audit for iOS apps on real hardware. Connects to a real iPhone, screenshots every screen, evaluates against Apple HIG.
- **Prerequisites:** Real iPhone connected via USB. iOS app running.
- **Output:** Per-screen design scores (0-10) with screenshots as evidence.
- **Presentation:** Screenshots and scores displayed inline.

0. ### /ios-fix
- **Usage:** Autonomous iOS bug fixer. Takes a bug found by `/ios-qa`, reads source, writes fix, rebuilds, redeploys, and verifies on real device.
- **Prerequisites:** Bug identified by `/ios-qa`. Real iPhone connected. iOS project source.
- **Output:** Fix committed atomically. Pre-bug state captured as regression test fixture.
- **Presentation:** Fix verification screenshot displayed inline.

0. ### /ios-qa
- **Usage:** Live-device iOS QA for SwiftUI apps. Connects to real iPhone via USB, reads Swift source, runs vision-driven agent loop (screenshot, analyze, decide, act, verify).
- **Prerequisites:** Real iPhone connected via USB. SwiftUI app with StateServer embedded.
- **Output:** Bug reports with screenshots, reproduction steps, and severity ratings.
- **Presentation:** Screenshots and findings displayed inline.

0. ### /ios-sync
- **Usage:** Regenerates the iOS debug bridge against the latest upstream gstack templates.
- **Prerequisites:** iOS app with existing gstack debug bridge.
- **Output:** Updated debug bridge files matching latest templates.
- **Presentation:** Files updated in-place.

### /land-and-deploy
- **Usage:** Land and deploy workflow. Merges the PR, waits for CI and deploy, verifies production health via canary checks. Takes over after `/ship`.
- **Prerequisites:** Open PR created by `/ship`. Deploy platform configured via `/setup-deploy`.
- **Output:** Merged PR, deployed production, health verification results.
- **Presentation:** Deploy status displayed inline.

### /landing-report
- **Usage:** Read-only queue dashboard for workspace-aware ship. Shows which VERSION slots are claimed by open PRs, sibling workspaces with WIP, and what slot `/ship` would pick next.
- **Prerequisites:** Git repo with PR history. No mutations.
- **Output:** Version queue snapshot showing open PRs, claimed versions, next available slot.
- **Presentation:** Dashboard table displayed inline.

### /learn
- **Usage:** Manage project learnings. Review, search, prune, and export what gstack has learned across sessions.
- **Prerequisites:** Existing learnings from previous skill sessions.
- **Output:** Learning entries displayed, pruned, or exported.
- **Presentation:** Learning list displayed inline.

### /make-pdf
- **Usage:** Turns any markdown file into a publication-quality PDF with 1in margins, page numbers, cover pages, running headers, clickable TOC, diagonal DRAFT watermark.
- **Prerequisites:** Markdown file to convert.
- **Output:** Publication-quality PDF file.
- **Presentation:** PDF saved to disk.

### /office-hours
- **Usage:** YC Office Hours. Two modes: Startup (six forcing questions) and Builder (design thinking brainstorming). Saves a design doc.
- **Prerequisites:** Product idea or design direction to explore. No code written.
- **Output:** Design document with problem statement, premise challenge, explored alternatives.
- **Presentation:** Design doc saved to `~/.gstack/projects/$SLUG/`. Displayed inline.

0. ### /open-gstack-browser
- **Usage:** Launches visible (headed) Chromium so you can watch agent actions in real time.
- **Prerequisites:** Browse daemon binary built. WSLg on Windows 11.
- **Output:** Visible Chromium window.
- **Presentation:** Browser window opens on desktop.

0. ### /pair-agent
- **Usage:** Pairs a remote AI agent with your browser. One command generates a setup key and prints instructions for the other agent to connect.
- **Prerequisites:** Browse daemon running. Remote agent that can make HTTP requests.
- **Output:** Setup key and connection instructions for the remote agent.
- **Presentation:** Instructions displayed inline.

### /plan-ceo-review
- **Usage:** CEO/founder-mode plan review. Rethinks the problem, finds the 10-star product, challenges premises. Four modes: Scope Expansion, Selective Expansion, Hold Scope, Scope Reduction.
- **Prerequisites:** A plan or proposal to review.
- **Output:** Review findings with scope expansion opportunities, risk analysis, edge cases, diagrams.
- **Presentation:** Interactive review via AskUserQuestion. Review report written to plan file.

### /plan-design-review
- **Usage:** Designer's eye plan review. Rates each design dimension 0-10, explains what would make it a 10, then fixes the plan to get there.
- **Prerequisites:** Plan with UI/UX components. Works in plan mode.
- **Output:** Design dimension scores, improvement recommendations, updated plan.
- **Presentation:** Scores and recommendations displayed inline. Plan file updated.

0. ### /plan-devex-review
- **Usage:** Interactive developer experience plan review. Explores developer personas, benchmarks against competitors, designs magical moments, traces friction points.
- **Prerequisites:** Plan for developer-facing product (APIs, CLIs, SDKs, docs).
- **Output:** DX dimension scores, competitive benchmarks, friction point analysis.
- **Presentation:** Interactive review via AskUserQuestion. Review report written to plan file.

### /plan-eng-review
- **Usage:** Eng manager-mode plan review. Locks in the execution plan — architecture, data flow, diagrams, edge cases, test coverage, performance.
- **Prerequisites:** Plan or design doc about to be implemented.
- **Output:** Architecture review with data flow diagrams, edge case mapping, test coverage plan.
- **Presentation:** Interactive review via AskUserQuestion. Review report written to plan file.

### /plan-tune
- **Usage:** Self-tuning question sensitivity + developer psychographic. Review which AskUserQuestion prompts fire across gstack skills, set per-question preferences, inspect dual-track profile.
- **Prerequisites:** Active gstack usage history.
- **Output:** Question preference settings, developer psychographic profile.
- **Presentation:** Conversational interface displayed inline.

### /qa
- **Usage:** Systematically QA tests a web application, then iteratively fixes bugs in source code with atomic commits and re-verification. Three tiers: Quick, Standard, Exhaustive.
- **Prerequisites:** Running web app URL. Browse daemon. Clean git working tree.
- **Output:** Before/after health scores, bug reports with screenshots, fix commits, ship-readiness summary.
- **Presentation:** Health scores, screenshots, and findings displayed inline. Fix commits in git history.

### /qa-only
- **Usage:** Report-only QA testing. Systematically tests a web application and produces a structured report — but never fixes anything.
- **Prerequisites:** Running web app URL. Browse daemon.
- **Output:** Structured bug report with health score, screenshots, reproduction steps, severity ratings.
- **Presentation:** Report displayed inline with screenshots. No code changes made.

### /retro
- **Usage:** Weekly engineering retrospective. Analyzes commit history, work patterns, and code quality metrics with persistent history and trend tracking.
- **Prerequisites:** Git repo with commit history. Default last 7 days.
- **Output:** Retrospective report with per-person breakdown, code quality metrics, trend analysis.
- **Presentation:** Report saved to `~/.gstack/projects/$SLUG/retros/`. Displayed inline.

### /review
- **Usage:** Pre-landing PR review. Analyzes diff against the base branch for SQL safety, LLM trust boundary violations, conditional side effects, and other structural issues.
- **Prerequisites:** Git repo with changes against base branch. PR or diff to review.
- **Output:** Review findings with severity ratings, specific file/line references, and recommendations.
- **Presentation:** Findings displayed inline with file:line references.

0. ### /scrape
- **Usage:** Pull data from a web page. First call prototypes the flow via browse primitives and returns JSON. Subsequent calls on matching intent route to a codified browser-skill (~200ms).
- **Prerequisites:** URL to scrape. Browse daemon.
- **Output:** Structured JSON data extracted from the page.
- **Presentation:** Data returned inline as JSON.

0. ### /setup-browser-cookies
- **Usage:** Imports cookies from your real Chromium browser into the headless browse session.
- **Prerequisites:** Real Chromium browser with active sessions. Browse daemon.
- **Output:** Imported cookies in browse session. Authenticated pages accessible.
- **Presentation:** Interactive domain picker displayed inline.

0. ### /setup-deploy
- **Usage:** Configures deployment settings for `/land-and-deploy`. Detects deploy platform (Fly.io, Render, Vercel, Netlify, Heroku, GitHub Actions, custom), production URL, health check endpoints.
- **Prerequisites:** Project with deploy configuration. Git repo.
- **Output:** Deploy configuration written to CLAUDE.md for automatic future deploys.
- **Presentation:** Configuration displayed inline and written to CLAUDE.md.

0. ### /setup-gbrain
- **Usage:** Sets up gbrain for the coding agent: installs the CLI, initializes a local PGLite or Supabase brain, registers MCP, captures per-remote trust policy.
- **Prerequisites:** None (handles full setup from scratch).
- **Output:** Installed gbrain CLI, initialized brain database, registered MCP server, trust policy.
- **Presentation:** Setup progress displayed inline. Configuration written to `~/.gbrain/config.json`.

### /ship
- **Usage:** Ship workflow: detect + merge base branch, run tests, review diff, bump VERSION, update CHANGELOG, commit, push, create PR.
- **Prerequisites:** Git repo with changes to ship. Tests configured. Remote origin set up.
- **Output:** Clean commit, pushed branch, created PR with changelog and version bump.
- **Presentation:** PR URL displayed inline.

0. ### /skillify
- **Usage:** Codifies the most recent successful `/scrape` flow into a permanent browser-skill on disk. Future `/scrape` calls with the same intent run the codified script in ~200ms.
- **Prerequisites:** A recent successful `/scrape` flow in the conversation.
- **Output:** `script.ts`, `script.test.ts`, and fixture file for the browser-skill.
- **Presentation:** Files saved to browser-skills directory.

### /spec
- **Usage:** Turns vague intent into a precise, executable spec in five phases. Files the issue, optionally spawns a Claude Code agent in a fresh worktree.
- **Prerequisites:** Vague intent or feature request to specify.
- **Output:** Precise spec document filed as GitHub issue. Optional worktree with agent.
- **Presentation:** Issue created on GitHub. Spec displayed inline.

### /sync-gbrain
- **Usage:** Keeps gbrain current with this repo's code and refreshes agent search guidance in CLAUDE.md. Wraps the gstack-gbrain-sync orchestrator with state management.
- **Prerequisites:** Gbrain installed and configured. Git repo to index.
- **Output:** Updated gbrain index with latest code. Updated CLAUDE.md with search guidance.
- **Presentation:** Sync progress and verdict displayed inline. CLAUDE.md updated.

### /unfreeze
- **Usage:** Clears the freeze boundary set by `/freeze`, allowing edits to all directories again.
- **Prerequisites:** An active freeze boundary from a previous `/freeze` invocation.
- **Output:** Edit restrictions removed. All directories editable again.
- **Presentation:** Confirmation displayed inline.

---

## Section 2: Pipelines and Workflows

### Planning Pipeline (run in order for new projects)

```
/office-hours → /plan-ceo-review → /plan-eng-review → /plan-design-review → /plan-devex-review → /plan-tune
```

Or run everything at once: `/autoplan`

### Build and Ship Pipeline (run in order once planning docs are locked)

```
/build → /review → /qa → /ship → /land-and-deploy
```

### Design Pipeline (optional, for new visual identity)

```
/design-consultation → /design-shotgun → /design-html → /design-review
```

### Daily Workflow

```
/context-restore → work on tasks → /context-save
```

### Post-Deploy

```
/land-and-deploy → /canary → /landing-report
```

### Investigation Workflow

```
/investigate → /review → /qa → /ship
```

---

## Section 3: Short Name Aliases

These are the same command — use whichever you prefer:

| Short | Full |
|---|---|
| `/learn` | `/gstack-learn` |
| `/context-save` | `/gstack-context-save` |
| `/upgrade` | `/gstack-upgrade` |

---

## Section 4: Related Documentation

| Document | Purpose |
|---|---|
| `gstack-reference.md` | GStack system overview, brain sync, artifact locations |
| `gbrain-reference.md` | GBrain commands, health score, troubleshooting |
| `opencode-reference.md` | OpenCode configuration, keyboard shortcuts, MCP |
