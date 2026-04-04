# Day's End

You are running the **day's end routine** — a single command that wraps up the work session and captures everything.

## Process

### Step 1: Run /retro

Invoke the `retro` skill. Follow its full process — it handles session review, user input, retro writing, source doc updates, and process self-analysis.

### Step 2: Persist memories

Review the conversation for anything worth persisting across sessions. Follow the auto-memory system instructions to save new memories and update existing ones. Check for duplicates before writing.

### Step 3: CLAUDE.md reflection

After retro and memories are complete, reflect on the entire day's work to identify improvements for the project's CLAUDE.md file. This is the daily feedback loop — each session should make future sessions better, faster, smarter, and more secure.

**What to look for:**

- **Workflow rules** — did we discover a process that should always be followed? (e.g., "always run X before Y", "never deploy without Z")
- **Codebase conventions** — did we learn a pattern or convention that isn't documented? (e.g., naming conventions, file organization, import ordering)
- **Tool/infra gotchas** — did we hit a pitfall that future sessions should avoid? (e.g., "this API silently drops fields", "tests must run with FLAG=true")
- **Security practices** — did we learn something about auth, secrets, permissions, or vulnerability patterns?
- **Performance insights** — did we discover something about what makes work faster or slower?
- **Decision principles** — did we make a judgment call that should become a standing rule?

**Process:**

1. Review the session's retro, any corrections the user made, surprises encountered, and patterns discovered
2. Read the current CLAUDE.md file (project root and/or `~/.claude/CLAUDE.md`) to check what's already documented
3. Draft candidate additions — each should be a concise, actionable rule or guideline
4. **Present the candidates to the user for approval** — do NOT write to CLAUDE.md without explicit approval. Format as:

> **Daily reflection — proposed CLAUDE.md additions:**
>
> 1. `[section]` — proposed rule or guideline
> 2. `[section]` — proposed rule or guideline
> ...
>
> Approve all, approve specific numbers, or skip?

5. Only persist approved items to the appropriate CLAUDE.md file
6. If nothing meaningful was discovered, say "No CLAUDE.md updates today" and move on — don't force it

**Where to write:**
- Project-specific rules → `./CLAUDE.md` (project root)
- Cross-project rules (personal workflow, general preferences) → `~/.claude/CLAUDE.md`

### Step 4: Commit everything

Stage all uncommitted changes from the session:

```bash
git status
```

Review what's being committed. Stage relevant files (not secrets, not .env, not node_modules). Commit with a summary message:

```
daysend: <date> — <1-line session summary>
```

### Step 5: Check for unpushed commits

```bash
git log @{u}..HEAD --oneline 2>/dev/null
```

If there are unpushed commits, tell the user how many and ask if they want to push. Do NOT push automatically.

If there are no unpushed commits (or no upstream is set), skip this step.

### Step 6: Confirm

Report to the user:
- Retro status (accepted/needs-revision/rejected)
- Memories saved/updated count
- CLAUDE.md updates (count added, or "none")
- Commit hash
- Unpushed commits (count, or "none")

## Rules

- Do NOT skip the user input step in /retro — it requires one response
- Do NOT force push — if push fails, ask the user
- Do NOT commit secrets, .env files, or credentials
- DO check for uncommitted work before committing — don't create empty commits
- Keep it moving — this is end of day, the user wants to wrap up and leave
