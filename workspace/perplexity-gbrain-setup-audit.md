Here’s the plan: first we establish the **prescribed target state** from the GStack GBrain guide, then we run a focused set of local and remote checks against your WSL/OpenCode setup, Supabase GitHub integration, and repo structure to see whether your current setup actually supports “perpetually up to date” behavior. The key early finding is that GStack’s own guide does **not** describe a fully automatic repo-ingest daemon; it describes `/setup-gbrain` for onboarding and `/sync-gbrain` as the command you run whenever you want fresh code indexed, with stateful incremental sync and optional artifacts sync layered on top. [github](https://github.com/marcelodosreis/sassy)

## Target state

According to the GStack guide, the intended steady-state setup has several parts working together: GBrain installed, initialized to either local PGLite, Supabase, or split-engine mode; the repo registered as a GBrain source with a `.gbrain-source` pin at repo root; and `/sync-gbrain` used to keep code, memory, and brain-sync stages current.  The guide is explicit that `/setup-gbrain` is one-time onboarding, while `/sync-gbrain` is the routine verb for getting fresh repo changes into GBrain, and that sync state is persisted so repeat runs are incremental. [github](https://github.com/marcelodosreis/sassy)

For Supabase’s GitHub integration, the “working directory” must be the path from repo root to the directory that contains the `supabase/` folder, or `.` if `supabase/` is at the repo root.  That matters because if your Supabase GitHub integration points at the wrong directory, the integration can still appear connected while not tracking the actual `supabase/` config and migrations you expect. [github](https://github.com/supabase/supabase)

## What we need to verify

We need to separate three different sync concerns, because they are easy to conflate. [github](https://github.com/supabase/supabase)

- **GBrain code indexing:** whether your current repo in WSL is registered as a GBrain source, pinned with `.gbrain-source`, and successfully synced through `/sync-gbrain`. [github](https://github.com/marcelodosreis/sassy)
- **GStack memory/artifacts sync:** whether `~/.gstack/` curated memory is being pushed to a private repo, which is optional and distinct from code indexing. [github](https://github.com/marcelodosreis/sassy)
- **Supabase GitHub integration:** whether the dashboard integration is correctly connected to `sassycomapp/project-library`, with the correct working directory for the `supabase/` folder and branching/deploy behavior matching your expectations. [github](https://github.com/supabase/supabase)

This distinction is important because “perpetually up to date with GitHub” is not guaranteed merely by linking Supabase to the repo. Supabase’s GitHub integration watches the repository for Supabase deployment/branching purposes, while GStack’s guide still expects repo code ingestion into GBrain to happen through `/sync-gbrain`. [github](https://github.com/supabase/supabase)

## Investigation checks

I’d structure the investigation as a checklist you can run and save to a file, grouped by evidence type. The goal is to produce a clear pass/fail/unknown matrix against the prescribed setup in `USING_GBRAIN_WITH_GSTACK.md`. [github](https://github.com/marcelodosreis/sassy)

### 1. Local GBrain install and engine state
Run these in WSL/OpenCode:

- `which gbrain`
- `gbrain --version`
- `gbrain doctor --json`
- `cat ~/.gbrain/config.json`
- `ls -la ~/.gstack/`
- `test -f ~/.gstack/gbrain-repo-policy.json && cat ~/.gstack/gbrain-repo-policy.json`

Why: the guide treats install path, doctor status, engine type, and repo trust policy as foundational signals of whether setup completed correctly. [github](https://github.com/marcelodosreis/sassy)

### 2. Repo-level source wiring
From the root of `project-library` in WSL:

- `git rev-parse --show-toplevel`
- `ls -la`
- `find .. -name .gbrain-source -print`
- `test -f .gbrain-source && cat .gbrain-source`
- `gbrain sources current --json`
- `gbrain sources list --json` or the nearest available equivalent
- `test -f CLAUDE.md && grep -n "GBrain Search Guidance" CLAUDE.md`

Why: the guide says `/sync-gbrain` writes a `.gbrain-source` pin file in the repo root and conditionally writes a `## GBrain Search Guidance` block to `CLAUDE.md` only if round-trip capability checks pass.  If these are absent, then your setup likely does not yet meet the prescribed per-repo integration state. [github](https://github.com/marcelodosreis/sassy)

### 3. Sync execution evidence
Still in the repo:

- `/sync-gbrain --dry-run`
- `/sync-gbrain --code-only`
- `cat ~/.gstack/.gbrain-sync-state.json`
- `ls -la ~/.gstack/ | grep brain`
- inspect any recent sync logs or transcript folders under `~/.gstack/transcripts/`

Why: the guide makes `/sync-gbrain` the canonical update mechanism and stores persistent sync state under `~/.gstack/.gbrain-sync-state.json`.  A successful dry-run and code-only pass will tell us whether the current setup can actually refresh the repo index on demand. [github](https://github.com/marcelodosreis/sassy)

### 4. Embeddings and semantic quality
Check environment availability in WSL:

- `env | grep -E 'VOYAGE|OPENAI|GSTACK_OPENAI|GSTACK_ANTHROPIC'`
- after sync, run a semantic query such as `gbrain search "unique phrase from this repo"`

Why: the guide notes that semantic search quality depends on embeddings, with `VOYAGE_API_KEY` preferred for code, otherwise OpenAI fallback; without a provider key, structural import may work while semantic search degrades. [github](https://github.com/marcelodosreis/sassy)

### 5. Supabase GitHub integration alignment
Manual dashboard checks to capture in your notes:

- Confirm project `fnjcssmqljshckdksvam` is connected to `sassycomapp/project-library`.
- Record the configured **Working directory**.
- In the repo, locate the actual `supabase/` folder path.
- Compare the two.

Expected rule: if `supabase/` is in repo root, working directory should be `.`; if it is under something like `apps/web/supabase/`, working directory should be `apps/web`. [github](https://github.com/supabase/supabase)

### 6. “Perpetually up to date” gap analysis
Check whether anything currently triggers `/sync-gbrain` automatically on repo change:

- inspect `.github/workflows/`
- inspect any local shell aliases/functions for `sync-gbrain`
- inspect OpenCode task hooks, repo hooks, or wrapper scripts you use in WSL
- inspect git hooks under `.git/hooks/`

Why: the guide itself describes manual or explicit invocation of `/sync-gbrain`; it does not claim a built-in automatic watcher that mirrors every GitHub commit into GBrain continuously.  So if your intent is continuous freshness, we need to verify whether you already added automation outside the default GStack flow. [github](https://github.com/marcelodosreis/sassy)

## How we’ll assess results

I suggest we record results in a matrix with four columns:

| Check | Expected by guide | Actual result | Status |
|---|---|---|---|
| `gbrain doctor --json` | healthy/usable engine  [github](https://github.com/marcelodosreis/sassy) | your output | Pass/Fail |
| `.gbrain-source` in repo root | present after source wire-up/sync  [github](https://github.com/marcelodosreis/sassy) | your output | Pass/Fail |
| `CLAUDE.md` contains GBrain Search Guidance | present only if round-trip works  [github](https://github.com/marcelodosreis/sassy) | your output | Pass/Fail |
| `/sync-gbrain --dry-run` | completes cleanly  [github](https://github.com/marcelodosreis/sassy) | your output | Pass/Fail |
| embeddings env | provider key available for semantic search  [github](https://github.com/marcelodosreis/sassy) | your output | Pass/Fail |
| Supabase working directory | matches parent of `supabase/` dir  [github](https://github.com/supabase/supabase) | your output | Pass/Fail |
| auto-update mechanism | not guaranteed by default guide  [github](https://github.com/marcelodosreis/sassy) | your output | Gap/Present |

That will let us answer two distinct questions cleanly: “Is the setup compliant with the guide?” and “Does it satisfy your stronger intent of perpetual freshness?” [github](https://github.com/supabase/supabase)

## Likely outcome and next step

My expectation is that one of three situations will emerge. [github](https://github.com/marcelodosreis/sassy)

- **Guide-compliant but not perpetual:** setup is correct, `/sync-gbrain` works, but there is no automation to run it after GitHub or local commits. [github](https://github.com/marcelodosreis/sassy)
- **Partially wired:** GBrain is installed and maybe linked to Supabase, but repo source pinning, trust policy, CLAUDE guidance, or sync state is missing. [github](https://github.com/marcelodosreis/sassy)
- **Supabase mismatch:** GitHub integration exists, but the working directory does not match the real `supabase/` folder location, so the dashboard connection is not aligned with repo layout. [github](https://github.com/supabase/supabase)

The best next step is for me to give you the exact command checklist in a copy-paste block, in the order you should run it in WSL so you can save every result to a file and we can do a precise compliance assessment against the guide.  Would you like that as a single bash-ready investigation script or as a numbered manual checklist? [github](https://github.com/marcelodosreis/sassy)