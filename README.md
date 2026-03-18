# skills

A set of reusable [Claude Code](https://docs.anthropic.com/en/docs/claude-code) slash commands for managing work sessions — planning what's next, running retrospectives, and wrapping up the day.

These commands are project-agnostic. Install them in any repo and they work with that project's structure.

## What's Included

### `/next` — What's Next

Reviews your project backlog and presents prioritized next steps for the session.

- Reads `BACKLOG.md` and parses all sections
- Runs a quick project health check (tests, uncommitted work, recent commits)
- Presents blocking items, a recommended next action, and quick wins
- Waits for your decision before doing anything

### `/retro` — Task Retrospective

A structured review of your work session. Captures what was built, what went wrong, what was learned, and feeds those learnings back into your project docs.

- Asks for your acceptance rating
- Writes a retro file to `docs/retros/`
- Maintains a running retro log for trend analysis
- Identifies source docs that need updating based on what was learned

### `/daysend` — Day's End

A single command that runs the full end-of-day routine: retro, memory persistence, and commit. Designed to wrap up your session in one shot.

- Runs `/retro`
- Persists any new learnings to Claude's memory system
- Commits all session work
- Checks for unpushed commits and asks before pushing

## Install

Clone this repo and copy the commands into your project's `.claude/commands/` directory:

```bash
# Copy all commands
cp -r skills/.claude/commands/ your-project/.claude/commands/

# Or symlink for automatic updates
ln -s /path/to/skills/.claude/commands/next.md your-project/.claude/commands/next.md
ln -s /path/to/skills/.claude/commands/retro.md your-project/.claude/commands/retro.md
ln -s /path/to/skills/.claude/commands/daysend.md your-project/.claude/commands/daysend.md
```

Or install globally so they're available in every project:

```bash
cp skills/.claude/commands/*.md ~/.claude/commands/
```

## Customization

These commands are meant to be forked and adapted. A few things you might want to change:

- **Output paths** — retros write to `docs/retros/`. Change these to match your project structure.
- **Commit message format** — each command uses a conventional prefix (`retro:`, `daysend:`). Adjust to match your project's conventions.

## License

MIT
