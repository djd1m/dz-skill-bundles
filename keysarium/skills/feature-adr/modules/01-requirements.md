# Step 1: Requirements Gathering

> Transform vague feature description into structured, actionable requirements.

## When

Always runs. Adapts depth by tier:
- **S:** 3-5 bullet points, inline (no file)
- **M/L/XL:** Full structured document

## Model

sonnet (analytical work, not creative)

## Input

- Feature description from user
- `{COMPLEXITY_TIER}` from Step 0
- Codebase context (project structure, existing patterns)

## Protocol

### 1. Load Explore Skill (M/L/XL only)

```
Read: .claude/skills/explore/SKILL.md
```

Use the explore skill to clarify ambiguous requirements through adaptive questioning.
For S-tier: skip explore, extract requirements directly from description.

### 2. Identify Stakeholders

| Question | Why |
|----------|-----|
| Who is the end user? | Drives UX decisions |
| Who is the developer consumer? | Drives API design |
| Who approves delivery? | Defines done criteria |

### 3. Extract Functional Requirements

For each requirement:
```
FR-{N}: {Description}
Priority: MUST | SHOULD | COULD
Acceptance Criteria:
  - Given [context], When [action], Then [outcome]
```

### 4. Extract Non-Functional Requirements (M+ only)

| Category | Questions |
|----------|-----------|
| Performance | Latency targets? Throughput? |
| Security | Auth? Data sensitivity? |
| Scalability | Expected load? Growth? |
| Compatibility | Browser/OS/API versions? |
| Accessibility | WCAG level? |

### 5. Identify Constraints

- Technical: language, framework, existing patterns to follow
- Business: timeline, budget, compliance
- Organizational: team capabilities, review process

### 6. Define Scope Boundaries

Explicitly state:
- **In scope:** What this feature DOES
- **Out of scope:** What this feature does NOT do (but someone might assume)
- **Dependencies:** What must exist before this feature works
- **Dependents:** What will break if this feature changes

## Output

### For S-tier (inline)
Set `{REQUIREMENTS}` variable with 3-5 bullets. No file created.

### For M/L/XL
Create `features/<slug>/01_requirements.md` with:
- Stakeholders
- Functional requirements (FR-1..N) with acceptance criteria
- Non-functional requirements (L/XL)
- Constraints
- Scope boundaries
- Open questions (if any remain)

Set `{REQUIREMENTS}` variable with structured data.

## Checkpoint 1 Format

```
═══════════════════════════════════════════════════════
⏸️ STEP 1/8: Requirements Complete
<promise>FEATURE_ADR_REQUIREMENTS_GATHERED</promise>
Tier: {COMPLEXITY_TIER}

{N} functional requirements identified
{M} constraints, {K} open questions

• "ок" — proceed
• "добавь [requirement]" — add more
• "убери [requirement]" — remove
═══════════════════════════════════════════════════════
```

## Quality Gates

- [ ] All requirements have acceptance criteria (M+)
- [ ] Scope boundaries explicitly defined
- [ ] No ambiguous terms ("fast", "good", "flexible") without quantification
- [ ] Dependencies identified
- [ ] User confirmed requirements before proceeding
