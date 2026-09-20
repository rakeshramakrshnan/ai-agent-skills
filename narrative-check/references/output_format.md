# Output format (fixed)

## Output format (fixed)

Always emit this block, with these section headers in this order and the tag positions
shown. Do not alter the section order or field positions.

```
NARRATIVE-CHECK REPORT
======================
Content type: [CT-xx name] Classification basis: [one line]
Word count: [n] Excluded non-prose: [n blocks / none]
Confidence cap: [High | Medium | Low] (reason)
Card behavior column: [literal | literal + analog | analog only | local only]

STRUCTURAL LEAN
---------------
AI-side features: [n] of [m] applicable
Human-side features: [n] of [m] applicable
Neutral (rated, neither side): [n]
Not applicable / no evidence: [n]

TOP AI-SIDE FEATURES (ranked by measured importance)
----------------------------------------------------
1. [NC-id] [name] | Table 14 rank: [#] | scope: global|local | validated: fiction | analog: unvalidated (if applicable)
 Text: [value] Human: [mean] AI: [mean] Gap ratio: [value]
 Evidence: P[n] "quote" ; P[n] "quote"
2. ...

LOCATED SPANS (local features only)
-----------------------------------
[NC-id] P[n] "quote" | why it fires | validated: fiction | analog: unvalidated (if applicable)
...

HUMAN-SIDE FEATURES PRESENT
---------------------------
[NC-id] [name] | Text: [value] | Evidence: P[n] "quote" ; P[n] "quote" | validated: fiction | analog: unvalidated (if applicable)

PARAGRAPH LEAN MAP (CT-12 only)
-------------------------------
P1 [AI-side n / human-side n] P2 [...] ...
S1 (P[a]-P[b]): AI-side [n] of [m] | human-side [n] | neutral [n] | leans [AI | human | mixed] at the structural level
 (the lean word compares two counts and is not a score; cells share evidence, so no
 probability, percentage or confidence may be derived from the margin — gap )
S2 (...): ...

MODEL FINGERPRINTS (UC-05 only; confidence: Low)
------------------------------------------------
[source] | [FP-id] | Evidence: P[n] "quote" | validated: fiction | analog: unvalidated (if applicable)

HANDOFF TO narrative-humanize
-----------------------------
[NC-id | scope | P-locations or S-range | direction to move | CT row]
...
```

Sections that do not apply (PARAGRAPH LEAN MAP outside CT-12, MODEL FINGERPRINTS outside
UC-05) are omitted from the report, not left empty.

Field notes that do not change the block: `Classification basis` is one line and on CT-02
must say the literal cells were validated on fiction and this input is not fiction; on
CT-12 / 13 / 14 it names the `base:` row; under UC-04 it names the target and the row the
input would have received. `Confidence cap` reasons are: the row's column, a length gate, a
modifier row, or UC-05. On CT-14 every feature line carries `language: unvalidated` after
its evidence tag. Under UC-05 the verbatim caveat block opens the MODEL FINGERPRINTS
section, before its first `[source] | [FP-id]` line. `Table 14 rank` prints `none (Table 15 #N)` for a human-elevated
card that fired AI-side (Step 6). The `S1 (P[a]-P[b]): …` lines appear only on CT-12.

### Blocks printed outside the fixed format (separate; never inserted into it)

In this order, after the fixed block:

1. **Lean sentence** (Step 8), one line.
2. **CARD LEDGER**, every run: one line per applicable card, both tiers, ordered by
 `importance_rank`: `NC-id | tier | scope (basis) | tag | value | side | importance_rank |
 Table 14 or 15 rank | gap ratio or n/d | evidence`. The `tier` field reads `core` or
 `extended`. A single-row AI-elevated option card that did not fire reads
 `side: neutral (option absent)`, never `human-side` (Step 4). The evidence field
 takes one of three values: `P<n> "quote"` (one span for a local card, two or three
 driving spans for a global card); `no evidence` (Step 4); or `absence (nothing to
 quote)` for a card that fires on the absence of a feature, such as NC-17 `no subplots`
 or NC-23 `never` . This is where cards
 that are AI-side but outside the top 5, neutral cards, and no-evidence cards are
 accounted for. `n/a` cells are listed at the end with the cell's reason.
3. **RUN NOTES**: the excluded-block list from Step 3; the length-gate disclaimer sentence
 when a gate fired; CAV-TRANSFER (verbatim from `use_case_matrix.md` §0.4)
 whenever the row is not CT-01; CAV-SHORT and CAV-LANG when F-SHORT or F-LANG fired;
 CAV-FP-TOTAL when per-source counts were printed; under UC-04, the line naming the
 input-row-only cards dropped by design (`use_case_matrix.md` UC-04); the use
 case's own closing line (UC-01: `To act on these findings, ask for a rewrite; the structural pass will use the
 HANDOFF block above.`).
4. **EXTENDED TIER (measured, not in Table 16)**, whenever any NC-31 … NC-40 card was rated:
 the same four counts as STRUCTURAL LEAN over the extended cards alone; then its own
 top-AI-side list in `importance_rank` order with the same fields as TOP AI-SIDE FEATURES
 except that `Table 14 rank` reads `none (extended tier)`; then its located spans and
 human-side lines. The block opens with one sentence: `Extended tier: features from
 StoryScope's 304-feature taxonomy whose human/AI separation and classifier weight this
 project measured against the authors' released data; not among the paper's 30 core
 features (`feature_cards.md` Part B).` Extended results are never added
 into the fixed block's counts, and the Step 8 lean sentence stays core-tier; this block
 carries its own equivalent sentence.
5. **Use-case blocks**, when the use case calls for one: the NC-id DIFF (UC-08, and P3 of a
 rewrite) as `use_case_matrix.md` §0.2 defines it, a separate block and not
 a report section ; the DISPUTE RECORD (UC-09); the UC-06 summary table.

Before the fixed block, and only there: the CT-15 opening sentence ("Classified as
`other`: …", Part 2 rule 13) and, under UC-11 or P3, the WORD-LEVEL REPORT, which this
report follows as a separate block with the COMPLEMENTARITY paragraph after both
(`use_case_matrix.md` §2.2; append, never merge).

---
