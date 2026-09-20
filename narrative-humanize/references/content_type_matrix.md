# Content-type matrix — Matrix 1 (content types × features) and the content-type classifier

Ids and names come from `nc_id_registry.md`; card definitions and scope come from
`feature_cards.md`. StoryScope's published evidence covers fiction only.
## 0. How to read this file

### 0.1 Cell vocabulary

Every cell in Part 1 is exactly one of three forms:

- **`literal`** — the fiction feature applies as written; the card's `fiction_detection` (feature_cards.md) is used unchanged. The report tag is `validated: fiction`. Permitted only where the content type actually contains the narrative element (CT-01 always; CT-02 where scenes and people exist).
- **`analog: <operational definition>. threshold: <verdict>`** — a translated feature. The definition names what to count or look for and gives the **AI-side condition**. **Every number inside an analog definition (one per paragraph, half of sections, ~300 words, 5%) is this build's proposal. None comes from the paper.** The paper reports nothing about any non-fiction genre (findings §E.1) and prints no detection thresholds even for fiction (findings §E.2; feature_cards.md scope note 4). What the closing `threshold:` tag now says is not whether the *number* came from the paper — it never did — but what this project's own measurement found when it tested that condition. The six possible verdicts and the report tag each one carries are in §0.8. A cell tagged `ESTABLISHED` reports as `established: <corpus>`; everything else still reports as `analog: unvalidated`, because nothing short of the four-condition bar licenses a stronger claim.
- **`n/a: <reason>`** — the feature has no meaningful counterpart in this content type. The report prints the reason. "Covers all content types" means every cell has a defined behavior including `n/a`; it does not mean the paper's findings hold for the type (operating rule 3).

The third column (`definition or reason`) carries, for `literal` rows, the report tag and any card-specific note; for `analog` and `n/a` rows, whose definition or reason is already inside the cell, it carries the card's scope as inferred by C1 (`scope: global` or `scope: local`; `FP` shows the 23/5 split). No third-column entry is blank.

### 0.2 Direction convention for analogs

"AI-side" always points the same way as the card's Table 16 `direction` (findings §B). For an **AI-elevated** feature (NC-01 … NC-20 rows in Table 14) the AI-side condition is *presence / high*; for a **Human-elevated** feature (NC-21 … NC-30 rows in Table 15, plus the human-elevated option rows of NC-06, NC-07, NC-17) the AI-side condition is *absence / low*. An analog never reverses the paper's direction. Whether the direction survives translation to a non-fiction genre is exactly what is unvalidated.

### 0.3 Scope and location (rule 4)

Scope per card comes from `feature_cards.md` (C1), which is the **source of truth** — read the file, never a version quoted here ; every scope statement in this file is a copy of it and must be re-derived from it, never edited independently. **global** (30) — NC-01, NC-02, NC-03, NC-05, NC-06, NC-07, NC-08, NC-09, NC-10, NC-11, NC-12, NC-13, NC-14, NC-17, NC-20, NC-24, NC-25, NC-26, NC-27, NC-28, NC-29, NC-30, NC-31, NC-32, NC-34, NC-35, NC-36, NC-37, NC-38, NC-39. **local** (10) — NC-04, NC-15, NC-16, NC-18, NC-19, NC-21, NC-22, NC-23, NC-33, NC-40. Fingerprint cards: 23 global, 5 local (FP-Human-1, FP-Claude-3, FP-DeepSeek-5, FP-Kimi-1, FP-Kimi-2).

**Basis.** All 40 NC cards use `scope_basis: detection_method`. The 28 FP cards still carry `figure-8` (11) or `inferred` (17). None of the 68 is a paper finding .

### 0.4 The fingerprint column (`FP`)

`FP` is the 28-card fingerprint set treated as one unit. It is `literal` on CT-01 only. On every other row it is `analog` or `n/a`, and every such cell carries the note **"UC-05 only; confidence Low; six-way ceiling 68.4 macro-F1 (Table 3, p. 8)"**. Per, UC-05 runs on a row only where this column says `literal` or `analog`; where it says `n/a`, UC-05 returns the cell's reason as its not-applicable sentence. The 68.4 figure is the paper's narrative-only six-way macro-F1 on fiction (findings §A.8); on any non-fiction row even that ceiling is unvalidated.

### 0.5 Length gates (build proposal, not the paper's)

Two word-count gates are used throughout this file and in the classifier (Part 2):

- **Under 800 words:** confidence capped Low; global features are reported as unreliable.
- **Under 300 words:** local cards only; every global card returns `n/a: global rating impossible under 300 words`.

**Neither number comes from the paper.** The paper's corpus averages 4,753 words (fn. 8, p. 3; findings §A.5) and its length audit never goes below its own shortest tertile of ~5,000-word stories (findings §E.5). E1 grepped all 30 pages and found no 800- or 300-word threshold (findings §E.2; E1 worklog item 1). These gates are design choices of this build.

### 0.6 Weighting notes

Where Matrix 1's notes column says certain features "carry most weight" for a row (CT-03, CT-08), that weighting is repeated in the row's header line as a build proposal. The paper ranks features only within fiction (core score, findings §A.6); it says nothing about which features matter for essays or news.

### 0.8 Measured cells, and why they must not be aggregated

**Thirteen of the fifteen rows have now been scored** against purpose-built analog corpora (23 of them, 11 double-rated), and every threshold-bearing cell carries its own verdict. Only CT-12 and CT-13 were never reached. The per-row counts are in each section's generated **Validation** block, and the full reasoning is in `content_type_matrix.md` and validation results.

**17 cells are established, of 519 rated** (558 in total; the 519 excludes a block-split
sub-study that cannot reach the reliability limb). A cell is `ESTABLISHED` only if it clears an exact two-sided permutation test under Benjamini–Hochberg at q = 0.10 with a stratified style control, **and** kappa ≥ .60, **and** AC1 ≥ .60, **and** separates in the direction its card predicts. Everything weaker gets a weaker tag, and the tags are not interchangeable:

| tag | meaning |
|---|---|
| `ESTABLISHED` | all four conditions above met on the named corpus |
| `separates … but its reliability is unestablished` | clears the statistics; its study's reliability estimate is withheld . Neither confirmed nor refuted |
| `NOT SUPPORTED` | measured on the named corpus and did not separate |
| `REFUTED … separates OPPOSITE` | separates significantly in the direction *contrary* to its card. Excluded by rule; never counted as a survivor |
| `NO REFERENT` | the cell never fired on that corpus, so it is untestable there — **not** disproven |
| `unvalidated` | no corpus reached this cell. No evidence either way |

**Genre supersession.** Where a row was first measured on a proxy genre and later re-measured on the genre the row actually names, **the named-genre corpus is authoritative** and the proxy result is historical. This is not bookkeeping: re-measuring four rows on their real genre took CT-11 to 0 of 23, CT-09 to 0 of 10, CT-15 to 0 of 10, and CT-06 to 1 of 16 against 7 of 16 on the wrong genre. Cells validated on a proxy genre mostly do not replicate. Each affected section's Validation block names what it supersedes.

**A measured tag licenses no aggregate.** (Reconciliation, gap : the accounting identity in `SKILL.md` Step 4 is *not* an aggregate in the sense forbidden here — it is a bucket-completeness check. What is forbidden is any inferential quantity built from the counts, including the Step 7 lean sentence being read as a score.) Both raters reported that the CT-04 cells are not independent tests: NC-01, NC-21 and NC-30 collapse onto a single axis — whether the text contains concrete particulars or only claims about itself — and NC-01 and NC-17 count the same sentences in opposite directions. Summing, averaging, or scoring across CT-04 cells therefore double-counts the same evidence, and no consumer of this file may do it. Report the cells individually with their AUCs.

The AUCs are the cells' own separation on that corpus, not a detector accuracy, and they carry every limit the study names: one content type, one AI model, n = 40, one corpus, and rater conventions that move the numbers (NC-19's binary is not exhaustive — the corpus's commonest opening, "This paper presents X", is neither of its two options and the rater's choice moved 11 of 20 documents; NC-02 swings on whether "deeper understanding of the field" counts as societal stakes).

### 0.9 The `not applicable` convention (one-sided conditions)

Several cells state only an AI-side condition, so a document on which that condition *cannot* fire — no attributions to classify, no citation clusters, no second paragraph — would otherwise be scored `human` by default. That is a vacuous verdict: the cell did not observe human behavior, it observed nothing.

**A cell whose AI-side condition cannot fire returns `not applicable`, never `human` and never `AI`.** It is reported as `not applicable: <what was absent>` and is excluded from any listing of cells that fired. This was forced by measurement: on abstract-only input NC-04 never fired on 16 of 20 documents for one rater and 17 of 20 for the other, and NC-03's "AI-side when zero" would have marked the entire corpus AI (validation results Step 6). It applies file-wide, but the cells known to need it are **NC-03, NC-04, NC-06, NC-13 and NC-30** on CT-04, which name the rung explicitly.

### 0.7 The extended tier (NC-31 … NC-40)

The last ten rows of every section are **not** among the paper's 30 core features and no cell may describe them as such. They are features from StoryScope's released 304-feature taxonomy that Table 16 never reported; this project's validation study measured their human/AI separation and classifier weight and carded them as `tier: extended (measured, not in Table 16)` (`feature_cards.md`; baselines, vocabularies, questions, and the authors' detection methods in `feature_cards.md` Part B; rationale in `feature_cards.md` §4). Their baselines are recomputed by this project, not printed in the paper, so operating rule 2 is satisfied by citing the validation study rather than a table.

Two carry standing caveats that the cells below honor:

- **NC-38 (Primary Genre Category)** may be *rated* but is not actionable: changing a text's genre is not a rewrite operation. Every NC-38 cell that rates says "rated only — genre is not a rewrite target", and `narrative-humanize` takes no NC-38 action.
- **NC-32 (Dominant Narrative Tense)** is categorical past/present/future/mixed and was moved by 0 of 10 rewrites in the validation study. Where a content type fixes tense by convention — methods and results in the past, documentation in the imperative present — the cell says so plainly and returns `n/a`, because a convention-forced value carries no authorship signal.

Everything else in §0.1 – §0.6 applies to these ten exactly as to NC-01 … NC-30: the same three cell forms, the same per-cell `threshold:` verdict vocabulary (§0.8) on every analog, the same direction convention (§0.2, read off each card's `direction`, which names the biggest-gap option), and the same scope handling (§0.3).

---

## Part 1 — The matrix, one section per content type

### CT-01 — Short fiction, novel excerpt

**Row rule:** the paper's domain. All 31 cells literal. Report tag `validated: fiction` on every line. Length gates (§0.5) still apply: a 250-word flash piece gets local cards only.

<!-- VALIDATION:BEGIN — generated by _foldin_verdicts.py; do not hand-edit -->
**Validation.** Scored on `analog_CT-01b`. Of 0 threshold-bearing cells: . This row's cells are `literal` (fiction is the paper's own genre), so they carry no threshold clause and the verdict here is row-level. On the crossed corpus the row's failure tracks the GENERATOR, not length: cells sit at chance on 2022-era text (mean AUC 0.475, 18 of 41 pointing the predicted way) and at 0.592 on current-model text (32 of 41; shift +0.117, sign p=0.017), while length moves nothing across a 4x range (-0.002, p=0.74). One cell, NC-03, separates opposite to its card. The paper's published 0-of-40 null was a fact about two 2022 models, not about the features.
<!-- VALIDATION:END -->

| id | cell | definition or reason |
|---|---|---|
| NC-01 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-02 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-03 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-04 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-05 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-06 | literal | card `fiction_detection` unchanged (both option rows); validated: fiction |
| NC-07 | literal | card `fiction_detection` unchanged (both option rows); validated: fiction |
| NC-08 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-09 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-10 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-11 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-12 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-13 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-14 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-15 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-16 | literal | card `fiction_detection` unchanged; validated: fiction. Per, never attributed to Gemini specifically |
| NC-17 | literal | card `fiction_detection` unchanged (both option rows); validated: fiction |
| NC-18 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-19 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-20 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-21 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-22 | literal | card `fiction_detection` unchanged; report shows Table 16 value then §4.1 percentage in parentheses ; validated: fiction |
| NC-23 | literal | card `fiction_detection` unchanged; report shows Table 16 value then §4.1 percentage in parentheses ; validated: fiction |
| NC-24 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-25 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-26 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-27 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-28 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-29 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-30 | literal | card `fiction_detection` unchanged; validated: fiction |
| NC-31 | literal | card `fiction_detection` unchanged; validated: fiction; tier: extended (measured, not in Table 16) |
| NC-32 | literal | card `fiction_detection` unchanged; validated: fiction; tier: extended (measured, not in Table 16) |
| NC-33 | literal | card `fiction_detection` unchanged; validated: fiction; tier: extended (measured, not in Table 16) |
| NC-34 | literal | card `fiction_detection` unchanged; validated: fiction; tier: extended (measured, not in Table 16) |
| NC-35 | literal | card `fiction_detection` unchanged; validated: fiction; tier: extended (measured, not in Table 16) |
| NC-36 | literal | card `fiction_detection` unchanged; validated: fiction; tier: extended (measured, not in Table 16) |
| NC-37 | literal | card `fiction_detection` unchanged; validated: fiction; tier: extended (measured, not in Table 16) |
| NC-38 | literal | card `fiction_detection` unchanged; validated: fiction; tier: extended (measured, not in Table 16); rated only — genre is not a rewrite target |
| NC-39 | literal | card `fiction_detection` unchanged; validated: fiction; tier: extended (measured, not in Table 16) |
| NC-40 | literal | card `fiction_detection` unchanged; validated: fiction; tier: extended (measured, not in Table 16) |
| FP | literal | all 28 fingerprint cards as written; UC-05 only; confidence Low; six-way ceiling 68.4 macro-F1 (Table 3, p. 8); fingerprints belong to five specific model versions (findings §E.12) |

### CT-02 — Narrative nonfiction (memoir, personal essay, reportage with scenes)

**Row rule:** mostly literal because scenes, people, dialogue, and chronology are present; analogs only where the fiction feature presupposes an invented protagonist or where genre convention makes the literal reading misfire (NC-04, NC-12, NC-17, NC-30). Literal cells still carry `validated: fiction`, which is the honest tag: the paper validated these on fiction, and this row is not fiction. The report must say so in its header for every CT-02 run.

<!-- VALIDATION:BEGIN — generated by _foldin_verdicts.py; do not hand-edit -->
**Validation.** Scored on `analog_CT-02`. Of 7 threshold-bearing cells: 7 never rated.
<!-- VALIDATION:END -->

| id | cell | definition or reason |
|---|---|---|
| NC-01 | literal | card `fiction_detection` applied to the piece's stated meaning; validated: fiction (genre differs; header disclaimer) |
| NC-02 | literal | card `fiction_detection`; validated: fiction (genre differs) |
| NC-03 | literal | card `fiction_detection`; validated: fiction (genre differs) |
| NC-04 | analog: reflective commentary is genre-normal in memoir, so count only commentary that states what a scene meant immediately after that scene ("that was the day I learned…"); AI-side when more than half of the scenes are followed by such a statement. threshold: unvalidated — not rated on analog_CT-02; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-05 | literal | reported dialogue is rated by the card's question; validated: fiction (genre differs) |
| NC-06 | literal | card `fiction_detection` (both option rows); validated: fiction (genre differs) |
| NC-07 | literal | card `fiction_detection` (both option rows); validated: fiction (genre differs) |
| NC-08 | literal | card `fiction_detection`; validated: fiction (genre differs) |
| NC-09 | literal | card `fiction_detection`; validated: fiction (genre differs) |
| NC-10 | literal | card `fiction_detection`; validated: fiction (genre differs) |
| NC-11 | literal | card `fiction_detection`; validated: fiction (genre differs) |
| NC-12 | analog: the narrator's own interior is the genre's default vantage, so count only sentences asserting the inner thoughts or feelings of a person other than the narrator without attribution to something they said or did; AI-side when one or more such assertions appear per scene. threshold: unvalidated — not rated on analog_CT-02; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-13 | literal | card `fiction_detection` on the chain of real events as told; validated: fiction (genre differs) |
| NC-14 | literal | card `fiction_detection`; validated: fiction (genre differs) |
| NC-15 | literal | card `fiction_detection`, protagonist = narrator or central real person; validated: fiction (genre differs) |
| NC-16 | literal | card `fiction_detection` on the first appearance of each real person; validated: fiction (genre differs) |
| NC-17 | analog: count secondary threads (another person's story, a second time period, a parallel event) that receive at least one paragraph of their own; AI-side when zero, i.e. the piece follows one thread from start to finish. threshold: unvalidated — not rated on analog_CT-02; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-18 | literal | card `fiction_detection`; validated: fiction (genre differs) |
| NC-19 | literal | card `fiction_detection` on the opening; validated: fiction (genre differs) |
| NC-20 | literal | card `fiction_detection`; validated: fiction (genre differs) |
| NC-21 | literal | card `fiction_detection`; real names, places, works, dates all count as named references; validated: fiction (genre differs) |
| NC-22 | literal | card `fiction_detection`; the narrator acknowledging the act of writing counts as a break; dual value shown; validated: fiction (genre differs) |
| NC-23 | literal | card `fiction_detection`; dual value shown; validated: fiction (genre differs) |
| NC-24 | literal | card `fiction_detection`; validated: fiction (genre differs) |
| NC-25 | literal | card `fiction_detection`; validated: fiction (genre differs) |
| NC-26 | literal | card `fiction_detection`; validated: fiction (genre differs) |
| NC-27 | literal | card `fiction_detection`; validated: fiction (genre differs) |
| NC-28 | literal | card `fiction_detection`; validated: fiction (genre differs) |
| NC-29 | literal | card `fiction_detection`; validated: fiction (genre differs) |
| NC-30 | analog: the "protagonist" is the narrator or the central real person; count sentences that complicate that person's portrayal (a flaw, a wrong turn, a contested motive); AI-side when zero, i.e. the portrayal is uniformly sympathetic or uniformly critical. threshold: unvalidated — not rated on analog_CT-02; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-31 | literal | card `fiction_detection` unchanged; validated: fiction (genre differs); tier: extended (measured, not in Table 16) |
| NC-32 | literal | card `fiction_detection` unchanged; validated: fiction (genre differs); tier: extended (measured, not in Table 16) |
| NC-33 | literal | card `fiction_detection` unchanged; validated: fiction (genre differs); tier: extended (measured, not in Table 16) |
| NC-34 | literal | card `fiction_detection` unchanged; validated: fiction (genre differs); tier: extended (measured, not in Table 16) |
| NC-35 | literal | card `fiction_detection` unchanged; validated: fiction (genre differs); tier: extended (measured, not in Table 16) |
| NC-36 | literal | card `fiction_detection` unchanged; validated: fiction (genre differs); tier: extended (measured, not in Table 16) |
| NC-37 | analog: apply the card's vocabulary to the piece's central real person, but rate only where that person is someone other than the first-person narrator, since a first-person memoirist is an unnamed "I" by genre convention rather than by choice; AI-side when a third-person central subject is referred to throughout by personal name rather than by role or shifting identifiers. threshold: unvalidated — not rated on analog_CT-02; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-38 | analog: assign the card's genre vocabulary to the piece's reader contract; most texts in this row take `nonfictional_mode_pastiche_or_essayistic`, and a piece that instead reads as one of the fiction contracts (historical_realist, thriller_suspense) is the notable case; AI-side on `historical_realist`, the card's biggest-gap option. Rated only — genre is not a rewrite target. threshold: unvalidated — not rated on analog_CT-02; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-39 | analog: count the distinct real people whose inner experience the piece renders directly, as opposed to reporting what they said or did; AI-side when only the narrator's interior is rendered in a piece that has other central figures. threshold: unvalidated — not rated on analog_CT-02; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-40 | literal | card `fiction_detection` unchanged; validated: fiction (genre differs); tier: extended (measured, not in Table 16) |
| FP | analog: apply each fingerprint card's `fiction_detection` to the scene-bearing text only (paragraphs with people, place, and event); cards whose object is absent (no ending, no embedded story) return "no evidence"; UC-05 only; confidence Low; six-way ceiling 68.4 macro-F1 (Table 3, p. 8). threshold: proposed, unvalidated | scope: FP set — 23 global / 5 local (feature_cards.md, basis 11 figure-8 / 17 inferred) |

### CT-03 — Opinion essay / blog post (argument-driven, no scenes)

**Row rule:** analogs throughout; no scenes by definition (a piece with scenes is CT-02). **Weighting (build proposal, Matrix 1 note):** NC-21/NC-06 (named vs vague references) and NC-22/NC-23 (reader address) carry most weight for this row.

<!-- VALIDATION:BEGIN — generated by _foldin_verdicts.py; do not hand-edit -->
**Validation.** Scored on `analog_CT-03`. Of 26 threshold-bearing cells: **2 established**, 2 separating but with reliability unestablished, 20 measured and not supported, 2 with no referent (never fired).
<!-- VALIDATION:END -->

| id | cell | definition or reason |
|---|---|---|
| NC-01 | analog: count sentences that state the thesis or takeaway outright ("the point is", "what this shows is"); AI-side when the thesis is restated at least once in every body section beyond the introduction and conclusion. threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.56, p 0.5589, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-02 | analog: count sentences that lift the topic to a general claim about human nature, society, or meaning ("this is ultimately about what it means to…"); AI-side when at least one appears per section or the conclusion pivots to one in a piece whose topic is practical. threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.50, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-03 | analog: count paragraphs that do not tie back to the thesis (a digression, tangent, or aside left unresolved); AI-side when zero — every paragraph returns to the central claim. threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.47, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-04 | analog: count sentences in which the author steps out of the argument to tell the reader what the preceding evidence means ("in other words, this reminds us that…"); AI-side when present after more than half of the body paragraphs. threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.38, p 0.2003, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-05 | analog: classify each quotation of another person as evidential (a fact, event, or position) or abstract (raises a general question or reflection); AI-side when abstract quotations outnumber evidential ones; does not fire when the piece contains no quotations. Global card: the classification above is the *basis* of one document-level rating plus the 2–3 instances that most drive it — it is never reported as a per-instance location list (§0.3). threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.55, p 0.4872, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-06 | analog: classify each reference to outside work or opinion as named (author, title, outlet, dataset) or vague ("studies show", "experts agree", "it is often said"); AI-side when vague references outnumber named ones. Global card: the classification above is the *basis* of one document-level rating plus the 2–3 instances that most drive it — it is never reported as a per-instance location list (§0.3). threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.55, p 0.6614, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-07 | analog: classify each rendering of emotion as a named label ("I was angry") or a bodily/embodied metaphor ("a gut-punch", "a knot in the collective stomach"); AI-side when embodied renderings outnumber labels; does not fire when no emotion is rendered. Global card: the classification above is the *basis* of one document-level rating plus the 2–3 instances that most drive it — it is never reported as a per-instance location list (§0.3). threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.48, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-08 | n/a: the row has no scenes by definition; environment described to mirror a mood would reclassify the piece as CT-02 | scope: global (feature_cards.md, basis detection_method) |
| NC-09 | n/a: no setting to carry environmental emphasis; topical mentions of nature are content, not a structural choice | scope: global (feature_cards.md, basis detection_method) |
| NC-10 | n/a: sensory imagery in a scene-less argument is prose texture, which is the surface layer (humanize) rather than a narrative choice | scope: global (feature_cards.md, basis detection_method) |
| NC-11 | n/a: same as NC-10; density of sensory description has no structural counterpart without scenes | scope: global (feature_cards.md, basis detection_method) |
| NC-12 | n/a: the author's own interior is the essay's default vantage; there is no character whose depth of access varies | scope: global (feature_cards.md, basis detection_method) |
| NC-13 | analog: count paragraphs that open with a logical connective ("Therefore", "This means", "As a result", "Ultimately") and count counter-considerations raised and left unresolved; AI-side when connectives open at least half of the paragraphs and zero counter-considerations remain unresolved. threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.57, p 0.2308, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-14 | n/a: physical space is not depicted in a scene-less argument | scope: global (feature_cards.md, basis detection_method) |
| NC-15 | analog: read the conclusion for where resolution is located — in the individual reader's choice or will ("we must choose", "it is up to each of us") versus in external or structural forces, or left open; AI-side when the close is an individual-choice exhortation. threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.62, p 0.0471, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-16 | analog: for each person first mentioned, note whether the introduction is a descriptor tag ("a 34-year-old teacher from Ohio", "renowned economist") or something the person said or did; AI-side when descriptor-first introductions outnumber action-first; does not fire when no person is introduced. threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.50, p 0.7538, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-17 | analog: count secondary lines of argument or extended examples that stand on their own rather than being folded back into the thesis; AI-side when zero — a single track from claim to conclusion. threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.65, p 0.0648, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-18 | analog: classify the closing paragraph as a stated realization or reframing ("what we come to see is…") versus a concrete proposal, an open question, or an unresolved tension; AI-side when the close is a realization. threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.57, p 0.5006, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-19 | analog: classify the first sentence as a broad context frame ("In today's world of…", "For decades, X has…") versus a specific claim, fact, or question; AI-side when the opening is a context frame. threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.62, p 0.0471, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-20 | analog: count sentences before the first statement of the problem or tension the essay addresses; AI-side when more than one full paragraph of setup precedes it. threshold: NO REFERENT on analog_CT-03 — the cell never fired, so it is untestable here, not disproven | scope: global (feature_cards.md, basis detection_method) |
| NC-21 | analog: count named specifics — particular works, people, places, brands, dates, figures; AI-side when there are none per ~300 words or references are generic plurals only ("many writers", "some critics"). threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.57, p 0.5145, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-22 | analog: count remarks that acknowledge the essay as an essay or the reader as reading ("bear with me", "I know how this sounds", "you're probably thinking"); AI-side when zero. threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.57, p 0.4075, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-23 | analog: count second-person sentences directed at the reader as a person (a question to them, an imperative, "you" with a specific situation), excluding generic "you" ("you can see that…"); AI-side when zero or formulaic only. threshold: separates on analog_CT-03 (AUC 0.72, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: local (feature_cards.md, basis detection_method) |
| NC-24 | analog: read for any later section that changes the meaning of an earlier one (a concession that reframes the thesis, a reversal, a "but here is what I got wrong"); AI-side when no later section alters the reading of an earlier one. threshold: separates on analog_CT-03 (AUC 0.68, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-25 | analog: rate the piece once for how often it breaks canonical order, using as the basis a count of order-breaking moves (an anecdote from the past inserted mid-argument, a jump ahead to the conclusion, a return to an earlier point) and citing the 2–3 moves that most drive the rating; AI-side when the rating is at the low end — zero moves, strictly context → problem → argument → conclusion. threshold: ESTABLISHED on analog_CT-03 (AUC 0.75, BH PASS at q=.10; kappa 0.82 / AC1 0.89) | scope: global (feature_cards.md, basis detection_method) |
| NC-26 | analog: read for whether the main point is withheld and staged for later disclosure (thesis delayed past the first section) or stated in the opening and never withheld; AI-side when stated up front with no delayed disclosure. threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.55, p 0.4872, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-27 | n/a: the count of order-breaking moves is NC-25; the weight of anachrony has no separate counterpart in a scene-less argument | scope: global (feature_cards.md, basis detection_method) |
| NC-28 | analog: count distinct concrete domains or settings from which examples are drawn (workplaces, countries, historical periods, industries); AI-side when all examples are abstract or come from a single domain. threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.68, p 0.0197, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-29 | analog: estimate the proportion of quoted words (other people's words in quotation marks or block quotes) to the author's own prose; AI-side when quoted material is under ~5% of the piece. threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.55, p 0.4872, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-30 | analog: count sentences conceding merit or complexity to the opposed position, or admitting the author's own uncertainty; AI-side when zero — the stance is uniformly one-sided. threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.65, p 0.1128, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-31 | analog: mark each mode that regularly conveys feeling in the piece — named emotion words, bodily sensation, metaphorical or environmental imagery, actions and choices, quoted speech tone; AI-side when metaphorical or environmental imagery is among the regular modes. threshold: NOT SUPPORTED on analog_CT-03 (AUC 0.47, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-32 | n/a: narrative tense is fixed by the register: an argument is written in the timeless present and narrates no line of events, so the choice is not free | scope: global (feature_cards.md, basis detection_method) |
| NC-33 | analog: count paragraphs that follow the essay's strongest or final argumentative move and add no new reasoning; AI-side when more than one paragraph of wrap-up follows. threshold: NO REFERENT on analog_CT-03 — the cell never fired, so it is untestable here, not disproven | scope: local (feature_cards.md, basis detection_method) |
| NC-34 | n/a: the row depicts no characters; figurative texture in general prose is the surface layer and belongs to humanize, not here | scope: global (feature_cards.md, basis detection_method) |
| NC-35 | analog: note where the reader learns the essay's key context and stakes; AI-side when the context is front-loaded or spread evenly with nothing held back for the final quarter. threshold: ESTABLISHED on analog_CT-03 (AUC 0.80, BH PASS at q=.10; kappa 0.82 / AC1 0.89) | scope: global (feature_cards.md, basis detection_method) |
| NC-36 | n/a: no relationship network is depicted in a scene-less argument | scope: global (feature_cards.md, basis detection_method) |
| NC-37 | n/a: the row has no central character to identify; how referenced people are named is carried by NC-21 | scope: global (feature_cards.md, basis detection_method) |
| NC-38 | n/a: the card's vocabulary is a fiction-genre reader contract; every text in this row takes the same value (a nonfictional, essayistic mode), so the rating carries no signal | scope: global (feature_cards.md, basis detection_method) |
| NC-39 | n/a: no character's inner experience is rendered in a scene-less argument | scope: global (feature_cards.md, basis detection_method) |
| NC-40 | n/a: the row narrates no line of events, so there is no story time in which an ending can sit | scope: local (feature_cards.md, basis detection_method) |
| FP | n/a: fingerprint features (event escalation, gossip as plot mechanism, epilogue endings, focalization, character introduction) presuppose narrated events and characters, which this row has none of; UC-05 only; confidence Low; six-way ceiling 68.4 macro-F1 (Table 3, p. 8) | scope: FP set — 23 global / 5 local (feature_cards.md, basis 11 figure-8 / 17 inferred) |

### CT-04 — Research paper: abstract and introduction

**Row rule:** analogs for over-determination (NC-01–NC-04), vague attribution (NC-06, NC-21), and the framing features (NC-13, NC-17, NC-19, NC-20, NC-30); most narrative-object features n/a. Register-mandated absences (reader address, up-front disclosure) are n/a because absence is a genre requirement, not an authorship signal.

**Corpus (the validation study).** Eleven CT-04 analogs were applied as counting rules by two blind raters to 40 length-matched research abstracts (20 human arXiv, 20 GPT-4, from RAID). The per-cell verdicts below come from the rescored run, not from that first pass — **the early AUCs this paragraph used to quote were superseded** when the corpus was rescored under the exact permutation null with a stratified style control (NC-01 moved 0.81 → 0.72, NC-17 0.87 → 0.82, NC-21 0.93 → 0.88). Of the eleven, only **NC-01 is established**; six clear the statistics but have no reliability estimate. NC-20 was reversed and its direction is corrected below — note that its cell carries the one direction-corrected tag in this file, because the figure the rescoring produces scores the superseded binary rather than the condition the cell now states.

NC-03, NC-04 and NC-13 are structurally inapplicable to abstract-only input and carry an explicit `not applicable` rung; the corpus is a partial confound — this row is defined as abstract **and** introduction, and the corpus was abstracts only, so those three are *untested* here rather than disproven. Scope of the whole result: one content type, one AI model, n = 40, one corpus.

**These cells are not independent; do not aggregate them.** Both raters reported that NC-01, NC-21 and NC-30 collapse onto one underlying axis — whether the text contains concrete particulars or only claims about itself — and that NC-01 and NC-17 count the same sentences in opposite directions. Any score summed over CT-04 cells double-counts (§0.8).

<!-- VALIDATION:BEGIN — generated by _foldin_verdicts.py; do not hand-edit -->
**Validation.** Scored on `analog`. Of 12 threshold-bearing cells: **1 established**, 6 separating but with reliability unestablished, 3 measured and not supported, 1 never rated, 1 carrying a corrected direction.
<!-- VALIDATION:END -->

| id | cell | definition or reason |
|---|---|---|
| NC-01 | analog: count sentences that restate the contribution or main claim ("we show", "our results demonstrate", "this work highlights"); AI-side when the contribution is stated more than twice across abstract plus introduction, or restated at the end of every introduction paragraph. threshold: ESTABLISHED on analog (AUC 0.72, BH PASS at q=.10; kappa 0.80 / AC1 0.89) | scope: global (feature_cards.md, basis detection_method) |
| NC-02 | analog: count sentences that elevate the study to broad societal or philosophical stakes ("with profound implications for", "fundamentally reshapes our understanding of"); AI-side when at least one appears in the abstract or at least one per introduction paragraph. threshold: separates on analog (AUC 0.82, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-03 | analog: **not applicable when the input has no introduction paragraphs** (a standalone abstract is one paragraph, so the count below has no population and the AI-side condition would fire on every document — return `not applicable`, never `AI`). Where an introduction is present: count introduction paragraphs that do not end by tying back to the paper's contribution; AI-side when zero — every paragraph funnels to the contribution. threshold: NOT SUPPORTED on analog (AUC 0.49, p 1.0000, does not clear BH at q=.10) — measured and did not separate — inapplicable to abstract-only input, see the validation study | scope: global (feature_cards.md, basis detection_method) |
| NC-04 | analog: **not applicable when the input contains no citation clusters** (the denominator is undefined; on abstract-only input it went unfired on 16–17 of 20 documents per rater — return `not applicable`, never `human`). Where citations are present: count sentences that gloss the meaning of cited prior work rather than reporting it ("this underscores the need for", "highlighting the importance of"); AI-side when such a gloss follows more than half of the citation clusters. threshold: separates on analog (AUC 0.68, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted — inapplicable to abstract-only input, see the validation study | scope: local (feature_cards.md, basis detection_method) |
| NC-05 | n/a: no dialogue in an abstract or introduction | scope: global (feature_cards.md, basis detection_method) |
| NC-06 | analog: classify each attribution as cited (a specific citation attached to a specific claim) or vague ("prior work has shown", "it is widely recognized", "recent studies suggest" with no citation); AI-side when vague attributions outnumber cited ones. Global card: the classification above is the *basis* of one document-level rating plus the 2–3 instances that most drive it — it is never reported as a per-instance location list (§0.3). threshold: NOT SUPPORTED on analog (AUC 0.66, p 0.0827, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-07 | n/a: emotions are not rendered in a research abstract or introduction | scope: global (feature_cards.md, basis detection_method) |
| NC-08 | n/a: no setting; environment as psychological mirror has no counterpart | scope: global (feature_cards.md, basis detection_method) |
| NC-09 | n/a: mentions of the natural environment are topic content (an ecology paper), not a structural choice | scope: global (feature_cards.md, basis detection_method) |
| NC-10 | n/a: no sensory description in the register | scope: global (feature_cards.md, basis detection_method) |
| NC-11 | n/a: no sensory description in the register | scope: global (feature_cards.md, basis detection_method) |
| NC-12 | n/a: no characters whose interior is accessed | scope: global (feature_cards.md, basis detection_method) |
| NC-13 | analog: **not applicable when the input has fewer than two paragraphs** (a standalone abstract has no paragraph transitions to count — return `not applicable`, never `human`). Where two or more paragraphs are present: count paragraph-opening connectives and check whether the gap statement is followed immediately by the contribution with no acknowledged tension, competing explanation, or open dispute in the field; AI-side when every paragraph transitions with a connective and no tension is left standing. threshold: NOT SUPPORTED on analog (AUC 0.50, p 1.0000, does not clear BH at q=.10) — measured and did not separate — inapplicable to abstract-only input, see the validation study | scope: global (feature_cards.md, basis detection_method) |
| NC-14 | n/a: physical space is not depicted | scope: global (feature_cards.md, basis detection_method) |
| NC-15 | n/a: an abstract or introduction does not resolve anything; the contribution statement is the genre's mandatory "resolution" | scope: local (feature_cards.md, basis detection_method) |
| NC-16 | n/a: no characters are introduced | scope: local (feature_cards.md, basis detection_method) |
| NC-17 | analog: count secondary contributions, side observations, or negative results named in the introduction that are not folded into the main claim; AI-side when zero — one contribution, one track. threshold: separates on analog (AUC 0.82, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-18 | n/a: no resolution in an abstract or introduction | scope: local (feature_cards.md, basis detection_method) |
| NC-19 | analog: classify the first sentence as a broad field frame ("X is a fundamental problem in Y", "In recent years, … has attracted increasing attention") versus a specific observation, number, or claim; AI-side when the opening is a broad frame. threshold: separates on analog (AUC 0.70, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: local (feature_cards.md, basis detection_method) |
| NC-20 | analog: **direction corrected — the AI-side is now the *absence* of background.** Count the background or context sentences that precede the first statement of the gap or problem; AI-side when zero, i.e. the text opens on its own contribution with no situating context. (As originally written the cell called *more* background AI-side; measurement reversed it — human abstracts average 0.70 background sentences against GPT-4's 0.00.) threshold: NOT SUPPORTED as originally printed on analog (AUC 0.47, p 1.0000) — but that figure scores the *superseded* binary, not the corrected zero-background condition stated above, which separates at 0.70 on the same corpus (the validation study). The corrected condition has not been independently rescored; treat it as proposed. On multi-paragraph input it remains unvalidated | scope: global (feature_cards.md, basis detection_method) |
| NC-21 | analog: count named specifics — datasets, systems, benchmarks, prior papers by name, exact numbers; AI-side when generic-plural references ("existing methods", "many approaches") outnumber named ones. threshold: separates on analog (AUC 0.88, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: local (feature_cards.md, basis detection_method) |
| NC-22 | n/a: the register forbids meta-remarks to the reader; "In this paper, we…" is a genre convention, and absence of anything further is required, not a signal | scope: local (feature_cards.md, basis detection_method) |
| NC-23 | n/a: the register forbids second-person address; absence is genre-required | scope: local (feature_cards.md, basis detection_method) |
| NC-24 | n/a: no revelation structure in an abstract or introduction | scope: global (feature_cards.md, basis detection_method) |
| NC-25 | n/a: the order background → gap → contribution is genre-mandated; deviation is a genre violation, not an authorship signal | scope: global (feature_cards.md, basis detection_method) |
| NC-26 | n/a: the abstract must state its finding up front; withholding is a genre violation | scope: global (feature_cards.md, basis detection_method) |
| NC-27 | n/a: no anachrony in the register | scope: global (feature_cards.md, basis detection_method) |
| NC-28 | n/a: no locales | scope: global (feature_cards.md, basis detection_method) |
| NC-29 | n/a: no quoted speech in the register (block quotations are rare and ornamental) | scope: global (feature_cards.md, basis detection_method) |
| NC-30 | analog: count sentences conceding a limit of the contribution, a competing explanation, or merit in the approaches being superseded; AI-side when prior work is framed uniformly negatively and the contribution uniformly positively with no concession. threshold: separates on analog (AUC 0.69, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-31 | n/a: emotions are not rendered in a research abstract or introduction | scope: global (feature_cards.md, basis detection_method) |
| NC-32 | n/a: tense is fixed by the register's conventions (past for what was done, present for what is claimed), so it is not a free choice | scope: global (feature_cards.md, basis detection_method) |
| NC-33 | analog: count paragraphs that follow the contribution statement and add no new claim (roadmap, restatement of results); AI-side when more than one such paragraph follows. threshold: unvalidated — not rated on analog; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-34 | n/a: no characters are depicted | scope: global (feature_cards.md, basis detection_method) |
| NC-35 | n/a: disclosure order is genre-mandated — an abstract must front-load its finding — so the pacing pattern is not a free choice | scope: global (feature_cards.md, basis detection_method) |
| NC-36 | n/a: no relationship network is depicted | scope: global (feature_cards.md, basis detection_method) |
| NC-37 | n/a: no central character is identified | scope: global (feature_cards.md, basis detection_method) |
| NC-38 | n/a: the card's vocabulary is a fiction-genre reader contract; every text in this row takes the same non-fiction value, so the rating carries no signal | scope: global (feature_cards.md, basis detection_method) |
| NC-39 | n/a: no character's inner experience is rendered | scope: global (feature_cards.md, basis detection_method) |
| NC-40 | n/a: no line of narrated events, so there is no story time in which an ending can sit | scope: local (feature_cards.md, basis detection_method) |
| FP | n/a: fingerprint features presuppose narrated events and characters; none exist in an abstract or introduction; UC-05 only; confidence Low; six-way ceiling 68.4 macro-F1 (Table 3, p. 8) | scope: FP set — 23 global / 5 local (feature_cards.md, basis 11 figure-8 / 17 inferred) |

### CT-05 — Research paper: methods and results

**Row rule (Matrix 1, binding):** most cells n/a; only over-determination (NC-01/02/03/04) and named-reference (NC-06/21) analogs.

<!-- VALIDATION:BEGIN — generated by _foldin_verdicts.py; do not hand-edit -->
**Validation.** Scored on `analog_CT-05b`. Of 6 threshold-bearing cells: **1 established**, 1 separating but with reliability unestablished, 3 measured and not supported, 1 with no referent (never fired). Supersedes `analog_CT-05`, which was measured on abstracts rather than the complete Methods+Results this row names. On the named genre all six cells have a real referent; the earlier 0-or-1 denominators were an artifact of the wrong genre.
<!-- VALIDATION:END -->

| id | cell | definition or reason |
|---|---|---|
| NC-01 | analog: count sentences that state what a result "means" or "demonstrates" beyond reporting it; AI-side when more than one interpretive sentence per results paragraph. threshold: ESTABLISHED on analog_CT-05b (AUC 0.67, BH PASS at q=.10; kappa 1.00 / AC1 1.00) | scope: global (feature_cards.md, basis detection_method) |
| NC-02 | analog: count sentences elevating a method or result to broad significance ("this fundamentally", "paves the way for"); AI-side when any appears in methods or more than one appears in results. threshold: NOT SUPPORTED on analog_CT-05b (AUC 0.54, p 0.6098, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-03 | analog: count results paragraphs that end with a sentence tying the result to the paper's main claim; AI-side when every results paragraph does. threshold: NOT SUPPORTED on analog_CT-05b (AUC 0.58, p 0.1104, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-04 | analog: count "this highlights / underscores / suggests the importance of" glosses attached to reported numbers; AI-side when more than half of the reported results carry one. threshold: NO REFERENT on analog_CT-05b — the cell never fired, so it is untestable here, not disproven | scope: local (feature_cards.md, basis detection_method) |
| NC-05 | n/a: no dialogue | scope: global (feature_cards.md, basis detection_method) |
| NC-06 | analog: classify attributions of methods, tools, and prior results as named (citation, version, dataset name) or vague ("standard procedures", "widely used methods", "as is common"); AI-side when vague attributions outnumber named ones. threshold: NOT SUPPORTED on analog_CT-05b (AUC 0.54, p 0.4902, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-07 | n/a: emotions are absent from methods and results | scope: global (feature_cards.md, basis detection_method) |
| NC-08 | n/a: no setting | scope: global (feature_cards.md, basis detection_method) |
| NC-09 | n/a: environment is study content, not a structural choice | scope: global (feature_cards.md, basis detection_method) |
| NC-10 | n/a: no sensory description | scope: global (feature_cards.md, basis detection_method) |
| NC-11 | n/a: no sensory description | scope: global (feature_cards.md, basis detection_method) |
| NC-12 | n/a: no characters | scope: global (feature_cards.md, basis detection_method) |
| NC-13 | n/a: procedural order is genre-mandated; a continuous chain is required, not a signal | scope: global (feature_cards.md, basis detection_method) |
| NC-14 | n/a: physical detail, where present, is method content (apparatus), not depiction | scope: global (feature_cards.md, basis detection_method) |
| NC-15 | n/a: no resolution | scope: local (feature_cards.md, basis detection_method) |
| NC-16 | n/a: no characters are introduced | scope: local (feature_cards.md, basis detection_method) |
| NC-17 | n/a: methods and results are single-track by genre requirement | scope: global (feature_cards.md, basis detection_method) |
| NC-18 | n/a: no resolution | scope: local (feature_cards.md, basis detection_method) |
| NC-19 | n/a: the opening (participants, materials, setup) is template-mandated | scope: local (feature_cards.md, basis detection_method) |
| NC-20 | n/a: no jeopardy or investment | scope: global (feature_cards.md, basis detection_method) |
| NC-21 | analog: count named specifics — instruments, software and versions, dataset names, parameter values, exact figures; AI-side when specifics are replaced by generic descriptors ("a standard dataset", "appropriate parameters", "commonly used settings"). threshold: separates on analog_CT-05b (AUC 0.69, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: local (feature_cards.md, basis detection_method) |
| NC-22 | n/a: register forbids reader-directed meta-remarks | scope: local (feature_cards.md, basis detection_method) |
| NC-23 | n/a: register forbids second-person address | scope: local (feature_cards.md, basis detection_method) |
| NC-24 | n/a: no revelation structure | scope: global (feature_cards.md, basis detection_method) |
| NC-25 | n/a: order is genre-mandated | scope: global (feature_cards.md, basis detection_method) |
| NC-26 | n/a: order is genre-mandated | scope: global (feature_cards.md, basis detection_method) |
| NC-27 | n/a: no anachrony | scope: global (feature_cards.md, basis detection_method) |
| NC-28 | n/a: no locales | scope: global (feature_cards.md, basis detection_method) |
| NC-29 | n/a: no quoted speech | scope: global (feature_cards.md, basis detection_method) |
| NC-30 | n/a: evaluative stance is excluded from methods and results by genre; it belongs to the discussion (CT-06) | scope: global (feature_cards.md, basis detection_method) |
| NC-31 | n/a: emotions are absent from methods and results | scope: global (feature_cards.md, basis detection_method) |
| NC-32 | n/a: tense is fixed by convention — procedures and results are reported in the past tense — so the choice is not free | scope: global (feature_cards.md, basis detection_method) |
| NC-33 | n/a: methods and results have no climactic event and therefore no denouement; outside the row's analog set | scope: local (feature_cards.md, basis detection_method) |
| NC-34 | n/a: no characters are depicted | scope: global (feature_cards.md, basis detection_method) |
| NC-35 | n/a: presentation order is genre-mandated; outside the row's analog set | scope: global (feature_cards.md, basis detection_method) |
| NC-36 | n/a: no relationship network is depicted | scope: global (feature_cards.md, basis detection_method) |
| NC-37 | n/a: no central character is identified | scope: global (feature_cards.md, basis detection_method) |
| NC-38 | n/a: the card's vocabulary is a fiction-genre reader contract; every text in this row takes the same non-fiction value, so the rating carries no signal | scope: global (feature_cards.md, basis detection_method) |
| NC-39 | n/a: no character's inner experience is rendered | scope: global (feature_cards.md, basis detection_method) |
| NC-40 | n/a: no line of narrated events, so there is no story time in which an ending can sit | scope: local (feature_cards.md, basis detection_method) |
| FP | n/a: fingerprint features presuppose narrated events and characters; none exist in methods or results; UC-05 only; confidence Low; six-way ceiling 68.4 macro-F1 (Table 3, p. 8) | scope: FP set — 23 global / 5 local (feature_cards.md, basis 11 figure-8 / 17 inferred) |

### CT-06 — Research paper: discussion, limitations, conclusion

**Row rule:** analogs for tidy resolution (NC-15, NC-18, NC-30) and single-track (NC-13, NC-17), plus over-determination and named-reference analogs; narrative-object features n/a.

<!-- VALIDATION:BEGIN — generated by _foldin_verdicts.py; do not hand-edit -->
**Validation.** Scored on `analog_CT-06b`. Of 16 threshold-bearing cells: 1 separating but with reliability unestablished, 15 measured and not supported. Supersedes `analog_CT-06`, which was measured on peer reviews rather than the Discussion/Limitations this row names. 7 of 16 cells appeared to survive there; six of the seven do not replicate. NC-19 and NC-24 separate in the wrong direction (p<0.05) on the named genre and are excluded by rule.
<!-- VALIDATION:END -->

| id | cell | definition or reason |
|---|---|---|
| NC-01 | analog: count sentences restating what the main finding means; AI-side when the main takeaway is restated more than twice across the section. threshold: NOT SUPPORTED on analog_CT-06b (AUC 0.46, p 0.7712, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-02 | analog: count sentences lifting the findings to broad significance ("implications for society", "fundamentally changes how we think about"); AI-side when at least one appears per conclusion paragraph. threshold: NOT SUPPORTED on analog_CT-06b (AUC 0.58, p 0.3487, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-03 | analog: count discussion paragraphs that end by tying back to the main claim; AI-side when all of them do. threshold: NOT SUPPORTED on analog_CT-06b (AUC 0.52, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-04 | analog: count "this suggests / highlights the importance of" glosses on discussed results; AI-side when more than half of the paragraphs carry one. threshold: separates on analog_CT-06b (AUC 0.75, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: local (feature_cards.md, basis detection_method) |
| NC-05 | n/a: no dialogue | scope: global (feature_cards.md, basis detection_method) |
| NC-06 | analog: classify attributions of comparison work as cited or vague ("previous studies", "it has been argued"); AI-side when vague outnumber cited. threshold: NOT SUPPORTED on analog_CT-06b (AUC 0.46, p 0.4902, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-07 | n/a: emotions are not rendered in the register | scope: global (feature_cards.md, basis detection_method) |
| NC-08 | n/a: no setting | scope: global (feature_cards.md, basis detection_method) |
| NC-09 | n/a: environment is study content, not a structural choice | scope: global (feature_cards.md, basis detection_method) |
| NC-10 | n/a: no sensory description | scope: global (feature_cards.md, basis detection_method) |
| NC-11 | n/a: no sensory description | scope: global (feature_cards.md, basis detection_method) |
| NC-12 | n/a: no characters | scope: global (feature_cards.md, basis detection_method) |
| NC-13 | analog: count limitations or contradictory results that are stated and left standing versus those immediately neutralized ("however, this is unlikely to affect…"); AI-side when every limitation is neutralized in the same paragraph and no tension remains. threshold: NOT SUPPORTED on analog_CT-06b (AUC 0.50, p 0.7399, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-14 | n/a: physical space is not depicted | scope: global (feature_cards.md, basis detection_method) |
| NC-15 | analog: read how open issues are resolved — by the authors' own promised action ("we will address this in future work") versus by external constraint or left open; AI-side when every limitation is paired with a future-work promise. threshold: NOT SUPPORTED on analog_CT-06b (AUC 0.51, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-16 | n/a: no characters are introduced | scope: local (feature_cards.md, basis detection_method) |
| NC-17 | analog: count secondary strands — unexpected results, side observations, alternative explanations — that receive a paragraph of their own; AI-side when zero. threshold: NOT SUPPORTED on analog_CT-06b (AUC 0.56, p 0.5551, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-18 | analog: classify the closing as a stated synthesis or realization ("taken together, our findings reveal…") versus a concrete open question or unresolved contradiction; AI-side when the close is a synthesis. threshold: NOT SUPPORTED on analog_CT-06b (AUC 0.38, p 0.0828, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-19 | analog: classify the discussion's first sentence as a restatement of the study's aim or context ("In this study, we set out to…") versus a direct statement of a finding; AI-side when the opening restates context. threshold: NOT SUPPORTED on analog_CT-06b (AUC 0.33, p 0.0245, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-20 | analog: count sentences of recap before the first limitation or tension is raised; AI-side when more than one paragraph of recap precedes it. threshold: NOT SUPPORTED on analog_CT-06b (AUC 0.50, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-21 | analog: count named specifics — particular comparison studies, numbers, datasets, effect sizes; AI-side when comparisons are generic ("prior methods", "other approaches"). threshold: NOT SUPPORTED on analog_CT-06b (AUC 0.44, p 0.3497, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-22 | n/a: register forbids reader-directed meta-remarks | scope: local (feature_cards.md, basis detection_method) |
| NC-23 | n/a: register forbids second-person address | scope: local (feature_cards.md, basis detection_method) |
| NC-24 | analog: read whether any limitation or discussion point forces reinterpretation of an earlier result (states that a result may not mean what the results section implied); AI-side when limitations are listed without changing the reading of any result. threshold: NOT SUPPORTED on analog_CT-06b (AUC 0.40, p 0.0431, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-25 | n/a: results precede discussion by genre mandate | scope: global (feature_cards.md, basis detection_method) |
| NC-26 | n/a: disclosure order (results before discussion) is genre-mandated | scope: global (feature_cards.md, basis detection_method) |
| NC-27 | n/a: no anachrony | scope: global (feature_cards.md, basis detection_method) |
| NC-28 | n/a: no locales | scope: global (feature_cards.md, basis detection_method) |
| NC-29 | n/a: no quoted speech | scope: global (feature_cards.md, basis detection_method) |
| NC-30 | analog: count sentences conceding that an alternative interpretation or a rival method could be correct; AI-side when the stance toward the authors' own findings is uniformly positive and toward rivals uniformly negative (limitations present but none conceding a rival). threshold: NOT SUPPORTED on analog_CT-06b (AUC 0.46, p 0.6680, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-31 | n/a: emotions are not rendered in the register | scope: global (feature_cards.md, basis detection_method) |
| NC-32 | n/a: tense is fixed by convention (past for what was found, present for what is claimed), so the choice is not free | scope: global (feature_cards.md, basis detection_method) |
| NC-33 | analog: count paragraphs that follow the paper's main conclusion statement and add no new content (future work, broader impact, a second restatement); AI-side when more than one such paragraph follows. threshold: NOT SUPPORTED on analog_CT-06b (AUC 0.54, p 0.6098, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-34 | n/a: no characters are depicted | scope: global (feature_cards.md, basis detection_method) |
| NC-35 | n/a: disclosure order is genre-mandated (results precede discussion), so the pacing pattern is not a free choice | scope: global (feature_cards.md, basis detection_method) |
| NC-36 | n/a: no relationship network is depicted | scope: global (feature_cards.md, basis detection_method) |
| NC-37 | n/a: no central character is identified | scope: global (feature_cards.md, basis detection_method) |
| NC-38 | n/a: the card's vocabulary is a fiction-genre reader contract; every text in this row takes the same non-fiction value, so the rating carries no signal | scope: global (feature_cards.md, basis detection_method) |
| NC-39 | n/a: no character's inner experience is rendered | scope: global (feature_cards.md, basis detection_method) |
| NC-40 | analog: read where the closing leaves the reader relative to the work reported: stopping at the stated finding and its limits, versus a jump to a distant-future vantage surveying long-term prospects ("in the coming decades this will transform…"); AI-side on the distant-future vantage. threshold: NOT SUPPORTED on analog_CT-06b (AUC 0.52, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| FP | n/a: fingerprint features presuppose narrated events and characters; none exist in a discussion or conclusion; UC-05 only; confidence Low; six-way ceiling 68.4 macro-F1 (Table 3, p. 8) | scope: FP set — 23 global / 5 local (feature_cards.md, basis 11 figure-8 / 17 inferred) |

### CT-07 — Technical documentation, tutorials, READMEs

**Row rule (Matrix 1, binding):** analogs limited to reader address (NC-22/23), single-track (NC-13/17), and the over-determination group (NC-01–NC-04, NC-06; NC-05 is in that group but has no dialogue counterpart). Everything else n/a. Code fences route the input through CT-13 first (Part 2, rule 2).

<!-- VALIDATION:BEGIN — generated by _foldin_verdicts.py; do not hand-edit -->
**Validation.** Scored on `analog_CT-07b`. Of 9 threshold-bearing cells: 1 separating but with reliability unestablished, 8 measured and not supported.
<!-- VALIDATION:END -->

| id | cell | definition or reason |
|---|---|---|
| NC-01 | analog: count sentences stating the purpose or benefit of a step that the step itself already makes obvious ("This ensures your environment is properly configured"); AI-side when above one such sentence per step. threshold: NOT SUPPORTED on analog_CT-07b (AUC 0.55, p 0.4872, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-02 | analog: count sentences elevating a tool or practice to a principle or value ("good documentation is the foundation of…", "this reflects best practices"); AI-side when any appears outside an explicitly motivational section. threshold: NOT SUPPORTED on analog_CT-07b (AUC 0.53, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-03 | analog: count sections that end by tying back to the overall goal ("Now you have a solid foundation for…"); AI-side when every section does. threshold: NOT SUPPORTED on analog_CT-07b (AUC 0.60, p 0.1060, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-04 | analog: count sentences that tell the reader what they have learned or why it matters after a step ("Congratulations! You've now…", "This is important because…"); AI-side when more than half of the steps carry one. threshold: NOT SUPPORTED on analog_CT-07b (AUC 0.64, p 0.0958, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-05 | n/a: no dialogue in documentation | scope: global (feature_cards.md, basis detection_method) |
| NC-06 | analog: classify references to outside resources as named (linked URL, exact document section, version number) or vague ("refer to the official documentation", "consult relevant resources"); AI-side when vague outnumber named. threshold: NOT SUPPORTED on analog_CT-07b (AUC 0.40, p 0.2557, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-07 | n/a: emotions are not rendered in documentation | scope: global (feature_cards.md, basis detection_method) |
| NC-08 | n/a: no setting | scope: global (feature_cards.md, basis detection_method) |
| NC-09 | n/a: no setting | scope: global (feature_cards.md, basis detection_method) |
| NC-10 | n/a: no sensory description | scope: global (feature_cards.md, basis detection_method) |
| NC-11 | n/a: no sensory description | scope: global (feature_cards.md, basis detection_method) |
| NC-12 | n/a: no characters | scope: global (feature_cards.md, basis detection_method) |
| NC-13 | analog: count steps whose transition is an explicit connective ("Next,", "Once this is done,", "With that in place,") and count acknowledged failure paths or branches ("if X fails", "alternatively", "on Windows"); AI-side when every step transitions with a connective and zero branches or failure modes appear. threshold: NOT SUPPORTED on analog_CT-07b (AUC 0.55, p 0.4872, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-14 | n/a: physical space is not depicted | scope: global (feature_cards.md, basis detection_method) |
| NC-15 | n/a: outside the row's analog set; a tutorial has no resolution to attribute | scope: local (feature_cards.md, basis detection_method) |
| NC-16 | n/a: no characters are introduced | scope: local (feature_cards.md, basis detection_method) |
| NC-17 | analog: count side-tracks — alternatives, edge cases, platform variations, troubleshooting — that receive a subsection of their own; AI-side when zero. threshold: separates on analog_CT-07b (AUC 0.72, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-18 | n/a: outside the row's analog set; closing sections in documentation are template-driven | scope: local (feature_cards.md, basis detection_method) |
| NC-19 | n/a: outside the row's analog set; openings in documentation are template-driven (title, purpose, prerequisites) | scope: local (feature_cards.md, basis detection_method) |
| NC-20 | n/a: no jeopardy or investment | scope: global (feature_cards.md, basis detection_method) |
| NC-21 | n/a: named-reference behavior for this row is carried by NC-06; counting it twice would double-weight one observation | scope: local (feature_cards.md, basis detection_method) |
| NC-22 | analog: count remarks acknowledging the document as a document or the reader's likely state ("if you're impatient, skip to…", "this part is confusing, bear with me", "you may be wondering why"); AI-side when zero. threshold: NOT SUPPORTED on analog_CT-07b (AUC 0.53, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-23 | analog: documentation addresses the reader in second person by convention, so count only reader-specific address — anticipating the reader's particular situation or asking them a question — and not imperative steps ("Run the following"); AI-side when zero reader-specific address. threshold: NOT SUPPORTED on analog_CT-07b (AUC 0.57, p 0.2308, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-24 | n/a: no revelation structure | scope: global (feature_cards.md, basis detection_method) |
| NC-25 | n/a: step order is task-mandated | scope: global (feature_cards.md, basis detection_method) |
| NC-26 | n/a: withholding information is a defect in documentation, not a choice | scope: global (feature_cards.md, basis detection_method) |
| NC-27 | n/a: no anachrony | scope: global (feature_cards.md, basis detection_method) |
| NC-28 | n/a: no locales | scope: global (feature_cards.md, basis detection_method) |
| NC-29 | n/a: no quoted speech | scope: global (feature_cards.md, basis detection_method) |
| NC-30 | n/a: outside the row's analog set; documentation is not an evaluative stance toward a protagonist | scope: global (feature_cards.md, basis detection_method) |
| NC-31 | n/a: emotions are not rendered in documentation | scope: global (feature_cards.md, basis detection_method) |
| NC-32 | n/a: tense is fixed by convention — steps are imperative and behavior is described in the present — so the choice is not free | scope: global (feature_cards.md, basis detection_method) |
| NC-33 | n/a: outside the row's analog set (reader address, single-track, over-determination); a task document has no climactic event | scope: local (feature_cards.md, basis detection_method) |
| NC-34 | n/a: no characters are depicted | scope: global (feature_cards.md, basis detection_method) |
| NC-35 | n/a: withholding information is a defect in documentation, not a pacing choice | scope: global (feature_cards.md, basis detection_method) |
| NC-36 | n/a: no relationship network is depicted | scope: global (feature_cards.md, basis detection_method) |
| NC-37 | n/a: no central character is identified | scope: global (feature_cards.md, basis detection_method) |
| NC-38 | n/a: the card's vocabulary is a fiction-genre reader contract; every text in this row takes the same non-fiction value, so the rating carries no signal | scope: global (feature_cards.md, basis detection_method) |
| NC-39 | n/a: no character's inner experience is rendered | scope: global (feature_cards.md, basis detection_method) |
| NC-40 | n/a: no line of narrated events, so there is no story time in which an ending can sit | scope: local (feature_cards.md, basis detection_method) |
| FP | n/a: fingerprint features presuppose narrated events and characters; none exist in documentation; UC-05 only; confidence Low; six-way ceiling 68.4 macro-F1 (Table 3, p. 8) | scope: FP set — 23 global / 5 local (feature_cards.md, basis 11 figure-8 / 17 inferred) |

### CT-08 — Journalism / news feature

**Row rule:** analogs throughout, because features carry scenes, people, quotes, and chronology while being non-fiction with genre-specific conventions (lede, nut graf, attribution). **Weighting (build proposal, Matrix 1 note):** named sources (NC-21, NC-06), chronological ordering (NC-25), and reader address (NC-22/23) carry most weight.

<!-- VALIDATION:BEGIN — generated by _foldin_verdicts.py; do not hand-edit -->
**Validation.** Scored on `analog_CT-08`. Of 39 threshold-bearing cells: **7 established**, 11 separating but with reliability unestablished, 19 measured and not supported, 2 with no referent (never fired).
<!-- VALIDATION:END -->

| id | cell | definition or reason |
|---|---|---|
| NC-01 | analog: count sentences in the reporter's voice that state the story's significance outright ("the case underscores a broader trend"); AI-side when the significance is restated more than once after the nut graf. threshold: separates on analog_CT-08 (AUC 0.93, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-02 | analog: count sentences lifting the story to moral or philosophical stakes in the reporter's own voice (not inside a quote); AI-side when any appears outside quotations. threshold: ESTABLISHED on analog_CT-08 (AUC 0.68, BH PASS at q=.10; kappa 0.75 / AC1 0.90) | scope: global (feature_cards.md, basis detection_method) |
| NC-03 | analog: count paragraphs that do not serve the story's angle (tangential detail, color, a digression); AI-side when zero. threshold: separates on analog_CT-08 (AUC 0.82, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-04 | analog: count interpretive glosses in the reporter's voice following a quote or fact ("a reminder that…", "a sign of…"); AI-side when present after more than half of the quotes. threshold: separates on analog_CT-08 (AUC 0.75, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: local (feature_cards.md, basis detection_method) |
| NC-05 | analog: classify each quote as factual or testimonial (what happened, what was said, what someone wants) or abstract reflection ("it makes you think about what community means"); AI-side when abstract quotes outnumber factual ones. Global card: the classification above is the *basis* of one document-level rating plus the 2–3 instances that most drive it — it is never reported as a per-instance location list (§0.3). threshold: ESTABLISHED on analog_CT-08 (AUC 0.80, BH PASS at q=.10; kappa 1.00 / AC1 1.00) | scope: global (feature_cards.md, basis detection_method) |
| NC-06 | analog: classify each attribution as named (person with name and title, organization, document) or vague ("experts say", "officials", "critics argue" with no name); AI-side when vague attributions outnumber named. Global card: the classification above is the *basis* of one document-level rating plus the 2–3 instances that most drive it — it is never reported as a per-instance location list (§0.3). threshold: ESTABLISHED on analog_CT-08 (AUC 0.78, BH PASS at q=.10; kappa 0.80 / AC1 0.89) | scope: global (feature_cards.md, basis detection_method) |
| NC-07 | analog: classify each rendering of a subject's emotion as a named label ("she was devastated") or an embodied metaphor in the reporter's voice ("grief hung in the room like smoke"); AI-side when embodied renderings outnumber labels. Global card: the classification above is the *basis* of one document-level rating plus the 2–3 instances that most drive it — it is never reported as a per-instance location list (§0.3). threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.53, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-08 | analog: count setting descriptions in the reporter's voice used to mirror a subject's mood (weather for grief, clutter for chaos); AI-side when any appears. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.53, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-09 | analog: count references to weather, landscape, or the natural environment not required by the story's subject; AI-side when at least one per scene paragraph. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.53, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-10 | analog: rate the story once for whether smell and bodily sensation are among its most frequently engaged modalities, using as the basis a count of such details in the reporter's voice and citing the 2–3 that most drive the rating; AI-side when olfactory or bodily detail is present at all in a story whose subject is not sensory. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.45, p 0.4872, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-11 | analog: count sensory-descriptive clauses per scene paragraph; AI-side when above one per scene paragraph. threshold: separates on analog_CT-08 (AUC 0.67, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-12 | analog: count sentences asserting a subject's inner thoughts or feelings without attribution to a quote, "said", or an observed action; AI-side when at least one unattributed interior assertion appears. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.57, p 0.4801, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-13 | analog: count paragraphs opening with a connective and count contradictions between sources left standing; AI-side when every transition is a connective and no contradiction remains unresolved. threshold: NO REFERENT on analog_CT-08 — the cell never fired, so it is untestable here, not disproven | scope: global (feature_cards.md, basis detection_method) |
| NC-14 | analog: count scene paragraphs that give object-level physical detail (the specific mug, the peeling paint); AI-side when every scene paragraph does. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.63, p 0.1311, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-15 | analog: read the ending for whether the outcome is attributed to the central subject's own decision ("she chose to…") versus an external event, institution, or unresolved process; AI-side when the close is protagonist-choice. threshold: separates on analog_CT-08 (AUC 0.68, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: local (feature_cards.md, basis detection_method) |
| NC-16 | analog: note whether the central subject is first introduced by a descriptor list ("a soft-spoken 52-year-old nurse and mother of three") or by something they did or said; AI-side when descriptor-first. threshold: ESTABLISHED on analog_CT-08 (AUC 0.72, BH PASS at q=.10; kappa 0.65 / AC1 0.78) | scope: local (feature_cards.md, basis detection_method) |
| NC-17 | analog: count secondary threads (another person, another angle) that receive paragraphs of their own; AI-side when zero. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.55, p 0.4872, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-18 | analog: classify the ending as a stated realization or emotional resolution ("has found peace", "now understands") versus a concrete pending action or open question; AI-side when the close is internal resolution. threshold: ESTABLISHED on analog_CT-08 (AUC 0.80, BH PASS at q=.10; kappa 0.83 / AC1 0.89) | scope: local (feature_cards.md, basis detection_method) |
| NC-19 | analog: classify the lede as a fully grounded scene frame ("On a gray morning in the back room of…") versus a fact, event, or quote; AI-side when the lede is a full spatial frame. threshold: NO REFERENT on analog_CT-08 — the cell never fired, so it is untestable here, not disproven | scope: local (feature_cards.md, basis detection_method) |
| NC-20 | analog: count paragraphs of background or character setup before the central conflict or event is stated; AI-side when more than two. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.47, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-21 | analog: count named specifics — people with full names and titles, documents, dates, figures, places; AI-side when fewer than one named specific per paragraph or when anonymous plurals dominate. threshold: separates on analog_CT-08 (AUC 0.68, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: local (feature_cards.md, basis detection_method) |
| NC-22 | analog: count acknowledgments of the reporting act or the reader ("I asked", "readers may recall", "as this newspaper reported"); AI-side when zero. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.57, p 0.3416, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-23 | analog: count second-person sentences directed at the reader ("if you have driven this road, you know…"); AI-side when zero. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.55, p 0.6050, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-24 | analog: read for a later paragraph that changes the meaning of an earlier one (a source's account contradicted, a document that reverses the framing); AI-side when none. threshold: ESTABLISHED on analog_CT-08 (AUC 0.72, BH PASS at q=.10; kappa 1.00 / AC1 1.00) | scope: global (feature_cards.md, basis detection_method) |
| NC-25 | analog: rate the story once for how often it jumps in time, using as the basis a count of time jumps between consecutive paragraphs after the lede (a move backward or forward relative to the preceding paragraph) and citing the 2–3 jumps that most drive the rating; AI-side when the rating is at the low end — zero jumps, a strictly chronological account. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.55, p 0.4872, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-26 | analog: read for a key fact withheld past the nut graf and disclosed later for effect; AI-side when nothing is withheld — all key facts are front-loaded. threshold: separates on analog_CT-08 (AUC 0.80, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-27 | analog: estimate the share of paragraphs that are background or flashback relative to present-time paragraphs; AI-side when below ~20%. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.57, p 0.5006, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-28 | analog: count distinct physical locales in which scenes occur; AI-side when one. threshold: separates on analog_CT-08 (AUC 0.79, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-29 | analog: estimate the proportion of quoted speech to reporter prose; AI-side when quotes are under ~15% of the words. threshold: ESTABLISHED on analog_CT-08 (AUC 0.75, BH PASS at q=.10; kappa 1.00 / AC1 1.00) | scope: global (feature_cards.md, basis detection_method) |
| NC-30 | analog: count sentences complicating the central subject's portrayal (a flaw, a contested claim, an opposing view given weight); AI-side when the portrayal is uniformly sympathetic or uniformly critical. threshold: separates on analog_CT-08 (AUC 0.72, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-31 | analog: mark each mode that regularly conveys a subject's feeling — named emotion words, bodily sensation, metaphorical or environmental imagery in the reporter's voice, actions and choices, quoted speech tone; AI-side when metaphorical or environmental imagery is among the regular modes. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.55, p 0.4872, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-32 | analog: identify the tense of most narrative sentences outside quotations; AI-side when uniformly past with no present-tense scene work, noting that hard news is past by convention so the rating carries little signal outside features. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.50, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-33 | analog: locate the story's central event or turn, then count the paragraphs that follow it; AI-side when more than a short aftermath follows — multiple paragraphs of consequence, reflection, or where-they-are-now. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.47, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-34 | analog: count metaphors and similes used to render people and their inner states in the reporter's voice, per character-descriptive paragraph; AI-side when above one per such paragraph. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.57, p 0.2308, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-35 | analog: note where the reader learns the most about the underlying situation; AI-side when it is spread evenly or front-loaded with nothing clarifying held for the final quarter, noting that the inverted pyramid front-loads by convention in hard news. threshold: separates on analog_CT-08 (AUC 0.88, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-36 | analog: note whether relationships between people are mainly summarized by the reporter ("X, who runs the shelter where Y volunteers") or shown through dialogue and action in scenes; AI-side when scenes dominate and no summary enumeration appears. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.50, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-37 | analog: note how the central subject is usually referred to: personal name, a role or descriptor ("the mother", "the sergeant"), or shifting identifiers; AI-side on a personal name used throughout. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.57, p 0.5231, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-38 | n/a: the card's vocabulary is a fiction-genre reader contract; every text in this row takes the same non-fiction value, so the rating carries no signal | scope: global (feature_cards.md, basis detection_method) |
| NC-39 | analog: count the distinct people whose inner experience the piece renders directly rather than attributing to a quote or an observed action; AI-side when exactly one. threshold: NOT SUPPORTED on analog_CT-08 (AUC 0.45, p 0.7512, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-40 | analog: locate the last narrated scene relative to the central event: at or just after it, a brief look ahead, stopping short while consequences are pending, or a distant-future vantage summarizing long-term outcomes; AI-side on the distant-future vantage. threshold: separates on analog_CT-08 (AUC 0.71, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: local (feature_cards.md, basis detection_method) |
| FP | analog: apply each fingerprint card's `fiction_detection` to scene-bearing paragraphs only; cards whose object is absent return "no evidence"; UC-05 only; confidence Low; six-way ceiling 68.4 macro-F1 (Table 3, p. 8). threshold: proposed, unvalidated | scope: FP set — 23 global / 5 local (feature_cards.md, basis 11 figure-8 / 17 inferred) |

### CT-09 — Marketing / product copy / landing pages

**Row rule (Matrix 1, binding):** over-determination (NC-01–NC-04, NC-06) and tidy-resolution (NC-15/18/30) analogs; most others n/a.

<!-- VALIDATION:BEGIN — generated by _foldin_verdicts.py; do not hand-edit -->
**Validation.** Scored on `analog_CT-09b`. Of 10 threshold-bearing cells: 10 measured and not supported. Supersedes `analog_CT-09`, which was measured on government visitor copy rather than the commercial marketing this row names. On 18 real vendor pages archived 2015-2020, no cell survives; the highest is NC-02 at 0.62.
<!-- VALIDATION:END -->

| id | cell | definition or reason |
|---|---|---|
| NC-01 | analog: count sentences stating the value proposition outright; AI-side when it is restated more than once per section beyond the headline. threshold: NOT SUPPORTED on analog_CT-09b (AUC 0.52, p 0.7244, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-02 | analog: count sentences elevating the product to a mission, value, or philosophy ("we believe", "reimagining what's possible"); AI-side when at least one per section. threshold: NOT SUPPORTED on analog_CT-09b (AUC 0.62, p 0.0230, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-03 | analog: count sentences or blocks not tied to the value proposition (a joke, an aside, a specific anecdote that does not sell); AI-side when zero. threshold: NOT SUPPORTED on analog_CT-09b (AUC 0.53, p 0.5808, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-04 | analog: count sentences telling the reader what a feature means for them directly after the feature ("so you can focus on what matters"); AI-side when more than half of the feature lines carry one. threshold: NOT SUPPORTED on analog_CT-09b (AUC 0.54, p 0.5450, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-05 | n/a: testimonials are quotes whose function is fixed by the genre; there is no dialogue whose function varies | scope: global (feature_cards.md, basis detection_method) |
| NC-06 | analog: classify social proof as named (client name, verifiable number, cited source) or vague ("trusted by industry leaders", "thousands of teams"); AI-side when vague outnumber named. threshold: NOT SUPPORTED on analog_CT-09b (AUC 0.50, p 0.9993, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-07 | n/a: emotional appeals in copy are conventional and not renderings of a character's emotion | scope: global (feature_cards.md, basis detection_method) |
| NC-08 | n/a: no setting | scope: global (feature_cards.md, basis detection_method) |
| NC-09 | n/a: no setting; "sustainability" claims are content, not structure | scope: global (feature_cards.md, basis detection_method) |
| NC-10 | n/a: no sensory description of a scene | scope: global (feature_cards.md, basis detection_method) |
| NC-11 | n/a: no sensory description of a scene | scope: global (feature_cards.md, basis detection_method) |
| NC-12 | n/a: no characters | scope: global (feature_cards.md, basis detection_method) |
| NC-13 | n/a: outside the row's analog set; copy has no causal chain of events | scope: global (feature_cards.md, basis detection_method) |
| NC-14 | n/a: physical space is not depicted | scope: global (feature_cards.md, basis detection_method) |
| NC-15 | analog: read whether the copy frames the outcome as the reader's own choice or empowerment ("take control", "it's your move") versus as what the product does; AI-side when the close is a choice exhortation. threshold: NOT SUPPORTED on analog_CT-09b (AUC 0.47, p 0.6652, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-16 | n/a: no characters are introduced | scope: local (feature_cards.md, basis detection_method) |
| NC-17 | n/a: outside the row's analog set; copy is single-track by genre design | scope: global (feature_cards.md, basis detection_method) |
| NC-18 | analog: classify the closing as a realization statement ("discover a new way of working") versus a concrete action with mechanics (price, trial length, exact next step); AI-side when the close is a realization. threshold: NOT SUPPORTED on analog_CT-09b (AUC 0.54, p 0.4390, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-19 | n/a: outside the row's analog set; headline conventions dominate the opening | scope: local (feature_cards.md, basis detection_method) |
| NC-20 | n/a: no jeopardy or investment | scope: global (feature_cards.md, basis detection_method) |
| NC-21 | n/a: named-reference behavior for this row is carried by NC-06; named testimonials are also a compliance-constrained genre convention | scope: local (feature_cards.md, basis detection_method) |
| NC-22 | n/a: outside the row's analog set; copy is not a narrated text to acknowledge | scope: local (feature_cards.md, basis detection_method) |
| NC-23 | n/a: second person is universal in copy by genre; its presence carries no signal and the row's analog set excludes it | scope: local (feature_cards.md, basis detection_method) |
| NC-24 | n/a: no revelation structure | scope: global (feature_cards.md, basis detection_method) |
| NC-25 | n/a: no chronology | scope: global (feature_cards.md, basis detection_method) |
| NC-26 | n/a: no chronology | scope: global (feature_cards.md, basis detection_method) |
| NC-27 | n/a: no chronology | scope: global (feature_cards.md, basis detection_method) |
| NC-28 | n/a: no locales | scope: global (feature_cards.md, basis detection_method) |
| NC-29 | n/a: no quoted speech beyond fixed-function testimonials | scope: global (feature_cards.md, basis detection_method) |
| NC-30 | analog: count sentences acknowledging a limitation, a trade-off, or who the product is not for; AI-side when zero. threshold: NOT SUPPORTED on analog_CT-09b (AUC 0.54, p 0.7771, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-31 | n/a: emotional appeals in copy are conventional and are not renderings of a character's feeling | scope: global (feature_cards.md, basis detection_method) |
| NC-32 | n/a: tense is fixed by convention — benefits in the present, outcomes in the future — so the choice is not free | scope: global (feature_cards.md, basis detection_method) |
| NC-33 | analog: count the blocks of copy that follow the primary call to action and add no new offer (reassurance, a second value restatement, a vision paragraph); AI-side when more than one such block follows. threshold: NOT SUPPORTED on analog_CT-09b (AUC 0.43, p 0.0841, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-34 | n/a: no characters are depicted; figurative texture in copy is the surface layer and belongs to humanize | scope: global (feature_cards.md, basis detection_method) |
| NC-35 | n/a: outside the row's analog set; copy states its offer up front by genre design, so the pacing pattern is not a free choice | scope: global (feature_cards.md, basis detection_method) |
| NC-36 | n/a: no relationship network is depicted | scope: global (feature_cards.md, basis detection_method) |
| NC-37 | n/a: no central character is identified | scope: global (feature_cards.md, basis detection_method) |
| NC-38 | n/a: the card's vocabulary is a fiction-genre reader contract; every text in this row takes the same non-fiction value, so the rating carries no signal | scope: global (feature_cards.md, basis detection_method) |
| NC-39 | n/a: no character's inner experience is rendered | scope: global (feature_cards.md, basis detection_method) |
| NC-40 | analog: read where the copy leaves the reader: at the concrete offer and its mechanics, versus a jump to a distant-future vision of the reader's transformed life or work; AI-side on the distant-future vision. threshold: NOT SUPPORTED on analog_CT-09b (AUC 0.55, p 0.4925, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| FP | n/a: fingerprint features presuppose narrated events and characters; none exist in product copy; UC-05 only; confidence Low; six-way ceiling 68.4 macro-F1 (Table 3, p. 8) | scope: FP set — 23 global / 5 local (feature_cards.md, basis 11 figure-8 / 17 inferred) |

### CT-10 — Short-form: emails, Slack, social posts, cover letters (under 300 words)

**Row rule (Matrix 1, binding):** local-scope cards only; every global card is `n/a: global rating impossible under 300 words`. Scope values are those in §0.3 (C1 inferences, with NC-10 and NC-25 global per /2, and the extended tier's NC-33 and NC-40 local). The 300-word gate is this build's proposal, not the paper's (§0.5). Confidence Low on every line. If the text would otherwise have matched CT-01 or CT-02 (a flash-fiction piece, a 250-word scene), the local cards use that row's cell instead of the analog below (Part 2, rule 3).

<!-- VALIDATION:BEGIN — generated by _foldin_verdicts.py; do not hand-edit -->
**Validation.** Scored on `analog_CT-10`. Of 9 threshold-bearing cells: **2 established**, 1 separating but with reliability unestablished, 4 measured and not supported, 2 with no referent (never fired).
<!-- VALIDATION:END -->

| id | cell | definition or reason |
|---|---|---|
| NC-01 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-02 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-03 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-04 | analog: count sentences stating the takeaway or lesson of the message ("the key point here is", "this reminds us that"); AI-side when at least one appears in a message under 300 words. threshold: ESTABLISHED on analog_CT-10 (AUC 0.70, BH PASS at q=.10; kappa 1.00 / AC1 1.00) | scope: local (feature_cards.md, basis detection_method) |
| NC-05 | n/a: global card — global rating impossible under 300 words (its card scope changed to global; independently, quoted speech in a short message is rare and functional, so its function does not vary) | scope: global (feature_cards.md, basis detection_method) |
| NC-06 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-07 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-08 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-09 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-10 | n/a: global card — global rating impossible under 300 words | scope: global (feature_cards.md, basis detection_method) |
| NC-11 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-12 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-13 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-14 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-15 | n/a: a message has no plot resolution to attribute; how the message closes is carried by NC-18. (This is a content reason, not a scope reason — NC-15 is a local card and is therefore eligible in this row; it is `n/a` because the row has nothing for it to rate.) | scope: local (feature_cards.md, basis detection_method) |
| NC-16 | analog: where the writer introduces themself or another person (cover letters, introductions), note whether the first introduction is a descriptor list ("a results-driven professional with a passion for…") or a concrete action or event; AI-side when descriptor-first. threshold: NOT SUPPORTED on analog_CT-10 (AUC 0.50, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-17 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-18 | analog: classify the closing as reflective ("I look forward to the opportunity to grow", "excited about what's ahead") versus a concrete action (a specific ask, a time, a next step); AI-side when the close is reflective. threshold: ESTABLISHED on analog_CT-10 (AUC 0.70, BH PASS at q=.10; kappa 0.75 / AC1 0.90) | scope: local (feature_cards.md, basis detection_method) |
| NC-19 | analog: classify the opening as a context frame that restates what the recipient already knows ("I hope this finds you well. I am writing to follow up regarding…") versus opening on the point; AI-side when the first sentence or two are a context frame. threshold: NOT SUPPORTED on analog_CT-10 (AUC 0.53, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-20 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-21 | analog: count named specifics — people, dates, document titles, numbers, a prior message referred to by content; AI-side when zero. threshold: NOT SUPPORTED on analog_CT-10 (AUC 0.47, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-22 | analog: count remarks acknowledging the message as a message ("sorry for the wall of text", "tl;dr", "long story short"); AI-side when zero. threshold: NOT SUPPORTED on analog_CT-10 (AUC 0.55, p 0.4872, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-23 | analog: second person is the default in a message, so count only reader-specific address — a question to the recipient or a reference to their particular situation — as opposed to generic "you"; AI-side when zero reader-specific address. threshold: NO REFERENT on analog_CT-10 — the cell never fired, so it is untestable here, not disproven | scope: local (feature_cards.md, basis detection_method) |
| NC-24 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-25 | n/a: global card — global rating impossible under 300 words | scope: global (feature_cards.md, basis detection_method) |
| NC-26 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-27 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-28 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-29 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-30 | n/a: global rating impossible under 300 words (scope: global, C1 inference) | scope: global (feature_cards.md, basis detection_method) |
| NC-31 | n/a: global card — global rating impossible under 300 words | scope: global (feature_cards.md, basis detection_method) |
| NC-32 | n/a: global card — global rating impossible under 300 words | scope: global (feature_cards.md, basis detection_method) |
| NC-33 | analog: count the sentences that follow the message's main ask or point and add no new information (restated thanks, reassurance, a second sign-off); AI-side when more than one such sentence follows. threshold: NO REFERENT on analog_CT-10 — the cell never fired, so it is untestable here, not disproven | scope: local (feature_cards.md, basis detection_method) |
| NC-34 | n/a: global card — global rating impossible under 300 words | scope: global (feature_cards.md, basis detection_method) |
| NC-35 | n/a: global card — global rating impossible under 300 words | scope: global (feature_cards.md, basis detection_method) |
| NC-36 | n/a: global card — global rating impossible under 300 words | scope: global (feature_cards.md, basis detection_method) |
| NC-37 | n/a: global card — global rating impossible under 300 words | scope: global (feature_cards.md, basis detection_method) |
| NC-38 | n/a: global card — global rating impossible under 300 words | scope: global (feature_cards.md, basis detection_method) |
| NC-39 | n/a: global card — global rating impossible under 300 words | scope: global (feature_cards.md, basis detection_method) |
| NC-40 | analog: read where the message stops: at the ask and its concrete next step, versus a forward-looking flourish that surveys a longer horizon ("excited for everything ahead", "looking forward to all we'll build together"); AI-side on the forward-looking flourish. threshold: separates on analog_CT-10 (AUC 0.72, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: local (feature_cards.md, basis detection_method) |
| FP | n/a: 23 of the 28 fingerprint cards are global (C1 inference) and impossible under 300 words; the five local cards (FP-Human-1, FP-Claude-3, FP-DeepSeek-5, FP-Kimi-1, FP-Kimi-2) concern character introduction, an epilogue ending, an embedded told story, and an in-medias-res entry, none of which a functional short message contains; UC-05 only; confidence Low; six-way ceiling 68.4 macro-F1 (Table 3, p. 8) | scope: FP set — 23 global / 5 local (feature_cards.md, basis 11 figure-8 / 17 inferred) |

### CT-11 — Grant, fellowship, and application statements

**Row rule (Matrix 1):** analogs drawn from CT-03 (argument) and CT-06 (tidy resolution, single track); reader address restricted to register — NC-22/NC-23 are n/a because addressing the reviewer is outside the register, so absence is required rather than telling.

<!-- VALIDATION:BEGIN — generated by _foldin_verdicts.py; do not hand-edit -->
**Validation.** Scored on `analog_CT-11y`. Of 23 threshold-bearing cells: 18 measured and not supported, 5 with no referent (never fired). Supersedes `analog_CT-11` (NSF award abstracts) and `analog_CT-11x`. This row names application statements; on 8 real fellowship statements 0 of 23 cells survive. NC-01, NC-13 and NC-17 had been separating OPPOSITE to their cards on the wrong genre, and two of them were published as survivors.
<!-- VALIDATION:END -->

| id | cell | definition or reason |
|---|---|---|
| NC-01 | analog: count sentences restating the applicant's central aim or claim; AI-side when it is restated more than once beyond the opening and the close. threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.48, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-02 | analog: count sentences lifting the project to sweeping significance ("transform the field", "profound implications for humanity"); AI-side when at least one per section. threshold: NO REFERENT on analog_CT-11y — the cell never fired, so it is untestable here, not disproven | scope: global (feature_cards.md, basis detection_method) |
| NC-03 | analog: count paragraphs not tied to the central aim (a detour, a recounted failure left as a failure); AI-side when zero. threshold: NO REFERENT on analog_CT-11y — the cell never fired, so it is untestable here, not disproven | scope: global (feature_cards.md, basis detection_method) |
| NC-04 | analog: count sentences glossing what an experience or result "taught" or "demonstrates" ("this experience taught me the value of…"); AI-side when more than half of the paragraphs end with one. threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.53, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-05 | n/a: no dialogue | scope: global (feature_cards.md, basis detection_method) |
| NC-06 | analog: classify references to prior work, mentors, and institutions as named or vague ("leading researchers", "a prominent lab"); AI-side when vague outnumber named. threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.41, p 0.3476, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-07 | analog: classify each rendering of the applicant's emotion as a named label ("I was frustrated") or an embodied metaphor ("a fire was lit within me"); AI-side when embodied outnumber labels. Global card: the classification above is the *basis* of one document-level rating plus the 2–3 instances that most drive it — it is never reported as a per-instance location list (§0.3). threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.52, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-08 | n/a: no setting | scope: global (feature_cards.md, basis detection_method) |
| NC-09 | n/a: no setting; environmental research topics are content | scope: global (feature_cards.md, basis detection_method) |
| NC-10 | n/a: no sensory description of a scene | scope: global (feature_cards.md, basis detection_method) |
| NC-11 | n/a: no sensory description of a scene | scope: global (feature_cards.md, basis detection_method) |
| NC-12 | n/a: the applicant's own interior is the genre's default vantage | scope: global (feature_cards.md, basis detection_method) |
| NC-13 | analog: count setbacks or obstacles that are stated and left standing versus those immediately converted into a lesson or strength in the same paragraph; AI-side when every setback is converted and no tension remains. threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.48, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-14 | n/a: physical space is not depicted | scope: global (feature_cards.md, basis detection_method) |
| NC-15 | analog: read whether every turning point in the applicant's path is attributed to their own choice versus to circumstance, luck, or other people; AI-side when all turning points are protagonist-choice. threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.39, p 0.3199, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-16 | analog: note whether the applicant's self-introduction is a trait list ("I am a passionate, detail-oriented researcher") or a concrete event; AI-side when trait-first. threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.55, p 0.5984, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-17 | analog: count secondary strands (a side project, an unrelated interest, a second field) that receive a paragraph of their own; AI-side when zero. threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.42, p 0.6948, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-18 | analog: classify the closing as a realization synthesis ("I have come to understand that…") versus a concrete plan (named aim, timeline, specific mentor or resource); AI-side when the close is a realization. threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.58, p 0.6526, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-19 | analog: classify the opening as a broad frame ("Since childhood, I have been fascinated by…", "Science has always…") versus a specific moment, number, or claim; AI-side when the opening is a broad frame. threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.38, p 0.1282, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-20 | analog: count sentences of background before the first obstacle, problem, or gap is named; AI-side when more than one paragraph precedes it. threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.44, p 0.2000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-21 | analog: count named specifics — labs, papers, mentors, techniques, numbers, dates; AI-side when references are generic. threshold: NO REFERENT on analog_CT-11y — the cell never fired, so it is untestable here, not disproven | scope: local (feature_cards.md, basis detection_method) |
| NC-22 | n/a: meta-remarks to the reviewer are outside the register; absence is genre-required, not a signal | scope: local (feature_cards.md, basis detection_method) |
| NC-23 | n/a: direct address of the reviewer is outside the register; absence is genre-required, not a signal | scope: local (feature_cards.md, basis detection_method) |
| NC-24 | analog: read for a later paragraph that reframes an earlier one (a failure that changes the meaning of a prior success, a stated change of mind); AI-side when none. threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.28, p 0.0095, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-25 | analog: rate the statement once for how often the account of the applicant's path leaves chronological order, using a count of non-chronological moves as the basis and citing the 2–3 that most drive the rating; AI-side when the rating is at the low end — strictly chronological. threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.52, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-26 | n/a: funders' templates dictate where the aim appears; withholding is not a free choice | scope: global (feature_cards.md, basis detection_method) |
| NC-27 | n/a: statements are retrospective by genre; the weight of retrospection is fixed, not chosen | scope: global (feature_cards.md, basis detection_method) |
| NC-28 | n/a: no locales are inhabited | scope: global (feature_cards.md, basis detection_method) |
| NC-29 | n/a: quoted speech is rare and ornamental | scope: global (feature_cards.md, basis detection_method) |
| NC-30 | analog: count sentences conceding a weakness, a wrong turn, or an unresolved doubt about the applicant's own path or project; AI-side when the self-portrait is uniformly positive. threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.36, p 0.0819, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-31 | analog: mark each mode that regularly conveys the applicant's feeling — named emotion words, bodily sensation, metaphorical or environmental imagery, actions and choices, quoted speech tone; AI-side when metaphorical or environmental imagery is among the regular modes. threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.61, p 0.3955, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-32 | n/a: tense is dictated by each section's function (past for the applicant's path, future for the plan), so it is not a free choice | scope: global (feature_cards.md, basis detection_method) |
| NC-33 | analog: count the paragraphs that follow the statement of the applicant's aim and add no new content; AI-side when more than one such paragraph follows. threshold: NO REFERENT on analog_CT-11y — the cell never fired, so it is untestable here, not disproven | scope: local (feature_cards.md, basis detection_method) |
| NC-34 | analog: count metaphors and similes used to render the applicant, mentors, or collaborators and their inner states, per character-descriptive paragraph; AI-side when above one per such paragraph. threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.52, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-35 | analog: note where the reader learns the key context of the applicant's situation and stakes; AI-side when it is front-loaded or spread evenly with nothing held back for the closing section. threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.44, p 0.6887, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-36 | analog: note whether the applicant's relationships with mentors, collaborators, and communities are mainly listed and labelled or shown through described episodes; AI-side when episodes dominate and no enumeration appears. threshold: NO REFERENT on analog_CT-11y — the cell never fired, so it is untestable here, not disproven | scope: global (feature_cards.md, basis detection_method) |
| NC-37 | n/a: the central figure is the first-person applicant by genre requirement, so the naming practice is fixed rather than chosen | scope: global (feature_cards.md, basis detection_method) |
| NC-38 | n/a: the card's vocabulary is a fiction-genre reader contract; every text in this row takes the same non-fiction value, so the rating carries no signal | scope: global (feature_cards.md, basis detection_method) |
| NC-39 | n/a: only the applicant's own interior is rendered, by genre convention; there is no breadth to count | scope: global (feature_cards.md, basis detection_method) |
| NC-40 | analog: read where the statement stops: at the stated aim and its concrete plan, versus a jump to a distant-future vantage surveying the applicant's long-term legacy; AI-side on the distant-future vantage. threshold: NOT SUPPORTED on analog_CT-11y (AUC 0.56, p 0.5658, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| FP | n/a: fingerprint features presuppose narrated events and characters; an application statement's narrated path is too thin to carry them; UC-05 only; confidence Low; six-way ceiling 68.4 macro-F1 (Table 3, p. 8) | scope: FP set — 23 global / 5 local (feature_cards.md, basis 11 figure-8 / 17 inferred) |

### CT-12 — Mixed-authorship documents (human draft with AI sections, or the reverse)

**Row rule (Matrix 1):** cells as for the nearest base type. **A modifier row's evidence tag overrides the base row's:** the base row supplies the cell definitions, this row supplies the evidence tag and the confidence cap, so a CT-01 base becomes `analog: unvalidated` here rather than `literal` / `validated: fiction`, and stays capped at Low. This holds however the base row was reached — assigned directly by rules 3–13 or inherited after an exclusion — and it holds for every feature line in the report, not only the summary (gap ). Plus the rule that rating is per paragraph with a section-level lean map (analogous to the word-level layer's AI-edited-fraction overlay). **Scope handling (rule 4):** local cards fire per paragraph; global cards are rated once per lean-map section (a contiguous run of paragraphs the classifier assigns to one author candidate, Part 2 rule 4), never per document and never per paragraph. If the base-type cell is `n/a`, `n/a` carries over. The classifier's base assignment may differ per section (an AI-drafted introduction on a human-drafted paper: CT-04 and CT-04 with different leans, or CT-03 on CT-08).

<!-- VALIDATION:BEGIN — generated by _foldin_verdicts.py; do not hand-edit -->
**Validation.** No cell-rating corpus reached this row, so all 40 threshold-bearing cells are unvalidated. No cell-rating corpus reached this row. Splice detection was measured separately (`behav_CT-12b`: boundary recall 0.99, precision 1.00, n=18); that is a different instrument from these cells.
<!-- VALIDATION:END -->

| id | cell | definition or reason |
|---|---|---|
| NC-01 | analog: inherit the base-type cell for NC-01 of each lean-map section (n/a carries over); rate once per section, never per document; report the sections whose ratings differ most as the lean map. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-02 | analog: inherit the base-type cell for NC-02 per section; rate once per section; a section that carries broad-significance sentences when its neighbors do not is a lean signal. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-03 | analog: inherit the base-type cell for NC-03 per section; rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-04 | analog: inherit the base-type cell for NC-04; fire per paragraph; a run of paragraphs ending in glosses adjacent to a run without is a lean boundary. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-05 | analog: inherit the base-type cell for NC-05 (n/a carries over); rate once per section (NC-05 is a global card; §0.3). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-06 | analog: inherit the base-type cell for NC-06 per section; rate once per section; a change in citation habit between sections (named in one, vague in the next) is a primary lean signal (Part 2 rule 4). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-07 | analog: inherit the base-type cell for NC-07 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-08 | analog: inherit the base-type cell for NC-08 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-09 | analog: inherit the base-type cell for NC-09 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-10 | analog: inherit the base-type cell for NC-10 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-11 | analog: inherit the base-type cell for NC-11 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-12 | analog: inherit the base-type cell for NC-12 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-13 | analog: inherit the base-type cell for NC-13 per section; rate once per section; a section whose paragraphs all open with connectives beside one whose paragraphs do not is a lean signal. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-14 | analog: inherit the base-type cell for NC-14 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-15 | analog: inherit the base-type cell for NC-15 (n/a carries over); fire per paragraph on the paragraph(s) that carry the resolution. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-16 | analog: inherit the base-type cell for NC-16 (n/a carries over); fire per paragraph at each first introduction. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-17 | analog: inherit the base-type cell for NC-17 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-18 | analog: inherit the base-type cell for NC-18 (n/a carries over); fire per paragraph on each section's closing paragraph. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-19 | analog: inherit the base-type cell for NC-19 (n/a carries over); fire on each section's opening paragraph. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-20 | analog: inherit the base-type cell for NC-20 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-21 | analog: inherit the base-type cell for NC-21; fire per paragraph; a drop to zero named specifics across a run of paragraphs is a primary lean signal (Part 2 rule 4). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-22 | analog: inherit the base-type cell for NC-22 (n/a carries over); fire per paragraph; a change in meta-remark habit between sections is a lean signal. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-23 | analog: inherit the base-type cell for NC-23 (n/a carries over); fire per paragraph; a change in reader-address habit between sections is a lean signal. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-24 | analog: inherit the base-type cell for NC-24 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-25 | analog: inherit the base-type cell for NC-25 per section (n/a carries over); rate once per section, with paragraph boundaries inside the section as the driving spans. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-26 | analog: inherit the base-type cell for NC-26 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-27 | analog: inherit the base-type cell for NC-27 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-28 | analog: inherit the base-type cell for NC-28 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-29 | analog: inherit the base-type cell for NC-29 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-30 | analog: inherit the base-type cell for NC-30 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-31 | analog: inherit the base-type cell for NC-31 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-32 | analog: inherit the base-type cell for NC-32 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-33 | analog: inherit the base-type cell for NC-33 (n/a carries over); fire per paragraph on the paragraphs that follow each section's climactic point. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-34 | analog: inherit the base-type cell for NC-34 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-35 | analog: inherit the base-type cell for NC-35 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-36 | analog: inherit the base-type cell for NC-36 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-37 | analog: inherit the base-type cell for NC-37 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-38 | analog: inherit the base-type cell for NC-38 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-39 | analog: inherit the base-type cell for NC-39 per section (n/a carries over); rate once per section. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-40 | analog: inherit the base-type cell for NC-40 (n/a carries over); fire on each section's closing paragraph. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| FP | analog: inherit the base-type FP cell per section (n/a carries over, so UC-05 runs only on sections whose base row is CT-01, CT-02, or CT-08); never combine sections into one attribution; UC-05 only; confidence Low; six-way ceiling 68.4 macro-F1 (Table 3, p. 8). threshold: proposed, unvalidated | scope: FP set — 23 global / 5 local (feature_cards.md, basis 11 figure-8 / 17 inferred) |

### CT-13 — Text containing code blocks, tables, equations, or figure captions

**Row rule (Matrix 1):** cells as for the base type. **A modifier row's evidence tag overrides the base row's:** the base row supplies the cell definitions, this row supplies the evidence tag and the confidence cap, so a CT-01 base becomes `analog: unvalidated` here rather than `literal` / `validated: fiction`, and stays capped at Low. This holds however the base row was reached — assigned directly by rules 3–13 or inherited after an exclusion — and it holds for every feature line in the report, not only the summary (gap : the rule had been enforced by 41 enumerated cells and a tally script, and stated in prose for CT-14 only). Add the exclusion rule — code blocks, tables, equations, and figure/table captions are segmented out before rating and the report lists what was excluded (block type, location, word count). The base type is assigned by re-running the classifier on the prose remainder (Part 2, rule 2). If the base-type cell is `n/a`, `n/a` carries over. Word-count gates (§0.5) are computed on the prose remainder only.

<!-- VALIDATION:BEGIN — generated by _foldin_verdicts.py; do not hand-edit -->
**Validation.** No cell-rating corpus reached this row, so all 40 threshold-bearing cells are unvalidated. No cell-rating corpus reached this row. Its tag-override question was never observable without code-bearing fiction or memoir input (gap ).
<!-- VALIDATION:END -->

| id | cell | definition or reason |
|---|---|---|
| NC-01 | analog: inherit the base-type cell for NC-01, rated on the prose remainder only; a caption that restates a figure's takeaway is excluded, not counted. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-02 | analog: inherit the base-type cell for NC-02 on the prose remainder. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-03 | analog: inherit the base-type cell for NC-03 on the prose remainder; a paragraph that exists only to introduce a table or figure ("Table 2 shows…") is prose and counts. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-04 | analog: inherit the base-type cell for NC-04 on the prose remainder; glosses inside captions are excluded. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-05 | analog: inherit the base-type cell for NC-05 (n/a carries over); quoted strings inside code are not dialogue. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-06 | analog: inherit the base-type cell for NC-06 on the prose remainder; citations inside table cells are excluded from both counts. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-07 | analog: inherit the base-type cell for NC-07 on the prose remainder (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-08 | analog: inherit the base-type cell for NC-08 on the prose remainder (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-09 | analog: inherit the base-type cell for NC-09 on the prose remainder (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-10 | analog: inherit the base-type cell for NC-10 on the prose remainder (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-11 | analog: inherit the base-type cell for NC-11 on the prose remainder (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-12 | analog: inherit the base-type cell for NC-12 on the prose remainder (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-13 | analog: inherit the base-type cell for NC-13 on the prose remainder; a step-by-step code listing is not a causal chain and is excluded. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-14 | analog: inherit the base-type cell for NC-14 on the prose remainder (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-15 | analog: inherit the base-type cell for NC-15 on the prose remainder (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-16 | analog: inherit the base-type cell for NC-16 on the prose remainder (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-17 | analog: inherit the base-type cell for NC-17 on the prose remainder; an appendix table of alternatives does not count as a side-track unless prose discusses it. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-18 | analog: inherit the base-type cell for NC-18 on the prose remainder (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-19 | analog: inherit the base-type cell for NC-19 on the first prose paragraph after any leading code or table block (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-20 | analog: inherit the base-type cell for NC-20 on the prose remainder (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-21 | analog: inherit the base-type cell for NC-21 on the prose remainder; identifiers, URLs, and version strings inside code blocks are excluded and do not count as named references. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-22 | analog: inherit the base-type cell for NC-22 on the prose remainder (n/a carries over); code comments addressed to the reader are excluded. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-23 | analog: inherit the base-type cell for NC-23 on the prose remainder (n/a carries over); imperatives inside code comments are excluded. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-24 | analog: inherit the base-type cell for NC-24 on the prose remainder (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-25 | analog: inherit the base-type cell for NC-25 on the prose remainder (n/a carries over); paragraph adjacency is computed after block removal. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-26 | analog: inherit the base-type cell for NC-26 on the prose remainder (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-27 | analog: inherit the base-type cell for NC-27 on the prose remainder (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-28 | analog: inherit the base-type cell for NC-28 on the prose remainder (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-29 | analog: inherit the base-type cell for NC-29 on the prose remainder (n/a carries over); code, comments, and table text are neither dialogue nor narration and are excluded from the proportion. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-30 | analog: inherit the base-type cell for NC-30 on the prose remainder (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-31 | analog: inherit the base-type cell for NC-31, rated on the prose remainder only (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-32 | analog: inherit the base-type cell for NC-32 on the prose remainder (n/a carries over); tense inside code comments, captions, and table cells is excluded. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-33 | analog: inherit the base-type cell for NC-33 on the prose remainder (n/a carries over); trailing code blocks, tables, and appendices are excluded from the post-climax count. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| NC-34 | analog: inherit the base-type cell for NC-34 on the prose remainder (n/a carries over); figurative identifiers inside code are excluded. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-35 | analog: inherit the base-type cell for NC-35, rated on the prose remainder only (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-36 | analog: inherit the base-type cell for NC-36, rated on the prose remainder only (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-37 | analog: inherit the base-type cell for NC-37, rated on the prose remainder only (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-38 | analog: inherit the base-type cell for NC-38, rated on the prose remainder only (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-39 | analog: inherit the base-type cell for NC-39, rated on the prose remainder only (n/a carries over). threshold: unvalidated — no corpus reached this row; no evidence either way | scope: global (feature_cards.md, basis detection_method) |
| NC-40 | analog: inherit the base-type cell for NC-40 on the prose remainder (n/a carries over); the last prose passage, not a trailing block, is the ending. threshold: unvalidated — no corpus reached this row; no evidence either way | scope: local (feature_cards.md, basis detection_method) |
| FP | analog: inherit the base-type FP cell on the prose remainder (n/a carries over; in practice a code-bearing text almost always resolves to CT-07, CT-04/05/06, or CT-03, where FP is n/a); UC-05 only; confidence Low; six-way ceiling 68.4 macro-F1 (Table 3, p. 8). threshold: proposed, unvalidated | scope: FP set — 23 global / 5 local (feature_cards.md, basis 11 figure-8 / 17 inferred) |

### CT-14 — Non-English or translated text

**Row rule (Matrix 1):** every cell inherits from the base type but is additionally tagged `language: unvalidated`; confidence Low on every line. The paper never states its corpus language; "English" appears on no page, and English is inferred from the Books3 anthologies and the English-language prompt and schema figures (findings §E.9). A CT-01 base therefore becomes `analog` here rather than `literal`: the translation is across language rather than genre, and it is exactly as unvalidated. Base type is assigned by running Part 2 rules 2–13 on the text (translated or read in the source language). Per, the humanize pass is skipped by default for this row.

**The length gate has no defined unit for scripts without word delimiters**. Whitespace tokenisation of Chinese, Japanese, or Thai returns a near-zero word count, so the §0.5 gates would route a document of *any* length to CT-10 and silently switch off all 29 global cards. For CJK input the gate is **600 and 1600 characters** as the analogues of the 300- and 800-word gates, counting characters excluding punctuation and whitespace, and the report must state that the character gate was used. **Those two numbers are this build's proposal and are weaker than the word gates** — a reading-rate convention, not a measurement — so no CT-14 length claim for a non-delimited script should be presented as reliable until the unit is properly defined. The skill implements the same gate.

<!-- VALIDATION:BEGIN — generated by _foldin_verdicts.py; do not hand-edit -->
**Validation.** Scored on `analog_CT-14d`. Of 39 threshold-bearing cells: **4 established**, 10 separating but with reliability unestablished, 19 measured and not supported, 6 with no referent (never fired). Supersedes `analog_CT-14`, `analog_CT-14b` and `analog_CT-14c`. This row cannot be measured as a single instrument (gap ): it has no cells of its own and its base row resolves per document, so these verdicts come from `analog_CT-14d` and are not poolable with the base row they inherit from.
<!-- VALIDATION:END -->

| id | cell | definition or reason |
|---|---|---|
| NC-01 | analog: inherit the base-type cell for NC-01 (a CT-01 base uses the card's `fiction_detection` as the definition); language: unvalidated; confidence Low. threshold: ESTABLISHED on analog_CT-14d (AUC 0.91, BH PASS at q=.10; kappa 0.88 / AC1 0.92) | scope: global (feature_cards.md, basis detection_method) |
| NC-02 | analog: inherit the base-type cell for NC-02; language: unvalidated; confidence Low. threshold: ESTABLISHED on analog_CT-14d (AUC 0.70, BH PASS at q=.10; kappa 0.61 / AC1 0.77) | scope: global (feature_cards.md, basis detection_method) |
| NC-03 | analog: inherit the base-type cell for NC-03; language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.58, p 0.2396, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-04 | analog: inherit the base-type cell for NC-04; language: unvalidated; confidence Low. threshold: separates on analog_CT-14d (AUC 0.70, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: local (feature_cards.md, basis detection_method) |
| NC-05 | analog: inherit the base-type cell for NC-05 (n/a carries over); language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.53, p 0.4923, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-06 | analog: inherit the base-type cell for NC-06; transliterated or translated titles and names still count as named; language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.58, p 0.1048, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-07 | analog: inherit the base-type cell for NC-07; do not count fixed idioms of the source language as embodied metaphors (a conventional idiom is closer to a label); language: unvalidated; confidence Low. threshold: NO REFERENT on analog_CT-14d — the cell never fired, so it is untestable here, not disproven | scope: global (feature_cards.md, basis detection_method) |
| NC-08 | analog: inherit the base-type cell for NC-08 (n/a carries over); language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.52, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-09 | analog: inherit the base-type cell for NC-09 (n/a carries over); language: unvalidated; confidence Low. threshold: NO REFERENT on analog_CT-14d — the cell never fired, so it is untestable here, not disproven | scope: global (feature_cards.md, basis detection_method) |
| NC-10 | analog: inherit the base-type cell for NC-10 (n/a carries over); language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.50, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-11 | analog: inherit the base-type cell for NC-11 (n/a carries over); language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.52, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-12 | analog: inherit the base-type cell for NC-12 (n/a carries over); languages differ in how free indirect thought is marked, so count only unambiguous interior access; language: unvalidated; confidence Low. threshold: ESTABLISHED on analog_CT-14d (AUC 0.65, BH PASS at q=.10; kappa 0.88 / AC1 0.92) | scope: global (feature_cards.md, basis detection_method) |
| NC-13 | analog: inherit the base-type cell for NC-13; connective inventories differ by language, so count clause-initial logical connectives of the source language, not an English list; language: unvalidated; confidence Low. threshold: NO REFERENT on analog_CT-14d — the cell never fired, so it is untestable here, not disproven | scope: global (feature_cards.md, basis detection_method) |
| NC-14 | analog: inherit the base-type cell for NC-14 (n/a carries over); language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.47, p 0.4923, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-15 | analog: inherit the base-type cell for NC-15 (n/a carries over); language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.53, p 0.7085, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-16 | analog: inherit the base-type cell for NC-16 (n/a carries over); language: unvalidated; confidence Low. threshold: separates on analog_CT-14d (AUC 0.62, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: local (feature_cards.md, basis detection_method) |
| NC-17 | analog: inherit the base-type cell for NC-17 (n/a carries over); language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.55, p 0.3553, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-18 | analog: inherit the base-type cell for NC-18 (n/a carries over); language: unvalidated; confidence Low. threshold: separates on analog_CT-14d (AUC 0.80, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: local (feature_cards.md, basis detection_method) |
| NC-19 | analog: inherit the base-type cell for NC-19 (n/a carries over); language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.53, p 0.4923, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-20 | analog: inherit the base-type cell for NC-20 (n/a carries over); language: unvalidated; confidence Low. threshold: NO REFERENT on analog_CT-14d — the cell never fired, so it is untestable here, not disproven | scope: global (feature_cards.md, basis detection_method) |
| NC-21 | analog: inherit the base-type cell for NC-21; named references in any script count; language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.55, p 0.2385, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-22 | analog: inherit the base-type cell for NC-22 (n/a carries over); language: unvalidated; confidence Low. threshold: separates on analog_CT-14d (AUC 0.70, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: local (feature_cards.md, basis detection_method) |
| NC-23 | analog: inherit the base-type cell for NC-23 (n/a carries over); count all second-person forms including formal/informal (T/V) variants and pro-drop second-person verb forms; language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.53, p 0.6132, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-24 | analog: inherit the base-type cell for NC-24 (n/a carries over); language: unvalidated; confidence Low. threshold: separates on analog_CT-14d (AUC 0.62, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-25 | analog: inherit the base-type cell for NC-25 (n/a carries over); tense systems differ, so identify jumps by stated time reference, not by tense shift alone; language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.53, p 0.7085, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-26 | analog: inherit the base-type cell for NC-26 (n/a carries over); language: unvalidated; confidence Low. threshold: separates on analog_CT-14d (AUC 0.59, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-27 | analog: inherit the base-type cell for NC-27 (n/a carries over); language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.58, p 0.3248, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-28 | analog: inherit the base-type cell for NC-28 (n/a carries over); language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.50, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-29 | analog: inherit the base-type cell for NC-29 (n/a carries over); dialogue punctuation conventions differ (dashes, guillemets, none), so count speech acts rather than quotation marks; language: unvalidated; confidence Low. threshold: separates on analog_CT-14d (AUC 0.70, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-30 | analog: inherit the base-type cell for NC-30 (n/a carries over); language: unvalidated; confidence Low. threshold: separates on analog_CT-14d (AUC 0.68, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-31 | analog: inherit the base-type cell for NC-31 (n/a carries over); language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.55, p 0.3553, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-32 | n/a: the base language may not grammaticalise tense at all — Chinese marks aspect rather than tense, so the card's four values (past / present / future / mixed_or_shifted) have nothing to attach to. The feature is then not merely unvalidated but **undefined**: there is no referent to rate, which is a different failure from rating it with low confidence. Because this row spans every non-English language and the base language is not known when the cell is written, the cell returns `n/a` uniformly; a future per-language split (rating it for languages that do grammaticalise tense, such as French or Russian) would be a new design choice and must be stated as one, not assumed here. (the operating rules, CT-14 study.) | scope: global (feature_cards.md, basis detection_method) |
| NC-33 | analog: inherit the base-type cell for NC-33 (n/a carries over); language: unvalidated; confidence Low. threshold: ESTABLISHED on analog_CT-14d (AUC 0.68, BH PASS at q=.10; kappa 0.87 / AC1 0.92) | scope: local (feature_cards.md, basis detection_method) |
| NC-34 | analog: inherit the base-type cell for NC-34 (n/a carries over); language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.58, p 0.1048, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-35 | analog: inherit the base-type cell for NC-35 (n/a carries over); language: unvalidated; confidence Low. threshold: separates on analog_CT-14d (AUC 0.64, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-36 | analog: inherit the base-type cell for NC-36 (n/a carries over); language: unvalidated; confidence Low. threshold: NO REFERENT on analog_CT-14d — the cell never fired, so it is untestable here, not disproven | scope: global (feature_cards.md, basis detection_method) |
| NC-37 | analog: inherit the base-type cell for NC-37 (n/a carries over); naming practices, honorifics, and patronymics differ by language, so read the dominant form of reference in the source convention; language: unvalidated; confidence Low. threshold: separates on analog_CT-14d (AUC 0.65, BH PASS at q=.10) but its reliability is unestablished — neither confirmed nor refuted | scope: global (feature_cards.md, basis detection_method) |
| NC-38 | analog: inherit the base-type cell for NC-38 (n/a carries over); genre contracts differ across literary traditions, so the card's vocabulary may not have a matching value; rated only — genre is not a rewrite target; language: unvalidated; confidence Low. threshold: NO REFERENT on analog_CT-14d — the cell never fired, so it is untestable here, not disproven | scope: global (feature_cards.md, basis detection_method) |
| NC-39 | analog: inherit the base-type cell for NC-39 (n/a carries over); language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.55, p 0.5105, does not clear BH at q=.10) — measured and did not separate | scope: global (feature_cards.md, basis detection_method) |
| NC-40 | analog: inherit the base-type cell for NC-40 (n/a carries over); language: unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-14d (AUC 0.52, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| FP | analog: inherit the base-type FP cell (n/a carries over; a CT-01 base applies the 28 cards as analogs, not literally); fingerprints are properties of five model versions generating English fiction (findings §E.12, §E.9) and attribution on other-language text has no support at all; language: unvalidated; UC-05 only; confidence Low; six-way ceiling 68.4 macro-F1 (Table 3, p. 8). threshold: proposed, unvalidated | scope: FP set — 23 global / 5 local (feature_cards.md, basis 11 figure-8 / 17 inferred) |

### CT-15 — Anything not matching CT-01 to CT-14 (`other`)

**Row rule (Matrix 1, binding):** local cards only (§0.3 scope, re-derived from `feature_cards.md`: NC-04, NC-15, NC-16, NC-18, NC-19, NC-21, NC-22, NC-23, NC-33, NC-40 — ten cards); every global card is n/a under the row rule; everything tagged unvalidated; confidence Low; and the report opens with a one-line statement of what the text was classified as and why it fit no row (Part 2, rule 13).

<!-- VALIDATION:BEGIN — generated by _foldin_verdicts.py; do not hand-edit -->
**Validation.** Scored on `analog_CT-15`. Of 10 threshold-bearing cells: 9 measured and not supported, 1 with no referent (never fired). These cells measure REGISTER, not authorship. NC-21 returns `human` on all 28 AI documents and is identical on both sides in 8 of 8 paired forms; NC-23 in 7 of 8; NC-19 separates in the wrong direction (AUC 0.39).
<!-- VALIDATION:END -->

| id | cell | definition or reason |
|---|---|---|
| NC-01 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-02 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-03 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-04 | analog: count sentences that tell the reader what the preceding content means; AI-side when at least one per ~200 words; unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-15 (AUC 0.39, p 0.1707, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-05 | n/a: global card — CT-15 rates local cards only | scope: global (feature_cards.md, basis detection_method) |
| NC-06 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-07 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-08 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-09 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-10 | n/a: global card — CT-15 rates local cards only | scope: global (feature_cards.md, basis detection_method) |
| NC-11 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-12 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-13 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-14 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-15 | analog: if the text ends with an outcome, note whether it is attributed to a person's choice or to an external cause; AI-side when choice; does not fire without an outcome; unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-15 (AUC 0.50, p 0.8314, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-16 | analog: at the first mention of any person, note whether the introduction is a descriptor list or an action or speech; AI-side when descriptor-first; does not fire without a person; unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-15 (AUC 0.51, p 0.9439, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-17 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-18 | analog: classify the closing as a realization or reframing versus a concrete action or open question; AI-side when realization; unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-15 (AUC 0.41, p 0.0515, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-19 | analog: classify the opening as a context frame versus a specific claim, fact, or event; AI-side when frame; unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-15 (AUC 0.39, p 0.0511, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-20 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-21 | analog: count named specifics (people, works, places, dates, figures); AI-side when zero per ~300 words; unvalidated; confidence Low. threshold: NO REFERENT on analog_CT-15 — the cell never fired, so it is untestable here, not disproven | scope: local (feature_cards.md, basis detection_method) |
| NC-22 | analog: count remarks acknowledging the text as a text or the reader as reading; AI-side when zero; unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-15 (AUC 0.52, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-23 | analog: count reader-directed second-person sentences (question, imperative, reference to the reader's situation), excluding generic "you"; AI-side when zero; unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-15 (AUC 0.52, p 1.0000, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-24 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-25 | n/a: global card — CT-15 rates local cards only | scope: global (feature_cards.md, basis detection_method) |
| NC-26 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-27 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-28 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-29 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-30 | n/a: global card (C1 inference); CT-15 rates local cards only; confidence Low | scope: global (feature_cards.md, basis detection_method) |
| NC-31 | n/a: global card — CT-15 rates local cards only | scope: global (feature_cards.md, basis detection_method) |
| NC-32 | n/a: global card — CT-15 rates local cards only | scope: global (feature_cards.md, basis detection_method) |
| NC-33 | analog: locate the text's climactic point or main claim, then count how much text follows; AI-side when more than a brief wrap-up follows; unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-15 (AUC 0.54, p 0.7287, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| NC-34 | n/a: global card — CT-15 rates local cards only | scope: global (feature_cards.md, basis detection_method) |
| NC-35 | n/a: global card — CT-15 rates local cards only | scope: global (feature_cards.md, basis detection_method) |
| NC-36 | n/a: global card — CT-15 rates local cards only | scope: global (feature_cards.md, basis detection_method) |
| NC-37 | n/a: global card — CT-15 rates local cards only | scope: global (feature_cards.md, basis detection_method) |
| NC-38 | n/a: global card — CT-15 rates local cards only | scope: global (feature_cards.md, basis detection_method) |
| NC-39 | n/a: global card — CT-15 rates local cards only | scope: global (feature_cards.md, basis detection_method) |
| NC-40 | analog: read where the text stops relative to whatever line of events or argument it has: at its main point, or with a jump to a distant-future vantage; AI-side on the distant-future vantage; does not fire when the text has no line of events; unvalidated; confidence Low. threshold: NOT SUPPORTED on analog_CT-15 (AUC 0.59, p 0.2772, does not clear BH at q=.10) — measured and did not separate | scope: local (feature_cards.md, basis detection_method) |
| FP | analog: only the five local fingerprint cards (FP-Human-1, FP-Claude-3, FP-DeepSeek-5, FP-Kimi-1, FP-Kimi-2) are applied, and only where the text introduces a person, has an ending, embeds a told story, or opens mid-action; the 23 global cards are not rated; everything unvalidated; UC-05 only; confidence Low; six-way ceiling 68.4 macro-F1 (Table 3, p. 8). threshold: proposed, unvalidated | scope: FP set — 23 global / 5 local (feature_cards.md, basis 11 figure-8 / 17 inferred) |

### Part 1 tally

Mechanical count over this file (`awk`/regex on the `| id | cell |` rows of each section; the `FP` row is included in each section's 41). 15 sections × 41 rows (NC-01 … NC-40 + FP) = 615 cells. Totals are summed from the rows below, not written by hand.

| Row | literal | analog | n/a |
|---|---|---|---|
| CT-01 | 41 | 0 | 0 |
| CT-02 | 33 | 8 | 0 |
| CT-03 | 0 | 26 | 15 |
| CT-04 | 0 | 12 | 29 |
| CT-05 | 0 | 6 | 35 |
| CT-06 | 0 | 16 | 25 |
| CT-07 | 0 | 9 | 32 |
| CT-08 | 0 | 40 | 1 |
| CT-09 | 0 | 10 | 31 |
| CT-10 | 0 | 9 | 32 |
| CT-11 | 0 | 23 | 18 |
| CT-12 | 0 | 41 | 0 |
| CT-13 | 0 | 41 | 0 |
| CT-14 | 0 | 40 | 1 |
| CT-15 | 0 | 11 | 30 |
| **Total** | **74** | **292** | **249** |

`FP` is `literal` on CT-01 only.

---

## Part 2 — Content-type classifier

Maps an input text to **exactly one** CT row. Rules are ordered; the first match wins. Rows CT-12, CT-13, and CT-14 are assigned as the row when their trigger fires and carry a `base:` pointer to the row the remaining rules would assign (per section for CT-12; to the prose remainder for CT-13; to the text for CT-14). Every check below is something a reader can perform on the text; none is a paper finding. **All thresholds in this part are the build's proposals, `proposed, unvalidated`.**

### 2.0 Rule 0 — user-named target register (UC-04) overrides the input

If the user names a target register ("make it read like a Nature abstract", "like a magazine feature", "like a README"), **the row is set by the target, not by the input.** Map the named target to the closest row (Nature abstract → CT-04; a magazine feature → CT-08; README → CT-07; personal essay → CT-02; landing page → CT-09; cover letter → CT-10; fellowship statement → CT-11; short story → CT-01). The input is still segmented (rule 2) and length-gated (rule 3), but rules 4–13 are skipped. The report states: "Row set by target register `<name>` → CT-xx; input would have classified as CT-yy" (running rules 4–13 once for the second value).

### 2.1 Rule 1 — language (CT-14)

Two independent clauses; **either** one assigns **CT-14**. The trigger of each is stated with it and applies to that clause only.

- **Clause A — marked or evidently translated.** The text presents itself as a translation: a translator's note or credit, "translated from", source-language proper nouns with English glosses, or non-English quotation conventions throughout. Trigger: any one such marker anywhere in the text (proposed). **Language share is irrelevant to this clause** — a marked translation *into English* is CT-14 even though every sentence is English, because the structural choices being rated were made in another language.
- **Clause B — not in English.** The text is written in a language other than English. Trigger: more than half of the prose sentences are not English (proposed). Below that share the text is not CT-14 under this clause; its non-English spans are segmented out and reported under rule 2 as excluded blocks of type `non-English span`.

When CT-14 is assigned by either clause, set `base:` by running rules 2–13 on the text (read in the source language or translated), tag every result `language: unvalidated`, and cap confidence Low. A bilingual document with an English majority and no translation marker is not CT-14 (Clause A fails, Clause B fails).

### 2.2 Rule 2 — non-prose segmentation (CT-13)

If the text contains one or more of: a fenced or indented code block, a table (markdown, ASCII, or tab-aligned), a display equation (LaTeX delimiters, numbered equation), or a figure/table caption ("Figure 3:", "Table 2."), assign **CT-13** and:

1. Segment every such block out. Trigger: any single block (proposed; no minimum size).
2. Compute the prose word count on the remainder.
3. Set `base:` by running rules 3–13 on the prose remainder.
4. Report each excluded block: type, location (line range or heading), and word count, with a one-line total ("excluded 4 blocks, 612 words; rated 1,930 words of prose").

Inline code spans, inline math, and single-line captions that sit inside a prose sentence are not blocks and stay in the prose.

### 2.3 Rule 3 — length gates (CT-10 and the confidence cap)

Computed on the prose word count (after rule 2 if it fired).

- **Under 300 words → CT-10.** Local cards only; every global card returns `n/a: global rating impossible under 300 words`. Record the would-be genre by running rules 4–13 once: if it is CT-01 or CT-02, the local cards use that row's cells (literal where the span exists) instead of CT-10's analogs; otherwise CT-10's cells apply. Confidence Low.
- **300–799 words → row by rules 4–13, confidence capped Low, and the report header states "under 800 words: global features unreliable."** Global cards are still rated and shown, with that header caveat on each.
- **800 words and above → no length caveat from this rule.**

**These thresholds do not come from the paper.** The paper's stories average 4,753 words (fn. 8, p. 3); its length audit never examines texts below its own shortest tertile of roughly 5,000-word stories, and E1 confirmed by grep that no 800- or 300-word figure appears on any page (findings §E.2, §E.5; E1 worklog item 1). They are this build's design choice and are labelled as such wherever they appear.

### 2.4 Rule 4 — mixed authorship (CT-12)

Split the document into per-paragraph rating with a section-level lean map when **either** of these holds (thresholds proposed, unvalidated):

- **(a) User states the document has mixed authorship** ("I wrote the intro, the rest is generated", or the reverse). Assign CT-12; base rows per section as the user indicates or, if not indicated, per rules 5–13 run on each section.
- **(b) Two or more of the following signals coincide at the same paragraph boundary**, or one signal persists across a run of three or more paragraphs against the rest of the document — **except signal 6, which is sufficient on its own**. Verbatim repetition across a seam is not a stylistic drift that two sections might share by coincidence; it is the same text twice, and the one boundary the CT-12 splice study missed was decided by it while scoring one out of five on the others (gap ):
   1. **Register shift** — an abrupt change between adjacent paragraphs in sentence length distribution, formality, or person (first person to impersonal, or back), not explained by a heading.
   2. **Citation or reference habit shift** — one section names specific works, people, numbers (NC-21/NC-06 human-side) and the adjacent section uses only vague plurals ("studies show", "experts agree").
   3. **Reader-address habit shift** — meta-remarks or second-person address (NC-22/NC-23) present in one section and absent in the adjacent one.
   4. **Gloss-density shift** — a run of paragraphs each ending in a "what this means" sentence (NC-04 analog) next to a run without.
   5. **Formatting-convention shift** — a change in heading style, list style, quotation marks, dash usage, or spelling variant (US/UK) between sections.
   6. **Verbatim repetition across the seam** — a sentence, clause or distinctive phrase from one section reappearing near-verbatim in the adjacent one, most often the earlier section's opening restated in the later section's second or third sentence. This is the **strongest single signal** in the set: a human draft and an inserted machine section, or two machine passes over the same brief, both produce it, while a single author writing continuously rarely repeats themselves that exactly. Added at gap, which found the CT-12 splice study's one missed boundary was decided by exactly this and that the rule did not list it — the rater had to call it "the conclusive evidence" for a boundary the five signals above scored at one out of five.

 Assign CT-12. Cut the document into **sections** at every boundary where the trigger fired; each section gets a `base:` row from rules 5–13 and a lean (`human-lean`, `AI-lean`, `uncertain`) from its own NC results. Local cards fire per paragraph; global cards are rated once per section (Part 1, CT-12 row rule). The report shows the lean map: one line per section with its paragraph range, base row, and lean. No document-level rating for any global card is produced on a CT-12 document.

If neither (a) nor (b) holds, continue.

### 2.5 Rule 5 — fiction (CT-01)

Assign **CT-01** when the text has **all** of: (i) **narrated events involving at least one character**, where a character is any animate participant the text individuates — including a speaker in a transcript or an exchange of messages, whose utterances are themselves the events. A text carried entirely by dialogue or correspondence satisfies (i); (ii) at least one **scene**: a bounded stretch with a place, a time, and events whose **order is carried by the prose itself**. The operative test: *strip every timestamp, date line and entry header — is the sequence of events still recoverable from the sentences?* In narrative it is, because the prose sequences itself. In a record it is not, because the chronology lived in the headers you removed, and what remains is a set of observations in no intrinsic order.

> **Dated-entry form alone is evidence for neither fiction nor records.** Classify by whether the prose or only the header carries the chronology; if both tests are marginal, say so in the report.

(iii): the field notebook, the ship's log, the incident report and the security-camera annotation all assert their contents as fact about the real world and are excluded there, while the transcript fiction does not. If (ii) and (iii) both look marginal, say so in the report rather than resolving it silently — this is gap and it has already nearly captured one field notebook. (iii) no frame asserting the events as fact about the real world (no byline, dateline, "I" identified as the author reporting real events, or citations). Dialogue is typical but not required. Headings such as "Chapter", "Part", or a title alone do not disqualify. If scenes and characters are present but a real-world factual frame is also present, continue to rules 6–7.

### 2.6 Rule 6 — journalism / news feature (CT-08)

Assign **CT-08** when the text has a **reporting frame** — a byline or dateline, attributed quotation ("said", "told", "according to") from named sources, present-tense or recent-past factual reporting about real named people, organizations, or events — and scene-bearing paragraphs (people, place, event) make up **half or fewer** of the paragraphs (proposed). If scene-bearing paragraphs exceed half, the piece is reportage carried by scenes and goes to rule 7.

### 2.7 Rule 7 — narrative nonfiction (CT-02)

Assign **CT-02** when scenes and real people are present (rule 5 conditions i–ii) **and** the events are framed as real: first-person memoir or personal essay ("I" as the author recounting lived events), or reportage in which scene-bearing paragraphs exceed half of the text (from rule 6). A personal essay with no scenes at all is not CT-02; it goes to rule 12.

### 2.8 Rule 8 — research paper sections (CT-04, CT-05, CT-06)

If the text carries research-paper section signals — headings or running text matching Abstract, Introduction, Related Work, Background, Methods/Methodology/Materials, Experiments/Experimental Setup, Results, Discussion, Limitations, Conclusion/Future Work, or numbered citations / author-year citations at research density (at least one per paragraph, proposed) — assign by section:

- Abstract, Introduction, Related Work, Background → **CT-04**
- Methods, Materials, Experimental Setup, Results, Experiments, Evaluation → **CT-05**
- Discussion, Limitations, Conclusion, Future Work, Broader Impact → **CT-06**

If the input contains sections from more than one of these groups (a whole paper, or intro-plus-discussion), it is **not one row**: split at the section headings and treat each part as its own unit under UC-06 (batch), each with its own row and report; no cross-unit averaging of global features. An unheaded single passage with research-density citations is CT-04 if it states a contribution or gap, CT-06 if it interprets results, CT-05 if it reports procedures or numbers without interpretation.

### 2.9 Rule 9 — technical documentation, tutorials, READMEs (CT-07)

Assign **CT-07** when the text is organized around performing a task with software or hardware: imperative steps ("Run", "Install", "Open"), inline code identifiers, headings such as Installation, Usage, Configuration, API, Prerequisites, Troubleshooting, or a README title and badges. (Code fences will already have routed the text through rule 2 with `base: CT-07`.)

### 2.10 Rule 10 — marketing / product copy / landing pages (CT-09)

Assign **CT-09** when the text promotes a product, service, or organization to a reader in the second person with **at least one call to action** ("Sign up", "Get started", "Book a demo", "Buy now"), benefit claims without an argumentative structure (no thesis defended against objections), and a repeated product or brand name. Testimonials in quotation marks do not make it CT-08.

### 2.11 Rule 11 — grant, fellowship, and application statements (CT-11)

Assign **CT-11** when the text is a first-person case addressed implicitly to a selection committee: statement-of-purpose signals ("I propose", "specific aims", "my research program", "this fellowship would", "I am applying"), a named funder, program, or position, and a retrospective account of the applicant's path leading to a stated aim. Checked before rule 12 because these statements are argumentative and would otherwise fall into CT-03.

### 2.12 Rule 12 — opinion essay / blog post (CT-03)

Assign **CT-03** when the text defends a position — a thesis stated or clearly implied, supporting reasons or evidence, and a conclusion — **without scenes** (no bounded place-time-event stretches; passing anecdotes of one or two sentences do not count as scenes). Blog-post furniture (a title, a date, subheadings, a sign-off) does not change the row.

### 2.13 Rule 13 — fallback (CT-15, `other`)

If no rule above matched, assign **CT-15**. The report **must open** with a one-line statement in this form: "Classified as `other`: the text is <what it appears to be — e.g. a transcript, a poem, a legal contract, a recipe, a dataset description, a forum thread>; it fit no row because <the specific failed condition — e.g. no argumentative structure (CT-03), no scenes (CT-01/02), no task steps (CT-07), no reporting frame (CT-08)>." Then apply the CT-15 row: local cards only, everything unvalidated, confidence Low. Common CT-15 inputs include poetry, drama and screenplays (dialogue without narration), transcripts, legal and policy text, recipes, and lists.

### 2.14 What the classifier writes into the report

For every run, three lines before any feature output:

1. `content_type: CT-xx <row name>` (with `base: CT-yy` for CT-12 per section, CT-13, CT-14; with `target: <name>` when rule 0 fired).
2. `prose_words: N` and the length-gate outcome from rule 3, with the sentence "length gates are this build's proposal, not the paper's (paper corpus mean 4,753 words, fn. 8, p. 3)" whenever a gate fires.
3. `evidence_tag: validated: fiction` (CT-01 only, and literal cells on CT-02) or `evidence_tag: analog: unvalidated` (every other cell), plus `language: unvalidated` on CT-14 and the exclusion list on CT-13.

---
