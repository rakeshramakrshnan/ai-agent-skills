# Use-case matrix (Matrix 2) and handoff interfaces — `narrative-check` / `narrative-humanize`

This file defines UC-01 … UC-12 and the handoffs between the structural, surface, and
word-level stages. Content-type behavior remains in `content_type_matrix.md`.
**Binding overrides honored:** the operating rules … . Where a decision touches a row below it is named inline.

## How to read this file

- §0 defines the shared vocabulary every row uses: the fixed pipeline order, the artifact names, the common failure behaviors, and the standing caveat texts. Rows reference these by id instead of restating them.
- §1 has the twelve use-case entries. Every entry fills the same nine fields. Wording in a `caveat_or_refusal_wording` block is paste-ready for A1 (`narrative-check` SKILL.md) and A2 (`narrative-humanize` SKILL.md).
- §2 documents the two layer interfaces. Quotes from `surface_levers.md` and `word_level_signals.md` are verbatim and name the section they come from.
- §3 is a one-page cross-reference for the README author.
- Nothing here writes a card, an intervention, or a Matrix 1 cell. Those are C1's, A2's, and C2's slices.

---

## §0 Shared definitions

### 0.1 The fixed pipeline order

Every rewrite in this system runs the same four stages in the same order. The order is fixed. No row below reverses it, and no row may.

| Stage | Skill | Layer | What it produces |
|---|---|---|---|
| **P0** | `narrative-check` | discourse-level measurement (baseline) | NARRATIVE-CHECK REPORT with a HANDOFF block |
| **P1** | `narrative-humanize` | structural rewrite | rewritten text (structure changed, surface untouched) + STRUCTURAL CHANGE LOG + `[SOURCE NEEDED]` list |
| **P2** | the surface pass | surface rewrite (final prose pass) | rewritten text only, per humanize's own hard rule 6 |
| **P3** | `narrative-check` **and** the word-level layer, re-run on the P2 output | verification | second NARRATIVE-CHECK REPORT + NC-id DIFF against P0; WORD-LEVEL REPORT with the NARRATIVE-CHECK REPORT appended (see §2.2) |

Why the surface pass is last and never earlier: the surface pass documents, in its own protocol, that re-touching text after a humanize pass leaks tells back in. Protocol step 2: "Do not stop and ask before rewriting. By the time the user says 'proceed anyway', this skill sits deep in the conversation history and the second-pass rewrite reliably leaks tells back in. Rewrite now, flag the gap after." Protocol step 4: "After fixing hits, re-scan the sentences you rewrote: regenerated prose reintroduces the same tells at the same rate as the first draft." A structural rewrite regenerates whole paragraphs. If it ran after the surface pass, every regenerated paragraph would carry a fresh crop of surface tells that nothing downstream removes. So the structural layer is closed first, and the surface pass is the last thing that touches the prose. The full reasoning is written out once, in UC-02, and referenced elsewhere.

Check-only rows (UC-01, UC-05, UC-06 check variant, UC-08, UC-09, UC-11) run P0 alone. P3 is always a fresh read of the P2 output, never a recollection of what P1 intended to change.

An honesty note that applies to P3 in every row: the model that re-checks is the model that rewrote. the surface pass names this problem for its own gate ("Hard rules" intro: "the model that wrote the draft is the model checking it"). The P3 report is therefore a self-assessment and is labelled as one in the NC-id DIFF header (§0.2).

### 0.2 Artifact glossary

| Artifact | Produced by | Definition |
|---|---|---|
| **NARRATIVE-CHECK REPORT** | `narrative-check` (P0, P3) | The fixed block defined in the fixed report layout below: content type and classification basis, word count and excluded non-prose, confidence cap, STRUCTURAL LEAN (AI-side, human-side, `Neutral (rated, neither side)`, not applicable), TOP AI-SIDE FEATURES **ranked by Table 14 core rank** (the paper's own ordering, p. 24; each line prints the rank, and the gap ratio is printed as a value, not used as the sort key), LOCATED SPANS, HUMAN-SIDE FEATURES PRESENT, PARAGRAPH LEAN MAP (CT-12 only; per-paragraph line plus per-section lines `S1 (P[a]-P[b]): AI-side n of m | human-side n | neutral n | leans …`), MODEL FINGERPRINTS (UC-05 only), HANDOFF TO narrative-humanize. Every feature line, including every LOCATED SPANS, HUMAN-SIDE, and MODEL FINGERPRINTS line, carries `validated: fiction` or `analog: unvalidated` (rule 3) and a scope with its `scope_basis` . Global features carry a document-level value plus the two or three spans that most drive it (HUMAN-SIDE evidence takes two or three spans for global cards); local features carry exact `P[n]` spans; no global feature ever gets a single line number (rule 4). |
| **HANDOFF block** | `narrative-check` | Last section of the report: one line per targeted feature, `NC-id | scope | P-locations or S-range | direction to move | CT row`. The location field accepts a section range (`S1`, `S2-S3`) on CT-12 documents where a global card differs across sections . This is the only thing `narrative-humanize` reads as its work order. |
| **STRUCTURAL CHANGE LOG** | `narrative-humanize` (P1) | One entry per HANDOFF line acted on or skipped: `NC-id | action taken | where (P[n], S-range, or "document-level") | rationale | skipped: <reason>` where applicable. The `action` and `skipped:` cells are not mutually exclusive: for a local card with spans both inside and outside a UC-07 scope, one line carries the action taken in the `action` cell and `out of scope: P<n>` in the `skipped:` cell . Entries are written *before* the surface pass runs, because the surface pass's hard rule 6 forbids it from emitting any changelog of its own. Format details beyond these five columns are A2's. |
| **`[SOURCE NEEDED]` list** | `narrative-humanize` (P1), delivered inside the STRUCTURAL CHANGE LOG | Every span annotated `[SOURCE NEEDED: <what would fit>]` during the pass, with its `P[n]`, what would fit there, and its disposition (`resolved: <source>` or `removed`). Annotations are preserved verbatim *through* the surface pass so the surface pass cannot delete the sentence carrying one (§2.1), and are then resolved or removed by `narrative-humanize`'s Step 8 delivery gate: **no `[SOURCE NEEDED` string may appear in delivered prose**, because leaving them inline measurably changed the classifier in this build's validation study (validation results Step 5, caveat 4). The list is the record; the prose is delivered clean. See UC-10, which defines two notice variants (requested / not requested) selected by whether UC-10 was invoked . |
| **Fingerprint cards** | `narrative-check`, UC-05 only | The MODEL FINGERPRINTS section of the report, one line per fired `FP-id`, `source | FP-id | Evidence: P[n] "quote" | validated: fiction`, under a Low confidence cap. Card content itself is C1's slice. |
| **Summary table** | `narrative-check`, UC-06 | One row per unit: `unit | CT row | words | confidence cap | AI-side n/m | human-side n/m | top 3 AI-side NC-ids`. The "top 3" are the first three lines of that unit's TOP AI-SIDE FEATURES section, i.e. ordered by Table 14 core rank, not by gap ratio (following ). Counts and ids only, never averaged values (see UC-06). |
| **NC-id DIFF** | `narrative-check` (P3, and UC-08) | **Header, P3 (pipeline rewrites):** `NC-id DIFF — self-assessment by the rewriting model; treat as measurement of the text, not a detector prediction`. **Header, UC-08 (user's own edits):** `NC-id DIFF — edits: user's own`; the self-assessment header is dropped for UC-08 because no skill in this pipeline edited the text . **Gate-status line (both headers):** whenever the P0 (or prior-report) text and the P3 (or revised) text sit on different sides of a length gate, the header adds `Gate status: before [n] words, [gate not fired | F-SHORT fired]; after [n] words, [gate not fired | F-SHORT fired]` so a global feature that became unratable (or ratable) is not read as having moved (; T1 observed 806 → 757 words crossing the CT-10 threshold at P3 only). Body: one line per NC-id that appeared in either report: `NC-id | scope | before value | after value | status: moved-toward-human / moved-toward-AI / unmoved / newly fired / no longer applicable / not ratable after gate | evidence after: P[n] "quote"`. For global features "value" is the document-level rating; for local features it is the span set. |
| **REGISTER LINE** | `narrative-humanize`, passed to the surface pass | The explicit register string the P2 invocation must carry (§2.1 template). Mapping from CT row to register string is in §2.1.4. |

### 0.3 Common failure behaviors (referenced by id)

| Id | Trigger | Behavior |
|---|---|---|
| **F-SHORT** | Text falls under the short-form rule of Matrix 1 row CT-10 ("Under 300 words: local features only; state that global rating is impossible"). | Rate local features only. Print `Global rating: impossible at this length` in the report header. The 300-word figure is a **skill-design choice from `content_type_matrix.md`, not a paper finding**: the findings file confirms the paper "contains no 800-word or 300-word threshold, gate, or minimum" (§E.2, checked all 30 pages). Any report that prints the threshold prints that attribution with it. |
| **F-NONPROSE** | Text contains code blocks, tables, equations, or figure captions (CT-13). | Segment; rate prose only; the report's `Excluded non-prose:` field lists what was dropped and where. |
| **F-LANG** | Text is not English, or is a translation (CT-14). | Run anyway. Every feature line tagged `language: unvalidated`; confidence capped Low. Basis: the paper never states its language; English is an inference from Books3 and the English prompt figures (findings §E.9). |
| **F-OTHER** | Text matches no CT-01 … CT-14 row (CT-15). | Local features only, everything `analog: unvalidated`, confidence Low, plus one line stating what the text was classified as and why it did not fit. |
| **F-NOTHING** | HANDOFF block is empty (no AI-side feature applicable or all already human-side). | `narrative-humanize` writes a STRUCTURAL CHANGE LOG with a single line `no structural intervention: HANDOFF empty`, makes no structural edit, and proceeds to P2 if the user asked for a rewrite. The user is told the structural layer had nothing to move. |
| **F-NOREPORT** | UC-08 invoked with no prior NARRATIVE-CHECK REPORT available. | Run P0 fresh; print `No prior report supplied; this is a baseline, not a diff`; tell the user to keep this report for the next re-check. |
| **F-NOSCOPE** | UC-07 names a scope that cannot be located (no heading or paragraph matches). | Print the segmentation actually found (`P1–P4 = opening; P5–P11 = body; …`), ask which segment is meant, and do not intervene until answered. Read-only rating of the whole document proceeds. |
| **F-NOSAMPLE** | UC-03 phrasing used but no sample attached. | Fall back to UC-02 and say so in one line: `No voice sample found; running the default pipeline. Attach a sample of your own writing to enable voice matching.` |
| **F-NOANCHOR** | User wants named references added and the text has none to work from. | UC-10 behavior: `[SOURCE NEEDED]` markers only, plus the UC-10 notice verbatim. |
| **F-DISPUTE** | User contests a rating. | UC-09 behavior: re-quote span and question; no re-score on disagreement alone. |

### 0.4 Standing caveat texts (paste-ready; referenced by id)

**CAV-TRANSFER** (printed once in every report whose CT row is not CT-01):

> Transfer gap. Every feature in this report was validated by StoryScope on fiction only: 10,272 human short stories from Books3 and five LLMs' stories from the same prompts, corpus mean 4,753 words (fn. 8, p. 3). The paper reports nothing about any other genre. Features marked `analog: unvalidated` are translations of a fiction feature to this content type; the paper offers no evidence that they carry over.

**CAV-SCOPE** (printed in every `narrative-humanize` output and in the README of both skills; operating rule 7):

> Scope note. StoryScope's only robustness test is a surface-edit test: 278 Gemini-written stories edited with LAMP, Gemini as rewriter, narrative-classifier macro-F1 95.5 before and 93.9 after (Section 4.2, p. 8). The paper argues from that result that narrative features are "far harder to 'humanize'" because changing them "requires significant structural rewrites" (p. 2), and it never runs the experiment where the structural rewrite is actually performed. This build ran a small version of it. On ten AI-written stories from the paper's own dev split, the structural pass moved mean P(human) from 0.057 to 0.607 (mean Δ +0.549), with every one of the ten deltas positive (sign test, two-sided p = 0.002) and 66 core-feature movements toward the human baseline against 13 away; **five of the ten — half — crossed the 0.5 decision boundary, and the other half did not** (validation results Step 5). Five caveats bound that number and travel with it everywhere: n = 10; fiction only; one direction (AI → human); scored by a classifier retrained on the authors' released features (dev macro-F1 0.942), not their released weights; and features assigned by Claude agents following the authors' prompts, not by Gemini 3 Flash. So the honest position is neither "unmeasured" nor "it works": the structural layer moved this classifier on ten fiction texts far more than surface editing moved the paper's, and that is all it has been shown to do. What this pipeline reports is which features moved in your text, not whether any detector's verdict will change.

**CAV-SHORT** (F-SHORT): `Global rating: impossible at this length. The short-form threshold is this skill's design choice (Matrix 1, CT-10), not a StoryScope finding; the paper's corpus averages 4,753 words (fn. 8, p. 3) and states no minimum.`

**CAV-LANG** (F-LANG): `Language: unvalidated. StoryScope does not state its corpus language; all evidence is inferred to be English. Confidence capped Low.`

**CAV-FP-TOTAL** (any place fingerprint counts appear): `Fingerprint counts per source (p. 25): Human 32, Claude 26, GPT 11, Gemini 11, DeepSeek 7, Kimi 3. No single overall total is cited: the paper's own figures (per-source sum 90; "75 fingerprint features", fn. 15, p. 5; Core+FP 101 implying 71, p. 6) do not reconcile and the paper offers no reconciliation.`

---

## §1 Use-case entries

Field order in every entry: `id`, `name`, `trigger_phrases`, `entry_point`, `required_inputs`, `pipeline`, `output_artifact`, `failure_behavior`, `caveat_or_refusal_wording`.

---

### UC-01 — Check only

- **id:** UC-01
- **name:** Check only ("does this read as AI at the structure level?")
- **trigger_phrases:**
   1. "Does this read as AI at the structure level?"
   2. "Run narrative-check on this."
   3. "Is the story structure giving this away?"
   4. "Check the narrative features, don't rewrite anything."
   5. "What does this do structurally that a human writer wouldn't?"
- **entry_point:** `narrative-check`
- **required_inputs:** The text. Optional: the content type if the user knows it (otherwise `narrative-check` classifies and prints its `Classification basis`). Nothing else. If the text is missing (the user describes a document but pastes nothing), ask for the text; do not classify a description.
- **pipeline:** P0 only. No P1, P2, or P3. the word-level layer is not run unless the user asks for it or the phrasing lands in UC-11.
- **output_artifact:** One NARRATIVE-CHECK REPORT, including the HANDOFF block (so the user can hand it to UC-02 later without re-running). Report closes with one line: `To act on these findings, ask for a rewrite; the structural pass will use the HANDOFF block above.` No rewritten text is produced.
- **failure_behavior:** F-SHORT, F-NONPROSE, F-LANG, F-OTHER as applicable. If the report has zero AI-side features applicable, STRUCTURAL LEAN says so and TOP AI-SIDE FEATURES prints `none fired`; that is a valid result, not a failure.
- **caveat_or_refusal_wording:** CAV-TRANSFER when CT ≠ CT-01. CAV-SHORT / CAV-LANG when triggered. No other caveat; a check-only report does not print CAV-SCOPE because nothing is being rewritten.

---

### UC-02 — Check then rewrite (default)

- **id:** UC-02
- **name:** Check then rewrite (default)
- **trigger_phrases:**
   1. "Make this read less like AI, structurally and stylistically."
   2. "Fix the narrative tells and then humanize it."
   3. "Run the whole pipeline on this."
   4. "Rewrite this so it doesn't have the AI story shape."
   5. "Narrative-humanize this."
   6. "It passes the AI detectors but still feels machine-written; fix it properly."
- **entry_point:** `narrative-humanize` (which runs P0 first; a user who lands in `narrative-check` and then says "now fix it" is routed here with the existing report as P0).
- **required_inputs:** The text. Optional: content type, target register (if given, UC-04 rules apply), voice sample (if given, UC-03 rules apply). Missing text: ask. If a P0 report already exists from the same session for the same text, reuse it rather than re-rating.
- **pipeline:** P0 → P1 → P2 → P3, exactly in that order. This is the default pipeline every other rewrite row inherits.

 **Why the order is never reversed.** The surface pass has to be the last thing that touches the prose, for a reason the surface pass documents about itself. Its protocol step 2 says: "Do not stop and ask before rewriting. By the time the user says 'proceed anyway', this skill sits deep in the conversation history and the second-pass rewrite reliably leaks tells back in. Rewrite now, flag the gap after." Its step 4 gate says: "After fixing hits, re-scan the sentences you rewrote: regenerated prose reintroduces the same tells at the same rate as the first draft." And step 3 warns about exactly the situation a structural rewrite creates: "Rewriting your own recent output anchors you to its phrasing, and the rewrite silently degrades into word swaps that leave the original's em dashes, negation pivots, and rhythm intact." A structural pass regenerates whole paragraphs (a moved flashback, an added subplot beat, a deleted moral). If the surface pass ran before that pass, every regenerated paragraph would arrive carrying the surface tells the surface pass had just removed, and no later stage would remove them again. Running the surface pass after `narrative-humanize` means it sees the final structure and cleans all of it once. Running it before would be a second-pass leak by construction. The order also protects the structural changes: the surface pass is told what P1 changed and to preserve it (§2.1.3), which is only possible if P1 has already happened.

- **output_artifact:** (a) the final rewritten text (P2 output); (b) STRUCTURAL CHANGE LOG; (c) `[SOURCE NEEDED]` list (may be empty); (d) NC-id DIFF, before (P0) vs after (P3), one line per targeted NC-id; (e) the P3 WORD-LEVEL REPORT with the P3 NARRATIVE-CHECK REPORT appended as a separate block (§2.2). Delivered in that order, each under its own heading, so the user can read the text first and the evidence second.
- **failure_behavior:** F-SHORT (structural pass limited to local features; global features reported as not ratable and not intervened on), F-NONPROSE, F-LANG, F-OTHER, F-NOTHING (skip straight from P0 to P2 with a one-line log), F-NOANCHOR (UC-10 rules kick in for any named-reference intervention). If P3 shows a targeted feature `unmoved`, the DIFF says so; the pipeline does not loop P1 automatically. If P3 the word-level layer still scores above its own "Human" band, at most one further the surface pass audit loop is offered, because the surface pass step 5.5 says: "Loop once only — past iteration 2 you over-edit into choppy, voiceless prose."
- **caveat_or_refusal_wording:** CAV-SCOPE printed once, immediately after the rewritten text. CAV-TRANSFER when CT ≠ CT-01. UC-10 notice if any `[SOURCE NEEDED]` marker was inserted. UC-12 refusal if the user asked for a pass guarantee.

---

### UC-03 — Rewrite with a voice sample supplied

- **id:** UC-03
- **name:** Rewrite with a voice sample supplied
- **trigger_phrases:**
   1. "Here's a sample of my writing; rewrite this to match it, structure and all."
   2. "Make this sound like me. Sample attached."
   3. "Match my voice, not a generic human voice."
   4. "Use these two essays of mine as the model when you fix this."
   5. "Rewrite in the style of the attached, including how I structure things."
- **entry_point:** `narrative-humanize`
- **required_inputs:** The text; a voice sample (one or more pieces the user wrote). Optional: content type, target register. Missing sample: F-NOSAMPLE (fall back to UC-02 with the one-line notice). If the sample is very short, proceed, and the STRUCTURAL CHANGE LOG records the sample's word count so the user can judge how much weight the register scan deserved; no minimum is imposed because neither the paper nor the surface pass states one.
- **pipeline:** P0 → P1 (with a **sample register scan** added before any intervention) → P2 (with the sample passed through so the surface pass runs its step 0) → P3.

 **Sample register scan (P1, owned by `narrative-humanize`, distinct from humanize's step 0).** Before applying any HANDOFF line, `narrative-humanize` reads the sample for the structural devices its interventions could introduce: direct reader address (NC-23), fourth-wall breaks (NC-22), explicit named references (NC-21), dialogue proportion (NC-29), time jumps and flashbacks (NC-25, NC-27), and morally mixed framing (NC-30). Any intervention that would introduce a device the sample never uses is **skipped**, and the log line reads `skipped: absent from voice sample`. The row rule from Matrix 2 is the canonical example: a sample with no reader address means the reader-address intervention (NC-23) is skipped, even though the paper reports humans address the reader more often (Table 16: 0.28 vs 0.07 ordinal mean; §4.1: 28% vs 7%; both encodings per ). The reasoning mirrors the surface pass's own step 0 rule for surface voice, which this row extends to structure: "don't just remove AI patterns — replace them with patterns from the sample. If the sample is casual, don't upgrade the vocabulary. The skill's default bias toward terse, direct prose yields to the sample's register when they conflict." Interventions that *remove* an AI-side device (deleting a stated moral, breaking a single causal track) are not gated by the scan, because removing something is not contradicting the sample's register.

 **Pass-through to humanize step 0.** The P2 invocation attaches the sample and says so on the `Voice sample:` line (§2.1.3). the surface pass then runs its "(Optional) Writer-profile distillation" step, extracting "style hypotheses across six dimensions before touching the new text" (sentence length pattern, word choice level, paragraph openers, punctuation habits, recurring phrases, transition style). `narrative-humanize` does not attempt any of those six; they are surface properties and belong to the surface pass.

- **output_artifact:** As UC-02, plus: the STRUCTURAL CHANGE LOG opens with a `Sample register scan` block listing each of the six devices above as `present in sample` / `absent from sample`, and every skipped intervention names the device.
- **failure_behavior:** F-NOSAMPLE. If the sample is in a different content type from the text (e.g. fiction sample, essay to rewrite), proceed, and print: `Voice sample is [CT-xx]; text is [CT-yy]. Structural devices are matched on presence only; register-specific analogs follow the text's CT row.` All UC-02 failure behaviors also apply.
- **caveat_or_refusal_wording:** As UC-02, plus this line at the top of the STRUCTURAL CHANGE LOG: `Structural moves were gated against your voice sample: any device your sample never uses was not introduced, even where StoryScope reports humans use it more (fiction-only evidence; Table 16, p. 26).`

---

### UC-04 — Rewrite with a target register named

- **id:** UC-04
- **name:** Rewrite with a target register named ("make it read like a Nature abstract")
- **trigger_phrases:**
   1. "Make it read like a Nature abstract."
   2. "Rewrite this as a personal essay for a literary magazine."
   3. "Turn this into something that reads like long-form journalism."
   4. "I want this to read like a grant statement, not a blog post."
   5. "Rewrite as a short story; right now it's a summary."
- **entry_point:** `narrative-humanize`
- **required_inputs:** The text; a target register or genre named clearly enough to map to one Matrix 1 row. Optional: voice sample (UC-03 rules stack on top). If the target cannot be mapped to a single CT row, ask one question naming the two candidate rows; do not guess. If the user names a venue rather than a genre ("Nature"), map the venue to its section type (abstract → CT-04) and print the mapping in the report header.
- **pipeline:** P0 → P1 → P2 → P3, with one rule change: **the CT row is set by the target register, not by the input.** P0 rates the input against the *target's* CT row, so that the before/after DIFF is comparable and the HANDOFF block already speaks the target's analog vocabulary. The report header prints `Content type: [CT-xx] (set by target register "<user's phrase>", not by input)`. P2's REGISTER LINE is derived from the target CT row (§2.1.4); for CT-04, CT-05, CT-06, and CT-11 that line is `formal academic prose`, per the project directive quoted in §2.1.2.

 **Input-row-only cards are dropped by design .** Because the input is rated against the target row, any card that is `literal` or `analog` on the input's own CT row but `n/a` on the target row is never measured in this run. That is intended: the user asked for the text to become the target genre, and a feature the target genre does not carry is not a target for intervention. It is not silent: the report prints one RUN NOTES line, `RUN NOTES: input classified as [CT-yy]; rated against target row [CT-xx]; cards applicable only on [CT-yy] and not measured here: [NC-id, NC-id, …]`. If the user wants those cards read, a UC-01 check on the input's own row supplies them.

- **output_artifact:** As UC-02. The NC-id DIFF header adds `CT row fixed at [CT-xx] for both reports`. The P0 report carries the RUN NOTES line above.
- **failure_behavior:** Target unmappable → one clarifying question, as above. Target is CT-01 (fiction) and input is non-fiction → proceed; the report notes that the input's features are being read as fiction features (`validated: fiction` applies to the *target*, but the transformation itself has no evidence base, so CAV-SCOPE is printed in full). All UC-02 failure behaviors apply.
- **caveat_or_refusal_wording:** As UC-02, plus in the report header: `Rated against the target register's content-type row ([CT-xx]). The paper's features are fiction-validated; for this row every feature is [validated: fiction | analog: unvalidated].`

---

### UC-05 — Which model wrote this?

- **id:** UC-05
- **name:** Which model wrote this?
- **trigger_phrases:**
   1. "Which model wrote this?"
   2. "Is this Claude or GPT?"
   3. "Can you tell which LLM generated this story?"
   4. "Fingerprint this for me."
   5. "Does this look like Gemini output?"
- **entry_point:** `narrative-check`
- **required_inputs:** The text. Nothing else. If the user also asks for a rewrite, UC-05 runs first as a check and the rewrite follows UC-02 rules; the fingerprint reading is never a HANDOFF input (fingerprints are not core features and `narrative-humanize` does not target them).
- **pipeline:** P0 only, with the MODEL FINGERPRINTS section populated. Fingerprint cards (C1's slice) are consulted for the `FP-<source>-<n>` ids in `nc_id_registry.md`. Which CT rows the fingerprint set applies to is Matrix 1's fingerprint-set column (C2's slice); this row follows it. Default until that column says otherwise: fingerprints are read literally on CT-01 and CT-02, and on every other CT row the section prints `Model fingerprints: not applicable to [CT-xx]; fingerprint features are fiction-only (Table 17, p. 27)` with no per-source reading.
- **output_artifact:** NARRATIVE-CHECK REPORT with MODEL FINGERPRINTS populated as fingerprint cards: `[source] | [FP-id] | Evidence: P[n] "quote"`, grouped by source, plus the verbatim caveat below at the top of that section. No single-source verdict line. The section may say which sources had the most fired cards, and must also say how many of that source's fingerprints (per-source count, p. 25) were checkable at all (FP rows with no printed option value carry `value: none printed in paper` per and can fire only on the §5 prose description).
- **failure_behavior:** F-SHORT, F-NONPROSE, F-LANG, F-OTHER as applicable (on top of the Low cap, which cannot go lower; the report says `confidence: Low (already at floor)`). If no fingerprint card fires: `No fingerprint evidence found. Absence of fingerprints is not evidence of human authorship; the human profile also has fingerprints (32, p. 25), of which only five are printed with values (Table 17, p. 27).` If the user asks about a model not in the paper's five (or a later version of one of them): `StoryScope fingerprints exist only for Gemini 3 Flash, Kimi K2.5, DeepSeek V3.2, Claude Sonnet 4.6, and GPT-5.4 (p. 3). No reading is possible for [named model].` Per, the report never attributes NC-16 (Character Introduction → external description) to Gemini specifically; it is a core all-AI feature (30% vs 52%, Table 16, p. 26).
- **caveat_or_refusal_wording (verbatim, required):**

 > **MODEL ATTRIBUTION — CONFIDENCE: LOW (capped; cannot be raised).** This is a fingerprint reading, not an identification. The features it uses are StoryScope's per-source fingerprints (Table 17, p. 27), validated on fiction only and for five specific model versions: Gemini 3 Flash, Kimi K2.5, DeepSeek V3.2, Claude Sonnet 4.6, GPT-5.4 (p. 3). Even on that fiction test set, using all 257 narrative features and a trained classifier, the paper's six-way authorship attribution reaches only 68.4 macro-F1 (Table 3, p. 8) against a 16.7% chance baseline (p. 8); the Core+Fingerprint subset reaches 63.4 (Table 3, p. 8). Gemini, DeepSeek, and Kimi are confused with one another far more than with anything else (Figure 3, p. 9); a reading that points at any of those three is weaker still. This skill has no classifier and no test set. Its reading is weaker than the paper's number, not equal to it, and the paper itself notes that "AI style is increasingly fleeting" (p. 2). Do not cite this output as evidence of which model wrote the text.

 Followed by CAV-FP-TOTAL if any per-source count is printed, and CAV-TRANSFER when CT ≠ CT-01.

---

### UC-06 — Batch: several documents or sections

- **id:** UC-06
- **name:** Batch: several documents or sections
- **trigger_phrases:**
   1. "Check all five of these stories."
   2. "Run narrative-check on each chapter separately."
   3. "Here are twelve abstracts; which ones read as AI?"
   4. "Batch these and give me a table."
   5. "Rewrite each of these three sections, one at a time."
- **entry_point:** `narrative-check` (check variant) or `narrative-humanize` (rewrite variant); the phrase decides.
- **required_inputs:** Two or more units, with unit boundaries either explicit (separate files, separators, headings the user names) or stated by the user. If boundaries are ambiguous, print the segmentation found and ask before rating; a wrong split corrupts every global rating. A **unit** is whatever the user wants rated as one document. Sections of a single document that the user wants rated as separate documents are units; sections the user wants rated *within* one document are UC-07 (partial scope) or CT-12 (mixed authorship), not UC-06.
- **pipeline:** Check variant: P0 per unit, then the summary table. Rewrite variant: P0 → P1 → P2 → P3 **per unit, completed for one unit before the next begins**, then the summary table over the P3 reports. Never P1 on all units then P2 on all units; each unit's the surface pass pass must be the last thing that touches that unit's prose.
- **output_artifact:** One NARRATIVE-CHECK REPORT per unit (each with its own CT row, word count, confidence cap, and CAV-TRANSFER where applicable), then one **summary table**: `unit | CT row | words | confidence cap | AI-side n/m | human-side n/m | top 3 AI-side NC-ids`. The "top 3" column lists the first three entries of that unit's TOP AI-SIDE FEATURES section in the order the report prints them, which is Table 14 core rank (p. 24), not gap ratio; units with fewer than three AI-side features list what they have . In the rewrite variant, each unit also gets its own rewritten text, STRUCTURAL CHANGE LOG, `[SOURCE NEEDED]` list, and NC-id DIFF.

 **No cross-unit averaging of global features.** The summary table carries counts and ids only. It never prints a mean of a global feature's value across units (no "average Thematic Explicitness 4.1 across the batch"). A global feature is a property of one whole document (rule 4; Figure 8's `[GLOBAL]` fields "require story-level analysis across the entire narrative", Fig. 8, p. 29, caption p. 30), and a mean over documents is a property of nothing the user wrote. The paper's own Table 16 means are corpus statistics over 61,608 stories (fn. 8, p. 3); a batch of five is not a corpus. If the user asks for the average anyway, print the counts and say why the average is withheld.

- **failure_behavior:** Per-unit failures (F-SHORT, F-NONPROSE, F-LANG, F-OTHER) are recorded on that unit's row and do not stop the batch. Ambiguous boundaries → ask before rating. If one unit is a duplicate or near-duplicate of another, say so and rate it anyway; do not silently dedupe.
- **caveat_or_refusal_wording:** Per unit, as UC-01 or UC-02. Above the summary table, this line verbatim: `Summary shows counts per unit only. Global features are not averaged across units: each is a property of one whole document, and StoryScope's reference means (Table 16, p. 26) are corpus-level statistics that a batch does not reproduce.`

---

### UC-07 — Partial scope ("only fix the introduction")

- **id:** UC-07
- **name:** Partial scope
- **trigger_phrases:**
   1. "Only fix the introduction; leave the rest alone."
   2. "Just work on the opening two paragraphs."
   3. "Rewrite the ending, nothing else."
   4. "Touch only section 3."
   5. "Don't change the methods; fix the discussion."
- **entry_point:** `narrative-humanize`
- **required_inputs:** The text; a scope name or paragraph range. If the scope cannot be located, F-NOSCOPE. If the user gives only the scope (pastes just the introduction), ask for the whole document, because global features cannot be rated on a fragment; if the user declines, treat the fragment as the whole document, print `Fragment rated as a whole document; global ratings describe the fragment only`, and proceed.
- **pipeline:** P0 on the **whole document** (global features are rated document-wide; local features are located everywhere, in and out of scope) → P1 **intervening only inside the named scope** → P2 on the named scope only, with the surrounding text supplied to the surface pass as read-only context so register and voice stay continuous → P3 on the whole reassembled document.

 **Global features may not move.** A HANDOFF line for a global feature (e.g. NC-13 Causal Chain Continuity, NC-17 Subplot Integration, NC-01 Thematic Explicitness) is a property of the whole document. Editing only the introduction can change the spans that drive it, but usually cannot change the rating. P1 acts on such a line only where the driving spans fall inside the scope, and the STRUCTURAL CHANGE LOG records `partial: global feature, scope-limited intervention; rating may not move`. Local features whose spans fall entirely outside the scope are listed in the log as `out of scope: not touched` with their `P[n]`. A local feature with spans both inside and outside the scope gets **one** log line: the in-scope action in the `action` cell and `out of scope: P<n>[, P<m>]` in the `skipped:` cell (; §0.2).

- **output_artifact:** Rewritten text (scope changed, remainder byte-identical to the input; the log asserts this), STRUCTURAL CHANGE LOG with the `partial` and `out of scope` markers above, `[SOURCE NEEDED]` list, NC-id DIFF over the whole document, P3 reports.
- **failure_behavior:** F-NOSCOPE. If every HANDOFF line is a global feature whose driving spans are all outside the scope, P1 makes no structural change and says so (F-NOTHING variant: `no structural intervention possible inside "[scope]"; all AI-side features are global with driving spans outside the scope`), then P2 runs on the scope for the surface layer. All UC-02 failure behaviors apply.
- **caveat_or_refusal_wording:** As UC-02, plus this line verbatim before the rewritten text: `Scope limited to "[scope]". Global features were rated on the whole document and may not move after a scope-limited edit; the NC-id DIFF marks which ones did.`

---

### UC-08 — Re-check after user's own manual edits

- **id:** UC-08
- **name:** Re-check after user's own manual edits
- **trigger_phrases:**
   1. "I edited it myself; check it again."
   2. "Here's my revision. What moved?"
   3. "Re-run narrative-check against the last report."
   4. "Did my changes fix the things you flagged?"
   5. "Compare this version to the earlier report."
- **entry_point:** `narrative-check`
- **required_inputs:** The revised text; the prior NARRATIVE-CHECK REPORT (pasted, or present earlier in the same session). Missing prior report: F-NOREPORT. If the prior report was for a different CT row than the revised text now classifies as, print both rows, rate against the prior report's CT row for comparability, and say so.
- **pipeline:** P0 on the revised text, then the NC-id DIFF against the prior report. No P1 or P2 unless the user then asks for a rewrite (which becomes UC-02 with this P0 as its baseline). Not P3, because no skill in this pipeline edited the text. **The DIFF header for UC-08 is `NC-id DIFF — edits: user's own` and nothing else; the §0.2 self-assessment header (`self-assessment by the rewriting model …`) is dropped for UC-08** . The gate-status line of §0.2 is still added when the prior text and the revised text differ in F-SHORT status .
- **output_artifact:** New NARRATIVE-CHECK REPORT, then the **NC-id DIFF** under the UC-08 header, with every NC-id that appeared in either report, each with `status: moved-toward-human / moved-toward-AI / unmoved / newly fired / no longer applicable / not ratable after gate` and the after-evidence span. For global features "moved" compares the document-level rating; for local features it compares span sets (a span removed, a span added). A closing line counts each status.
- **failure_behavior:** F-NOREPORT. If the prior report is for a visibly different text (word count differs by more than the edits could explain, or no quoted span from the prior report exists in the new text), print `Prior report does not appear to describe this text; treating this as a baseline` and skip the DIFF. F-SHORT, F-NONPROSE, F-LANG, F-OTHER as applicable.
- **caveat_or_refusal_wording:** CAV-TRANSFER when CT ≠ CT-01. On the DIFF header, verbatim: `Moved / unmoved / newly fired is a measurement of this text against the prior report's features, not a detector result. Feature movement of this kind has been measured once, on ten AI-written fiction stories rewritten by this pipeline and scored by a retrained classifier: half of them crossed the 0.5 boundary (validation results Step 5). That says nothing about your text, your edits, or any commercial detector, and StoryScope itself tested only surface edits (Section 4.2, p. 8).`

---

### UC-09 — User disputes a finding

- **id:** UC-09
- **name:** User disputes a finding
- **trigger_phrases:**
   1. "That's wrong; the story does have a subplot."
   2. "I disagree with the Thematic Explicitness rating."
   3. "NC-04 shouldn't have fired; the narrator never states a theme."
   4. "Re-score that one; you're being too harsh."
   5. "Why did you flag paragraph 6? That's not what it says."
- **entry_point:** `narrative-check`
- **required_inputs:** The disputed NC-id (or enough of the user's words to identify it) and the report it came from. If the report is not in context, ask for it. The text must be available to re-quote from.
- **pipeline:** P0 is not re-run. The skill re-opens the disputed line, re-quotes the evidence span(s) from the text, re-quotes the card's question (verbatim from Table 14 or 15 via C1's card), and states in one sentence why the span answers the question at the rated value. **It never re-scores to please the user.** A rating changes only on an error of fact, and the change is logged.
- **output_artifact:** A **DISPUTE RECORD** block (format below), plus the same report line re-printed with either `disputed: user` appended (rating unchanged) or `re-rated: <old> → <new>; reason: <error of fact>` appended (rating changed).
- **failure_behavior:** If the user's dispute identifies an error of fact (the quoted span is not in the text; the `P[n]` is wrong; a passage the rater missed answers the question differently), the feature is re-rated on the corrected evidence and the DIFF-style line records the change and reason. If the dispute is an interpretation ("I meant that ironically", "that's not really a moral"), the rating stands and the dispute is recorded. If the user asks the skill to change a rating without pointing to text, the skill declines in the words below. Repeated disputes on the same line produce the same record; the skill does not drift.
- **caveat_or_refusal_wording (verbatim):**

 > **DISPUTE RECORD — [NC-id] [feature name]**
 > Card question (verbatim from StoryScope): "[question]" ([Table 14 | Table 15], p. [24 | 25]).
 > Second printed wording (only where the paper prints two; NC-06 Reference Explicitness does: "Are intertextual gestures primarily explicit or diffuse?" Table 14 #16, p. 24; "Are intertextual gestures explicit or diffuse?" Table 15 #3, p. 25): "[second wording]" ([Table], p. [n]). The paper does not say which wording was used at assignment time; both are quoted and the rating is defended against both.
 > Evidence re-quoted: P[n] "[exact span]" [; P[m] "[exact span]"].
 > Rating stands at [value] because [one sentence linking the span to the question].
 > Your reading is recorded on this line as `disputed: user`. The rating is not changed on disagreement alone. It changes only if one of these is shown: the quoted span is not in the text; the paragraph reference is wrong; or the text contains a passage that answers the question differently and was missed. Point to it and the feature is re-rated with the correction logged.
 > For calibration: in StoryScope's own validation, two human annotators agreed with each other less (Cohen's κ = 0.74) than either agreed with the model (mean κ = 0.84; Table 7, p. 19). Disagreement on these features is expected, and is not by itself evidence that a rating is wrong or right.

 **Location-only correction, rating stands** (; used when the user shows that a `P[n]` reference or a quoted span was wrong but the corrected evidence supports the same value):

 > **DISPUTE RECORD — [NC-id] [feature name] — location corrected, rating unchanged**
 > Card question: "[question]" ([Table], p. [n])[; second wording as above].
 > Correction accepted: the report cited P[n] "[old span]"; the span is at P[m] / the span quoted was "[corrected span]". The report line is amended to `Evidence: P[m] "[corrected span]"`.
 > Rating stands at [value] because [one sentence: the corrected span answers the question at the same value]. Logged as `corrected: location only; rating unchanged`.

 **Re-rating on an error of fact** (rating changes): the report line is amended to `re-rated: [old] → [new]; reason: [span not in text | wrong P[n] | missed passage P[k] "quote"]` and the DISPUTE RECORD's "Rating stands" line is replaced by `Rating changed to [new] because [one sentence]`.

 When declining a re-score with no evidence offered: `I won't change the rating without a span. Show me the text that answers "[question]" differently and I will re-rate it and log why.`

---

### UC-10 — Input has no factual anchors and the user wants named references added

- **id:** UC-10
- **name:** No factual anchors; user wants named references added
- **trigger_phrases:**
   1. "Add some real references so it reads less generic."
   2. "Can you name specific works and authors in this?"
   3. "It says humans cite real things; make it cite real things."
   4. "Put in actual names, dates, and sources."
   5. "Fill in the specifics; I don't have them."
- **entry_point:** `narrative-humanize`
- **required_inputs:** The text. A HANDOFF line for NC-21 (Intertextual Strategy → explicit named reference) or NC-06 (Reference Explicitness → balanced mix), or the user's explicit request for references. No source material is required, because none will be invented; if the user *does* supply sources, they are used and this row's notice is not printed.
- **pipeline:** P0 → P1 → P2 → **delivery gate** → P3, with these constraints. P1 annotates the spans where a named reference would move the feature with `[SOURCE NEEDED: <what kind of reference would fit here>]`, and changes nothing else at those spans. P2 is instructed to preserve every annotation verbatim and not to delete the sentence carrying one (§2.1.3); that instruction protects the sentence during the surface pass, because the surface pass's Lever 4 table says of vague attributions "Name a specific source or drop the claim", and dropping is exactly what must not happen mid-pipeline. **The annotations do not survive to the user.** After P2 and before delivery, `narrative-humanize`'s Step 8 delivery gate walks the annotation list and either *resolves* each one (a source the user supplied is written in) or *removes* it (the sentence is restored to its pre-annotation wording, or cut if it carried nothing), then confirms by search that the string `[SOURCE NEEDED` does not occur in the delivered text. This is measured, not stylistic: annotations stayed inline in 7 of the 10 validation rewrites, and stripping them changed the classifier score on all seven — reversing one flip entirely (0.822 → 0.092) and raising another (0.021 → 0.333) (validation results Step 5, caveat 4). P3 therefore runs on marker-free prose and verifies the absence, not the presence, of the string.
- **output_artifact:** Rewritten text with **no annotation in it**; the `[SOURCE NEEDED]` list carried in the STRUCTURAL CHANGE LOG (`P[n] | what would fit there | which NC-id it serves | resolved: <source> | removed`), which is where the record lives now that the prose is delivered clean; NC-id DIFF (NC-21/NC-06 show `unmoved` when the annotation was removed, and `moved` only when a real source was written in — never on the strength of an annotation); P3 reports.
- **failure_behavior:** F-NOANCHOR is this row's normal condition, not its failure. Failures are: any `[SOURCE NEEDED` string present in the delivered text (the delivery gate did not run; strip and re-verify before delivering), an annotation on the list with neither a `resolved:` nor a `removed` disposition, or the user asking the skill to "just make something plausible" (refuse in the words below; the list still travels, the prose stays clean). If the user later supplies real sources, the listed spans are revised on request or by a fresh UC-08 re-check of the user's own edit; the skill does not fill them from memory.
- **caveat_or_refusal_wording (verbatim, required; two variants, ).** `narrative-humanize` Step 8 selects the variant by one test: **was UC-10 invoked** (the user asked for references to be added, or the input had no factual anchors and the user asked for specifics)? If yes, print variant (a). If markers were placed for any other reason (a NC-21 or NC-06 HANDOFF line fired on a text the user did not ask to have references added to, whether or not the input already carries citations), print variant (b). Never print both; never print (a) when the user did not ask for references.

 **Variant (a) — `SOURCE-NEEDED-REQUESTED` (UC-10 invoked):**

 > **[SOURCE NEEDED] NOTICE.** You asked for named references, specific works, people, places, dates, or figures to be added, and the input contains none to work from. This pipeline does not invent them. During the structural pass, each place where a real reference would move the text toward the human-side pattern StoryScope reports (NC-21 Intertextual Strategy → explicit named reference, 47% human vs 24% AI; NC-06 Reference Explicitness → balanced mix, 37% vs 16%; both Table 16, p. 26; fiction-only evidence, `analog: unvalidated` outside fiction) was annotated in the working draft. **None of those annotations is in the text above.** Each was either resolved with a source you supplied or removed before delivery, because leaving them inline measurably changes how the text scores: in this build's validation study, stripping them reversed one story's result entirely (0.822 → 0.092) and raised another's (0.021 → 0.333) (validation results Step 5). The full record is in the change log below: every span, what would fit there, and whether it was resolved or removed. Fabricating a citation, a name, a statistic, or a quotation is forbidden here. To move these features for real, give me a verifiable source for a listed span and ask for that passage to be revised; until then the report shows NC-21 and NC-06 as `unmoved`, which is the honest result.

 **Variant (b) — `SOURCE-NEEDED-UNREQUESTED` (UC-10 not invoked; annotations placed by a NC-21 / NC-06 intervention):**

 > **[SOURCE NEEDED] record.** You did not ask for references to be added; this is a by-product of the features that fired. The structural pass annotated [n] span(s) where a vague attribution would need a named source to move toward the human-side pattern StoryScope reports (NC-21 / NC-06, Table 16, p. 26; fiction-only evidence). No source was invented, and **no annotation remains in the text above** — each was resolved or removed before delivery, because leaving them inline measurably changes how the text scores (validation results Step 5). The change log below lists each span and what would fit there, so you can supply a source if you want those features actually moved.

 When the user asks for plausible invented specifics (either variant): `No. A plausible-looking source that does not exist is a fabrication, and this pipeline does not produce them. The change log shows you exactly where a real one would go.`

---

### UC-11 — Input is already clean on the word-level layer but user suspects it still reads as AI

- **id:** UC-11
- **name:** Clean on the word-level layer, still suspected AI. **This is the primary motivating case for `narrative-check`, and the README must say so.**
- **trigger_phrases:**
   1. "The word-level check says Human but this still feels like a machine wrote it. Why?"
   2. "It passed the surface checks. What's left?"
   3. "The prose is fine; the shape is off. Can you tell me what's wrong?"
   4. "I already humanized this and it still reads generated."
   5. "Everything scores clean and it still smells like AI."
- **entry_point:** `narrative-check`
- **required_inputs:** The text; ideally the existing WORD-LEVEL REPORT (if absent, `narrative-check` runs the word-level layer first so the two blocks can be presented together; this is a precondition check, not a change to the fixed pipeline order, which begins at P0 for any rewrite).
- **pipeline:** P0. If the user then asks for a fix, UC-02 from this P0. The P0 report is appended to the WORD-LEVEL REPORT as a separate block (§2.2), so the user sees the surface verdict and the discourse-level verdict side by side without either being merged into the other.
- **output_artifact:** WORD-LEVEL REPORT (existing or freshly run) followed by the NARRATIVE-CHECK REPORT as a separate block, followed by the **complementarity line** (§2.2.3) stating which layer each report covers.
- **failure_behavior:** F-SHORT, F-NONPROSE, F-LANG, F-OTHER as applicable. If `narrative-check` also finds nothing (`none fired`), the honest output is: `Neither the surface signals (the word-level layer A–I) nor the discourse-level features (StoryScope core set) fire on this text. This pipeline has no third layer. If it still reads as AI to you, the tell is outside what either skill measures.`
- **caveat_or_refusal_wording:** CAV-TRANSFER when CT ≠ CT-01. Plus this line under the complementarity statement: `A clean word-level score is evidence about surface and rhetorical signals only. StoryScope's narrative features separated human from AI fiction at 93.2 macro-F1 with every style feature excluded (Table 2, p. 6), which is why a surface-clean text can still fire here. The paper did not test the specific case of a text that a surface tool has passed; that correspondence is this skill's design premise, not a paper result.`

 **What the README must say about UC-11 (paste-ready):**

 > **Primary use case.** Run `narrative-check` on text that the word-level layer has already scored Human or Likely Human but that still reads as machine-made. the word-level layer grades nine surface and rhetorical signals: perplexity, burstiness deficit, hedge density, structural tells, specificity deficit, transition-word fingerprint, punctuation fingerprint, voice and register, and rhetorical scaffolding. None of them measures what a text does at the discourse level: whether it states its theme outright, resolves everything on a single causal track, never names a real work or author, never addresses its reader, tells its story in strict order. StoryScope (Russell et al., COLM 2026) found that features of that kind separate human from AI fiction at 93.2 macro-F1 with every style feature removed (Table 2, p. 6), and that a surface-edit pass with LAMP moved its narrative detector only from 95.5 to 93.9 macro-F1 (Section 4.2, p. 8). That is the layer `narrative-check` covers and the word-level layer does not. Its evidence is fiction-only (10,272 human stories, five LLMs, mean 4,753 words; fn. 8, p. 3); on any other content type every feature is tagged `analog: unvalidated`, and the report shows the tag.

---

### UC-12 — User asks for a guarantee of passing a detector

- **id:** UC-12
- **name:** Guarantee of passing a detector
- **trigger_phrases:**
   1. "Make sure this passes GPTZero."
   2. "Guarantee it won't get flagged."
   3. "I need this to come back 0% AI."
   4. "Will this beat Pangram after you're done?"
   5. "Rewrite it so no detector can tell."
- **entry_point:** Whichever skill the rest of the phrase lands in; almost always `narrative-humanize`. The refusal wording is identical in both skills.
- **required_inputs:** As for the underlying use case (usually UC-02). The guarantee request adds no input requirement and removes none.
- **pipeline:** The underlying use case's pipeline, unchanged and in the fixed order. The refusal is emitted **before** P0 begins and the work proceeds immediately after it; the user is not asked to confirm. (The rationale is the surface pass's own: stopping to ask and then resuming leaks tells back in, protocol step 2.)
- **output_artifact:** The refusal block, then the underlying use case's artifacts in full. The P3 NC-id DIFF and WORD-LEVEL REPORT are presented as measurements of the text; no line anywhere predicts a detector's verdict.
- **failure_behavior:** If the user re-asks for the guarantee after the refusal, repeat the refusal's first two sentences and continue. If the user names a specific detector, the word-level layer's "Reference detector landscape" section may be quoted for context, and the refusal stands. If the user makes the guarantee a condition of proceeding ("only do it if you can promise"), state that the work will be done without the promise and do it. All failure behaviors of the underlying use case apply.
- **caveat_or_refusal_wording (verbatim, required):**

 > **NO GUARANTEE.** I will do the structural and surface rewrite, but I cannot promise that the result passes any detector, and I will not say that it does. There is now a measurement, and it is small enough that you should hear it in full. On ten AI-written stories from StoryScope's own dev split, this pipeline's structural pass moved mean P(human) from 0.057 to 0.607 (mean Δ +0.549), every one of the ten deltas was positive (sign test, two-sided p = 0.002), and **five of the ten — half — crossed the 0.5 decision boundary. The other half did not.** That is one benchmark of ten fiction stories, in one direction (AI → human), scored by a classifier we retrained on the authors' released features (dev macro-F1 0.942) rather than their released weights, with features assigned by Claude agents following the authors' prompts rather than by Gemini 3 Flash (validation results Step 5). It is not a result about your text, about non-fiction, or about any commercial detector, none of which were tested. For contrast, the paper's own robustness test is a surface-edit test: 278 Gemini-written stories edited with LAMP, its narrative classifier moving from 95.5 to 93.9 macro-F1 (Section 4.2, p. 8); it asserts that narrative features are "far harder to 'humanize'" because changing them "requires significant structural rewrites" (p. 2) and never performs that rewrite. So the structural layer does more than surface editing did, on that one benchmark, half the time. The surface pass, the surface pass, disclaims its own layer in its scope section: it does not "Guarantee 0% AI scores on commercial detectors (no method does reliably)." What you will get is a before/after NARRATIVE-CHECK REPORT and an WORD-LEVEL REPORT showing which features moved. That is a measurement of the text, not a prediction about a detector. Proceeding with the rewrite now.

---

## §2 Handoff interfaces

### 2.1 Into the surface pass (P2)

File: `surface_levers.md`. The structural and surface stages remain distinct.

#### 2.1.1 Which protocol steps run

**All of them, 0 through 8**, as the spec requires. `surface_levers.md` names them in its "Rewrite protocol" section:

- Step 0, "(Optional) Writer-profile distillation", runs **only** when a voice sample is attached (UC-03, or UC-02/UC-04 with a sample). It extracts "style hypotheses across six dimensions before touching the new text": sentence length pattern, word choice level, paragraph openers, punctuation habits, recurring phrases / verbal tics, transition style.
- Step 1, "Read the full input first. Identify topic domain, audience, register, length target." The REGISTER LINE (§2.1.4) pre-answers the register question so the surface pass does not have to infer it.
- Step 2, "Inventory the AI tells" including the anchor count. On the zero-anchor path the surface pass appends its own note ("*[Note: the input had no factual anchors … ]*"). Under UC-10 both notes will appear: the surface pass's anchor note and this pipeline's `[SOURCE NEEDED]` notice. That is intended; they say different things (one asks for specifics, the other forbids inventing them).
- Step 3, "Rewrite in a single pass applying all nine levers." The step's warning applies with full force here, because the P1 output is text the model wrote moments earlier: "Treat your own prior output as foreign text: extract what it says, re-derive the prose from the content. If your edit log would read as a list of substitutions, you light-edited. Start over."
- Steps 4 and 5, the "Pre-output gate" and "Self-check", with their written counts.
- Step 5.5, "Audit pass", the Signal I checklist and one rewrite-and-recheck loop. "Loop once only."
- Step 5.6, "Output-length sanity check."
- Steps 6 and 7, optional high-stakes checks; `narrative-humanize` does not request them by default and passes the request through if the user asked.
- Step 8, "Output the rewritten text only."

#### 2.1.2 The register must be stated explicitly

The surface levers gate two of their own rules on the register being *named*. Hard rule 2: "**Semicolons:** none, unless a list item itself contains commas or the register is explicitly formal/academic (Lever 8)." Lever 8: "Treat every semicolon as a bug unless the register is explicitly formal/academic." This matters most on the academic CT rows: the carve-out fires only when the register is stated, and without it the default banned-vocabulary list strips conventions that are correct, and sometimes required, in that register. The REGISTER LINE in the instruction template is therefore mandatory and **never inferred**, and for CT-04, CT-05, CT-06, and CT-11 it reads exactly `formal academic prose`.

#### 2.1.3 The exact instruction `narrative-humanize` works to (template)

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

The fact-ledger line exists because the surface pass's step 4 gate scans for "Comparative framing" ("more ... than") and its hard rule 5 bans "more X than Y" as a rhetorical pivot; a claim in `narrative-humanize`'s fact ledger whose *content* is a comparison (a result that is larger, faster, or more frequent than another) must keep that comparison, and only its phrasing may change . The optional second-person line exists because the surface pass's Lever 6 lists "Occasional second-person direct address" and "Mild rhetorical questions as transitions" as human-direction moves; on a CT row where the reader-address analog is `n/a`, or under UC-03 where the sample lacks it, that lever would reintroduce a structural device P1 deliberately did not add. The REGISTER LINE handles most of this (the surface pass calibrates by domain), and the explicit line closes the gap.

#### 2.1.4 CT row → REGISTER LINE (design proposal; C2 and A2 may refine)

| CT row | REGISTER LINE | the surface pass calibration section that then applies |
|---|---|---|
| CT-01 | `literary short fiction` | "Creative / lyrical prose (fiction, poetic passages, mood pieces)" |
| CT-02 | `narrative nonfiction, personal essay` | "Narrative / blog / essay" |
| CT-03 | `opinion essay / blog` | "Narrative / blog / essay" |
| CT-04, CT-05, CT-06 | `formal academic prose` | hard rule 2 and Lever 8 semicolon carve-out; the word-level layer's "Academic or legal writing legitimately uses hedges and semicolons" note applies at P3 |
| CT-07 | `technical documentation` | "Technical (engineering, code, systems)" |
| CT-08 | `journalism / news feature` | "Narrative / blog / essay" (closest; humanize has no journalism section) |
| CT-09 | `marketing / product copy` | "Professional / business" |
| CT-10 | one of `email`, `Slack / async update`, `social post`, `cover letter` | "Professional / business" or "Slack / async team updates" |
| CT-11 | `formal academic prose` (application register) | as CT-04 |
| CT-12 | register of the dominant human-authored portion, or as the user states | as mapped |
| CT-13 | register of the prose portion; code blocks and tables are passed as read-only context | as mapped |
| CT-14 | `<language>, <register>` | none: the surface pass's text does not address non-English input, so its behavior there is unverified (worklog) |
| CT-15 | as the user states, else `general expository prose` | none specific |

#### 2.1.5 What the surface pass must NOT be asked to do

- **Any structural work.** Adding or removing subplots, reordering chronology, inserting flashbacks or reader asides, removing a stated moral, changing how a resolution happens, introducing a named reference. That is `narrative-humanize`'s layer and it is closed before P2 begins. the surface pass describes itself as changing "only the expression": "What this skill does NOT do: … Change the factual content of the input, only the expression."
- **Rate or report on narrative features.** the surface pass has no NC-id vocabulary and produces no report.
- **Emit a changelog.** Its hard rule 6: "Output shape: the rewritten text only. No preamble … no trailing changelog … The ONLY permitted additions are the two meta-notes mandated by protocol steps 2 and 5.6." The STRUCTURAL CHANGE LOG is written by P1 before P2 runs.
- **Fill a `[SOURCE NEEDED]` marker or add specifics of its own.** Its scope section: it does not "Add false information to increase specificity — plausible framing only." Its Lever 5 "plausible-specificity frames" are permitted; a named source, number, or date that P1 did not supply is not.
- **Loop more than once.** Step 5.5: "Loop once only — past iteration 2 you over-edit into choppy, voiceless prose." If P3 still flags surface signals, one further the surface pass loop may be offered, never two.
- **Run before `narrative-humanize`.** See §0.1 and UC-02.

### 2.2 Into the word-level layer (P3, and UC-11)

File: `word_level_signals.md`.

#### 2.2.1 Append, never merge

The NARRATIVE-CHECK REPORT is placed **after** the WORD-LEVEL REPORT as a separate block with its own header. It is never folded into the word-level layer's scoring. the word-level layer's score is defined by its own text as "9 categories × 3 = 27 maximum" ("Total score cap") with verdict bands "0–4 Human, 5–8 Likely Human, 9–13 Uncertain, 14–19 Likely AI, 20–27 AI" ("Scoring thresholds"). No narrative feature adds to that number, no narrative finding changes the word-level layer's VERDICT or CONFIDENCE line, and no combined verdict is printed. The two reports have independent confidence lines: the word-level layer's "CONFIDENCE: [Low | Medium | High]" and `narrative-check`'s "Confidence cap: [High | Medium | Low] (reason)". The layout is:

```
WORD-LEVEL REPORT
===============
… (the word-level layer's exact output format, unchanged) …

NARRATIVE-CHECK REPORT
======================
… (A1's exact output format, unchanged) …

COMPLEMENTARITY
---------------
<the one-paragraph statement in §2.2.3>
```

#### 2.2.2 the word-level layer's signal categories, by name

the word-level layer scores "nine signal categories", each 0–3 (section "The nine signal categories"):

- **Signal A: Perplexity** (word predictability)
- **Signal B: Burstiness deficit** (sentence uniformity)
- **Signal C: Hedge density**
- **Signal D: Structural tells** — document architecture: "Bullet list where prose would serve better", "Topic sentence + evidence + restatement", "Tricolon parallel structure", "Perfect paragraph-per-idea arc", "Three-act Slack/update structure", "Strawman pivot". These are paragraph- and document-architecture patterns of *exposition*, not discourse-level narrative choices; Signal D and `narrative-check` are designed not to overlap in what they measure even though both use the word "structure"(the non-overlap is a design intent, not a tested result).
- **Signal E: Specificity deficit** — "Abstract claim with no number, name, time reference, or example". Related to but distinct from NC-21 / NC-06: Signal E asks whether claims are anchored; NC-21 asks what kind of intertextual engagement the text uses (Table 15 #1, p. 25). A text can pass E (numbers and dates present) and still fire NC-21's AI-side reading (no named works or authors).
- **Signal F: Transition word fingerprint**
- **Signal G: Punctuation fingerprint**
- **Signal H: Voice and register**
- **Signal I: Rhetorical scaffolding** — sentence- and paragraph-level construction patterns (aphoristic closers, thesis-first openers, chiasmus, and the rest of the word-level layer's checklist). A sentence that states the text's lesson can fire Signal I (as a "Mini-aphorism paragraph closer" or "Aphoristic / chiasmus closer") and also be the evidence span for NC-01 Thematic Explicitness & Moralizing or NC-04 Narratorial Thematic Commentary. Shared evidence spans between Signal I and NC-01/NC-04 are expected and are not double-counting: the same sentence is being measured for two different properties (its rhetorical shape by the word-level layer, its discourse function by `narrative-check`), in two reports whose scores are never combined .

Plus the **"Mixed-authorship overlay"**, which estimates the "AI-edited fraction" in five buckets ("Pure human (~0%)" … "Pure AI (~100%)"). `narrative-check`'s PARAGRAPH LEAN MAP (CT-12 only) is the discourse-level analog of that overlay and, like it, is reported as a separate line, never combined with it.

the word-level layer's "Calibration notes" that P3 must honor rather than override: "Short texts (<100 words) have fewer signals available; note this and adjust confidence to Medium max"; "Academic or legal writing legitimately uses hedges and semicolons — adjust Signal C and G accordingly"; "A text can score AI on structure/transitions but human on voice — report both honestly"; and under "Known detection ceilings", "Refuse High confidence on non-English text unless calibration is known" (which lines up with F-LANG).

#### 2.2.3 Complementarity statement (paste-ready for the README and for the block above)

> the word-level and discourse-level layers measure different layers and their results are reported side by side, never combined. the word-level layer grades nine surface and rhetorical signals (A perplexity, B burstiness, C hedge density, D structural tells, E specificity, F transitions, G punctuation, H voice and register, I rhetorical scaffolding) on a 0–27 scale. `narrative-check` rates discourse-level narrative features from StoryScope's 30-feature core set (Table 16, p. 26): what the text does with theme, causality, subplots, resolution, time order, sensory and emotional rendering, named references, and reader address. A clean the word-level layer result says nothing about the narrative layer, and a clean `narrative-check` result says nothing about the surface. StoryScope's own comparison is the reason both are needed: its narrative features detected AI fiction at 93.2 macro-F1 with all style features excluded, its style-only features at 85.8, and both together at 96.0 (Table 2, p. 6). Fiction-only evidence; outside fiction every `narrative-check` feature is `analog: unvalidated`.

---
