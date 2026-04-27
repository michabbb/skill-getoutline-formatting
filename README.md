# skill-getoutline-formatting

A Claude Code skill for authoring polished, human-readable pages in a [getOutline](https://www.getoutline.com) wiki via MCP tools (e.g. `mcp__<your-wiki>__create_document` / `update_document` / `fetch`).

Outline supports markdown but extends it in several non-obvious ways. This skill teaches Claude to use those extensions properly — coloured callouts, collapsible toggles, mermaid diagrams in dark mode, multi-colour highlights, LaTeX, `diff` code blocks, file attachments, embeds — and to write pages that a human actually wants to read.

## What this skill makes Claude do differently

Without it, Claude tends to dump a long wall of prose into a wiki document. With it, Claude:

- Leads with a one-sentence summary in an `:::info` callout.
- Splits findings into numbered evidence sections with right-aligned tables.
- Hides optional detail in `++++` collapsible toggles.
- Uses mermaid diagrams (with the correct `mermaidjs` language tag and dark-mode init) for flows.
- Picks the right `editMode` for `update_document` so existing highlights and comments are preserved.
- Keeps sentences short.

## Installation

### As a Claude Code plugin

Clone or symlink this repo into your Claude Code plugins directory:

```bash
git clone https://github.com/michabbb/skill-getoutline-formatting.git ~/.claude/plugins/skill-getoutline-formatting
```

Claude Code discovers skills in any `skills/<skill-name>/SKILL.md` under registered plugin directories.

### Manually (single-skill install)

Copy `skills/getoutline-formatting/` into your Claude `~/.claude/skills/` directory:

```bash
cp -r skills/getoutline-formatting ~/.claude/skills/
```

## What's inside

```
skills/getoutline-formatting/
├── SKILL.md                          # Main skill — when to trigger, what to do
└── references/
    ├── outline-syntax.md             # Merged extract of the official getOutline docs
    ├── mermaid-cheatsheet.md         # Copy-paste dark-mode mermaid recipes (all 9 supported types)
    └── page-templates.md             # Three full skeletons: investigation, reference doc, log entry
```

| File | Purpose |
|---|---|
| `SKILL.md` | The main entry point. Loaded into Claude's context whenever the skill triggers. ~380 lines. |
| `references/outline-syntax.md` | Lightly merged copy of the official `docs.getoutline.com` formatting / blocks / code / embeds / diagramming pages. Used for offline lookup of exact syntax. |
| `references/mermaid-cheatsheet.md` | Ready-to-paste mermaid blocks for flowcharts, sequence diagrams, gantt, class, ER, state, user journey, quadrant, XY chart, and git graph — all pre-configured with `theme: dark` and the `subGraphTitleMargin` fix. |
| `references/page-templates.md` | Three full Outline page skeletons you can copy-paste: investigation report, reference / how-it-works doc, daily log entry. |

## When the skill triggers

Per the description in `SKILL.md`, the skill activates when:

- The user asks to write into a wiki, "schreib das ins wiki", "logbuch", "outline", or names an Outline document ID / URL slug.
- Claude is about to call any `mcp__*__update_document`, `mcp__*__create_document`, or `mcp__*__fetch` tool that targets an Outline wiki.
- The user complains a wiki page is "zu lang", "schwer zu lesen", or asks for it to be made "menschenfreundlicher".
- Claude is about to write more than ~5 paragraphs of markdown into Outline.

Pure reading of a wiki page does not need this skill — it kicks in for *writing*.

## Outline-specific things this skill remembers so you don't have to

A short tour of what the skill teaches Claude — the full reference is in `SKILL.md`:

| Feature | Plain markdown | Outline |
|---|---|---|
| Mermaid fence | ```` ```mermaid ```` | **```` ```mermaidjs ````** |
| Callouts | none (or `> [!NOTE]`) | `:::info` / `:::tip` / `:::warning` / `:::success` ... `:::` |
| Collapsibles | `<details><summary>` HTML | `++++` ... `++++` |
| Underline | none / HTML `<u>` | `__text__` |
| Highlight | none | `==text==` (multi-colour) |
| Inline math | none | `$x = y$` (LaTeX) |
| Math block | none | `$$ ... $$` (KaTeX), or `/math` |
| `diff` code block | rendered as plain code | red/green coloured `+` / `-` lines |
| File attachment card | bare link | `[name.ext bytes](url)` |
| External embeds | bare link | URL on its own line auto-embeds (Figma, YouTube, GitHub, …) |
| Line breaks in mermaid labels | `\n` | `<br/>` |

## Mermaid in Outline — the dark-mode init

Every diagram should start with this:

````text
```mermaidjs
%%{init: {'theme': 'dark'}}%%
flowchart TD
    A[Source data<br/>raw input] --> B[Filter stage<br/>only matching rows]
    B --> C[Output stage<br/>final result]
```
````

If the diagram uses subgraphs, extend the init so the subgraph title doesn't collide with its child nodes:

```text
%%{init: {'theme': 'dark', 'flowchart': {'subGraphTitleMargin': {'top': 10, 'bottom': 15}}}}%%
```

## Editing existing pages without breaking other people's work

Outline `update_document` MCP calls expose four `editMode` values. The skill teaches Claude to pick deliberately:

| editMode | When to use |
|---|---|
| `patch` | Default for edits inside an existing document. Preserves highlights, comments, table column widths. |
| `replace` | Whole-document rewrite. Use only when you wrote it this session or the user explicitly asks for a full rewrite. |
| `append` | Add a new section at the bottom (e.g. a daily log entry). Safe. |
| `prepend` | Add to the very top (rare). |

The trap to avoid: never `replace` a document you didn't author yourself, or you'll silently destroy other people's rich formatting that markdown can't represent.

## Credits

The Outline-specific syntax and feature list in `references/outline-syntax.md` is derived from the official getOutline guide at https://docs.getoutline.com.

## License

MIT
