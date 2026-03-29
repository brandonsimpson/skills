---
name: runbook
description: Use when the user asks to create, update, or refresh a production runbook, ops guide, or incident response documentation for a deployed application — audits actual infrastructure config to generate reality-based operations documentation
---

# Production Runbook Generator

## Overview

Generate a production runbook by auditing the **actual deployed infrastructure** — not by writing generic docs. The runbook should be copy-pasteable commands, not prose.

## When to Use

- User asks for a runbook, ops guide, or production playbook
- User wants to document how to manage their app in production
- User wants incident response or troubleshooting documentation
- New team member needs to understand production operations

## Process

### 0. Check for Existing Runbook

Before generating, check if a runbook already exists:

- Look for `docs/operations/production-runbook.md` or similar (`**/runbook*`, `**/ops-guide*`, `**/playbook*`)
- If found, **read it** and switch to **update mode**:
  - Re-audit infrastructure (step 1) to detect what changed
  - Diff against existing runbook sections
  - Present a summary: "Here's what changed since the runbook was last generated: [list]"
  - Update only the sections that are stale or missing — preserve any hand-written notes, customizations, or tribal knowledge the team added
  - Update the "Last generated" date
- If not found, proceed to full generation (steps 1-4)

### 1. Audit Infrastructure

Read every deployment-related file in the project:

- **Container/deploy config:** Dockerfiles, docker-compose.yml, fly.toml, railway.json, render.yaml, vercel.json, netlify.toml, serverless.yml, terraform files, k8s manifests
- **CI/CD:** .github/workflows/, .gitlab-ci.yml, Jenkinsfile, buildspec.yml
- **Environment:** .env.example, .env.template (never .env itself)
- **Database:** migration files, schema files, seed scripts, ORM config
- **Monitoring:** alerting rules, health check endpoints, logging config
- **Package config:** package.json scripts, Makefile, Procfile

If files reference external services (databases, queues, CDNs, auth providers), note them.

### 2. Generate Runbook

Write a single markdown file covering these sections. **Every section must use actual commands derived from the audit, not generic examples.**

```markdown
# Production Runbook — [Project Name]

> Last generated: [date]
> Infrastructure: [platforms discovered]

## Service Inventory

| Service | Platform | URL/Host | Health Check |
|---------|----------|----------|--------------|
| [from config] | [from config] | [from config] | [from config] |

## Prerequisites

- CLI tools needed (with install commands)
- Access/permissions required
- Environment setup

## Common Operations

### Deploy
[Exact deploy commands from CI/CD config or deploy scripts]

### Rollback
[Platform-specific rollback commands]

### Restart
[Per-service restart commands]

### Scale
[Scale up/down commands with safe defaults]

## Health Checks

[Per-service: exact curl/CLI commands to verify health]
[What a healthy response looks like]
[What an unhealthy response looks like]

## Database Operations

### Backups
[Backup commands specific to their database]

### Migrations
[How migrations run — from actual migration config]

### Connection
[How to connect for debugging — from actual connection config]

## Secrets Management

| Secret | Location | Rotation Frequency |
|--------|----------|--------------------|
| [from env templates and deploy config] | | |

[How to update secrets per platform]

## Logs and Observability

[Where logs live — from actual logging config]
[How to tail, search, filter]
[Common error patterns to watch for]

## Incident Response

### Triage Checklist
1. Check health endpoints (commands from Health Checks section)
2. Check recent deploys (git log / platform deploy history command)
3. Check logs for errors (commands from Logs section)
4. Check database connectivity (commands from Database section)
5. Check external service status (list discovered dependencies)

### Troubleshooting Decision Tree

| Symptom | Check First | Then Check | Common Fix |
|---------|-------------|------------|------------|
| [derived from architecture] | | | |

### Escalation
[Who to contact, where to post, what information to include]

## Cost and Resource Monitoring

[Platform-specific commands to check usage/spend]
[How to identify and kill runaway processes]
[Budget alerts if configured]

## Scheduled Tasks

[Cron jobs, scheduled functions, recurring tasks from config]
[How to verify they're running]
[How to run manually]
```

### 3. Incorporate Project Knowledge

After generating from config, scan for additional context:

- **CLAUDE.md / README / docs/**: deployment gotchas, conventions, known issues
- **Git history**: `git log --oneline --grep="deploy\|hotfix\|rollback" -20` for recent operational patterns
- **Comments in config files**: often contain tribal knowledge

Fold any relevant findings into the appropriate runbook section.

### 4. Output

Write to `docs/operations/production-runbook.md` (or ask user for preferred location).

## Rules

- **Commands, not prose.** Every operation should be a copy-pasteable command.
- **Audit first, write second.** Never generate generic content — everything comes from actual config.
- **Flag gaps.** If a section can't be filled from project files, say "NOT CONFIGURED — recommend setting up [X]" rather than inventing content.
- **No secrets.** Never include actual secret values, connection strings, or credentials. Reference where they're stored.
- **Include verification.** Every operation should end with how to verify it worked.
