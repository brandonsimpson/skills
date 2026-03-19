# Technical Spike

Run a rapid validation spike on a technology, framework, API, or architectural approach before committing to it.

## When to use
- Evaluating a new framework or library ("will this work for us?")
- Validating an architectural assumption before building
- Testing an API or SDK before integrating it
- Comparing approaches with real code, not just docs

## Process

### Step 1: Understand what to validate

Ask the user ONE question:
> "What's the riskiest assumption? What would make this NOT work for us?"

Wait for their response. This focuses the spike on what actually matters.

### Step 2: Set up the spike

Create a spike directory using whatever language/tooling fits the project:
```
spikes/<name>/
  <minimal project setup>   — only what's needed to run the spike
  FINDINGS.md                — results (created after running)
```

Match the project's existing language and tooling. Install only the dependencies needed. Load env vars from the project root `.env` if applicable.

### Step 3: Write targeted tests

Each test validates ONE specific capability:
- Does it work at all? (basic connectivity/import)
- Does the key feature work? (the thing we actually need)
- Does it work with our constraints? (budget, security, isolation, existing stack)
- What are the gotchas? (unexpected behavior, missing features, wrong defaults)

Tests should be runnable scripts, not unit tests. The point is to see real behavior, not mock it.

### Step 4: Run and observe

Run each test. Capture:
- Does it work? (yes/no/partially)
- What's the actual API shape? (may differ from docs)
- Performance (latency, cost, resource usage)
- Gotchas discovered (things the docs don't mention)

### Step 5: Document findings

Write `FINDINGS.md` with:
```markdown
# Spike Findings: <name>

**Date:** YYYY-MM-DD
**Status:** VALIDATED / BLOCKED / NEEDS MORE INVESTIGATION

## Results

| Feature | Status | Notes |
|---------|--------|-------|
| Feature 1 | WORKS / FAILS | details |
| Feature 2 | WORKS / FAILS | details |

## Key Observations
- What surprised us
- What the docs got wrong
- What we'd do differently

## Impact on Architecture
- Changes needed to existing plans
- New patterns discovered
- Recommended integration approach

## Next Steps
- What to build based on these findings
```

### Step 6: Commit and update backlog

```bash
git add spikes/<name>/
git commit -m "spike: <name> — <VALIDATED/BLOCKED>"
```

Update BACKLOG.md with findings if relevant.

## Tips

- **Keep spikes small.** Each test should take <5 minutes to run. The whole spike should take <1 hour.
- **Test with real APIs.** Don't mock — the point is to find real-world surprises.
- **Document the actual API shape.** Copy the working code patterns into FINDINGS.md so future implementation can copy-paste.
- **Kill early.** If the first test fails fundamentally, document why and stop. Don't polish a spike.
- **Always check: does it work with the project's stack?** Validate compatibility with the existing language, frameworks, and infrastructure.

## Examples

```
/spike redis vs memcached for session storage
/spike stripe metered billing
/spike cloudflare r2 for artifact storage
/spike grpc between python and go services
/spike sqlite with embedded replicas
```
