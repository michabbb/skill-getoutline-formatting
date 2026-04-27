# Mermaid in Outline — Dark-Mode Cheatsheet

Outline ships **mermaid v11.9.0** and renders diagrams from a code block whose language tag is **`mermaidjs`** (not the plain `mermaid` you'd use elsewhere). Every example below is ready to paste — already wrapped in the right fence, already with the dark-mode init line on top.

## Two init directives you almost always want

**Dark mode (always):**

```text
%%{init: {'theme': 'dark'}}%%
```

**Subgraphs need extra margin** so the subgraph title doesn't collide with its child nodes. Combine both into one init line:

```text
%%{init: {'theme': 'dark', 'flowchart': {'subGraphTitleMargin': {'top': 10, 'bottom': 15}}}}%%
```

You can extend the same `init` object with other config — `theme` and `flowchart.subGraphTitleMargin` are the two that matter for Outline. See https://mermaid.js.org/config/configuration.html for the full surface.

## Universal label rules

- **Line breaks: `<br/>`, never `\n`.** A literal `\n` shows up as the two characters `\` and `n` inside the node.
- **Quote labels with spaces or punctuation:** `A["Drop with reason"]`.
- **Node IDs uppercase:** `A`, `B`, `C1`. Easier to read once the diagram grows.

## Recipes

Each block below is an entire copy-paste-ready Outline code block.

### Flowchart (top-down)

````text
```mermaidjs
%%{init: {'theme': 'dark'}}%%
flowchart TD
    A[Source data<br/>raw input] --> B[Filter stage<br/>only matching rows]
    B --> C[Output stage<br/>final result]
```
````

### Flowchart (left-to-right)

````text
```mermaidjs
%%{init: {'theme': 'dark'}}%%
flowchart LR
    A[Request] --> B[Validate]
    B --> C[Process]
    C --> D[Respond]
```
````

### Flowchart with a decision diamond

````text
```mermaidjs
%%{init: {'theme': 'dark'}}%%
flowchart TD
    A[Incoming event] --> B{Has required field?}
    B -- yes --> C[Process normally]
    B -- no  --> D[Drop with reason]
```
````

### Flowchart with subgraphs

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

### Sequence diagram

````text
```mermaidjs
%%{init: {'theme': 'dark'}}%%
sequenceDiagram
    participant U as User
    participant S as Service
    participant D as Database
    U->>S: Request
    S->>D: Query
    D-->>S: Result
    S-->>U: Response
```
````

### Gantt chart

````text
```mermaidjs
%%{init: {'theme': 'dark'}}%%
gantt
    title Project timeline
    dateFormat  YYYY-MM-DD
    section Phase 1
    Design          :a1, 2025-01-01, 7d
    Implement       :a2, after a1, 14d
    section Phase 2
    Test            :b1, after a2, 5d
    Release         :milestone, after b1, 0d
```
````

### Class diagram

````text
```mermaidjs
%%{init: {'theme': 'dark'}}%%
classDiagram
    class Order {
        +String id
        +Customer customer
        +addItem(Item)
        +total() Money
    }
    class Customer {
        +String id
        +String email
    }
    Order "1" --> "1" Customer
```
````

### Entity-relationship diagram

````text
```mermaidjs
%%{init: {'theme': 'dark'}}%%
erDiagram
    CUSTOMER ||--o{ ORDER : places
    ORDER ||--|{ LINE_ITEM : contains
    PRODUCT ||--o{ LINE_ITEM : "is in"
```
````

### State diagram

````text
```mermaidjs
%%{init: {'theme': 'dark'}}%%
stateDiagram-v2
    [*] --> Pending
    Pending --> Active: approve
    Pending --> Rejected: reject
    Active --> Closed: complete
    Rejected --> [*]
    Closed --> [*]
```
````

### User journey

````text
```mermaidjs
%%{init: {'theme': 'dark'}}%%
journey
    title Buying flow
    section Discover
      Search: 4: User
      View results: 3: User
    section Decide
      Compare: 3: User
      Read reviews: 4: User
    section Purchase
      Add to cart: 5: User
      Checkout: 2: User, System
```
````

### Quadrant chart

````text
```mermaidjs
%%{init: {'theme': 'dark'}}%%
quadrantChart
    title Reach vs. Effort
    x-axis Low effort --> High effort
    y-axis Low reach --> High reach
    quadrant-1 Do later
    quadrant-2 Do first
    quadrant-3 Skip
    quadrant-4 Maybe
    Feature A: [0.3, 0.6]
    Feature B: [0.45, 0.23]
    Feature C: [0.57, 0.69]
```
````

### XY chart

````text
```mermaidjs
%%{init: {'theme': 'dark'}}%%
xychart-beta
    title "Monthly conversions"
    x-axis [Jan, Feb, Mar, Apr, May, Jun]
    y-axis "Count" 0 --> 100
    bar [12, 24, 38, 52, 67, 81]
    line [12, 24, 38, 52, 67, 81]
```
````

### Git graph

````text
```mermaidjs
%%{init: {'theme': 'dark'}}%%
gitGraph
   commit
   branch feature
   checkout feature
   commit
   commit
   checkout main
   merge feature
   commit
```
````

## Tips

- **Use the document's "full width" option** (in the document menu, not in markdown) for any page with a diagram wider than the standard text column — diagrams compress unpleasantly otherwise.
- **One diagram per concept.** If you need two diagrams to explain one thing, split the section in two so each diagram has its own H2.
- **Label the edges** in flowcharts when the meaning isn't obvious (`-- yes -->`, `-- on error -->`).
- **Don't recreate a table as a diagram.** If the data is rows and columns, it belongs in a markdown table — diagrams are for *flow* and *relationships*.
