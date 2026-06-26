# dz skill bundles

Portable, **self-contained** skill bundles produced by [`dz bundle`](https://www.npmjs.com/package/@dzhechkov/harness-cli)
from the [DZ Harness Hub](https://github.com/djd1m/dz-harness-hub) monorepo.

Each bundle is a minimal [agentskills.io](https://agentskills.io) skill tree — `SKILL.md`
(YAML/Markdown frontmatter + instructions, **verbatim**) plus its `references/`, `scripts/`,
`assets/`, `modules/`, `examples/`, `templates/` folders. dz-internal QA artifacts
(`schemas/`, `evals/`, `sources.json`) are stripped, and **no `manifest.json` is written** —
the consuming app builds its own file-manifest on load.

These bundles are meant to be loaded by a generic runtime — e.g. a
[**LangGraph**](https://github.com/langchain-ai/langgraph)
([docs](https://langchain-ai.github.io/langgraph/)) app — that points its own `load_skill` at a
skill directory. A skill provides *instructions + resources* that an agent pulls into its
prompt; it does **not** describe graph nodes, and ships no Python graph scaffold.

> Generated: 2026-06-26 · `dz bundle` from `@dzhechkov/harness-cli`

## Layout

```
<bundle>/
  skills/
    <skill-id>/
      SKILL.md
      references/  scripts/  modules/  examples/  templates/   (as applicable)
```

Each bundle directory is independent and self-contained: point your loader at
`<bundle>/skills/` and it will find every skill it needs in-tree.

## Bundles

| Bundle | Skills | Files | Self-contained | Notes |
|--------|-------:|------:|:--------------:|-------|
| [`analyst-manual-full`](./analyst-manual-full) | 4 | 11 | ✅ | composite + 3 sub-skills |
| [`news-digest`](./news-digest) | 2 | 12 | ✅ | digest + mandatory goap backend |
| [`news-monitor`](./news-monitor) | 2 | 9 | ✅ | monitor + mandatory goap backend |
| [`keysarium`](./keysarium) | 12 | 109 | ⚠️ | full package; 1 external host skill (`docx`, optional) |

### `analyst-manual-full`
A 3-phase composite analyst skill. It loads three sibling sub-skills by path, so the bundle
includes all of them — it would not work standalone otherwise:
- `analyst-manual-full` (orchestrator)
- `explore` (Phase: scoping / questioning)
- `goap-research-ed25519` (Phase: verified research)
- `problem-solver-enhanced` (Phase: synthesis)

### `news-digest`
The news-digest skill with its **mandatory** verified-research backend
`goap-research-ed25519` (GOAP planning + ed25519 source signatures). All source-gathering in
Phase 1 routes through goap; the digest is signed.

### `news-monitor`
The news-monitor skill with the same mandatory `goap-research-ed25519` backend for scan +
delta verification.

### `keysarium`
The **full `keysarium` package** — all **12 skills** it ships, including the pipeline
orchestrators:
- `reverse-engineering-unicorn` (the master orchestrator — company → launch playbook + CJM)
- `feature-adr` (11-step feature pipeline) · `ai-factory-mapper` (Cloud.ru AI Factory mapping)
- `explore` · `goap-research-ed25519` · `problem-solver-enhanced` · `frontend-design`
- `analyst-manual-full` · `knowledge-extractor` · `presentation-storyteller`
- `edu-site-generator` · `transcript-site-generator`

> This is the full package set, **not** the `keysarium` CLI preset (which curates only 9 and
> omits `reverse-engineering-unicorn`, `frontend-design`, `ai-factory-mapper`). For a portable
> repo we bundle everything the package ships.

**Source migrations applied** so the package bundles cleanly:
- `edu-site-generator` + `transcript-site-generator` — added YAML frontmatter (legacy
  Markdown-header format the agentskills.io loader rejected).
- `presentation-storyteller` + `ai-factory-mapper` — rewrote in-bundle cross-skill references
  from absolute `/mnt/skills/user/<id>` paths to bundle-relative `.claude/skills/<id>`.

**Expected (honest) warnings** — these are informative, not defects:
- `ai-factory-mapper` depends on the Anthropic **public `docx` skill** (`/mnt/skills/public/docx`)
  for its *optional* final DOCX-export step. That's a **host-provided** skill, not part of
  keysarium — provide your own DOCX generation, or skip that step. Everything else is
  self-contained.
- `ai-factory-mapper` and `reverse-engineering-unicorn` use `view()` to load their in-bundle
  sibling skills — expose a file-read tool in your runtime so `view(".claude/skills/<id>/…")`
  resolves against the bundle.
- `reverse-engineering-unicorn` also *names* a few optional DEEP/VERIFIED-mode skills that are
  not in this package (`brutal-honesty-review`, `idea2prd-manual`, `md2pptx`, plain
  `goap-research`). They're mentioned by name only (no hard path coupling) and the skill works
  without them.

## Use with LangGraph

A skill is *instructions + resources*, not a graph. Your [LangGraph](https://langchain-ai.github.io/langgraph/)
app reads a bundle's `SKILL.md` and injects it into the model's prompt; the graph wiring stays
yours. Minimal `load_skill` + node:

```python
from pathlib import Path
from langgraph.graph import StateGraph, MessagesState
from langchain_anthropic import ChatAnthropic

BUNDLES = Path("bundles")  # this repo, checked out locally

def load_skill(skill_id: str, bundle: str) -> str:
    """Return a bundled skill's SKILL.md (instructions) for prompt injection.
    references/ and scripts/ sit alongside it and are read on demand when the
    instructions say to (expose a file-read tool for that)."""
    return (BUNDLES / bundle / "skills" / skill_id / "SKILL.md").read_text("utf-8")

llm = ChatAnthropic(model="claude-opus-4-8")

def analyst_node(state: MessagesState):
    system = {"role": "system", "content": load_skill("analyst-manual-full",
                                                       bundle="analyst-manual-full")}
    return {"messages": [llm.invoke([system, *state["messages"]])]}

graph = StateGraph(MessagesState)
graph.add_node("analyst", analyst_node)
graph.set_entry_point("analyst")
app = graph.compile()
```

Notes for a faithful loader:
- **Resolve `view(".claude/skills/<id>/…")` against the bundle root**, not a host path — those
  refs point to sibling skills inside `<bundle>/skills/`. Expose a file-read tool so the model
  can pull a `references/*.md` or sibling `SKILL.md` when the instructions ask.
- **Build your own file-manifest on load** (list `<bundle>/skills/`); the bundles ship no
  `manifest.json` on purpose.
- **A few skills name external/host capabilities** (e.g. `keysarium`'s `ai-factory-mapper`
  wants the Anthropic public `docx` skill for its optional DOCX export). Provide your own, or
  skip that step — see the keysarium notes above.

## How these were generated

```bash
# from the dz-harness-hub monorepo
dz bundle --select analyst-manual-full,explore,goap-research-ed25519,problem-solver-enhanced \
  --skills-dir <skills> --out ./analyst-manual-full

dz bundle --select news-digest,goap-research-ed25519  --skills-dir <skills> --out ./news-digest
dz bundle --select news-monitor,goap-research-ed25519 --skills-dir <skills> --out ./news-monitor

# keysarium: the FULL package (all 12 skills), not the 9-skill CLI preset
dz bundle --select ai-factory-mapper,analyst-manual-full,edu-site-generator,explore,feature-adr,frontend-design,goap-research-ed25519,knowledge-extractor,presentation-storyteller,problem-solver-enhanced,reverse-engineering-unicorn,transcript-site-generator \
  --skills-dir <skills> --out ./keysarium
```

Regenerate / extend with the published CLI:

```bash
npm i -g @dzhechkov/harness-cli   # >= 0.3.113
dz bundle --select <ids>  --out ./my-bundle
dz bundle --preset <name> --out ./my-bundle
```

## License

The skills retain the licenses of their source packages in the DZ Harness Hub monorepo.
