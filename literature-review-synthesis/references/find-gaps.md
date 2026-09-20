# Find gaps & opportunities

Modes that surface what the literature has not resolved. All read from the corpus index. Every gap, question, or opportunity must trace to specific papers and, when available, source locators. Reject anything that cannot — an untraceable gap is a fabricated one.

---

## gaps — typed research gaps

Ignore generic gaps ("more research is needed"). Find gaps with a traceable cause: contradictory findings, missing populations, weak measurement, unexplored variables, outdated datasets, methodological limitations, untested assumptions, or theories never compared directly.

Per gap: `evidence from the papers → why the gap matters → a testable research question → a suggested study design`.

Do not rank. Label each gap by type (contradiction, missing-population, measurement, etc.).

---

## hidden-questions — implied but uninvestigated

Read the corpus for problems the authors reveal indirectly but never fully pursue: limitations, discussion sections, conflicting results, unexplained observations, stated assumptions, boundary conditions, future-work notes.

Generate up to 15 specific research questions that logically emerge from those openings. If fewer are traceable, give fewer and say so. Per question: which papers created the opening, and 2-3 sentences of reasoning.

---

## untested-mechanisms — implied, never measured

Find mechanisms, variables, mediators, moderators, or boundary conditions that appear important but are not directly tested: relationships authors imply but don't measure, variables named in theory but omitted from models, mechanisms assumed without evidence, contextual factors treated as background.

Per item: `implied mechanism/variable → papers suggesting it → why it stays untested → a proposed hypothesis → the minimum study design to test it`. Reject any item not traceable to the papers.

---

## future-work — extracted and combined

Focus on Discussion, Limitations, Conclusion, and Future Research sections.

Procedure:
1. Extract every concrete future-research suggestion.
2. Combine overlapping suggestions into larger research directions.
3. Per direction: what researchers already know, what they explicitly say is missing, which papers mention it, and 2-3 stronger research questions that extend the suggestion.

Drop vague suggestions; keep only the concrete.

---

## weak-assumptions — repeated but untested

Find assumptions researchers accept repeatedly without directly testing: theoretical, measurement, causal, population, and data assumptions, plus definitions treated as established fact.

Per assumption: `the assumption → papers relying on it → whether the evidence actually supports it → how a future study could test it directly`.

---

## opportunity-scanner — full scan

Treat the corpus as a dataset of unanswered opportunities. Scan across all categories: contradictions, missing populations, missing contexts, missing variables, untested mechanisms, methodological weaknesses, outdated evidence, theory conflicts, replication opportunities, unexpected findings, unanswered author questions.

Generate up to 20 concrete opportunities. If fewer are traceable, give fewer and say so. Output as a table:

`Opportunity | Evidence from papers | Why unresolved | Research question | Minimum study needed`

Reject any opportunity that cannot be traced back to evidence in the corpus. Offer this mode as `.xlsx` in addition to markdown.
