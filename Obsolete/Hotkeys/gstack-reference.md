# GStack Reference

## What is GStack?

GStack is the workflow layer. It provides slash commands (skills) for planning, building, reviewing, and shipping software.

**GBrain** = memory (stores and searches)  
**GStack** = workflow (plans, builds, ships)

---
## Opening the GStack projects folder
[Gstack Artifacts Output Folder - Do not remove this section]

**From: PowerShell**
explorer.exe "\\wsl.localhost\Ubuntu\home\dev-p\.gstack\projects\"

**From: WSL**	
explorer.exe "$(wslpath -w /home/dev-p/.gstack/projects/)"

**From: Windows Explorer**	
\\wsl.localhost\Ubuntu\home\dev-p\.gstack\projects\

---

## Skills Installation

**Location:** `~/.config/opencode/skills/` (symlinked to `/mnt/c/_mb2-cs-app/skills/`)

**Total skills:** 59 (54 top-level + 4 openclaw + 1 browser-skills)

**How it works:** All GStack skills are symlinked to the global OpenCode skills directory. This makes them available in every project automatically. OpenCode discovers skills from `~/.config/opencode/skills/<name>/SKILL.md`.

**To verify:**
```bash
ls ~/.config/opencode/skills/ | wc -l
```

**To update skills:**
```bash
# Skills are symlinked — updates to the source files are automatic
# Just edit the SKILL.md in /mnt/c/_mb2-cs-app/skills/
```

**To undo:**
```bash
rm -rf ~/.config/opencode/skills/
```

---

## Section 1: Day-to-Day Commands

### Start a Work Session

**Apply in:** OpenCode web UI (type in chat)

```
/context-restore
```

This restores your previous working context so you can resume where you left off.

---

### End a Work Session

**Apply in:** OpenCode web UI (type in chat)

```
/context-save
```

This saves your current progress, decisions, and remaining tasks.

---

### Save Something Useful You Learned

**Apply in:** OpenCode web UI (type in chat)

```
/gstack-learn
```

Or just tell the agent: "Remember that the Anvil YAML files are read-only."

---

### Ask the Agent to Search GBrain

**Apply in:** OpenCode web UI (just ask naturally)

> "Search GBrain for onboarding"
> "What did we decide about authentication?"
> "Where is the LoginForm code?"

The agent calls GBrain automatically. You don't need to run any command.

---

### Run a Health Check

**Apply in:** OpenCode web UI (type in chat)

```
/health
```

---

## Section 2: Planning Pipeline

Run these in order for any new project. Each skill feeds the next.

**Apply in:** OpenCode web UI (type in chat)

```
/office-hours → /plan-ceo-review → /plan-eng-review → /plan-design-review → /plan-devex-review → /plan-tune
```

| Command | What it does |
|---|---|
| `/office-hours` | Starting point. YC partner grilling on demand reality. Produces a design doc. |
| `/plan-ceo-review` | Product thinking. Asks what the 10-star product is. Four modes: Scope Expansion, Selective Expansion, Hold Scope, Scope Reduction. |
| `/plan-eng-review` | Technical architecture. Data flow, system boundaries, edge cases, failure modes. Produces engineering plan + test plan. |
| `/plan-design-review` | Design completeness. Rates 7 dimensions (0–10). |
| `/plan-devex-review` | Developer experience. Maintainability, setup friction, handoff quality. |
| `/plan-tune` | Tighten the plan before execution. Reduce ambiguity. |

Or run everything at once:

```
/autoplan
```

---

## Section 3: Build and Ship Pipeline

Run in order once planning docs are locked.

**Apply in:** OpenCode web UI (type in chat)

```
/build → /review → /qa → /ship → /land-and-deploy
```

| Command | What it does |
|---|---|
| `/build` | Main coding skill. Takes the plan and writes/edits code. |
| `/review` | Paranoid staff engineer. Audits for security, race conditions, broken invariants. |
| `/qa` | Browser testing (Playwright). Reads git diff, tests changed pages. |
| `/qa-only` | Same as `/qa` but report only — no code changes. |
| `/ship` | Creates GitHub PR. Syncs with main, runs tests, audits coverage. |
| `/land-and-deploy` | Merges PR, waits for CI, runs canary checks. Requires `/setup-deploy` first. |
| `/landing-report` | Post-landing report summarizing outcomes. |

---

## Section 4: Design Pipeline

Use when building a new visual identity or refining UI.

**Apply in:** OpenCode web UI (type in chat)

```
/design-consultation → /design-shotgun → /design-html → /design-review
```

| Command | What it does |
|---|---|
| `/design-consultation` | Builds complete design system. Researches competitors, proposes visual identity. Writes `DESIGN.md`. |
| `/design-shotgun` | Generates 3 visual variants. Opens comparison board. |
| `/design-html` | Converts mockup to production HTML. Live-reload preview. |
| `/design-review` | 80-item visual checklist. Fix loop with before/after screenshots. |

---

## Section 5: Debugging and Investigation

**Apply in:** OpenCode web UI (type in chat)

| Command | What it does |
|---|---|
| `/investigate` | Root-cause debugger. No fixes without proven root cause. |
| `/health` | Quick health check. |
| `/cso` | OWASP Top 10 + STRIDE security audit. |
| `/benchmark` | Page load times and Core Web Vitals. |
| `/canary` | Post-deploy monitoring. Watches for console errors. |

---

## Section 6: Browser Skills

All browser skills use Playwright with Chromium.

**Apply in:** OpenCode web UI (type in chat)

| Command | What it does |
|---|---|
| `/browse` | Headless browser engine (used by `/qa`, `/design-review`, etc.) |
| `/open-gstack-browser` | Visible browser so you can watch agent actions. |
| `/setup-browser-cookies` | Import cookies for testing authenticated pages. |
| `/scrape` | Structured data extraction from web pages. |

If you hit missing library errors on WSL2:

**Apply in:** WSL terminal  
**Path:** any directory

```bash
npx playwright install --with-deps chromium
```

---

## Section 7: Safety Skills

**Apply in:** OpenCode web UI (type in chat)

| Command | What it does |
|---|---|
| `/careful` | Warns before destructive commands (`rm -rf`, `DROP TABLE`, force-push). |
| `/freeze [path]` | Locks edits to one directory. |
| `/guard` | Combines `/careful` + `/freeze`. Maximum safety. |
| `/unfreeze` | Removes the freeze boundary. |

---

## Section 8: Maintenance and Operations

**Apply in:** OpenCode web UI (type in chat)

| Command | What it does |
|---|---|
| `/retro` | Weekly retrospective from commit history. |
| `/document-release` | Cross-references docs against latest diff. |
| `/setup-deploy` | One-time config for `/land-and-deploy`. |
| `/gstack-upgrade` | Upgrade GStack to latest version. |
| `/upgrade` | Shorter upgrade entry point. |
| `/make-pdf` | Produce PDF artifact. |
| `/skillify` | Turn a workflow into a reusable skill. |
| `/devex-review` | Audit repo for developer-experience issues. |
| `/pair-agent` | Interactive pairing for implementation/debugging. |

---

## Section 9: GStack Brain Sync

GStack brain sync pushes working state (learnings, plans, retros) to a private git repo for cross-machine memory.

**Remote:** `https://github.com/sassycomapp/gbrain-repo.git`  
**Sync mode:** `full`  
**Worktree:** `~/.gstack-brain-worktree/`

### Sync Now

**Apply in:** WSL terminal  
**Path:** any directory

```bash
bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-brain-sync --once
```

### Check Sync Status

**Apply in:** WSL terminal  
**Path:** any directory

```bash
bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-brain-sync --status
```

### Check Both GBrain and GStack Status

**Apply in:** WSL terminal  
**Path:** any directory

```bash
bash /mnt/c/_mb2-cs-app/gstack/bin/gstack-gbrain-detect
```

---

## Section 10: Artifact Locations

### Where GStack Skills Save by Default

GStack skills save to `~/.gstack/projects/$SLUG/` internally. This is the GStack working directory.

| Skill | Saves to |
|---|---|
| `/office-hours` | `~/.gstack/projects/$SLUG/` |
| `/plan-ceo-review` | `~/.gstack/projects/$SLUG/ceo-plans/` |
| `/plan-eng-review` | `~/.gstack/projects/$SLUG/` |
| `/design-consultation` | `~/.gstack/projects/$SLUG/designs/` + `DESIGN.md` in project root |
| `/context-save` | `~/.gstack/projects/$SLUG/.context/` |
| `/retro` | `~/.gstack/projects/$SLUG/` |

### Where to Save in project-library

Per `AGENTS.md`, GStack outputs should be saved to `gstack-outputs/`. **Skills don't do this automatically — you copy artifacts manually.**

| Artifact | Save to |
|---|---|
| Office-hours reports | `gstack-outputs/` |
| CEO review reports | `gstack-outputs/` |
| Engineering review reports | `gstack-outputs/` |
| Design consultation plans | `gstack-outputs/` |
| Design review reports | `gstack-outputs/` |
| Dev plans | `gstack-outputs/` |
| Handoff notes | `gstack-outputs/` |
| DESIGN.md | `docs/` |
| Wireframes, mockups | `wireframes/` |
| ADRs | `adr/` |

### Open GStack Projects Folder in Windows Explorer

**Apply in:** WSL terminal  
**Path:** any directory

```bash
explorer.exe "$(wslpath -w /home/dev-p/.gstack/projects/)"
```

**Important:** Always use `wslpath -w` from WSL. Using a Linux path directly opens the wrong location (OneDrive Documents).

---

## Section 11: Short Name Aliases

These are the same command — use whichever you prefer:

| Short | Full |
|---|---|
| `/learn` | `/gstack-learn` |
| `/context-save` | `/gstack-context-save` |
| `/upgrade` | `/gstack-upgrade` |

---

## Section 12: File Locations

| Item | Location |
|---|---|
| GStack repo | `/mnt/c/_mb2-cs-app/gstack/` |
| GStack bin scripts | `/mnt/c/_mb2-cs-app/gstack/bin/` |
| GStack skills | `/mnt/c/_mb2-cs-app/gstack/.opencode/skills/` |
| GStack home | `~/.gstack/` |
| GStack brain remote | `~/.gstack-brain-remote.txt` |
| GStack brain worktree | `~/.gstack-brain-worktree/` |
| OpenCode commands | `/mnt/c/_mb2-cs-app/project-library/.opencode/commands/` |
| GStack outputs | `/mnt/c/_mb2-cs-app/project-library/gstack-outputs/` |

### Open GStack Home in Windows Explorer

**Apply in:** WSL terminal  
**Path:** any directory

```bash
explorer.exe "$(wslpath -w /home/dev-p/.gstack/)"
```

---

## Section 13: Recommended Workflows

### Starting a New Project

1. `/office-hours` — define the problem
2. `/plan-ceo-review` — product decisions
3. `/plan-eng-review` — technical architecture
4. `/plan-design-review` — design review
5. `/plan-devex-review` — developer experience
6. `/plan-tune` — tighten the plan
7. Copy artifacts from `~/.gstack/projects/$SLUG/` to `gstack-outputs/`

### Building a Feature

1. `/build` — implement
2. `/review` — audit
3. `/qa` — test
4. `/ship` — PR

### Design Workflow

1. `/design-consultation` — design system
2. `/design-shotgun` — variants
3. `/design-html` — production HTML
4. `/design-review` — audit and fix

### Daily Workflow

1. `/context-restore` — resume
2. Work on tasks
3. `/context-save` — save progress

### Post-Deploy

1. `/land-and-deploy` — merge and deploy
2. `/canary` — monitor
3. `/landing-report` — summarize

---

## Related Documentation

| Document | Purpose |
|---|---|
| `docs/gbrain-operating-guide.md` | GBrain commands, health score, troubleshooting |
| `workspace/gbrain/gbrain-overview.md` | GBrain overview |
| `workspace/gstack/gstack-operating-model.md` | GStack operating model |
| `workspace/gstack/gstack-workflows.md` | GStack workflow details |
| `workspace/gstack/gstack-handoff-contract.md` | Handoff contract |
| `gstack-outputs/gbrain-assessment.md` | Full GBrain assessment |
