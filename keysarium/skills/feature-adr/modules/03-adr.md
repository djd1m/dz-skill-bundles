# Step 3: Architecture Decision Records

> Document each significant architectural decision with context, alternatives, and rationale.

## When

M+ tiers. Depth varies:
- **M:** 1 ADR for the main decision
- **L/XL:** N ADRs for each significant decision

Can run **in parallel** with Step 2 (Research) for L/XL tiers.

## Model

opus (complex reasoning for trade-off analysis)

## Input

- `{REQUIREMENTS}` from Step 1
- `{RESEARCH_FINDINGS}` from Step 2 (if available — may arrive later in parallel)
- Codebase context

## Protocol

### 1. Identify Decisions

Scan requirements for decision points:

| Signal | Example |
|--------|---------|
| Technology choice | "Which database/queue/framework?" |
| Pattern choice | "Repository vs. Active Record?" |
| Integration strategy | "REST vs. GraphQL vs. gRPC?" |
| Data modeling | "Normalized vs. denormalized?" |
| Deployment strategy | "Monolith vs. microservice?" |
| Trade-off | "Consistency vs. availability?" |

For M-tier: pick the SINGLE most impactful decision.
For L/XL: identify ALL decisions, prioritize by impact.

### 2. Draft ADR per Decision

Use template from `references/adr-template.md`:

```markdown
# ADR-{NNN}: {Title}

## Status
Proposed | Accepted | Superseded by ADR-{M}

## Context
What is the issue that we're seeing that is motivating this decision?

## Decision Drivers
- {driver 1}
- {driver 2}

## Considered Options
1. {Option A} — {1-line summary}
2. {Option B} — {1-line summary}
3. {Option C} — {1-line summary}

## Decision
We chose **{Option X}** because...

## Consequences
### Positive
- ...
### Negative
- ...
### Risks
- ...
```

### 3. Evaluate Alternatives

For each ADR, evaluate alternatives using a decision matrix:

| Criterion | Weight | Option A | Option B | Option C |
|-----------|--------|----------|----------|----------|
| Simplicity | 3 | ⭐⭐⭐ | ⭐⭐ | ⭐ |
| Performance | 2 | ⭐⭐ | ⭐⭐⭐ | ⭐⭐ |
| Maintainability | 3 | ⭐⭐ | ⭐⭐ | ⭐⭐⭐ |
| Team familiarity | 2 | ⭐⭐⭐ | ⭐ | ⭐⭐ |

If decision involves complex trade-offs, load `problem-solver-enhanced` for TRIZ analysis.

### 4. Validate Against Requirements

Each ADR must trace back to specific requirements:
- Which FR-{N} does this decision support?
- Does the decision satisfy all relevant NFRs?
- Does the decision respect constraints?

## Anti-Patterns

| Anti-Pattern | Detection | Fix |
|-------------|-----------|-----|
| Only one alternative | Less than 2 options considered | Require ≥2 alternatives |
| No consequences | Missing positive/negative analysis | BLOCK until filled |
| Decision without context | Why is unclear | Add context section |
| Premature optimization | Choosing complex option for hypothetical scale | Prefer simpler option |

## Output

Create `features/<slug>/03_adr/` directory with:
- `001-{decision-slug}.md` for each ADR
- Each ADR follows the template

Set `{ADR_DECISIONS}` variable with list of decisions.

## Checkpoint Format

```
═══════════════════════════════════════════════════════
⏸️ STEP 3/8: ADR Complete
Tier: {COMPLEXITY_TIER}

{N} architectural decisions documented:
1. ADR-001: {title} → chose {option}
2. ADR-002: {title} → chose {option}

• "ок" — proceed
• "пересмотри ADR-{N}" — reconsider decision
• "добавь ADR для [topic]" — add new decision
═══════════════════════════════════════════════════════
```

## Shift-Left Validation (after ADR creation)

After all ADRs are drafted, apply **shift-left-testing** protocol (Level 4: Risk Analysis in Design).

> Load: `references/agentic-qe/shift-left-testing.md`

### Testability Check per ADR

For each ADR decision, run risk analysis:

```
ADR-{NNN}: {Title}
  Testability Questions:
    1. What happens when {chosen option} fails under load?
    2. How do we handle {alternative failure modes}?
    3. What if {external dependency} becomes unavailable?
    4. Can we test this decision in isolation?
  Generated BDD Scenarios:
    Given {precondition from ADR context}
    When {action that exercises the decision}
    Then {expected outcome per ADR consequences}
  Risk Level: LOW | MEDIUM | HIGH
```

### Shift-Left Gate

| Check | Threshold |
|-------|-----------|
| All ADRs have testability questions answered | 100% |
| BDD scenarios generated for non-trivial decisions | ≥1 per ADR |
| No HIGH risk without mitigation documented | 0 unmitigated |

If any ADR has HIGH risk without mitigation → flag at checkpoint for user decision.

### Integration with Step 3.5

The shift-left validation output feeds directly into Step 3.5 (QCSD Ideation Swarm):
- BDD scenarios become input for the requirements-validator agent
- Risk analysis feeds the risk-assessor agent
- Testability questions inform the quality-criteria-recommender

## Quality Gates

- [ ] Every ADR has ≥2 alternatives considered
- [ ] Every ADR has consequences (positive + negative)
- [ ] Every ADR traces to requirements
- [ ] Decision matrix used for non-trivial choices
- [ ] No premature optimization detected
- [ ] Shift-left testability check passed for all ADRs
- [ ] BDD scenarios generated for non-trivial ADRs
