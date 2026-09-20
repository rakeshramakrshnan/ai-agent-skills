---
name: narrative-check
description: >
 Audit text for structural AI-writing signals and return located evidence with revision
 priorities. Use for narrative-check, StoryScope checks, or AI-like narrative assessments.
 Does not rewrite.
license: MIT
---

# Narrative-Check Skill

`narrative-check` applies a StoryScope-derived rubric and returns located evidence. It is a model reading a rubric, not StoryScope's classifier and not proof of authorship.

The core tier contains the paper's 30 Table 16 features. The extended tier contains ten measured taxonomy features marked `extended (measured, not in Table 16)` in `references/feature_cards.md`; never describe those as paper core features.

StoryScope's published evidence covers fiction. For every other content type, use the verdict attached to that exact cell in `references/content_type_matrix.md`; never generalize a result across genres. Report individual features with their confidence tags, never as detector accuracy.

## Reference files (read as needed; never copy their content into the report)

All relative paths are relative to this file.

| File | What it holds | When to open it |
|---|---|---|
| `references/nc_id_registry.md` | The shared key: NC-01 … NC-30 and FP-<source>-<n> ids and names | Every run, to name cards |
| `references/feature_cards.md` | One card per id: `question`, `response_options`, `human_mean`, `ai_mean`, `direction`, `scope`, `scope_basis`, `fiction_detection`, `locate_by`, plus `importance_rank` and (on NC-31 … NC-40) `tier` | Every run, for every card rated |
| `references/content_type_matrix.md` | CT-01 … CT-15 rows, one cell per card (`literal` / `analog` / `n/a`), the classifier (Part 2), the length gates (§0.5) | Step 1, Step 2, Step 4 |
| `references/use_case_matrix.md` | UC-01 … UC-12 entries, failure behaviors (F-*), standing caveat texts (CAV-*), NC-id DIFF and DISPUTE RECORD definitions, word-level append layout | Whenever a use case beyond plain check applies |
| `references/word_level_signals.md` | The word-level / surface layer: nine signal categories, severity mapping, mixed-authorship overlay, scoring thresholds | UC-11 and P3, to append (never merge) the two layers' reports |

---

## Protocol

Eight steps, in this order. Do not skip a step; where a step does not apply, say so in
the report rather than leaving it out silently.

### Step 1 — Read the full input, then classify it to one CT row

Read the whole text before rating anything. Classify with the ordered classifier in
`references/content_type_matrix.md` Part 2 (rules 0–13, first match wins). In brief:

1. Rule 0: a user-named target register (UC-04) sets the row; the row the input would have received on its own is reported second.
2. Rules 1–2: CT-14 by either clause of rule 1 — Clause A, a marked or evidently translated text (any translation marker, whatever the language share), or Clause B, more than half of the prose sentences not in English; any code block, table, display equation, or figure/table caption → CT-13, with a `base:` row taken from the prose remainder.
3. Rule 3: prose word count under 300 → CT-10 (local cards only); 300 words up to but not including 800 → row by rules 4–13 with confidence capped Low.
4. Rule 4: user-stated or signalled mixed authorship → CT-12, with a `base:` row per section.
5. Rules 5–13, in order: fiction (CT-01), journalism (CT-08), narrative nonfiction (CT-02), research-paper sections (CT-04 / CT-05 / CT-06, split into separate units if more than one group is present), documentation (CT-07), marketing (CT-09), application statements (CT-11), opinion essay (CT-03), fallback `other` (CT-15) with its required "fit no row because" sentence.

State the row, a one-line classification basis, and the card-behavior column the row
selects. The column is fixed by the row:

| CT row | Card-behavior column |
|---|---|
| CT-01 Short fiction, novel excerpt | literal |
| CT-02 Narrative nonfiction | literal + analog |
| CT-03 Opinion essay / blog post | analog only |
| CT-04 Research paper: abstract, introduction | analog only |
| CT-05 Research paper: methods, results | analog only |
| CT-06 Research paper: discussion, limitations, conclusion | analog only |
| CT-07 Technical documentation, tutorials, READMEs | analog only |
| CT-08 Journalism / news feature | analog only |
| CT-09 Marketing / product copy | analog only |
| CT-10 Short-form, under the short gate | local only |
| CT-11 Grant, fellowship, application statements | analog only |
| CT-12 Mixed authorship | analog only (inherits the base row's cells per section) |
| CT-13 Contains code, tables, equations, captions | analog only (inherits the base row's cells on the prose remainder) |
| CT-14 Non-English or translated | analog only (inherits; every line also tagged `language: unvalidated`) |
| CT-15 `other` | local only |

The column above is a summary of how each row behaves, not a copy of its contents; where it
and the matrix row disagree, the matrix row governs. **Whether UC-05 runs is decided by the
row's own `FP` cell, which is why no FP column appears here** : fingerprints are
rated where that cell is `literal` or `analog`, and where it is `n/a` UC-05 returns the
not-applicable sentence in Step 5. Read the cell; it names any restriction that applies.

What the columns mean: **literal** — every card's `fiction_detection` is used as written and
every line carries `validated: fiction`. **literal + analog** — the same, except the cells
the matrix marks `analog` on that row, and the report's `Classification basis` line must say
that the literal cells were validated on fiction and this input is not fiction. **analog
only** — every non-`n/a` cell is a translated feature carrying its own `threshold:` verdict in
the matrix; lines carry `established: <corpus>` only where that verdict is `ESTABLISHED`, and
`analog: unvalidated` otherwise. **local only** — the row rates a restricted subset
and marks the rest `n/a`; the report prints each `n/a` cell's own reason.

**Which cards a row rates is the matrix row's business, not this file's.** Do not work from a
remembered list, and do not expect a number here: open the row and read its cells. This skill
deliberately names neither the cards a row rates nor how many there are, because both drift
the moment a card's scope changes or the matrix gains a row. A stale list here once
contradicted both the matrix and the cards; the count that replaced it went stale within a
day, for the same reason and by the same mechanism. Rows do not rate equal numbers of cards,
and the live set on any row includes extended-tier cards and the fingerprint set. Count them
in the matrix if you need a total for the report.

Honor UC-04 exactly: when the user names a target register, the target's row governs
every cell choice, and the report prints both rows as Part 2 rule 0 specifies.

### Step 2 — Apply the length gate

Compute the prose word count (after Step 3's exclusions, which is why the count is
finalized only after segmentation; classify provisionally, then confirm).

- **Under 800 prose words:** confidence cap Low, and the report header states that global
 features cannot be rated reliably at this length. Global cards are still rated and shown
 with that caveat.
- **Under 300 prose words:** rate local cards only; every global card returns `n/a: global
 rating impossible under 300 words`. The row is CT-10 unless the would-be genre is CT-01
 or CT-02, in which case the row label stays CT-10 and the local cards use that genre's
 cells (the operating rules, C2 Q4 ruling).

**The gate is measured on post-exclusion prose**, i.e. after Step 3 removes code, tables,
equations, captions and other non-prose. That is the correct basis — non-prose is not rated —
but it means non-prose volume can decide which cards run. Report both numbers, always:
`Word count: [total] ([n] prose after exclusion)`. When exclusion moves the text across the
300- or 800-word gate, **say so explicitly and name the base row it changed** (;
observed on a 343-word document that fell to 285 prose words and changed row). A reader must
never have to infer that a table is why the global cards did not run.

**Scripts without word delimiters cannot be gated by words** . Whitespace tokenisation
of Chinese, Japanese, or Thai text returns a near-zero count and would route a document of any
length to CT-10, stripping every global card. For CJK input use **600 and 1600 characters**
as the analogues of the 300- and 800-word gates, counting characters excluding punctuation and
whitespace; state in the report that the character gate was used. **These two numbers are this
build's proposal and are less grounded than the word gates** — they are a reading-rate
convention, not a measurement, and no CT-14 length claim for a non-delimited script should be
presented as reliable until the unit is properly defined.

**None of these thresholds comes from the paper.** StoryScope's corpus averages 4,753 words
(fn. 8, p. 3), its length audit never examines texts below its own shortest tertile, and no
800- or 300-word figure appears on any page (findings §E.2). Whenever a gate fires, the report
prints that sentence with it.

Confidence cap, by build convention (not a paper measure): **High** only on CT-01 at 800
prose words or more with no modifier row (CT-12 / 13 / 14); **Medium** on CT-02 at 800 words
or more; **Low** everywhere else: any analog-only or local-only row, any length gate, any
modifier row, and always for MODEL FINGERPRINTS. "High" means the cards were applied as
the paper validated them; it never means an authorship verdict is certain (see Calibration).

### Step 3 — Segment into numbered paragraphs

Number every prose paragraph in order: P1, P2, P3 … A paragraph is a block of prose as
typeset; each line of dialogue set on its own line is its own paragraph. Paratext is not a
paragraph and is never quoted as evidence for a card: titles, headings, bylines, datelines,
subject lines, salutations, sign-offs, translator's notes, and speaker labels (the name
before a line in a transcript or script) are not numbered.

Exclude non-prose before numbering (CT-13, Part 2 rule 2): code blocks, tables, display
equations, figure and table captions, and non-English spans in an English-majority
document with no translation marker (rule 1, Clause B not met). Report what was excluded: block type, where it sat, word count, and a one-line
total. Inline code and inline math inside a prose sentence stay in the prose.

Every location in the report is `P<n>` plus a verbatim quote of at most 25 words. Never
a line number, never a character offset, never a paraphrase in quotation marks. A quote
must be findable in the text by exact search.

Counts for analog cells are taken over the numbered paragraphs only. Ties: the cells use
strict comparatives ("more than", "outnumber", "above one per"), so a count that equals
the threshold does not meet the condition and falls on the non-AI side .

### Step 4 — Rate every card whose cell for this CT row is not `n/a`

Open `references/content_type_matrix.md` at the row from Step 1 and go down every cell in it.
For each card whose cell is `literal` or `analog`, open the card in
`references/feature_cards.md` and record, in this order:

**Every card list in this file is navigational, not authoritative.** Where a list here names
cards (the option-card groups below, the gap-ratio exclusions, the fidelity examples), it is a
reading aid. The authority is the file that owns the property: the **matrix row** decides
which cards a content type rates and whether each is `literal`, `analog`, or `n/a`; the
**card** decides its own `scope`, `option_rows`, `response_type`, `importance_rank`, and
`tier`. If a list here disagrees with either, they win — and the disagreement is a defect to
report, not to smooth over (see Calibration).

**Both tiers are rated, and both are read off the matrix.** Each CT section carries a cell for
every core card, every extended card, and the fingerprint set.
**The extended tier is rated wherever the matrix defines a cell for it** — every CT row does,
`literal` / `analog` / `n/a` exactly as for a core card — so an extended card is skipped only
where its own cell says `n/a`, never because of its tier . Earlier builds of this
skill said the extended tier had no matrix cell and returned `n/a` off `literal` rows; that
text was stale, and a rater who followed it rated no extended card at all. Extended cards
obey the same scope, length-gate, side, and evidence rules as core cards; only the reporting
differs, and their results go in their own block (Output format, below), never mixed into the
core counts.

1. **Value** on the card's own scale: the 1–5 or ordinal rung, or the option chosen, using
 the card's `response_options`. For an `analog` cell, the value is what the cell's
 operational definition asks you to count or judge; still express it on the card's scale.
2. **Human mean** and **AI mean** from the card (`human_mean`, `ai_mean`; for two-row cards,
 the `option_rows` of the row that fired). For NC-22 and NC-23 print the Table 16 value
 followed by the §4.1 percentage in parentheses, as the cards do : `Human: 0.67
 (67%) AI: 0.39 (39%)` and `Human: 0.28 (28%) AI: 0.07 (7%)`. These are means over 0-based
 ordinal codes; §4.1's "67%/39%" and "28%/7%" are the same numbers with the decimal moved,
 a paper erratum settled by recomputation from the released features
 (validation results Step 1).
3. **Side**: `AI-side`, `human-side`, or `neutral`. For scale and ordinal cards, the text
 value is AI-side when it lies on the AI-mean side of the human mean, human-side when on
 the far side, neutral when it equals the human mean at the rating resolution.

 For **option cards, the rule depends on how many option rows the card has, and it is not
 symmetric.** Read this before rating any of them.

  - **Single-row AI-elevated option cards — NC-04, NC-05, NC-10, NC-15, NC-16, NC-18** (one
 printed `→ option` row, on the AI-elevated side). Two outcomes only: the AI-elevated
 option fired → **AI-side**; any other option fired, or the option is absent →
 **`neutral`, never human-side**. **This holds for `analog` cells too, and the two
 rulings behind it do not conflict** : settles *which rule decides* an
 analog cell — the cell's own condition, not the mean comparison — while settles
 *what a one-sided condition returns when it does not fire*, which is `neutral`. So on
 an analog cell for one of these six cards, read the cell's condition rather than the
 means, and if the condition is not met record `neutral`, not `human-side`. Reading
 as "human-side when the condition is not met" was measured to roughly double the
 human-side count on half a batch; a one-sided condition that does not fire is no
 evidence for either side. Two-sided analog cells — those whose text names both an
 AI-side and a human-side condition — are unaffected and can still land human-side. The card measures the presence of one AI-elevated
 option, and the paper prints no human-elevated option for it, so absence is not evidence
 of the human-elevated option — it is only the absence of the AI one. The human base
 rates are not zero and make that concrete: narratorial thematic commentary appears in
 51% of human stories (NC-04), external-description introductions in 30% (NC-16),
 olfactory imagery in 57% (NC-10), so "did not fire" is a much weaker statement than
 "reads human". **This was the largest single source of lean-count variance between two
 blind raters** (validation results Step 4), which is why it is ruled here
 rather than left to judgment.
 **NC-18 is one of these six, and it is one-sided — this is the case raters get wrong.**
 Table 16 prints exactly one option row for Resolution Mode, `→ internal understanding`
 (27% human / 47% AI), on the AI-elevated side. The trap is §4.1's sentence: "AI
 resolutions favor internal understanding or acceptance (47% vs. 27%), whereas humans are
 more comfortable with ambiguous endings" (p. 7). That reads like a human-elevated
 option, but the paper prints no `→ ambiguous` row for NC-18, and a prose remark is not an
 option row. **An ambiguous or unresolved ending is `neutral` on NC-18, never human-side.**
 Where the ambiguity is moral, the human-side credit belongs to NC-30
 `→ ambivalent/mixed` (59% human / 38% AI), which *is* a printed human-elevated row. Read
 NC-18 as two-sided and every document with an unresolved ending shifts by one card.
  - **Two-row cards — NC-06, NC-07, NC-17.** These do print a human-elevated option, so all
 three outcomes are available: AI-elevated option → AI-side; human-elevated option →
 human-side; any other option → neutral (see Calibration).
  - **Single-row human-elevated cards — NC-21, NC-30, and the human-elevated ordinals.**
 Absence *is* the AI-side condition here, by the direction convention in matrix §0.2 and
 by §4.1's phrasing of these features as things human authors do more (p. 7). The
 asymmetry with the first bullet is deliberate: the paper ranks the alternative to a
 human-elevated option, and does not rank the alternatives to an AI-elevated one.

 For a human-elevated feature that the text simply lacks (no time jumps, no reader address,
 one location), the side is therefore AI-side, unless the cell is `n/a` because the
 register forbids the feature. **For an `analog` cell the cell's own AI-side
 condition sets the side**: AI-side when the condition is met, human-side when it is
 not, neutral only where the cell itself names a neutral outcome; the mean rule above
 applies to `literal` cells only . Ties follow Step 3.
4. **Scope and basis**, copied from the card: its `scope` and its `scope_basis`, both
 verbatim, whatever they say. Do not check them against a remembered vocabulary — the
 `scope_basis` values have already been re-based once, when the validation study moved the
 NC cards onto the authors' own locating text, and a list of permitted values here
 would go stale exactly as the row-card lists did. What does not change is the caveat: a
 card's scope is **not** a paper finding . Even where the basis is the authors'
 `detection_method`, that text documents what the feature means, not how the assigner
 located it, so scope remains an inference with a stated basis rather than a measurement.
5. **Evidence.** A **global** card gets exactly one document-level rating plus the two or
 three spans that most drive it (`locate_by` on the card names the typical drivers). A
 **local** card gets an exact `P<n>` quote for every firing instance. Never attach a
 single `P<n>` to a global card as if it were the rating's location; the driving spans
 illustrate a whole-document judgment, they do not locate it.
6. **Tag**: `validated: fiction` for a literal cell; for an analog cell, read that cell's
 `threshold:` verdict in `content_type_matrix.md` and tag `established: <corpus>` if and
 only if it says `ESTABLISHED`, otherwise `analog: unvalidated`. Where the verdict is
 `NOT SUPPORTED` or `REFUTED`, append it — `analog: unvalidated (not supported on
 <corpus>)` / `analog: unvalidated (REFUTED — separates opposite on <corpus>)` — so a
 reader is not left to infer that an untested cell and a failed one are the same thing.
 Plus `language: unvalidated` on CT-14, plus
 `tier: extended (measured, not in Table 16)` on every NC-31 … NC-40 line.
7. **Ranks**, copied from the card: `importance_rank` (both tiers) and the Table 14 or
 Table 15 rank (core tier only). Step 6 uses the first to order, the second to display.

**Identifying the protagonist (governs NC-15, NC-16, NC-30).** All three cards key on "the
protagonist", and no card defines one; on the validation stories this decided the rating on a
two-hander and on a story whose central figure is dead (`references/feature_cards.md` §5).
Apply this sequence and stop at the first step that settles it:

1. **Focalization.** The protagonist is the character whose inner experience the narration
 renders directly and most often (the authors' own detection method for breadth of
 focalization counts exactly this: characters "whose thoughts or felt experience are
 directly rendered", `feature_cards.md` NC-39).
2. **Causal role**, when two or more characters are focalized comparably. The protagonist is
 the one whose choice resolves, or fails to resolve, the central conflict.
3. **Joint protagonists.** If the resolution is genuinely shared (a two-hander), rate the
 pair as one protagonist unit: NC-15 asks whether *their* choice drove the outcome, NC-16
 uses the first of the two introductions to appear, NC-30 rates the pair's combined moral
 presentation. Say so on the ledger line.
4. **A central figure who cannot act** (dead, absent, or an object of others' attention).
 The rating protagonist is the acting character whose pursuit organizes the events; NC-16
 then describes *that* character's first appearance. Name the substitution in RUN NOTES.

Record the outcome once per run in RUN NOTES as
`protagonist: <name or descriptor> (basis: focalization | causal role | joint | acting agent)`,
and use that one identification for all three cards. Changing protagonists between cards in a
single run is a defect.

**NC-04 and free indirect thought.** NC-04 fires on the *narrator* explaining the story's
theme, and free indirect discourse — a character's thought rendered in the narrator's third
person without quotation marks or "he thought" — decided the rating on one validation story.
A thematic statement in free indirect thought is **the character's, and does not fire NC-04**,
when all three hold: the surrounding sentences sit in that character's perspective; the
judgment is one that character could plausibly hold; and it can be rewritten as "she thought
that …" without changing what the sentence means. It **does fire NC-04** when the statement
steps outside every character's view — a generalizing claim in the narrator's present tense, a
remark addressed past the characters to the reader, or a judgment no character in the scene is
in a position to make. Ambiguous cases do not fire, per the tie convention in Step 3, and the
ledger line says `ambiguous: free indirect`.

Write **`no evidence`** explicitly when a card cannot be rated from the text: the object the
question asks about is absent or outside the excerpt (no ending for NC-15 / NC-18, no
character introduced for NC-16, the excerpt does not begin at the opening for NC-19, no
dialogue for NC-05 / NC-29). `no evidence` is neither AI-side nor human-side, and it is
distinct from `n/a` (the cell says the feature has no counterpart in this content type)
and from "absent" (the feature could appear and does not, which is a rated value).

An unrated card is a card not run. The report must account for every applicable card:
AI-side + human-side + neutral + no evidence = m, the number of non-`n/a` cells. **This is a completeness audit, not a score.** It asserts only that every applicable card was reported in exactly one bucket. It makes no claim that the cells are independent, and no quantity derived from it is a measurement (gap, reconciling this with `content_type_matrix.md` §0.8). These
four quantities are the four STRUCTURAL LEAN lines : `AI-side features`,
`Human-side features`, `Neutral (rated, neither side)`, and `Not applicable / no evidence`;
the fourth line prints the no-evidence count plus the `n/a` cells, which sit outside m.
The CARD LEDGER (below) lists each card.

The accounting above is per tier: the four STRUCTURAL LEAN lines count the core tier, and the
extended tier gets the same four counts in its own block.

On CT-12, local cards fire per paragraph and global cards are rated once per lean-map
section, never per paragraph and never for the document as a whole (the operating rules, C2 Q3
ruling; matrix CT-12 row rule).

### Step 5 — Fingerprint cards: UC-05 only

Rate the fingerprint cards (`FP-<source>-<n>`, Part 2 of `references/feature_cards.md`)
only when the user asks which model wrote the text (UC-05), and only where the CT row's
FP cell is `literal` or `analog` (Step 1 table). Where the FP cell is `n/a`, the
MODEL FINGERPRINTS section prints exactly this sentence from `references/use_case_matrix.md`
UC-05, the single canonical wording, and nothing else:
`Model fingerprints: not applicable to [CT-xx]; fingerprint features are fiction-only (Table 17, p. 27)`. Fingerprints never enter the HANDOFF block; `narrative-humanize` does not
target them.

Confidence for this section is Low and cannot be raised: even with all its narrative
features and a trained classifier, the paper's six-way authorship attribution reaches
68.4 macro-F1 (Table 3, p. 8), and this skill has neither. Fingerprints belong to five
specific model versions at one point in time. Print the UC-05 caveat block (verbatim
below, under Use-case coverage) at the top of the section. Cards with `value: none printed
in paper` may fire only on the §5 prose description their card quotes. Never
attribute NC-16 (Character Introduction → external description) to Gemini; it is a core
all-AI feature . When per-source fingerprint counts are printed, print CAV-FP-TOTAL
from `references/use_case_matrix.md` §0.4 with them; never state a single overall total .

### Step 6 — Summary and ranking

Fill the four STRUCTURAL LEAN lines: AI-side count, human-side count, neutral count (rated
cards on neither side), and the combined not-applicable / no-evidence count, the first two
against m, the number of applicable cards (Step 4). On a CT-12 document these are document
totals by **section majority**: a card counts AI-side if it is AI-side in more lean-map
sections than human-side, human-side in the reverse case, neutral when the sections split
evenly; the `AI-side features` line then carries the pointer `(section majority; see
PARAGRAPH LEAN MAP)` after its value .

**TOP AI-SIDE FEATURES is ranked by `importance_rank`** — the card's position in the measured
classifier importance ordering — and lists the top 5 AI-side core cards in that order. Every
card carries the field (`references/feature_cards.md`); the ordering itself is
`references/feature_cards.md` Part A for the core tier and Part B for the extended tier,
where rank 1 is the heaviest feature. Ties are broken by the Table 14 or Table 15 rank, then
by NC-id.

**Table 14 rank stays as a displayed field, not the sort key.** Print it as `Table 14 rank: N`
from the card's `question` cite (`Table 14 #N`). Human-elevated cards that fire AI-side by
absence (NC-21 … NC-30; e.g. NC-22 at its lowest rung) have no Table 14 row and print
`Table 14 rank: none (Table 15 #N)`. The two-row cards NC-06, NC-07, NC-17 fire AI-side only
through their Table 14 option and use that row. Extended cards have neither and print
`Table 14 rank: none (extended tier)`.

**Ranking basis.** Gap ratio can be dominated by cards whose human–AI mean distance is a
fraction of a point. Table 14 rank is the paper's ordering of its own core-*selection* score,
which is not the same thing as how much a feature moves a verdict. A narrative-only
classifier retrained on the authors' released features (macro-F1 0.942 on held-out dev,
validation results Step 3) gives a per-feature importance, and the rewrite
experiment showed it is what matters — volume of change did not predict effect, but which
features moved did (validation results Step 5). Ranking by measured importance puts the
features that carry weight at the top of the report and of the HANDOFF block, which is where
`narrative-humanize` takes its priority from. The gap ratio and the Table 14 rank both remain
printed, so a reader can see all three orderings.

The **gap ratio** is still computed and printed on every TOP AI-SIDE line as
`Gap ratio: [value]`; it is a displayed value, not the sort key. It is defined as:

 gap ratio = |text value − human mean| / |AI mean − human mean|

using the card's `human_mean` and `ai_mean` for the row that fired. A ratio of 1.0 puts the
text at the AI mean; 0 puts it at the human mean; values above 1.0 lie beyond the AI mean.
Print it to two decimals; do not round the means, which are printed as the card gives them
(rule 2; printed gaps in Table 16 are never recomputed, ). The ratio is **undefined**
for cards whose text value is an option rather than a number: the binary NC-04, the
categorical and multi-select cards (NC-05, NC-06, NC-07, NC-10, NC-15, NC-16, NC-17, NC-18,
NC-21, NC-30), and NC-22 / NC-23, whose Table 16 means do not sit on their printed rungs
. Those lines print `Gap ratio: n/d (option card)`. Because the sort key is
`importance_rank`, no ranking convention is needed for them.

LOCATED SPANS lists every firing span of every local card that fired AI-side (not only the
top 5), each with its evidence tag. HUMAN-SIDE FEATURES PRESENT lists every human-side card
with its value, its evidence tag, and its spans: for a local card, its firing spans; for a
global card, the two or three driving spans, never one. HANDOFF TO narrative-humanize
carries one line per AI-side card, all of them, in the fixed five-field form. The location
field is the card's P-locations (a local card's firing spans, a global card's driving
spans) or, on a CT-12 document where the card fires AI-side in some sections only, the
S-range of those sections (e.g. `S2-S3`) . "Direction to move" names the human-side
value or option on the card's own scale and nothing more. It is not an editing
instruction; rewriting is `narrative-humanize`'s job.

**HANDOFF carries both tiers, ordered by `importance_rank` across them**, because that is the
priority order the structural rewrite works in. No tier field is needed to tell them apart:
any id from NC-31 up is extended. A single-row AI-elevated option card that came out
`neutral` is not an AI-side card and does not appear in HANDOFF.

### Step 7 — Paragraph lean map (CT-12 only)

For a CT-12 document, add the PARAGRAPH LEAN MAP in two parts . First the
per-paragraph line: for each paragraph, the count of local cards firing AI-side and
human-side in it. Then one line per lean-map section (the contiguous runs the classifier
cut at rule-4 boundaries), in the fixed form
`S1 (P[a]-P[b]): AI-side [n] of [m] | human-side [n] | neutral [n] | leans [AI | human | mixed] at the structural level`,
where m is the number of cards applicable to that section's `base:` row, the counts include
the global cards rated once for that section, and the lean word is `AI` when the section's
AI-side count exceeds its human-side count, `human` in the reverse case, `mixed` when they
are equal. **The lean word is a comparison of two counts, not a score** (gap ): several cells share evidence — NC-01/NC-21/NC-30 lie on one axis and NC-01/NC-17 count the same sentences in opposite directions — so the margin between the counts carries no magnitude, and no probability, percentage, confidence or ranking may be derived from it. A one-card margin and a ten-card margin both print the same word, and neither is evidence of strength. `content_type_matrix.md` §0.8 forbids the aggregate; this line is the one place the skill comes closest to it, so the constraint is stated here rather than only at Step 8. The section's `base:` row is named in `Classification basis`. No global card is
given a document-level value on a CT-12 document; the STRUCTURAL LEAN totals are section
majorities that point here (Step 6). This map is the discourse-level counterpart of
the word-level layer's AI-edited-fraction overlay (`references/word_level_signals.md`) and, like it, is never combined with the other
report's number.

### Step 8 — No verdict headline

The report never prints "AI" or "Human" as a headline verdict, in any section, in any
example. The paper's classifier is not being run, and an LLM applying a rubric is not that
classifier. The one-sentence summary, printed immediately after the fixed report block,
takes exactly this form:

 Leans AI at the structural level on N of M applicable features.
 — a count of cells that fired, not a detector score. Several cells share evidence
 (NC-01/NC-21/NC-30 lie on one axis; NC-01/NC-17 count the same sentences in opposite
 directions), so N is not proportional to strength of evidence, and no probability,
 percentage or confidence may be derived from N/M.

with N the AI-side count and M the applicable-card count. Where N is zero, the sentence
still prints (with `0 of M`), and TOP AI-SIDE FEATURES reads `none fired`; that is a valid
result, not a failure (UC-01).

---

## Output format (fixed)

**Output format is fixed and lives in `references/output_format.md`.** Read it before
emitting anything. In outline: NARRATIVE-CHECK REPORT header with the classification basis
and confidence cap; TOP AI-SIDE FEATURES; LOCATED SPANS; HUMAN-SIDE FEATURES PRESENT; MODEL
FINGERPRINTS; STRUCTURAL LEAN; HANDOFF block. Every feature line carries its tag slot
(`validated: fiction` / `established: <corpus>` / `analog: unvalidated`). Do not improvise a
layout, reorder the blocks, or merge this report with the word-level one.

## Calibration notes

**Calibration notes are in `references/calibration_notes.md`** — confidence caps, the short-text
and length-gate behaviour, rater-convention effects, and what an AUC in this skill does and does
not mean. Read them before stating a confidence level.

## Use-case coverage

**Use-case coverage (UC-01 … UC-12) is in `references/use_case_matrix.md`.** Look up the row
before deviating from the default check-only behaviour.

## What this skill does NOT do

- It does not run StoryScope's classifier or prove authorship.
- It keeps the discourse and word-level reports separate.
- It does not rewrite; `narrative-humanize` consumes its HANDOFF block.
- It does not transfer fiction evidence to other genres; use each matrix cell's verdict.
- It never guarantees a detector outcome.
- Model-fingerprint attribution is optional and always capped Low.
