# Step 2: Research — Analogues & Patterns

> Research existing patterns in codebase and external best practices before designing.

## When

L/XL tiers only. Can run **in parallel** with Step 3 (ADR).

## Model

sonnet (research synthesis)

## Input

- `{REQUIREMENTS}` from Step 1
- `{COMPLEXITY_TIER}` from Step 0
- Codebase access

## Protocol

### 1. Codebase Pattern Research (Agent 1 — sonnet)

Search the current codebase for:

| What | How |
|------|-----|
| Similar features | Grep for related domain terms, find analogous implementations |
| Established patterns | How does existing code handle similar problems? |
| Shared utilities | What existing helpers, services, or abstractions can be reused? |
| Test patterns | How are similar features tested? |
| Data access patterns | How do similar features access data? |

Document each finding with file path + line reference.

### 2. External Pattern Research (Agent 2 — sonnet)

Research external analogues:

| What | How |
|------|-----|
| Industry patterns | How do similar products solve this? |
| Framework conventions | What does the framework recommend? |
| Library options | Are there well-maintained libraries for this? |
| Anti-patterns | What are known pitfalls to avoid? |

Use `goap-research-ed25519` for verified research if the feature involves:
- Security-sensitive decisions
- Compliance requirements
- Performance-critical paths

### 3. Synthesize Findings

Combine both research streams into:

```
## Pattern Summary
1. Codebase has [N] similar implementations using [pattern]
2. Framework recommends [approach] for this type of feature
3. Key libraries considered: [list with trade-offs]
4. Anti-patterns to avoid: [list]

## Recommendation
Based on research, the recommended approach is [X] because [reasons].
Alternative considered: [Y], rejected because [reasons].
```

## Agent Swarm (2 parallel, sonnet)

| Agent | Task | Model |
|-------|------|-------|
| Agent 1 | Codebase pattern research | sonnet |
| Agent 2 | External analogues research | sonnet |

After both complete, synthesize results sequentially.

## Output

Create `features/<slug>/02_research.md` with:
- Codebase patterns found (with file references)
- External patterns researched
- Library evaluation (if applicable)
- Anti-patterns identified
- Recommended approach

Set `{RESEARCH_FINDINGS}` variable.

## Quality Gates

- [ ] At least 3 codebase patterns examined
- [ ] External patterns verified (not hallucinated)
- [ ] Trade-offs documented for each alternative
- [ ] Clear recommendation with rationale
