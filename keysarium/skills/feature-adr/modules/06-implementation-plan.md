# Step 6: Implementation Plan

> Decompose the feature into ordered, estimable tasks with dependencies.

## When

Always runs. Adapts depth by tier:
- **S:** Inline checklist (3-5 items, no file)
- **M/L/XL:** Full structured plan document

## Model

sonnet (analytical decomposition) + code-goal-planner SPARC-GOAP methodology

## Input

- `{REQUIREMENTS}` from Step 1
- `{ADR_DECISIONS}` from Step 3 (M+)
- `{IDEATION_VERDICT}` and `{QUALITY_RISKS}` from Step 3.5 (M+)
- `{DOMAIN_MODEL}` from Step 4 (L/XL)
- `{ARCHITECTURE}` from Step 5 (M+)

## Protocol

> Load: `references/agentic-qe/code-goal-planner.md` for SPARC-GOAP methodology.

### 0. Apply SPARC-GOAP Goal State Analysis

Before decomposing tasks, define the goal state using the code-goal-planner SPARC methodology:

```yaml
goal: implement_{feature_slug}
current_state:
  features_complete: [existing features in codebase]
  test_coverage: {current %}
  relevant_files: [files that will be affected]

goal_state:
  features_complete: [...current, {new feature}]
  test_coverage: {target %}
  acceptance_criteria: [from {REQUIREMENTS}]
  adr_decisions_implemented: [from {ADR_DECISIONS}]
  quality_risks_mitigated: [from {QUALITY_RISKS}]

sparc_phases:
  specification: {REQUIREMENTS} + {ADR_DECISIONS}
  pseudocode: Algorithm/logic design from ADRs
  architecture: {ARCHITECTURE} + {DOMAIN_MODEL}
  refinement: TDD cycles per task group
  completion: Integration + deployment + validation
```

This analysis provides the planning context for task decomposition below.

### 1. Decompose into Tasks

Break the feature into atomic tasks. Each task should be:
- **Independent** — can be reviewed/tested in isolation
- **Completable** — produces working code (not half-done)
- **Testable** — has clear done criteria

Task format (SPARC-enhanced):
```
TASK-{N}: {Title}
  Description: {What to do}
  SPARC Phase: specification | pseudocode | architecture | refinement | completion
  Files: {Files to create/modify}
  Depends on: TASK-{M} (or none)
  Test: {How to verify it works}
  Success Criteria: {Measurable outcome}
  Risk Mitigation: {From QUALITY_RISKS if applicable}
```

### 2. Order by Dependencies

Build a dependency graph:

```
TASK-1: Create data model
TASK-2: Create repository (depends: TASK-1)
TASK-3: Create service (depends: TASK-2)
TASK-4: Create controller (depends: TASK-3)
TASK-5: Create tests (depends: TASK-2, TASK-3, TASK-4)
TASK-6: Create migration (depends: TASK-1)
```

Rules:
- Tasks with no dependencies go first
- Parallel-safe tasks are marked for concurrent execution
- Tests depend on the code they test (but can be written first — TDD)
- Map each task group to a SPARC phase (spec → pseudo → arch → refine → complete)

### 3. Identify Parallel Groups

Group tasks that can be worked on simultaneously:

```
Group 1: TASK-1, TASK-6  (no dependencies, can parallel)  [SPARC: architecture]
Group 2: TASK-2, TASK-3  (depend on Group 1)               [SPARC: refinement]
Group 3: TASK-4          (depends on Group 2)               [SPARC: refinement]
Group 4: TASK-5          (depends on all)                   [SPARC: completion]
```

For L/XL: assign parallel groups to different agents.

### 4. Define Checkpoints

Insert checkpoints between groups:
- After data model + migrations → verify schema
- After services → verify business logic
- After controllers → verify API contract
- After tests → verify coverage

### 5. Risk Assessment (enhanced with QCSD findings)

For each group, identify:
- What could block this task?
- What's the fallback if approach doesn't work?
- Does this task have external dependencies (APIs, packages)?
- **NEW**: Which risks from `{QUALITY_RISKS}` (Step 3.5) affect this task?
- **NEW**: What mitigation from the ideation report applies here?

### 6. Requirements Gap Check

Before finalizing the plan, validate completeness:

1. Cross-reference every `{REQUIREMENT}` (FR-N) → at least one TASK covers it
2. Cross-reference every `{ADR_DECISION}` → at least one TASK implements it
3. Cross-reference every critical risk from `{QUALITY_RISKS}` → mitigation in some TASK
4. If gaps found → add missing TASKs and re-order the DAG
5. **Iterate until no gaps remain** (max 3 iterations, then flag for user)

This gap-check loop ensures the implementation plan is complete before coding begins.

## Output

### S-tier (inline)
Inline checklist in conversation:
```
Implementation plan:
- [ ] {task 1}
- [ ] {task 2}
- [ ] {task 3}
```

### M/L/XL
Create `features/<slug>/06_implementation_plan.md` with:
- Task list with dependencies
- Parallel groups
- Checkpoint schedule
- Risk assessment (L/XL)

Set `{IMPL_PLAN}` variable.

## Checkpoint Format

```
═══════════════════════════════════════════════════════
⏸️ STEP 6/8: Implementation Plan Complete
<promise>FEATURE_ADR_PLANNED</promise>
Tier: {COMPLEXITY_TIER}

{N} tasks in {M} parallel groups
Estimated {K} files to create/modify

• "ок" — start coding
• "разбей [TASK-N]" — split task further
• "объедини [TASK-N, TASK-M]" — merge tasks
═══════════════════════════════════════════════════════
```

## Quality Gates

- [ ] Every task has clear done criteria
- [ ] Dependencies form a valid DAG (no cycles)
- [ ] Parallel groups correctly identified
- [ ] Each task touches identifiable files
- [ ] Total file count matches complexity tier expectation
