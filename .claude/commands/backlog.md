---
name: backlog
description: Use when managing, viewing, adding, removing, or prioritizing backlog items in ~/dev/BACKLOG.md. Triggered by /backlog or when user mentions backlog, todo items, or task tracking.
---

# Backlog

Manage the backlog of todo items stored in `~/dev/BACKLOG.md`.

## Usage

`/backlog` — Show all current backlog items
`/backlog add <item>` — Add a new item
`/backlog done <number>` — Mark item as complete (moves to Done section)
`/backlog remove <number>` — Remove an item
`/backlog prioritize` — Interactively reorder items

## Behavior

1. **Read** `~/dev/BACKLOG.md` to get current state
2. **Apply** the requested operation (show, add, done, remove, prioritize)
3. **Write** updated file back if changed
4. **Display** the current backlog to the user after any operation

## File Format

```markdown
# Backlog

## Todo
- [ ] 1. Item description
- [ ] 2. Item description

## Done
- [x] Item description (completed YYYY-MM-DD)
```

## Rules

- Items in Todo are numbered sequentially starting from 1
- Renumber after any removal or completion
- When marking done, append completion date
- When showing the backlog, display Todo items with their numbers
- If no args provided, just display the current backlog
- If BACKLOG.md doesn't exist, create it with the standard format
