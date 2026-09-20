---
name: literature-review-synthesis
description: Build a traceable literature review, evidence matrix, gap analysis, critical appraisal, or research agenda from a supplied corpus of academic papers. Use when several papers must be analyzed together, not one at a time.
license: MIT
metadata:
  short-description: Traceable synthesis from a paper corpus
---

# Literature Review & Synthesis

Analyze a supplied research corpus as a body of evidence. The value of this
skill is traceability: each substantive assertion must point to a source in the
corpus, with a page, section, table, or figure locator whenever the source makes
one available.

## Scope and guardrails

- Use only papers, text, or local paths the user supplied or authorized. Do not
  silently broaden the corpus with web search.
- Do not invent bibliographic details, findings, numbers, quotations, or
  citations. Record missing fields as `not reported`.
- Separate findings from synthesis and inference. Label proposed mechanisms,
  hypotheses, and study designs as **Inference**.
- Preserve disagreement. Explain evidence on both sides and plausible design or
  context differences; do not manufacture consensus.
- Treat the result as a corpus synthesis, not an independent verification,
  systematic review, or formal quality appraisal unless the user specifically
  supplies the needed protocol and criteria.

## Build a corpus index

First locate the papers and confirm the intended set if the count or scope is
ambiguous. Name unreadable, duplicate, or missing items explicitly. Assign a
stable ID (`P1`, `P2`, …) and build one normalized record per paper:

| Field | Required handling |
| --- | --- |
| Citation | Authors, year, title, venue, and persistent identifier when reported |
| Evidence locator | Page plus section, table, or figure when available |
| Research question and framework | Record the authors’ wording or a concise faithful paraphrase |
| Sample or dataset | Size, population, source, time period, and geography when reported |
| Method | Design, measures, and analytic approach |
| Findings | Direction, effect, and uncertainty only as reported |
| Limitations and future work | Attribute to the authors |

Keep the index in the response by default. Save it only when the user requests a
file or provides an output location. Every later claim must be traceable through
the index to its source paper.

## Choose a mode

If the user names an output, use it. Otherwise choose the narrowest matching
mode; when two modes are equally plausible, state the choice and ask before
producing a large artifact.

| Family | Modes | Reference |
| --- | --- | --- |
| Synthesize and map | `lit-review`, `knowledge-map`, `evidence-matrix`, `conceptual-framework`, `field-evolution` | [references/synthesize.md](references/synthesize.md) |
| Find gaps | `gaps`, `hidden-questions`, `untested-mechanisms`, `future-work`, `weak-assumptions`, `opportunity-scanner` | [references/find-gaps.md](references/find-gaps.md) |
| Appraise | `agreement-map`, `methodology-audit` | [references/appraise.md](references/appraise.md) |
| Design future work | `research-agenda`, `novelty-angles` | [references/ideate.md](references/ideate.md) |

Read only the reference for the selected mode. Follow its structure, but reduce
fixed counts when the corpus cannot support them and say why. For tabular modes,
offer a spreadsheet only when the environment can create one and the user wants
a file.

## Deliver

Start with corpus coverage: number reviewed, number unusable, date range, and
any scope limitation. Cite each substantive claim with its corpus ID or
author–year citation and locator. Finish with an honest coverage statement.

Before delivering, check that every cited item belongs to the corpus, each
inference is labeled, and each claim of agreement or disagreement names the
papers involved.
