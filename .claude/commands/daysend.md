# Day's End

You are running the **day's end routine** — a single command that wraps up the work session and captures everything.

## Process

### Step 1: Run /retro

Invoke the `retro` skill. Follow its full process:
- Review the session
- Ask the user for acceptance status (wait for response)
- Write the retro file
- Update the retro log
- Update any source docs identified

### Step 2: Persist memories

Review the conversation for anything worth persisting across sessions. Save memories as markdown files in the project directory at `.claude/memories/`:

1. **Check for existing memories** — read `.claude/memories/MEMORY.md` if it exists to avoid duplicates
2. **Write memory files** — save each memory to its own file (e.g., `user_role.md`, `feedback_testing.md`) with this frontmatter:

```markdown
---
name: {{memory name}}
description: {{one-line description}}
type: {{user | feedback | project | reference}}
---

{{memory content}}
```

3. **Update the index** — add/update entries in `.claude/memories/MEMORY.md` (create if it doesn't exist). This is an index of pointers, not memory content.

**Memory types:**
- **User** — role, preferences, knowledge, working style
- **Feedback** — corrections or guidance given during the session (include why)
- **Project** — decisions, milestones, context changes (convert relative dates to absolute)
- **Reference** — pointers to external resources, tools, dashboards

**What NOT to save:** code patterns derivable from the codebase, git history, debugging solutions already in code, ephemeral task details.

Check existing memories first. Update rather than duplicate.

### Step 3: Commit everything

Stage all uncommitted changes from the session:

```bash
git status
```

Review what's being committed. Stage relevant files (not secrets, not .env, not node_modules). Commit with a summary message:

```
daysend: <date> — <1-line session summary>
```

### Step 4: Check for unpushed commits

```bash
git log @{u}..HEAD --oneline 2>/dev/null
```

If there are unpushed commits, tell the user how many and ask if they want to push. Do NOT push automatically.

If there are no unpushed commits (or no upstream is set), skip this step.

### Step 5: Confirm

Report to the user:
- Retro status (accepted/needs-revision/rejected)
- Memories saved/updated count
- Commit hash
- Unpushed commits (count, or "none")

## Rules

- Do NOT skip the user input step in /retro — it requires one response
- Do NOT force push — if push fails, ask the user
- Do NOT commit secrets, .env files, or credentials
- DO check for uncommitted work before committing — don't create empty commits
- Keep it moving — this is end of day, the user wants to wrap up and leave
