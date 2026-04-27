---
name: getoutline-formatting
description: Use whenever the user writes, edits, or formats a getOutline wiki document — including any mcp__*__create_document / update_document / fetch call on an Outline wiki. Activate on "schreib das ins wiki", "logbuch eintrag", "outline", "wiki seite", a getOutline document ID or URL slug, or any request to make a wiki page scannable or human-friendly. Covers Outline-specific markdown that plain markdown lacks — `:::info`/`:::warning`/`:::tip`/`:::success` callouts, `++++` toggles, mermaid (MUST use ```mermaidjs, not ```mermaid) with dark-mode init, `==highlight==`, `__underline__`, LaTeX math, `diff` blocks, file-attachment cards, auto-embeds (Figma/YouTube/GitHub/Linear/Google). Also covers update_document editMode (patch/replace/append) to preserve existing highlights and comments.
---

# getOutline Wiki Formatting

This skill captures how to make a [getOutline](https://www.getoutline.com) wiki page look polished and read well for a human. Outline supports markdown but extends it in several non-obvious ways. Knowing those extensions — and when to use them — is the difference between a flat dump of text and a page someone actually wants to read.

## When to reach for this skill

Trigger this skill the moment any of these appear:

- The user says "schreib das ins wiki", "logbuch", "outline", "wiki seite", "befund ins wiki", or names an Outline document ID / URL slug.
- You are about to call any `mcp__*__update_document`, `mcp__*__create_document`, or `mcp__*__fetch` tool that targets an Outline wiki.
- The user complains that a wiki page is "zu lang", "schwer zu lesen", "keine struktur", or asks for it to be made "menschenfreundlicher".
- You are about to write more than ~5 paragraphs of markdown into Outline.

If you are *only* reading a wiki page, the skill is not needed. It kicks in for *writing*.

## The mental model

A good Outline page is built like a news article, not like a chat reply.

1. **Lead with the punchline.** A `:::info` callout right under the title with the one-sentence answer. The reader should be able to stop reading after the first screen and still know the conclusion.
2. **Then explain in layers.** Numbered evidence sections or named sections, each with one job. Tables for data, bullet lists for steps, prose only when prose is actually clearer.
3. **Hide the nice-to-haves.** Detail that not every reader needs goes into a `++++` collapsible toggle, not into the main flow.
4. **Visualise the flow.** When something has more than two stages or a branching decision, a mermaid diagram is faster to grok than three paragraphs of prose.
5. **Mark the danger.** Anything the reader could trip over (separate topics, side effects, tradeoffs) goes into a `:::warning` callout, not into a footnote nobody will read.

Short sentences win. If a sentence has more than one comma, try to split it.

## Outline-specific markdown — the things vanilla markdown can't do

These are the Outline extensions you actually need. Everything else is just normal markdown. The full per-feature reference is in `references/outline-syntax.md` (extracted from the official `docs.getoutline.com` guide); read it when you need exact syntax for something not covered here.

### 1. Callouts — the coloured info boxes

Use these for emphasis. They render as coloured panels with an icon.

```text
:::info
**Summary**

The investigation found no defect. The drop in volume is the expected result of an upstream filter change.
:::
```

Available types and when to use which:

| Type | Use for |
|---|---|
| `:::info` | The main takeaway, the summary, the "if you only read one thing" |
| `:::tip` | A helpful nudge, a useful fact, a "now the picture is complete" moment |
| `:::warning` | A caveat, a side effect, a separate topic deliberately not covered |
| `:::success` | A green-checkmark moment, e.g. "all values verified against the source of truth" |

Always put a bold title on the first line of the callout (`**Summary**`, `**Note**`, `**Heads up**`) — it makes the callout scannable.

### 2. Collapsible sections — toggles with `++++`

Outline's "details" widget. Use it whenever a section has detail that *some* readers want and *some* don't. The fence is exactly four `+` characters on a line by themselves, then a bold title, then content, then four `+` again to close.

```text
++++
**Edge cases and worked examples**

When the input value is exactly at the boundary, the rule still triggers because the comparison
is inclusive. When the input is one unit below the boundary, the rule is skipped — see the
verification table above.
++++
```

Common uses:

- Detailed example breakdowns under a summary table
- Edge-case lists ("input → output → reason")
- "Technical source" mappings under user-facing field names
- Long code snippets that prove a claim but interrupt the prose

If you find yourself writing "This is only relevant if …" — that paragraph belongs in a toggle.

### 3. Mermaid diagrams — `mermaidjs`, dark mode, line breaks

**Outline uses ```mermaidjs as the code-fence language, not ```mermaid.** This is an Outline-specific deviation from plain markdown. If you use ```mermaid, the diagram will not render in Outline. Always `mermaidjs`.

Always start the diagram with the dark-mode init directive so it matches the wiki's reading experience. If the diagram contains subgraphs, also set `subGraphTitleMargin` so the subgraph title does not collide with its child nodes.

**Minimal flowchart (no subgraphs):**

````text
```mermaidjs
%%{init: {'theme': 'dark'}}%%
flowchart TD
    A[Source data<br/>raw input] --> B[Filter stage<br/>only matching rows]
    B --> C[Output stage<br/>final result]
```
````

**With subgraphs (adds the margin fix):**

````text
```mermaidjs
%%{init: {'theme': 'dark', 'flowchart': {'subGraphTitleMargin': {'top': 10, 'bottom': 15}}}}%%
flowchart TD
    subgraph Ingest["Ingestion"]
        A[Source A] --> B[Normalize]
    end
    subgraph Process["Processing"]
        C[Validate] --> D[Aggregate]
    end
    Ingest --> Process
```
````

Rules for node labels:

- **Line breaks: always `<br/>`, never `\n`.** A literal `\n` will render as the two characters `\` and `n` inside the node.
- **Quoting:** if a label contains spaces or special characters, wrap it: `A["Drop with reason"]`.
- **Node IDs:** use uppercase letters (`A`, `B`, `C1`) — keeps the diagram readable when it grows.

If the user asks for a sequence diagram, state diagram, or class diagram, the same rules apply: use ```mermaidjs and start with the dark-mode init.

### 4. Right-aligned tables for numbers

Standard markdown tables — but always right-align numeric columns so monetary values, counts, and percentages line up by the decimal point. The right-align hint is `|---:|`.

```text
| Item | Description | Old value | New value |
|---|---|---:|---:|
| A1 | First metric  | 4,091.07 | 2,908.00 |
| A2 | Second metric | 4,399.90 | 3,499.00 |
| A3 | Third metric  |   499.99 |   379.00 |
```

Bold the most important number per row to draw the eye (`**2,908.00**`).

### 5. Inline emphasis beyond bold and italic

Outline supports a richer set of inline formats than vanilla markdown:

| Syntax | Renders as | When to use |
|---|---|---|
| `**bold**` | **bold** | Key term, the headline number in a sentence |
| `*italic*` | *italic* | Light emphasis, a name, a quote |
| `__underline__` | underline | A defined term you want to highlight as a definition |
| `~~strikethrough~~` | ~~strikethrough~~ | Showing what was removed or no longer applies |
| `==text==` | yellow highlight | Marking a value the reader should not miss |
| `` `monospace` `` | `monospace` | Code, identifiers, file paths, IDs |

`==…==` is Outline's highlighter. It supports several colours (yellow is the default). Multiple consecutive `==text==` spans render as a continuous highlight. Use it sparingly — if everything is highlighted, nothing is.

### 6. Math — inline LaTeX and KaTeX blocks

Outline ships KaTeX and supports LaTeX both inline and as blocks:

- **Inline math:** wrap the formula in `$$` markers — e.g. `$x = y$` renders as a typeset formula. `$_{subscript}$` and `$^{superscript}$` work too.
- **Math block:** insert via the `/math` slash command or with the markdown shortcut `$$$ ` followed by your formula. For multi-line content inside a math block, use `\newline`.

Example block:

```text
$$
c = \pm\sqrt{a^2 + b^2}
$$
```

Use math blocks for any formula that wouldn't fit cleanly on a single line, or that you want centred and visually separated from the surrounding prose.

### 7. Code blocks — language tags and the `diff` trick

Outline highlights any language a normal markdown editor would, plus one very useful special: `diff`.

````text
```diff
- old line that was removed
+ new line that was added
  unchanged context line
```
````

Use `diff` whenever you're showing a change: a config edit, a query rewrite, a migration plan. Lines starting with `-` render red, lines starting with `+` render green, and unchanged lines stay neutral.

For regular code, use the normal language tag (` ```typescript`, ` ```python`, ` ```sql`, ` ```bash`, etc.). Outline auto-detects the language when code is pasted from VS Code.

### 8. File attachments

To embed a file (PDF, ZIP, image, anything binary), use this exact link form:

```text
[filename.ext size_in_bytes](https://...)
```

Outline renders this as a download card showing the file name and size. Example:

```text
[invoice-2024.pdf 184320](https://example.com/invoice-2024.pdf)
```

The size is in bytes. If you don't know the size, omit the number — the card still renders, just without the size badge.

### 9. Embeds — interactive content from external services

Many services auto-embed when you paste their URL on its own line. The page renders an interactive preview instead of a plain link. Common ones:

- Figma, Framer, Miro, Pitch, Lucidsheet (design / whiteboard)
- YouTube, Vimeo (video)
- Airtable, Google Sheets, Google Docs, Google Slides, Google Maps
- GitHub, Linear (issue trackers)
- Codepen, Typeform

For services that don't auto-embed, use the explicit `/embed` slash command in the editor. From an MCP write, you simply paste the URL on its own line and Outline does the auto-detection itself.

If you want a *link* to one of these services without an embed, link the URL to a piece of inline text (`[click here](https://figma.com/...)`) instead of pasting the bare URL.

## Writing style — how a good Outline page reads

A reader scans first, reads second. The page must survive scanning.

**Lead with the answer.** First section after the title is the one-sentence summary in an `:::info` callout. Optionally a "Answer in one sentence" heading right after.

**Use H2 headings as chapter markers.** Every H2 is a new question or a new piece of evidence. Examples that work:

- `## What is this about?`
- `## Answer in one sentence`
- `## Evidence 1: [name of the check]`
- `## Concrete examples`
- `## Conclusion`
- `## Out of scope`

Number your evidence sections (`Evidence 1`, `Evidence 2`, `Evidence 3`) — it makes the structure obvious at a glance.

**Short sentences.** No more than one main idea per sentence. If you find yourself writing "because … and also … so that …", split it.

**Bulleted facts beat prose paragraphs** for any list of more than two items.

**Tables beat bulleted lists** for any structured data with more than one attribute per row.

**End with a Q&A table.** A two-column "Question / Answer" table at the bottom lets readers verify they understood without re-reading the whole page.

```text
| Question | Answer |
|---|---|
| Is the implementation broken? | No. |
| Does the new rule cause this? | No. |
| Why did the count drop? | An upstream change reduced the input set; the change is correct. |
```

**Bracket open topics in a `:::warning`.** If you investigated A but the user also asked about B and you did not answer B yet, end the page with a `:::warning` titled e.g. "Out of scope: …" so the unfinished part is impossible to miss.

## Page skeleton you can adapt

```text
# [Title — phrased as the question being answered]

:::info
**Summary**

[One sentence. The answer.]
:::

## What is this about?

[Short framing of the situation. Bullets.]

## Answer in one sentence

[Direct. One sentence, with the key word in bold.]

## [Optional: a mermaid flow showing the mechanic]

## Evidence 1: [name]

[Table.]

## Evidence 2: [name]

[Table.]

## Concrete examples

[Table with examples.]

++++
**What do the examples show in detail?**

[Detailed walk-through inside a toggle.]
++++

## Conclusion

| Question | Answer |
|---|---|
| ... | ... |

## Out of scope

:::warning
**Separate topic: ...**

[What was deliberately not covered here, and why.]
:::
```

## Using the Outline MCP tools

A getOutline wiki is read and written through MCP tools. The exact tool name is project-specific — typical patterns are `mcp__<wiki-name>__fetch`, `mcp__<wiki-name>__update_document`, `mcp__<wiki-name>__create_document`. The argument shape is the same across instances.

### Reading first

Before you update an existing document, fetch it so you know what's already there:

```text
mcp__<wiki>__fetch
  resource: "document"
  id: "<url-slug-or-uuid>"
```

You can pass either the URL slug from the document URL (e.g. the part after the last `/`) or the full UUID. The slug is fine.

### Choosing the editMode

`update_document` typically exposes four `editMode` values. Pick deliberately:

| editMode | Use when |
|---|---|
| `patch` | **Default for edits inside an existing document.** You give a `findText` (verbatim from the doc) and the replacement `text`. Only the matched section changes — highlights, comments, table column widths, and any other rich formatting outside the match are preserved. |
| `replace` | Whole-document rewrite. Use when the doc is empty, when you authored it yourself in this session, or when the user explicitly asks for a full rewrite. Will discard any rich formatting that markdown cannot represent. |
| `append` | Adding a new section at the bottom (e.g. a daily log entry). Safe — does not touch existing content. |
| `prepend` | Adding to the very top (e.g. a new summary banner). Rare. |

The trap to avoid: do *not* use `replace` on a document you did not author yourself in this session, unless the user explicitly asks for a full rewrite — you will silently destroy other people's highlights and comments.

### Iterating on a page you just wrote

When you write a long page and the user comes back with "make it nicer / restructure it":

1. The doc currently contains your previous draft. You wrote it, so `replace` is safe.
2. Re-author the whole page from the skeleton above, applying the user's specific feedback.
3. Use `editMode: "replace"`.

If the user says "add this on top of what's there" or "extend the log":

1. Use `editMode: "append"` (or `prepend`).
2. Start the new content with an H1 or H2 so it visually separates from what's there.

## Quick reference — Outline vs. plain markdown

| Feature | Plain markdown | Outline |
|---|---|---|
| Mermaid fence | ```` ```mermaid ```` | **```` ```mermaidjs ````** |
| Callouts | none (some flavours have `> [!NOTE]`) | `:::info` / `:::tip` / `:::warning` / `:::success` ... `:::` |
| Collapsibles | `<details><summary>` HTML | `++++` ... `++++` |
| Underline | none / HTML `<u>` | `__text__` |
| Highlight | none | `==text==` (multi-colour) |
| Inline math | none | `$x = y$` (LaTeX) |
| Math block | none | `$$ ... $$` (KaTeX), or `/math` |
| `diff` code block | rendered as plain code | red/green coloured `+`/`-` lines |
| File attachment card | bare link | `[name.ext bytes](url)` |
| External embeds | bare link | URL on its own line auto-embeds (Figma, YouTube, GitHub, …) |
| Line breaks in mermaid labels | `\n` | `<br/>` |
| Right-align table column | `|---:|` | same syntax — and actually worth using here for numeric columns |

## Further reading bundled with this skill

- `references/outline-syntax.md` — the official `docs.getoutline.com` formatting / blocks / code / embeds / diagramming pages, lightly merged. Read this when you need exact syntax for a feature not covered in SKILL.md, or to confirm what Outline officially supports (e.g. the full list of mermaid diagram types in the bundled v11.9.0).
- `references/mermaid-cheatsheet.md` — extra mermaid recipes (sequence, gantt, class, ER, state, user journey, quadrant, XY) all pre-configured with the Outline dark-mode init.
- `references/page-templates.md` — three full page skeletons you can copy-paste into a new document: investigation report, reference / how-it-works doc, daily logbuch entry.

---

## The non-negotiable: what every Outline page produced with this skill must be

Everything in this skill — the callouts, the toggles, the mermaid diagrams, the right-aligned tables, the highlights, the math blocks, the diff fences, the embeds — exists for one reason: **to make the page genuinely good for a human to read.**

That means three rules apply to every page you write:

1. **Cleanly structured.** Every section has one job. Every H2 announces what's coming. The reader can scan the headings alone and understand the shape of the page.
2. **Short sentences.** One main idea per sentence. If you find yourself reaching for a second comma, split the sentence. Bullet lists and tables almost always beat prose paragraphs.
3. **Easy on the eyes for a human.** A wall of text is a failure, even if every word is correct. Use the *full visual range* Outline gives you — `:::info` callouts for the punchline, `:::warning` for caveats, `:::tip` for asides, `++++` toggles for optional detail, mermaid diagrams (dark mode) for any flow, right-aligned tables for numbers, `==highlight==` for the value the reader must not miss, `__underline__` for defined terms, `**bold**` for the key word in a sentence, and `diff` blocks when you're showing a change.

A well-formatted Outline page is not decoration — it is the difference between "I read it" and "I had to re-read it three times". Use the visual tools. That's why they're there.
