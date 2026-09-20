# Calibration notes

**Extracted from** `SKILL.md` at publication, to keep the SKILL.md body within the
agentskills.io 500-line guidance. Content unchanged.

## Calibration notes

- **Present is not AI-side.** The human means are not zero. Humans moralize at 3.28 on
 NC-01 (AI 3.94, Table 16, p. 26); a story that states its theme once sits near the human
 mean, not at 5. Rate the whole document's explicitness, then ask which side of 3.28 it
 falls on.
- **A single sentence is not a document-level 5.** One narratorial aside that names the
 lesson is one NC-04 firing (a local, binary card) and one driving span for NC-01. It does
 not by itself make NC-01 a 5; a 5 is a text that states what it means repeatedly and
 leaves nothing to infer.
- **For several option cards the AI-side option is also the human majority.** "No
 subplots" is 57% of human stories against 79% of AI (NC-17); narratorial thematic
 commentary appears in 52% of human stories against 77% (NC-04); implicit echoes are 50%
 against 72% (NC-06); olfactory imagery is 57% against 82% (NC-10). An AI-side firing on
 these cards is a relative statement about where the text sits, not evidence on its own.
 The count in STRUCTURAL LEAN carries the weight; no single card does.
- **Two-row cards (NC-06, NC-07, NC-17) have three outcomes.** Each is rated once on its
 dominant option. NC-06: `implicit echoes` (50% human / 72% AI) is AI-side, `balanced mix`
 (37% / 16%) is human-side, `none` and `explicit named` are neutral. NC-07: `embodied`
 (38% / 81%) is AI-side, `explicit labels` (29% / 8%) is human-side, `behavioral cues` and
 `ambiguous` are neutral. NC-17: `no subplots` (57% / 79%) is AI-side, `thematically
 parallel` (42% / 21%) is human-side, `contrasting` and `independent` are neutral. A
 neutral outcome is still a rated card and appears in the CARD LEDGER.
- **`no evidence` is a finding, not a gap in the work.** Use it when the object of the
 question is missing from the text or the excerpt (Step 4). Do not convert it to AI-side
 because "the text does not do X": that reading is reserved for human-elevated features
 the text had room to show and did not.
- **Short texts are likely to starve the human-elevated diversity cards.** Subplots (NC-17),
 location variety (NC-28), time jumps (NC-25), and anachrony (NC-27) need room; the paper's
 stories average 4,753 words (fn. 8, p. 3). On a text far shorter than that these cards
 are likely to drift AI-side for lack of space rather than lack of craft (this build's
 inference; the paper's length audit does not go below its shortest tertile, Table 11,
 p. 22). That is one reason for the Low cap under 800 words; name the possibility in the
 RUN NOTES when it plausibly applies.
- **Small gap ratios are weak leans.** A rating of 3 on NC-01 is human-side by a fraction of
 a point. The side label is binary; the gap ratio is not. Print the ratio and let the
 reader weigh it; do not inflate a value to make a side "count".
- **Rate from the text, not from its polish.** Surface fluency, vocabulary, and punctuation
 are the word-level layer's domain (`word_level_signals.md`). A text can be surface-clean and fire here (UC-11), or
 surface-rough and rate human-side on every card. Do not let one report's result steer
 the other.
- **Register-mandated absence is `n/a`, not AI-side.** A research abstract that never
 addresses its reader is obeying its genre; the matrix marks NC-22 / NC-23 `n/a` on those
 rows for that reason. Follow the cell, not the instinct.
- **NC-22 and NC-23 are read as prevalence.** Table 16 prints 0.67 / 0.39 and 0.28 / 0.07 as
 means over 0-based ordinal codes; §4.1's 67% / 39% and 28% / 7% are the same numbers with
 the decimal moved, a paper erratum settled by recomputing Table 16 from the released
 features (; validation results Step 1). Count the firing spans, place the
 text on the card's rungs, and show both numbers as the cards do. These two were the
 rubric's strongest cards in the fidelity study — 10/10 exact, zero bias — so a firing here
 is worth more than a firing on a card that disagreed.
- **FP-Human-3 and NC-23 are both in the paper and point opposite ways.** NC-23 (PER
 dimension) shows humans addressing the reader more often; FP-Human-3 "no direct
 address" (SIT dimension) is a human fingerprint. They are different features, the paper
 does not relate them, and a UC-05 report may show both without contradiction .
- **Disagreement between honest raters is expected.** In the paper's own validation, two
 human annotators agreed with each other at Cohen's κ 0.74 (Table 7, p. 19), lower than
 either agreed with the model (mean κ 0.84). The confidence caps reflect that: a rating
 here is one reading of a feature that trained readers disagree about, and a user's
 different reading is handled by UC-09, not by re-scoring to agreement.
- **Some cards are measurably weaker than others; weight them accordingly.** In the fidelity
 study (validation results Step 4) exact agreement with the authors' assignments
 ran from 1.00 (NC-22, NC-23, NC-30) down to 0.20 (NC-20), 0.30 (NC-06, NC-10, NC-14) and
   0.40 (NC-01, NC-02, NC-05, NC-07, NC-21, NC-24, NC-27). The cards carry corrected rules for
 the weak ones, but a lean that rests mostly on those cards is softer than the count makes it
 look; say so in RUN NOTES rather than reporting a bare count.
- **A non-firing option card is neutral, and neutral is not a quiet human-side.** The
 `Neutral (rated, neither side)` line exists so that "the AI-elevated option did not fire"
 is visible without being counted as evidence for the human side (Step 4). Resist the pull
 to read a large neutral count as a human-leaning result; it means the card found nothing.
- **Watch the length-gate cascade on documents with a lot of non-prose.** The gate runs on
 prose after exclusion, so a document that looks long enough can fall under 300 or 800 words
 once its tables and code are removed, which changes the confidence cap and can change the
 base row and therefore which cards run at all . This is correct behavior, not a bug,
 but it is invisible unless reported: print the total and the post-exclusion count together,
 and when the exclusion crossed a gate, say which row the document would otherwise have had.
- **A CJK document cannot be word-gated at all.** Whitespace tokenisation reports almost no
 words for Chinese, Japanese, or Thai and would send a long document to CT-10, dropping every
 global card . Use the 600 / 1600-character gate from Step 2, name it in the report,
 and treat the resulting cap as softer than a word-gated one: the character thresholds are
 this build's proposal, with less behind them than the word gates, which are themselves not
 the paper's.
- **Numbers from the paper are safe to quote; numbers about the build are not.** A Table 16
 mean or a page cite is frozen — it cannot drift, so this file states it directly. Anything
 the build owns and revises — which cards a row rates, how many cells are live, which values
 a field may take, how many cards exist — is read from the matrix or the card at run time and
 never quoted from memory. That is the line to hold when you are tempted to write a total
 into a report sentence.
- **When this skill, the matrix, and the cards disagree about which cards apply, the matrix
 row governs — and the disagreement is a defect to report, not to resolve silently.** Rate
 the row as its cells stand, then say in RUN NOTES exactly what disagreed and where
 (`NC-05: matrix CT-15 cell live, card scope: global, SKILL text listed it as local`). These
 files are revised on different cycles and drift between revisions; a rater who quietly picks
 the reading that looks right destroys the only signal that would have caught it. The CT-15
 three-way disagreement was found precisely because a rater reported it instead of choosing.
- **Overlapping evidence is allowed across cards, not within one.** One sentence may be a
 span for NC-01, NC-02, and NC-04 at once; they are distinct features. The same sentence
 may not be counted twice inside one local card's span list.
- **Prompts, titles, and blurbs are not evidence.** Rate only the numbered paragraphs.

---
