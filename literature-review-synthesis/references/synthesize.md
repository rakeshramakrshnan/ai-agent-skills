# Synthesize & map

Modes that organize the existing literature. All read from the corpus index. Cite a specific paper for every important claim and include a page, section, table, or figure locator when available; introduce no finding absent from the corpus.

---

## lit-review — structured thematic review

Build a thematic review, not a paper-by-paper summary. If the corpus cannot support four themes or five unresolved questions, use the supported number and state the limitation.

Procedure:
1. Cluster papers into 4-7 themes by shared argument, not surface topic.
2. Per theme: what it argues, its strongest evidence (name the paper), and where papers within it agree or diverge.
3. Trace how thinking evolved over time across the themes.
4. End with exactly 5 unresolved questions the corpus leaves open.

ALWAYS use this structure:
```
# Literature Review: [topic]
## Overview — scope, N papers, year span
## Theme 1: [name]
- What it argues
- Strongest evidence: (Author, Year)
- Agreement / divergence within the theme
## Theme 2 … (through 4-7 themes)
## How the thinking evolved
## Five unresolved questions
```

---

## knowledge-map — concept map

Show how ideas connect, rather than summarizing papers individually.

Procedure — organize the corpus into these layers, then draw the connections:
core concepts → major theories → key variables → proposed relationships → common methods → major findings → contradictions → unresolved areas.

Output a text map in arrow notation, e.g.:
```
Concept A --influences--> Concept B --measured by--> Method C
Theory X --predicts--> Relationship Y --contested by--> (Author, Year)
```
Group arrows by layer. Every node must trace to at least one paper; annotate contested links with the papers on each side.

---

## evidence-matrix — per-paper extraction grid

Turn the corpus into a matrix, one row per paper.

Columns (keep entries concise but specific):
`ID | Citation | Evidence locator | Research question | Theory/framework | Sample | Dataset | Methodology | IVs | DVs | Key findings | Effect/direction | Limitations | Future research`

After the matrix, add two short sections:
- **Recurring methodological patterns** across the corpus.
- **Where results differ despite similar designs** — name the papers and the divergence.

Offer this mode as `.xlsx` in addition to markdown.

---

## conceptual-framework — variable model

Synthesize the corpus into a testable framework.

Procedure:
1. Identify the most evidence-supported dependent variable.
2. Identify major predictors, candidate mediators, moderators, and control variables.
3. Identify the relevant theories.
4. For every proposed relationship, cite the papers that support it and mark it **well-established** or **still uncertain**.

Output:
```
Independent Variables --> Mediators --> Outcome
```
with moderators listed separately (which relationships they condition), controls listed, and a supporting-papers citation on each arrow.

---

## field-evolution — chronological phases

Explain how research on the topic changed over the period the corpus covers.

Procedure:
1. Divide the timeline into meaningful phases defined by shifts in theory, dataset, methodology, assumptions, or conclusions — not arbitrary decades.
2. Per phase: what researchers believed, what evidence challenged those beliefs (cite it), and what new questions appeared afterward.

ALWAYS use this structure:
```
# How the field evolved: [topic], [start]–[end]
## Phase 1: [name] ([years]) — [what defines the phase]
- Prevailing belief
- Challenging evidence: (Author, Year)
- New questions that opened
## Phase 2 … (as many phases as the corpus supports)
```
