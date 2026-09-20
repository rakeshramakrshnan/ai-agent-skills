# Interventions — structural moves for `narrative-humanize`, one per NC-id

**Consumed by:** `../SKILL.md`, protocol steps 3 and 4.
Human and AI values come from StoryScope Table 16. `feature_cards.md` supplies each card's
human-side value, scope, rank, and tier.
**Cells:** `content_type_matrix.md` (this directory; C2). For every NC-id, the set of `n/a` cells in the table below is identical, cell for cell, to C2's `n/a` cells for that NC-id (generated mechanically from the matrix and re-checked before this file was written). Where C2 has `analog`, the move acts on the thing C2's analog definition counts.
Ids and names follow `nc_id_registry.md`; use-case behavior follows `use_case_matrix.md`.

## Priority: act by measured importance, not by printed gap

`measured_importance` on each entry is that feature's weight in the retrained classifier (`feature_cards.md` Part A for NC-01 to NC-30, Part B for NC-31 to NC-40), with its rank among the 30 cards and its share of total mass. Tiers: **Tier 1 (heavy)** at or above 0.0100, **Tier 2** from 0.0020 to 0.0099, **Tier 3 (light)** below 0.0020.

A structural pass is not complete while a Tier 1 line in the HANDOFF is unacted and unlogged. Tier 2 and Tier 3 moves remain useful but never replace a Tier 1 move. Reader address stays last.

## Length policy

The CT-01 cells are overwhelmingly subtractive, so keep the finished text within the band below.

- **Band: plus or minus 15% of the input word count**, the band the validation experiment used. Record before and after counts in the change log on every run.
- **Subtractive on CT-01:** NC-01, NC-02, NC-03, NC-04, NC-05, NC-08, NC-09, NC-10, NC-11, NC-12, NC-14, NC-20, and (new) NC-33, NC-34, NC-40.
- **Additive on CT-01, the moves that restore length: NC-17 (a secondary thread), NC-27 (expand a summarized flashback into a scene), NC-29 (render summary as dialogue).** Smaller additions come from NC-22, NC-23, NC-24, NC-28, and (new) NC-31, NC-36, NC-39.
- **The additive moves offset the cuts; they do not merely accompany them.** If the pass is below -15%, extend the additive moves that are already licensed and under-applied before calling it complete. Do not restore what was cut: that would undo an earlier move.
- **Never cut a text across a `narrative-check` length gate** (800 words, 300 words). Those gates are this build's design choice rather than the paper's, but crossing one makes most features unratable at P3. If a pass would cross one, stop short of it and log that.

## Marker delivery gate (measured)

`[SOURCE NEEDED]` annotations must not survive into the delivered text. This is a measured requirement, not a stylistic one: markers remained inline in 7 of the 10 rewrites, and stripping them changed the classifier score on every one, reversing one flip entirely (0.822 to 0.092) and raising another (0.021 to 0.333). The headline results are the marker-free numbers. Every annotation is therefore **resolved** (with a real source the user supplies) or **removed** (the sentence returns to its pre-annotation wording, or goes) before delivery, and the list travels in the STRUCTURAL CHANGE LOG, never in the prose (SKILL.md Step 8). The budget still caps how many spans may be annotated during the pass.


## How to read an entry

- **`id`, `name`** — registry values, verbatim.
- **`moves_toward`** — the human-side value or option from the card, quoted with its Table 16 number. For an AI-elevated feature the move is toward *lower / absent*; for a human-elevated feature it is toward *higher / present*. The number is the target's direction, not a target to hit: the paper prints corpus means, not thresholds (findings §E.2), and this skill never claims a value was reached.
- **`scope`** — `global` or `local` from the card (C1 inference under, with the two Gate 2 rulings applied). A `global` feature is edited at its 2–3 driving spans and logged as `document-level` plus those spans; it is never pinned to a single line. A `local` feature is edited at each exact span the HANDOFF line gives.
- **`fact_risk`** — one of three levels, each with a one-line reason:
  - `none` — the move only deletes or reorders the author's own sentences; no proposition enters the text.
  - `low` — the move adds or rewrites a proposition built from material already in the text or supplied by the user (a mentioned character, an existing concession, an event already narrated); no real-world fact enters.
  - `high` — the move calls for a specific external fact (author, title, year, system, dataset, number, quotation). The skill inserts `[SOURCE NEEDED: <what would fit here>]` at the span and lists it; it never invents the fact (use_case_matrix.md UC-10).
- **`never_do`** — the moves that would defeat the intervention or violate the fact ledger.
- **`by_content_type`** — fifteen rows, CT-01 … CT-15. Every cell is exactly one of:
  - `literal: <move>` — the fiction move applied as written (C2 cell `literal`; report tag `validated: fiction`).
  - `analog: <move>` — a move on the thing C2's analog counts for that row (C2 cell `analog`; every frequency inside is C2's proposal, never the paper's).

 **Check the cell's verdict before applying the move.** Since the validation fold-in, each analog cell in `content_type_matrix.md` carries its own `threshold:` verdict rather than one blanket unvalidated tag (§0.8). The verdict changes what the move is worth, and the log must say which applied:
  - `ESTABLISHED` — the only case where moving the marker is moving something measured. Report tag `established: <corpus>`.
  - `separates … reliability unestablished`, `unvalidated`, `NO REFERENT` — apply the move if the user asked for the row, tag `analog: unvalidated`, and do not imply the edit is known to help.
  - `NOT SUPPORTED` — the cell was measured on its named genre and did not separate. The move is cosmetic: it changes the text without changing anything the measurement could see. Apply it only on explicit request and log `unsupported: <cell> did not separate on <corpus>`.
  - `REFUTED … separates OPPOSITE` — **do not apply.** The cell separates contrary to its card, so moving the text in the card's direction moves it the wrong way. Log `skipped: <cell> refuted (opposite direction)`.
  - `n/a` — no intervention on this row. The reason is C2's, in the matching cell of `content_type_matrix.md`; the SKILL logs `skipped: n/a for CT-xx (content_type_matrix.md)`. Applying a fiction intervention to an `n/a` cell is a defect (SKILL.md protocol step 4).

**Rows CT-12, CT-13, CT-14 inherit.** Their cells say `inherit the base-type move`: look up the base row the classifier assigned (per lean-map section for CT-12; for the prose remainder for CT-13; for the whole text for CT-14) and apply that row's cell for this NC-id, with the adjustment stated. If the base row's cell is `n/a`, `n/a` carries over and nothing is done. On CT-14 the humanize pass is skipped by default .

**Marker budget .** `[SOURCE NEEDED]` markers are placed by NC-21 and NC-06 only, and the two share one budget: when the user has supplied no sources, at most **two or three markers in the whole text**, at the spans that most drive the rating; every other vague attribution or unnamed echo is listed in the STRUCTURAL CHANGE LOG as `unmarked: cap` and left as written. Stop-conditions of the form "until named references are at least as many as vague ones" apply only when sources are supplied to name them with. A marker is never counted as movement in the NC-id DIFF (UC-10).

**Precedence.** `content_type_matrix.md` governs row membership and cell type. `feature_cards.md` governs scope, values, rank, and tier. This file governs the move. Report disagreements and follow the owning file.

**Protect what is already human-side.** A feature the report rates human-side has no HANDOFF line and is therefore invisible to the moves. NC-12's externalizing move deletes the explicit emotion labels that hold NC-07 on the human side, and NC-14's spatial cut can take the named objects that hold NC-21 there. Before acting on a span, check it against the report's HUMAN-SIDE FEATURES PRESENT section; where it is cited there, preserve that element, act at the remaining driving spans, and log `held: span carries human-side NC-xx`.

**New story material is permitted on fiction.** NC-17, NC-27 and NC-29 (and, in Part 1B, NC-31 and NC-39) cannot execute on a story without writing sentences that were not there. Propositions invented inside the story-world are diegetic material, not ledger items, and are not what the Hard rule against inventing forbids. The ceiling: they introduce no new proper noun, date, or figure, and act through elements the text already names. On every non-fiction row the prohibition is unchanged and absolute.

**Direction is never inverted.** Where an analog's AI-side condition in C2 is *presence* (AI-elevated), the move removes or thins; where it is *absence* (human-elevated), the move adds one instance from material present. Whether the direction survives translation to a non-fiction genre is exactly what is unvalidated (C2 §0.2).

**Precedence when moves conflict.** Core moves (this table) outrank fingerprint moves (§ Fingerprint-specific moves). Among core moves the SKILL's step-3 order applies; a later move never undoes an earlier one (a subplot added under NC-17 is not tied back under a later pass; a loose end left under NC-13 is not re-explained). NC-17 and NC-03 are compatible by construction: the added thread is thematically parallel (NC-17's human option) and is not gathered into the ending (NC-03's cut).

**Standing scope note (CAV-SCOPE, `use_case_matrix.md` §0.4; printed in every `narrative-humanize` output). Quoted verbatim; C3 owns this text:**

> Scope note. StoryScope's only robustness test is a surface-edit test: 278 Gemini-written stories edited with LAMP, Gemini as rewriter, narrative-classifier macro-F1 95.5 before and 93.9 after (Section 4.2, p. 8). The paper argues from that result that narrative features are "far harder to 'humanize'" because changing them "requires significant structural rewrites" (p. 2), and it never runs the experiment where the structural rewrite is actually performed. This build ran a small version of it. On ten AI-written stories from the paper's own dev split, the structural pass moved mean P(human) from 0.057 to 0.607 (mean Δ +0.549), with every one of the ten deltas positive (sign test, two-sided p = 0.002) and 66 core-feature movements toward the human baseline against 13 away; **five of the ten — half — crossed the 0.5 decision boundary, and the other half did not** (validation results Step 5). Five caveats bound that number and travel with it everywhere: n = 10; fiction only; one direction (AI → human); scored by a classifier retrained on the authors' released features (dev macro-F1 0.942), not their released weights; and features assigned by Claude agents following the authors' prompts, not by Gemini 3 Flash. So the honest position is neither "unmeasured" nor "it works": the structural layer moved this classifier on ten fiction texts far more than surface editing moved the paper's, and that is all it has been shown to do. What this pipeline reports is which features moved in your text, not whether any detector's verdict will change.

### Content-type rows (names from content_type_matrix.md)

- CT-01 — Short fiction, novel excerpt
- CT-02 — Narrative nonfiction (memoir, personal essay, reportage with scenes)
- CT-03 — Opinion essay / blog post (argument-driven, no scenes)
- CT-04 — Research paper: abstract and introduction
- CT-05 — Research paper: methods and results
- CT-06 — Research paper: discussion, limitations, conclusion
- CT-07 — Technical documentation, tutorials, READMEs
- CT-08 — Journalism / news feature
- CT-09 — Marketing / product copy / landing pages
- CT-10 — Short-form: emails, Slack, social posts, cover letters (under 300 words)
- CT-11 — Grant, fellowship, and application statements
- CT-12 — Mixed-authorship documents
- CT-13 — Text containing code blocks, tables, equations, or figure captions
- CT-14 — Non-English or translated text
- CT-15 — Anything not matching CT-01 to CT-14 (other)

---

## Part 1 — Core interventions (NC-01 … NC-30, registry order)

### NC-01 — Thematic Explicitness & Moralizing

- **id:** NC-01
- **name:** Thematic Explicitness & Moralizing
- **moves_toward:** lower on the 1–5 scale — human mean 3.28 against AI 3.94 (Table 16, p. 26). Not to 1: the human mean sits above the midpoint, so the target is fewer statements of meaning, not none.
- **scope:** global (basis: detection_method; C1 inference under ). One document-level rating; the 2–3 driving spans in the HANDOFF line are where the cuts land.
- **measured_importance:** 0.0417 (4.2% of total mass), rank 1 of the 30 cards — Tier 1 (heavy). `feature_cards.md` Part A.
- **fact_risk:** none — the move only deletes sentences that tell the reader what already-stated material means; no proposition enters the text.
- **never_do:**
  - Never remove an interpretive sentence that carries a claim not stated elsewhere in the text; if the sentence is the only place a claim lives, it stays and the log says why.
  - Never cut the thesis itself in an argumentative row (CT-03, CT-04, CT-06, CT-11); only restatements go.
  - Never reword a stated theme into a hedged or softened version. Rewording is surface work and leaves the feature firing; the structural choices are cut, relocate, or leave.
  - Never cut to zero. A text with no articulated meaning is not the human pattern the card describes.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: cut the sentences in which the narrator or a character states what the story means or what lesson it carries, starting with the driving spans (a closing paragraph that states the lesson; a speech that summarizes what events "mean"). Leave at most one such statement, and only if the ending depends on it; let events and images carry the rest. |
| CT-02 | literal: same cuts, applied to the piece's stated meaning. Memoir may keep one reflective statement that carries a claim found nowhere else; cut the restatements. |
| CT-03 | analog: cut thesis restatements ("the point is", "what this shows is") out of the body sections so the thesis is stated in the introduction and the conclusion only; body paragraphs end on their evidence. |
| CT-04 | analog: cut restatements of the contribution ("we show", "this work highlights") to at most two across abstract plus introduction, and remove every end-of-paragraph restatement in the introduction. |
| CT-05 | analog: cut interpretive sentences ("this demonstrates", "this means") so no results paragraph carries more than one; keep the one that states a claim not made elsewhere; leave the reported numbers untouched. |
| CT-06 | analog: cut restatements of what the main finding means to at most two across the discussion and conclusion; the remaining paragraphs discuss, they do not recap. |
| CT-07 | analog: cut purpose-or-benefit sentences that the step itself already makes obvious ("This ensures your environment is properly configured"); at most one per step, and only where the purpose is not evident from the command. |
| CT-08 | analog: cut sentences in the reporter's voice that state the story's significance after the nut graf, leaving at most one; let quoted sources carry significance (do not add quotes). |
| CT-09 | analog: cut restatements of the value proposition so it appears at most once per section beyond the headline. |
| CT-10 | n/a |
| CT-11 | analog: cut restatements of the applicant's central aim so it appears in the opening and the close only. |
| CT-12 | analog: inherit the base-type move for NC-01 for each lean-map section (n/a carries over); apply the cuts once per section, never across the document; record the section range in the log's `where` column. |
| CT-13 | analog: inherit the base-type move for NC-01 on the prose remainder only; captions that restate a figure's takeaway are excluded from rating and are not edited by this skill. |
| CT-14 | analog: inherit the base-type move for NC-01; language: unvalidated; confidence Low; the humanize pass is skipped by default on this row . |
| CT-15 | n/a |

### NC-02 — Moral / Philosophical Weighting

- **id:** NC-02
- **name:** Moral / Philosophical Weighting
- **moves_toward:** lower on the 1–5 scale — human mean 3.26 against AI 3.68 (Table 16, p. 26). Fewer passages foregrounding questions of right, wrong, meaning, or existence; not their removal.
- **scope:** global (basis: detection_method; C1 inference under ). One document-level rating plus 2–3 driving spans.
- **measured_importance:** 0.0006 (0.1% of total mass), rank 29 of the 30 cards — Tier 3 (light). `feature_cards.md` Part A.
- **fact_risk:** none — cuts and shortenings of the author's own reflective passages; nothing is added.
- **never_do:**
  - Never remove a moral or philosophical question the plot or argument depends on.
  - Never insert new philosophical framing anywhere, including as a replacement for what was cut.
  - Never cut moral content from a text whose subject is ethics or philosophy (an essay on a moral question, a discussion section in a bioethics paper). The C2 analogs fire on the lift from a practical topic to a general claim; a text that is about the general claim is not lifting.
  - Never reduce a character's stated values in dialogue that reveals character; the target is narratorial and thematic weighting, and dialogue that argues ideas is NC-05.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: cut or shorten the passages that frame the situation as an ethical dilemma or reflect on meaning and justice in the narrator's voice, where the plot does not depend on them; keep the incident, lose the meditation on it. |
| CT-02 | literal: same cuts on the author's reflective passages; keep those that carry a claim found nowhere else. |
| CT-03 | analog: cut sentences that lift a practical topic to a general claim about human nature, society, or meaning ("this is ultimately about what it means to…"), especially the conclusion's pivot to one; end on the practical point. |
| CT-04 | analog: cut broad-stakes sentences ("with profound implications for", "fundamentally reshapes our understanding of") from the abstract entirely and to at most one in the introduction. |
| CT-05 | analog: cut every significance-elevating sentence from methods; leave at most one in results. |
| CT-06 | analog: cut broad-significance sentences ("implications for society") so that fewer than one conclusion paragraph in two carries one; keep at most one in the whole conclusion. |
| CT-07 | analog: cut sentences elevating a tool or practice to a principle or value ("good documentation is the foundation of…") everywhere outside a section that is explicitly motivational. |
| CT-08 | analog: cut sentences lifting the story to moral or philosophical stakes in the reporter's own voice; stakes may remain inside quotations already present; do not add quotations. |
| CT-09 | analog: cut mission-or-philosophy sentences ("we believe", "reimagining what's possible") from feature sections; keep at most one, in the section whose purpose is the mission. |
| CT-10 | n/a |
| CT-11 | analog: cut sweeping-significance sentences ("transform the field") to at most one, placed in the aims section; the rest of the statement describes the project. |
| CT-12 | analog: inherit the base-type move for NC-02 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-02 on the prose remainder. |
| CT-14 | analog: inherit the base-type move for NC-02; language: unvalidated; confidence Low; humanize skipped by default on this row . |
| CT-15 | n/a |

### NC-03 — Thematic Unity

- **id:** NC-03
- **name:** Thematic Unity
- **moves_toward:** lower on the 1–5 scale — human mean 4.41 against AI 4.74 (Table 16, p. 26). Both means are high; the move is to let one subplot or flourish stand without being reconciled to the central concern, not to scatter the text.
- **scope:** global (basis: detection_method; C1 inference under ). One document-level rating plus 2–3 driving spans (a digression and the point where it is tied back; an ending that gathers every thread).
- **measured_importance:** 0.0076 (0.8% of total mass), rank 12 of the 30 cards — Tier 2. `feature_cards.md` Part A.
- **fact_risk:** none — the move cuts the tie-back sentences that reconcile a secondary element to the theme; it adds nothing. (Adding a digression is NC-17's job, with its own risk level.)
- **never_do:**
  - Never remove a tie-back sentence that carries a claim not stated elsewhere.
  - Never cut the element itself (the subplot, the flourish, the example); cut only the sentence that forces it to serve the central concern.
  - Never uncouple every secondary element; one un-reconciled thread is the target, and the human mean of 4.41 says most threads still serve the theme.
  - Never treat this as license to remove an argumentative text's conclusion; the conclusion may still gather the main threads, minus one.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: for one subplot, digression, or recurring image, cut the sentence(s) that tie it back to the central concern, most often in the ending that gathers every thread; let that element end on its own terms. |
| CT-02 | literal: same on the piece's secondary threads and flourishes; leave one un-reconciled. |
| CT-03 | analog: cut the tie-back sentence from at least one body paragraph so that paragraph ends on its own point (an aside, a tangent) rather than returning to the thesis. |
| CT-04 | analog: cut the funnel sentence ("which motivates our approach") from at least one introduction paragraph so it ends on the state of the prior work rather than on the contribution. |
| CT-05 | analog: cut the closing tie-to-main-claim sentence from at least one results paragraph; the paragraph ends on the reported result. |
| CT-06 | analog: cut the closing tie-back sentence from at least one discussion paragraph; the paragraph ends on the point it discussed. |
| CT-07 | analog: cut section-closing tie-backs to the overall goal ("Now you have a solid foundation for…") from every section but the last. |
| CT-08 | analog: where a color or tangential-detail paragraph exists, cut the sentence that bends it to serve the story's angle and let it stand as detail; if no such paragraph exists, log `skipped: no material` (this skill does not add reporting). |
| CT-09 | analog: where an aside, joke, or specific anecdote exists, cut the sentence that converts it into a selling point; if none exists, log `skipped: no material`. |
| CT-10 | n/a |
| CT-11 | analog: cut the tie-back sentence from at least one paragraph; a recounted detour or failure stays a detour or failure (see also NC-13, NC-30). |
| CT-12 | analog: inherit the base-type move for NC-03 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-03 on the prose remainder; a paragraph that only introduces a table or figure is prose and may be edited. |
| CT-14 | analog: inherit the base-type move for NC-03; language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-04 — Narratorial Thematic Commentary

- **id:** NC-04
- **name:** Narratorial Thematic Commentary
- **moves_toward:** option "no" — human 52% "yes" against AI 77% (Table 16, p. 26). The binary flips to "no" when no narratorial passage states the theme beyond any character's perspective.
- **scope:** local (basis: detection_method; C1 inference under ). Every firing instance has an exact span in the HANDOFF line; each is acted on individually.
- **measured_importance:** 0.0015 (0.2% of total mass), rank 23 of the 30 cards — Tier 3 (light). `feature_cards.md` Part A.
- **fact_risk:** none — deletion or relocation of the author's own commentary; nothing is added.
- **never_do:**
  - Never remove a character's stated view; the question is about commentary "beyond characters' perspectives", and a character's speech or thought does not fire it.
  - Never remove a commentary sentence that carries a claim stated nowhere else; relocate it into a character's speech or thought instead (fiction, CT-01) or leave it and log why (every other row).
  - Never soften commentary into a hedged version; the instance still fires. Cut or relocate.
  - Never relocate commentary into a real person's mouth in CT-02 or CT-08. Putting words a real person did not say into their speech is fabrication.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: delete each narratorial sentence or paragraph that states the lesson or theme beyond any character's perspective, starting with the closing paragraph; where the claim must survive, move it into a character's speech or thought, where it no longer fires. |
| CT-02 | analog: cut the post-scene statement of what the scene meant ("that was the day I learned…") after at least half of the scenes; reflective commentary that is not pinned to a scene is genre-normal and stays. |
| CT-03 | analog: cut the sentences in which the author steps out of the argument to tell the reader what the preceding evidence means ("in other words, this reminds us that…") until fewer than half of the body paragraphs carry one. |
| CT-04 | analog: cut glosses on cited prior work ("this underscores the need for", "highlighting the importance of") until fewer than half of the citation clusters carry one; report the prior work, do not interpret it. |
| CT-05 | analog: cut "this highlights / underscores / suggests the importance of" glosses attached to reported numbers until fewer than half of the reported results carry one. |
| CT-06 | analog: cut the same glosses on discussed results until fewer than half of the paragraphs carry one. |
| CT-07 | analog: delete every congratulation line and cut post-step statements of what the reader has learned or why it matters until fewer than half of the steps carry one. |
| CT-08 | analog: cut interpretive glosses in the reporter's voice that follow a quote or fact ("a reminder that…", "a sign of…") until fewer than half of the quotes carry one. |
| CT-09 | analog: cut the reader-benefit gloss that follows a feature ("so you can focus on what matters") until fewer than half of the feature lines carry one. |
| CT-10 | analog: delete every sentence that states the takeaway or lesson of the message ("the key point here is", "this reminds us that"); a message under 300 words carries none. |
| CT-11 | analog: cut the "this experience taught me the value of…" gloss until fewer than half of the paragraphs end with one; keep the experience. |
| CT-12 | analog: inherit the base-type move for NC-04 (n/a carries over); act per paragraph; note in the log where a run of gloss-ending paragraphs sat, since that run was a lean boundary in the report. |
| CT-13 | analog: inherit the base-type move for NC-04 on the prose remainder; glosses inside captions are excluded and not edited. |
| CT-14 | analog: inherit the base-type move for NC-04; language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | analog: delete gloss sentences that tell the reader what the preceding content means until fewer than one per ~200 words remains (the ~200-word rate is C2's proposal, that cell's `threshold:` verdict in `content_type_matrix.md` §0.8); confidence Low. |

### NC-05 — Dialogue Function

- **id:** NC-05
- **name:** Dialogue Function
- **moves_toward:** "philosophical debate" not among the main functions — human 34% against AI 59% (Table 16, p. 26). Dialogue that negotiates the immediate situation, advances events, or reveals character rather than argues ideas.
- **scope:** global (basis: detection_method; `feature_cards.md`). Global because the authors' detection method selects "each function that is consistently prominent across the text", a whole-text distribution rather than one exchange. One document-level rating plus the 2-3 driving spans.
- **measured_importance:** 0.0098 (1.0% of total mass), rank 9 of the 30 cards — Tier 2. `feature_cards.md` Part A.
- **fact_risk:** low — in fiction the exchange is re-anchored to the situation the scene already establishes; in every row where the quoted words belong to a real person (CT-02, CT-03, CT-08, CT-15) quotations are ledger items that may be cut or kept, never altered or added.
- **never_do:**
  - Never alter the words of a real person's quotation. Cut the exchange or keep it; do not rewrite what someone said.
  - Never invent a quotation, testimonial, or line of reported speech to rebalance the count.
  - Never cut a dialogue exchange the plot or argument depends on; shorten it to the lines that do the work.
  - Never treat this as a per-instance local card. It is `scope: global`, so CT-15 (local cards only) is `n/a` for it. A live move on CT-15 was the one core cell where this file disagreed with the matrix.
  - Never make dialogue more philosophical to "add depth"; that is the AI-side move.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: for each exchange in which characters debate meaning, ethics, fate, or belief, cut it, shorten it to the lines that move the plot, or re-anchor it to the immediate situation (what to do next, what just happened); leave at most one such exchange if the story needs it. |
| CT-02 | literal: cut or shorten exchanges of reported dialogue that argue ideas; because the speakers are real people, do not rewrite the content of what they said; if cutting is impossible without losing a claim, log `skipped: reported speech cannot be altered`. |
| CT-03 | analog: cut abstract-reflection quotations ("it makes you wonder what…") until evidential quotations (a fact, event, or position) are at least as many; do not add quotations; if no evidential quotation exists, cut abstract ones to at most one and log it. |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: same as CT-03 on the story's quotes; every quote is a real person's words: cut or keep, never alter; never add a quote. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | n/a |
| CT-12 | analog: inherit the base-type move for NC-05 (n/a carries over); act per paragraph. |
| CT-13 | analog: inherit the base-type move for NC-05 (n/a carries over); quoted strings inside code are not dialogue and are not touched. |
| CT-14 | analog: inherit the base-type move for NC-05 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-06 — Reference Explicitness

- **id:** NC-06
- **name:** Reference Explicitness
- **moves_toward:** option "balanced mix" — human 37% against AI 16% — and away from "implicit echoes" — AI 72% against human 50% (both rows Table 16, p. 26). A mix of named and diffuse gestures, not all-named.
- **scope:** global (basis: detection_method; C1 inference under ). One document-level classification of the mix; the individual gestures are the driving spans and the places the move acts.
- **measured_importance:** 0.0135 (1.3% of total mass), rank 5 of the 30 cards — Tier 1 (heavy). `feature_cards.md` Part A.
- **fact_risk:** high — naming the work, author, outlet, dataset, or client behind a vague gesture is a fact. Unless the text or the user supplies the name, the skill inserts `[SOURCE NEEDED: <what would fit>]` at the span and lists it; it never invents one. Marker budget : when no sources are supplied, NC-06 and NC-21 together place at most two or three markers, at the spans that most drive the rating; every remaining vague attribution is listed in the STRUCTURAL CHANGE LOG as `unmarked: cap`, not marked.
- **never_do:**
  - Never invent an author, title, year, outlet, system, dataset, client name, or number to convert a vague reference into a named one.
  - Never fill a `[SOURCE NEEDED]` marker from memory, and never let the surface pass fill or delete one (the humanize invocation carries a preserve-markers line).
  - Never convert every implicit echo; "balanced mix" is the human option, and a text with only named references has moved past it.
  - Never exceed the shared marker budget. With no sources supplied, NC-06 and NC-21 together place at most two or three `[SOURCE NEEDED]` markers in the whole text, at the spans that most drive the rating; the rest of the vague attributions go into the log as `unmarked: cap` . The stop-condition "until named references are at least as many as vague ones" applies only when the user has supplied sources to name them with.
  - Never count a marker as a moved feature. In the NC-id DIFF, NC-06 stays `unmoved` or `pending: source needed` until a real name is in the text (UC-10).
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: where the text gestures at a work, myth, author, place, or brand without naming it, name it only if the text or the user has already established the name. With no source supplied, insert `[SOURCE NEEDED: named work / author / place this echo points to]` at the one or two echoes that most drive the rating, within the shared NC-06/NC-21 budget of two or three markers; log the rest `unmarked: cap`. Leave other echoes diffuse so the result is a mix. |
| CT-02 | literal: same; real names of people, places, works, and dates the author knows are supplied by the author, not by this skill; marker at the driving spans only, within the shared budget; the rest `unmarked: cap`. |
| CT-03 | analog: replace "studies show", "experts agree", "it is often said" with the named author, title, or outlet when supplied, continuing until named references are at least as many as vague ones. When no sources are supplied, insert `[SOURCE NEEDED: author / outlet / title behind "studies show"]` at the two or three vague attributions that most drive the rating (shared NC-06/NC-21 budget) and log every remaining vague attribution as `unmarked: cap`; do not mark them all. |
| CT-04 | analog: attach a specific citation to each vague attribution ("prior work has shown", "it is widely recognized") from the manuscript's own reference list or the user's supply, continuing until cited attributions outnumber vague ones. When no citation is available, insert `[SOURCE NEEDED: citation for "…"]` at the two or three vague attributions that most drive the rating (shared NC-06/NC-21 budget), delete a vague claim only if it carries nothing, and log the remaining vague attributions as `unmarked: cap`. |
| CT-05 | analog: replace "standard procedures", "widely used methods", "as is common" with the named method, tool, version, or dataset when supplied; otherwise `[SOURCE NEEDED: exact method / tool / version / dataset]` at the driving spans within the shared budget, the rest `unmarked: cap`. |
| CT-06 | analog: replace "previous studies", "it has been argued" with the cited comparison work when supplied; otherwise `[SOURCE NEEDED: cited comparison study]` at the driving spans within the shared budget, the rest `unmarked: cap`. |
| CT-07 | analog: replace "refer to the official documentation", "consult relevant resources" with the exact document section, URL, or version number when supplied; otherwise `[SOURCE NEEDED: exact URL / section / version]` at the driving spans within the shared budget, the rest `unmarked: cap`. |
| CT-08 | analog: replace "experts say", "officials", "critics argue" with the named person and title, organization, or document from the reporter's notes when supplied; otherwise `[SOURCE NEEDED: named source for "experts say"]` at the driving spans within the shared budget (the rest `unmarked: cap`), or cut the sentence if it carries nothing; never anonymize-to-name. |
| CT-09 | analog: replace "trusted by industry leaders", "thousands of teams" with a named client, verifiable number, or cited source when supplied; otherwise `[SOURCE NEEDED: named client / verifiable figure]` at the driving spans within the shared budget, the rest `unmarked: cap`; never invent a client or a number. |
| CT-10 | n/a |
| CT-11 | analog: replace "leading researchers", "a prominent lab" with the named person, paper, or institution when supplied; otherwise `[SOURCE NEEDED: named mentor / lab / paper]` at the driving spans within the shared budget, the rest `unmarked: cap`. |
| CT-12 | analog: inherit the base-type move for NC-06 per lean-map section; apply once per section; a section-to-section change in citation habit was a primary lean signal in the report, so the log records which section was edited. |
| CT-13 | analog: inherit the base-type move for NC-06 on the prose remainder; citations inside table cells are excluded and not edited. |
| CT-14 | analog: inherit the base-type move for NC-06; transliterated or translated titles and names count as named; language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-07 — Emotional Expression

- **id:** NC-07
- **name:** Emotional Expression
- **moves_toward:** option "explicit labels" — human 29% against AI 8% — and away from "embodied" — AI 81% against human 38% (both rows Table 16, p. 26). The paper's own contrast: a human author "might write that a character 'felt afraid,'" where AI "renders fear as a tightening chest, cold sweat, and dimming lamplight" (p. 7).
- **scope:** global (basis: detection_method; C1 inference under ). One document-level dominant-vehicle rating; the embodied renderings named as driving spans are where the conversions land.
- **measured_importance:** 0.0358 (3.6% of total mass), rank 2 of the 30 cards — Tier 1 (heavy). `feature_cards.md` Part A.
- **fact_risk:** low — naming an emotion the passage already renders adds no proposition; the skill never assigns an emotion the passage does not render, and in CT-08 never asserts a real subject's feeling that the reporting does not already state.
- **never_do:**
  - Never name an emotion the embodied passage does not already convey; the label replaces the vehicle, it does not add a feeling.
  - Never convert every embodied rendering; the human option is a mix in which labels are the most common vehicle, and 38% of human stories still lead with embodiment.
  - Never treat this as synonym work. The unit replaced is the whole embodied clause or sentence (the sensation, the metaphor), replaced by a statement of the feeling or by an observed behavior; word swaps inside the metaphor leave the vehicle embodied.
  - Never apply on rows where C2 marks the cell n/a (CT-04, CT-05, CT-06, CT-07, CT-09, CT-10, CT-15).
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: where emotion is rendered as bodily sensation or metaphor (chest, breath, hands, temperature, light), replace enough of those renderings with the named feeling ("she was afraid") or an outward behavior that embodiment is no longer the most common vehicle; keep some embodied renderings. |
| CT-02 | literal: same for the author's own feelings and those of real people already stated in the text; name only what the passage already renders. |
| CT-03 | analog: where emotion is rendered as an embodied metaphor ("a gut-punch", "a knot in the collective stomach"), replace the rendering with the named label ("I was angry") until labels are at least as many as embodied renderings; no move if the piece renders no emotion. |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: replace embodied metaphors in the reporter's voice ("grief hung in the room like smoke") with the named label already implied ("she was grieving") or with the subject's own words already quoted; never assert a feeling the reporting does not already contain. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | analog: replace the applicant's embodied metaphors ("a fire was lit within me") with the named feeling ("I was excited") until labels are at least as many as embodied renderings. |
| CT-12 | analog: inherit the base-type move for NC-07 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-07 on the prose remainder (n/a carries over). |
| CT-14 | analog: inherit the base-type move for NC-07; fixed idioms of the source language are not embodied metaphors and are not converted; language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-08 — Setting as Psychological Mirror

- **id:** NC-08
- **name:** Setting as Psychological Mirror
- **moves_toward:** lower on the 1–5 scale — human mean 3.58 against AI 4.07 (Table 16, p. 26). Thin toward the human rate: environment described independently of mood in most scenes, not never mirroring it.
- **scope:** global (basis: detection_method; C1 inference under ). One document-level rating; the mirroring passages named as driving spans are the targets.
- **measured_importance:** 0.0008 (0.1% of total mass), rank 27 of the 30 cards — Tier 3 (light). `feature_cards.md` Part A.
- **fact_risk:** none for cuts; low for decoupling, which replaces a mood-matched detail with a neutral detail drawn from the setting the text already establishes. No real-world fact enters.
- **never_do:**
  - Never invent a setting detail that the text's established setting does not support.
  - Never strip all mirroring; the human mean of 3.58 is well above the midpoint.
  - Never add setting description to a row whose cell is n/a; on CT-03 environment described to mirror a mood would reclassify the piece (C2).
  - Never treat weather or light as a fact to be preserved in fiction; in CT-02 and CT-08 the weather on a real day is a ledger item and is cut, not changed.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: cut or decouple the weather, light, room, and landscape descriptions that shift with a character's mood, starting with the driving spans; leave the environment described on its own terms in most scenes, mirroring in a few. |
| CT-02 | literal: same cuts; where the description is of a real place on a real day, cut rather than alter it. |
| CT-03 | n/a |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: cut every setting description in the reporter's voice that is used to mirror a subject's mood (weather for grief, clutter for chaos); a factual setting detail may stand in only if it is already in the reporting. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | n/a |
| CT-12 | analog: inherit the base-type move for NC-08 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-08 on the prose remainder (n/a carries over). |
| CT-14 | analog: inherit the base-type move for NC-08 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-09 — Environmental & Ecological Emphasis

- **id:** NC-09
- **name:** Environmental & Ecological Emphasis
- **moves_toward:** lower on the 1–5 scale — human mean 2.83 against AI 3.21 (Table 16, p. 26). The natural world present but less prominent.
- **scope:** global (basis: detection_method; C1 inference under ). One document-level rating plus driving spans (extended landscape, weather, or wildlife passages).
- **measured_importance:** 0.0003 (0.0% of total mass), rank 30 of the 30 cards — Tier 3 (light). `feature_cards.md` Part A.
- **fact_risk:** none — cuts and shortenings of description; nothing is added.
- **never_do:**
  - Never cut a nature passage in which the natural world acts on the plot or the characters (a storm that strands them, a season that sets a deadline).
  - Never remove the natural environment where it is the subject (nature writing, an ecology feature); the C2 analog on CT-08 counts only references not required by the subject.
  - Never add environmental detail anywhere as compensation for other cuts.
  - Never apply on the n/a rows, where mentions of nature are topic content, not a structural choice (C2).
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: shorten extended passages of landscape, weather, plant, and animal description, and cut those that neither act on the plot nor place a scene; thin toward the human rate, not to absence. |
| CT-02 | literal: same shortenings and cuts. |
| CT-03 | n/a |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: cut references to weather, landscape, or the natural environment not required by the story's subject until fewer than one per scene paragraph remains. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | n/a |
| CT-12 | analog: inherit the base-type move for NC-09 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-09 on the prose remainder (n/a carries over). |
| CT-14 | analog: inherit the base-type move for NC-09 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-10 — Sensory Modalities

- **id:** NC-10
- **name:** Sensory Modalities
- **moves_toward:** "olfactory" not among the most frequently engaged modalities — human 57% against AI 82% (Table 16, p. 26). Smell present in some scenes, not a leading sense.
- **scope:** global (basis: detection_method; orchestrator ruling ). One document-level rating of the dominant modalities; the most prominent smell images are the driving spans and the targets.
- **measured_importance:** 0.0099 (1.0% of total mass), rank 8 of the 30 cards — Tier 2. `feature_cards.md` Part A.
- **fact_risk:** none for cuts; low where a smell image is re-sensed into a modality the scene already supports (what is seen or heard in that place). No real-world fact enters.
- **never_do:**
  - Never remove every smell image; 57% of human stories still engage smell frequently.
  - Never re-sense an image into a detail the scene cannot support (a sound in a scene established as silent).
  - Never treat this as diction work; the unit is the sensory image, and the move is to cut it or change which sense it engages, not to change the words for the same smell.
  - Never cut olfactory detail from a text whose subject is sensory (food, perfume, a market) on CT-08; C2's analog exempts those.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: cut the most prominent smell and scent descriptions (of places, people, objects, food, weather) or re-sense them into what the scene already shows or sounds like, until smell is no longer among the story's most frequently engaged modalities; leave some. |
| CT-02 | literal: same on the piece's smell images. |
| CT-03 | n/a |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: cut every olfactory or bodily-sensation detail in the reporter's voice unless the story's subject is itself sensory; a story about a bakery keeps its smells, a story about a zoning hearing does not. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | n/a |
| CT-12 | analog: inherit the base-type move for NC-10 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-10 on the prose remainder (n/a carries over). |
| CT-14 | analog: inherit the base-type move for NC-10 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-11 — Sensory Density

- **id:** NC-11
- **name:** Sensory Density
- **moves_toward:** lower on the minimal–lush scale — human mean 3.66 against AI 3.93 (Table 16, p. 26). The paper places this feature on the non-style side of its own style rule (p. 18), which is why cutting sensory clauses is a structural move here rather than surface work.
- **scope:** global (basis: detection_method; C1 inference under ). One document-level rating; the densest sensory paragraphs are the driving spans and the targets.
- **measured_importance:** 0.0012 (0.1% of total mass), rank 26 of the 30 cards — Tier 3 (light). `feature_cards.md` Part A.
- **fact_risk:** none — the move cuts sensory clauses; nothing is added.
- **never_do:**
  - Never strip sensory detail from every scene; the target is at least one scene that proceeds with little texture while others keep theirs.
  - Never cut a sensory clause that carries plot information (the smell of gas, the sound of the key).
  - Never rewrite the sensory clause more plainly instead of cutting it; that leaves the density and is surface work besides.
  - Never add sensory detail to a row whose cell is n/a.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: in the densest sensory paragraphs, cut sensory clauses until at least one scene proceeds through event and speech with little sensory texture; keep the detail where it does plot or character work. |
| CT-02 | literal: same cuts on the piece's densest passages. |
| CT-03 | n/a |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: cut sensory-descriptive clauses in scene paragraphs until no scene paragraph carries more than one. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | n/a |
| CT-12 | analog: inherit the base-type move for NC-11 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-11 on the prose remainder (n/a carries over). |
| CT-14 | analog: inherit the base-type move for NC-11 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-12 — Depth of Interior Access

- **id:** NC-12
- **name:** Depth of Interior Access
- **moves_toward:** lower on the 1–5 scale — human mean 3.67 against AI 3.93 (Table 16, p. 26). Less sustained dwelling in thought, memory, and motive; some scenes seen from outside. On the non-style side of the paper's style rule (p. 18).
- **scope:** global (basis: detection_method; C1 inference under ). One document-level rating; passages of sustained interior monologue are the driving spans and the targets.
- **measured_importance:** 0.0014 (0.1% of total mass), rank 24 of the 30 cards — Tier 3 (light). `feature_cards.md` Part A.
- **fact_risk:** none for cuts; low for externalizing, which renders a reported thought or motive as speech or action the scene already implies. In CT-02 and CT-08 an assertion about a real person's inner state is cut or attributed to something they said or did that is already in the text, never re-attributed to invented speech.
- **never_do:**
  - Never invent a line of speech or an action to externalize a thought; use what the scene already implies, or cut.
  - Never remove interior access that carries plot information (a plan, a secret) with no other carrier.
  - Never externalize every passage; the human mean of 3.67 is above the midpoint.
  - Never delete an explicit emotion label while cutting the interiority around it. The labels live inside this move's driving spans, and converting them to outward behavior pushes NC-07 from its human-side value (explicit labels, 29% human / 8% AI) to its AI-side one (embodied, 81% AI / 38% human). Preserve the label, cut the passage around it, and log `held: span carries human-side NC-07`. This collision was measured on three stories in the validation study.
  - Never assert or keep an unattributed claim about a real person's thoughts in CT-02 or CT-08 unless the text attributes it to something they said or did.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: cut or externalize passages of sustained interior monologue and reported motive, rendering the same content as speech or action the scene already implies; leave at least one scene rendered wholly from outside. |
| CT-02 | analog: for each sentence asserting the inner thoughts or feelings of a person other than the narrator without attribution, cut it or attribute it to something that person said or did that the text already records; the narrator's own interior is genre-normal and stays. |
| CT-03 | n/a |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: cut every unattributed assertion of a subject's inner thoughts or feelings, or attribute it to a quote or observed action already in the reporting; never add a quote. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | n/a |
| CT-12 | analog: inherit the base-type move for NC-12 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-12 on the prose remainder (n/a carries over). |
| CT-14 | analog: inherit the base-type move for NC-12 (n/a carries over); act only on unambiguous interior access, since languages mark free indirect thought differently; language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-13 — Causal Chain Continuity

- **id:** NC-13
- **name:** Causal Chain Continuity
- **moves_toward:** lower on the 1–5 scale — human mean 3.92 against AI 4.20 (Table 16, p. 26). The paper's description of the human side: "human stories are messier, with time jumps and disjointed causal chains" and AI "favors single-track narratives with fewer loose ends" (p. 7). One loose end, not a broken chain.
- **scope:** global (basis: detection_method; C1 inference under ). One document-level rating; the hinge sentences and any existing loose end are the driving spans.
- **measured_importance:** 0.0093 (0.9% of total mass), rank 11 of the 30 cards — Tier 2. `feature_cards.md` Part A.
- **fact_risk:** low — the move removes a stated causal link or leaves an existing counter-consideration standing, from material already in the text. On CT-04, CT-07, and CT-08 a counter-consideration, failure path, or contradiction not already in the text is a fact: it gets a `[SOURCE NEEDED]` marker, never an invention.
- **never_do:**
  - Never invent an event, a competing explanation, a failure mode, or a contradicting source to break the chain.
  - Never remove a causal link the ending depends on; one consequence arriving without its stated cause is the target.
  - Never do the connective work. C2's analogs on CT-03, CT-04, CT-07, and CT-08 count paragraph-opening connectives together with unresolved tensions; the connectives are transition words and belong to the surface pass at the surface pass (by path). This skill acts only on the tension half of each analog, which alone breaks the AI-side condition.
  - Never apply on CT-05, where procedural order is genre-mandated (C2).
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: leave one loose end: cut a hinge sentence so that one consequence arrives without its stated cause, or let one event stand that the preceding events do not explain; make one scene transition abrupt (no bridging sentence). Keep the chain the ending needs. |
| CT-02 | literal: same on the chain of real events as told: remove a stated link the author supplied as explanation, or restore an unexplained event the author's account contains; never add an event. |
| CT-03 | analog: raise one counter-consideration the essay already mentions and leave it unresolved (stop dismissing it in the same paragraph); this alone breaks C2's AI-side condition. Paragraph-opening connectives are left for the surface pass. |
| CT-04 | analog: between the gap statement and the contribution, leave one competing explanation or open dispute standing, taken from the manuscript's own related-work material or the user's supply; if none is available, insert `[SOURCE NEEDED: competing explanation or open dispute in the field]` rather than inventing one. |
| CT-05 | n/a |
| CT-06 | analog: leave at least one limitation standing: cut the same-paragraph neutralizing sentence ("however, this is unlikely to affect…") so the tension remains. |
| CT-07 | analog: add one acknowledged failure path or branch ("if X fails", "on Windows", "alternatively") from behavior the material or the user supplies; if none is supplied, insert `[SOURCE NEEDED: known failure mode or platform variant for this step]`; step connectives are left for the surface pass. |
| CT-08 | analog: leave one contradiction between sources unresolved: cut the reporter's reconciling sentence; never add a source or a document. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | analog: leave one setback standing as a setback: cut the same-paragraph sentence that converts it into a lesson or strength. |
| CT-12 | analog: inherit the base-type move for NC-13 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-13 on the prose remainder; a step-by-step code listing is not a causal chain and is not edited. |
| CT-14 | analog: inherit the base-type move for NC-13; language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-14 — Spatial Granularity

- **id:** NC-14
- **name:** Spatial Granularity
- **moves_toward:** lower on the very_low–high ordinal — human mean 2.27 against AI 2.53 (Table 16, p. 26). Space left schematic in some scenes.
- **scope:** global (basis: detection_method; C1 inference under ). One document-level rating; the most finely rendered spaces are the driving spans and the targets.
- **measured_importance:** 0.0016 (0.2% of total mass), rank 22 of the 30 cards — Tier 3 (light). `feature_cards.md` Part A.
- **fact_risk:** none — cuts of spatial detail; nothing is added.
- **never_do:**
  - Never cut spatial detail that the action depends on (the locked door, the distance to the exit).
  - Never cut every rendered space; the target is at least one scene set in an unspecified "somewhere".
  - Never add spatial detail anywhere.
  - Never apply on rows where physical space is not depicted (C2 n/a cells).
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: cut object-level spatial detail (layouts, distances, room inventories) from at least one scene so that space is schematic there; keep the detail where movement through specified space matters. |
| CT-02 | literal: same cuts. |
| CT-03 | n/a |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: cut object-level physical detail (the specific mug, the peeling paint) from at least one scene paragraph so that not every scene paragraph carries it. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | n/a |
| CT-12 | analog: inherit the base-type move for NC-14 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-14 on the prose remainder (n/a carries over). |
| CT-14 | analog: inherit the base-type move for NC-14 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-15 — Agency in Resolution

- **id:** NC-15
- **name:** Agency in Resolution
- **moves_toward:** away from "protagonist choice" toward "mixed" or "external_fate" — human 46% "protagonist choice" against AI 69% (Table 16, p. 26).
- **scope:** local (basis: detection_method; C1 inference under ). The resolution passage and the precipitating decision have exact spans.
- **measured_importance:** 0.0054 (0.5% of total mass), rank 16 of the 30 cards — Tier 2. `feature_cards.md` Part A.
- **fact_risk:** low — in fiction the resolution is re-caused from an element already seeded in the text; in every nonfiction row the cause of a real outcome is a fact, so the move can only restore an external cause the account already mentions or cut the choice framing, and otherwise logs `skipped: outcome was the person's choice`.
- **never_do:**
  - Never change the cause of a real outcome (CT-02, CT-08, CT-11) unless the text or the user establishes the external cause.
  - Never introduce a new character, accident, or coincidence to resolve the plot; use what the text has seeded.
  - Never make the protagonist passive throughout; the target is the resolution, and "mixed" is a legitimate landing.
  - Never apply on CT-04, CT-05, CT-07, CT-10, where C2 marks the cell n/a.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: re-cause the resolution so the outcome arrives through an external event, another character's act, or a coincidence already seeded in the text, or make it mixed (choice plus circumstance); rewrite the decision passage so it precipitates less. |
| CT-02 | literal: only where the real events allow: restore an external cause the author's account mentions (luck, another person, an institution) and reduce the "I chose" framing; if the outcome was in fact the person's choice, log `skipped: outcome was the person's choice`. |
| CT-03 | analog: rewrite the close so resolution is located in structural or external forces the essay already names, or is left open, rather than in an individual-choice exhortation ("it is up to each of us"). |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | analog: remove the future-work promise from at least one limitation and attribute the open issue to an external constraint already stated (data, cost, access) or leave it open. |
| CT-07 | n/a |
| CT-08 | analog: re-anchor the close to an external event, institution, or pending process already reported instead of the subject's own decision; the facts are unchanged; if the reported outcome was the subject's choice, log `skipped`. |
| CT-09 | analog: replace the closing choice exhortation ("take control", "it's your move") with what the product does, stated from the copy's own claims. |
| CT-10 | n/a |
| CT-11 | analog: attribute at least one turning point in the applicant's path to circumstance, luck, or another person where the account allows (a mentor, a chance encounter already mentioned); never invent a person. |
| CT-12 | analog: inherit the base-type move for NC-15 (n/a carries over); act on the paragraph(s) that carry the resolution. |
| CT-13 | analog: inherit the base-type move for NC-15 on the prose remainder (n/a carries over). |
| CT-14 | analog: inherit the base-type move for NC-15 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | analog: if the text ends with an outcome attributed to a person's choice, re-anchor it to an external cause the text already names; otherwise log `skipped: no outcome / no external cause in text`; confidence Low. |

### NC-16 — Character Introduction

- **id:** NC-16
- **name:** Character Introduction
- **moves_toward:** away from "external description" — human 30% against AI 52% (Table 16, p. 26) — toward an introduction in action, in dialogue, in inner thought, or through another's report. Per this is a core all-AI feature and is never attributed to any single model.
- **scope:** local (basis: detection_method; C1 inference under ). The introducing passage has an exact span.
- **measured_importance:** 0.0166 (1.7% of total mass), rank 4 of the 30 cards — Tier 1 (heavy). `feature_cards.md` Part A.
- **fact_risk:** low — the move reorders material already in the text: the first appearance opens on an action, line, or thought the text already gives the character, and the descriptive facts (age, title, appearance) move later. In nonfiction rows the descriptor facts are ledger items and are relocated, never dropped or altered.
- **never_do:**
  - Never invent an action or a line of speech for a real person (CT-02, CT-08, CT-10, CT-11); use one the text already records.
  - Never delete the descriptor facts in a nonfiction row; place them after the action.
  - Never attribute this feature to Gemini or to any single model .
  - Never apply on rows where no character is introduced (C2 n/a cells).
  - Never treat the landing options as three. The card's response set is external_desc, in-action, in-dialogue, inner_thought, others_reports. The HANDOFF line and this cell now offer the same four non-external options.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: reorder the central character's first appearance so it opens with one of the card's four non-external landings — an action in progress, a line of dialogue, an inner thought, or **another character's report of them** (`others_reports`) — using material already in the text; move the external description (looks, clothing, age, situation seen from outside) later, or cut it. **Dead, absent, or silent protagonist:** where the central figure never acts, speaks, or thinks in the text (a character already dead at the opening, or present only in others' accounts), `others_reports` is the landing — introduce them through what another character claims, remembers, or says of them. Only if that too is unavailable, log `skipped: protagonist neither acts, speaks, thinks, nor is reported`. |
| CT-02 | literal: same for the first appearance of each real person; their real attributes relocate, they do not vanish. |
| CT-03 | analog: for each person introduced by a descriptor tag ("a 34-year-old teacher from Ohio", "renowned economist"), lead with what they said or did (already in the text) and place the tag after, until action-first introductions are at least as many as descriptor-first. |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: introduce the central subject through an act or a quote already in the story; the descriptor list ("a soft-spoken 52-year-old nurse and mother of three") follows it. |
| CT-09 | n/a |
| CT-10 | analog: where the writer introduces themself or another person (cover letters, introductions), lead with a concrete action or event already in the message ("I shipped X", "we met at Y") and put the descriptor list ("a results-driven professional…") after it, or cut the list. |
| CT-11 | analog: open the applicant's self-introduction on a concrete event from the statement rather than a trait list ("I am a passionate, detail-oriented researcher"); the traits follow or go. |
| CT-12 | analog: inherit the base-type move for NC-16 (n/a carries over); act per paragraph at each first introduction. |
| CT-13 | analog: inherit the base-type move for NC-16 on the prose remainder (n/a carries over). |
| CT-14 | analog: inherit the base-type move for NC-16 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | analog: at the first mention of any person, lead with an action or speech already in the text and place the descriptor list after; no move if no person is introduced; confidence Low. |

### NC-17 — Subplot Integration

- **id:** NC-17
- **name:** Subplot Integration
- **moves_toward:** option "thematically parallel" — human 42% against AI 21% — and away from "no subplots" — AI 79% against human 57% (both rows Table 16, p. 26). One secondary thread that echoes the central concern and is not folded into the main line.
- **scope:** global (basis: detection_method; C1 inference under ). One document-level rating; the entry points of any secondary thread are the driving spans.
- **measured_importance:** 0.0226 (2.3% of total mass), rank 3 of the 30 cards — Tier 1 (heavy). `feature_cards.md` Part A.
- **fact_risk:** low — **on fiction this move writes new diegetic material and that is permitted** (SKILL.md Hard rule 1 and Step 2: propositions invented inside the story-world are not ledger items and are not "inventing a fact"; the prohibition is on real-world facts and is unchanged on every non-fiction row). Ceiling on the licence: the new thread introduces no new proper noun, date, or figure — it acts through elements the text already names. The thread is built from material already present (a secondary character with a mentioned stake, a second time period, a side observation) or supplied by the user. In fiction, new story material is permitted only when developed from an element already in the text and logged as `added from existing element P[n]`; in every nonfiction row no event, person, result, or edge case is invented, and where the material has none the log says `skipped: no material` or a `[SOURCE NEEDED]` marker is placed (CT-07).
- **never_do:**
  - Never invent a real event, person, result, or edge case to make a second thread.
  - Never add a thread that resolves or explains the main plot; that tightens NC-13 back toward the AI side. The thread runs beside the main line, resolved separately or left open.
  - Never add an unrelated thread; the human option is thematically parallel, and NC-03's target (one un-reconciled element) is met by not tying the thread back in the ending, not by making it random.
  - Never add a subplot on CT-05, CT-09, CT-10, or CT-15, where C2 marks the cell n/a.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: develop one secondary thread from an element already in the text (a named secondary character's own stake, a mentioned past event) into two or three short passages interleaved with the main line, thematically parallel to the central concern, resolved separately or left open; log `added from existing element P[n]`. |
| CT-02 | analog: give one secondary thread the piece already mentions (another person's story, a second time period, a parallel event) at least one paragraph of its own, from the author's material or supply; never invent. |
| CT-03 | analog: let one secondary line of argument or one extended example stand as its own paragraph rather than being folded back into the thesis; built from material already in the essay. |
| CT-04 | analog: name one side observation, secondary contribution, or negative result the manuscript actually contains (elsewhere in the paper or supplied) as a standalone item in the introduction; if the manuscript has none, log `skipped: no material` rather than inventing a result. |
| CT-05 | n/a |
| CT-06 | analog: give one unexpected result, side observation, or alternative explanation already present in the manuscript a paragraph of its own. |
| CT-07 | analog: add one subsection for an alternative, edge case, platform variation, or troubleshooting item the material or the user supplies; if none is supplied, insert `[SOURCE NEEDED: known edge case / alternative / platform variant]` under a subsection heading rather than inventing behavior. |
| CT-08 | analog: give one secondary person or angle already in the reporting paragraphs of its own; never add a source. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | analog: give one side project, second field, or unrelated interest the applicant mentions a paragraph of its own; never invent. |
| CT-12 | analog: inherit the base-type move for NC-17 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-17 on the prose remainder; an appendix table of alternatives counts as a side-track only if prose discusses it, so the move adds the prose, not the table. |
| CT-14 | analog: inherit the base-type move for NC-17 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-18 — Resolution Mode

- **id:** NC-18
- **name:** Resolution Mode
- **moves_toward:** away from "internal understanding" — human 27% against AI 47% (Table 16, p. 26) — toward "resolved externally" or "unresolved"; the paper notes humans "are more comfortable with ambiguous endings" (p. 7).
- **scope:** local (basis: detection_method; C1 inference under ). The resolving passage has an exact span.
- **measured_importance:** 0.0060 (0.6% of total mass), rank 15 of the 30 cards — Tier 2. `feature_cards.md` Part A.
- **fact_risk:** low — the ending is re-closed on an external act or event already seeded, or left open by cutting the realization sentence; in nonfiction rows the closing action, plan, price, or next step must already be in the text.
- **never_do:**
  - Never invent a closing event, plan, price, date, or next step; close on one the text already contains, or leave the ending open.
  - Never leave a text open where the register mandates a close and C2 marks the cell n/a (CT-04, CT-05, CT-07).
  - Never resolve by adding a second realization in a character's mouth; that still fires.
  - Never cut the realization if it carries a claim stated nowhere else and the row is argumentative; relocate it earlier and end on something else.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: cut the sentence(s) in which the character comes to understand or accept, and close the main chain on an external act or event already seeded, or leave it unresolved. |
| CT-02 | literal: same within the actual events: end on the last external event the account contains, or leave the ending open; cut the realization sentence. |
| CT-03 | analog: close on a concrete proposal or an open question the essay has already raised; cut the closing realization or reframing ("what we come to see is…"). |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | analog: end the section on a concrete open question or an unresolved contradiction already discussed rather than on a synthesis ("taken together, our findings reveal…"). |
| CT-07 | n/a |
| CT-08 | analog: end on a concrete pending action or open question already in the reporting; cut the realization or emotional-resolution close ("has found peace", "now understands"). |
| CT-09 | analog: close on the concrete action with its mechanics already stated in the copy (price, trial length, exact next step); cut the realization close ("discover a new way of working"); never invent a price or term. |
| CT-10 | analog: close on a specific ask, time, or next step already in the message; cut the reflective close ("excited about what's ahead"). Under UC-03 the sample gate wins: when the voice sample has no reader-directed questions, the specific ask is declarative ("I need the figures by Thursday"), not a question to the recipient . |
| CT-11 | analog: close on the named aim, timeline, mentor, or resource already stated; cut the realization synthesis ("I have come to understand that…"). |
| CT-12 | analog: inherit the base-type move for NC-18 (n/a carries over); act on each section's closing paragraph. |
| CT-13 | analog: inherit the base-type move for NC-18 on the prose remainder (n/a carries over). |
| CT-14 | analog: inherit the base-type move for NC-18 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | analog: close on a concrete action or an open question already in the text; cut the realization or reframing; confidence Low. |

### NC-19 — Opening Spatial Grounding

- **id:** NC-19
- **name:** Opening Spatial Grounding
- **moves_toward:** lower rung on the none/vague – minimal – clear local – clear local+global ordinal — human mean 2.12 against AI 2.33 (Table 16, p. 26). The opening places the reader less firmly before events begin.
- **scope:** local (basis: detection_method; C1 inference under ). The opening passage has an exact span.
- **measured_importance:** 0.0025 (0.3% of total mass), rank 18 of the 30 cards — Tier 2. `feature_cards.md` Part A.
- **fact_risk:** none for cuts of establishing description; low for reordering, which moves the place detail after the first event. Nothing is added.
- **never_do:**
  - Never invent an opening action or line; open on one the text already contains.
  - Never delete the setting entirely; relocate the grounding after the first event or thin it to the minimal rung.
  - Never touch a template-mandated opening (CT-05 participants and materials; CT-07 title and prerequisites; CT-09 headline), where C2 marks the cell n/a.
  - Never confuse this with humanize's advice to open with a scene; that concerns thesis-first openers, this concerns how firmly the opening maps physical space.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: open on action, speech, or thought already in the text and delay or thin the establishing description of place; the spatial detail follows the first event. |
| CT-02 | literal: same reordering. |
| CT-03 | analog: cut the broad context-frame opening ("In today's world of…", "For decades, X has…") and open on the specific claim, fact, or question the piece already makes. |
| CT-04 | analog: cut the broad field-frame opener ("X is a fundamental problem in Y", "In recent years, … has attracted increasing attention") and open on the specific observation, number, or claim the introduction already contains. |
| CT-05 | n/a |
| CT-06 | analog: open the discussion on a finding rather than a restatement of the study's aim or context ("In this study, we set out to…"). |
| CT-07 | n/a |
| CT-08 | analog: open the lede on a fact, event, or quote already in the story; move the full scene frame ("On a gray morning in the back room of…") later. |
| CT-09 | n/a |
| CT-10 | analog: cut the context frame that restates what the recipient already knows ("I hope this finds you well. I am writing to follow up regarding…") and open on the point. |
| CT-11 | analog: open on a specific moment, number, or claim already in the statement rather than a broad frame ("Since childhood, I have been fascinated by…"). |
| CT-12 | analog: inherit the base-type move for NC-19 (n/a carries over); act on each section's opening paragraph. |
| CT-13 | analog: inherit the base-type move for NC-19 on the first prose paragraph after any leading code or table block (n/a carries over). |
| CT-14 | analog: inherit the base-type move for NC-19 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | analog: open on a specific claim, fact, or event already in the text and cut the context frame; confidence Low. |

### NC-20 — Pre-Threat Character Investment

- **id:** NC-20
- **name:** Pre-Threat Character Investment
- **moves_toward:** lower on the 1–5 scale — human mean 2.76 against AI 2.99 (Table 16, p. 26). Jeopardy, problem, or gap arrives sooner; setup is shorter or comes after.
- **scope:** global (basis: detection_method; C1 inference under ). One document-level rating; the onset of jeopardy and the principal setup passages are the driving spans.
- **measured_importance:** 0.0007 (0.1% of total mass), rank 28 of the 30 cards — Tier 3 (light). `feature_cards.md` Part A.
- **fact_risk:** none — cuts and relocations of setup material; nothing is added.
- **never_do:**
  - Never cut setup that later events depend on; relocate it after the threat instead.
  - Never invent an earlier threat; move the existing one up.
  - Never remove all setup; the human mean of 2.76 is not 1.
  - Never apply on rows with no jeopardy or investment (C2 n/a cells: CT-05, CT-07, CT-09, CT-10, CT-15).
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: bring the first major jeopardy earlier by cutting or relocating investment-building passages (routine, relationships, wants) from before it to after it; keep some setup. |
| CT-02 | literal: same with the real events as told: reorder the telling, do not invent. |
| CT-03 | analog: cut or relocate setup so the problem or tension the essay addresses is stated within the first paragraph. |
| CT-04 | analog: move the gap or problem statement up so that at most one introduction paragraph (or at most two abstract sentences) of background precedes it; relocate the rest of the background after it, or cut. |
| CT-05 | n/a |
| CT-06 | analog: cut recap so the first limitation or tension is raised within the first paragraph of the discussion. |
| CT-07 | n/a |
| CT-08 | analog: move the central conflict or event up so that at most two paragraphs of background or character setup precede it; relocate the rest after. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | analog: name the first obstacle, problem, or gap within the first paragraph; relocate background after it. |
| CT-12 | analog: inherit the base-type move for NC-20 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-20 on the prose remainder (n/a carries over). |
| CT-14 | analog: inherit the base-type move for NC-20 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-21 — Intertextual Strategy

- **id:** NC-21
- **name:** Intertextual Strategy
- **moves_toward:** option "explicit named reference" present — human 47% against AI 24% (Table 16, p. 26); the paper: humans "reference specific texts and authors at nearly double the AI rate" while AI "avoids naming real brands, places, or works" (p. 7).
- **scope:** local (basis: detection_method; C1 inference under ). Each named reference, and each span where one would fit, has an exact location.
- **measured_importance:** 0.0126 (1.3% of total mass), rank 6 of the 30 cards — Tier 1 (heavy). `feature_cards.md` Part A.
- **fact_risk:** high — every named author, title, work, system, dataset, version, date, or figure is a fact. Unless the text or the user supplies it, the skill inserts `[SOURCE NEEDED: <what would fit>]` at the span and lists it; it never invents one, and a marker never counts as movement (UC-10). Marker budget : shared with NC-06, at most two or three markers in the whole text when no sources are supplied; the rest `unmarked: cap`.
- **never_do:**
  - Never invent an author, title, year, system, dataset, version number, parameter value, person, date, or figure.
  - Never manufacture a span so the move has somewhere to land. Where NC-21 fires by absence and the text neither names nor gestures at anything, the honest outcome is `skipped: no vague attribution to mark`, not an annotation on a simile.
  - Never fill a marker from memory, and never let the surface pass fill or delete one (preserve-markers line in the invocation).
  - Never exceed the shared marker budget : with no sources supplied, NC-21 and NC-06 together place at most two or three `[SOURCE NEEDED]` markers in the whole text, at the spans that most drive the rating; every other span that would take a named reference is logged `unmarked: cap`, not marked. Rates such as "one named specific per ~300 words" are reached only by naming with supplied sources, never by markers.
  - Never report NC-21 as `moved` in the NC-id DIFF on the strength of a marker; it is `unmoved` or `pending: source needed` until a real reference is in the text.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: **two branches.** (a) *An unnamed allusion exists* — the text gestures at a real author, work, figure from another text, brand, or place without naming it: name it if the text or the user has established the name; otherwise annotate the one or two spans that most drive the rating, within the shared NC-21/NC-06 budget, and log the rest `unmarked: cap`. (b) *The feature fires by absence* — the report's evidence reads `absence (nothing to quote)` and the text makes no allusion at all: annotate only a span where the text **reports, quotes, or attributes something to an unnamed source** ("he had read the articles"; a technique credited to a tradition; a remembered maxim from a book never named). If there is no such span, log `skipped: no vague attribution to mark; NC-21 fires by absence and this text names nothing` and place nothing. Never manufacture a span: annotating a bare simile or an image that points at no real work is unfillable by construction, and two rewriters independently produced non-reproducible placements that way. **Premise-level intertextuality** — the whole text reframes a canonical work without naming it — has no span either: log `skipped: intertextuality is premise-level, no span to mark`. Every annotation is removed or resolved before delivery (SKILL.md Step 8). |
| CT-02 | literal: same; real names, places, works, and dates come from the author, not from this skill; markers within the shared budget, the rest `unmarked: cap`. |
| CT-03 | analog: replace generic plurals ("many writers", "some critics") with the specific person, work, or figure when supplied; otherwise `[SOURCE NEEDED: specific work / person / figure behind "many writers"]` at the spans that most drive the rating, within the shared NC-21/NC-06 budget of two or three markers, the rest `unmarked: cap`; the rate of at least one named specific per ~300 words (C2's proposal, that cell's `threshold:` verdict in `content_type_matrix.md` §0.8) is a target only when sources are supplied. |
| CT-04 | analog: replace "existing methods", "many approaches" with the named system, dataset, benchmark, or prior paper where the manuscript's own references supply it; otherwise `[SOURCE NEEDED: named system / dataset / paper for "existing methods"]` at the driving spans within the shared budget, the rest `unmarked: cap`. |
| CT-05 | analog: replace "a standard dataset", "appropriate parameters", "commonly used settings" with the exact dataset name, software version, instrument, or parameter value when supplied; otherwise `[SOURCE NEEDED: exact dataset / version / parameter value]` at the driving spans within the shared budget, the rest `unmarked: cap`; never guess a version number or a value. |
| CT-06 | analog: replace "prior methods", "other approaches" with the particular comparison study, number, dataset, or effect size when supplied; otherwise `[SOURCE NEEDED: named comparison study / effect size]` at the driving spans within the shared budget, the rest `unmarked: cap`. |
| CT-07 | n/a |
| CT-08 | analog: replace anonymous plurals with people's full names and titles, documents, dates, figures, and places from the reporter's notes when supplied; otherwise `[SOURCE NEEDED: full name and title / document / date]` at the driving spans within the shared budget, the rest `unmarked: cap`; never anonymize-to-name. |
| CT-09 | n/a |
| CT-10 | analog: add the specific person, date, document title, number, or prior message referred to by content, from the message's own context or the user's supply; otherwise `[SOURCE NEEDED: specific date / document / person]` at the driving spans within the shared budget, the rest `unmarked: cap`. |
| CT-11 | analog: replace generic references with the named lab, paper, mentor, technique, number, or date when supplied; otherwise `[SOURCE NEEDED: named lab / paper / mentor]` at the driving spans within the shared budget, the rest `unmarked: cap`. |
| CT-12 | analog: inherit the base-type move for NC-21; act per paragraph; a run of paragraphs with zero named specifics was a primary lean signal in the report, so the log records the run edited. |
| CT-13 | analog: inherit the base-type move for NC-21 on the prose remainder; identifiers, URLs, and version strings inside code blocks are not named references and are not targets. |
| CT-14 | analog: inherit the base-type move for NC-21; named references in any script count; language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | analog: add named specifics (people, works, places, dates, figures) from the text's own context or the user's supply toward at least one per ~300 words (C2's proposal); otherwise `[SOURCE NEEDED: …]` within the shared budget, the rest `unmarked: cap`; confidence Low. |

### NC-22 — Fourth-Wall Permeability

- **id:** NC-22
- **name:** Fourth-Wall Permeability
- **moves_toward:** higher rung — human 0.67 against AI 0.39 as Table 16 prints them (p. 26), 67% against 39% as §4.1 prints them (p. 7); the paper does not reconcile the two encodings . Read as prevalence: at least one moment where the text acknowledges its reader or its own telling. The paper's example: "an aside to 'you, dear reader'" (p. 7).
- **scope:** local (basis: detection_method; C1 inference under ). Each break has an exact span; the insertion point is chosen by the skill and logged.
- **measured_importance:** 0.0067 (0.7% of total mass), rank 14 of the 30 cards — Tier 2. `feature_cards.md` Part A.
- **fact_risk:** none in fiction and argument (the aside is the author's own); low on CT-08, where an acknowledgment of the reporting act ("I asked", "as this newspaper reported") is a claim about what the reporter did and must be confirmed by the user or skipped.
- **never_do:**
  - Never insert an aside as filler. One break where the narration already pauses, or none.
  - Never insert on rows where C2 marks the cell n/a (CT-04, CT-05, CT-06, CT-09, CT-11); the register forbids it and absence is required, not telling.
  - Never assert a reporting act that did not happen (CT-08).
  - Never introduce a break the voice sample never uses (UC-03); log `skipped: absent from voice sample`.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: insert one aside in which the narrator acknowledges the reader or the act of telling, at a point where the narration already pauses; at most two in a long text; "you, dear reader" is the paper's own example of the human pattern. |
| CT-02 | literal: one remark in which the narrator acknowledges the act of writing or remembering ("I have told this story badly before"); this counts as a break on this row (C2). |
| CT-03 | analog: insert one remark that acknowledges the essay as an essay or the reader as reading ("bear with me", "you're probably thinking") where the argument turns; roughly one per ~800 words at most (a build proposal, not a paper figure), never as filler. |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | analog: insert one remark anticipating the reader's likely state ("if you're impatient, skip to…", "this part is confusing, bear with me") at a point of real difficulty or a real shortcut. |
| CT-08 | analog: insert one acknowledgment of the reporting act or the reader ("I asked", "readers may recall", "as this newspaper reported") only where the user confirms the act occurred as described; otherwise log `skipped: reporting act unconfirmed`. |
| CT-09 | n/a |
| CT-10 | analog: insert one remark acknowledging the message as a message ("tl;dr", "long story short", "sorry for the wall of text") if the register of the message allows it; do not force it into a formal note. |
| CT-11 | n/a |
| CT-12 | analog: inherit the base-type move for NC-22 (n/a carries over); act per paragraph; a section-to-section change in meta-remark habit was a lean signal in the report, so the log records the section edited. |
| CT-13 | analog: inherit the base-type move for NC-22 on the prose remainder (n/a carries over); code comments addressed to the reader are excluded and not edited. |
| CT-14 | analog: inherit the base-type move for NC-22 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | analog: insert one remark acknowledging the text as a text or the reader as reading, only if the register allows; confidence Low. |

### NC-23 — Direct Reader Address

- **id:** NC-23
- **name:** Direct Reader Address
- **moves_toward:** from "never" toward "occasional asides" — human 0.28 against AI 0.07 as Table 16 prints them (p. 26), 28% against 7% as §4.1 prints them (p. 7); the paper does not reconcile the two encodings . One or two sentences that speak to the reader as a person.
- **scope:** local (basis: detection_method; C1 inference under ). Each address has an exact span; the insertion point is chosen by the skill and logged.
- **measured_importance:** 0.0100 (1.0% of total mass), rank 7 of the 30 cards — Tier 1 (heavy). `feature_cards.md` Part A.
- **fact_risk:** none — an address to the reader asserts nothing about the world; on CT-08 the address ("if you have driven this road, you know…") must not assert anything about the reader that the story does not support.
- **never_do:**
  - Never insert address as filler; one per ~800 words at most (a build proposal, not a paper figure), and only where the argument or narration invites it.
  - Never insert on rows where C2 marks the cell n/a (CT-04, CT-05, CT-06, CT-09, CT-11); on CT-09 second person is universal by genre and carries no signal.
  - Never use generic "you" ("you can see that…"); it does not count. The address is a question to the reader, an imperative aimed outward, or "you" with a specific situation.
  - Never introduce address the voice sample never uses (UC-03; the canonical example of the sample gate is this feature); log `skipped: absent from voice sample`. Rhetorical questions as transitions are humanize's voice-and-register work and are not added here.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: insert one direct address to the reader (a second-person sentence aimed outward, an imperative, a question to the audience) at a natural pause; this moves the text from "never" to "occasional asides"; do not make address structural unless the text already leans that way. |
| CT-02 | literal: same; one address at a natural pause. |
| CT-03 | analog: insert one reader-specific second-person sentence (a question to them, an imperative, "you" with a specific situation) where the argument invites it; at most one per ~800 words (build proposal); generic "you" does not count. |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | analog: documentation is second-person by convention, so insert one reader-specific address (anticipating the reader's particular situation, or asking them a question) at a real decision point; imperative steps ("Run the following") do not count. |
| CT-08 | analog: insert one second-person sentence directed at the reader ("if you have driven this road, you know…") where the story's subject touches the reader's likely experience; at most one per ~800 words (build proposal). |
| CT-09 | n/a |
| CT-10 | analog: second person is the default in a message, so insert one reader-specific address — a question to the recipient or a reference to their particular situation — drawn from the message's context; generic "you" does not count. |
| CT-11 | n/a |
| CT-12 | analog: inherit the base-type move for NC-23 (n/a carries over); act per paragraph; a section-to-section change in address habit was a lean signal in the report, so the log records the section edited. |
| CT-13 | analog: inherit the base-type move for NC-23 on the prose remainder (n/a carries over); imperatives inside code comments are excluded and not edited. |
| CT-14 | analog: inherit the base-type move for NC-23 (n/a carries over); formal/informal (T/V) and pro-drop second-person forms all count; language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | analog: insert one reader-directed second-person sentence (question, imperative, reference to the reader's situation) if the register allows; generic "you" does not count; confidence Low. |

### NC-24 — Depth of Recontextualization After Surprise

- **id:** NC-24
- **name:** Depth of Recontextualization After Surprise
- **moves_toward:** higher on the 1 (none) – 5 (complete re-reading) scale — human mean 3.28 against AI 2.95 (Table 16, p. 26). One revelation after which one or two earlier passages read differently.
- **scope:** global (basis: detection_method; C1 inference under ). One document-level rating; the revelation passage and the earlier passages it overturns are the driving spans.
- **measured_importance:** 0.0022 (0.2% of total mass), rank 19 of the 30 cards — Tier 2. `feature_cards.md` Part A.
- **fact_risk:** low — in fiction a fact the text already states early is withheld and disclosed late, consistent with everything in the ledger; in nonfiction rows the reframing must already be in the author's material (a later-learned truth, a concession, a limitation), placed so it lands after what it reframes. No twist fact is invented.
- **never_do:**
  - Never invent a revelation, a reversal, a contradicting document, or a limitation.
  - Never let the withheld fact contradict anything earlier in the text; the fact ledger is checked against the reordered text.
  - Never withhold on rows where disclosure order is genre-mandated (CT-04, CT-05, CT-07, CT-09, CT-10; C2 n/a).
  - Never withhold the core news of a story (CT-08); the reframing is a source's account contradicted or a document that changes the framing, already in the reporting.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: take a fact the text states early (a motive, an identity, a cause), withhold it until late, and make its disclosure change the meaning of one or two earlier passages; check the ledger for consistency after the move. |
| CT-02 | literal: same with a real fact the author's account already contains (a later-learned truth), disclosed later in the telling than it was first stated. |
| CT-03 | analog: add one later section that changes the reading of an earlier one — a concession that reframes the thesis, a reversal, a "here is what I got wrong" — drawn from qualifications the author already makes. |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | analog: for one limitation already listed, state which earlier result it changes the reading of and how, using the manuscript's own content; never add a limitation. |
| CT-07 | n/a |
| CT-08 | analog: sequence one contradiction already in the reporting (a source's account contradicted, a document that reverses the framing) so that it lands after the account it contradicts; never add a source or a document. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | analog: place one recounted failure or stated change of mind after the success it reframes, from the applicant's own account. |
| CT-12 | analog: inherit the base-type move for NC-24 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-24 on the prose remainder (n/a carries over). |
| CT-14 | analog: inherit the base-type move for NC-24 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-25 — Chronological Discontinuity

- **id:** NC-25
- **name:** Chronological Discontinuity
- **moves_toward:** higher on the 1–5 scale — human mean 2.40 against AI 2.12 (Table 16, p. 26). The paper's image: "a human mystery might open at the funeral and spiral backward through decades, while AI tells the same story from first clue to the grand reveal" (p. 7). A few jumps, not constant jumping.
- **scope:** global (basis: detection_method; orchestrator ruling ). One document-level rating; the individual jumps (with their cues) are the driving spans.
- **measured_importance:** 0.0070 (0.7% of total mass), rank 13 of the 30 cards — Tier 2. `feature_cards.md` Part A.
- **fact_risk:** low — the move reorders the telling of events the text already narrates; dates, ages, and sequence of the underlying events are ledger items and never change. Nothing is added.
- **never_do:**
  - Never invent an event to jump to; jump to one the text already narrates or summarizes.
  - Never change a date, an age, or the underlying order of real events (CT-02, CT-08, CT-11); only the order of telling moves.
  - Never reorder on rows where order is genre-mandated (CT-04, CT-05, CT-06, CT-07, CT-09, CT-10; C2 n/a).
  - Never jump without a cue the reader can follow ("years earlier", a dated heading, "by the time"); an uncued jump is a continuity error, not discontinuity.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: move one or two scenes out of chronological order — open later and flash back, or insert a flashback to an event the text already narrates — each with a cue ("years earlier", "by the time", a dated section break); keep every date and age consistent. |
| CT-02 | literal: same with the real events: reorder the telling with cues; the chronology of what happened is a ledger item and does not change. |
| CT-03 | analog: insert one or two order-breaking moves from material already present — move an anecdote from the introduction into the middle of the argument, state the conclusion early and return to an earlier point — so the piece no longer runs strictly context → problem → argument → conclusion. |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: reorder one or two paragraphs after the lede so they move backward or forward in time relative to their neighbors (background after present-time, or present-time after background), with a time cue; dates unchanged. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | analog: tell one episode of the applicant's path out of order (open on a recent moment, then go back to how it began), with a cue; dates unchanged. |
| CT-12 | analog: inherit the base-type move for NC-25 per lean-map section (n/a carries over); apply once per section, with paragraph boundaries inside the section as the driving spans. |
| CT-13 | analog: inherit the base-type move for NC-25 on the prose remainder (n/a carries over); paragraph adjacency is computed after block removal. |
| CT-14 | analog: inherit the base-type move for NC-25 (n/a carries over); mark jumps by stated time reference, not by tense shift alone, since tense systems differ; language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-26 — Nonlinear Framing for Delayed Disclosure

- **id:** NC-26
- **name:** Nonlinear Framing for Delayed Disclosure
- **moves_toward:** higher on the 1 (linear) – 5 (heavily fragmented) scale — human mean 1.96 against AI 1.68 (Table 16, p. 26). Both means are low: one time jump that does revelatory work, not a fragmented telling.
- **scope:** global (basis: detection_method; C1 inference under ). One document-level rating; the withholding jump and the later disclosing scene are the driving spans.
- **measured_importance:** 0.0047 (0.5% of total mass), rank 17 of the 30 cards — Tier 2. `feature_cards.md` Part A.
- **fact_risk:** low — a fact already in the text is disclosed later than it is first known, via a time jump (NC-25) or a frame; nothing is invented; the ledger is re-checked.
- **never_do:**
  - Never invent the withheld fact; withhold one the text already states.
  - Never withhold on rows where the finding or key fact must appear up front (CT-04, CT-05, CT-06, CT-07, CT-09, CT-10, CT-11; C2 n/a).
  - Never withhold the core news of a journalism piece (CT-08); hold a secondary key fact past the nut graf.
  - Never fragment the telling; the human mean of 1.96 is near the bottom of the scale.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: pair one time jump (NC-25) with a withheld fact (NC-24): the later-placed scene supplies a cause, fact, or identity the earlier scenes leave open; or add a frame (a prologue set after the events, a retrospective opening) built from material already in the text. |
| CT-02 | literal: same with real facts the author already knows, disclosed later in the telling than first known. |
| CT-03 | analog: delay the thesis past the first section: open on the case or the evidence and state the main point after it, rather than stating it up front. |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: hold one key fact (not the core news) past the nut graf and disclose it later for effect; the fact is already in the reporting. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | n/a |
| CT-12 | analog: inherit the base-type move for NC-26 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-26 on the prose remainder (n/a carries over). |
| CT-14 | analog: inherit the base-type move for NC-26 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-27 — Anachrony Intensity

- **id:** NC-27
- **name:** Anachrony Intensity
- **moves_toward:** higher on the 1 (absent) – 5 (dominant anachronic) scale — human mean 2.58 against AI 2.31 (Table 16, p. 26). Anachronic material carries a larger share of the telling; a present-time frame remains.
- **scope:** global (basis: detection_method; C1 inference under ). One document-level rating; the longest flashback or flash-forward and the frame it departs from are the driving spans.
- **measured_importance:** 0.0013 (0.1% of total mass), rank 25 of the 30 cards — Tier 3 (light). `feature_cards.md` Part A.
- **fact_risk:** low — **on fiction this move writes new diegetic material and that is permitted** (Hard rule 1 / Step 2: new story propositions are not ledger items; the real-world prohibition is unchanged, and no new proper noun, date, or figure enters). An event the text summarizes in a sentence is expanded into a scene from the text's own material; in nonfiction rows only background already in the account is expanded; nothing is added.
- **never_do:**
  - Never invent the content of a flashback; expand an event the text already summarizes.
  - Never remove the present-time frame; "dominant anachronic" is the top rung, not the target.
  - Never expand on rows where retrospection is fixed by genre or absent (C2 n/a: CT-03, CT-04, CT-05, CT-06, CT-07, CT-09, CT-10, CT-11).
  - Never add facts to background in journalism (CT-08); redistribute paragraphs, do not add reporting.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: expand one flashback or flash-forward the text summarizes in a sentence or two into a full scene, so anachronic material carries a larger share of the telling; keep the present-time frame it departs from. |
| CT-02 | literal: same with a remembered real event the author already recounts in summary. |
| CT-03 | n/a |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: expand background already in the reporting into paragraphs of its own so that background or flashback paragraphs rise toward C2's proposed background share for this row (that cell's `threshold:` verdict in `content_type_matrix.md` §0.8); never add facts. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | n/a |
| CT-12 | analog: inherit the base-type move for NC-27 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-27 on the prose remainder (n/a carries over). |
| CT-14 | analog: inherit the base-type move for NC-27 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-28 — Location Variety Scope

- **id:** NC-28
- **name:** Location Variety Scope
- **moves_toward:** higher rung on the single–multiworld ordinal — human mean 1.34 against AI 1.08 (Table 16, p. 26). Both means sit near the bottom: one additional inhabited locale, not many.
- **scope:** global (basis: detection_method; C1 inference under ). One document-level rating; the passages that establish each locale are the driving spans.
- **measured_importance:** 0.0017 (0.2% of total mass), rank 21 of the 30 cards — Tier 3 (light). `feature_cards.md` Part A.
- **fact_risk:** low — in fiction a scene is set in a second locale the text already mentions; in nonfiction rows no real event is relocated, and a second locale or domain must already be in the material; on CT-03 an example from a second domain that is not in the essay is a fact and gets a `[SOURCE NEEDED]` marker.
- **never_do:**
  - Never relocate a real event (CT-02, CT-08).
  - Never invent an example, a place, or a domain; use one the text mentions or the user supplies, or mark.
  - Never add locales on rows with none (C2 n/a: CT-04 through CT-07, CT-09 through CT-11, CT-15).
  - Never move toward "multiworld"; one more inhabited place is the target.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: set one scene in a second locale the text already mentions (a place a character refers to, a place the story came from), or add a short scene there built from material present, so the story inhabits more than one place. |
| CT-02 | literal: only where the real events occurred in more than one place: give a second real place its own scene from the author's account; never relocate an event. |
| CT-03 | analog: draw one example from a second concrete domain (a different workplace, country, period, or industry) already present in the essay or supplied by the user; if every example is abstract and nothing is supplied, insert `[SOURCE NEEDED: concrete example from a second domain]`. |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: give a second reported location its own scene paragraph from reporting already present; never invent. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | n/a |
| CT-12 | analog: inherit the base-type move for NC-28 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-28 on the prose remainder (n/a carries over). |
| CT-14 | analog: inherit the base-type move for NC-28 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-29 — Dialogue-to-Narration Proportion

- **id:** NC-29
- **name:** Dialogue-to-Narration Proportion
- **moves_toward:** higher on the 1 (no dialogue) – 5 (dialogue dominates) scale — human mean 2.95 against AI 2.70 (Table 16, p. 26). More of the text as direct speech relative to narration.
- **scope:** global (basis: detection_method; C1 inference under ). One document-level rating; the longest runs of dialogue and of narration are the driving spans.
- **measured_importance:** 0.0020 (0.2% of total mass), rank 20 of the 30 cards — Tier 2. `feature_cards.md` Part A.
- **fact_risk:** low in fiction — **composing lines a character did not speak on the page is permitted diegetic material** (Hard rule 1 / Step 2), provided the content is what the narration already asserts and no new proper noun, date, or figure enters; summarized speech already in the text is rendered as direct dialogue; high wherever the speaker is a real person: a quotation is a fact, and this skill never composes one. On CT-02 indirect speech the author already reports may be rendered direct only with the author's confirmation; on CT-03 and CT-08 the proportion moves only by extending quotations the user supplies or by trimming the author's prose around existing quotes, and otherwise a `[SOURCE NEEDED]` marker is placed.
- **never_do:**
  - Never compose a quotation, testimonial, or line of speech for a real person.
  - Never alter the words of an existing quotation.
  - Never pad dialogue with exchanges that carry nothing; convert summary that already carries the speech content.
  - Never source the new dialogue from the philosophical exchanges NC-05 has just cut or shortened; that restores an AI-side value under a human-side id.
  - Never apply on rows with no quoted speech in the register (C2 n/a: CT-04 through CT-07, CT-09 through CT-11, CT-15).
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: render summarized or reported speech ("she told him she was leaving") as direct dialogue exchanges carrying the same content, and cut narration that paraphrases what a character then says aloud. **Take the converted material from narration outside the exchanges NC-05 cut or shortened** — NC-05 runs first and lowers the same ratio this move raises, so drawing on what it removed would undo an earlier move. **Where the text reports action rather than speech** and no summarized speech exists to convert, compose only lines whose content the narration already asserts and log them as NC-17 does (`added from existing element P[n]`); if even that is unavailable, log `skipped: no summarized speech and no assertable content`. |
| CT-02 | literal: render the author's own indirect reports of a real person's speech as direct speech only where the author confirms the words; otherwise trim narration around the dialogue already present; never invent a real person's words. |
| CT-03 | analog: raise the share of quoted words by quoting sources the essay already names at greater length from text the user supplies, or by trimming the author's prose around existing quotations; if no quotable source text is supplied, insert `[SOURCE NEEDED: quotable passage from <named source>]`; C2's proposed quoted-share band for this row is the reference (that cell's `threshold:` verdict in `content_type_matrix.md` §0.8). |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: extend existing quotations from the reporter's notes when supplied, or trim reporter prose around them; never compose a quote; C2's proposed quoted-share band for this row is the reference (that cell's `threshold:` verdict in `content_type_matrix.md` §0.8). |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | n/a |
| CT-12 | analog: inherit the base-type move for NC-29 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-29 on the prose remainder (n/a carries over); code, comments, and table text are neither dialogue nor narration and are not touched. |
| CT-14 | analog: inherit the base-type move for NC-29 (n/a carries over); count and render speech acts rather than quotation marks, since dialogue punctuation conventions differ; language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-30 — Moral Polarity

- **id:** NC-30
- **name:** Moral Polarity
- **moves_toward:** option "ambivalent/mixed" — human 59% against AI 38% (Table 16, p. 26). The protagonist's (or the text's own) central choices carry a cost, a compromise, or an open judgment.
- **scope:** global (basis: detection_method; C1 inference under ). One document-level rating; the most consequential choice and the passage that judges it are the driving spans.
- **measured_importance:** 0.0096 (1.0% of total mass), rank 10 of the 30 cards — Tier 2. `feature_cards.md` Part A.
- **fact_risk:** low — in fiction the cost or compromise is one the text already implies; in nonfiction rows a concession, limitation, trade-off, or contested claim must already be in the author's material, and a flaw of a real person is never invented; on CT-09 a limitation not supplied gets a `[SOURCE NEEDED]` marker.
- **never_do:**
  - Never invent a flaw, a failure, a rival's merit, a counterargument fact, or a product limitation; concede what the material already contains, or mark.
  - Never flip the polarity (a vindicated protagonist made villainous); the human option is mixed, not negative.
  - Never apply on rows where evaluative stance is excluded by genre (C2 n/a: CT-05, CT-07, CT-10, CT-15).
  - Never treat this as hedging the prose; the move is a structural concession or an unresolved judgment, not softened wording (hedges belong to the surface pass).
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: give the protagonist's most consequential choice a cost or compromise the text already implies, and cut the sentence in which the ending vindicates or condemns the choice; leave the judgment open. |
| CT-02 | analog: let one of the narrator's (or the central real person's) choices stand with its cost unresolved; cut the self-vindicating sentence; never invent a flaw of a real person. |
| CT-03 | analog: add one sentence conceding merit or complexity to the opposed position, or admitting the author's own uncertainty, drawn from qualifications already in the essay. |
| CT-04 | analog: add one concession — a limit of the contribution, or a merit of the approaches being superseded — drawn from the manuscript's own limitations or related-work material. |
| CT-05 | n/a |
| CT-06 | analog: add one sentence conceding that an alternative interpretation or a rival method already discussed could be correct. |
| CT-07 | n/a |
| CT-08 | analog: give one opposing view, contested claim, or flaw already in the reporting its weight in the central subject's portrayal; never invent a flaw or a source. |
| CT-09 | analog: add one sentence naming a trade-off, a limitation, or who the product is not for, from product facts the user supplies; otherwise insert `[SOURCE NEEDED: genuine limitation or non-target user]`. |
| CT-10 | n/a |
| CT-11 | analog: add one sentence conceding a wrong turn, a weakness, or an unresolved doubt that the applicant's own account already contains. |
| CT-12 | analog: inherit the base-type move for NC-30 per lean-map section (n/a carries over); apply once per section. |
| CT-13 | analog: inherit the base-type move for NC-30 on the prose remainder (n/a carries over). |
| CT-14 | analog: inherit the base-type move for NC-30 (n/a carries over); language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

---

## Part 1B — Extended interventions (NC-31 to NC-40): measured, not in Table 16

**Status.** These ten features sit in the paper's 304-feature taxonomy and in the narrative (non-style) set the classifier uses, but they are **not** Table 16 core features and must never be described as such. Tag: `tier: extended (measured, not in Table 16)`. They are here because the validation study measured their classifier weight and their human/AI separation and found that all ten sit in Tier 1 while none had an entry: the stories that failed to flip left their value on the table. Baselines are recomputed from `storyscope_features.parquet`; questions, value lists and detection methods are verbatim from `taxonomy.json`; "moved by rewrites" comes from the ten-story experiment.

**These entries run on every content type.** Read each row in `content_type_matrix.md` before applying its move.

**Scope still defers to C1.** Each `scope` below is A2's reading of the authors' `detection_method`, superseded by C1's card wherever the card's own `scope` differs. See the precedence rule in the header: the matrix governs row membership and cell type, the card governs scope, and a disagreement is reported rather than silently resolved.

**A caution that applies to all ten.** Classifier weight is not the same thing as a defect worth repairing. **NC-38 (genre) gets no move on any row** — C2's own cells say "rated only — genre is not a rewrite target" — because changing a text's genre means writing a different story. **NC-37 and NC-39 are rate-only on every non-fiction row**, because converting a real person's name, or rendering interiority the source never reported, would break the fact ledger. NC-32 is a whole-text conversion that is usually right to skip outside fiction. Weight tells you where the score is; the entry tells you whether a move is legitimate.

### NC-31 — Modes of Conveying the Central Character's Emotions `AGENT_EMO_002`

- **id:** NC-31 — `tier: extended (measured, not in Table 16)`
- **name:** Modes of Conveying the Central Character's Emotions
- **taxonomy_id:** `AGENT_EMO_002` (agents, multi_select)
- **measured_importance:** 0.0410 — **rank 2 of all features the classifier weights**. Tier 1 (heavy).
- **moved by the rewrites:** 7 of 10 rewrites moved it (validation results Step 5).
- **question:** "Through which modes are the central character's emotions mainly conveyed?" (taxonomy.json)
- **values:** explicit emotion words/labels; bodily sensations or physiology (e.g., heartbeat, nausea, trembling); metaphorical or environmental imagery reflecting mood; actions and choices as emotional indicators; dialogue content or tone revealing emotion (taxonomy.json)
- **detection_method (authors'):** "Identify how the text signals the character's feelings. Mark each mode that regularly appears as a vehicle for emotional information, not just one-off instances." (taxonomy.json)
- **baseline:** 'metaphorical or environmental imagery reflecting mood' human 50% / AI 89% (gap −39, the largest of the three separating options); 'dialogue content or tone revealing emotion' human 69% / AI 49% (+20); 'bodily sensations or physiology' human 59% / AI 74% (−15). Recomputed from `storyscope_features.parquet`.
- **moves_toward:** drop 'metaphorical or environmental imagery reflecting mood' out of the set of modes that *regularly* carry emotion, and get 'dialogue content or tone revealing emotion' into it. Multi-select: the target is the composition of the set, not a single value.
- **scope:** global (basis: detection_method — "each mode that regularly appears" is a judgment over the whole text; the imagery instances are the driving spans). A2's reading; `feature_cards.md` carries the card and governs scope where the two differ.
- **fact_risk:** low — the move re-vehicles emotion the text already conveys, and (for the dialogue mode) renders as speech what the narration already asserts. On fiction the new lines are diegetic material under the Step 5 licence; in non-fiction rows the move is governed by that row's own cell in `content_type_matrix.md`, which C2 has defined for all fifteen rows.
- **never_do:**
  - Never confuse this with NC-07. NC-07 asks which single mode is *dominant* (`AGENT_EMO_009`); NC-31 asks which modes *regularly appear* and is multi-select. A story can satisfy NC-07 by naming feelings and still fail NC-31 by keeping mood-imagery as a regular vehicle throughout.
  - Never leave the environmental-imagery mode in place because individual images are good writing; the AI base rate on this option is 89% against 50% human, the widest option gap in the extended set.
  - Never manufacture dialogue for a character who does not speak in the text; take the content from what the narration already asserts, as NC-29 does.
  - Never treat imagery removal as prose-texture work — cutting a mood-image is only this move when the emotional load moves to another vehicle, not when the sentence is merely rewritten.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: list the modes that regularly carry the central character's emotions. Where mood-matched weather, light, rooms, or landscape is one of them, cut or re-vehicle enough of those images that it is no longer a regular carrier (this overlaps NC-08's driving spans; act once and log both ids). Then move emotional load into dialogue content or tone, from speech already present or composed from what the narration asserts. Bodily sensation is also AI-elevated (74% against 59%) and must not become the replacement vehicle — see NC-07. |
| CT-02 | literal: same, on the real people the piece renders. Emotional load moves into speech the author already records; never compose a real person's words. |
| CT-03 | analog: act on the modes C2 counts — named emotion words, bodily sensation, metaphorical or environmental imagery, actions and choices, quoted speech tone. Cut or re-vehicle the metaphorical and environmental imagery until it is no longer a regular mode, and let named emotion words or the tone of quoted speech carry that load instead. |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: same, on the imagery in the reporter's voice. Emotional load moves to a named feeling already supported by the reporting or to the tone of a quote already in the story; never assert a subject's feeling the reporting does not contain, and never compose a quote. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | analog: same, on the applicant's own feelings. Cut the environmental and metaphorical imagery; name the feeling, or let a recorded exchange carry it. |
| CT-12 | analog: inherit the base-type move for NC-31 per lean-map section (n/a carries over); act once per section, never across the document. |
| CT-13 | analog: inherit the base-type move for NC-31 on the prose remainder (n/a carries over); code blocks, tables, equations and captions are never edited. |
| CT-14 | analog: inherit the base-type move for NC-31 (n/a carries over); language: unvalidated; confidence Low; the humanize pass is skipped by default on this row . |
| CT-15 | n/a |

### NC-32 — Dominant Narrative Tense `TMP_DUR_011`

- **id:** NC-32 — `tier: extended (measured, not in Table 16)`
- **name:** Dominant Narrative Tense
- **taxonomy_id:** `TMP_DUR_011` (temporal_structure, categorical)
- **measured_importance:** 0.0172 — rank 5 of all weighted features. Tier 1 (heavy).
- **moved by the rewrites:** **0 of 10.** No rewrite in the experiment moved this feature at all; it is the clearest single instance of the coverage gap (validation results Step 5).
- **question:** "In what tense is the bulk of the narrative discourse written?" (taxonomy.json)
- **values:** past; present; future; mixed_or_shifted (taxonomy.json)
- **detection_method (authors'):** "Identify the grammatical tense used in most narrative sentences (excluding dialogue). If tense shifts markedly between large sections, classify as 'mixed_or_shifted'." (taxonomy.json)
- **baseline:** 'past' human 86% / AI 96% (gap −10); 'present' human 13% / AI 4% (+8); 'mixed_or_shifted' human 1% / AI 0% (+1). Recomputed from `storyscope_features.parquet`.
- **moves_toward:** 'present' — the human-elevated value at 13% against 4%. Note the size of what is on offer: the AI-side value 'past' is 96% against 86% human, so most human stories are also past tense. This is a low-prevalence, heavily-weighted feature, not a majority pattern.
- **scope:** global (basis: detection_method — "the tense used in most narrative sentences" is a whole-text property; there are no driving spans, only the whole discourse). A2's reading; `feature_cards.md` carries the card and governs scope where the two differ.
- **fact_risk:** none — tense conversion changes no proposition. It is the only extended move with no fact risk at all.
- **never_do:**
  - Never convert partially. A half-converted text lands on 'mixed_or_shifted', which is 1% human and 0% AI — a value almost nothing in the corpus carries, and a defect rather than a move. Convert every narrative sentence or none, and log it document-level.
  - Never convert the dialogue. The detection method excludes dialogue explicitly; quoted speech keeps its own tense.
  - Never convert a text whose structure depends on retrospective distance — a frame narrator looking back, an epilogue vantage (NC-40), a distant-retrospective telling. Log `skipped: retrospective frame; conversion would break the telling` instead.
  - Never treat this as the surface pass's business. Tense is a discourse-level choice in the temporal_structure dimension and the surface pass has no instruction about it; but it is also the move most likely to look like a mechanical edit, so the log must show it as one document-level decision.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: if the narrative discourse is past tense and no retrospective frame depends on it, convert every narrative sentence to present tense, leaving dialogue untouched. One decision, one log line, `where: document-level`. If a frame or an epilogue vantage depends on retrospection, log `skipped: retrospective frame; conversion would break the telling`. This move was attempted by none of the three validation rewriters and moved 0/10; it is written because the classifier weighs it fifth. |
| CT-02 | literal: same test, applied to the telling of real events. Memoir's retrospective vantage is usually load-bearing, so `skipped: retrospective frame` is the common and correct outcome here. |
| CT-03 | n/a |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: **rate, and usually skip.** C2's cell records that hard news is past by convention, so the rating carries little signal outside features. Convert only in a feature whose scene work would genuinely sit in the present, and never in hard news; otherwise log `skipped: past tense is genre convention on this row`. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | n/a |
| CT-12 | analog: inherit the base-type move for NC-32 per lean-map section (n/a carries over); act once per section, never across the document. |
| CT-13 | analog: inherit the base-type move for NC-32 on the prose remainder (n/a carries over); tense inside code comments, captions and table cells is excluded from the count and never converted. |
| CT-14 | n/a |
| CT-15 | n/a |

### NC-33 — Post-Climax Denouement Length `PLT_MOR_007`

- **id:** NC-33 — `tier: extended (measured, not in Table 16)`
- **name:** Post-Climax Denouement Length
- **taxonomy_id:** `PLT_MOR_007` (plot, ordinal)
- **measured_importance:** 0.0170 — rank 6 of all weighted features. Tier 1 (heavy).
- **moved by the rewrites:** 3 of 10.
- **question:** "How extended is the narrative after the main climactic event?" (taxonomy.json)
- **values:** none_or_minimal (story ends immediately after climax); brief (a short scene or paragraph of aftermath); extended (multiple scenes/time jumps of aftermath/epilogue) (taxonomy.json)
- **detection_method (authors'):** "Locate the main climax, then count how much textual space follows. Classify as immediate cutoff, quick wrap-up, or substantial epilogue/after-story." (taxonomy.json)
- **baseline:** human 1.02 / AI 1.47 (0-based code mean over the three values). Recomputed from `storyscope_features.parquet`.
- **moves_toward:** the lower end — 'none_or_minimal' or 'brief'. The AI mean sits half a rung above the human mean, so the move is to cut aftermath, not to add it.
- **scope:** local (basis: detection_method — the climax and everything after it is a bounded terminal passage that can be quoted). A2's reading; `feature_cards.md` carries the card and governs scope where the two differ.
- **fact_risk:** none — the move deletes aftermath; nothing is added.
- **never_do:**
  - Never cut aftermath that carries a ledger item stated nowhere else; relocate the item before the climax or keep the shortest passage that holds it.
  - Never confuse this with NC-18 (how the chain resolves) or NC-40 (where in story time the text stops). This move is about *how much space* follows the climax; a text can end on an external act (NC-18 satisfied) and still trail three scenes of epilogue.
  - Never create an abrupt stop that leaves the climax itself unnarrated; 'none_or_minimal' means the story ends immediately after the climax, not before it.
  - Never apply together with an added epilogue from any other move; NC-40's human-side value ('ends at or just after the main climax') pulls the same way, and FP-Claude-3's epilogue move pulls against both — the core moves win.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: find the main climax and measure what follows. Where multiple scenes, time jumps, or an epilogue follow it, cut them to a short scene or paragraph of aftermath, or to nothing; relocate any ledger item they carried to before the climax. Log the climax paragraph and the cut range. Execute with NC-40 where both fire on the same terminal passage. |
| CT-02 | literal: same, on the real aftermath as told; the events are unchanged, the space given to them shrinks. |
| CT-03 | analog: cut the paragraphs that follow the essay's strongest or final argumentative move and add no new reasoning, leaving at most one. |
| CT-04 | analog: cut the paragraphs that follow the contribution statement and add no new claim — roadmap paragraphs, restatements of results — leaving at most one. |
| CT-05 | n/a |
| CT-06 | analog: cut the paragraphs that follow the main conclusion statement and add no new content — future work, broader impact, a second restatement — leaving at most one. Where a venue mandates one of those sections, keep it and log `skipped: section required by venue`. |
| CT-07 | n/a |
| CT-08 | analog: locate the central event or turn and cut the trailing paragraphs of consequence, reflection, or where-they-are-now to a short aftermath; never cut a paragraph carrying a reported fact stated nowhere else. |
| CT-09 | analog: cut the blocks that follow the primary call to action and add no new offer — reassurance, a second value restatement, a vision paragraph — leaving at most one. |
| CT-10 | analog: cut the sentences that follow the message's main ask and add no new information — restated thanks, reassurance, a second sign-off — leaving at most one. |
| CT-11 | analog: cut the paragraphs that follow the statement of the aim and add no new content, leaving at most one. |
| CT-12 | analog: inherit the base-type move for NC-33 (n/a carries over); act per paragraph on the paragraphs following each section's climactic point. |
| CT-13 | analog: inherit the base-type move for NC-33 on the prose remainder (n/a carries over); trailing code blocks, tables and appendices are excluded from the count and are never cut. |
| CT-14 | analog: inherit the base-type move for NC-33 (n/a carries over); language: unvalidated; confidence Low; the humanize pass is skipped by default on this row . |
| CT-15 | analog: locate the text's climactic point or main claim and cut the trailing text to a brief wrap-up; unvalidated; confidence Low. |

### NC-34 — Density of Figurative Language in Character Depiction `AGENT_ATTR_024`

- **id:** NC-34 — `tier: extended (measured, not in Table 16)`
- **name:** Density of Figurative Language in Character Depiction
- **taxonomy_id:** `AGENT_ATTR_024` (agents, scale)
- **measured_importance:** 0.0160 — rank 8 of all weighted features. Tier 1 (heavy).
- **moved by the rewrites:** 3 of 10.
- **question:** "On a 1–5 scale, how dense is the use of figurative language (metaphors, similes, symbolic images) specifically in describing characters and their inner states?" (taxonomy.json)
- **values:** 1; 2; 3; 4; 5 (taxonomy.json)
- **detection_method (authors'):** "Sample several character-descriptive passages across stories. Count how often metaphors/similes are used per paragraph to render traits or states […] If almost none occur, assign 1. If occasional but not constant, assign 2. If roughly balanced between literal and figurative, assign 3. If figurative language is frequent, assign 4. If nearly every description leans on rich, inventive imagery, assign 5." (taxonomy.json)
- **baseline:** human 3.06 / AI 3.78 (1–5 mean). Recomputed from `storyscope_features.parquet`.
- **moves_toward:** lower — toward the human mean of 3.06, roughly 'balanced between literal and figurative'. Not toward 1: the human mean is at the middle rung.
- **scope:** global (basis: detection_method — a density sampled "across" character-descriptive passages; the densest passages are the driving spans). A2's reading; `feature_cards.md` carries the card and governs scope where the two differ.
- **fact_risk:** none — the move cuts or literalizes figurative clauses in character description; nothing is added.
- **never_do:**
  - Never strip figuration from every character passage; the target is the middle rung, and a text at 1 is as far from the human mean as one at 5.
  - Never let this become the surface pass's vocabulary work. The unit is the figurative rendering of a trait or state ("built like a question"), replaced by a literal depiction or cut; rewriting the same simile in plainer words is the surface pass's business and does not move the rating.
  - Never confuse the target with NC-11 (sensory density across the whole narrative) or NC-31 (mood imagery as an emotional vehicle). This card counts figuration *in character depiction* only; the three overlap in their spans and must be logged as separate ids acting once.
  - Never cut a figurative rendering that is the only carrier of a character fact in the ledger; make the depiction literal instead.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: sample the character-descriptive passages, count metaphors and similes per paragraph, and cut or literalize enough of them that description is roughly balanced between literal and figurative rather than frequently figurative (human mean 3.06 against AI 3.78). Cite the two or three densest passages as driving spans. |
| CT-02 | literal: same, on the depiction of real people. |
| CT-03 | n/a |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: cut or literalize the metaphors and similes rendering people and their inner states in the reporter's voice until fewer than one per character-descriptive paragraph remains. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | analog: same, on the figurative rendering of the applicant, mentors and collaborators. |
| CT-12 | analog: inherit the base-type move for NC-34 per lean-map section (n/a carries over); act once per section, never across the document. |
| CT-13 | analog: inherit the base-type move for NC-34 on the prose remainder (n/a carries over); figurative identifiers inside code are excluded and never edited. |
| CT-14 | analog: inherit the base-type move for NC-34 (n/a carries over); language: unvalidated; confidence Low; the humanize pass is skipped by default on this row . |
| CT-15 | n/a |

### NC-35 — Overall Revelation Pacing Pattern `REV_DIS_001`

- **id:** NC-35 — `tier: extended (measured, not in Table 16)`
- **name:** Overall Revelation Pacing Pattern
- **taxonomy_id:** `REV_DIS_001` (revelation, categorical)
- **measured_importance:** 0.0124 — rank 11 of all weighted features. Tier 1 (heavy).
- **moved by the rewrites:** 5 of the 10 rewrites moved this feature (a per-feature movement count, not the boundary-crossing result).
- **question:** "How are major pieces of explanatory information (about stakes, backstory, ontology) distributed across the story?" (taxonomy.json)
- **values:** front_loaded_exposition_early; evenly_layered_throughout; back_loaded_key_revelations_near_end; episodic_or_serial_pulses_of_revelation (taxonomy.json)
- **detection_method (authors'):** "Mark where the reader learns the most about the underlying situation. If the bulk arrives in the opening scenes, choose front_loaded; if there is a steady drip, choose evenly_layered; if the biggest clarifications cluster in the last quarter, choose back_loaded; if the story is organized into distinct cases/episodes each with its own reveal, choose episodic_or_serial." (taxonomy.json)
- **baseline:** 'back_loaded_key_revelations_near_end' human 66% / AI 48% (+18); 'evenly_layered_throughout' human 32% / AI 49% (−17); 'front_loaded_exposition_early' human 1% / AI 3% (−2). Recomputed from `storyscope_features.parquet`.
- **moves_toward:** 'back_loaded_key_revelations_near_end' — 66% human against 48% AI — and away from 'evenly_layered_throughout'. This is the same human fingerprint the paper records as FP-Human-4 ('Overall revelation pacing → back-loaded', Table 17, p. 27).
- **scope:** global (basis: detection_method — a distribution across the whole story; the relocated disclosures are the driving spans). A2's reading; `feature_cards.md` carries the card and governs scope where the two differ.
- **fact_risk:** low — the move relocates explanatory material the text already contains into the last quarter; nothing is invented, and the fact ledger is re-checked for contradiction after the move, because a fact disclosed later can strand an earlier passage that assumed it.
- **never_do:**
  - Never withhold by deleting; the material must arrive, later. A fact that never lands is a ledger loss.
  - Never leave an earlier passage that only makes sense once the withheld fact is known. This is the contradiction case the Step 2 check exists for — a rewriter caught exactly this by hand in the experiment.
  - Never confuse this with NC-26 (whether time jumps *stage* revelations) or NC-24 (how much earlier text a revelation forces you to reinterpret). NC-35 is only about *where the explanatory mass sits*; it can be moved without any anachrony at all.
  - Never front-load as a repair; 'front_loaded_exposition_early' is 1% human and 3% AI and is the worst landing available.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: mark where the reader learns most about the underlying situation. If the explanation is spread evenly or front-loaded, gather the biggest clarifications — stakes, backstory, the rules of the world — and relocate them into the last quarter, leaving earlier scenes to run on less information. Re-read every earlier passage against the moved facts for contradiction (Step 2). Cite source and destination paragraphs as driving spans. |
| CT-02 | literal: same, with real facts the account already contains, disclosed later in the telling than they were first known. |
| CT-03 | analog: hold back some of the essay's key context and stakes for the final quarter rather than front-loading it or spreading it evenly; the material is the essay's own. |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: **rate, and move with care.** C2's cell records that the inverted pyramid front-loads by convention, so hold back only a secondary clarification, never the core news; if the piece is hard news, log `skipped: inverted pyramid is genre convention on this row`. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | analog: hold back some key context of the applicant's situation and stakes for the closing section rather than front-loading it. |
| CT-12 | analog: inherit the base-type move for NC-35 per lean-map section (n/a carries over); act once per section, never across the document. |
| CT-13 | analog: inherit the base-type move for NC-35 on the prose remainder (n/a carries over); code blocks, tables, equations and captions are never edited. |
| CT-14 | analog: inherit the base-type move for NC-35 (n/a carries over); language: unvalidated; confidence Low; the humanize pass is skipped by default on this row . |
| CT-15 | n/a |

### NC-36 — Explicit Enumeration vs Enacted Interaction `SOC_REL_004`

- **id:** NC-36 — `tier: extended (measured, not in Table 16)`
- **name:** Explicit Enumeration vs Enacted Interaction
- **taxonomy_id:** `SOC_REL_004` (social_networks, categorical)
- **measured_importance:** 0.0124 — rank 12 of all weighted features. Tier 1 (heavy).
- **moved by the rewrites:** 2 of 10.
- **question:** "Are relationships mainly described through list-like summaries and labels, or through dramatized scene interactions?" (taxonomy.json)
- **values:** predominantly_enumerated_or_summarized; mixed_enumerated_and_scenic; predominantly_enacted_in_scenes (taxonomy.json)
- **detection_method (authors'):** "Note whether the narrator frequently pauses to summarize the network ('X is Y's aunt; Z works for W…') versus mostly showing relationships via dialogue and action in scenes. Classify based on which mode dominates the text." (taxonomy.json)
- **baseline:** 'predominantly_enacted_in_scenes' human 75% / AI 83% (−8); 'mixed_enumerated_and_scenic' human 23% / AI 17% (+7); 'predominantly_enumerated_or_summarized' human 2% / AI 1% (+1). Recomputed from `storyscope_features.parquet`.
- **moves_toward:** 'mixed_enumerated_and_scenic' — 23% human against 17% AI. Note the direction: the human side here is *less* purely scenic, which runs against the usual craft advice, and the gaps are small (8 points at most). Move it only after the heavier cards are done.
- **scope:** global (basis: detection_method — "which mode dominates the text"). A2's reading; `feature_cards.md` carries the card and governs scope where the two differ.
- **fact_risk:** low — a summarizing sentence about a relationship the text already establishes; no new relationship is invented.
- **never_do:**
  - Never invent a relationship, a kinship, or an employment to summarize; the network must already be in the text.
  - Never convert the story to summary wholesale; 'predominantly_enumerated_or_summarized' is 2% human and 1% AI, a landing almost nothing occupies.
  - Never let this undo NC-29 (dialogue proportion) or NC-36's own scene material — the move adds one or two summarizing passages beside the scenes, it does not replace them.
  - Never apply on a text whose relationships are already summarized; check the current value first.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: if relationships are shown almost entirely through scene interaction, add one or two short narratorial passages that state the network directly — who is related to whom, who works for whom — drawn from what the text already establishes, so the text reads as mixed rather than purely enacted. Additive: this move restores length. |
| CT-02 | literal: same, from relationships the piece already establishes among real people; invent no kinship or employment. |
| CT-03 | n/a |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: add one or two summarizing attributions of the kind C2 counts ("X, who runs the shelter where Y volunteers") from relationships already reported; never invent a relationship. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | analog: add one or two passages that list and label the applicant's mentors, collaborators, or communities, from relationships the statement already describes in episodes. |
| CT-12 | analog: inherit the base-type move for NC-36 per lean-map section (n/a carries over); act once per section, never across the document. |
| CT-13 | analog: inherit the base-type move for NC-36 on the prose remainder (n/a carries over); code blocks, tables, equations and captions are never edited. |
| CT-14 | analog: inherit the base-type move for NC-36 (n/a carries over); language: unvalidated; confidence Low; the humanize pass is skipped by default on this row . |
| CT-15 | n/a |

### NC-37 — Naming Practice for the Central Character `AGENT_ID_005`

- **id:** NC-37 — `tier: extended (measured, not in Table 16)`
- **name:** Naming Practice for the Central Character
- **taxonomy_id:** `AGENT_ID_005` (agents, categorical)
- **measured_importance:** 0.0123 — rank 13 of all weighted features. Tier 1 (heavy).
- **moved by the rewrites:** **0 of 10.**
- **question:** "How is the central character primarily identified in the narration?" (taxonomy.json)
- **values:** named personal name (e.g., 'Maria', 'Leon'); unnamed but specific 'I/he/she' narrator; role/descriptor used as main identifier (e.g., 'the mother', 'the Anchor'); multiple or shifting identifiers (aliases, titles, different names in different contexts) (taxonomy.json)
- **detection_method (authors'):** "For the story's most central figure, note how they are usually referred to. Choose the single label that best describes the dominant practice over the whole text." (taxonomy.json)
- **baseline:** 'named personal name' human 70% / AI 80% (−10); 'unnamed but specific I/he/she narrator' human 23% / AI 17% (+6); 'role/descriptor as main identifier' human 4% / AI 2% (+2). Recomputed from `storyscope_features.parquet`.
- **moves_toward:** 'unnamed but specific I/he/she narrator' or 'role/descriptor used as main identifier' — both human-elevated, though by 6 and 2 points against a majority practice (70% of human stories still use a personal name).
- **scope:** global (basis: detection_method — "the dominant practice over the whole text"). A2's reading; `feature_cards.md` carries the card and governs scope where the two differ.
- **fact_risk:** high on any row where the person is real — a real person's name is a ledger item and is never removed or replaced. In fiction the character's name is a diegetic fact: dropping it to a pronoun or a role is permitted and logged, but it must be dropped consistently.
- **never_do:**
  - Never remove or alter a real person's name in any non-fiction row; on CT-02, CT-08, CT-10 and CT-11 this move does not run.
  - Never half-convert; a name that appears in three places and a role in ten lands on 'multiple or shifting identifiers', which the recomputation does not show as human-elevated.
  - Never strip a name that the plot needs (a character addressed by name in dialogue, a name that is the story's subject); dialogue keeps its vocatives.
  - Never do this for its own sake on a story where the name carries meaning; it is a 10-point option gap and Tier 1 only by classifier weight.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: if the central character is carried by a personal name throughout, decide whether the story survives being carried by a pronoun or a role instead ("the mother", "the Anchor"); if it does, convert every narratorial reference consistently, leaving dialogue vocatives intact. One document-level decision. If the name is load-bearing, log `skipped: name is load-bearing`. |
| CT-02 | analog: **rate only; no rewrite.** C2's cell rates a third-person central real person referred to throughout by personal name. A real person's name is a ledger item, so this skill does not convert it to a role or a pronoun: log `rated, no move: real person's name is a ledger item`. (First-person memoir is excluded by the cell itself.) |
| CT-03 | n/a |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: **rate only; no rewrite**, for the same reason — the subject's name is a reported fact. Log `rated, no move: real person's name is a ledger item`. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | n/a |
| CT-12 | analog: inherit the base-type move for NC-37 per lean-map section (n/a carries over); act once per section, never across the document. |
| CT-13 | analog: inherit the base-type move for NC-37 on the prose remainder (n/a carries over); code blocks, tables, equations and captions are never edited. |
| CT-14 | analog: inherit the base-type move for NC-37 (n/a carries over); naming practices, honorifics and patronymics differ by language, so read the dominant form of reference in the source convention before deciding anything; language: unvalidated; confidence Low; humanize skipped by default . |
| CT-15 | n/a |

### NC-38 — Primary Genre Category `SIT_GEN_001`

- **id:** NC-38 — `tier: extended (measured, not in Table 16)`
- **name:** Primary Genre Category
- **taxonomy_id:** `SIT_GEN_001` (situatedness, categorical)
- **measured_importance:** 0.0121 — rank 14 of all weighted features. Tier 1 (heavy).
- **moved by the rewrites:** 2 of 10.
- **question:** "What is the story's primary genre classification based on its dominant setting, plot devices, and reader expectations?" (taxonomy.json)
- **values:** realist_contemporary; historical_realist; science_fiction; fantasy; horror; mystery_detective; thriller_suspense; romance; comedy_satire; myth_fable_fairy_tale; literary_realist_fiction; multi_genre_blend; nonfictional_mode_pastiche_or_essayistic (taxonomy.json)
- **detection_method (authors'):** "Identify the dominant setting (realistic vs speculative), core plot engine (investigation, romance, quest, etc.), and conventional markers (magic, technology, monsters, clues) to assign the genre that best matches the story's main reader contract." (taxonomy.json)
- **baseline:** 'historical_realist' human 8% / AI 15% (−6); 'realist_contemporary' human 27% / AI 25% (+3); 'comedy_satire' human 4% / AI 1% (+2). Recomputed from `storyscope_features.parquet`.
- **moves_toward:** **no move.** The classifier weighs genre heavily, but genre is a property of the commission, not a defect to repair: changing it means writing a different story. This entry exists so that the feature is accounted for and explicitly declined, not so that it is acted on.
- **scope:** global (basis: detection_method — a whole-text reader contract). A2's reading; `feature_cards.md` carries the card and governs scope where the two differ.
- **fact_risk:** not applicable — no move is offered.
- **never_do:**
  - Never change a text's genre to move a classifier score. It is outside what a structural pass may do to a user's text and the user did not ask for a different story.
  - Never report NC-38 as `unmoved` without this note; the DIFF line should read `declined: genre is the commission, not a defect`.
  - Never read the 6-point 'historical_realist' gap as a target; the option set has thirteen values and no human-side landing the paper or the recomputation identifies as a repair.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: **no move.** Record the genre in the log for completeness and decline the intervention: changing a text's genre means writing a different story, and the user did not ask for one. The DIFF line reads `declined: genre is the commission, not a defect`. |
| CT-02 | analog: **no move**, and C2's cell says the same in its own words ("Rated only — genre is not a rewrite target"). Rate and decline. |
| CT-03 | n/a |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | n/a |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | n/a |
| CT-12 | analog: inherit the base-type cell for NC-38 per lean-map section (n/a carries over) — which is to rate and decline, per section. |
| CT-13 | analog: inherit the base-type cell for NC-38 on the prose remainder (n/a carries over) — rate and decline. |
| CT-14 | analog: inherit the base-type cell for NC-38 (n/a carries over) — rate and decline; C2 adds that genre contracts differ across literary traditions, so the card's vocabulary may not have a matching value at all; language: unvalidated; confidence Low. |
| CT-15 | n/a |

### NC-39 — Breadth of Focalization Across Characters `PER_FOC_002`

- **id:** NC-39 — `tier: extended (measured, not in Table 16)`
- **name:** Breadth of Focalization Across Characters
- **taxonomy_id:** `PER_FOC_002` (perspective, categorical)
- **measured_importance:** 0.0116 — rank 15 of all weighted features. Tier 1 (heavy).
- **moved by the rewrites:** 2 of 10.
- **question:** "Across the story, how many distinct characters' inner experiences does the narration give direct access to?" (taxonomy.json)
- **values:** single_focal_character; dual_focal_characters; multiple_small_set_3_to_5; many_or_godseye (taxonomy.json)
- **detection_method (authors'):** "List all characters whose thoughts or felt experience are directly rendered (beyond the narrator reporting guesses). Count distinct centers: 1 = single_focal_character; 2 = dual_focal_characters; 3–5 distinct minds = multiple_small_set_3_to_5; more than 5 or a sense of generalised human minds = many_or_godseye." (taxonomy.json)
- **baseline:** 'single_focal_character' human 79% / AI 91% (−12); 'multiple_small_set_3_to_5' human 11% / AI 4% (+7); 'dual_focal_characters' human 7% / AI 5% (+3). Recomputed from `storyscope_features.parquet`.
- **moves_toward:** 'dual_focal_characters' or 'multiple_small_set_3_to_5' — a second (or third) mind the narration enters directly. Note the tension with the paper's own human fingerprint FP-Human-2 ('Breadth of focalization → single focal', Table 17, p. 27), which points the other way; the recomputed core baseline and the classifier weight are the measured evidence and win here, and the conflict is recorded rather than resolved.
- **scope:** global (basis: detection_method — a count of distinct focal centers across the story). A2's reading; `feature_cards.md` carries the card and governs scope where the two differ.
- **fact_risk:** low — entering a second character's interiority renders feeling the scene already implies; on fiction that is diegetic material, permitted and logged.
- **never_do:**
  - Never open a second mind in a first-person text; a first-person narrator cannot render another character's interiority without breaking the perspective, and 'unnamed but specific I/he/she narrator' texts should log `skipped: first-person narration`.
  - Never scatter access across many characters; 'many_or_godseye' is not the human-elevated landing.
  - Never let this undo NC-12 (depth of interior access), which moves the *amount* of interiority down while this moves the *number of minds* up. Add a second center by relocating access, not by adding a new layer of sustained interiority.
  - Never assert a real person's inner state on any non-fiction row.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: if the narration enters only the protagonist's mind, give one existing secondary character a passage of directly rendered thought or felt experience — ideally by converting narration that already reports their behavior — so the story has two focal centers. Keep the total amount of interiority flat, per NC-12. Skip in first-person narration (`skipped: first-person narration`). |
| CT-02 | analog: **rate only; no rewrite.** C2 counts the distinct real people whose inner experience the piece renders directly. Rendering a real person's interiority that the author did not report is an invented fact about a real person: log `rated, no move: would assert a real person's inner state`. |
| CT-03 | n/a |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | n/a |
| CT-07 | n/a |
| CT-08 | analog: **rate only; no rewrite**, for the same reason. NC-12's move runs the other way on this row and is the one to act on. |
| CT-09 | n/a |
| CT-10 | n/a |
| CT-11 | n/a |
| CT-12 | analog: inherit the base-type move for NC-39 per lean-map section (n/a carries over); act once per section, never across the document. |
| CT-13 | analog: inherit the base-type move for NC-39 on the prose remainder (n/a carries over); code blocks, tables, equations and captions are never edited. |
| CT-14 | analog: inherit the base-type move for NC-39 (n/a carries over); language: unvalidated; confidence Low; the humanize pass is skipped by default on this row . |
| CT-15 | n/a |

### NC-40 — Ending Temporal Scope `TMP_ORD_014`

- **id:** NC-40 — `tier: extended (measured, not in Table 16)`
- **name:** Ending Temporal Scope
- **taxonomy_id:** `TMP_ORD_014` (temporal_structure, categorical)
- **measured_importance:** 0.0103 — rank 16 of all weighted features. Tier 1 (heavy).
- **moved by the rewrites:** 3 of 10.
- **question:** "Relative to the main line of events, where in story time does the narrative end?" (taxonomy.json)
- **values:** ends_at_or_just_after_main_climax_no_forward_jump; ends_with_brief_forward_epilogue_or_flashforward; ends_before_external_resolution_mid_arc_or_cliffhanger; ends_at_distant_future_or_far_past_vantage_summarizing_long_term (taxonomy.json)
- **detection_method (authors'):** "Identify the last narrated scene and determine whether it coincides with the primary external resolution, follows it by a short look ahead, stops earlier while major external consequences are offstage, or jumps far ahead (or back) to survey long-term outcomes from another era." (taxonomy.json)
- **baseline:** 'ends_at_or_just_after_main_climax_no_forward_jump' human 69% / AI 51% (+18); 'ends_at_distant_future_or_far_past_vantage_summarizing_long_term' human 25% / AI 38% (−13); 'ends_with_brief_forward_epilogue_or_flashforward' human 4% / AI 7% (−4). Recomputed from `storyscope_features.parquet`.
- **moves_toward:** 'ends_at_or_just_after_main_climax_no_forward_jump' — 69% human against 51% AI, the widest gap in the extended set after NC-31.
- **scope:** local (basis: detection_method — "the last narrated scene" is a bounded terminal passage). A2's reading; `feature_cards.md` carries the card and governs scope where the two differ.
- **fact_risk:** none — the move cuts the forward vantage; nothing is added.
- **never_do:**
  - Never cut past the climax itself; the landing is *at or just after* it.
  - Never leave the story mid-arc to avoid an epilogue; 'ends_before_external_resolution_mid_arc_or_cliffhanger' is not the human-elevated value here.
  - Never act on this without NC-33; they land on the same terminal passage (NC-33 cuts the *amount* of aftermath, NC-40 the *temporal vantage* of the last scene) and should be executed as one edit logged under both ids.
  - Never restore a long-term summarizing close because it reads as resonant; that is the AI-elevated value at 38% against 25%.
- **by_content_type:**

| CT row | move |
|---|---|
| CT-01 | literal: find the last narrated scene. If it surveys long-term outcomes from a distant vantage, or looks forward past the resolution, cut back so the story ends at or just after the main climax (69% human against 51% AI). Execute together with NC-33 and log both ids on one line. |
| CT-02 | literal: same, on the real ending as told; cut the distant-future vantage, not the events. |
| CT-03 | n/a |
| CT-04 | n/a |
| CT-05 | n/a |
| CT-06 | analog: cut the jump to a distant-future vantage surveying long-term prospects ("in the coming decades this will transform…") so the closing stops at the stated finding and its limits. |
| CT-07 | n/a |
| CT-08 | analog: cut the distant-future vantage summarizing long-term outcomes so the last narrated scene sits at or just after the central event, or at a pending consequence already reported. |
| CT-09 | analog: cut the distant-future vision of the reader's transformed life or work so the copy ends at the concrete offer and its mechanics. |
| CT-10 | analog: cut the forward-looking flourish ("excited for everything ahead") so the message ends at the ask and its concrete next step. Execute with NC-18, which lands on the same sentence. |
| CT-11 | analog: cut the jump to a distant-future vantage surveying long-term legacy so the statement ends at the aim and its concrete plan. |
| CT-12 | analog: inherit the base-type move for NC-40 (n/a carries over); act on each section's closing paragraph. |
| CT-13 | analog: inherit the base-type move for NC-40 on the prose remainder (n/a carries over); the last prose passage, not a trailing block, is the ending. |
| CT-14 | analog: inherit the base-type move for NC-40 (n/a carries over); language: unvalidated; confidence Low; the humanize pass is skipped by default on this row . |
| CT-15 | analog: cut a distant-future vantage at the close so the text ends at its main point; does not fire when the text has no line of events; unvalidated; confidence Low. |

---

## Part 2 — Fingerprint-specific moves (UC-05 only)

**Status of this section.** Every line is `validated: fiction`, confidence **Low**, drawn only from the fingerprint cards in `feature_cards.md` Part 2 (Table 17, p. 27; §5, pp. 8–9). Fingerprints are properties of five specific model versions — Gemini 3 Flash, Kimi K2.5, DeepSeek V3.2, Claude Sonnet 4.6, GPT-5.4 (p. 3) — and the paper's own six-way attribution reaches only 68.4 macro-F1 with all 257 narrative features (Table 3, p. 8). These moves are applied **only** when the user explicitly asks for them after a UC-05 reading ("make it read less like Claude"); they never enter from the HANDOFF block, because `narrative-check`'s HANDOFF carries core NC-ids only and `narrative-humanize` does not target fingerprints by default (use_case_matrix.md UC-05). Where a fingerprint move conflicts with a core move, the core move wins and the fingerprint move is logged `skipped: conflicts with core NC-xx`. Fingerprint moves are `literal` on CT-01 and `analog` on CT-02 and CT-08 (scene-bearing paragraphs only), following C2's `FP` column; on every other row the `FP` cell is `n/a` and no fingerprint move is made . Per, cards with no printed option have no direction and therefore no move. Per, no move is ever described as making a text "less Gemini" on the strength of NC-16.

| FP-id | Printed option (Table 17, p. 27) | Move away from the source's option | Conflicts / notes |
|---|---|---|---|
| FP-Claude-1 | none printed; §5: "event intensity escalates less than in any other source" (p. 9) | let event intensity rise toward the climax: escalate stakes across the ordered beats already in the text (no new events) | none |
| FP-Claude-2 | none printed; no direction in §5 | no move (: direction unknown) | — |
| FP-Claude-3 | epilogue/flashforward | cut or fold the epilogue; end on the last beat of the main action | none |
| FP-Claude-4 | no (dreams/visions as temporal distortion) | optional and rarely worth it: a dream or vision sequence is new story material and is added only from an element already in the text; usually log `skipped: no material` | none |
| FP-Claude-5 | uncanny/haunted setting mood | shift the prevailing atmosphere of settings away from uncanny/haunted where the plot does not require it (cut the haunting details) | none |
| FP-GPT-1 | salient (gossip and rumor) | let one belief that characters form through hearsay be formed through direct witness instead; reduce rumor as the plot's mechanism | none |
| FP-GPT-2 | distant retrospective | narrate from closer to the events (inside or shortly after) rather than from years or decades later | none |
| FP-GPT-3 | subverts (reader expectation) | let one set-up expectation be fulfilled rather than overturned | may reduce NC-24; core wins if NC-24 is targeted |
| FP-GPT-4 | no (iterative/habitual narration) | add one passage of habitual narration ("every morning she would…") from routine the text already implies | none |
| FP-GPT-5 | partial/ambiguous (reconciliation) | close a broken relationship clearly (repaired or refused) rather than partially | may push NC-18/NC-30 toward the AI side; core wins |
| FP-Gemini-1 | expands (protagonist social trajectory) | hold the protagonist's circle constant or narrow it, using relationships already in the text | none |
| FP-Gemini-2 | primarily direct (balance of speech) | render some direct speech as reported speech | conflicts with NC-29 (human-side: more dialogue); core wins, log skipped |
| FP-Gemini-3 | siege/ordeal (global narrative schema) | no move offered: changing the story's overall schema is a rewrite of the story, outside this skill | — |
| FP-Gemini-4 | named personal name (naming practice) | refer to one or more characters by role or epithet rather than personal name | none |
| FP-Gemini-5 | frequent flashbacks | fewer flashbacks | conflicts with NC-25/NC-27 (human-side: more anachrony); core wins, log skipped |
| FP-DeepSeek-1 | none printed; no direction | no move | — |
| FP-DeepSeek-2 | behavioral cues (emotional expression) | shift the dominant vehicle to explicit labels | same direction as NC-07's human-side move; no conflict |
| FP-DeepSeek-3 | none printed; no direction | no move | — |
| FP-DeepSeek-4 | evenly interleaved (backstory placement) | no move offered: the card records that §5 says DeepSeek "front-loads crucial context" (p. 9) while Table 17 says evenly interleaved, and the paper does not reconcile them | — |
| FP-DeepSeek-5 | none printed; no direction | no move | — |
| FP-Kimi-1 | in-action event (character introduction) | none offered: moving away from in-action would move NC-16 toward external description, the AI side | conflicts with NC-16; core wins |
| FP-Kimi-2 | in medias res (narrative entry frame) | none offered: adding setup before action raises NC-20, the AI side | conflicts with NC-20; core wins |
| FP-Kimi-3 | no (explicit trait labeling) | add one sentence naming a character's trait outright, where action already shows it | none |

**Human fingerprint cards (FP-Human-1 … 5).** These are the human side and are not targets; a user asking to move *toward* them gets: FP-Human-1 (in-dialogue introduction) — the same reorder as NC-16's CT-01 move, opening on a line already in the text; FP-Human-2 (single focal) — keep narration with one focal character, cutting scenes focalized through others only if they carry nothing the main line needs; FP-Human-3 (no direct address) — **not applied**: it points the opposite way from core NC-23 (humans address the reader more, Table 16, p. 26); the paper records both and does not relate them, and the core feature wins; FP-Human-4 (back-loaded revelation) — the same staging as NC-24/NC-26; FP-Human-5 (crossover genre) — no move offered, genre positioning is the whole work.

---
