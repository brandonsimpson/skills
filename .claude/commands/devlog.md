# Developer Log Entry

You are writing a **public dev log entry** — a sanitized summary of the current work session for sharing on LinkedIn, a blog, and social media. The goal is to build in public: share the process, the decisions, and the lessons without revealing proprietary details.

## Process

### Step 1: Review the session

Look at what happened in this conversation:
- What was built, researched, or decided?
- What technical challenges were encountered?
- What assumptions were validated or invalidated?
- How long did it take?
- What would have gone wrong without this work?

### Step 2: Gather input

Ask the user ONE question:
> "What's the main takeaway you want people to get from this session? Anything specific to highlight or avoid mentioning?"

Wait for their response before proceeding.

### Step 3: Write the dev log

Write to `docs/devlog/YYYY-MM-DD-<topic>.md` (create the directory if it doesn't exist):

```markdown
# The Build #N: <topic>

**Date:** YYYY-MM-DD

---

<body>

---

*This is The Build, a series about building something real with AI. More entries to come.*
```

**Writing style:**

Voice & Tone — Conversational Authority:
- Write like explaining something important to a smart colleague over coffee
- Show your reasoning process — let readers see how you arrived at conclusions, not just what you concluded
- Build arguments incrementally rather than stating thesis upfront, but arrive at clear insights
- Balance vulnerability and confidence: use exploratory language ("I think", "it seems like") when working through genuinely uncertain problems; state things directly when speaking from direct experience
- Maintain professional positioning — conversational tone makes you more approachable, not less credible

Structure & Rhythm:
- Mix short (1-3 sentences) and medium (4-6 sentences) paragraphs to create rhythm
- Use single-sentence paragraphs for emphasis, transitions, or conclusions — not as default
- Don't front-load your thesis. Start with observation, question, or example. Let argument unfold.
- Use "But..." and "So..." to pivot and progress. Show turns in your thinking.
- End sections with clear takeaways — journey is exploratory, destination should be definite

Language & Expression:
- Use simple words for complex ideas
- Prefer "use" to "leverage", "improve" to "elevate", "explore" to "dive into"
- Be specific: "reduced query time from 2s to 50ms" not "improved performance"
- Avoid corporate buzzwords and AI cliches ("synergy", "paradigm shift", "unlock value", "unleash potential", "dive into")
- Don't apologize for having opinions — "I believe X" is stronger than "It seems like maybe X could be true"

Formatting:
- Avoid em dashes and en dashes; restructure sentences or use commas instead
- First person voice ("I built", "I discovered", not "we")
- Focus on outcomes and results
- Substance over hype

Content rules:
- Share the HOW and WHY, never the WHAT (product specifics)
- No product name, feature names, or competitive details
- No third-party service names — use generic descriptions ("my database provider", "the embedding API", "the hosting platform")
- No pricing strategy, unit economics, or customer details
- Be honest about mistakes, wrong assumptions, and course corrections
- If AI was used in the process, be transparent about it — including where AI-generated plans were wrong
- Keep it useful: readers should learn something they can apply to their own work

### Step 4: Generate social versions

After the dev log is written and approved, generate:

**LinkedIn version (short):**
- 3-5 short paragraphs max
- Hook in the first line
- End with a takeaway or question
- No hashtag spam (2-3 relevant ones max)

**Twitter/X version (thread or single):**
- If it fits in one post: single post
- If not: 3-5 tweet thread
- Punchy, direct

Save these as a comment block at the bottom of the dev log file:

```markdown
<!--
## Social Versions

### LinkedIn
<linkedin version>

### X/Twitter
<twitter version>
-->
```

### Step 5: Sanitization check

Before committing, verify:
- [ ] No product name or codename mentioned
- [ ] No feature names or product details
- [ ] No third-party service names (database providers, hosting platforms, auth services, etc.)
- [ ] No pricing, costs, or revenue numbers that reveal strategy
- [ ] No customer information
- [ ] No security implementation details
- [ ] No system prompt content or AI personality details
- [ ] The post is useful to other builders, not just self-promotion

### Step 6: Commit

Stage and commit with message:
```
devlog: <topic> — <1-line summary>
```

## What to Log (and When)

Good times to write a dev log:
- After completing a spike or technical validation
- After making a significant architecture decision
- After catching a major issue before it became a problem
- After a particularly productive or challenging build session
- After launching a tier or milestone
- When you learn something other builders would benefit from

## Rules

- Do NOT reveal proprietary details — the dev log is public
- Do NOT mention third-party services by name — competitors can read this
- Do NOT skip the sanitization check — review every line
- DO be honest about AI-assisted development — it's part of the story
- DO be honest about mistakes — that's what makes it valuable
- Keep it concise — readers are busy. If it's longer than 2 minutes to read, cut it.
