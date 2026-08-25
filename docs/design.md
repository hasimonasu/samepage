# samepage — design notes

> *Get on the same page with your AI — literally.*

samepage is a removable review layer for static HTML. A human opens the page,
anchors comments to text ranges / elements / insertion points / SVG diagram
nodes / the whole document, and exports structured JSON. An AI agent applies
the feedback to the **source** (not the HTML), replies with a responses JSON,
and can plant question pins for the human to answer on the same page. When
everything is resolved, `--finalize` strips the layer and produces the
publishable HTML.

## Repository layout

```
samepage/                 Python package (also runnable as a script)
  cli.py                  entry point: python3 samepage/cli.py <html> [options]
  assets.py               builds the injected block from assets/
  assets/
    samepage.js           review layer runtime (vanilla JS, no deps)
    samepage.css          review layer styles
    panel.html            panel / popup markup injected into <body>
tests/                    pytest (+ playwright browser tests)
docs/                     design notes, this file
SKILL.md                  Claude Code skill definition (repo root = skill dir)
README.md / README.ja.md
```

Clone the repository into `~/.claude/skills/samepage` and it works as a skill
as-is: `python3 ${CLAUDE_SKILL_DIR}/samepage/cli.py <html> ...`.

## Naming (fixed — do not deviate)

| Concept | Name |
|---|---|
| Project / CLI / skill name | `samepage` |
| HTML marker comments (idempotent block) | `<!-- samepage:begin -->` … `<!-- samepage:end -->` |
| Unit attributes (set by CLI) | `data-sp-unit`, `data-sp-label`, `data-sp-index` |
| Diagram node attributes (set by generator) | `data-sp-node`, `data-sp-label` |
| Discussion block (stripped by finalize) | `data-sp-discussion` |
| Runtime badge/action attrs | `data-sp-badge`, `data-sp-action` |
| Runtime decoration attrs (stripped by finalize) | `data-sp-q-id`, `data-sp-cmt-id`, `data-sp-resp-id` |
| Config globals | `window.SAMEPAGE_CONFIG`, `window.SAMEPAGE_RESPONSES`, `window.SAMEPAGE_QUESTIONS` |
| localStorage key | `samepage:<doc>` (config field `storageKey`) |
| CSS class / id prefix | `sp-` (e.g. `sp-panel`, `sp-mark`, `sp-qpin`, `sp-node-halo`) |
| Export JSON `_skill` | `"samepage"` |
| Export JSON `_howto` / `_rules` | English text (rules table in SKILL.md is the source of truth) |
| Finalized output default | `<stem>.final.html` |

Keyboard shortcuts are unchanged: `a` add comment, `e` element pick,
`↑`/`↓` widen/narrow pick, `c` toggle panel, `j` copy JSON.

## JSON contracts

Unchanged from the behaviour described in SKILL.md §4 (export), §6 (responses),
§7 (questions). Field names (`targets[].kind` = `text-range` | `element` |
`insertion-point` | `diagram-node` | `document`, `action`, `status`, `answers`,
…) are kept verbatim so existing agent-side tooling keeps working; only the
`_skill` value and the `data-*` / global names change.

## Non-goals

- No Markdown → HTML conversion.
- No server. Everything runs from `file://`.
- The CLI never adds `data-sp-node` to SVG automatically (only the generator
  knows the semantic unit).
