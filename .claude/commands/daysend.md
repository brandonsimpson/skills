# Day's End

You are running the **day's end routine** — a single command that wraps up the work session, captures everything, and pushes to remote.

## Process

### Step 1: Run /retro

Invoke the `retro` skill. Follow its full process:
- Review the session
- Ask the user for acceptance status (wait for response)
- Write the retro file
- Update the retro log
- Update any source docs identified

### Step 2: Persist memories

Review the conversation for anything that should be saved to the Claude memory system:

- **User memories:** Any new details about the user's role, preferences, or knowledge
- **Feedback memories:** Any corrections or guidance given during the session
- **Project memories:** Any decisions, milestones, or context changes
- **Reference memories:** Any external resources or tools discovered

Check existing memories first to avoid duplicates. Update existing memories if they've changed.

### Step 3: Commit everything

Stage all uncommitted changes from the session:

```bash
git status
```

Review what's being committed. Stage relevant files (not secrets, not .env, not node_modules). Commit with a summary message:

```
daysend: <date> — <1-line session summary>
```

### Step 4: Push to remote

```bash
git push
```

If push fails (behind remote, etc.), report the issue to the user rather than force-pushing.

### Step 5: Confirm

Report to the user:
- Retro status (accepted/needs-revision/rejected)
- Memories saved/updated count
- Commit hash
- Push status (success/failed)

## Rules

- Do NOT skip the user input step in /retro — it requires one response
- Do NOT force push — if push fails, ask the user
- Do NOT commit secrets, .env files, or credentials
- DO check for uncommitted work before committing — don't create empty commits
- Keep it moving — this is end of day, the user wants to wrap up and leave
