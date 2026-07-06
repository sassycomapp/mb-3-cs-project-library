# OpenCode Reference

## What is OpenCode?

OpenCode is the AI coding agent that runs your workspace. It's the interface where you type prompts, run slash commands, and interact with the agent.

**You use OpenCode to:** talk to the agent, run GStack skills, search GBrain, manage sessions.

**Interface:** Web UI only (`opencode web --hostname 0.0.0.0` from WSL)

**Two modes:**
- **Build mode** (default in mb-3-cs) — agent can edit files and run commands
- **Plan mode** (default in project-library) — agent can only read and suggest, no changes

Switch between modes by pressing **Tab**.

---

## Section 1: Day-to-Day Usage

### Start OpenCode

**Apply in:** WSL terminal  
**Path:** the project directory you want to work in

```bash
# For project-library (planning)
cd /mnt/c/mybizz/project-library
opencode web --hostname 0.0.0.0

# For mb-3-cs (code)
cd /mnt/c/mybizz/mb-3-cs
opencode web --hostname 0.0.0.0
```

Then open your browser to `http://localhost:3000` (or whatever port OpenCode shows).

---

### Ask the Agent a Question

**Apply in:** OpenCode web UI (type in the chat box)

Just type naturally:

> "What did we decide about onboarding?"
> "Search GBrain for login authentication"
> "Where is the LoginForm code?"
> "Explain how the revenue report works"

The agent will use GBrain, Grep, or file reading as needed.

---

### Switch Between Plan and Build Mode

**Apply in:** OpenCode web UI

Press **Tab** to toggle between:
- **Plan mode** — agent can only read and suggest (safe)
- **Build mode** — agent can edit files and run commands

The current mode is shown in the bottom-right corner.

---

### Run a Slash Command

**Apply in:** OpenCode web UI (type in the chat box)

Type `/` followed by the command name:

```
/context-restore
/build
/health
/design-consultation
```

See Section 3 for the full list of slash commands.

---

### Start a New Session

**Apply in:** OpenCode web UI

Press **Ctrl+X** then **N**

---

### Switch to a Different Session

**Apply in:** OpenCode web UI

Press **Ctrl+X** then **L**

---

### Save Your Work

**Apply in:** OpenCode web UI (type in chat)

```
/context-save
```

---

### Restore Previous Work

**Apply in:** OpenCode web UI (type in chat)

```
/context-restore
```

---

### Undo the Agent's Last Change

**Apply in:** OpenCode web UI (type in chat)

```
/undo
```

You can run `/undo` multiple times to undo multiple changes.

---

### Redo an Undone Change

**Apply in:** OpenCode web UI (type in chat)

```
/redo
```

---

### Interrupt the Agent

**Apply in:** OpenCode web UI

Press **Ctrl+G** to stop the agent mid-response.

---

## Section 2: Configuration

**Important:** OpenCode reads the model from the **UI selector**, not from config files. The `model` field in config files is a fallback/initial value only.

### Config Files (Web UI Only)

| # | File | Location | Purpose |
|---|---|---|---|
| 1 | `opencode.jsonc` | `~/.config/opencode/` (WSL) | Global config (provider, agents, MCP, permissions) |
| 2 | `AGENTS.md` | `~/.config/opencode/` (WSL) | Global behavior rules |
| 3 | `agents.md` | `project-library/` | Project rules |
| 4 | `opencode.json` | `project-library/` | Project config (default agent: plan) |
| 5 | `build.md` | `project-library/.opencode/agents/` | Build agent definition |
| 6 | `plan.md` | `project-library/.opencode/agents/` | Plan agent definition |
| 7 | `agents.md` | `mb-3-cs/` | Project rules |
| 8 | `opencode.json` | `mb-3-cs/` | Project config (default agent: build) |
| 9 | `build.md` | `mb-3-cs/.opencode/agents/` | Build agent definition |
| 10 | `plan.md` | `mb-3-cs/.opencode/agents/` | Plan agent definition |

---

### Skills Installation

**Location:** `~/.config/opencode/skills/` (symlinked to `/mnt/c/mybizz/skills/`)

**Total skills:** 59 (54 top-level + 4 openclaw + 1 browser-skills)

**How it works:** All GStack skills are symlinked to the global OpenCode skills directory. This makes them available in every project automatically. OpenCode discovers skills from `~/.config/opencode/skills/<name>/SKILL.md`.

**To verify:**
```bash
ls ~/.config/opencode/skills/ | wc -l
```

**To update skills:**
```bash
# Skills are symlinked — updates to the source files are automatic
# Just edit the SKILL.md in /mnt/c/mybizz/skills/
```

**To undo:**
```bash
rm -rf ~/.config/opencode/skills/
```

---

### File 1: WSL Global Config

**Path:** `/home/dev-p/.config/opencode/opencode.jsonc`

**To read in WSL:**
```bash
cat ~/.config/opencode/opencode.jsonc
```

**To edit in WSL:**
```bash
nano ~/.config/opencode/opencode.jsonc
```

**Current contents:**
```json
{
  "default_agent": "plan",
  "provider": {
    "openrouter": {
      "api_key": "sk-or-v1-...",
      "models": {
        "xiaomi/mimo-v2.5-pro": { "name": "MiMo-V2.5-Pro" }
      }
    }
  },
  "agents": {
    "plan": { "description": "...", "mode": "primary" },
    "build": { "description": "...", "mode": "primary" }
  },
  "shell": { "path": "/bin/bash", "args": ["-l"] },
  "permission": {
    "edit": { "*": "ask" },
    "webfetch": "ask",
    "bash": { "*": "ask" },
    "external_directory": { "C:\\mybizz\\**": "allow" }
  },
  "mcp": {
    "gbrain": {
      "type": "local",
      "command": ["gbrain", "serve"],
      "enabled": true,
      "environment": { "OPENAI_API_KEY": "{env:OPENAI_API_KEY}" }
    }
  }
}
```

---

### File 2: WSL Global AGENTS.md

**Path:** `/home/dev-p/.config/opencode/AGENTS.md`

Global behavior rules that apply to all sessions:
- Plan agent = planning, analysis, architecture reasoning
- Build agent = implementation, refactors, tests (with explicit approval)
- Safety rules (no destructive ops without confirmation)
- Communication rules (concise, structured, no emojis)

---

### File 4: project-library opencode.json

**Path:** `/mnt/c/mybizz/project-library/opencode.json`

```json
{
  "default_agent": "plan",
  "permission": {
    "edit": { "**/anvil.yaml": "deny", "*": "ask" },
    "external_directory": { "C:\\mybizz\\mb-3-cs\\**": "allow" }
  }
}
```

---

### File 8: mb-3-cs opencode.json

**Path:** `/mnt/c/mybizz/mb-3-cs/opencode.json`

```json
{
  "default_agent": "build",
  "permission": {
    "edit": {
      "*": "ask",
      "anvil.yaml": "deny",
      "parameters.yaml": "deny",
      "templates.yaml": "deny",
      "**/form_template.yaml": "deny",
      "standard-page.html": "ask",
      "theme.css": "ask"
    },
    "external_directory": { "C:\\mybizz\\project-library\\**": "allow" }
  }
}
```

---

### Config Precedence

When OpenCode loads config, the order is:
1. **Per-project config** (`.opencode/opencode.json` in project root)
2. **Global config** (`~/.config/opencode/opencode.jsonc`)

The **strictest rule wins** when there's a conflict.

**Model selection:** The UI selector overrides all config file model settings. Use the UI to switch models.

---

## Section 3: Slash Commands

### Built-in Commands

**Apply in:** OpenCode web UI (type in chat)

| Command | What it does |
|---|---|
| `/init` | Analyze project and create `AGENTS.md` |
| `/undo` | Undo the agent's last change |
| `/redo` | Redo an undone change |
| `/share` | Share conversation via link |
| `/help` | Show help |

### GStack Skill Commands

**Apply in:** OpenCode web UI (type in chat)

For the full skill catalog (59 skills with usage, prerequisites, output, presentation), see `gstack-skills-reference.md`.

Key commands:

| Command | What it does |
|---|---|
| `/office-hours` | YC partner grilling — design doc |
| `/autoplan` | Full planning pipeline in one command |
| `/build` | Implement the plan |
| `/review` | Paranoid staff engineer audit |
| `/qa` | Browser-based testing |
| `/ship` | Create GitHub PR |
| `/design-consultation` | Build design system |
| `/investigate` | Root-cause debugger |
| `/health` | Quick health check |
| `/sync-gbrain` | Sync GBrain state to git repo |

### GBrain Commands

**Apply in:** OpenCode web UI (type in chat)

| Command | What it does |
|---|---|
| `/setup-gbrain` | Install and configure GBrain |
| `/setup-gbrain doctor` | GBrain health check |
| `/sync-gbrain` | Sync GBrain state to git repo |

### Context Commands

**Apply in:** OpenCode web UI (type in chat)

| Command | What it does |
|---|---|
| `/context-save` | Save current working context |
| `/context-restore` | Restore previous context |
| `/gstack-learn` | Store useful information |
| `/learn` | Search stored learnings |

---

## Section 4: Keyboard Shortcuts

### Session Management

| Shortcut | Action |
|---|---|
| **Ctrl+X, N** | New session |
| **Ctrl+X, L** | Session list |
| **Ctrl+X, R** | Rename session |
| **Ctrl+X, D** | Delete session |

### Session Navigation

| Shortcut | Action |
|---|---|
| **Up Arrow** | Parent session |
| **Down Arrow** | First child session |
| **Left Arrow** | Previous sibling session |
| **Right Arrow** | Next sibling session |

### Prompt Editing

| Shortcut | Action |
|---|---|
| **Ctrl+A** | Move to line start |
| **Ctrl+E** | Move to line end |
| **Ctrl+B** | Back one character |
| **Ctrl+F** | Forward one character |
| **Alt+B** | Back one word |
| **Alt+F** | Forward one word |
| **Ctrl+W** | Delete previous word |
| **Alt+D** | Delete next word |
| **Ctrl+K** | Kill to end of line |
| **Ctrl+U** | Kill to start of line |

### Prompt Submission

| Shortcut | Action |
|---|---|
| **Enter** | Submit prompt |
| **Shift+Enter** | New line (without sending) |
| **Esc** | Cancel current action |
| **Ctrl+G** | Interrupt/stop response |

### Command Access

| Shortcut | Action |
|---|---|
| **Ctrl+P** | Command palette |
| **Tab** | Switch Plan/Build mode |
| **Ctrl+X, A** | Agent list |
| **Ctrl+X, M** | Model list |
| **Ctrl+X, B** | Toggle sidebar |

---

## Section 5: How OpenCode Works (Reference)

### Workspace Structure

```
~/.config/opencode/              # WSL global config
├── opencode.jsonc               # Provider, agents, MCP, permissions
└── AGENTS.md                    # Global behavior rules

project-library/                 # Planning repo
├── agents.md                    # Project rules
├── opencode.json                # Project config (default: plan)
├── .opencode/
│   ├── agents/
│   │   ├── build.md             # Build agent definition
│   │   └── plan.md              # Plan agent definition
│   └── commands/                # 52 slash commands
├── adr/                         # Architecture Decision Records
├── docs/                        # Documentation
├── gstack-outputs/              # GStack session outputs
└── wireframes/                  # HTML wireframes

mb-3-cs/                         # Code repo
├── agents.md                    # Project rules
├── opencode.json                # Project config (default: build)
├── .opencode/
│   ├── agents/
│   │   ├── build.md             # Build agent definition
│   │   └── plan.md              # Plan agent definition
│   └── commands/                # Inherited slash commands
├── client_code/                 # Python client code
├── server_code/                 # Python server code
└── anvil.yaml                   # Anvil app config (protected)
```

### Agent Modes

| Mode | What the agent can do | Default in |
|---|---|---|
| **Plan** | Read files, suggest changes, no edits | project-library |
| **Build** | Read and edit files, run commands | mb-3-cs |

Switch with **Tab** key.

### MCP Integration

GBrain is registered as an MCP server in the WSL config. When the agent needs to search memories, it calls GBrain tools automatically:

- `gbrain_search` — semantic search
- `gbrain_query` — vector similarity search
- `gbrain_code_def` — find code symbol definitions
- `gbrain_code_refs` — find code symbol references

---

## Section 6: File Locations

| Item | WSL Path | Windows Path |
|---|---|---|
| Global config | `/home/dev-p/.config/opencode/opencode.jsonc` | `\\wsl.localhost\Ubuntu\home\dev-p\.config\opencode\` |
| Global AGENTS.md | `/home/dev-p/.config/opencode/AGENTS.md` | `\\wsl.localhost\Ubuntu\home\dev-p\.config\opencode\` |
| Global skills | `/home/dev-p/.config/opencode/skills/` | `\\wsl.localhost\Ubuntu\home\dev-p\.config\opencode\skills\` |
| Skills source | `/mnt/c/mybizz/skills/` | `C:\mybizz\skills\` |
| project-library config | `/mnt/c/mybizz/project-library/opencode.json` | `C:\mybizz\project-library\opencode.json` |
| project-library agents | `/mnt/c/mybizz/project-library/.opencode/agents/` | `C:\mybizz\project-library\.opencode\agents\` |
| project-library commands | `/mnt/c/mybizz/project-library/.opencode/commands/` | `C:\mybizz\project-library\.opencode\commands\` |
| mb-3-cs config | `/mnt/c/mybizz/mb-3-cs/opencode.json` | `C:\mybizz\mb-3-cs\opencode.json` |
| mb-3-cs agents | `/mnt/c/mybizz/mb-3-cs/.opencode/agents/` | `C:\mybizz\mb-3-cs\.opencode\agents\` |

---

## Section 7: Troubleshooting

### Agent won't edit a file

Check if the file is protected in the per-project `opencode.json`:
- `anvil.yaml` → always `deny`
- `form_template.yaml` → always `deny`
- `parameters.yaml` → always `deny`
- `templates.yaml` → always `deny`

### Agent is in the wrong mode

Press **Tab** to switch between Plan and Build mode.

### MCP tools not available

Restart OpenCode to reload MCP config:

**Apply in:** WSL terminal

```bash
pkill -f "opencode web"
opencode web --hostname 0.0.0.0
```

### Commands not showing up

Check that command files exist in `.opencode/commands/` in your project directory.

### Wrong model being used

OpenCode reads the model from the **UI selector**, not from config files. If the wrong model is active:
1. Check the model selector in the OpenCode UI
2. Switch to the correct model using the UI
3. Config file `model` fields are fallback/initial values only

---

## Related Documentation

| Document | Purpose |
|---|---|
| `gbrain-reference.md` | GBrain commands, health score, troubleshooting |
| `gstack-reference.md` | GStack skills and workflows |
| `gstack-outputs/config-audit-final.md` | Full config audit report |
| `https://opencode.ai/docs/` | Official OpenCode documentation |
