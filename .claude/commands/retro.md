# Task Retrospective

You are running a **task retrospective** — a structured review of the current work session. The goal is to capture what was learned so future sessions are faster and more reliable.

## Process

### Step 1: Review the session

Look at what happened in this conversation:
- What was the task?
- What tools, services, or APIs were involved?
- How many iterations did it take to get to acceptance?
- Were there any surprises, failures, or workarounds?
- What patterns or workflows were discovered?

**If conversation context is thin** (e.g., invoked in a new session), reconstruct from: `git log --oneline -20`, `git diff HEAD~5..HEAD --stat`, and the most recent session JSONL in `~/.claude/projects/<project-slug>/`.

### Step 2: Gather input

Ask the user ONE question:
> "How would you rate this work — accepted, needs revision, or rejected? Anything else you want to capture?"

Wait for their response before proceeding.

### Step 3: Check for source doc updates

Determine if any existing docs need updating based on what was learned:

1. **Integration gotchas** — if work touched third-party services, check for integration docs that need updates
2. **Operational learnings** — if something unexpected happened, check for learning docs to update
3. **Agent/automation protocols** — if a new protocol or behavior was discovered
4. **Memory files** — if a product idea or workflow pattern emerged

### Step 4: Write the retro

Write to `docs/retros/YYYY-MM-DD-<topic>.md` (create the directory if it doesn't exist):

```markdown
# Retro: <topic>
Date: YYYY-MM-DD
Status: accepted | needs-revision | rejected

## Summary
<1-3 sentences: what was the task and what was delivered>

## Metrics
- **Iterations:** <number of revision rounds before acceptance>
- **Key tools:** <tools, services, APIs used>
- **Docs updated:** <list of docs created or modified>

## Learnings
- <what surprised us or went wrong>
- <what we'd do differently next time>
- <what worked well>

## Patterns Discovered
<new workflows, tools, or approaches worth remembering — or "None">

## Source Doc Updates
<list of docs updated as part of this retro, with what changed — or "None needed">

## Process Improvements
- <what broke in the planning/review workflow>
- <what rule or check would have prevented it>
- <skills/commands updated as a result>
- <or "Process worked as designed — no changes needed">
```

### Step 5: Update the retro log

Read `docs/retros/retro-log.md` (create if it doesn't exist). Append a one-line summary:

```
| YYYY-MM-DD | <topic> | <status> | <iterations> | <1-line summary> |
```

The retro log is a running index for trend analysis — keep entries concise.

### Step 6: Update source docs

If Step 3 identified docs that need updating, update them now.

### Step 7: Process Self-Analysis

Evaluate the planning and review process itself — not just the work output. This step improves how we work in future sessions.

**Planning quality check:**
- Did plans go through a full critique cycle before implementation? If not, why?
- Were library APIs verified before writing code, or assumed?
- Were previous decisions/memory cross-checked before planning?

**Review quality check:**
- Were duplicates caught before creating new backlog items?
- Did critiques catch real issues or just surface-level formatting?
- Were any issues only caught by the user that the process should have caught?

**Process improvements:**
- What broke in the workflow that should be automated or enforced?
- What new rule or checklist item would prevent the mistakes made this session?
- Should any skill or command be updated?

Write findings in the `## Process Improvements` section of the retro. If improvements are identified:
- Update the relevant skill/command file immediately
- Add to CLAUDE.md if it's a universal rule
- Save to memory if it's a workflow preference

### Step 8: Commit

Stage and commit all retro files and source doc updates with message:
```
retro: <topic> — <1-line summary>
```

## Rules

- Do NOT skip the user input step — always ask for acceptance status
- Do NOT fabricate metrics — if you can't determine iteration count, say "unknown"
- Do NOT write empty learnings — if nothing was learned, say "Routine task, no new learnings"
- DO update source docs — the retro is only valuable if it feeds back into the system
- Keep retros concise — 1 page max. The value is in the source doc updates, not the retro itself.
