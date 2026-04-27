# Page Templates

Three full page skeletons. Each is ready to copy into an empty Outline document and adapt. They demonstrate the patterns from `SKILL.md` working together.

## When to use which template

| Template | Use when the page is mainly … |
|---|---|
| **Investigation report** | "Did X happen because of Y?" — you investigated something, you have evidence, you have a verdict. |
| **Reference / how-it-works doc** | "Here is how this thing works." — long-lived documentation, fields explained, edge cases listed. |
| **Logbuch entry** | "What did we do today?" — chronological session entry that lives inside a daily/weekly log doc. |

---

## Template 1: Investigation report

Use this when you've investigated a question and want the conclusion to be obvious in the first 5 seconds.

```text
# [Title — phrased as the question being answered]

:::info
**Summary**

[One sentence. The verdict.]
:::

## What is this about?

[Three to five bullets. The situation. What was reported, what was suspected, what the question is.]

- ...
- ...
- ...

## Answer in one sentence

[A direct, non-hedging sentence. Bold the key word.]

## How the system works

```mermaidjs
%%{init: {'theme': 'dark'}}%%
flowchart TD
    A[Input] --> B[Filter]
    B --> C[Output]
```

## Evidence 1: [name of the first check]

[A short sentence introducing what the table proves.]

| Header A | Header B | Header C |
|---|---|---:|
| ... | ... | ... |

## Evidence 2: [name of the second check]

| Header A | Header B |
|---|---:|
| ... | ... |

## Concrete examples

[A summary table with the most illustrative examples.]

| ID | Description | Old | New |
|---|---|---:|---:|
| ... | ... | ... | ... |

++++
**What do the examples show in detail?**

[A few short paragraphs of context for readers who want the deeper read. Most readers skip this.]
++++

## Conclusion

| Question | Answer |
|---|---|
| Is the system broken? | No. |
| Why did the metric move? | [Short cause.] |
| What needs to happen next? | [Action or "nothing".] |

## Out of scope

:::warning
**Separate topic: [name]**

[Anything the user mentioned that you deliberately did not investigate here, with one sentence on why it's separate.]
:::
```

---

## Template 2: Reference / how-it-works doc

Use this for the long-lived "this is how component X works" documentation. Stable structure, lots of fields explained, hides edge-case detail in toggles.

```text
# [Component or feature name]

[One paragraph describing what this component does and who reads this page.]

:::info
**Source of truth**

[Where the canonical definition / config / code lives. A link or a path.]
:::

## At a glance

| Term | Meaning |
|---|---|
| **A** | ... |
| **B** | ... |
| **C** | ... |

## How it works

```mermaidjs
%%{init: {'theme': 'dark'}}%%
flowchart TD
    A[Stage 1] --> B[Stage 2]
    B --> C[Stage 3]
```

## Core logic

[Prose explanation, two or three short paragraphs. Code snippets where they actually clarify.]

## Special rules

### Rule 1: [name]

:::warning
**[Title of the gotcha]**

[One sentence on the gotcha so the reader sees it.]
:::

[Explanation of when the rule fires and what it does.]

++++
**Worked example: rule 1 fires**

[A small table or short scenario showing the rule applied.]
++++

++++
**Worked example: rule 1 does *not* fire**

[The mirror case so the reader sees the boundary.]
++++

### Rule 2: [name]

[Same shape — short prose, optional toggles for examples.]

## Field reference

++++
**Identification fields**

| Field | What it means | Technical source |
|---|---|---|
| `id`   | ... | ... |
| `name` | ... | ... |
++++

++++
**Pricing fields**

| Field | What it means | Technical source |
|---|---|---|
| `price_net`   | ... | ... |
| `price_gross` | ... | ... |
++++

## How to read the output

1. First look at [main field].
2. Then check [secondary field].
3. If [condition], compare [field A] against [field B].
```

---

## Template 3: Logbuch entry

Use this when the page is a dated entry inside a chronological log (daily standup, weekly recap, on-call shift report). Append to the existing log doc with `editMode: "append"`.

```text
# [YYYY-MM-DD — short headline]

:::info
**TL;DR**

[One sentence. What happened today / this session.]
:::

## What we did

- ...
- ...
- ...

## Decisions

| Decision | Why |
|---|---|
| [What was decided] | [The reason] |

## Open items

- [ ] [Item 1]
- [ ] [Item 2]

++++
**Detail / evidence**

[Anything that backs up the decisions but most readers skip — query results, screenshots, tables.]
++++

## Tomorrow / next session

[One short paragraph or bullet list. What's next.]
```

---

## Mixing the templates

Real pages often blend two templates. A common combination:

- **Investigation report** at the top (the "why are we here" and the verdict).
- **Reference doc** patterns at the bottom (a permanent field reference that lives on after the investigation is closed).

Use H2 headings as the seam between the two halves so the reader can tell where the investigation narrative ends and the reference material begins.
