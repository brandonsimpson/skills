# What's Next

Review the backlog and present the prioritized next steps for this session.

## Process

### Step 1: Read the backlog

Read `BACKLOG.json` at the project root. Parse all sections and their items.

If `BACKLOG.json` doesn't exist, say so and suggest creating one.

### Step 2: Check project state

Quick health check:
```bash
git status --short
git log --oneline -5
```

If the project has a test command, detect and run it:
- `package.json` scripts → `npm test`
- `Makefile` with test target → `make test`
- `pytest.ini` / `pyproject.toml` / `setup.cfg` → `pytest`
- `Cargo.toml` → `cargo test`
- `go.mod` → `go test ./...`

Run the detected command and capture the last few lines of output.

Report: tests passing, any uncommitted work, last few commits.

### Step 3: Present priorities

Show the user:

**Blocking items** (cannot launch without these):
- List each with a one-line description
- Flag any that have open research dependencies

**Next recommended action:**
- Based on what's blocking and what's ready to build, recommend what to do this session
- If research is needed first, say so
- If a product decision is needed, flag it
- If everything is ready to build, recommend the next build step

**Quick wins** (items that could be knocked out in <30 minutes):
- Pull from Important or Blocking sections
- Things like TODO fixes, small refactors, config changes

**Format:**

```
## Project Health
Tests: X passing | Build: clean/broken | Uncommitted: yes/no

## Blocking (X items)
1. Item — status/notes
2. Item — status/notes
...

## Recommended Next Action
[What to do and why]

## Quick Wins
- Item (est. 10 min)
- Item (est. 15 min)
```

### Step 4: Ask

> "What do you want to tackle?"

Wait for the user's response before doing anything.

## Rules

- Do NOT start working on anything — just present the options
- Do NOT skip the health check — always verify the project state
- Keep it concise — the user wants a quick briefing, not a novel
- If BACKLOG.json doesn't exist, say so and suggest creating one
