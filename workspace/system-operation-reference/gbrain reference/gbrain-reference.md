# GBrain Reference

## What is GBrain?

GBrain is the memory layer. It stores and searches your project documents (ADRs, specs, docs) and code (Python, YAML) as searchable vector memories.

**Engine:** PGlite (local database on your PC)  
**Database:** `~/.gbrain/brain.pglite/`  
**Pages:** 700 (10 sources across all projects, 2901 chunks, all embedded)

---

## Section 1: Day-to-Day Commands

### Search GBrain

**Apply in:** OpenCode web UI (just ask the agent)

> "Search GBrain for onboarding"
> "What did we decide about authentication?"
> "Where is the LoginForm code?"

The agent calls GBrain automatically. You don't need to run any command.

**Apply in:** WSL terminal (any directory)

```bash
gbrain query "onboarding"
```

---

### Check GBrain Health

**Apply in:** WSL terminal  
**Path:** any directory (GBrain is on PATH)

```bash
gbrain doctor
```

Expected result: Health score ~90/100.

---

### List What GBrain Knows About

**Apply in:** WSL terminal  
**Path:** any directory

```bash
gbrain sources list
```

Expected result: Shows `default` (143 pages federated), `project-library` (318 pages), `mb-3-cs` (165 pages), plus 7 other project sources.

---

### Sync GStack Working State to Git

**Apply in:** WSL terminal  
**Path:** any directory

```bash
bash /mnt/c/mybizz/gstack/bin/gstack-brain-sync --once
```

This pushes learnings, plans, and retros to the private git repo.

---

### Open GBrain Folder in Windows Explorer

**Apply in:** WSL terminal  
**Path:** any directory

```bash
explorer.exe "$(wslpath -w /home/dev-p/.gbrain/)"
```

---

## Section 2: Occasional Maintenance

### Re-embed After Adding New Content

If you add new files to project-library or mb-3-cs and want GBrain to index them:

**Apply in:** WSL terminal  
**Path:** any directory

```bash
# Re-index project-library docs
gbrain sync --repo /mnt/c/mybizz/project-library

# Embed new/changed chunks
gbrain embed --stale
```

**Note:** Code files (Python) from mb-3-cs require a different approach. See "Index Code Files" below.

---

### Index Code Files from mb-3-cs

GBrain's `sync` command only handles markdown files by default. Code files require a direct import.

**Apply in:** WSL terminal  
**Path:** `/mnt/c/mybizz/gbrain`

```bash
cd /mnt/c/mybizz/gbrain
export OPENAI_API_KEY="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"
bun -e "
import { PGLiteEngine } from './src/core/pglite-engine.ts';
import { importCodeFile } from './src/core/import-file.ts';
import { isCodeFilePath } from './src/core/sync.ts';
import { readFileSync, readdirSync, statSync } from 'fs';
import { relative, join } from 'path';
const REPO = '/mnt/c/mybizz/mb-3-cs';
const engine = new PGLiteEngine();
await engine.connect({ engine: 'pglite', database_path: '/home/dev-p/.gbrain/brain.pglite' });
function walk(dir) {
  const files = [];
  for (const entry of readdirSync(dir, { withFileTypes: true })) {
    if (entry.name.startsWith('.') || entry.name === 'node_modules' || entry.name === '__pycache__') continue;
    const full = join(dir, entry.name);
    if (entry.isDirectory()) files.push(...walk(full));
    else if (entry.isFile()) {
      const rel = relative(REPO, full);
      if (isCodeFilePath(rel) && !rel.endsWith('.yaml') && !rel.endsWith('.yml')) {
        const stat = statSync(full);
        if (stat.size < 5_000_000) files.push(full);
      }
    }
  }
  return files;
}
const codeFiles = walk(REPO);
console.log('Found', codeFiles.length, 'code files');
let imported = 0, skipped = 0;
for (const file of codeFiles) {
  const rel = relative(REPO, file);
  try {
    const content = readFileSync(file, 'utf-8');
    const result = await importCodeFile(engine, rel, content, { noEmbed: true, force: false });
    if (result.status === 'imported') imported++; else skipped++;
  } catch (e) { console.error('Error:', rel, e.message); }
}
console.log('Imported:', imported, 'Skipped:', skipped);
await engine.disconnect();
"
```

Then embed:

```bash
gbrain embed --stale
```

---

### Fix OPENAI_API_KEY Error

If you see `OPENAI_API_KEY environment variable is missing`:

**Apply in:** WSL terminal  
**Path:** any directory

```bash
source ~/.bashrc
```

Then retry the command that failed.

---

### Check GBrain + GStack Sync Status

**Apply in:** WSL terminal  
**Path:** any directory

```bash
bash /mnt/c/mybizz/gstack/bin/gstack-gbrain-detect
```

Expected result: JSON showing `gbrain_on_path: true`, `gbrain_engine: "pglite"`, `gbrain_doctor_ok: true`, `gstack_brain_sync_mode: "full"`, `gstack_brain_git: true`.

---

## Section 3: OpenCode Slash Commands

GBrain-related slash commands are typed in the OpenCode web UI chat.

| Command | What it does |
|---|---|
| `/setup-gbrain` | Install and configure GBrain (one-time setup) |
| `/setup-gbrain doctor` | Re-run GBrain health check |
| `/sync-gbrain` | Sync GBrain state to private git repo |

For all other slash commands (context, learn, pipelines), see `gstack-skills-reference.md`.

---

## Section 4: How GBrain Works (Reference)

### Sources

GBrain organizes content into sources:

| Source | What it contains | Pages |
|---|---|---|
| `default` | Federated: all project-library + code sources | 143 |
| `project-library` | mb-3-cs project-library docs | 318 |
| `mb-3-cs` | mb-3-cs application code | 165 |
| `gbrain-repo` | GStack working state (learnings, plans, retros) | 0 |
| `mb-3-cs` | Registered but code imported to `default` | 0 |

### GBrain Search Guidance (for the agent)

The agent uses GBrain automatically when:
- The question is semantic ("what did we decide about X?")
- Looking up code symbols (`LoginForm`, `DashboardForm`)
- Searching across repos

The agent uses Grep/Glob when:
- Searching for exact strings
- Finding files by pattern

### Health Score

**Current score: ~90/100** — Normal for a well-maintained documentation + code brain.

| Component | Score | Why it's low |
|---|---|---|
| `embed` | 35/35 | Perfect |
| `links` | 1/25 | ADRs don't use entity directory patterns |
| `timeline` | 0/15 | ADRs don't have date-based timelines |
| `orphans` | 0/15 | ADRs are standalone documents |
| `dead-links` | 10/10 | Perfect |

The low scores don't indicate any functional problem.

### Expected Warnings (ignore these)

| Warning | Why |
|---|---|
| `pgvector: Could not check` | PGlite limitation |
| `jsonb_integrity: Could not check` | PGlite limitation |
| `graph_coverage: Entity link coverage 0%` | ADRs don't use entity patterns |
| `brain_score: 49/100` | Scoring designed for entity-based knowledge |
| `resolver_health: Could not find skills directory` | Non-critical |

### Environment Variables

GBrain requires `OPENAI_API_KEY` for embedding. It's saved in `~/.bashrc` and auto-loaded in new WSL sessions.

If embedding fails, run `source ~/.bashrc` in WSL.

### GStack Brain Sync

GStack brain sync pushes working state to `https://github.com/sassycomapp/gbrain-repo.git`.

| Privacy mode | What syncs |
|---|---|
| `off` | Nothing |
| `artifacts-only` | Plans, designs, retros, learnings |
| `full` | Everything (current setting) |

What does NOT sync: credentials, browser profiles, caches, question preferences.

---

## Section 5: File Locations

| Item | Location |
|---|---|
| GBrain CLI | On PATH (installed globally) |
| GBrain config | `~/.gbrain/config.json` |
| GBrain database | `~/.gbrain/brain.pglite/` |
| GStack home | `~/.gstack/` |
| GStack brain remote | `~/.gstack-brain-remote.txt` |
| GStack brain worktree | `~/.gstack-brain-worktree/` |
| GStack bin scripts | `/mnt/c/mybizz/gstack/bin/` |
| OPENAI_API_KEY | `~/.bashrc` |

### Open GStack Folder in Windows Explorer

**Apply in:** WSL terminal  
**Path:** any directory

```bash
explorer.exe "$(wslpath -w /home/dev-p/.gstack/)"
```

**Important:** Always use `wslpath -w` from WSL. Using a Linux path directly with `explorer.exe` opens the wrong location (OneDrive Documents).

---

## Section 6: Troubleshooting

### GBrain CLI not found

**Apply in:** WSL terminal

```bash
which gbrain
# If not found:
export PATH="$HOME/.bun/bin:$PATH"
gbrain doctor
```

### Extract commands return 0 pages

You need `--source db` because content is in the PGlite database, not on disk:

**Apply in:** WSL terminal  
**Path:** any directory

```bash
gbrain extract links --source db
gbrain extract timeline --source db
```

### GStack brain sync issues

**Apply in:** WSL terminal  
**Path:** any directory

```bash
bash /mnt/c/mybizz/gstack/bin/gstack-brain-sync --status
cd ~/.gstack && git status
cd ~/.gstack && git fetch origin
```

### Stale sync lock

If you see `Another sync is in progress`:

**Apply in:** WSL terminal  
**Path:** `/mnt/c/mybizz/gbrain`

```bash
cd /mnt/c/mybizz/gbrain
bun -e "
import { PGLiteEngine } from './src/core/pglite-engine.ts';
const engine = new PGLiteEngine();
await engine.connect({ engine: 'pglite', database_path: '/home/dev-p/.gbrain/brain.pglite' });
await engine.executeRaw(\"DELETE FROM gbrain_cycle_locks WHERE id = 'gbrain-sync'\");
console.log('Lock cleared');
await engine.disconnect();
"
```

---

## Related Documentation

| Document | Purpose |
|---|---|
| `workspace/gbrain/gbrain-overview.md` | High-level overview |
| `workspace/gbrain/gbrain-operating-model.md` | Authority model and rules |
| `workspace/gbrain/gbrain-source-tiers.md` | Source hierarchy |
| `workspace/gbrain/gbrain-sync-policy.md` | Freshness rules |
| `gstack-outputs/gbrain-assessment.md` | Full assessment from 2026-06-10 |
