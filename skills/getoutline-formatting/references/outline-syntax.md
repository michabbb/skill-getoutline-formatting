# Outline Syntax Reference

Lightly merged extract from the official getOutline guide (docs.getoutline.com), captured for offline use. When you need exact syntax for something the main `SKILL.md` does not cover, look here first. Source pages:

- https://docs.getoutline.com/s/guide/doc/formatting-kn6wBtxlQ1.md
- https://docs.getoutline.com/s/guide/doc/blocks-iwAQVA8kAf.md
- https://docs.getoutline.com/s/guide/doc/code-ICONXGE8ct.md
- https://docs.getoutline.com/s/guide/doc/embeds-bqUBtgpqR0.md
- https://docs.getoutline.com/s/guide/doc/diagramming-KQiKoT4wzK.md

---

## Formatting philosophy

Outline's stated approach: **formatting should be focused on *semantics* and meaning rather than decorative elements.** A "notice" block is not coloured because colour is pretty — it is coloured because the reader needs to recognise *info* vs *warning* at a glance. Apply the same discipline when authoring: pick the syntax whose meaning matches your intent, not the one that looks nicest.

---

## Inline formatting

All of these are supported in any paragraph, list item, or table cell:

| Syntax | Renders as |
|---|---|
| `**bold**` | **bold** |
| `*italic*` | *italic* |
| `~~strikethrough~~` | ~~strikethrough~~ |
| `==highlighting==` | yellow highlight (multi-colour available) |
| `` `monospace` `` | `monospace` |
| `__underlined__` | underlined |
| `$LaTeX$` | typeset inline LaTeX |

`==…==` highlight notes:

- The default colour is yellow.
- Multiple consecutive `==span==` segments render as a continuous highlight.
- Other colours are available via the in-editor toolbar; from a markdown write you only get the default.

LaTeX inline notes:

- Wrap formula in `$$` markers: `$x = y$`.
- Subscript: `$_{subscript}$`. Superscript: `$^{superscript}$`.
- Renders via KaTeX.

---

## Headings, lists, and links

Standard markdown applies — `#`/`##`/`###` for headings, `-`/`*` for bullets, `1.` for numbered lists, `[text](url)` for links. Nothing Outline-specific to remember here.

---

## Notices (callouts)

Four built-in styles. Open with `:::TYPE`, close with `:::`:

```text
:::success
This is a success notice
:::

:::info
This is an info notice
:::

:::tip
This is a tip
:::

:::warning
This is a warning
:::
```

The empty line before the closing `:::` is the standard form Outline emits, but Outline accepts content tightly packed against the closing fence too. Always put a bold title on the first line for scannability:

```text
:::warning
**Heads up**

The migration runs for ~30 minutes during which writes are blocked.
:::
```

---

## Code blocks

Standard fenced code blocks with a language tag:

````text
```typescript
const counter = function() {
    let count = 0;
    return function() {
        return ++count;
    }
};
```
````

Special useful language: `diff`. Lines starting with `-` render red, lines with `+` render green, unchanged lines stay neutral.

````text
```diff
@@ -37,7 +37,7 @@ sql {
        #       sqlite
        #       mongo
-       dialect = "sqlite"
+       dialect = "postgresql"
```
````

Other notes:

- When code is pasted from VS Code, the language is auto-detected.
- Per-block line wrap can be toggled in the floating menu (in-editor only).
- Line numbers are a per-user preference (Settings → Preferences).

---

## Math

Two forms — inline and block.

**Inline:** wrap in `$$` markers — e.g. `$x = y$` renders as a typeset formula.

**Block:** insert via the `/math` slash command, or via the markdown shortcut `$$$ ` (three dollars + space). Renders via KaTeX.

```text
$$
c = \pm\sqrt{a^2 + b^2}
$$
```

For multi-line content inside a math block, use `\newline` — without it, content is combined onto a single line.

For the full TeX command surface KaTeX supports, see https://katex.org/docs/support_table.html.

---

## Images, videos, file attachments

**Images:** standard markdown `![alt](url "caption =widthxheight")`. Common formats supported (jpg, png, gif). Retina images are detected automatically.

**Videos:** drag-and-drop or insert via block menu. Uploads are not re-encoded — use a [widely supported codec](https://www.chromium.org/audio-video/) so the video plays everywhere.

**File attachments:** the link form is `[filename.ext bytes](url)`. Outline renders this as a download card showing the file name and size. Example:

```text
[invoice-2024.pdf 184320](https://example.com/invoice-2024.pdf)
```

Images can also be uploaded as file attachments using this same form.

---

## Embeds

Outline auto-embeds many services. Just paste the URL on its own line and Outline turns it into an interactive preview. Popular ones:

- **Design / whiteboard:** Figma, Framer, Miro, Pitch, Lucidsheet
- **Video:** YouTube, Vimeo
- **Spreadsheets / forms:** Airtable, Google Sheets, Typeform
- **Docs / slides / maps:** Google Docs, Google Slides, Google Maps
- **Code / project mgmt:** GitHub, Linear, Codepen

For services not on the auto-embed list, use the `/embed` slash command (in-editor) or paste an iframe.

If you want a **link** to a Figma / YouTube / etc. URL *without* embedding the content, link the URL to a piece of inline text (`[click here](https://figma.com/...)`) — that suppresses the auto-embed.

The canonical list lives at https://www.getoutline.com/integrations.

---

## Diagrams

Outline supports two diagramming approaches:

### 1. Mermaid (text-based, what you'll use from MCP)

Insert via a code block with the language set to **Mermaid diagram**, or via `/diagram` in the editor. From a markdown write, the language tag is `mermaidjs`:

````text
```mermaidjs
sequenceDiagram
    Alice->>John: Hello John, how are you?
    John-->>Alice: Great!
    Alice-)John: See you later
```
````

Bundled mermaid version: **v11.9.0**. Supported diagram types:

- Flowchart
- Sequence
- Gantt
- Class
- Git
- Entity relationship
- User journey
- Quadrant chart
- XY chart

The "full width" document option (per-page setting in the document menu) works much better with mermaid diagrams — turn it on for any page that has a diagram wider than ~600px.

For dark-mode init and Outline-specific recipes, see `references/mermaid-cheatsheet.md`.

### 2. Diagrams.net (graphical editor)

Insert via the block menu or `/diagram`. Outline opens an in-line drawing canvas; saves are stored as PNGs with embedded data so you can re-edit later by clicking the pencil icon. **Not usable from MCP** — you'd be embedding pre-rendered PNGs as regular images.

To import an existing diagram: in diagrams.net, **File → Export as → PNG**, then insert the PNG into the document.

---

## The block menu and the slash command

In the editor, an empty line shows a `[+]` icon that opens the block menu. Typing `/` opens the same menu inline and filters as you type. From an MCP write you don't have access to either — but knowing the slash names lets you describe what to insert when you hand a draft to a human reviewer:

| Slash | Inserts |
|---|---|
| `/math` | Math block |
| `/diagram` | Diagram (mermaid or diagrams.net) |
| `/embed` | Custom URL embed |
| `/code` | Code block |
| `/notice` | Notice / callout |

---

## Markdown shortcuts (in-editor)

Outline accepts standard markdown shortcuts as you type:

| Type this | Get this |
|---|---|
| `**` | bold |
| `*` | italic |
| `==` | highlight |
| `__` | underline |
| `~~` | strikethrough |
| `#` … `######` at line start | heading levels 1–6 |
| ` ``` ` | code block (then language) |
| `$$$ ` | math block |
| `$x$` | inline math |
| `>` | blockquote |
| `[ ]` / `[x]` | task list item |

The full keyboard-shortcut guide is available in-editor by pressing `?`.

---

## Document settings worth knowing about

- **Full width**: per-document toggle in the document menu. Removes the centred narrow column. Recommended for any page with mermaid, large tables, or diagrams.
- **Comments**: when enabled, selecting text shows a comment icon in the floating toolbar. Comments are visible to everyone with access to the document.
- **Insights**: track who has viewed the document.

These are workspace/document-level settings, not markdown — but knowing they exist helps you advise the user when something looks cramped or when they ask why a comment they wrote is visible to colleagues.
