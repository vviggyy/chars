---
name: respawn
description: Session continuity skill. Reconstructs context from the last session and orients toward what's next. Use when starting a new conversation on an existing project.
---

## Respawn — Session Continuity

You are respawning into a new session. Before doing anything else, reconstruct context from the last session and orient toward what's next.

### Step 1: Git Archaeology

Run these commands to understand recent work:

```bash
# Last 15 commits with details
git log --oneline --decorate -15

# What changed in the last session (last 24-48 hours of commits)
git log --since="2 days ago" --stat

# Any uncommitted work in progress
git status

# Diff of any staged/unstaged changes (the user may have been mid-task)
git diff --stat
git diff --cached --stat
```

Summarize: what was built, fixed, or refactored recently? What's the trajectory of the work?

### Step 2: Check Memory

Read the project memory index at the memory directory for this project (check `.claude/projects/` for the matching project path). Look for:
- Recent project memories that capture context about ongoing work
- Any feedback memories about how the user wants to work
- Reference memories pointing to relevant external resources

### Step 3: Check for In-Progress Work

Look for signs of interrupted work:
- Uncommitted changes (from git status/diff)
- TODO comments added recently: `git log --since="2 days ago" -p | grep -i "TODO\|FIXME\|HACK\|XXX"`
- Any open branches besides main: `git branch`
- Stashed work: `git stash list`

### Step 4: Synthesize and Present

Give the user a concise briefing:

**Last Session Recap**
- What was accomplished (from commits and memory)
- What was in progress when the session ended (from uncommitted changes, branches, stashes)

**Current State**
- Branch, clean/dirty status
- Any build issues or broken state to be aware of

**Suggested Directions**
Based on the trajectory of recent work, propose 2-4 concrete next steps. Prioritize:
1. Finishing anything left mid-task
2. Natural next features/fixes given the momentum
3. Any issues or TODOs that were flagged but not addressed
4. Stretch goals that align with the project's direction

Keep the whole briefing tight — no more than ~20 lines. The user wants to get oriented and get moving, not read an essay.
