---
name: repo-audit
description: Use when asked to audit a repository's health, assess technical debt, evaluate codebase quality end-to-end, or produce a prioritized improvement plan — including "how bad is this code", new-team-takeover or pre-acquisition reviews, and periodic engineering health checks. Analysis-only; never modifies code.
version: "1.0.0"
---

# Repo Audit

Evidence-based, four-phase repository audit producing a single deliverable: health grade, findings with file:line citations, improvement strategy, and a milestone task plan. Core principle: **every claim is grounded in verified source; anything unverifiable is labeled as such, never guessed.**

## When NOT to use

- Reviewing a single diff/PR → use a code-review skill
- Security-only assessment → use a security-review skill
- The owner wants fixes applied → audit first, fix in a separate pass (this skill is read-only)

## Phase 0 — Calibration (do this before anything else)

1. **Read what the repo already knows about itself.** README, CONTRIBUTING, ADRs, architecture/status docs, CLAUDE.md/AGENTS.md, known-gaps or tech-debt lists, open issues. **Only report deltas** — a finding the repo already self-documents is noise unless the documentation is wrong (stale docs that contradict code ARE a finding, in either direction: gaps documented-as-open-but-fixed matter too).
2. **Record the owner's stated goals** (launch gates, cost constraints, scale targets, deadlines — from docs or by asking). Every severity rating in Phase 2 must be anchored to these goals, not to generic best practice. Without an anchor, everything drifts to "Medium."
3. **Size the repo** (`find . -name "*.<ext>" -not -path "*/node_modules/*" | xargs wc -l`). This decides the execution shape:
   - **< ~30k LOC:** run all phases in this context.
   - **Larger:** fan out parallel **read-only** subagents in Phase 2, one per dimension cluster (see template below). No single context can audit 8 dimensions at depth on a large repo — without fan-out the audit silently goes shallow.

## Phase 1 — Discovery & Mapping (read before judging)

Map directory structure, languages, frameworks, runtime targets. Identify entry points, core modules, main data/control flow. Read package manifests, lockfiles, build/CI/env config. Determine purpose, intended users, maturity (prototype / internal tool / production / library). Note existing conventions (naming, boundaries, error handling, test style) so recommendations fit the culture.

**Output:** a concise Repo Map — purpose, stack, architecture sketch, key directories with one-liners, LOC per major area, and anything that surprised you.

## Phase 2 — Audit (evidence-based, severity-rated)

Dimensions (cluster for subagent fan-out as shown):

1. **Architecture & design** — boundaries, coupling, cycles, god files, layering violations, scalability assumptions (single-process state, boot-time snapshots, frozen caches)
2. **Code quality** — complexity hotspots (measure: largest files/functions), duplication (especially duplicated taxonomies/registries/allowlists — cite ALL copies), dead code, swallowed exceptions, type-safety escapes (`as any` density per package), inconsistent patterns (e.g. mixed error-response shapes)
3. **Security** — secrets in tracked files, injection (SQL, command — check every shell-string composition), input validation ratio, authN/authZ (read the actual middleware), permissive configs, **and whether claimed enforcement mechanisms actually enforce** (read the CI gate/test that claims to guarantee a property; substring checks over curated allowlists are theater)
4. **Testing + performance** — coverage distribution (zero-test pockets in core logic), placeholder/skip-green tests, e2e suites that exist but never run in CI, flaky patterns (real-timer sleeps); N+1s, blocking calls in async paths, missing indexes (read migrations vs query WHERE clauses), **unbounded growth** (append-only tables, dead TTL columns, cache maps without eviction)
5. **Dependencies + DevEx/ops + docs** — version splits of the same lib across a monorepo, dev-deps in prod deps, CVE counts; lint/format actually enforced in CI (verify by reading workflow files, not by config presence), build artifacts tracked in git, health checks on every deployed service, silent-failure infra (`|| true`, `2>/dev/null` in Dockerfiles); doc staleness vs the repo's own freshness standard, docs contradicting code (spot-check 3+ concrete claims)

**Evidence rules (mandatory, include verbatim in every subagent prompt):**
- Every finding cites file:line. READ the lines before citing — never cite from inference or memory.
- Label each finding **FACT** (verified in code) or **JUDGMENT** (assessment).
- Severity Critical/High/Medium/Low, justified by a **concrete consequence anchored to the owner's stated goals** — who/what breaks, costs, or slows.
- Prefer ~10-15 high-confidence findings per dimension over 50 speculative ones. Unverifiable suspicions go in a separate "Unverified" list.
- List genuine **strengths** — they decide what to preserve, and they keep severity calibration honest.
- End with **coverage notes**: what got light or zero review.

**Phase 2.5 — Verification pass (before publishing, non-negotiable):** independently re-read the source behind the 3-5 most load-bearing claims (the ones the grade and Milestone 1 rest on). A finding that fails re-verification is dropped or downgraded, never softened in place. If subagents did Phase 2, this pass happens in the main context.

## Phase 3 — Improvement Strategy

- Identify the 3-5 **themes** that explain most findings.
- Per theme: target state + the principle behind it.
- **Explicit non-goals:** what you recommend NOT fixing and why (effort vs payoff, risk, maturity). Match the owner's stated build-vs-defer philosophy if they have one.
- **Definition of done:** measurable signals (grep-able counts hitting zero, CI jobs that fail on regression, tests that exist and run).

## Phase 4 — Task Plan

- Discrete tasks: title, one-paragraph description, files affected, acceptance criteria, effort (S <2h / M half-day / L 1-2 days / XL needs breakdown), risk of the change itself, dependencies.
- Milestones: **M0** safety net (tests/characterization around anything M1 will change, CI gates) → **M1** critical correctness + security → **M2** high-leverage (makes future work easier) → **M3** polish.
- Flag **quick wins** (high impact, S effort) separately.
- Implementation sketch (approach, key steps, gotchas) for the top 3 tasks.

## Final deliverable

One document: **Executive Summary** (≤10 sentences: grade A-F with justification, top 3 risks, top 3 opportunities) → Repo Map → Audit Report (findings by dimension sorted by severity, strengths, coverage notes) → Improvement Strategy → Task Plan (milestones + table + quick wins + sketches) → **Open Questions** for the owner. Save it where the repo keeps research/analysis docs if such a convention exists; otherwise present it directly. If a tracking system exists (backlog/issues), offer to file the tasks.

## Constraints

- **Never modify code.** Analysis only. Subagents get an explicit read-only hard constraint (no stash/checkout/reset/writes).
- Don't pad — a healthy dimension gets one sentence.
- Calibrate to maturity; don't prescribe enterprise infra for a prototype unless the owner's goals demand it.
- On large repos, go deep on the core 20% that does 80% of the work; say which areas got lighter review.

## Subagent prompt template (Phase 2 fan-out)

> You are a principal-level auditor examining <repo> at <path>. HARD CONSTRAINT: read-only — never modify files, never run git stash/checkout/reset or any tree-mutating command.
> CONTEXT: <purpose, package layout + LOC, owner's stated goals>. The repo already self-documents these gaps — do NOT re-report them, but verify whether each is still true: <list>.
> YOUR DIMENSION: <dimension + specific checks from Phase 2 list, with named suspect files>.
> EVIDENCE RULES: <the mandatory block above, verbatim>.
> OUTPUT: structured findings ([SEVERITY] [FACT|JUDGMENT] title — what, where file:line, why it matters), counts, strengths, coverage notes. Raw data, not prose.

## Common mistakes

| Mistake | Fix |
|---|---|
| Re-discovering documented gaps | Phase 0 step 1; report deltas only |
| Generic severity ("violates best practice") | Anchor every rating to the owner's stated goals |
| Trusting claimed enforcement ("CI gate exists") | Read the gate; verify it can actually fail |
| Single-context audit of a large repo | Fan out per-dimension subagents; synthesize centrally |
| Publishing without re-verifying top claims | Phase 2.5 — re-read source behind the load-bearing findings |
| All-findings-no-strengths reports | Strengths decide what to preserve; require them |
| Plan starts with refactors | M0 safety net first; characterization tests before engine changes |
