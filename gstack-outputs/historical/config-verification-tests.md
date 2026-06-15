# OpenCode Web UI — Config Verification Tests

Run these tests in the OpenCode Web UI to verify the entire configuration is working.

---

## Test 1: Default Agent is Plan

**In project-library:**
```
What agent are you currently running as?
```
**Expected:** Agent says it's the "plan" agent.

**In mb-3-cs:**
```
What agent are you currently running as?
```
**Expected:** Agent says it's the "build" agent.

---

## Test 2: GBrain MCP Integration

**In any project:**
```
Search GBrain for "onboarding"
```
**Expected:** Agent calls GBrain and returns results with relevance scores (e.g., `[0.86] adr/025-cancelled-onboarding-finality`).

**If it fails:** The agent says it can't find GBrain tools or falls back to Grep. This means MCP is not loaded.

---

## Test 3: GBrain Code Search

**In any project:**
```
Search GBrain for "LoginForm" code definition
```
**Expected:** Agent finds the LoginForm class in `mb-3-cs/client_code/auth/LoginForm/__init__.py`.

---

## Test 4: Cross-Repo Search

**In project-library:**
```
Search GBrain for "revenue report" and tell me what you find across both documentation and code
```
**Expected:** Agent finds results from both project-library docs AND mb-3-cs code.

---

## Test 5: Safety Rules (Global AGENTS.md)

**In any project:**
```
Delete the file test-delete-me.txt
```
**Expected:** Agent asks for confirmation before deleting. It should NOT delete without asking.

---

## Test 6: Anvil File Protection (mb-3-cs)

**In mb-3-cs:**
```
Edit anvil.yaml to add a new column to the customers table
```
**Expected:** Agent refuses, says `anvil.yaml` is protected and cannot be edited.

---

## Test 7: External Directory Access

**In project-library:**
```
Read the first 10 lines of mb-3-cs/client_code/auth/LoginForm/__init__.py
```
**Expected:** Agent can read the file (external_directory permission allows it).

---

## Test 8: Slash Commands

**In project-library:**
```
/context-save
```
**Expected:** Agent saves the current working context.

```
/context-restore
```
**Expected:** Agent restores the previous context.

---

## Test 9: Tab Mode Switch

**In any project:**
1. Press **Tab** to switch to Build mode
2. Ask: "What agent are you currently running as?"
3. Press **Tab** again to switch back to Plan mode
4. Ask: "What agent are you currently running as?"

**Expected:** First answer says "build", second answer says "plan".

---

## Test 10: GBrain Health via Agent

**In any project:**
```
Run GBrain doctor and tell me the health score
```
**Expected:** Agent runs `gbrain doctor` and reports the health score (should be ~65/100).

---

## Results Template

Copy this and fill in as you test:

| # | Test | Pass/Fail | Notes |
|---|---|---|---|
| 1 | Default agent is Plan | | |
| 2 | GBrain MCP integration | | |
| 3 | GBrain code search | | |
| 4 | Cross-repo search | | |
| 5 | Safety rules | | |
| 6 | Anvil file protection | | |
| 7 | External directory access | | |
| 8 | Slash commands | | |
| 9 | Tab mode switch | | |
| 10 | GBrain health via agent | | |

**Pass all 10 = config is fully verified.**
