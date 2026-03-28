---
name: backlog
description: Use when managing, viewing, adding, removing, or prioritizing backlog items in BACKLOG.json at the project root. Triggered by /backlog or when user mentions backlog, todo items, or task tracking.
---

# Backlog

Manage the backlog of todo items stored in `BACKLOG.json` at the project root.

## Usage

`/backlog` — Show all current backlog items
`/backlog add <item>` — Add a new item
`/backlog done <number>` — Mark item as complete (moves to done array)
`/backlog remove <number>` — Remove an item
`/backlog prioritize` — Interactively reorder items

## Behavior

1. **Read** `BACKLOG.json` at the project root to get current state
2. **Apply** the requested operation (show, add, done, remove, prioritize)
3. **Write** updated file back if changed (pretty-printed with 2-space indent)
4. **Display** the current backlog to the user after any operation

## File Format

```json
{
  "todo": [
    { "id": 1, "description": "Item description" },
    { "id": 2, "description": "Item description" }
  ],
  "done": [
    { "id": 3, "description": "Item description", "completed": "YYYY-MM-DD" }
  ]
}
```

## Rules

- Items in `todo` have sequential `id` values starting from 1
- Renumber ids after any removal or completion
- When marking done, add `completed` field with current date
- When showing the backlog, display todo items with their id numbers
- If no args provided, just display the current backlog
- If `BACKLOG.json` doesn't exist, create it with `{"todo":[],"done":[]}`
