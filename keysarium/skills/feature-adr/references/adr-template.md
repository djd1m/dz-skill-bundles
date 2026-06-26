# ADR Template

Use this template for every Architecture Decision Record.

---

```markdown
# ADR-{NNN}: {Short Title of Decision}

## Status

{Proposed | Accepted | Deprecated | Superseded by ADR-{M}}

## Date

{YYYY-MM-DD}

## Context

{What is the issue that we're seeing that is motivating this decision or change?
Describe the forces at play — technical, business, political, social.
Describe the problem space, not the solution.}

## Decision Drivers

- {driver 1: e.g., "Team has no experience with technology X"}
- {driver 2: e.g., "Feature requires sub-100ms latency"}
- {driver 3: e.g., "Must integrate with existing auth system"}

## Considered Options

### Option 1: {Name}

{1-2 paragraph description of this option}

- ✅ {Pro 1}
- ✅ {Pro 2}
- ❌ {Con 1}
- ❌ {Con 2}

### Option 2: {Name}

{1-2 paragraph description}

- ✅ {Pro 1}
- ❌ {Con 1}

### Option 3: {Name} (optional)

{1-2 paragraph description}

- ✅ {Pro 1}
- ❌ {Con 1}

## Decision Matrix

| Criterion | Weight | Option 1 | Option 2 | Option 3 |
|-----------|--------|----------|----------|----------|
| {Criterion 1} | {1-5} | {1-5} | {1-5} | {1-5} |
| {Criterion 2} | {1-5} | {1-5} | {1-5} | {1-5} |
| {Criterion 3} | {1-5} | {1-5} | {1-5} | {1-5} |
| **Weighted Total** | | **{sum}** | **{sum}** | **{sum}** |

## Decision

We chose **{Option N}** because {1-2 sentence rationale linking back to decision drivers}.

## Consequences

### Positive
- {Positive consequence 1}
- {Positive consequence 2}

### Negative
- {Negative consequence 1 — with mitigation strategy}

### Risks
- {Risk 1} — Mitigation: {how to mitigate}
- {Risk 2} — Mitigation: {how to mitigate}

## Links

- Requirement: FR-{N}
- Related: ADR-{M} (if any)
```

---

## ADR Numbering

- Start from 001 for each feature
- Use kebab-case slugs: `001-choose-database-engine.md`
- Keep sequential within a feature, no gaps

## ADR Quality Checklist

- [ ] At least 2 options considered
- [ ] Every option has both pros and cons
- [ ] Decision matrix used for non-trivial decisions
- [ ] Consequences include both positive AND negative
- [ ] Risks have mitigation strategies
- [ ] Links to requirements present
