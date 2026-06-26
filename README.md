# dz skill bundles

Portable, **self-contained** skill bundles produced by [`dz bundle`](https://www.npmjs.com/package/@dzhechkov/harness-cli)
from the [DZ Harness Hub](https://github.com/djd1m/dz-harness-hub) monorepo.

Each bundle is a minimal [agentskills.io](https://agentskills.io) skill tree — `SKILL.md`
(YAML/Markdown frontmatter + instructions, **verbatim**) plus its `references/`, `scripts/`,
`assets/`, `modules/`, `examples/`, `templates/` folders. dz-internal QA artifacts
(`schemas/`, `evals/`, `sources.json`) are stripped, and **no `manifest.json` is written** —
the consuming app builds its own file-manifest on load.

These bundles are meant to be loaded by a generic runtime (e.g. a **LangGraph** app) that
points its `load_skill` at a skill directory. A skill provides *instructions + resources* that
an agent pulls into its prompt; it does **not** describe graph nodes, and ships no Python graph
scaffold.

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
| [`keysarium`](./keysarium) | 7 | 60 | ⚠️ | 7/9 of preset; 2 skills excluded (see note) |

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
The `keysarium` research preset (9 skills). **7 are bundled**; two are intentionally excluded:

| Excluded skill | Reason |
|----------------|--------|
| `edu-site-generator` | legacy `SKILL.md` has **no YAML frontmatter** — fails the agentskills.io loader |
| `transcript-site-generator` | same — legacy Markdown-header format, predates the schema |

These two need a frontmatter migration before they can be bundled. The 7 valid skills are
fully self-contained.

One advisory warning is expected for keysarium:
- `presentation-storyteller` references absolute `/mnt/skills/...` paths — rewrite these to
  bundle-relative ids for your runtime.

## How these were generated

```bash
# from the dz-harness-hub monorepo
dz bundle --select analyst-manual-full,explore,goap-research-ed25519,problem-solver-enhanced \
  --skills-dir <skills> --out ./analyst-manual-full

dz bundle --select news-digest,goap-research-ed25519  --skills-dir <skills> --out ./news-digest
dz bundle --select news-monitor,goap-research-ed25519 --skills-dir <skills> --out ./news-monitor
dz bundle --preset keysarium                          --skills-dir <skills> --out ./keysarium
```

Regenerate / extend with the published CLI:

```bash
npm i -g @dzhechkov/harness-cli   # >= 0.3.113
dz bundle --select <ids>  --out ./my-bundle
dz bundle --preset <name> --out ./my-bundle
```

## License

The skills retain the licenses of their source packages in the DZ Harness Hub monorepo.
