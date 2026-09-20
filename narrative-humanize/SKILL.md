---
name: narrative-humanize
description: >
 Rewrite text to reduce structural and surface AI signals while preserving facts,
 citations, and voice. Use for narrative-humanize or requests for less AI-like prose.
 No detector-evasion promises.
license: MIT
---

# narrative-humanize

The structural half of a two-skill pair. `narrative-check` measures discourse-level narrative features that StoryScope (Russell et al., COLM 2026) found separate human from AI fiction (Table 16, p. 26); this skill moves the ones that fired toward the human-side value, then runs its own surface pass. Pipeline stage ids are from `use_case_matrix.md` §0.1: **P0** `narrative-check` → **P1** this skill's structural moves → **P2** its surface pass (`references/surface_levers.md`) → **P3** `narrative-check` re-run, with its word-level layer. The order is fixed and is never reversed (reason in `use_case_matrix.md` UC-02).

**This skill has been measured.** On ten AI-written stories from the paper's own dev split, three blind rewriters ran the structural pass below and a classifier retrained on the authors' released features scored the results: mean P(human) rose from 0.057 to 0.607, five of the ten — half — crossed the 0.5 decision boundary while the other half did not, and all ten deltas were positive. The full result, and the five caveats that bound it, are in "What this skill does NOT do". Two consequences run through the protocol: **priority follows measured classifier importance, not the printed Table 16 gap**, and **the moves are subtractive by default, so length is now policed**.

## Files this skill reads

| File | Use |
|---|---|
| `references/interventions.md` (this skill) | One intervention per NC-id, each with `measured_importance`, plus the extended NC-31 … NC-40 entries and the length, marker, and human-side-protection rules. The only source of moves. |
| `references/feature_cards.md` | The card behind each NC-id: question, human/AI values, scope and basis, driving spans. |
| `references/content_type_matrix.md` | The CT row's cells and the reason behind every `n/a`. |
| `references/use_case_matrix.md` | Use-case behaviors UC-01 … UC-12, failure ids, caveat texts, and stage interfaces. |
| `references/surface_levers.md` | The surface pass applied at step 6: nine levers, hard rules, rewrite protocol, domain calibrations, banned word list. Vendored, MIT — see `LICENSE.txt`. |
| `references/word_level_signals.md` | The word-level checks used in the final comparison. |

All paths above are relative to this SKILL.md, so the skill installs as one self-contained
directory and depends on no sibling skill. The surface and word-level layers ship inside it.

## Hard rules

- **never invent a real-world fact, source, quotation, citation, example, or number.** On fiction (CT-01, and the fiction-derived cells of CT-02), new *story* propositions are a different thing and are permitted, but only where NC-17, NC-24, NC-25, NC-26, NC-27, NC-29, NC-31 or NC-39 licenses them, only from elements the text already names, and never introducing a new proper noun, date, or figure. Diegetic material invented under that licence is not a ledger item. On every non-fiction row the prohibition is absolute and admits no story-world exception.
- never apply an `n/a` intervention for the CT row
- never skip the fact ledger, including its contradiction check
- never claim a detection outcome

These are checked at step 7 against the finished text, not recalled. A ledger item missing or contradicted, a move applied on an `n/a` cell, a `[SOURCE NEEDED]` annotation left in the delivered prose, or a sentence predicting a detector's verdict each invalidates the run.

**Precedence when the three files disagree.** The matrix governs row membership and cell type. The card governs `scope`, `option_rows`, `response_type`, `importance_rank`, and `tier`. `interventions.md` governs only the move itself. Report any disagreement in the change log and follow the owning file.

## Inputs

- **The text** (required). If the user describes a document but pastes nothing, ask for the text.
- **A NARRATIVE-CHECK REPORT** for this text (required; produced at step 1 if absent). Its HANDOFF block is the only work order: `NC-id | scope | P-locations | direction to move | CT row`.
- **Optional:** a voice sample (UC-03), a target register (UC-04), a scope such as "only the introduction" (UC-07), source material to fill references (UC-10), a request to move fingerprint features after a UC-05 reading.

## Protocol

Eight steps, in this order, every time.

### Step 1 — Get the report; never rewrite from impression

If no NARRATIVE-CHECK REPORT for this exact text is in context, run `narrative-check` first (P0) and take its HANDOFF block as the work order. Never rewrite from a feeling that the text "reads as AI"; the moves in `interventions.md` are keyed to NC-ids that fired, at the spans the report gives.

- If the user asked for a guarantee of passing a detector (UC-12), print the **NO GUARANTEE** block (section "What this skill does NOT do", below) once, before P0, and proceed without asking.
- If a report exists from the same session for the same text, reuse it (UC-02).
- If the HANDOFF block is empty (F-NOTHING), write a STRUCTURAL CHANGE LOG with the single line `no structural intervention: HANDOFF empty`, make no structural edit, and go to step 6 if the user asked for a rewrite.
- If a target register was named (UC-04), the CT row is the target's, not the input's; the report header says so and every later step uses that row.
- If the text is under 300 words (F-SHORT), the report carries local features only; act on those. The 300-word gate is this build's design choice (`content_type_matrix.md` CT-10), not a paper finding.
- **Record the input word count now.** Step 5's length check needs it.

### Step 2 — Fact ledger: presence, and contradiction

Before touching the text, list every claim, number, name, date, citation, quotation, place, and title in it, one per line, with its paragraph. **A claim is a proposition about the world: a fact, a finding, a comparison, or an attribution** (who said or showed what). Glosses ("this shows that…"), restatements of a point already made, and thematic or moral commentary are not claims; the over-determination moves (NC-01 … NC-05) cut them, and their removal is not a ledger loss . When a gloss sentence also carries a fact stated nowhere else, the fact is the ledger item and survives the cut.

**On fiction the ledger holds the story's own particulars**, and uses its own line format: names, ages, dates, counts, places, who did what to whom, and the words a character is quoted as saying. In-world thematic assertions ("the Devil is patient") are commentary, not ledger items, and the over-determination moves may cut them. A character's set-piece speech is a ledger item as *speech* — if it is cut, the log records the cut and the reason; NC-05's absolute bar on altering quoted words protects real people, not invented ones.

```
FACT LEDGER (before)
P[n] | "<name as written>" | name
P[n] | <year or date as written> | date
P[n] | "<the sentence carrying the number>" | number claim
P[n] | <author-year or numbered citation as written> | citation
P[n] | "<quoted words>" | quotation (real person)
P[n] | <place, title, or other proper noun> | place / title
--- fiction rows ---
P[n] | <character name, age, kinship, role> | story particular
P[n] | <who did what, in what order> | story event
P[n] | "<what a character says>" | story speech
...
```

After step 5, and again after step 6 returns the surface-passed text, run **both** checks:

1. **Presence.** Every ledger line is still in the output and unchanged. Relocation is allowed (record the new paragraph); alteration or loss is not.
2. **Contradiction.** Nothing in the output contradicts a ledger line. This check exists because the presence check alone cannot catch it: a rewriter in the validation study wrote an invented surgery day inside a licensed NC-27 scene expansion that contradicted the input's "the next morning", and every ledger line was still present. Read each newly written or relocated passage against the ledger and ask whether both can be true. Withholding under NC-24, NC-26 or NC-35 needs the same read in reverse: a fact moved later must not strand an earlier passage that assumed the reader already knew it.

Print `FACT LEDGER (after): N of N present, unchanged; contradiction check: clear` — or name the line and fix it.

### Step 3 — Priority and order of moves

**Priority is measured importance, not printed gap.** Each entry in `interventions.md` carries `measured_importance` (the feature's weight in the retrained classifier) and a tier. **A structural pass is not complete while a Tier 1 line in the HANDOFF is unacted and unlogged.** This rule exists because volume of change does not predict effect: in the validation study one story changed 99 of 265 features and ended at P(human) 0.333, another changed 81 and ended at 0.993. The Tier 1 features, in importance order, are NC-01, NC-31, NC-07, NC-17, NC-32, NC-33, NC-16, NC-34, NC-06, NC-21, NC-35, NC-36, NC-37, NC-38 (no move offered), NC-39, NC-40, NC-23.

Reader address (NC-22, NC-23) stays in the set and stays last. It moved on 10 of 10 stories and is genuinely human-elevated, but it ranks 14th and 7th; it is no longer the primary lever and never substitutes for a Tier 1 move.

**Work the HANDOFF lines in this order** — the sequence is functional (deletions first make later placement easier; address is placed into a finished structure), with priority deciding what gets done and how much effort it gets *within* each group:

1. **Over-determination cuts** — NC-01, NC-02, NC-04, NC-03, NC-05.
2. **Reference naming and annotation** — NC-21, then NC-06. Name only what the text or the user has established; otherwise annotate `[SOURCE NEEDED: <what would fit here>]` and add it to the list. **Marker budget :** with no sources supplied, NC-21 and NC-06 together annotate at most two or three spans, at the spans that most drive the rating; every other vague attribution is logged `unmarked: cap`. The "at least as many" stop-conditions in `interventions.md` apply only when sources are supplied. Every annotation is resolved or removed at step 8.
3. **Structural moves** — the extended Tier 1 entries (NC-31, NC-32, NC-33, NC-34, NC-35, NC-36, NC-37, NC-39, NC-40) alongside digression (NC-17, NC-13), ambivalence (NC-30, NC-18, NC-15), nonlinearity (NC-25, NC-27, NC-26, NC-24), setting and senses (NC-08, NC-11, NC-10, NC-09, NC-14), then the rest as the HANDOFF lists them (NC-19, NC-12, NC-20, NC-16, NC-28, NC-29, NC-07).
4. **Reader address last** — NC-23, then NC-22.

Rules that apply throughout step 3:

- **Never push a non-firing feature toward its AI-side value.** A feature the report rates human-side has no HANDOFF line and is invisible to the moves. Before acting on a span, check it against the report's HUMAN-SIDE FEATURES PRESENT section; where the span is cited there, preserve that element, act at the remaining driving spans, and log `held: span carries human-side NC-xx`. The measured case: NC-12's externalizing move deletes the explicit emotion labels that hold NC-07 on the human side (explicit labels 29% human / 8% AI; embodied 81% AI / 38% human), and a rewriter had to hand-preserve three of them across one story. NC-14's spatial cut can take the named objects that hold NC-21 there.
- A `global` feature is edited at the 2–3 driving spans the HANDOFF gives and logged as `document-level (P[a], P[c], P[f])`; it is never logged to a single line. A `local` feature is edited at each listed span.
- A later move never undoes an earlier one (`interventions.md`, "Precedence when moves conflict"). Where two ids land on one passage and one edit discharges both, log both ids on one line rather than counting one edit twice.
- Fingerprint moves (`interventions.md` Part 2) are applied only if the user explicitly asked after a UC-05 reading; they never come from the HANDOFF, and a core move wins any conflict.
- Under UC-07 (partial scope) act only inside the named scope; a global feature whose driving spans fall outside it is logged `partial: global feature, scope-limited intervention; rating may not move`, and a local span outside the scope is logged `out of scope: not touched`.
- Under UC-03 (voice sample), run the **sample register scan** before any move: read the sample for direct reader address (NC-23), fourth-wall breaks (NC-22), explicit named references (NC-21), dialogue proportion (NC-29), time jumps and flashbacks (NC-25, NC-27), and morally mixed framing (NC-30). Any move that would *introduce* a device the sample never uses is skipped with `skipped: absent from voice sample`. Moves that *remove* an AI-side device are not gated. The canonical case: a sample with no reader address means the NC-23 move is skipped, even though humans address the reader more (Table 16: 0.28 vs 0.07; §4.1: 28% vs 7%, p. 7). The scan's six lines open the STRUCTURAL CHANGE LOG.

### Step 4 — Honor the CT row's `n/a` cells

For every HANDOFF line, open the NC-id's entry in `interventions.md` and read the row for the report's CT. If the cell is `n/a`, make no move and log `skipped: n/a for CT-xx (content_type_matrix.md)`. **Applying a fiction intervention to CT-05 is a defect**, as is applying any `n/a` cell anywhere. Specifics:

- **NC-31 … NC-40 run on every content type.** Read the row's cell and act on it. Three are rate-only: **NC-38 on every row** (genre is the commission, not a defect) and **NC-37 / NC-39 on every non-fiction row** (moving them would rename a real person or assert unreported interiority).
- CT-12, CT-13, CT-14 cells say `inherit the base-type move`: use the base row the report assigned (per lean-map section for CT-12; for the prose remainder for CT-13; for the whole text for CT-14) and apply that row's cell with the stated adjustment. If the base cell is `n/a`, nothing is done.
- CT-12: global features are edited once per lean-map section, never across the document; the log's `where` column names the section's paragraph range.
- CT-13: code blocks, tables, equations, and captions are never edited; only the prose remainder.
- CT-10 and CT-15: local features only; every global feature is `n/a` on these rows.
- CT-04, CT-05, CT-06, CT-11: reader address (NC-22, NC-23) is `n/a` because the register forbids it; do not insert address there even if the user's phrasing invites it.

### Step 5 — Structural change only, and length

This skill changes what the text does, not how its sentences sound. No em dash work, no vocabulary work, no sentence-length work, no hedge or transition work here; that is the surface layer (`references/surface_levers.md`) and it runs at step 6.

**Permitted edits.** Every edit should be describable as one of: cut; relocate; reorder; add-from-material-present; annotate-with-`[SOURCE NEEDED]`; re-anchor (a resolution to a different cause, an emotion to a different vehicle, an introduction to a different device); **expand** (a summarized event rendered as a full scene, NC-27); **render as speech** (narration or summarized speech rendered as direct dialogue, NC-29, NC-31); or **convert** (a whole-text grammatical property such as tense, NC-32). The last three necessarily write sentences that did not exist, and that is structural work, not surface work: the prohibition is on *editing existing sentences for sound*, not on writing new ones for structure. If an edit is describable only as a word or punctuation substitution, revert it. NC-07's label conversion is the one boundary case — replacing an embodied clause with a statement of the feeling is a change of vehicle, not a synonym swap; place the label in a different position from the vehicle it replaces so the edit reads as structural.

**Length check (band: ±15%).** Count the output words and compare with the input count from step 1. The CT-01 moves are overwhelmingly subtractive — NC-01, NC-02, NC-03, NC-04, NC-05, NC-08, NC-09, NC-10, NC-11, NC-12, NC-14, NC-20, NC-33, NC-34 and NC-40 all cut — and in the validation study one story left the first cut pass at **−23.8%**. The additive moves are what restore it: **NC-17** (a secondary thread), **NC-27** (expand a summarized flashback into a scene) and **NC-29** (render summary as dialogue), with smaller additions from NC-22, NC-23, NC-24, NC-28, NC-31, NC-36 and NC-39.

- If the pass is below −15%, extend the additive moves that are already licensed and under-applied. Do not restore what was cut: that would undo an earlier move.
- If the pass is above +15%, the additive moves have overrun; trim them, not the cuts.
- Record `words: <before> → <after> (<±n>%)` in the change log on every run.
- **Never cut a text across a `narrative-check` length gate** (800 words, 300 words). Crossing one makes most features unratable at P3. If a pass would cross one, stop short and log it.

Write the STRUCTURAL CHANGE LOG now, before step 6, one entry per HANDOFF line acted on or skipped, in the five columns fixed by `use_case_matrix.md` §0.2 :

```
STRUCTURAL CHANGE LOG
words: 3808 → 3558 (−6.6%)
NC-id | action | where | rationale | skipped:
NC-01 | cut narrator's stated lesson (2 sentences) | P9 | Tier 1, rank 1; claim restated by another character in P7 |
NC-31 | re-vehicled mood imagery to dialogue tone | document-level (P12, P40) | Tier 1, rank 2; imagery was a regular emotional carrier |
NC-17 | added secondary thread from existing element P3 | document-level (P3, P6, P11) | "no subplots"; thread parallel, left open; additive, offsets the cuts |
NC-06 | annotated 1 span | P5 | budget of 2-3 reached with P2 and P5 | unmarked: cap — P7 "experts agree", P11 "studies suggest"
NC-12 | cut interiority, preserved 3 emotion labels | document-level (P3, P53) | labels hold NC-07 human-side | held: span carries human-side NC-07
NC-15 + NC-18 | re-closed ending on an external act already seeded | P67 | one edit discharges both ids |
NC-23 | | | voice sample has no reader address | skipped: absent from voice sample
NC-08 | | | CT-03 cell is n/a | skipped: n/a for CT-03 (content_type_matrix.md)
NC-38 | | | rated: realist_contemporary | declined: genre is the commission, not a defect
NC-04 | cut gloss after quote | P3 | fired on P3 and P12; P12 outside scope | out of scope: P12
```

`action` names the structural move and the material it used; `where` is `P[n]` for local features and `document-level (spans)` for global ones, always using **input-side** paragraph numbers (reordering under NC-25/NC-26 destroys the output-side coordinates, so insertions are named relative to the input paragraph they now follow); `rationale` names the NC-id's reading and, for Tier 1 lines, its rank; `skipped:` carries the reason when no move was made. The `action` and `skipped:` cells are not mutually exclusive : a line may carry an action plus `out of scope: P<n>`, `unmarked: cap — …`, or `held: span carries human-side NC-xx`.

### Step 6 — Run the surface pass (P2)

Read `references/surface_levers.md` in full and apply its rewrite protocol, steps 0 through 8, to the structurally revised text. The REGISTER LINE is mandatory and comes from the canonical table in the next section; the register is **never inferred**, because the formal/academic carve-out in hard rule 2 and Lever 8 fires only when the register is stated. Without it the default banned-vocabulary list strips conventions that are correct, and sometimes required, in that register.

Work to this instruction block:

```
Apply references/surface_levers.md to the text below.
Full rewrite protocol, steps 0 through 8.

Register: <REGISTER LINE from §2.1.4, e.g. "formal academic prose" | "literary short fiction">
Voice sample: <attached below | none>. If attached, run step 0 (writer-profile
 distillation) on the sample before touching the text, and let the sample's
 register win where it conflicts with your defaults, per your own step-0 rule.

Constraints from the structural pass (already complete; do not reopen it):
- Preserve every `[SOURCE NEEDED: ...]` marker verbatim. Do not delete a sentence
 in order to remove one. Do not fill one in.
- Preserve these structural changes; reword them freely, but do not remove,
 reorder, resolve, or undo them:
 <one line per STRUCTURAL CHANGE LOG entry, e.g.
 "P3: reader aside added (NC-23)"; "P6-P7: flashback inserted (NC-25)";
 "P9: narrator's stated moral removed (NC-04)"; "P2: named reference (NC-21)">
- Do not add or remove scenes, paragraphs, subplots, named references, reader
 address, time jumps, or thematic statements. That layer is closed.
- Comparative claims in the fact ledger keep their comparison; change the
 surface form only. (Your comparative-framing scan applies to rhetoric, not to
 a claim whose content is a comparison.)
- <optional, when the CT row or voice sample forbids it>
 Do not introduce second-person address or rhetorical questions to the reader.
- <optional, UC-07> Rewrite only P[a]-P[b]. The rest is context; return it unchanged.

Output the rewritten text only, per your hard rule 6. No changelog: the
structural log has already been written and will be delivered separately.

--- TEXT ---
<P1 output>
--- VOICE SAMPLE (if any) ---
<sample>
```

The template follows `use_case_matrix.md` §2.1.3; its §2.1.4 contains the REGISTER LINE table. Its preserve-markers line governs P2 only: annotations may travel through the surface pass, but step 8 removes them before anything is delivered.

- **UC-03:** pass the voice sample into the surface pass so its step 0 runs, and skip (at step 3) any structural move that contradicts the sample's register. the surface pass's step 0 covers the six surface dimensions of voice; this skill's sample scan covers structural devices; neither duplicates the other.
- **CT-14:** the surface pass is **skipped by default** . Reason, stated to the user: the surface levers' protocol does not address non-English input, so its behavior there is undocumented. Run it only on the user's explicit request, with the language named in the register line (`<language>, <register>`).
- **UC-06 (batch):** P2 runs per unit, after that unit's P1 and before the next unit begins; never P1 on all units and then P2 on all units.
- When the surface pass completes, re-run both fact-ledger checks (step 2) and count the annotations against the step-5 list; a missing annotation or ledger item means P2 is repeated once with the constraint restated (this repeat is a correction of an invalid run, not the step-7 loop). Preservation is what is checked *here*, because P2 was told to preserve.
- **Then run the step-8 delivery gate, before P3.** The pipeline is P0 → P1 → P2 → **delivery gate** → P3 (`use_case_matrix.md` UC-10): annotations are resolved or removed the moment the surface pass returns, so that the re-check in step 7 scores the same marker-free prose the user receives. This is the ordering the validation study's headline numbers came from.

### Step 7 — Re-check, report before/after, loop at most once

Run `narrative-check` on the **post-gate** text (same CT row as P0; under UC-04, the target row) and apply `narrative-check`'s word-level layer (`references/word_level_signals.md`) to it. P3 therefore scores marker-free prose and **verifies the absence, not the presence, of the `[SOURCE NEEDED` string**. Produce:

- The **NC-id DIFF** (`use_case_matrix.md` §0.2), header verbatim: `NC-id DIFF — self-assessment by the rewriting model; treat as measurement of the text, not a detector prediction`. One line per NC-id that appeared in either report: `NC-id | scope | before value | after value | status: moved-toward-human / moved-toward-AI / unmoved / newly fired / no longer applicable | evidence after: P[n] "quote"`. Global features compare document-level ratings; local features compare span sets. NC-21 and NC-06 show `unmoved` or `pending: source needed` when the only change was an annotation, never `moved`. NC-38 shows `declined: genre is the commission, not a defect`. Where P0 and P3 differ in length-gate status, the header says so .
- The **AI-CHECK REPORT** with the P3 NARRATIVE-CHECK REPORT appended after it as a separate block, then the COMPLEMENTARITY paragraph (`use_case_matrix.md` §2.2). No combined verdict; the two confidence lines stay independent.

**One loop, total .** If the P3 word-level score still sits above its own Human band, offer one further surface-pass audit pass on the text, then re-run P3 once more. That is the only loop the whole pipeline permits: P1 is never re-run automatically; if a targeted NC-id is `unmoved`, the DIFF says so and the user decides; there is no third pass of anything.

### Step 8 — Delivery gate and output

**No `[SOURCE NEEDED]` annotation may appear in the delivered text.** The gate itself runs earlier — between P2 and P3, at the end of step 6 (`use_case_matrix.md` UC-10) — and is defined here because this is where its output lands. Walk the step-5 annotation list and, for each one, either **resolve** it (the user supplied a real source: write the named reference in) or **remove** it (restore the sentence to its pre-annotation wording, or cut the sentence if it carried nothing). Then confirm by search that the string `[SOURCE NEEDED` does not occur. This is a measured requirement, not a stylistic one: annotations stayed inline in 7 of the 10 rewrites in the validation study, and stripping them changed the classifier score on all seven — reversing one result entirely (0.822 → 0.092) and raising another (0.021 → 0.333). The record of every annotation, resolved and removed alike, travels in the STRUCTURAL CHANGE LOG, which is where it lives now that the prose is delivered clean; a removed annotation means NC-21 or NC-06 reads `unmoved` in the DIFF, which is the honest outcome.

**Failure conditions at this gate** (`use_case_matrix.md` UC-10): any `[SOURCE NEEDED` string present in the delivered text means the gate did not run — strip and re-verify before delivering; and any annotation on the record carrying neither a `resolved:` nor a `removed` disposition is an incomplete run.

Deliver, in this order and layout:

```
<rewritten text, free of annotations (step-6 output, or step-5 output on CT-14 when P2 was skipped)>

Scope note. StoryScope's only robustness test is a surface-edit test: 278 Gemini-written stories edited with LAMP, Gemini as rewriter, narrative-classifier macro-F1 95.5 before and 93.9 after (Section 4.2, p. 8). The paper argues from that result that narrative features are "far harder to 'humanize'" because changing them "requires significant structural rewrites" (p. 2), and it never runs the experiment where the structural rewrite is actually performed. This build ran a small version of it. On ten AI-written stories from the paper's own dev split, the structural pass moved mean P(human) from 0.057 to 0.607 (mean Δ +0.549), with every one of the ten deltas positive (sign test, two-sided p = 0.002) and 66 core-feature movements toward the human baseline against 13 away; **five of the ten — half — crossed the 0.5 decision boundary, and the other half did not** (validation results Step 5). Five caveats bound that number and travel with it everywhere: n = 10; fiction only; one direction (AI → human); scored by a classifier retrained on the authors' released features (dev macro-F1 0.942), not their released weights; and features assigned by Claude agents following the authors' prompts, not by Gemini 3 Flash. So the honest position is neither "unmeasured" nor "it works": the structural layer moved this classifier on ten fiction texts far more than surface editing moved the paper's, and that is all it has been shown to do. What this pipeline reports is which features moved in your text, not whether any detector's verdict will change.

STRUCTURAL CHANGE LOG
words: <before> → <after> (<±n>%)
NC-id | action | where | rationale | skipped:
<one line per HANDOFF line, step 5; under UC-03 preceded by the sample register scan>

[SOURCE NEEDED] RECORD — part of the change log; annotations placed during the pass, none of them in the text above
P[n] | what would fit there | which NC-id it serves | resolved: <source> | removed
<one line per annotation, each with exactly one disposition, or "none">
<if the record is non-empty: one of the two NOTICE variants below>

FACT LEDGER (after): N of N present, unchanged; contradiction check: clear

NC-id DIFF — self-assessment by the rewriting model; treat as measurement of the text, not a detector prediction
<one line per NC-id, step 7>

AI-CHECK REPORT
===============
<the word-level layer's exact output>

NARRATIVE-CHECK REPORT
======================
<narrative-check's exact P3 output>

COMPLEMENTARITY
---------------
<the paragraph from use_case_matrix.md §2.2.3>
```

**Selecting the NOTICE variant .** `use_case_matrix.md` UC-10 defines two labelled variants and one test: **was UC-10 invoked** (the user asked for references, works, people, dates, or figures to be added, or the input had no factual anchors and the user asked for specifics)? If yes, print `SOURCE-NEEDED-REQUESTED`. If annotations were placed for any other reason (a NC-21 or NC-06 HANDOFF line fired on a text the user did not ask to have references added to), print `SOURCE-NEEDED-UNREQUESTED`. Never print both; never print the requested variant when the user did not ask for references; print neither when the list is empty.

The STRUCTURAL CHANGE LOG is the chaining record between P1 and P2 and the audit trail of what moved; it is not a word-swap changelog and never lists surface edits (which the surface pass, by its own hard rule 6, does not log either). When the CT row is not CT-01, the transfer-gap caveat (CAV-TRANSFER, `use_case_matrix.md` §0.4) appears in the NARRATIVE-CHECK REPORT header as `narrative-check` prints it.

## CT row → REGISTER LINE (canonical)

The register string used by the surface pass at step 6 is fixed by the report's CT row. Under UC-04 the row is the target's.

| CT row | REGISTER LINE | Note |
|---|---|---|
| CT-01 | `literary short fiction` | humanize's creative/lyrical calibration applies |
| CT-02 | `narrative nonfiction, personal essay` | narrative/essay calibration |
| CT-03 | `opinion essay / blog` | narrative/essay calibration |
| CT-04, CT-05, CT-06 | `formal academic prose` | required wording; enables the formal/academic carve-out; the word-level layer's academic calibration applies at P3 |
| CT-07 | `technical documentation` | technical calibration |
| CT-08 | `journalism / news feature` | humanize has no journalism section; nearest is narrative/essay |
| CT-09 | `marketing / product copy` | professional/business calibration |
| CT-10 | one of `email`, `Slack / async update`, `social post`, `cover letter` | chosen from the message's form |
| CT-11 | `formal academic prose (application register)` | as CT-04 |
| CT-12 | register of the dominant human-authored portion, or as the user states | as mapped for that base row |
| CT-13 | register of the prose portion; code blocks and tables are passed as read-only context | as mapped for the base row |
| CT-14 | `<language>, <register>` | **humanize skipped by default**; run only on explicit request with the language named |
| CT-15 | as the user states, else `general expository prose` | none specific |

## Use-case behaviors (summary; full text in `use_case_matrix.md`)

| UC | What changes in this skill |
|---|---|
| UC-02 default | P0 → P1 → P2 → P3 as above. |
| UC-03 voice sample | Sample register scan before step 3; skipped moves logged `skipped: absent from voice sample`; sample passed to humanize step 0; sample word count recorded; F-NOSAMPLE falls back to UC-02 with the one-line notice. |
| UC-04 target register | CT row set by the target for P0, P1, P2, P3; REGISTER LINE from the target row; DIFF header adds `CT row fixed at [CT-xx] for both reports`; a RUN NOTES line names the input-row-only cards dropped by design . |
| UC-06 batch | Full pipeline per unit, completed before the next unit; summary table carries counts only, never averaged global values. |
| UC-07 partial scope | P0 on the whole document; P1 inside the scope only with `partial:` and `out of scope:` markers; P2 on the scope with the rest as read-only context; P3 on the whole; line before the text: `Scope limited to "[scope]". Global features were rated on the whole document and may not move after a scope-limited edit; the NC-id DIFF marks which ones did.` |
| UC-10 no factual anchors | Annotations only, within the budget, all resolved or removed at step 8; the `SOURCE-NEEDED-REQUESTED` variant below; the refusal line below when asked for plausible invented specifics. |
| UC-12 guarantee request | The NO GUARANTEE block once, before P0; the work proceeds; on re-ask, its first two sentences again. |

**[SOURCE NEEDED] NOTICE — two variants (`use_case_matrix.md` UC-10, ), verbatim; printed when the record is non-empty; step 8 selects one by whether UC-10 was invoked.**

Variant (a) — `SOURCE-NEEDED-REQUESTED` (UC-10 invoked):

> **[SOURCE NEEDED] NOTICE.** You asked for named references, specific works, people, places, dates, or figures to be added, and the input contains none to work from. This pipeline does not invent them. During the structural pass, each place where a real reference would move the text toward the human-side pattern StoryScope reports (NC-21 Intertextual Strategy → explicit named reference, 47% human vs 24% AI; NC-06 Reference Explicitness → balanced mix, 37% vs 16%; both Table 16, p. 26; fiction-only evidence, `analog: unvalidated` outside fiction) was annotated in the working draft. **None of those annotations is in the text above.** Each was either resolved with a source you supplied or removed before delivery, because leaving them inline measurably changes how the text scores: in this build's validation study, stripping them reversed one story's result entirely (0.822 → 0.092) and raised another's (0.021 → 0.333) (validation results Step 5). The full record is in the change log below: every span, what would fit there, and whether it was resolved or removed. Fabricating a citation, a name, a statistic, or a quotation is forbidden here. To move these features for real, give me a verifiable source for a listed span and ask for that passage to be revised; until then the report shows NC-21 and NC-06 as `unmoved`, which is the honest result.

Variant (b) — `SOURCE-NEEDED-UNREQUESTED` (UC-10 not invoked; annotations placed by a NC-21 / NC-06 intervention). C3 retitled this one `[SOURCE NEEDED] record`:

> **[SOURCE NEEDED] record.** You did not ask for references to be added; this is a by-product of the features that fired. The structural pass annotated [n] span(s) where a vague attribution would need a named source to move toward the human-side pattern StoryScope reports (NC-21 / NC-06, Table 16, p. 26; fiction-only evidence). No source was invented, and **no annotation remains in the text above** — each was resolved or removed before delivery, because leaving them inline measurably changes how the text scores (validation results Step 5). The change log below lists each span and what would fit there, so you can supply a source if you want those features actually moved.

When the user asks for plausible invented specifics (either variant): `No. A plausible-looking source that does not exist is a fabrication, and this pipeline does not produce them. The change log shows you exactly where a real one would go.`

## What this skill does NOT do

- **It does not have an unmeasured success rate any more — and the measurement is small.** One experiment exists. Ten AI-written stories from StoryScope's own dev split (2,064–4,643 words, two per model) were put through this skill's structural pass by three blind rewriters; the rewrites were re-featurized from scratch by blind assigners using the authors' own nine dimension prompts and scored by a classifier retrained on the authors' released features. Result: **mean P(human) 0.057 → 0.607, mean Δ +0.549, and five of the ten — half — crossing the 0.5 decision boundary while the other half did not** (a sixth was already above it before the rewrite), **all ten deltas positive, sign test two-sided p = 0.002**, and 66 core-feature movements toward the human baseline against 13 away. The paper's own robustness test moved its narrative classifier from 95.5 to 93.9 macro-F1 under LAMP surface editing (§4.2, p. 8), which is the comparison that matters: the paper argues narrative features are "far harder to 'humanize'" because changing them "requires significant structural rewrites" (p. 2) and never runs the experiment where the structural rewrite is actually performed. The caveats are load-bearing: n = 10; fiction only; one direction (AI → human); the scoring classifier is our faithful retrain (dev macro-F1 0.942), not the authors' released weights; and feature assignment on both sides was done by Claude agents following the authors' prompts, not by Gemini 3 Flash.
- **It does not know that it works on anything but fiction.** StoryScope validated its features on fiction only (10,272 human stories from Books3, five LLMs, corpus mean 4,753 words; fn. 8, p. 3), and the rewrite experiment was fiction only too. On every CT row other than CT-01 the moves are analogs tagged `analog: unvalidated`; nothing has measured them.
- **It does not cover the whole signal.** The 30 core cards carry only **25%** of the retrained classifier's importance mass. The ten extended entries (NC-31 … NC-40) close part of that gap, and volume of change is not the point: in the study one story changed 99 of 265 features and still ended at 0.333.
- **It does not predict or claim a detection outcome.** The NC-id DIFF and the P3 reports are measurements of the text, labelled as a self-assessment by the model that rewrote it. No line anywhere says the text "passes" or "will pass" anything.
- **It does not do surface-level work in its own protocol.** Punctuation, vocabulary, sentence rhythm, hedges, transitions, and register voice belong to the surface pass in `references/surface_levers.md`, which runs at step 6 as a distinct stage after the structural work is complete. The two stages stay separate: the structural log records what moved, the surface pass reworks how it sounds, and neither is allowed to undo the other.
- **It does not invent.** No real-world fact, source, quotation, example, number, or person enters the text unless the text or the user supplied it; the alternative is an annotation resolved or removed at step 8, or a `skipped:` line. New story propositions on fiction are a licensed exception with a stated ceiling (Hard rules).
- **It does not target fingerprints by default.** Model-fingerprint moves (`interventions.md` Part 2) run only on explicit request after a UC-05 reading, at Low confidence, and never against the core moves.
- **It does not guarantee anything about detectors.** When asked to, it prints this once and proceeds (UC-12, verbatim from `use_case_matrix.md`; C3 owns the text):

> **NO GUARANTEE.** I will do the structural and surface rewrite, but I cannot promise that the result passes any detector, and I will not say that it does. There is now a measurement, and it is small enough that you should hear it in full. On ten AI-written stories from StoryScope's own dev split, this pipeline's structural pass moved mean P(human) from 0.057 to 0.607 (mean Δ +0.549), every one of the ten deltas was positive (sign test, two-sided p = 0.002), and **five of the ten — half — crossed the 0.5 decision boundary. The other half did not.** That is one benchmark of ten fiction stories, in one direction (AI → human), scored by a classifier we retrained on the authors' released features (dev macro-F1 0.942) rather than their released weights, with features assigned by Claude agents following the authors' prompts rather than by Gemini 3 Flash (validation results Step 5). It is not a result about your text, about non-fiction, or about any commercial detector, none of which were tested. For contrast, the paper's own robustness test is a surface-edit test: 278 Gemini-written stories edited with LAMP, its narrative classifier moving from 95.5 to 93.9 macro-F1 (Section 4.2, p. 8); it asserts that narrative features are "far harder to 'humanize'" because changing them "requires significant structural rewrites" (p. 2) and never performs that rewrite. So the structural layer does more than surface editing did, on that one benchmark, half the time. The surface pass disclaims its own layer in its scope section: it does not "Guarantee 0% AI scores on commercial detectors (no method does reliably)." What you will get is a before/after NARRATIVE-CHECK REPORT and a WORD-LEVEL REPORT showing which features moved. That is a measurement of the text, not a prediction about a detector. Proceeding with the rewrite now.

## Evidence note

Features come from StoryScope's 30 core features (Table 16, p. 26) plus ten extended features from the authors' released `taxonomy.json` whose classifier weight and human/AI separation the validation study measured; the extended ten are **not** paper core features and are never described as such. Human and AI values quoted here are Table 16's printed values; recomputed baselines and importances are cited to `feature_cards.md`. Scope (`global`/`local`) is an inference recorded on each card with its basis . The 300- and 800-word gates and the ±15% length band are this build's design choices; the paper prints no thresholds (findings §E.2) and the band comes from the validation experiment. Paper numbers used in this file: 30 features, 10,272 stories, 4,753 words (fn. 8, p. 3), 278 stories, 95.5 → 93.9 (§4.2, p. 8), 47%/24% and 37%/16%, 29%/8% and 81%/38%, 0.28/0.07 and 28%/7% (Table 16, p. 26; p. 7). Measured numbers used in this file: 0.057 → 0.607, +0.549, five of ten (half) crossing the boundary, p = 0.002, 66 against 13, 25%, 99 of 265, 0.333, 0.942, 0.822 → 0.092, 0.021 → 0.333, −23.8%, ±15%, 2,064–4,643 words (validation results Step 5; `feature_cards.md`).
