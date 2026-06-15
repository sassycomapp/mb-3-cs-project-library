# Getting To Work
Here’s an updated version of **“Getting To Work”** that matches your actual setup: WSL server, Desktop client, GStack skills, and GBrain on Supabase/OpenRouter. It replaces the old “launch OpenCode directly from the repo folder” flow.

***

# Getting To Work

## Apps list

You only need these for normal work:

- Ubuntu 22.04.5 LTS (WSL)  
- Bash shell in Ubuntu  
- OpenCode Desktop (on Windows)  
- GStack (as skills used by the OpenCode agents)  
- GBrain (Supabase engine + OpenRouter embeddings, used through GStack)  
- Git (from Bash in Ubuntu)  

Everything else (Windows PowerShell, Azure Cloud Shell, full standalone PostgreSQL server) is optional and can be ignored for now.

***

## Open the right Ubuntu

1. Open **Windows Terminal**.  
2. In the dropdown (the little ▾ arrow), click **“Ubuntu 22.04.5 LTS”**.  
   - That profile is your WSL Ubuntu instance.  
3. You should get a prompt like:

   ```bash
   myubuntu@dev-p:~$
   ```

   This shell is **home base** for:

   - running the OpenCode server,  
   - running Git,  
   - inspecting the repos under `/mnt/c/_mb2-cs-app/...`.  

You will *not* normally launch the UI from here; you run the **server** here and let the Desktop app connect to it.

***

## Terminology (Windows side)

1. `C:\_mb2-cs-app` — **Workspace folder**  
   - Not a Git repo.  
   - Container for everything (repos, GStack, workspace docs).

2. `C:\_mb2-cs-app\mb-3-cs` — **Git code repo**  
   - Anvil‑linked production‑adjacent application code.  

3. `C:\_mb2-cs-app\project-library` — **Git docs repo**  
   - Design/planning/architecture documents.  
   - Not Anvil‑linked.

4. `C:\_mb2-cs-app\gstack` — **GStack repo**  
   - Workflow/skillpack used by the agents inside OpenCode.

***

## How this maps into Ubuntu/WSL

Inside Ubuntu 22.04.5 (WSL), these appear under `/mnt/c`:

- `C:\_mb2-cs-app\mb-3-cs` →  
  `/mnt/c/_mb2-cs-app/mb-3-cs`  

- `C:\_mb2-cs-app\project-library` →  
  `/mnt/c/_mb2-cs-app/project-library`  

- `C:\_mb2-cs-app\gstack` →  
  `/mnt/c/_mb2-cs-app/gstack`  

You **do not** switch projects by restarting the server; you switch projects from within OpenCode Desktop by sending the appropriate `/mnt/...` path in chat after the server is running.

***

## Launching OpenCode (current, correct workflow)

The correct flow now is:

1. Start the OpenCode **server** in Ubuntu (WSL).  
2. Connect OpenCode **Desktop** to that server.  
3. From Desktop, open the **project** by sending the WSL path in chat.  

### Step 1 – Start the OpenCode server in Ubuntu

1. In Windows Terminal, choose **Ubuntu 22.04.5 LTS** and get to:

   ```bash
   myubuntu@dev-p:~$
   ```

#### NEW METHOD: 
cd /mnt/c/_mb2-cs-app/gstack
opencode web --hostname 0.0.0.0

dev-p@dev-p:/mnt/c/_mb2-cs-app/gstack$ cd /mnt/c/_mb2-cs-app/gstack
dev-p@dev-p:/mnt/c/_mb2-cs-app/gstack$ opencode web

This starts the server and opens the WEB UI
This ensures the project root is gstack, so it will see gstack/.opencode/skills and the new gstack/.opencode/commands
```
#### OLD METHOD
3. 
     ```
    From your home directory (or anywhere), start the server:

   ```bash
   opencode serve --hostname 0.0.0.0 --port 4096
   
   Leave this terminal window **open and running** while you work.  
   - You should see output like:

     ```text
     Warning: OPENCODE_SERVER_PASSWORD is not set; server is unsecured.
     opencode server listening on http://0.0.0.0:4096```
	 
4. You do **not** run `opencode` again in this terminal for normal work.  
   - This process is the **backend** server that the Desktop app will talk to.

### Step 2 – Start OpenCode Desktop and connect to the server 
[OBSOlete - DESktop app is no longer being used]

1. On Windows, open **OpenCode Desktop**.  
2. Go to the **Servers** list and ensure there is an entry:

   - Name: `wsl-opencode`  
   - Address: `http://localhost:4096`

3. Make sure Desktop is **connected** to `wsl-opencode`.  
   - Once connected, Desktop will send all file operations and tools to the WSL server you started in Step 1.

### Step 3 – Open the desired project from Desktop

Because the “Open project” buttons are unreliable on this install, you use the **WSL path in chat** to open the project.

#### To work in `project-library` (planning & docs)

1. In an OpenCode Desktop chat (while connected to `wsl-opencode`), send:

   ```text
   /mnt/c/_mb2-cs-app/project-library
   ```

2. Wait for Desktop to switch the workspace to `C:\_mb2-cs-app\project-library`.  
   - You can confirm inside the OpenCode terminal with:

     ```bash
     pwd
     ```

     and you should see:

     ```bash
     /mnt/c/_mb2-cs-app/project-library
     ```

3. In this workspace:
   - The **default agent** is `plan` (Qwen 3.5 Flash).  
   - Use this for:
     - document analysis,  
     - MM/AM/ADR validation,  
     - architecture / refactor planning,  
     - prompt and workflow docs.  

4. The `plan` / `build` agents here can:
   - call **GStack** skills (e.g. `/office-hours`, `/plan-ceo-review`, `/plan-eng-review`) to structure your thinking,  
   - and, via those skills, use **GBrain** running on Supabase/OpenRouter for advisory memory (subject to the source-tier and provenance rules).

#### To work in `mb-3-cs` (live app code)

1. In an OpenCode Desktop chat (still connected to `wsl-opencode`), send:

   ```text
   /mnt/c/_mb2-cs-app/mb-3-cs
   ```

2. Wait for Desktop to switch the workspace to `C:\_mb2-cs-app\mb-3-cs`.  
   - Again, confirm via `pwd` in the OpenCode terminal:

     ```bash
     /mnt/c/_mb2-cs-app/mb-3-cs
     ```

3. In this workspace:
   - The **default agent** is `build`.  
   - Use this for:
     - explicit, user-directed code changes,  
     - implementing plans handed off from `project-library`,  
     - running tests and making tightly scoped edits.  

4. Switch to `plan` when you need higher-level reasoning:
   - structural or architectural decisions,  
   - refactor design,  
   - impact analysis across modules or protected areas.  

5. The agents here can:
   - call **GStack** skills for plan/build/investigate/review/ship,  
   - and those skills can read/write to **GBrain** (Supabase + OpenRouter) as advisory memory, while **always** obeying:
     - AGENTS rules,  
     - `opencode.json` permissions,  
     - and the protected-file rules for Anvil-related files and templates.

***

