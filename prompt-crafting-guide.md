# How to Craft an Effective Prompt

*By [Brandon Simpson](https://brandon.cc) · [@surfnscale](https://x.com/surfnscale)*

> Distilled from 20+ research papers (2025–2026), validated across independent benchmarks.
> See [References](#references) below for full citations.

---

## The Rules

**1. Keep it short.** The sweet spot is 150-300 words. Performance degrades around 3,000 tokens. Every extra word competes for the model's attention. If you can say it in fewer words, do it.

**2. Structure beats prose.** Use XML tags, headers, or clear sections — not long paragraphs. Treat your prompt like a spec, not an essay. Claude specifically performs better with XML-structured instructions.

**3. Put the important stuff first and last.** Models lose accuracy on information buried in the middle of long prompts ("lost in the middle" effect). Critical instructions go at the top or bottom.

**4. Personas help creative work, hurt factual work.** "You are an expert marketer" improves writing, brainstorming, and content generation. The same approach actively degrades accuracy on math, facts, coding, and analysis. If you need facts, skip the persona and just ask directly.

**5. Don't ask it to think step by step.** Modern models already reason internally. Explicit chain-of-thought instructions are redundant at best and harmful at worst — they add cost, latency, and can introduce errors the model wouldn't otherwise make.

**6. 2-3 examples max.** If you're giving examples, the biggest jump is from zero to two. After that, returns diminish fast and you're burning tokens.

**7. Be specific about what you want, not how to think.** Instead of "carefully analyze and consider all factors," say "list the top 3 risks with one sentence each." Constraints on output format beat instructions on reasoning process.

**8. Irrelevant details hurt.** Adding a persona's name, backstory, or favorite color can drop performance by up to 30 percentage points. Only include details that directly serve the task.

---

## Examples

### Example 1: API design task

**Bad prompt:**

```text
You are a world-class senior staff engineer with 25 years of experience building
distributed systems at Google, Amazon, and Netflix. You have deep expertise in
REST, GraphQL, gRPC, and event-driven architectures. Please carefully think
through step by step and design an API for a task management app. Consider all
relevant factors including scalability, security, versioning, rate limiting,
pagination, caching, error handling, authentication, and documentation. Be
thorough and comprehensive. Think about edge cases.
```

Problems: unnecessary persona backstory (Google, Amazon — irrelevant details that hurt accuracy), explicit chain-of-thought ("think step by step"), vague scope ("all relevant factors", "be thorough"), no concrete constraints on what the API actually needs to do, ~80 words of noise before the actual ask.

**Good prompt:**

```text
Design a REST API for a task management app.

<requirements>
- Users can create, read, update, delete tasks
- Tasks have: title, description, status (todo/in_progress/done), due date, assignee
- Tasks belong to projects; users belong to teams
- A user can only see tasks in their team's projects
</requirements>

For each endpoint, provide:
- Method and path
- Request body (if any)
- Response shape (TypeScript interface)
- One sentence on the auth check required

List the endpoints in a table. At the end, note which two endpoints will need
pagination and what cursor strategy you'd use.
```

Why it works: no persona (factual/technical task), no chain-of-thought, concrete data model, specific output format (table + TypeScript interfaces), scoped deliverable (endpoints + pagination note), authorization constraints stated upfront.

---

### Example 2: Debugging / code review task

**Bad prompt:**

```text
You are an expert React developer who specializes in performance optimization.
Please carefully review this component and think step by step about what might
be causing the performance issues. Consider all possible causes and provide a
comprehensive analysis with recommendations for improvement.
```

Problems: persona on a factual task (hurts accuracy), explicit chain-of-thought, "all possible causes" and "comprehensive analysis" invite bloat, no actual code or symptoms provided, no constraints on output.

**Good prompt:**

```text
This React component re-renders every 200ms when the user types in the
search box, causing visible input lag on mobile.

<code>
export function SearchPage() {
  const [query, setQuery] = useState('')
  const [results, setResults] = useState([])
  const allProducts = useContext(ProductContext)

  useEffect(() => {
    const filtered = allProducts.filter(p =>
      p.name.toLowerCase().includes(query.toLowerCase())
    )
    setResults(filtered)
  }, [query, allProducts])

  return (
    <div>
      <input value={query} onChange={e => setQuery(e.target.value)} />
      {results.map(p => <ProductCard key={p.id} product={p} />)}
    </div>
  )
}
</code>

1. What is causing the excessive re-renders?
2. Provide a fixed version of the component.
3. One sentence: would you debounce or use `useDeferredValue` here, and why?
```

Why it works: no persona (technical analysis — personas hurt accuracy on code), symptom described concretely ("every 200ms", "input lag on mobile"), actual code provided inline, three specific questions instead of "comprehensive analysis," forces a decision with rationale in the last question.

---

### Example 3: Mobile feature implementation task

**Bad prompt:**

```text
I need to add push notifications to my mobile app. Tell me everything I need
to know. Cover iOS and Android. Make sure it's production-ready and follows
best practices. Include error handling and edge cases.
```

Problems: unbounded scope ("everything I need to know"), no stack specified, "best practices" is vague, "production-ready" means different things in different contexts, no indication of what the app does or what notifications it needs.

**Good prompt:**

```text
<context>
React Native app (Expo managed workflow, SDK 52) with an Express backend
on Railway. Users receive order status updates — we need to notify them
when an order ships and when it's delivered.
</context>

<task>
Add push notifications for order status changes.
</task>

<constraints>
- Use expo-notifications (already installed but unconfigured)
- Backend sends notifications via Expo's push API
- Handle three cases: app foregrounded, backgrounded, killed
- Token storage: add a `push_token` column to the existing `users` table
</constraints>

Provide:
1. Backend: Express endpoint that accepts an order status webhook and sends
   the push notification (full code)
2. Frontend: Hook that registers for notifications and saves the token (full code)
3. A 3-row table: foreground / background / killed — what happens in each state
   and how to handle it
```

Why it works: specific stack and tooling (Expo SDK 52, Express, Railway), scoped to exactly two notification types, concrete constraints (existing library, specific DB column), three clear deliverables with explicit format expectations, no persona (technical task).

---

## References

Distilled from 20+ research papers (2025–2026), independently validated across multiple benchmarks and domains:

- **Multi-model testing** — Wharton GAIL reports tested across 6 LLM models on real-world tasks
- **Financial QA** — FinQA benchmarks comparing small models to GPT-4 (77.59 vs 77.51 accuracy)
- **Legal** — Analysis of 700+ real court cases involving AI hallucinations with documented sanctions
- **Healthcare** — Hallucination mitigation studies in production medical systems
- **Persona–accuracy tradeoff** — Confirmed independently by USC, Wharton, EMNLP, and multiple other research groups

Listed by topic area.

### Personas & Role Prompting

1. Hu, Rostami, Thomason. *Expert Personas Improve LLM Alignment but Damage Accuracy* (USC PRISM, 2026). [arXiv:2603.18507](https://arxiv.org/abs/2603.18507)
2. Basil, Shapiro, Shapiro, Mollick, Mollick, Meincke. *Playing Pretend: Expert Personas Don't Improve Factual Accuracy* (Wharton GAIL Report 4, 2025). [arXiv:2512.05858](https://arxiv.org/abs/2512.05858)
3. Hovy et al. *Principled Personas* (EMNLP 2025). [arXiv:2508.19764](https://arxiv.org/abs/2508.19764)
4. Park et al. *Persona is a Double-Edged Sword: Jekyll & Hyde Framework* (2024). [arXiv:2408.08631](https://arxiv.org/abs/2408.08631)
5. *When A Helpful Assistant Is Not Really Helpful* (2023). [arXiv:2311.10054](https://arxiv.org/abs/2311.10054)

### Chain-of-Thought & Reasoning

6. Meincke, Mollick, Mollick, Shapiro. *The Decreasing Value of Chain of Thought* (Wharton GAIL Report 2, 2025). [arXiv:2506.07142](https://arxiv.org/abs/2506.07142)
7. *Adaptive Prompting for Reasoning: Think Beyond Size* (2024). [arXiv:2410.08130](https://arxiv.org/abs/2410.08130)

### Prompt Structure & Position Effects

8. *Lost in the Middle: How Language Models Use Long Contexts* (2023). [arXiv:2307.03172](https://arxiv.org/abs/2307.03172)
9. *Context Rot* (Chroma Research, 2025). [research.trychroma.com](https://research.trychroma.com/context-rot)
10. *The Impact of Prompt Bloat on LLM Output Quality* (MLOps Community). [mlops.community](https://mlops.community/the-impact-of-prompt-bloat-on-llm-output-quality/)

### Surveys & Systematic Reviews

11. Meincke, Mollick, Mollick, Shapiro. *Prompt Engineering is Complicated and Contingent* (Wharton GAIL Report 1, 2025). [arXiv:2503.04818](https://arxiv.org/abs/2503.04818)
12. *Incentive Prompting: Threats and Tips* (Wharton GAIL Report 3, 2025). [arXiv:2508.00614](https://arxiv.org/abs/2508.00614)
13. *The Prompt Report: A Systematic Survey of Prompting Techniques* (2024). [arXiv:2406.06608](https://arxiv.org/abs/2406.06608)

### Prompt Routing & Automation

14. Cetintemel et al. *SPEAR: Making Prompts First-Class Citizens* (Brown University, CIDR 2026). [arXiv:2508.05012](https://arxiv.org/abs/2508.05012)
15. Zhang et al. *Scalable Prompt Routing via Fine-Grained Latent Task Discovery* (AWS, 2026). [arXiv:2603.19415](https://arxiv.org/abs/2603.19415)
16. *Automatic Prompt Generation via Adaptive Selection of Prompting Techniques* (2025). [arXiv:2510.18162](https://arxiv.org/abs/2510.18162)

### Domain-Specific Applications

17. *Healthcare Hallucination Mitigation: Granular Fact-Checking and Domain-Specific Adaptation* (2025). [arXiv:2512.16189](https://arxiv.org/abs/2512.16189)
18. *Domain Knowledge Injection Survey* (2025). [arXiv:2502.10708](https://arxiv.org/abs/2502.10708)

### Claude-Specific Guidance

19. *Claude Prompting Best Practices* (Anthropic, 2026). [platform.claude.com](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices)
20. *Use XML Tags* (Anthropic). [platform.claude.com](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/use-xml-tags)
21. *Prompt Engineering Guide 2026* (Lakera). [lakera.ai](https://www.lakera.ai/blog/prompt-engineering-guide)
