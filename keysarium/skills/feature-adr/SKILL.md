---
name: feature-adr
description: >
  11-step feature development pipeline with Complexity Router (S/M/L/XL),
  ADR-driven architecture decisions, DDD modeling, QCSD quality swarm,
  SPARC-GOAP planning, brutal-honesty review, and multi-agent fleet QE.
  Integrates 15 agentic-qe skills (9 core + 6 extended) for comprehensive
  quality engineering. Supports 3 modes: Reference (no setup), Direct (--full-qe),
  and Direct Extended (--full-qe-extended).
  Triggers on "реализуй фичу", "feature implementation", "добавь функциональность",
  "implement feature", "/feature-adr".
trust_tier: 0
trust_tier_label: "Advisory"
trust_tier_path: "Run /bto-test to promote to Tier 1"
agentic_qe_version: "7.5.1"
agentic_qe_source: "https://github.com/proffesor-for-testing/agentic-qe"
agentic_qe_skills_core: 9
agentic_qe_skills_extended: 6
agentic_qe_modes: ["reference", "direct", "direct-extended"]
---

# Feature ADR — Adaptive Feature Development Pipeline

> 11-шаговый pipeline для разработки фич любой сложности.
> Complexity Router автоматически определяет масштаб (S/M/L/XL) и пропускает ненужные шаги.
> Интегрирует 15 скиллов из [agentic-qe](https://github.com/proffesor-for-testing/agentic-qe)
> для shift-left quality engineering на каждом этапе.

## When To Activate

Trigger on:
- "реализуй фичу [описание]"
- "implement feature [description]"
- "добавь функциональность [описание]"
- "feature [описание]"
- `/feature-adr [описание или путь к issue]`
- `/feature-adr --full-qe [описание]` (полная интеграция с agentic-qe)
- `/feature-adr --full-qe-extended [описание]` (полная + доп. скиллы)

## Architecture

```
.claude/skills/feature-adr/
├── SKILL.md                           ← This file (orchestrator)
├── modules/
│   ├── 00-complexity-router.md        ← Step 0: S/M/L/XL classification
│   ├── 01-requirements.md             ← Step 1: Requirements gathering
│   ├── 02-research.md                 ← Step 2: Analogues & patterns research
│   ├── 03-adr.md                      ← Step 3: ADR + shift-left validation
│   ├── 03.5-ideation-swarm.md         ← Step 3.5: QCSD quality swarm (NEW)
│   ├── 04-ddd.md                      ← Step 4: Domain-Driven Design
│   ├── 05-architecture.md             ← Step 5: Technical architecture
│   ├── 06-implementation-plan.md      ← Step 6: SPARC-GOAP planning (enhanced)
│   ├── 07-code.md                     ← Step 7: Code generation
│   ├── 08-qe.md                       ← Step 8: QE + brutal-honesty review (enhanced)
│   └── 09-fleet-qe.md                 ← Step 9: Fleet QE assessment (NEW)
├── references/
│   ├── complexity-matrix.md           ← S/M/L/XL criteria & step activation
│   ├── adr-template.md                ← ADR document template
│   ├── c4-template.md                 ← C4 diagram templates
│   ├── qe-checklist.md                ← Quality engineering checklist
│   └── agentic-qe/                    ← Integrated agentic-qe skills (15 total)
│       ├── README.md                  ← Skill mapping, modes, install guide
│       ├── shift-left-testing.md      ← Step 3: ADR testability (Tier 3)
│       ├── qcsd-ideation-swarm.md     ← Step 3.5: Quality swarm (Tier 3)
│       ├── code-goal-planner.md       ← Step 6: SPARC-GOAP agent
│       ├── brutal-honesty-review.md   ← Step 8: Code review (Tier 2)
│       ├── qe-requirements-validation.md  ← Step 9: Traceability (Tier 3)
│       ├── risk-based-testing.md      ← Step 9: Risk scoring (Tier 3)
│       ├── enterprise-integration-testing.md ← Step 9: Integration (Tier 3, condensed)
│       ├── regression-testing.md      ← Step 9: Regression (Tier 3)
│       ├── qe-coverage-analysis.md    ← Step 9: Coverage (Tier 3)
│       ├── chaos-engineering-resilience.md ← Extended: Chaos (Tier 3)
│       ├── security-testing.md        ← Extended: OWASP (Tier 3)
│       ├── performance-testing.md     ← Extended: Load/stress (Tier 3)
│       ├── mutation-testing.md        ← Extended: Mutation (Tier 3)
│       ├── tdd-london-chicago.md      ← Extended: TDD schools (Tier 3)
│       └── qcsd-production-swarm.md   ← Extended: Prod health (Tier 3, condensed)
└── examples/
    └── sample-feature-output.md       ← Example output for M-tier feature
```

## Pipeline Overview

```
Step 0        Step 1          Step 2        Step 3          Step 3.5
ROUTER   →  REQUIREMENTS  →  RESEARCH  →   ADR         →  QCSD SWARM
 (all)        (all)          (L/XL)      + shift-left      (M+)
                                           (M+)             │
                                            │     Step 4    │
                                            └──→  DDD   ←──┘
                                                 (L/XL)
                                                   │
                                                Step 5
                                             ARCHITECTURE
                                                 (M+)
                                                   │
                                                Step 6
                                           SPARC-GOAP PLAN
                                                (all)
                                                   │
                                                Step 7
                                                 CODE
                                                (all)
                                                   │
                                                Step 8
                                           QE + BRUTAL REVIEW
                                              + GAP LOOP
                                                (all)
                                                   │
                                                Step 9
                                             FLEET QE
                                              (L/XL)
```

## Complexity Tiers

| Tier | Scope | Steps | Example |
|------|-------|-------|---------|
| **S** | 1-3 files, single domain | 0→1→6→7→8 | Bug fix, config change, small util |
| **M** | 4-10 files, 1-2 domains | 0→1→3→3.5→5(light)→6→7→8 | New API endpoint, UI component |
| **L** | 11-30 files, 2-4 domains | 0→1→2→3→3.5→4→5→6→7→8→9 | New module, integration |
| **XL** | 30+ files, cross-cutting | Full DAG with parallelism + fleet QE | New subsystem, major refactor |

See `references/complexity-matrix.md` for detailed classification criteria.

## Step Activation Matrix

```
Step                         | S | M | L | XL | Model  | Agentic QE Skill       |
─────────────────────────────────────────────────────────────────────────────────
0  Complexity Router         | ✓ | ✓ | ✓ | ✓  | haiku  | —                      |
1  Requirements              | ✓ | ✓ | ✓ | ✓  | sonnet | —                      |
2  Research                  | - | - | ✓ | ✓  | sonnet | —                      |
3  ADR + Shift-Left          | - | ✓ | ✓ | ✓  | opus   | shift-left-testing     |
3.5 QCSD Ideation Swarm     | - | ✓ | ✓ | ✓  | sonnet | qcsd-ideation-swarm    |
4  DDD                       | - | - | ✓ | ✓  | opus   | —                      |
5  Architecture              | - | ✓ | ✓ | ✓  | opus   | —                      |
6  SPARC-GOAP Impl Plan     | ✓ | ✓ | ✓ | ✓  | sonnet | code-goal-planner      |
7  Code                      | ✓ | ✓ | ✓ | ✓  | opus   | —                      |
8  QE + Brutal Honesty       | ✓ | ✓ | ✓ | ✓  | sonnet | brutal-honesty-review  |
9  Fleet QE Assessment       | - | - | ✓ | ✓  | sonnet | qe-req-val + risk-based + integration + regression + coverage |
```

## DAG Dependencies

Steps are NOT purely linear. The DAG defines what can run in parallel:

```
Group 1 (sequential): Step 0 → Step 1
Group 2 (parallel):   Step 2 ‖ Step 3  (after Step 1)
Group 3 (sequential): Step 3.5 (after Step 3, consumes shift-left output)
Group 4 (parallel):   Step 4 ‖ Step 5  (after Steps 2+3.5)
Group 5 (sequential): Step 6 (after Step 5, uses QCSD findings)
Group 6 (sequential): Step 7 (after Step 6)
Group 7 (sequential): Step 8 (after Step 7, brutal-honesty + gap loop)
Group 8 (sequential): Step 9 (after Step 8, L/XL only — fleet assessment)
```

For tiers S/M, the DAG collapses to a linear sequence (no parallelism needed).

## Agent Swarm Strategy

### Per-Step Parallelism

| Step | Agents | Tasks |
|------|--------|-------|
| Step 2 | 2 parallel (sonnet) | Codebase patterns ‖ External analogues |
| Step 3+4 | 2 parallel (opus) | ADR drafting ‖ DDD modeling |
| Step 3.5 | 3-9 parallel (sonnet) | Core: QC + Risk + Testability ‖ Conditional: A11y + Security + UX |
| Step 7 | N parallel (opus) | One agent per independent module/file |
| Step 8 | 3 parallel (sonnet) | Linus reviewer ‖ Security reviewer ‖ Ramsay reviewer |
| Step 9 | 4 parallel (sonnet) | Traceability ‖ Risk ‖ Integration ‖ Regression |

### Cross-Step Parallelism

For L/XL: Steps 2 and 3 run in parallel after Step 1 completes.
For L/XL: Steps 4 and 5 can overlap if Step 3.5 finishes before Step 2.

## Execution Protocol

### 1. Load Governance
```
Read: .claude/shards/feature-adr.shard.md
```

### 2. Run Step 0 (Complexity Router)
```
Read: modules/00-complexity-router.md
Read: references/complexity-matrix.md
→ Output: {COMPLEXITY_TIER}, {ACTIVE_STEPS}, {TIME_BUDGET}
→ Checkpoint 0
```

### 3. Execute Active Steps per DAG
For each active step:
```
Read: modules/<step>.md
Execute protocol
→ Create artifacts in features/<feature-slug>/
→ Checkpoint N
```

### 4. Final Verification
After Step 8 (or Step 9 for L/XL) completes, verify:
- All mandatory artifacts exist per tier
- QE checks passed
- ADR decisions are traceable to code (L/XL)
- QCSD quality risks are mitigated (M+)
- Gap detection loop closed with zero remaining gaps

## Cross-Phase Variables

| Variable | Set In | Used In | Type |
|----------|--------|---------|------|
| `{COMPLEXITY_TIER}` | Step 0 | All steps | S/M/L/XL |
| `{ACTIVE_STEPS}` | Step 0 | Orchestrator | list[int] |
| `{TIME_BUDGET}` | Step 0 | All steps | dict |
| `{REQUIREMENTS}` | Step 1 | Steps 2-9 | structured |
| `{RESEARCH_FINDINGS}` | Step 2 | Steps 3-5 | structured |
| `{ADR_DECISIONS}` | Step 3 | Steps 3.5-7 | list[ADR] |
| `{IDEATION_VERDICT}` | Step 3.5 | Steps 6-9 | GO/CONDITIONAL/NO-GO |
| `{QUALITY_RISKS}` | Step 3.5 | Steps 6-9 | list[risk] |
| `{DOMAIN_MODEL}` | Step 4 | Steps 5-7 | structured |
| `{ARCHITECTURE}` | Step 5 | Steps 6-7 | structured |
| `{IMPL_PLAN}` | Step 6 | Step 7 | list[task] (SPARC-enhanced) |
| `{CODE_CHANGES}` | Step 7 | Steps 8-9 | list[file] |
| `{QE_RESULTS}` | Step 8 | Step 9 | structured |
| `{FLEET_QE_VERDICT}` | Step 9 | Final report | COMPLETE/NEEDS_REMEDIATION |

## Output Directory Structure

All artifacts are created in `features/<feature-slug>/`:

```
features/<feature-slug>/
├── 00_complexity_assessment.md        ← Always
├── 01_requirements.md                 ← Always
├── 02_research.md                     ← L/XL only
├── 03_adr/                            ← M+ only
│   ├── 001-<decision>.md
│   └── ...
├── 03.5_ideation_report.md            ← M+ only (QCSD swarm output)
├── 04_domain_model.md                 ← L/XL only
├── 05_architecture.md                 ← M+ only
├── 06_implementation_plan.md          ← Always (SPARC-GOAP enhanced)
├── 07_code_changes/                   ← Always (actual code in repo)
│   └── change_manifest.md             ← List of modified files
├── 08_qe_report.md                    ← Always (brutal-honesty review)
├── 09_fleet_qe_assessment.md          ← L/XL only (fleet assessment)
├── diagrams/                          ← M+ only
│   ├── architecture-c4.mermaid
│   ├── sequence-*.mermaid
│   └── domain-model.mermaid
└── README.md                          ← Always (auto-generated summary)
```

## Promise Tags

| Step | Promise Tag |
|------|-------------|
| Step 0 | `<promise>FEATURE_ADR_ROUTED</promise>` |
| Step 1 | `<promise>FEATURE_ADR_REQUIREMENTS_GATHERED</promise>` |
| Steps 2-3 | `<promise>FEATURE_ADR_DESIGNED</promise>` |
| Step 3.5 | `<promise>FEATURE_ADR_QUALITY_ASSESSED</promise>` |
| Steps 4-5 | `<promise>FEATURE_ADR_ARCHITECTED</promise>` |
| Step 6 | `<promise>FEATURE_ADR_PLANNED</promise>` |
| Step 7 | `<promise>FEATURE_ADR_IMPLEMENTED</promise>` |
| Step 8 | `<promise>FEATURE_ADR_VERIFIED</promise>` |
| Step 9 | `<promise>FEATURE_ADR_FLEET_VERIFIED</promise>` |

## External Skills (loaded as needed)

### Keysarium Skills (internal)

| Skill | Used In | Purpose |
|-------|---------|---------|
| explore | Step 1 | Requirements clarification |
| problem-solver-enhanced | Step 3 | Trade-off analysis for ADR decisions |
| frontend-design | Step 7 | UI implementation (if feature has UI) |

### Agentic QE Skills (from [proffesor-for-testing/agentic-qe](https://github.com/proffesor-for-testing/agentic-qe))

#### Core Pipeline Skills (loaded by default)

| Skill | Used In | Purpose | Trust Tier |
|-------|---------|---------|------------|
| shift-left-testing | Step 3 | ADR testability validation (Level 4: Risk Analysis in Design) | 3 (Verified) |
| qcsd-ideation-swarm | Step 3.5 | HTSM quality criteria + SFDIPOT risk + testability scoring | 3 (Verified) |
| code-goal-planner | Step 6 | SPARC-GOAP milestone decomposition and goal state planning | — (Agent) |
| brutal-honesty-review | Step 8 | Linus/Ramsay/Bach modes for code + test review | 2 (Validated) |
| qe-requirements-validation | Step 9 | Traceability matrix, SMART validation, orphan test detection | 3 (Verified) |
| risk-based-testing | Step 9 | Probability×Impact 5×5 scoring, 4-tier test effort allocation | 3 (Verified) |
| enterprise-integration-testing | Step 9 | Cross-module contract testing, E2E flow validation | 3 (Verified) |
| regression-testing | Step 9 | Change-based test selection, impact analysis, regression pyramid | 3 (Verified) |
| qe-coverage-analysis | Step 9 | Risk-weighted coverage scoring, differential coverage analysis | 3 (Verified) |

#### Extended Skills (available in Direct Extended Mode via `--full-qe-extended`)

| Skill | Applicable Steps | Purpose | Trust Tier |
|-------|-----------------|---------|------------|
| chaos-engineering-resilience | Step 9 | Fault injection, blast radius progression, steady state verification | 3 (Verified) |
| security-testing | Step 8, 9 | OWASP Top 10, access control, injection, cryptographic failures | 3 (Verified) |
| performance-testing | Step 8, 9 | Load/stress/spike/endurance testing, SLO definition, k6 integration | 3 (Verified) |
| mutation-testing | Step 8 | Mutation score analysis, surviving mutant diagnosis, Stryker integration | 3 (Verified) |
| tdd-london-chicago | Step 7 | TDD school selection (London mocks vs Chicago state), mixed approach | 3 (Verified) |
| qcsd-production-swarm | Step 9+ | 12-agent post-release health assessment, DORA metrics, feedback loops | 3 (Verified) |

Skills are stored in `references/agentic-qe/` and loaded on demand by each module.

## Direct Integration Mode (agentic-qe flags)

Two flags for integrating with the full agentic-qe package:

| Flag | Mode | What it does |
|------|------|-------------|
| (none) | Reference | Condensed copies of 9 core skills, no install needed |
| `--full-qe` | Direct | Full agentic-qe protocols for existing 9 core skills |
| `--full-qe-extended` | Direct Extended | Full protocols + 6 additional skills (chaos, security, performance, mutation, TDD, production-swarm) |

### Installation

```bash
# Install agentic-qe globally
npm install -g agentic-qe

# Initialize in your project (auto-detects tech stack, configures MCP)
cd your-project && aqe init --auto
```

### Activation

```
/feature-adr [описание фичи]                      # Reference Mode (default)
/feature-adr --full-qe [описание фичи]             # Direct Mode: full protocols, same skills
/feature-adr --full-qe-extended [описание фичи]     # Direct Extended: full protocols + extra skills
```

Step 0 (Complexity Router) checks:
1. Is `--full-qe` or `--full-qe-extended` flag present?
2. Is agentic-qe installed? (check `which aqe` or `node_modules/agentic-qe/`)
3. If flag + installed → set `{AGENTIC_QE_MODE}` = `direct` or `direct-extended`
4. If flag present but not installed → WARN and fall back to reference mode

### What changes with `--full-qe`

Full agentic-qe protocols for the same 9 core skills. No new agents, just deeper methodology.

| Step | Reference Mode (default) | Direct Mode (`--full-qe`) |
|------|--------------------------|---------------------------|
| Step 3 | Condensed shift-left protocol | Full Level 1-4 shift-left with BDD generators |
| Step 3.5 | 3 core + flag-based conditionals | Full QCSD swarm with all 9 agents + DDD mapping |
| Step 6 | SPARC-GOAP goal state analysis | Full agent with milestone tracking + success metrics |
| Step 7 | Standard code generation | Standard code generation (no change) |
| Step 8 | Brutal-honesty 3-mode review | Full calibration levels (1-3) + evidence protocol |
| Step 9 | 4 agents with condensed protocols | 4 agents with full agentic-qe protocols |

### What `--full-qe-extended` adds on top

Everything from `--full-qe` plus 6 additional skills and up to 3 extra agents in Step 9.

| Skill | When Activated | Condition |
|-------|----------------|-----------|
| tdd-london-chicago | Step 7 | Always — guides test-first coding |
| mutation-testing | Step 8 | If test suite exists — validates test effectiveness |
| security-testing | Step 8, 9 | If `HAS_AUTH` or `HAS_EXTERNAL_API` flags set |
| performance-testing | Step 8, 9 | If `HAS_PERFORMANCE_SLA` flag set |
| chaos-engineering-resilience | Step 9 | If `HAS_INFRASTRUCTURE_CHANGE` flag set |
| qcsd-production-swarm | Post-Step 9 | Advisory — informs future pipeline runs via feedback loops |

### When to use

| Scenario | Mode |
|----------|------|
| S/M tier features | Reference (always) |
| L tier, standard QE | Reference or `--full-qe` |
| L tier, high QE maturity | `--full-qe` |
| XL tier features | `--full-qe` recommended |
| XL + security-critical (banking, ФЗ-152) | `--full-qe-extended` recommended |
| XL + regulatory compliance | `--full-qe-extended` recommended |
| Quick iteration / prototyping | Reference (faster) |

## npm Package Init Flags

```bash
# Core only (feature-adr pipeline)
npx @dzhechkov/skills-feature-adr init

# + Reward learning (memory protocol, reward tracker, learning rules)
npx @dzhechkov/skills-feature-adr init --with-learning

# + Knowledge extractor (/harvest command, 5 agents, 7 categories, 8 gates)
npx @dzhechkov/skills-feature-adr init --knowledge-extractor

# All features
npx @dzhechkov/skills-feature-adr init --with-learning --knowledge-extractor
```

| Flag | Installs | Purpose |
|------|----------|---------|
| (none) | Core skill + command + rules + shard | Feature development pipeline |
| `--with-learning` | + `lib/memory-protocol.md`, `lib/reward-tracker.md`, `.claude/rules/reward-learning.md` | Pipeline learns from checkpoint feedback |
| `--knowledge-extractor` | + `.claude/skills/knowledge-extractor/`, `.claude/commands/harvest.md` | Extract reusable patterns after feature completion |

> **Note:** If `@dzhechkov/keysarium` is already installed, these flags are not needed — keysarium includes all learning and extraction capabilities.

## Anti-Patterns

| Anti-Pattern | Detection | Fix |
|-------------|-----------|-----|
| Skip complexity assessment | Jump straight to coding | BLOCK — always run Step 0 |
| Over-engineer S-tier | Run full pipeline for a config change | Router should classify as S |
| Under-engineer XL-tier | Skip ADR/DDD for major refactor | Router should classify as XL |
| ADR without alternatives | Only one option considered | Require ≥2 alternatives per ADR |
| Architecture without ADR | Diagram before decisions | Step 5 requires Step 3 output |
| Code without plan | Start coding without Step 6 | BLOCK — plan is mandatory |
| No QE | Ship without testing | BLOCK — Step 8 is mandatory |
| Skip QCSD ideation | No Step 3.5 for M+ | BLOCK — quality assessment mandatory |
| Ignore NO-GO verdict | Proceed despite Step 3.5 NO-GO | BLOCK — rework required |
| Skip gap loop | No gap detection in Step 8 | Missing requirements coverage check |
| Skip fleet QE for L/XL | No Step 9 for large features | BLOCK — fleet assessment mandatory |

## Checkpoint Format

```
═══════════════════════════════════════════════════════
⏸️ STEP N: [Step Name] Complete
<promise>[PROMISE_TAG]</promise>
Tier: {COMPLEXITY_TIER} | Active Steps: {ACTIVE_STEPS}

[2-3 line summary]
Artifacts: [list] ✅

• "ок" — next step
• "углуби [section]" — elaborate
• "[feedback]" — adjust
═══════════════════════════════════════════════════════
```
