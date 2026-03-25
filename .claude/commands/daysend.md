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

### Step 3: Session self-analysis

Before reflecting on CLAUDE.md updates, review the entire session for process quality. This is how the process improves over time.

**Analyze the session for:**

1. **Process failures** — where were reviews skipped, deploys unverified, docs not read? Did you skip a skill, ignore a checklist step, or rush past verification?
2. **Debugging waste** — where did trial-and-error replace reading docs or logs? Count the cycles wasted on guessing vs. what reading the error message or docs would have told you immediately.
3. **New anti-patterns** — repeated mistakes that should become rules. If the same mistake happened twice, it needs a rule to prevent a third time.
4. **What worked well** — approaches worth reinforcing. What saved time, caught bugs early, or produced clean results on the first try?

**Process:**

1. Walk through the session chronologically, noting each category above
2. Present findings to the user as a brief summary:

> **Session self-analysis:**
>
> - **Process failures:** [list, or "none"]
> - **Debugging waste:** [list, or "none"]
> - **New anti-patterns:** [list, or "none"]
> - **What worked well:** [list]

3. Any new anti-patterns or process failures identified here should be carried forward as candidate CLAUDE.md rules in the next step

### Step 4: Daily CLAUDE.md reflection

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

### Step 5: Commit everything

Stage all uncommitted changes from the session:

```bash
git status
```

Review what's being committed. Stage relevant files (not secrets, not .env, not node_modules). Commit with a summary message:

```
daysend: <date> — <1-line session summary>
```

### Step 6: Check for unpushed commits

```bash
git log @{u}..HEAD --oneline 2>/dev/null
```

If there are unpushed commits, tell the user how many and ask if they want to push. Do NOT push automatically.

If there are no unpushed commits (or no upstream is set), skip this step.

### Step 7: Confirm

Report to the user:
- Retro status (accepted/needs-revision/rejected)
- Memories saved/updated count
- Session self-analysis (issues found count, or "clean session")
- CLAUDE.md updates (count added, or "none")
- Commit hash
- Unpushed commits (count, or "none")

## Rules

- Do NOT skip the user input step in /retro — it requires one response
- Do NOT force push — if push fails, ask the user
- Do NOT commit secrets, .env files, or credentials
- DO check for uncommitted work before committing — don't create empty commits
- Keep it moving — this is end of day, the user wants to wrap up and leave
