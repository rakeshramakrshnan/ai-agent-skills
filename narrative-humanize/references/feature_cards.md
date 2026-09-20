# Feature cards — the rating units for `narrative-check` and the targets for `narrative-humanize`

**Cards in this file: 68** — 30 core (NC-01 … NC-30, Table 16), 10 extended (NC-31 … NC-40, measured but not in Table 16), 28 fingerprint (FP-<source>-<n>, Table 17).
Values and questions come from the StoryScope paper and its released taxonomy. Names and ids
come from `nc_id_registry.md`. Extended-card measurements are included directly in each card.

## How to read a card

- `dimension` uses the NarraBench codes as the feature tables print them: SIT = Situatedness, PLT = plot, REV = revelation, TMP = temporal structure, AGENT, SET = setting, EVT = events, PER = perspective, SOC = social network (findings §A.2). "Structure" is never used as a dimension name .
- `question` is verbatim from Table 14 (p. 24) or Table 15 (p. 25) as reproduced in findings §B. Every core feature has a printed question; none needed the `not given in paper` marker (findings §B.8). `taxonomy_question` beside it is the same feature's question as the authors actually shipped it in `taxonomy.json`; the two wordings differ on most cards, and the taxonomy one is what their assigner answered.
- `taxonomy_id` gives the feature's id in the released taxonomy, so any card can be checked against `taxonomy.json` and the released per-story data.
- `tier` is `core (Table 16, p. 26)` for NC-01 … NC-30 and `extended (measured, not in Table 16)` for NC-31 … NC-40. **The extended cards are not paper core features and must never be described as such** — they are taxonomy features whose human/AI separation and classifier weight the validation study measured.
- `values (exact, taxonomy.json)` is the feature's exact option vocabulary as shipped. It supersedes the paper's abbreviated option names, which are retained on the same field as `printed_option_names (paper)` because Table 16's rows are labelled with them. Several cards gained options this way: NC-18 has **five** values, not the three the paper's prose implies; NC-30 has **four**; NC-14, NC-19, NC-28 and NC-29 have named middle rungs; NC-21 has **no absence value**, so a story with no intertextual engagement is written `[]`.
- `importance_rank` is the card's rank among the 30 core cards by measured classifier importance (`feature_cards.md` Part A), from our retrain on the authors' released features (macro-F1 0.942, dev held out). It is the ordering basis for anything that has to prioritise cards; the paper's own Table 14 core rank is a different quantity and is still quoted inside `fiction_detection` where relevant.
- `measured_baseline` records what the released per-story data gives for the card, either confirming the printed Table 16 values or showing where they differ in the last digit. ** is settled: the nine printed gaps that differ from Human − AI by one last-digit unit are rounding from unrounded means, not errors.** Printed gaps are still what `option_rows` carry, never recomputed.
- `rating_note (measured)` appears on the eleven cards where the fidelity study found a systematic rating error, and states the correction. Cards without one were not measured to be biased.
- `detection_method (authors', verbatim)` is the locating instruction the authors wrote for the feature in `taxonomy.json`. **Caveat that must travel with it:** their `apply_features.py` never passes `detection_method` to the assigner, so it documents what the feature means and what a reader should look at — not how the assigner actually located it.
- `option_rows` reproduce Table 16 (p. 26) exactly. `gap` is the **printed** gap; nine printed gaps differ from Human − AI by one last-digit unit and are kept as printed, never recomputed (; E1 worklog item 6). Negative gap = AI-elevated; positive = Human-elevated (Table 16 caption, findings §B).
- `direction` is taken from the Table 16 group header the row sits under, not from the sign of the gap (findings §B).
- **`scope`.** The paper does not tag Table 14–17 features as global or local. The released taxonomy supplies the basis: the authors' own `detection_method` says what a rater must read to answer the feature. All 40 core and extended cards now carry:
  - `scope_basis: detection_method` — the authors' text decides it: an instruction to locate a passage ("Locate the passage where the central figure first shows up", "Examine the first scene") gives `local`; an instruction that ranges over the text ("the dominant practice across the text", "select all modalities that appear regularly", "estimate the proportion of sentences") gives `global`. The reasoning, quoting that text, is on each card. **This is still the authors' description of the feature, not a scope tag they published** — and per the caveat above, their pipeline never showed the assigner this text.
  - `scope_basis: figure-8` / `inferred` survive only on the 28 fingerprint cards, which have no entry in `feature_cards.md`.
  - Operating rule for the report (rule 4): a `global` card gets one document-level rating plus the two or three spans that most drive it; a `local` card gets exact span locations for every firing instance. No `global` card may be pinned to a single line number.
- `validated_on: fiction` on every card. Both evidence sources are fiction only: 10,272 prompts, a 61,608-story corpus averaging 4,753 words (fn. 8, p. 3), five LLMs, human stories from Books3 anthologies (findings §A.5, §E.1); the release covers the same corpus (61,575 assigned stories), and the validation study rated and rewrote fiction only, n = 10. Nothing on any card is evidence about any other genre.
- `fiction_detection` is how a reader rates the card on narrative text. Where §4.1 prose (findings §D) speaks to the feature its numbers are quoted; where §D is silent the card says so and relies on the question wording and Table 16 values only.
- `content_type_behavior` on every core card points to the card's own row in `content_type_matrix.md` (section CT-xx), which defines one cell per content type — literal / analog / n/a. Non-fiction behavior is defined there, never here.
- Fingerprint cards carry `attribution_ceiling` verbatim on every card: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low. Fingerprints are properties of five specific model versions at a specific time (findings §E.12).

---

## Part 1 — Core cards (NC-01 … NC-30)

### Group: AI-elevated — Thematic over-determination (Table 16, p. 26)

```
id: NC-01
name: Thematic Explicitness & Moralizing
taxonomy_id: `SIT_MET_303` — "Thematic Explicitness and Moralizing" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 1 of 30 measured (importance 0.0417, 4.2% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Thematic Explicitness & Moralizing" (s); Table 14 "Thematic Explicitness and Moralizing" (findings §B.1)
dimension: SIT (Situatedness)
response_type: scale (1–5 Likert)
question: "How explicitly does the story articulate its themes or morals?" (Table 14 #1, p. 24; also Table 1, p. 4)
taxonomy_question: "How explicitly does the story articulate its themes or morals in the narration or character speech?" (taxonomy.json)
values (exact, taxonomy.json): 1; 2; 3; 4; 5
 printed_option_names (paper): "1–5" (Table 14 #1, p. 24)
option_rows:
  - option: scale mean | human: 3.28 | ai: 3.94 | gap (printed): −0.65 | direction: AI-elevated
human_mean: 3.28
ai_mean: 3.94
direction: AI-elevated
measured_baseline: matches the printed Table 16 values (human 3.28 / AI 3.94)
detection_method (authors', verbatim): Rate 1 when themes remain implicit and never spelled out; 3 when characters or narrator occasionally state generalizations about life or the story's issues; 5 when the story includes clear thesis-like statements or overt morals summarizing how readers should interpret events.
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): story.plot.themes [GLOBAL]; story.plot.moral [GLOBAL] ("one sentence if signaled; else null") (findings §F.2)
scope_reasoning: The authors' detection_method rates the whole story ("themes remain implicit" … "the story includes clear thesis-like statements"), not a passage. Global.
validated_on: fiction
fiction_detection: Read for how directly the text tells the reader what it means. At the low end (1) themes stay implicit in events and images and are never named; at the high end (5) the text states what the story is about or what lesson it carries, often more than once. §4.1: "AI stories are more explicit and moralizing (roughly 20% higher on a 1-5 scale)" and "AI spells out meaning rather than trusting the reader to infer it" (p. 7; findings §D.1). The AI side is the high end of the scale: AI mean 3.94 against human 3.28 (Table 16, p. 26). Rank 1 of the 20 AI-core features by core score (Table 14, p. 24).
rating_note (measured): measured bias **+0.60** (6/10 stories rated above the authors' assignment; validation fidelity study, `feature_cards.md` §2). Anchor correction: prose that moralizes moderately is a **3**, not a 4 — the authors reserve 5 for "clear thesis-like statements or overt morals summarizing how readers should interpret events", and 3 is "characters or narrator occasionally state generalizations about life".
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: a sentence in which the narrator names the theme outright; a closing paragraph that states the lesson; a character speech that summarizes what the events "mean".
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-01 (one cell per content type: literal / analog / n/a)
```

```
id: NC-02
name: Moral / Philosophical Weighting
taxonomy_id: `SIT_GEN_010` — "Moral / Philosophical Weighting" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 29 of 30 measured (importance 0.0006, 0.1% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Moral / Philosophical Weighting" (s); Table 14 same (findings §B.1)
dimension: SIT (Situatedness)
response_type: scale (1–5 Likert)
question: "How heavily does the story foreground moral or philosophical questions?" (Table 14 #15, p. 24)
taxonomy_question: "Relative to entertainment, how important are moral or philosophical arguments in the story's overall design?" (taxonomy.json)
values (exact, taxonomy.json): 1; 2; 3; 4; 5
 printed_option_names (paper): "1–5" (Table 14 #15, p. 24)
option_rows:
  - option: scale mean | human: 3.26 | ai: 3.68 | gap (printed): −0.42 | direction: AI-elevated
human_mean: 3.26
ai_mean: 3.68
direction: AI-elevated
measured_baseline: matches the printed Table 16 values (human 3.26 / AI 3.68)
detection_method (authors', verbatim): On a 1–5 scale, judge how far the text foregrounds explicit ethical debates, thematic exposition, or philosophical reflection versus prioritizing suspense, adventure, or sensory pleasure. Consider narrator commentary and climactic speeches.
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): story.plot.moral [GLOBAL]; story.plot.themes [GLOBAL] (findings §F.2)
scope_reasoning: The detection_method judges "how far the text foregrounds" ethical debate against suspense or sensory pleasure, considering narrator commentary and climactic speeches — a whole-text weighting. Global.
validated_on: fiction
fiction_detection: Read for how much of the text's attention goes to questions of right, wrong, meaning, or existence, as opposed to incident, relationship, or setting. Low end (1): the story poses no moral or philosophical question; high end (5): such questions are central and repeatedly foregrounded. §4.1 groups this with NC-01 and NC-03 as "more central moral questions" (p. 7; findings §D.1). The AI side is the high end: AI mean 3.68 against human 3.26 (Table 16, p. 26).
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: a passage that frames the protagonist's situation as an ethical dilemma; dialogue that debates a question of value; a narratorial reflection on meaning or justice.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-02 (one cell per content type: literal / analog / n/a)
```

```
id: NC-03
name: Thematic Unity
taxonomy_id: `PLT_THM_008` — "Thematic Unity" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 12 of 30 measured (importance 0.0076, 0.8% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Thematic Unity" (s); Table 14 same (findings §B.1)
dimension: PLT (plot)
response_type: scale (1–5 Likert)
question: "To what extent do subplots and flourishes serve a central thematic concern?" (Table 14 #3, p. 24)
taxonomy_question: "To what extent do subplots, set-pieces, and stylistic flourishes serve a central thematic concern?" (taxonomy.json)
values (exact, taxonomy.json): 1; 2; 3; 4; 5
 printed_option_names (paper): "1–5" (Table 14 #3, p. 24)
option_rows:
  - option: scale mean | human: 4.41 | ai: 4.74 | gap (printed): −0.33 | direction: AI-elevated
human_mean: 4.41
ai_mean: 4.74
direction: AI-elevated
measured_baseline: matches the printed Table 16 values (human 4.41 / AI 4.74)
detection_method (authors', verbatim): Rate 1 if many sequences feel ornamental or unrelated to any central idea; 5 if nearly every scene, subplot, and image clearly reinforces or complicates the same thematic core.
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): story.plot.themes [GLOBAL]; story.plot.summary [GLOBAL]; story.plot.plot_arc [GLOBAL] (findings §F.2)
scope_reasoning: The detection_method weighs whether "nearly every scene, subplot, and image" reinforces one thematic core — a whole-text judgment. Global.
validated_on: fiction
fiction_detection: Read for whether digressions, secondary threads, and ornamental passages tie back to one concern. Low end (1): subplots and flourishes wander and are not reconciled with a central theme; high end (5): everything converges on a single concern. §4.1 reports "tighter thematic unity" for AI (p. 7; findings §D.1). The AI side is the high end: AI mean 4.74 against human 4.41 (Table 16, p. 26); note both means sit in the upper part of the 1–5 range, so the distinguishing region is narrow.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: a subplot or digression and the point where it is (or is not) tied back to the main concern; an ending that gathers every thread to one meaning.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-03 (one cell per content type: literal / analog / n/a)
```

```
id: NC-04
name: Narratorial Thematic Commentary
taxonomy_id: `SIT_MET_501` — "Narratorial Thematic Commentary Presence" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 23 of 30 measured (importance 0.0015, 0.2% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Narratorial Thematic Commentary → yes"; Table 14 same (findings §B.1)
dimension: SIT (Situatedness)
response_type: binary
question: "Does the narrator explicitly comment on themes beyond characters' perspectives?" (Table 14 #10, p. 24; also Table 1, p. 4)
taxonomy_question: "Does the narrator explicitly comment on the story's themes, lessons, or the meaning of events beyond characters' situated perspectives?" (taxonomy.json)
values (exact, taxonomy.json): no; yes
 printed_option_names (paper): "no, yes" (Table 14 #10, p. 24)
option_rows:
  - option: → yes | human: 52% | ai: 77% | gap (printed): −25 | direction: AI-elevated
human_mean: 52% (prevalence of "yes")
ai_mean: 77% (prevalence of "yes")
direction: AI-elevated
measured_baseline: human 51% / AI 76% yes (`feature_cards.md` Part A). The printed 52 / 77 is recovered once the 111 human NaN rows are excluded from the denominator: 51.6 / 76.9 (the embedded recomputation notes)
detection_method (authors', verbatim): Identify passages where the narrating voice (not just a character) generalizes about what the events show (e.g. 'That is how people are', 'It was then she learned that...'); if such commentary appears, mark 'yes'.
scope: local
scope_basis: detection_method
scope_fields (Figure 8 match): narration.style.evaluative_language [LOCAL] ("judgmental/evaluative language plus scene context"); secondary: story.plot.moral [GLOBAL] ("one sentence if signaled") (findings §F.2)
scope_reasoning: The detection_method says "Identify passages where the narrating voice … generalizes" and mark yes if such commentary appears: the value is carried by locatable passages. Local.
validated_on: fiction
related_cells: NC-04, NC-18, NC-33, NC-40 — **not independent measurements**. In the analog studies these four collapsed onto a single observable, "does the text end on a generalising significance statement?", and shared a firing set across most of the documents they fired on at all (specific document ids deliberately omitted: naming them in a file every rater reads leaks answers into blind studies. Never count them as separate evidence, never aggregate them into a score or a lean count, and where they agree report **one** signal, not three or four. Matrix §0.8 already forbids aggregating measured cells; this line puts the same warning where a rater will see it.
fiction_detection: Look for passages in the narrator's own voice — not a character's speech or thought — that state what the story means or what its events teach. §4.1: "Narrators explicitly explain the story's theme 77% of the time, versus 52% for humans: a grieving character's arc will typically end with the narrator stating the lesson learned" (p. 7; findings §D.1). One clear instance makes the answer "yes"; "yes" is the AI side (77% vs 52%, Table 16, p. 26). Commentary voiced by a character does not count; the question says "beyond characters' perspectives".
locate_by: a firing span is a narratorial sentence or paragraph that comments on theme or lesson beyond any character's perspective — most often the closing paragraph, sometimes a paragraph opening or closing a scene. Quote each instance with its location.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-04 (one cell per content type: literal / analog / n/a)
```

```
id: NC-05
name: Dialogue Function
taxonomy_id: `PER_DIA_003` — "Primary functions of dialogue" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 9 of 30 measured (importance 0.0098, 1.0% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Dialogue Function → philosophical debate"; Table 14 same (findings §B.1)
dimension: PER (perspective)
response_type: multi-select
question: "What main functions does dialogue serve?" (Table 14 #12, p. 24)
taxonomy_question: "What main functions does dialogue serve in the story?" (taxonomy.json)
values (exact, taxonomy.json): advance_plot_or_deliver_crucial_information; reveal_character_psychology_or_relationships; worldbuilding_or_background_exposition; philosophical_or_thematic_debate; comic_relief_or_tone_coloration
 printed_option_names (paper): "advance plot, reveal character, worldbuilding, philosophical, comic" (Table 14 #12, p. 24)
option_rows:
  - option: → philosophical debate | human: 34% | ai: 59% | gap (printed): −25 | direction: AI-elevated
human_mean: 34% (prevalence of "philosophical" among selected functions)
ai_mean: 59% (prevalence of "philosophical" among selected functions)
direction: AI-elevated
measured_baseline: matches the printed Table 16 values (human 34% / AI 59% for philosophical debate)
detection_method (authors', verbatim): For key dialogue scenes, identify what changes as a result (plot developments, revelations about character, exposition, thematic arguments, jokes); select each function that is consistently prominent across the text.
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): nearest: narration.perspective.dialogue_speakers [LOCAL] ("named speakers by scene/section") (findings §F.2)
scope_reasoning: The detection_method selects "each function that is consistently prominent across the text" — a whole-text distribution, not one exchange, the same basis on which NC-07 and NC-10 are global. The dialogue scenes become driving spans.
validated_on: fiction
fiction_detection: Read each dialogue exchange for its job: does it move events, reveal who someone is, build the world, make a joke, or argue ideas? The option fires when characters debate meaning, ethics, fate, or belief rather than negotiate the plot. §4.1: "AI dialogue serves philosophical debate more often (59% vs. 34%)" (p. 7; findings §D.1). Philosophical debate is the AI side (Table 16, p. 26). Multi-select: a story can have several main functions; the card asks only whether philosophical debate is among them.
rating_note (measured): exact agreement 0.40; the authors' assigner emits a single dominant function. Fix: name the **primary** function first, then add any further function that is consistently prominent across the text. Do not return a flat unordered set.
locate_by: rate once for the document — primary function first, then any other consistently prominent function — then cite the 2–3 spans that most drive the rating. Driving spans are typically: a dialogue exchange in which characters argue or reflect on an abstract question (meaning, morality, mortality, belief); an exchange that instead moves the plot or reveals character, for contrast.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-05 (one cell per content type: literal / analog / n/a)
```

```
id: NC-06
name: Reference Explicitness
taxonomy_id: `SIT_MET_008` — "Reference Explicitness" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 5 of 30 measured (importance 0.0135, 1.3% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Reference Explicitness → implicit echoes" (AI-elevated group) and "Reference Explicitness → balanced mix" (Human-elevated: Intertextual richness group) (findings §B.1)
dimension: SIT (Situatedness)
response_type: categorical
question: Two printed wordings for the same feature; both reproduced, neither chosen (E1 worklog item 8):
  - "Are intertextual gestures primarily explicit or diffuse?" (Table 14 #16, p. 24)
  - "Are intertextual gestures explicit or diffuse?" (Table 15 #3, p. 25)
taxonomy_question: "Are intertextual gestures primarily explicit name-checks or more diffuse, generic echoes?" (taxonomy.json)
values (exact, taxonomy.json): None (no discernible references); Primarily explicit named references (titles, authors, specific works); Primarily implicit echoes (genres, archetypes, unnamed myths); Balanced mix of explicit and implicit
 printed_option_names (paper): "none, explicit named, implicit echoes, balanced mix" (Table 14 #16, p. 24; Table 15 #3, p. 25)
option_rows:
  - option: → implicit echoes | human: 50% | ai: 72% | gap (printed): −22 | direction: AI-elevated | Table 16 group: Thematic over-determination
  - option: → balanced mix | human: 37% | ai: 16% | gap (printed): +21 | direction: Human-elevated | Table 16 group: Intertextual richness
human_mean: see option_rows
ai_mean: see option_rows
direction: both (two-row feature)
measured_baseline: matches the printed Table 16 values (human 50% / AI 72% for implicit echoes)
detection_method (authors', verbatim): Distinguish between overt citations (e.g., naming Shakespeare, Star Wars) and subtler patterning where the story mirrors recognizable myths or genres without naming them. Classify based on the dominant mode.
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): nearest: narration.style.allusions [LOCAL] ("allusions plus scene/section context") (findings §F.2)
scope_reasoning: The detection_method ends "Classify based on the dominant mode" — a classification of the whole set of gestures. Global.
validated_on: fiction
fiction_detection: Collect every gesture toward another text, author, or specific work — whether named outright or echoed unnamed through genre, archetype or myth — then classify the set: none; mostly explicit and named; mostly diffuse echoes without a name; or a balance of the two. §4.1: "references to other works tend to be vague allusions (72% vs. 50%) rather than specific, named references" (p. 7; findings §D.1) and humans "balance explicit with implicit references more evenly (37% 'balanced mix' vs. 16%), whereas AI generally sticks to vague allusions and avoids naming real brands, places, or works" (p. 7; findings §D.4). The AI side is "implicit echoes"; the human side is "balanced mix" (Table 16, p. 26). See NC-21 for the companion multi-select on kinds of intertextual engagement.
rating_note (measured): exact agreement **0.30 — the worst-scoring card measured**. Collection-rule fix: count only named **texts, authors and specific works**, matching the taxonomy option "Primarily explicit named references (titles, authors, specific works)". Brand names and place names are **not** references for this card; do not count them.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: an unnamed echo of a well-known text, genre or myth; a named reference to a specific text, author or work (brand names and place names do not count — see rating_note); the passage that tips the balance between the two.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-06 (one cell per content type: literal / analog / n/a)
```

### Group: AI-elevated — Sensory & embodied performativity (Table 16, p. 26)

```
id: NC-07
name: Emotional Expression
taxonomy_id: `AGENT_EMO_009` — "Dominant mode of emotional expression" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 2 of 30 measured (importance 0.0358, 3.6% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Emotional Expression → embodied" and "Emotional Expression → explicit labels"; Table 14 #2 / Table 15 #12 "Dominant Emotional Expression" (findings §B.2)
dimension: AGENT
response_type: categorical
question: "How are characters' emotions most commonly conveyed?" (Table 14 #2, p. 24; Table 15 #12, p. 25 — identical wording in both)
taxonomy_question: "How are characters' emotions most commonly conveyed?" (taxonomy.json)
values (exact, taxonomy.json): explicit_emotion_labels; embodied_sensations_and_metaphors; behavioral_cues_and_action_only; consistently_ambiguous_or_suppressed
 printed_option_names (paper): "explicit labels, embodied metaphors, behavioral cues, ambiguous" (Table 14 #2, p. 24)
option_rows:
  - option: → embodied | human: 38% | ai: 81% | gap (printed): −42 | direction: AI-elevated | Table 16 group: Sensory & embodied performativity
  - option: → explicit labels | human: 29% | ai: 8% | gap (printed): +21 | direction: Human-elevated | Table 16 group: Narrative diversity
human_mean: see option_rows
ai_mean: see option_rows
direction: both (two-row feature)
measured_baseline: human 39% / AI 81% for embodied (`feature_cards.md` Part A); recomputation gives 38.8 / 80.8, printed as 38 / 81
detection_method (authors', verbatim): Review scenes with strong affect. If emotions are usually named directly (e.g., 'she was angry, terrified'), choose 'explicit_emotion_labels'. If emotions are more often shown via bodily states or metaphors (tight throat, weights on chest, storm imagery), choose 'embodied_sensations_and_metaphors'. If feelings are mostly inferred from what characters do and say without explicit naming or sensory description, choose 'behavioral_cues_and_action_only'. If the narration avoids or undercuts emotional disclosure so that states remain unclear, choose 'consistently_ambiguous_or_suppressed'.
related_fingerprint: FP-DeepSeek-2 "Emotional expression → behavioral cues" is a third option of this same feature (Table 17, p. 27; findings §B.2)
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): nearest: story.agents.major_characters[].emotion_trajectory [GLOBAL]; driving spans come from narration.style.figurative_language [LOCAL] and imagery [LOCAL] (findings §F.2)
scope_reasoning: The detection_method reviews "scenes with strong affect" and picks the mode emotions are "usually" or "more often" conveyed in — a dominant mode over the whole text. Global.
validated_on: fiction
fiction_detection: Each time a character's emotion reaches the page, note the vehicle: a named feeling ("she felt afraid"), a bodily sensation or metaphor, an outward behavior, or something left ambiguous; then rate the dominant vehicle. §4.1: "AI overwhelmingly conveys emotion through physical sensations and bodily metaphors (81% vs. 38% human)" and "Where a human author might write that a character 'felt afraid,' AI renders fear as a tightening chest, cold sweat, and dimming lamplight" (p. 7; findings §D.3); "Humans use explicit emotion labels 29% of the time versus just 8% for AI" (p. 7; findings §D.3). Embodied is the AI side; explicit labels is the human side (Table 16, p. 26). This is the largest printed gap in Table 16 (−42 on the embodied row).
rating_note (measured): exact agreement 0.40; we systematically **under-called** `embodied_sensations_and_metaphors`. Base rate to hold in mind: AI stories take this option **81%** of the time against 38% for humans (Table 16, p. 26) — under-calling it is the costly error on the highest-gap row in the table. Boundary rule: `embodied` means the body or an environmental image is the *vehicle* carrying the emotion (tight throat, weight on the chest, storm imagery); a physical action that merely accompanies a feeling, with the feeling inferable only from what the character does, is `behavioral_cues_and_action_only`.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: a passage that renders feeling as a physical sensation (chest, breath, hands, temperature); a sentence that names the emotion directly; a passage that shows emotion only through action.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-07 (one cell per content type: literal / analog / n/a)
```

```
id: NC-08
name: Setting as Psychological Mirror
taxonomy_id: `SET_ATM_022` — "Setting as Psychological Mirror" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 27 of 30 measured (importance 0.0008, 0.1% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Setting as Psychological Mirror" (s); Table 14 same (findings §B.2)
dimension: SET (setting)
response_type: scale (1–5 Likert)
question: "To what degree does physical environment mirror characters' inner states?" (Table 14 #6, p. 24)
taxonomy_question: "To what degree does the story use physical environment as a mirror or metaphor for characters' inner states?" (taxonomy.json)
values (exact, taxonomy.json): 1; 2; 3; 4; 5
 printed_option_names (paper): "1–5" (Table 14 #6, p. 24)
option_rows:
  - option: scale mean | human: 3.58 | ai: 4.07 | gap (printed): −0.49 | direction: AI-elevated
human_mean: 3.58
ai_mean: 4.07
direction: AI-elevated
measured_baseline: matches the printed Table 16 values (human 3.58 / AI 4.07)
detection_method (authors', verbatim): Look for explicit or implicit correlations between mood and place (dust as shroud of grief, collapsing houses as failing marriages, blight as guilt). If settings and emotions rarely echo each other, score 1. If some key scenes align weather/landscape/architecture with emotional turns, but many do not, score 3. If the story consistently externalizes psychology through place and repeatedly aligns environmental change with inner change, score 5.
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): story.setting.atmosphere [GLOBAL] (findings §F.2)
scope_reasoning: The detection_method asks whether "the story consistently externalizes psychology through place" across scenes. Global.
validated_on: fiction
fiction_detection: Read for weather, light, rooms, and landscape that change with, or stand in for, what a character feels. Low end (1): the environment is described independently of mood; high end (5): the environment consistently reflects inner states. §4.1: AI "uses setting as a reflection of characters' inner states more heavily" and its fear example ends in "dimming lamplight" (p. 7; findings §D.3). The AI side is the high end: AI mean 4.07 against human 3.58 (Table 16, p. 26). Rank 6 of the AI-core features (Table 14, p. 24).
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: a weather or light description that shifts at the moment a character's mood shifts; a room or landscape described in the character's emotional vocabulary.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-08 (one cell per content type: literal / analog / n/a)
```

```
id: NC-09
name: Environmental & Ecological Emphasis
taxonomy_id: `SET_LOC_014` — "Environmental and Ecological Emphasis" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 30 of 30 measured (importance 0.0003, 0.0% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Environmental & Ecological Emphasis" (s); Table 14 "Environmental and Ecological Emphasis" (findings §B.2)
dimension: SET (setting)
response_type: scale (1–5 Likert)
question: "How prominent is the natural environment or ecology in the narrative?" (Table 14 #17, p. 24)
taxonomy_question: "To what extent does the story foreground landscape, climate, and ecological processes as part of the setting's logic?" (taxonomy.json)
values (exact, taxonomy.json): 1; 2; 3; 4; 5
 printed_option_names (paper): "1–5" (Table 14 #17, p. 24)
option_rows:
  - option: scale mean | human: 2.83 | ai: 3.21 | gap (printed): −0.38 | direction: AI-elevated
human_mean: 2.83
ai_mean: 3.21
direction: AI-elevated
measured_baseline: human 2.84 / AI 3.21 (`feature_cards.md` Part A); printed human 2.83 — last-digit rounding from unrounded means
detection_method (authors', verbatim): Look for passages about rivers, forests, farms, weather patterns, dust, blight, floods, animals, or resource cycles that go beyond mere backdrop. If environment is scarcely described, score 1. If natural features occasionally shape mood or minor events, score 3. If ecological systems are central—villages eaten by jungle, grain-law deserts, derbies in wheat oceans—score 5.
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): story.setting.atmosphere [GLOBAL]; secondary: story.setting.locations [LOCAL/GLOBAL] (story-level half) (findings §F.2)
scope_reasoning: The detection_method scores whether ecological systems are "central" to the story as a whole. Global.
validated_on: fiction
fiction_detection: Read for how much page space and narrative weight go to the natural world — land, weather, plants, animals, seasons, ecological systems. Low end (1): nature is absent or incidental; high end (5): it is a prominent presence throughout. §4.1 says only that "AI pays closer attention to physical environment" (p. 7; findings §D.3); no percentage is given for this feature in prose. The AI side is the high end: AI mean 3.21 against human 2.83 (Table 16, p. 26).
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: an extended description of landscape, weather, or wildlife; a passage where the natural world acts on the plot or characters.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-09 (one cell per content type: literal / analog / n/a)
```

```
id: NC-10
name: Sensory Modalities
taxonomy_id: `SET_ATM_017` — "Dominant Sensory Modalities" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 8 of 30 measured (importance 0.0099, 1.0% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Sensory Modalities → olfactory"; Table 14 "Dominant Sensory Modalities → olfactory" (findings §B.2)
dimension: SET (setting)
response_type: multi-select
question: "Which sensory modalities does the story most frequently engage?" (Table 14 #4, p. 24)
taxonomy_question: "Which sensory modalities does the story most frequently engage when rendering setting?" (taxonomy.json)
values (exact, taxonomy.json): visual; auditory; olfactory; tactile; gustatory; kinesthetic
 printed_option_names (paper): "visual, auditory, olfactory, tactile, gustatory, kinesthetic" (Table 14 #4, p. 24)
option_rows:
  - option: → olfactory | human: 57% | ai: 82% | gap (printed): −26 | direction: AI-elevated
human_mean: 57% (prevalence of "olfactory" among selected modalities)
ai_mean: 82% (prevalence of "olfactory" among selected modalities)
direction: AI-elevated
measured_baseline: matches the printed Table 16 values (human 57% / AI 82% for olfactory)
detection_method (authors', verbatim): Reread descriptive passages and note which senses are invoked. Visual includes color, shape, light; auditory includes sounds, silence, music; olfactory includes smells (rot, perfume, smoke); tactile covers touch, texture, temperature; gustatory covers taste; kinesthetic covers bodily motion, vertigo, pressure. Select all modalities that appear regularly and distinctly in setting description, not just once or twice in passing.
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): no exact object match for a whole-text modality ranking; nearest: narration.style.imagery [LOCAL] ("vivid sensory descriptions plus scene context"), which supplies the driving spans (findings §F.2)
scope_reasoning: The detection_method says "Select all modalities that appear regularly and distinctly in setting description" — a selection over the whole text, confirmed by Gate 2 ruling . Global; the sensory passages are driving spans.
validated_on: fiction
fiction_detection: Mark every passage that engages a sense and note which one. The option fires when smell is among the modalities that appear regularly and distinctly — and the card requires every such modality to be listed, not only the dominant ones. §4.1: AI "deploys more smell-based imagery (82% vs. 57%)" (p. 7; findings §D.3). Olfactory is the AI side (Table 16, p. 26). Rank 4 of the AI-core features (Table 14, p. 24).
rating_note (measured): exact agreement 0.30 with **Jaccard 1.00** — our selections were always a strict subset of the authors'. Fix: **list every modality present**, i.e. every one that appears regularly and distinctly, not just the dominant three or four. The authors' assigner typically returns five.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: the most prominent smell or scent descriptions (of a place, person, object, food, weather); a passage where smell carries a scene that other senses could have; for contrast, a sensory passage with no olfactory element.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-10 (one cell per content type: literal / analog / n/a)
```

```
id: NC-11
name: Sensory Density
taxonomy_id: `SET_ATM_005` — "Sensory Density" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 26 of 30 measured (importance 0.0012, 0.1% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Sensory Density" (s); Table 14 same (findings §B.2)
dimension: SET (setting)
response_type: scale
question: "How dense is sensory description across the narrative?" (Table 14 #8, p. 24)
taxonomy_question: "How dense is the sensory description of the setting across the narrative?" (taxonomy.json)
values (exact, taxonomy.json): 1_minimal; 2_sparse; 3_moderate; 4_rich; 5_lush_overdetermined
 printed_option_names (paper): "minimal–lush" (Table 14 #8, p. 24). Table 16 marks the feature "s" (1–5 Likert, values are means); the mapping of the labels to integers is not printed (E1 worklog item 9).
option_rows:
  - option: scale mean | human: 3.66 | ai: 3.93 | gap (printed): −0.26 | direction: AI-elevated
human_mean: 3.66
ai_mean: 3.93
direction: AI-elevated
measured_baseline: human 3.67 / AI 3.92 (`feature_cards.md` Part A); printed 3.66 / 3.93 — last-digit rounding
detection_method (authors', verbatim): Estimate the proportion of sentences devoted to sensory detail and the layering of multiple senses per scene. Minimal: mostly abstract summary or dialogue with almost no concrete description. Sparse: occasional brief scene-setting. Moderate: regular but unobtrusive descriptive lines. Rich: most scenes include multi-sensory description. Lush_overdetermined: descriptions are frequent, extended, and highly sensuous, often slowing pace.
style_boundary_note: the paper explicitly places this feature "on the non-style side" of its style rule (p. 18; findings §A.3, §B.2)
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): nearest: narration.style.imagery [LOCAL] (findings §F.2)
scope_reasoning: The detection_method estimates "the proportion of sentences devoted to sensory detail" — a rate over the whole text. Global.
validated_on: fiction
fiction_detection: Judge how much of the prose, per unit of text, is given to sensory detail. Low end ("minimal"): events and speech proceed with little sensory texture; high end ("lush"): nearly every scene is layered with sight, sound, touch, smell. §4.1's heading for this group is "AI over-writes the body and senses" (p. 7; findings §D.3), but the prose gives no number for density itself. The AI side is the high end: AI mean 3.93 against human 3.66 (Table 16, p. 26).
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: the densest sensory paragraph in the text; a scene rendered almost entirely through sensory detail; for contrast, a scene with none.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-11 (one cell per content type: literal / analog / n/a)
```

```
id: NC-12
name: Depth of Interior Access
taxonomy_id: `PER_FOC_001` — "Depth of interior access to characters" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 24 of 30 measured (importance 0.0014, 0.1% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Depth of Interior Access" (s); Table 14 same (findings §B.2)
dimension: PER (perspective)
response_type: scale (1–5 Likert)
question: "How deep into characters' inner life does narration go?" (Table 14 #20, p. 24)
taxonomy_question: "How deep into characters' inner life does the non-dialogue narration regularly go?" (taxonomy.json)
values (exact, taxonomy.json): 1; 2; 3; 4; 5
 printed_option_names (paper): "1–5" (Table 14 #20, p. 24)
option_rows:
  - option: scale mean | human: 3.67 | ai: 3.93 | gap (printed): −0.26 | direction: AI-elevated
human_mean: 3.67
ai_mean: 3.93
direction: AI-elevated
measured_baseline: human 3.68 / AI 3.93 (`feature_cards.md` Part A); printed human 3.67 — last-digit rounding
detection_method (authors', verbatim): On a 1–5 scale: 1 = purely external behavior and speech, no thoughts or feelings rendered; 2 = occasional simple feeling labels ('she was angry'); 3 = regular access to thoughts and emotions in sentences paraphrasing them; 4 = sustained, detailed interior monologue or blended narration; 5 = immersive stream-of-consciousness or heavily merged narrator/character voice. Rate based on the dominant practice across the text.
style_boundary_note: the paper explicitly places this feature "on the non-style side" of its style rule (p. 18; findings §A.3, §B.2)
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): nearest: narration.perspective.point_of_view [GLOBAL]; driving spans from narration.perspective.focalization [LOCAL] (findings §F.2)
scope_reasoning: The detection_method ends "Rate based on the dominant practice across the text." Global.
validated_on: fiction
fiction_detection: Read for how far the narration enters thought, memory, sensation, and motive. Low end (1): characters are seen only from outside, through speech and action; high end (5): the narration dwells continuously in inner life. §4.1: "AI pays closer attention to physical environment and characters' inner mental states" (p. 7; findings §D.3); no number is given for this feature in prose. The AI side is the high end: AI mean 3.93 against human 3.67 (Table 16, p. 26).
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: a passage of sustained interior monologue or free indirect thought; a moment where narration reports motive the character has not spoken; for contrast, an externally rendered scene.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-12 (one cell per content type: literal / analog / n/a)
```

### Group: AI-elevated — Structural streamlining (Table 16, p. 26)

```
id: NC-13
name: Causal Chain Continuity
taxonomy_id: `EVT_CAU_002` — "Continuity of the main causal chain" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 11 of 30 measured (importance 0.0093, 0.9% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Causal Chain Continuity" (s); Table 14 "Continuity of Main Causal Chain" (findings §B.3)
dimension: EVT (events)
response_type: scale (1–5 Likert)
question: "How continuous is the single causal chain from inciting incident to ending?" (Table 14 #7, p. 24)
taxonomy_question: "To what extent does a single, continuous causal chain carry through from inciting incident to ending, versus multiple disjoint or restarting chains?" (taxonomy.json)
values (exact, taxonomy.json): 1; 2; 3; 4; 5
 printed_option_names (paper): "1–5" (Table 14 #7, p. 24)
option_rows:
  - option: scale mean | human: 3.92 | ai: 4.20 | gap (printed): −0.28 | direction: AI-elevated
human_mean: 3.92
ai_mean: 4.20
direction: AI-elevated
measured_baseline: matches the printed Table 16 values (human 3.92 / AI 4.20)
detection_method (authors', verbatim): Trace how often later events are direct or indirect consequences of prior ones in the same line of action; score 1 if the story is largely episodic with unrelated episodes, 3 if there is a discernible but occasionally broken chain, 5 if nearly every significant event is tightly linked in one continuous line of cause-and-effect.
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): story.events.causality [GLOBAL] ("causal links formatted as event1 -> event2: explanation") (findings §F.2)
scope_reasoning: The detection_method traces "how often later events are direct or indirect consequences of prior ones" across the whole line of action. Global.
validated_on: fiction
fiction_detection: Trace the chain of cause and effect from the first disturbance to the ending. Low end (1): the chain breaks, branches, or is interrupted by events that do not follow from what came before; high end (5): every major event follows from the one before with no loose ends. §4.1: "AI stories exhibit tighter causal chains" and "AI favors single-track narratives with fewer loose ends; human stories are messier, with time jumps and disjointed causal chains" (p. 7; findings §D.2). The AI side is the high end: AI mean 4.20 against human 3.92 (Table 16, p. 26).
rating_note (measured): measured bias **+0.70** — the joint-largest over-rating measured. Tighten: score 5 only when *nearly every* significant event is tightly linked in one continuous cause-and-effect line; a chain that is discernible but occasionally broken is a **3**, and a largely episodic story is a 1.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: a hinge where one event visibly produces the next; a break or loose end where an event arrives without cause; the ending's dependence (or not) on the inciting incident.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-13 (one cell per content type: literal / analog / n/a)
```

```
id: NC-14
name: Spatial Granularity
taxonomy_id: `SET_LOC_002` — "Spatial Granularity Level" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 22 of 30 measured (importance 0.0016, 0.2% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Spatial Granularity" (o); Table 14 "Spatial Granularity Level" (findings §B.3)
dimension: SET (setting)
response_type: ordinal
question: "How fine-grained is the story's depiction of physical space?" (Table 14 #13, p. 24)
taxonomy_question: "How fine-grained is the story's depiction of physical space and geography?" (taxonomy.json)
values (exact, taxonomy.json): 1_very_low; 2_low; 3_medium; 4_high
 printed_option_names (paper): "very_low–high" (Table 14 #13, p. 24). Table 16 values are "means over integer codes" (Table 16 caption, p. 26).
option_rows:
  - option: ordinal mean | human: 2.27 | ai: 2.53 | gap (printed): −0.26 | direction: AI-elevated
human_mean: 2.27
ai_mean: 2.53
direction: AI-elevated
measured_baseline: matches the printed Table 16 values (human 2.27 / AI 2.53)
detection_method (authors', verbatim): Assess the density and precision of spatial markers: number of distinct place names, room names, street names, and micro-locational cues. Very low: almost no concrete location detail. Low: a few coarse labels (house, city). Medium: recurring named rooms/streets with some layout hints. High: map-like inventories of spaces, routes, and relative positions.
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): nearest: story.setting.locations [LOCAL/GLOBAL] ("include scene-level and story-level locations, with scope noted") (findings §F.2)
scope_reasoning: The detection_method assesses "the density and precision of spatial markers" over the text. Global.
validated_on: fiction
fiction_detection: Judge how finely physical space is drawn: are rooms, streets, distances, and layouts specified, or is space left schematic? Low end ("very_low"): locations are named but not rendered; high end ("high"): space is mapped in detail as characters move through it. §4.1 does not name this feature; detection rests on the question wording and Table 16. The AI side is the high end: AI mean 2.53 against human 2.27 (Table 16, p. 26).
rating_note (measured): measured bias **−0.70** (7/10 under-rated) — our anchors were a full rung too demanding. Loosen: recurring named rooms or streets with some layout hints is already `3_medium`; `4_high` needs map-like inventories of spaces, routes and relative positions; a few coarse labels (house, city) is `2_low`.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: the most detailed rendering of a room or place; a passage tracking movement through specified space; for contrast, a scene set in an unspecified "somewhere".
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-14 (one cell per content type: literal / analog / n/a)
```

```
id: NC-15
name: Agency in Resolution
taxonomy_id: `PLT_CON_007` — "Agency in Resolution" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 16 of 30 measured (importance 0.0054, 0.5% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Agency in Resolution → protagonist choice"; Table 14 same (findings §B.3)
dimension: PLT (plot)
response_type: categorical
question: "Is resolution driven by protagonist's choices or external events?" (Table 14 #9, p. 24; also Table 1, p. 4)
taxonomy_question: "Is the resolution of the central conflict driven mainly by the protagonist's deliberate choices or by events beyond their control?" (taxonomy.json)
values (exact, taxonomy.json): primarily_protagonist_choice; mixed_choice_and_chance; primarily_external_fate/chance
 printed_option_names (paper): "protagonist_choice, mixed, external_fate" (Table 14 #9, p. 24)
option_rows:
  - option: → protagonist choice | human: 46% | ai: 69% | gap (printed): −23 | direction: AI-elevated
human_mean: 46% (prevalence of "protagonist_choice")
ai_mean: 69% (prevalence of "protagonist_choice")
direction: AI-elevated
measured_baseline: matches the printed Table 16 values (human 46% / AI 69% for protagonist choice)
detection_method (authors', verbatim): Assess who or what triggers the turning point: the protagonist's decisions and actions, a combination, or mostly accidents, prophecies, or external interventions.
scope: local
scope_basis: detection_method
scope_fields (Figure 8 match): nearest: story.plot.plot_arc [GLOBAL]; story.events.causality [GLOBAL] (findings §F.2)
scope_reasoning: The detection_method assesses "who or what triggers the turning point" — a bounded, locatable event. Local.
validated_on: fiction
fiction_detection: Find the passage where the central conflict resolves and ask what produced the outcome: a choice the protagonist makes, an external event or stroke of fate, or a mix. §4.1: AI stories have "more protagonist-driven resolutions (69% vs. 46%)" (p. 7; findings §D.2). Protagonist choice is the AI side (Table 16, p. 26). Rank 9 of the AI-core features (Table 14, p. 24).
locate_by: a firing span is the resolution passage itself plus the protagonist's decision that brings it about (which may sit earlier). Quote both with locations; if the outcome arrives by accident, weather, another character's act, or coincidence, the option does not fire.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-15 (one cell per content type: literal / analog / n/a)
```

```
id: NC-16
name: Character Introduction
taxonomy_id: `AGENT_ATTR_001` — "Dominant mode of introducing the central character" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 4 of 30 measured (importance 0.0166, 1.7% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Character Introduction → external description"; Table 14 same (findings §B.3)
dimension: AGENT
response_type: categorical
question: "What narrative device primarily introduces the central character?" (Table 14 #5, p. 24)
taxonomy_question: "In the first substantial appearance of the central character, what narrative device is primarily used to introduce them?" (taxonomy.json)
values (exact, taxonomy.json): external description (appearance/background summary); in-action event (we see them doing something significant); in-dialogue (they speak before being described); inner thought/monologue; through others' reports or gossip
 printed_option_names (paper): "external_desc, in-action, in-dialogue, inner_thought, others_reports" (Table 14 #5, p. 24)
option_rows:
  - option: → external description | human: 30% | ai: 52% | gap (printed): −22 | direction: AI-elevated
human_mean: 30% (prevalence of "external_desc")
ai_mean: 52% (prevalence of "external_desc")
direction: AI-elevated
measured_baseline: matches the printed Table 16 values (human 30% / AI 52% for external description)
detection_method (authors', verbatim): Locate the passage where the central figure first shows up. Decide which element most defines that introduction: descriptive paragraphs, action, their speech, internal monologue, or someone else talking about them.
related_fingerprints: FP-Human-1 "Character introduction → in-dialogue" and FP-Kimi-1 "Character introduction → in-action event" are other options of this same feature (Table 17, p. 27; findings §B.3)
attribution_note: this is a core feature elevated across all five AI models (Table 16, p. 26). It is not attributed to any single model; the abstract's Gemini wording has no Gemini-specific number anywhere in the paper (; findings §C.3).
scope: local
scope_basis: detection_method
scope_fields (Figure 8 match): nearest: story.agents.major_characters[].attributes [GLOBAL]; role [GLOBAL] (findings §F.2)
scope_reasoning: The detection_method opens "Locate the passage where the central figure first shows up" — an explicit instruction to find one passage. Local.
validated_on: fiction
fiction_detection: Locate the first appearance of the central character and classify the device: a description of appearance or circumstances from outside; an action in progress; a line of dialogue; an interior thought; or another character's report. §4.1 does not quote a number for this feature; Table 16 gives 52% external description for AI against 30% for humans (p. 26). External description is the AI side. Rank 5 of the AI-core features (Table 14, p. 24).
locate_by: a firing span is the passage that introduces the central character when it opens with external description (looks, clothing, age, situation seen from outside) rather than action, speech, thought, or report. Quote it with its location.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-16 (one cell per content type: literal / analog / n/a)
```

```
id: NC-17
name: Subplot Integration
taxonomy_id: `PLT_THM_009` — "Integration of Subplots with Theme" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 3 of 30 measured (importance 0.0226, 2.3% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Subplot Integration → no subplots" (Structural streamlining group) and "Subplot Integration → thematically parallel" (Human-elevated: Narrative diversity group) (findings §B.3)
dimension: PLT (plot)
response_type: categorical
question: "How directly do subplots echo the central theme?" (Table 14 #14, p. 24; Table 15 #7, p. 25 — identical wording in both)
taxonomy_question: "If subplots exist, how directly do they echo or complicate the central theme?" (taxonomy.json)
values (exact, taxonomy.json): no_subplots; directly_thematically_parallel; thematically_contrasting/ironic; largely_thematically_independent
 printed_option_names (paper): "no_subplots, thematically_parallel, contrasting, independent" (Table 14 #14, p. 24); printed as "no subplots, thematically parallel, contrasting, independent" in Table 15 #7 (p. 25)
option_rows:
  - option: → no subplots | human: 57% | ai: 79% | gap (printed): −22 | direction: AI-elevated | Table 16 group: Structural streamlining
  - option: → thematically parallel | human: 42% | ai: 21% | gap (printed): +22 | direction: Human-elevated | Table 16 group: Narrative diversity
human_mean: see option_rows
ai_mean: see option_rows
direction: both (two-row feature)
measured_baseline: matches the printed Table 16 values (human 57% / AI 79% for no subplots)
detection_method (authors', verbatim): Identify any secondary plotlines. Judge whether they mirror the main theme, provide a deliberate counterpoint, or mostly concern different issues.
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): story.plot.themes [GLOBAL]; story.plot.summary [GLOBAL] (findings §F.2)
scope_reasoning: The detection_method identifies any secondary plotlines and judges how they relate to the main theme across the story; "no_subplots" is an absence over the whole text. Global.
validated_on: fiction
fiction_detection: First decide whether there is any secondary thread with its own characters or stakes; if none, the answer is "no subplots". If there are, ask how each relates to the central theme: parallel, contrasting, or independent. §4.1: AI stories have "far fewer subplots (79% 'no subplots' vs. 57%)" (p. 7; findings §D.2) and humans "integrate more subplots into overarching themes (42% vs. 21%)" (p. 8; findings §D.5). "No subplots" is the AI side; "thematically parallel" is the human side (Table 16, p. 26).
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: the entry point of each secondary thread, if any; the passage where a subplot's outcome echoes or reflects the main theme; where no subplot exists, the report says so and cites nothing.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-17 (one cell per content type: literal / analog / n/a)
```

```
id: NC-18
name: Resolution Mode
taxonomy_id: `EVT_SCH_004` — "Mode of resolution of the main event chain" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 15 of 30 measured (importance 0.0060, 0.6% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Resolution Mode → internal understanding"; Table 14 "Mode of Resolution → internal understanding" (findings §B.3)
dimension: EVT (events)
response_type: categorical
question: "Is the main event chain resolved through internal acceptance or external action?" (Table 14 #18, p. 24)
taxonomy_question: "How is the central conflict or event chain brought to a close, if at all?" (taxonomy.json)
values (exact, taxonomy.json): resolved_through_external_action_or_intervention; resolved_through_internal_understanding_or_acceptance; partial_or_compromise_resolution; open_or_ambiguously_unresolved; catastrophic_or_irrevocably_negative_outcome
 printed_option_names (paper): "resolved externally, resolved internally, unresolved" (Table 14 #18, p. 24)
option_rows:
  - option: → internal understanding | human: 27% | ai: 47% | gap (printed): −21 | direction: AI-elevated
human_mean: 27% (prevalence of "resolved internally")
ai_mean: 47% (prevalence of "resolved internally")
direction: AI-elevated
measured_baseline: matches the printed Table 16 values (human 27% / AI 47% for internal understanding)
detection_method (authors', verbatim): Identify the final events addressing the core conflict; code whether they involve decisive external acts (escape, arrest, coup), an internal shift (forgiveness, letting go, insight), a negotiated or mixed outcome, a deliberately open ending with no clear outcome, or an outcome framed as catastrophic (death, collapse, unrepaired devastation).
scope: local
scope_basis: detection_method
scope_fields (Figure 8 match): nearest: story.plot.plot_arc [GLOBAL]; story.events.causality [GLOBAL] (findings §F.2)
scope_reasoning: The detection_method says "Identify the final events addressing the core conflict" and code them — a bounded terminal passage. Local.
validated_on: fiction
related_cells: NC-04, NC-18, NC-33, NC-40 — **not independent measurements**. In the analog studies these four collapsed onto a single observable, "does the text end on a generalising significance statement?", and shared a firing set across most of the documents they fired on at all (specific document ids deliberately omitted: naming them in a file every rater reads leaks answers into blind studies. Never count them as separate evidence, never aggregate them into a score or a lean count, and where they agree report **one** signal, not three or four. Matrix §0.8 already forbids aggregating measured cells; this line puts the same warning where a rater will see it.
boundary_vs_NC-40: **What this card claims about a closing span:** the *kind of settlement* — whether the core conflict is closed by a decisive external act, by an internal shift of understanding or acceptance, by a compromise, left deliberately open, or ended catastrophically. It claims **nothing about where in story time the narration stops**; that is NC-40's question alone. Apply both by locating the climax first, then answering the two questions separately, and never let one card's answer license the other's. A single closing sentence can carry both properties and can legitimately place the same document on opposite sides — measured case (document identifier and quoted span deliberately omitted — naming them in a file every rater reads leaks answers into blind studies): a close that names a concrete external act, which this card reads as external resolution, while ending on a forward reassurance, which NC-40 reads as a forward flourish. Both were correct applications of their own rules; the verdicts are not in conflict because they are not about the same property. **Governing rule:** where one sentence is the only evidence for both cards, it is one observable — report it once, cite the span once, and never sum NC-18 with NC-40.
fiction_detection: Read the ending and ask what closes the main chain of events: an external action or event, a character's internal acceptance or realization, or nothing (left unresolved). §4.1: "AI resolutions favor internal understanding or acceptance (47% vs. 27%), whereas humans are more comfortable with ambiguous endings" (p. 7; findings §D.2). Internal understanding is the AI side (Table 16, p. 26). An ending that closes on a character coming to terms with, understanding, or accepting the situation — without an external act settling it — fires the option.
locate_by: a firing span is the resolving passage where the main conflict is settled by a character's realization, acceptance, or change of understanding rather than by an outward event. Quote it with its location; if the chain is settled by external action, or not settled, the option does not fire.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-18 (one cell per content type: literal / analog / n/a)
```

```
id: NC-19
name: Opening Spatial Grounding
taxonomy_id: `SET_LOC_004` — "Opening Spatial Grounding" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 18 of 30 measured (importance 0.0025, 0.3% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Opening Spatial Grounding" (o); Table 14 same (findings §B.3)
dimension: SET (setting)
response_type: ordinal
question: "How clearly does the opening ground the reader in a specific physical setting?" (Table 14 #11, p. 24)
taxonomy_question: "How clearly does the story's opening ground the reader in a specific physical setting?" (taxonomy.json)
values (exact, taxonomy.json): none_or_very_vague; minimal_local_details_only; clear_local_setting_but_limited_global_context; clear_local_and_global_spatial_context
 printed_option_names (paper): "none/vague, minimal, clear local, clear local+global" (Table 14 #11, p. 24). Table 16 values are "means over integer codes" (Table 16 caption, p. 26).
option_rows:
  - option: ordinal mean | human: 2.12 | ai: 2.33 | gap (printed): −0.20 | direction: AI-elevated
human_mean: 2.12
ai_mean: 2.33
direction: AI-elevated
measured_baseline: matches the printed Table 16 values (human 2.12 / AI 2.33)
detection_method (authors', verbatim): Examine the first scene: note whether it gives no concrete location, only a room or generic area, a specific place without wider context, or both local scene and its position in a larger geography (e.g. city, country).
scope: local
scope_basis: detection_method
scope_fields (Figure 8 match): nearest: story.setting.locations [LOCAL/GLOBAL] (findings §F.2)
scope_reasoning: The detection_method says "Examine the first scene" — one bounded passage. Local.
validated_on: fiction
fiction_detection: Read the opening passage and ask how firmly it places the reader in a physical location. Rungs, in the paper's own labels: none/vague; minimal; clear local (a specific room, street, or place); clear local+global (a specific place set within a wider named world or region). §4.1 does not name this feature; detection rests on the question wording and Table 16. The AI side is the higher rung: AI mean 2.33 against human 2.12 (Table 16, p. 26). The gap (−0.20) is the smallest printed gap among the 30 core features.
locate_by: a firing span is the opening passage (first paragraph or first scene) when it establishes a specific physical place, and the specific sentences that do so. Quote them with locations.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-19 (one cell per content type: literal / analog / n/a)
```

```
id: NC-20
name: Pre-Threat Character Investment
taxonomy_id: `REV_SUS_007` — "Pre-Threat Character Investment" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 28 of 30 measured (importance 0.0007, 0.1% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Pre-Threat Character Investment" (s); Table 14 same (findings §B.3)
dimension: REV (revelation)
response_type: scale (1–5 Likert)
question: "How much does the story build investment before major jeopardy?" (Table 14 #19, p. 24)
taxonomy_question: "How much does the story build reader investment in threatened character(s) before placing them in major jeopardy?" (taxonomy.json)
values (exact, taxonomy.json): 1; 2; 3; 4; 5
 printed_option_names (paper): "1–5" (Table 14 #19, p. 24)
option_rows:
  - option: scale mean | human: 2.76 | ai: 2.99 | gap (printed): −0.23 | direction: AI-elevated
human_mean: 2.76
ai_mean: 2.99
direction: AI-elevated
measured_baseline: human 2.77 / AI 2.99 (`feature_cards.md` Part A); printed human 2.76 — last-digit rounding
detection_method (authors', verbatim): Assess the amount of interiority, backstory, and social embedding given to the character(s) before the main danger appears. Rate 1 if they are sketched minimally before peril; 5 if the narrative spends extensive time deepening them before exposing them to threat.
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): nearest: story.plot.plot_arc [GLOBAL] ("rising action -> climax -> falling action"); discourse.revelation.suspense [GLOBAL] (findings §F.2)
scope_reasoning: The detection_method assesses the *amount* of interiority, backstory and social embedding accumulated before the main danger appears — a quantity over a stretch of text, not a passage. Global.
validated_on: fiction
fiction_detection: Find the point where major jeopardy first arrives, then judge how much the text has done before that point to make the reader care about the characters at risk. Low end (1): danger arrives before the reader knows or cares about anyone; high end (5): extended character-building precedes the threat. §4.1 does not name this feature; detection rests on the question wording and Table 16. The AI side is the high end: AI mean 2.99 against human 2.76 (Table 16, p. 26).
rating_note (measured): exact agreement 0.20 — **the weakest scale card measured** — and no rule existed for in-medias-res openings. Added rule: when the story opens with the character already in jeopardy, rate 1–2; investment supplied retrospectively (backstory delivered after the threat has begun) does not count toward this card, because the authors' detection_method scores what is given "before the main danger appears".
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: the onset of major jeopardy; the principal investment-building passages before it (a character's routine, relationships, wants); for contrast, an opening that begins in danger.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-20 (one cell per content type: literal / analog / n/a)
```

### Group: Human-elevated — Intertextual richness (Table 16, p. 26)

```
id: NC-21
name: Intertextual Strategy
taxonomy_id: `SIT_MET_202` — "Intertextual Strategy Types" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 6 of 30 measured (importance 0.0126, 1.3% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Intertextual Strategy → explicit named reference"; Table 15 "Intertextual Strategy Types → explicit named reference" (findings §B.4)
dimension: SIT (Situatedness)
response_type: multi-select
question: "What kinds of intertextual engagement does the story employ?" (Table 15 #1, p. 25; also Table 1, p. 4)
taxonomy_question: "What kinds of intertextual engagement with other works or traditions does the story employ?" (taxonomy.json)
values (exact, taxonomy.json): explicit_named_reference_to_specific_texts_or_authors; retelling_or_reframing_of_specific_canonical_story; stylistic_pastiche_of_identifiable_author_or_period; use_of_mythological_or_religious_source_material; self_referential_links_to_other_works_in_same_storyworld
 printed_option_names (paper): "explicit named, retelling, pastiche, myth/religion, self referential" (Table 15 #1, p. 25)
option_rows:
  - option: → explicit named reference | human: 47% | ai: 24% | gap (printed): +23 | direction: Human-elevated
human_mean: 47% (prevalence of "explicit named" among selected kinds)
ai_mean: 24% (prevalence of "explicit named" among selected kinds)
direction: Human-elevated
measured_baseline: human 46% / AI 24% for explicit named reference (`feature_cards.md` Part A); recomputation gives 46.3 / 23.7, printed as 47 / 24
detection_method (authors', verbatim): Identify whether the text (a) names specific books, authors, films, etc.; (b) clearly retells or inverts a known story; (c) imitates a recognizable style (e.g. Doyle pastiche); (d) draws on myth/religion beyond generic reference; or (e) references prior stories in the same sequence; select all that apply.
scope: local
scope_basis: detection_method
scope_fields (Figure 8 match): narration.style.allusions [LOCAL] ("allusions plus scene/section context") (findings §F.2)
scope_reasoning: The detection_method enumerates what the text does — names works, retells, imitates a style, draws on myth — and says "select all that apply"; each is evidenced by a locatable gesture. Local.
validated_on: fiction
absence_rule: this feature has **no absence value** in `taxonomy.json` — the five values are all positive strategies. A story with no intertextual engagement is written as an empty selection, `[]`, not as a "none" option. (taxonomy.json; `feature_cards.md` §3)
fiction_detection: Mark every place the text names a real author, title, work, character from another work, or a specific text. §4.1: "Humans reference specific texts and authors at nearly double the AI rate (47% vs. 24%)" while "AI generally sticks to vague allusions and avoids naming real brands, places, or works" (p. 7; findings §D.4). Explicit named reference is the human side (Table 16, p. 26); this is the top-ranked human-core feature (Table 15 #1, p. 25). Unnamed echoes belong to NC-06, not here.
locate_by: a firing span is a sentence that names a specific external text, author, work, or figure from another work. Quote each instance with its location.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-21 (one cell per content type: literal / analog / n/a)
```

*(NC-06 Reference Explicitness → balanced mix, 37% / 16% / +21, also sits in this Table 16 group; see NC-06 above.)*

### Group: Human-elevated — Reader engagement (Table 16, p. 26)

```
id: NC-22
name: Fourth-Wall Permeability
taxonomy_id: `SIT_MET_004` — "Fourth-Wall Permeability" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 14 of 30 measured (importance 0.0067, 0.7% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Fourth-Wall Permeability" (o); Table 15 same (findings §B.5)
dimension: SIT (Situatedness)
response_type: ordinal
question: "To what extent does the story break the boundary between story-world and reader?" (Table 15 #6, p. 25)
taxonomy_question: "To what extent does the story break or blur the boundary between story-world and reader?" (taxonomy.json)
values (exact, taxonomy.json): 1-No fourth-wall breaking (reader never acknowledged); 2-Implicit winks or aside-like tonal gestures; 3-Occasional direct address or acknowledgement of reader; 4-Consistent or radical fourth-wall violations
 printed_option_names (paper): "1 (no breaking)–4 (radical violations)" (Table 15 #6, p. 25)
option_rows:
  - option: ordinal mean | human: 0.67 | ai: 0.39 | gap (printed): +0.28 | direction: Human-elevated
human_mean: 0.67 (mean over 0-based codes; Table 16, p. 26; reproduced from the release)
ai_mean: 0.39 (mean over 0-based codes; Table 16, p. 26; reproduced from the release)
encoding_note: **Encoding.** Table 16's 0.67 / 0.39 are means over **0-based ordinal codes** — this feature's four values coded 0–3 — and they reproduce exactly from the released per-story data (recomputed from storyscope_features.parquet; see the embedded recomputation notes). §4.1's "67% vs. 39%" (p. 7) is the same pair of means with the decimal point moved: a paper **erratum**, not a second measurement. Report the code mean together with the value labels below; never present it as a prevalence.
direction: Human-elevated
measured_baseline: matches the printed Table 16 values (human 0.67 / AI 0.39), reproduced exactly from the released per-story values
detection_method (authors', verbatim): Note any direct reader address, instructions, or acknowledgements. Also consider subtler asides that presume the reader's complicity. Judge overall pattern to place on the 1–4 scale.
scope: local
scope_basis: detection_method
scope_fields (Figure 8 match): nearest: narration.perspective.point_of_view [GLOBAL] (its "2nd person" option) (findings §F.2)
scope_reasoning: The detection_method says "Note any direct reader address, instructions, or acknowledgements" and then "Judge overall pattern" — the rung is read off located instances. Local. This card scored 10/10 exact in the fidelity study.
validated_on: fiction
fiction_detection: Look for moments where the text acknowledges that it is a story being read: asides to the reader, a narrator commenting on the act of narration, characters aware of the frame, or more radical violations. §4.1: "Humans break the fourth wall far more often (67% vs. 39%)" and "Human writing acknowledges its audience as a co-participant (e.g., an aside to 'you, dear reader'); AI writes as though no one is watching" (p. 7; findings §D.4). Breaking is the human side. Table 15's rungs run from 1 (no breaking) to 4 (radical violations); a text with no such moment sits at the bottom rung, the AI side.
locate_by: a firing span is any sentence in which the narration steps outside the story-world toward the reader or acknowledges its own telling. Quote each instance with its location; the number and severity of instances supports the rung.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-22 (one cell per content type: literal / analog / n/a)
```

```
id: NC-23
name: Direct Reader Address
taxonomy_id: `PER_POV_009` — "Frequency of direct address to the reader" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 7 of 30 measured (importance 0.0100, 1.0% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Direct Reader Address" (o); Table 15 "Frequency of Direct Reader Address" (findings §B.5)
dimension: PER (perspective)
response_type: ordinal
question: "How often does the text directly address the reader?" (Table 15 #2, p. 25)
taxonomy_question: "How often does the text directly address the reader ('you') as an addressee outside the story?" (taxonomy.json)
values (exact, taxonomy.json): never; occasional_asides; frequent_or_structural
 printed_option_names (paper): "never, occasional asides, frequent/structural" (Table 15 #2, p. 25)
option_rows:
  - option: ordinal mean | human: 0.28 | ai: 0.07 | gap (printed): +0.21 | direction: Human-elevated
human_mean: 0.28 (mean over 0-based codes; Table 16, p. 26; reproduced from the release)
ai_mean: 0.07 (mean over 0-based codes; Table 16, p. 26; reproduced from the release)
encoding_note: **Settled, as for NC-22.** Table 16's 0.28 / 0.07 are means over **0-based ordinal codes** — this feature's three values coded 0–2 — reproduced exactly from the released per-story data (recomputed from storyscope_features.parquet; see the embedded recomputation notes). §4.1's "28% vs. 7%" (p. 7) is the same pair of means with the decimal point moved: a paper **erratum**. Report the code mean with the value labels below, never as a prevalence.
related_fingerprint_note: FP-Human-3 "Narrator address mode → no direct address" (Table 17, p. 27) is a different feature in the SIT dimension, not this one (findings §B.5). The paper does not discuss the relation between the two.
direction: Human-elevated
measured_baseline: matches the printed Table 16 values (human 0.28 / AI 0.07), reproduced exactly from the released per-story values
detection_method (authors', verbatim): Identify vocative moments where the narrator speaks to an implied reader ('you may think', 'dear reader', 'let me tell you'). If absent, mark 'never'. If they appear a few times as stylistic flourishes, 'occasional_asides'. If this mode is pervasive or structurally central (e.g., 2nd-person essay voice), choose 'frequent_or_structural'.
scope: local
scope_basis: detection_method
scope_fields (Figure 8 match): nearest: narration.perspective.point_of_view [GLOBAL] (its "2nd person" option) (findings §F.2)
scope_reasoning: The detection_method identifies "vocative moments where the narrator speaks to an implied reader" and picks the rung by how many there are. Local. This card scored 10/10 exact in the fidelity study;.
validated_on: fiction
fiction_detection: Look for second-person address aimed at the reader rather than at a character: "you", "reader", imperatives directed outward, rhetorical questions posed to the audience. §4.1: humans "address the reader directly more frequently (28% vs. 7%)" (p. 7; findings §D.4). Address is the human side. Table 15's rungs are never; occasional asides; frequent/structural — the last for texts organized around address to the reader. Address to a character inside the story, or generic "you" in dialogue, does not count.
locate_by: a firing span is a sentence that speaks to the reader directly. Quote each instance with its location; the count places the text on the never / occasional / frequent rung.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-23 (one cell per content type: literal / analog / n/a)
```

### Group: Human-elevated — Temporal complexity (Table 16, p. 26)

```
id: NC-24
name: Depth of Recontextualization After Surprise
taxonomy_id: `REV_SUR_003` — "Depth of Recontextualization After Surprise" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 19 of 30 measured (importance 0.0022, 0.2% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Depth of Recontextualization After Surprise" (s); Table 15 same (findings §B.6)
dimension: REV (revelation)
response_type: scale
question: "How extensively does a revelation force reinterpretation of earlier scenes?" (Table 15 #4, p. 25; also Table 1, p. 4)
taxonomy_question: "How extensively does a key revelation force the reader to reinterpret earlier scenes or character behavior?" (taxonomy.json)
values (exact, taxonomy.json): 1_virtually_none; 2_small_number_of_moments_recolored; 3_several_scenes_gain_new_meaning; 4_most_of_the_story_reframed; 5_complete_re-reading_of_story_required
 printed_option_names (paper): "1 (none)–5 (complete re-reading)" (Table 15 #4, p. 25)
option_rows:
  - option: scale mean | human: 3.28 | ai: 2.95 | gap (printed): +0.34 | direction: Human-elevated
human_mean: 3.28
ai_mean: 2.95
direction: Human-elevated
measured_baseline: human 3.29 / AI 2.95 (`feature_cards.md` Part A); printed human 3.28 — last-digit rounding
detection_method (authors', verbatim): List which prior events read differently in light of the twist (e.g., new motives, hidden agendas). Count how much narrative ground is affected: only one or two incidents, or the majority of the plot. Choose the scale point that best matches the scope of this shift.
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): discourse.revelation.surprises [GLOBAL] ("what was revealed, and when?") (findings §F.2)
scope_reasoning: The detection_method counts "how much narrative ground is affected" by the twist — a proportion of the whole story. Global.
validated_on: fiction
fiction_detection: Identify the story's main revelation(s), then ask how much of what came before now reads differently. Low end (1, "none"): the revelation adds information but nothing earlier changes meaning; high end (5, "complete re-reading"): the whole prior text is reinterpreted. §4.1 speaks to this group only in general terms — humans use "nonlinear structure to delay key revelations" (p. 7; findings §D.2) — and gives no number for this feature. The human side is the high end: human mean 3.28 against AI 2.95 (Table 16, p. 26). This is the largest printed human-elevated gap among the scale features.
rating_note (measured): measured bias **−0.40**. Loosen: if several scenes gain new meaning that is already `3_several_scenes_gain_new_meaning`; reserve 4–5 for most of the story reframed or a complete re-reading required.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: the revelation passage; one or two earlier passages whose meaning it overturns.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-24 (one cell per content type: literal / analog / n/a)
```

```
id: NC-25
name: Chronological Discontinuity
taxonomy_id: `TMP_ORD_010` — "Degree of Chronological Discontinuity" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 13 of 30 measured (importance 0.0070, 0.7% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Chronological Discontinuity" (s); Table 15 "Degree of Chronological Discontinuity" (findings §B.6)
dimension: TMP (temporal structure)
response_type: scale (1–5 Likert)
question: "How often does the narrative jump across time?" (Table 15 #8, p. 25; also Table 1, p. 4)
taxonomy_question: "How often and how sharply does the narrative jump across time in ways that disrupt straightforward chronological flow?" (taxonomy.json)
values (exact, taxonomy.json): 1; 2; 3; 4; 5
 printed_option_names (paper): "1–5" (Table 15 #8, p. 25)
option_rows:
  - option: scale mean | human: 2.40 | ai: 2.12 | gap (printed): +0.28 | direction: Human-elevated
human_mean: 2.40
ai_mean: 2.12
direction: Human-elevated
measured_baseline: matches the printed Table 16 values (human 2.40 / AI 2.12)
detection_method (authors', verbatim): Assess the frequency and magnitude of temporal jumps (backwards or forwards) that interrupt linear progression. Rate 1 if the story moves smoothly forward with only small, well-signposted skips; 5 if it frequently leaps or shuffles in time, requiring active reconstruction.
style_boundary_note: the paper explicitly places this feature "on the non-style side" of its style rule (p. 18; findings §A.3, §B.6)
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): primary: discourse.temporal_order.structure [GLOBAL] ("choose linear, nonlinear, or mixed"); driving spans from discourse.temporal_order.time_jumps [LOCAL] ("ellipses or leaps in time/place, with scene references") (findings §F.2)
scope_reasoning: The detection_method assesses "the frequency and magnitude of temporal jumps" — a rate over the whole text, as ruled at Gate 2 . Global; the individual jumps are driving spans.
validated_on: fiction
fiction_detection: Mark every point where the narrative leaves its current moment for another — a flashback, a flash-forward, an ellipsis that skips time, a return. Low end (1): the story proceeds in one continuous chronological line; high end (5): the narrative jumps constantly. §4.1: "Humans use more time jumps, flashbacks and flash-forwards" and "a human mystery might open at the funeral and spiral backward through decades, while AI tells the same story from first clue to the grand reveal" (p. 7; findings §D.2); the prose gives no number for this feature. The human side is the high end: human mean 2.40 against AI 2.12 (Table 16, p. 26).
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: the sentence or scene break at which the narrative's present moment changes, with the cue that signals it ("years earlier", "by the time", a dated section heading); the largest leap in time; for contrast, the longest stretch told in continuous chronological order.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-25 (one cell per content type: literal / analog / n/a)
```

```
id: NC-26
name: Nonlinear Framing for Delayed Disclosure
taxonomy_id: `REV_DIS_003` — "Use of Nonlinear Framing for Delayed Disclosure" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 17 of 30 measured (importance 0.0047, 0.5% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Nonlinear Framing for Delayed Disclosure" (s); Table 15 same (findings §B.6)
dimension: REV (revelation)
response_type: scale
question: "To what extent does the story use time jumps to stage revelations?" (Table 15 #13, p. 25)
taxonomy_question: "To what extent does the story use flashbacks, frame narratives, or time jumps specifically to delay or stage revelations?" (taxonomy.json)
values (exact, taxonomy.json): 1_strictly_linear_no_frames; 2_minor_analepses_or_prologue_only; 3_regular_flashbacks_but_easy_to_track; 4_complex_frame_or_time_braid; 5_heavily_fragmented_time_with_reconstruction_required
 printed_option_names (paper): "1 (linear)–5 (heavily fragmented)" (Table 15 #13, p. 25)
option_rows:
  - option: scale mean | human: 1.96 | ai: 1.68 | gap (printed): +0.28 | direction: Human-elevated
human_mean: 1.96
ai_mean: 1.68
direction: Human-elevated
measured_baseline: matches the printed Table 16 values (human 1.96 / AI 1.68)
detection_method (authors', verbatim): Identify changes in narrative time: jumps backward/forward, stories told within stories, letters from the future, etc. Evaluate both frequency and structural importance of these devices in controlling when the reader learns key facts, then pick the best-fitting level.
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): discourse.revelation.surprises [GLOBAL] ("what was revealed, and when?"); discourse.temporal_order.structure [GLOBAL] (findings §F.2)
scope_reasoning: The detection_method evaluates "both frequency and structural importance of these devices in controlling when the reader learns key facts" — a whole-text strategy. Global.
validated_on: fiction
fiction_detection: Ask whether the story's departures from chronological order (NC-25 rates their frequency and cites its own driving spans) are doing revelatory work: withholding a cause, a fact, or an identity until a later-placed scene supplies it. Low end (1, "linear"): disclosures arrive in story order; high end (5, "heavily fragmented"): the order of telling is organized around delayed disclosure. §4.1: humans use "nonlinear structure to delay key revelations" (p. 7; findings §D.2); no number is given in prose. The human side is the high end: human mean 1.96 against AI 1.68 (Table 16, p. 26); both means are low on the scale, so this is a rarely-high feature for either source.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: a time jump that withholds a cause or fact; the later scene that supplies it; a framing device (a prologue set after the events, a retrospective opening) that delays disclosure.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-26 (one cell per content type: literal / analog / n/a)
```

```
id: NC-27
name: Anachrony Intensity
taxonomy_id: `TMP_ORD_002` — "Anachrony intensity" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 25 of 30 measured (importance 0.0013, 0.1% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Anachrony Intensity" (s); Table 15 same (findings §B.6)
dimension: TMP (temporal structure)
response_type: scale
question: "How heavily does the narrative rely on flashbacks or flash-forwards?" (Table 15 #10, p. 25)
taxonomy_question: "On a 1–5 scale, how heavily does the narrative rely on explicit flashbacks or flash-forwards as structural devices?" (taxonomy.json)
values (exact, taxonomy.json): 1_no_real_anachrony; 2_rare_or_minor_flashbacks; 3_regular_flashbacks_supporting_main_line; 4_frequent_or_extended_anachrony; 5_dominant_anachronic_structure
 printed_option_names (paper): "1 (absent)–5 (dominant anachronic)" (Table 15 #10, p. 25)
option_rows:
  - option: scale mean | human: 2.58 | ai: 2.31 | gap (printed): +0.27 | direction: Human-elevated
human_mean: 2.58
ai_mean: 2.31
direction: Human-elevated
measured_baseline: human 2.59 / AI 2.31 (`feature_cards.md` Part A); printed human 2.58 — last-digit rounding
detection_method (authors', verbatim): Count and evaluate the size of departures from the current present (multi-line or scene-level analepses/prolepses rather than quick references). Rate 1 if absent; 2 if there are only one or two brief backstory scenes; 3 if intermittent flashbacks are used but the main spine is forward; 4 if shifts are frequent or long; 5 if the story's logic depends on a heavily rearranged order (e.g., epistolary variations, nested timelines).
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): nearest: discourse.temporal_order.flashbacks [LOCAL] ("note which scenes are flashbacks"); discourse.temporal_order.structure [GLOBAL] (findings §F.2)
scope_reasoning: The detection_method counts departures from the present and evaluates their size, up to "the story's logic depends on a heavily rearranged order" — a whole-text weighting. Global.
validated_on: fiction
fiction_detection: Judge how much of the narrative is told out of its chronological place, and how much the story depends on that. Low end (1, "absent"): no flashbacks or flash-forwards; high end (5, "dominant anachronic"): most of the story is delivered through anachrony. §4.1: humans use more "flashbacks and flash-forwards" (p. 7; findings §D.2); no number is given in prose. The human side is the high end: human mean 2.58 against AI 2.31 (Table 16, p. 26).
rating_note (measured): measured bias **−0.40**. Loosen: intermittent flashbacks that support a forward main spine are already `3_regular_flashbacks_supporting_main_line`; `4_frequent_or_extended_anachrony` needs shifts that are frequent or long, and 5 needs the story's logic to depend on the rearrangement.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: the longest flashback or flash-forward; the point at which anachronic material becomes the main line of the telling; for contrast, the present-time frame it departs from.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-27 (one cell per content type: literal / analog / n/a)
```

### Group: Human-elevated — Narrative diversity (Table 16, p. 26)

```
id: NC-28
name: Location Variety Scope
taxonomy_id: `SET_LOC_011` — "Location Variety Scope" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 21 of 30 measured (importance 0.0017, 0.2% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Location Variety Scope" (o); Table 15 same (findings §B.7)
dimension: SET (setting)
response_type: ordinal
question: "How many distinct physical locales does the story inhabit?" (Table 15 #9, p. 25)
taxonomy_question: "How many distinct physical locales does the story meaningfully inhabit over its course?" (taxonomy.json)
values (exact, taxonomy.json): 1_single_primary_location; 2_two_to_four_locales; 3_many_within_one_region; 4_multi_regional_or_international; 5_multiworld_or_multiverse
 printed_option_names (paper): "single–multiworld" (Table 15 #9, p. 25). Table 16 values are "means over integer codes" (Table 16 caption, p. 26).
option_rows:
  - option: ordinal mean | human: 1.34 | ai: 1.08 | gap (printed): +0.26 | direction: Human-elevated
human_mean: 1.34
ai_mean: 1.08
direction: Human-elevated
measured_baseline: human 1.35 / AI 1.08 (`feature_cards.md` Part A); printed human 1.34 — last-digit rounding
detection_method (authors', verbatim): Count distinct settings that host full scenes (not just mentioned in passing). Single cottage or lab = 1. A house plus a school and one bar = 2–4. Numerous neighborhoods within the same city/valley = 3. Travel between cities/countries = 4. Regular shifts between planets, dimensions, or branched realities = 5.
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): nearest: story.setting.locations [LOCAL/GLOBAL] ("include scene-level and story-level locations, with scope noted") (findings §F.2)
scope_reasoning: The detection_method counts "distinct settings that host full scenes" across the story. Global.
validated_on: fiction
fiction_detection: List the distinct physical places the story actually inhabits (not merely mentions) and place the count on the ordinal from "single" (one locale) to "multiworld" (many, spanning distinct worlds or regions). §4.1: human stories "span more locations" (p. 8; findings §D.5); no number is given in prose. The human side is the higher rung: human mean 1.34 against AI 1.08 (Table 16, p. 26); both means sit near the bottom of the ordinal, so most stories from either source inhabit few locales.
rating_note (measured): measured bias **+0.70** — joint-largest over-rating. Tighten: count only settings that host a **full scene**, never places mentioned in passing. The authors' own worked examples: a single cottage or lab = `1_single_primary_location`; a house plus a school and one bar = `2_two_to_four_locales`; many neighbourhoods inside one city = `3_many_within_one_region`.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: the passages that first establish each distinct locale; the transition that moves the story to a new one.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-28 (one cell per content type: literal / analog / n/a)
```

```
id: NC-29
name: Dialogue-to-Narration Proportion
taxonomy_id: `PER_DIA_001` — "Dialogue-to-narration proportion" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 20 of 30 measured (importance 0.0020, 0.2% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Dialogue-to-Narration Proportion" (s); Table 15 same (findings §B.7)
dimension: PER (perspective)
response_type: scale
question: "What proportion of text is direct dialogue vs. narration?" (Table 15 #5, p. 25)
taxonomy_question: "Roughly what proportion of the text consists of direct dialogue (quoted speech) versus narration and description?" (taxonomy.json)
values (exact, taxonomy.json): 1_almost_no_dialogue_mostly_narration; 2_dialogue_is_rare_but_present; 3_balanced_mix_of_dialogue_and_narration; 4_dialogue_is_frequent_with_some_narrative_linking; 5_dialogue_dominates_with_minimal_narrative_exposition
 printed_option_names (paper): "1 (no dialogue)–5 (dialogue dominates)" (Table 15 #5, p. 25)
option_rows:
  - option: scale mean | human: 2.95 | ai: 2.70 | gap (printed): +0.24 | direction: Human-elevated
human_mean: 2.95
ai_mean: 2.70
direction: Human-elevated
measured_baseline: human 2.95 / AI 2.71 (`feature_cards.md` Part A); printed AI 2.70 — last-digit rounding
detection_method (authors', verbatim): Visually scan pages or sections and estimate the fraction of lines that are within dialogue markers versus those that are narrative; assign the closest category from nearly none (1) to almost all (5).
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): nearest: narration.perspective.dialogue_speakers [LOCAL] (findings §F.2)
scope_reasoning: The detection_method estimates "the fraction of lines that are within dialogue markers versus those that are narrative" — a proportion over the whole text. Global.
validated_on: fiction
fiction_detection: Estimate how much of the text is quoted speech against how much is narration, description, and interior. Low end (1, "no dialogue"); high end (5, "dialogue dominates"). §4.1: human stories "carry more dialogue relative to narration" (p. 8; findings §D.5); no number is given in prose. The human side is the high end: human mean 2.95 against AI 2.70 (Table 16, p. 26).
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: the longest run of uninterrupted dialogue; the longest run of narration without speech; a representative mixed passage.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-29 (one cell per content type: literal / analog / n/a)
```

```
id: NC-30
name: Moral Polarity
taxonomy_id: `PLT_MOR_002` — "Moral Polarity Toward Protagonist" (taxonomy.json)
tier: core (Table 16, p. 26)
importance_rank: 10 of 30 measured (importance 0.0096, 1.0% of total mass; `feature_cards.md` Part A). The 30 core cards together carry 25% of the classifier's importance mass.
paper_names: Table 16 "Moral Polarity → ambivalent/mixed"; Table 15 "Moral Polarity Toward Protagonist → ambivalent" (findings §B.7)
dimension: PLT (plot)
response_type: categorical
question: "Does the narrative frame the protagonist's choices as morally clear or ambiguous?" (Table 15 #11, p. 25)
taxonomy_question: "What evaluative stance does the narrative ultimately take toward its central protagonist?" (taxonomy.json)
values (exact, taxonomy.json): affirmative_heroic; critical_tragic_flaw; ambivalent_or_morally_mixed; subversive_or_antiheroic
 printed_option_names (paper): "clearly positive, ambivalent/mixed, clearly negative" (Table 15 #11, p. 25)
option_rows:
  - option: → ambivalent/mixed | human: 59% | ai: 38% | gap (printed): +21 | direction: Human-elevated
human_mean: 59% (prevalence of "ambivalent/mixed")
ai_mean: 38% (prevalence of "ambivalent/mixed")
direction: Human-elevated
measured_baseline: Table 16's row (ambivalent/mixed, human 59% / AI 38%) matches. Note: the **measured biggest-gap option for this feature is a different one** — `affirmative_heroic`, human 29% / AI 52% (gap −23, AI-elevated) — recomputed in `feature_cards.md` Part A. Both are the same four-value feature; Table 16 prints only the ambivalent row.
detection_method (authors', verbatim): Combine narrator commentary, plot consequences, and other characters' responses. If the protagonist's actions are framed as admirable and largely vindicated, mark affirmative_heroic. If they are sympathetically portrayed but ultimately condemned or destroyed by their own flaw, mark critical_tragic_flaw. If the story neither clearly endorses nor condemns them, or presents a complex blend, mark ambivalent_or_morally_mixed. If the narrative invites readers to side with someone conventionally villainous or to question all moral frames, mark subversive_or_antiheroic.
scope: global
scope_basis: detection_method
scope_fields (Figure 8 match): story.plot.moral [GLOBAL]; story.agents.major_characters[].role [GLOBAL] (findings §F.2)
scope_reasoning: The detection_method combines narrator commentary, plot consequences and other characters' responses into one stance toward the protagonist. Global.
validated_on: fiction
fiction_detection: Ask whether the text, taken whole, presents the protagonist's choices as clearly right, clearly wrong, or genuinely mixed — with costs, compromises, or unresolved judgment. §4.1: humans "present morally ambivalent protagonists more often (59% vs. 38%), resisting the pull to a narrow set of defaults" (p. 8; findings §D.5). Ambivalent/mixed is the human side (Table 16, p. 26). A protagonist whose choices are vindicated by the ending, or condemned by it, sits on the AI side of this row.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: the protagonist's most consequential choice; the passage where the narrative judges (or declines to judge) it; an ending that vindicates, condemns, or leaves the judgment open.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-30 (one cell per content type: literal / analog / n/a)
```

*(NC-17 Subplot Integration → thematically parallel, 42% / 21% / +22, and NC-07 Emotional Expression → explicit labels, 29% / 8% / +21, also sit in this Table 16 group; see NC-17 and NC-07 above.)*

### Extended cards (NC-31 … NC-40) — measured, not in Table 16

These ten sit in the paper's 304-feature taxonomy and in the narrative (non-style) feature set the classifier uses, but they are **not** among Table 16's 30 core features. The validation study found the 30 core cards carry only **25%** of the classifier's importance mass; these are the highest-weight uncovered features, and the rewrite experiment showed they are where non-flipping stories left value on the table. Every one carries `tier: extended (measured, not in Table 16)`. Their human/AI baselines were recomputed from the authors' released per-story data for the first time here; their weights come from our retrain on the released features (macro-F1 0.942). Never describe them as paper core features. (`feature_cards.md` §4; `feature_cards.md` Part B)

```
id: NC-31
name: Modes of conveying the central character's emotions
taxonomy_id: `AGENT_EMO_002` — "Modes of conveying the central character's emotions" (taxonomy.json)
tier: extended (measured, not in Table 16)
importance_rank: NC-31 carries measured importance 0.0410 (`feature_cards.md` Part B), which would place it in the top half of the 30 core cards. Not a paper core feature: it is a taxonomy feature in the classifier's narrative set whose human/AI separation and weight were measured by the validation study, never printed in Table 16.
dimension: AGENT
response_type: multi_select
question: "Through which modes are the central character's emotions mainly conveyed?" (taxonomy.json)
values (exact, taxonomy.json): explicit emotion words/labels; bodily sensations or physiology (e.g., heartbeat, nausea, trembling); metaphorical or environmental imagery reflecting mood; actions and choices as emotional indicators; dialogue content or tone revealing emotion
baseline (recomputed): biggest-gap option 'metaphorical or environmental imagery reflecting mood': human 50% / AI 89% (recomputed from storyscope_features.parquet; see the embedded recomputation notes and `feature_cards.md` Part B)
direction: AI-elevated on the biggest-gap option (metaphorical/environmental imagery)
moved_by_rewrites: 7/10 stories in the validation rewrite experiment
detection_method (authors', verbatim): Identify how the text signals the character's feelings. Mark each mode that regularly appears as a vehicle for emotional information, not just one-off instances.
scope: global
scope_basis: detection_method
scope_reasoning: The detection_method marks "each mode that regularly appears as a vehicle for emotional information, not just one-off instances" — a selection over the whole text. Global; the emotion renderings are driving spans.
validated_on: fiction
tier_caveat: not one of the paper's 30 core features and must never be described as one. Table 16 does not print it; its human/AI values here were recomputed from the authors' released per-story data, and its weight from our retrained classifier (macro-F1 0.942 on held-out dev).
fiction_detection: Collect every place the central character's feelings reach the page and mark each mode that recurs as a vehicle: named emotion words, bodily sensation, metaphorical or environmental imagery, action and choice, or the content and tone of speech. The measured separation sits on **metaphorical or environmental imagery reflecting mood: human 50% / AI 89%** (gap −39), a wider gap than most printed Table 16 rows. Two further options separate in the opposite direction: dialogue content or tone revealing emotion (human 69% / AI 49%) and bodily sensations or physiology (human 59% / AI 74%). This is the classifier's **#2 feature overall** by measured importance and moved on 7/10 stories in the rewrite experiment. It is close kin to NC-07 but is a multi-select over modes rather than a single dominant mode, and it is a separate taxonomy feature.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: a passage where weather, light or landscape carries the character's mood; a passage naming the feeling outright; an exchange whose tone conveys it.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-31 (one cell per content type: literal / analog / n/a)
```

```
id: NC-32
name: Dominant Narrative Tense
taxonomy_id: `TMP_DUR_011` — "Dominant Narrative Tense" (taxonomy.json)
tier: extended (measured, not in Table 16)
importance_rank: NC-32 carries measured importance 0.0172 (`feature_cards.md` Part B), which would place it in the top half of the 30 core cards. Not a paper core feature: it is a taxonomy feature in the classifier's narrative set whose human/AI separation and weight were measured by the validation study, never printed in Table 16.
dimension: TMP
response_type: categorical
question: "In what tense is the bulk of the narrative discourse written?" (taxonomy.json)
values (exact, taxonomy.json): past; present; future; mixed_or_shifted
baseline (recomputed): biggest-gap option 'past': human 86% / AI 96% (recomputed from storyscope_features.parquet; see the embedded recomputation notes and `feature_cards.md` Part B)
direction: AI-elevated on the biggest-gap option (past tense)
moved_by_rewrites: 0/10 stories in the validation rewrite experiment
detection_method (authors', verbatim): Identify the grammatical tense used in most narrative sentences (excluding dialogue). If tense shifts markedly between large sections, classify as 'mixed_or_shifted'.
scope: global
scope_basis: detection_method
scope_reasoning: The detection_method identifies "the grammatical tense used in most narrative sentences (excluding dialogue)" — a whole-text property. Global.
validated_on: fiction
tier_caveat: not one of the paper's 30 core features and must never be described as one. Table 16 does not print it; its human/AI values here were recomputed from the authors' released per-story data, and its weight from our retrained classifier (macro-F1 0.942 on held-out dev).
fiction_detection: Read the narration (not the dialogue) and name the tense the bulk of it is written in. Measured baseline: **past, human 86% / AI 96%** (gap −10); present runs the other way (human 13% / AI 4%), and mixed_or_shifted is rare in both (human 1% / AI 0%). Notable for `narrative-humanize`: this feature **moved on 0/10 stories** in the rewrite experiment, so it is currently untouched value — a present-tense narration is a real human-side signal that none of the structural passes produced.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: the opening narrative sentences; a passage where the tense visibly shifts, if any.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-32 (one cell per content type: literal / analog / n/a)
```

```
id: NC-33
name: Post-Climax Denouement Length
taxonomy_id: `PLT_MOR_007` — "Post-Climax Denouement Length" (taxonomy.json)
tier: extended (measured, not in Table 16)
importance_rank: NC-33 carries measured importance 0.0170 (`feature_cards.md` Part B), which would place it in the top half of the 30 core cards. Not a paper core feature: it is a taxonomy feature in the classifier's narrative set whose human/AI separation and weight were measured by the validation study, never printed in Table 16.
dimension: PLT
response_type: ordinal
question: "How extended is the narrative after the main climactic event?" (taxonomy.json)
values (exact, taxonomy.json): none_or_minimal (story ends immediately after climax); brief (a short scene or paragraph of aftermath); extended (multiple scenes/time jumps of aftermath/epilogue)
baseline (recomputed): human 1.02 / AI 1.47 (0-based code mean) (recomputed from storyscope_features.parquet; see the embedded recomputation notes and `feature_cards.md` Part B)
direction: AI-elevated (longer post-climax denouement)
moved_by_rewrites: 3/10 stories in the validation rewrite experiment
detection_method (authors', verbatim): Locate the main climax, then count how much textual space follows. Classify as immediate cutoff, quick wrap-up, or substantial epilogue/after-story.
scope: local
scope_basis: detection_method
scope_reasoning: The detection_method says "Locate the main climax, then count how much textual space follows" — the denouement is a bounded terminal region. Local.
validated_on: fiction
related_cells: NC-04, NC-18, NC-33, NC-40 — **not independent measurements**. In the analog studies these four collapsed onto a single observable, "does the text end on a generalising significance statement?", and shared a firing set across most of the documents they fired on at all (specific document ids deliberately omitted: naming them in a file every rater reads leaks answers into blind studies. Never count them as separate evidence, never aggregate them into a score or a lean count, and where they agree report **one** signal, not three or four. Matrix §0.8 already forbids aggregating measured cells; this line puts the same warning where a rater will see it.
tier_caveat: not one of the paper's 30 core features and must never be described as one. Table 16 does not print it; its human/AI values here were recomputed from the authors' released per-story data, and its weight from our retrained classifier (macro-F1 0.942 on held-out dev).
fiction_detection: Find the main climactic event, then measure what follows it: nothing, a short scene or paragraph of aftermath, or multiple scenes and time jumps of epilogue. Measured baseline **human 1.02 / AI 1.47** as a mean over 0-based codes of the three values, i.e. AI stories carry visibly longer after-matter. Moved on 3/10 stories in the rewrite experiment. Compare FP-Claude-3 (ending temporal scope → epilogue/flashforward), which is a per-source fingerprint rather than a human/AI separation.
locate_by: a firing span is the passage from the main climax to the end of the text. Quote the climax and the final scene with their locations; the distance between them is the rating.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-33 (one cell per content type: literal / analog / n/a)
```

```
id: NC-34
name: Density of figurative language in character depiction
taxonomy_id: `AGENT_ATTR_024` — "Density of figurative language in character depiction" (taxonomy.json)
tier: extended (measured, not in Table 16)
importance_rank: NC-34 carries measured importance 0.0160 (`feature_cards.md` Part B), which would place it in the top half of the 30 core cards. Not a paper core feature: it is a taxonomy feature in the classifier's narrative set whose human/AI separation and weight were measured by the validation study, never printed in Table 16.
dimension: AGENT
response_type: scale
question: "On a 1–5 scale, how dense is the use of figurative language (metaphors, similes, symbolic images) specifically in describing characters and their inner states?" (taxonomy.json)
values (exact, taxonomy.json): 1; 2; 3; 4; 5
baseline (recomputed): human 3.06 / AI 3.78 (1-5 mean) (recomputed from storyscope_features.parquet; see the embedded recomputation notes and `feature_cards.md` Part B)
direction: AI-elevated (denser figurative character depiction)
moved_by_rewrites: 3/10 stories in the validation rewrite experiment
detection_method (authors', verbatim): Sample several character-descriptive passages across stories. Count how often metaphors/similes are used per paragraph to render traits or states (e.g., 'built like a question', 'mind like a derailed carriage'). If almost none occur, assign 1. If occasional but not constant, assign 2. If roughly balanced between literal and figurative, assign 3. If figurative language is frequent, assign 4. If nearly every description leans on rich, inventive imagery, assign 5.
scope: global
scope_basis: detection_method
scope_reasoning: The detection_method samples "several character-descriptive passages" and counts how often figurative language renders traits or states — a density over the text. Global.
validated_on: fiction
tier_caveat: not one of the paper's 30 core features and must never be described as one. Table 16 does not print it; its human/AI values here were recomputed from the authors' released per-story data, and its weight from our retrained classifier (macro-F1 0.942 on held-out dev).
fiction_detection: Read the passages that describe characters and their inner states, and judge how often metaphor, simile and symbolic image do the work rather than literal description. The authors' anchors: almost none = 1; occasional but not constant = 2; roughly balanced literal and figurative = 3; frequent = 4; nearly every description leaning on rich inventive imagery = 5. Measured baseline **human 3.06 / AI 3.78** (recomputed; `feature_cards.md` Part B). The separation is wider than that of any 1–5 scale row printed in Table 16, though the paper prints no gap for this feature because it is not a Table 16 row. Moved on 3/10 stories. This overlaps the paper's style boundary in spirit but sits in the narrative (non-style) set the classifier uses.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: the most figurative character description in the text; a plainly literal one, for contrast.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-34 (one cell per content type: literal / analog / n/a)
```

```
id: NC-35
name: Overall Revelation Pacing Pattern
taxonomy_id: `REV_DIS_001` — "Overall Revelation Pacing Pattern" (taxonomy.json)
tier: extended (measured, not in Table 16)
importance_rank: NC-35 carries measured importance 0.0124 (`feature_cards.md` Part B), which would place it in the top half of the 30 core cards. Not a paper core feature: it is a taxonomy feature in the classifier's narrative set whose human/AI separation and weight were measured by the validation study, never printed in Table 16.
dimension: REV
response_type: categorical
question: "How are major pieces of explanatory information (about stakes, backstory, ontology) distributed across the story?" (taxonomy.json)
values (exact, taxonomy.json): front_loaded_exposition_early; evenly_layered_throughout; back_loaded_key_revelations_near_end; episodic_or_serial_pulses_of_revelation
baseline (recomputed): biggest-gap option 'back_loaded_key_revelations_near_end': human 66% / AI 48% (recomputed from storyscope_features.parquet; see the embedded recomputation notes and `feature_cards.md` Part B)
direction: Human-elevated on the biggest-gap option (back-loaded revelation)
moved_by_rewrites: 5/10 stories in the validation rewrite experiment
detection_method (authors', verbatim): Mark where the reader learns the most about the underlying situation. If the bulk arrives in the opening scenes, choose front_loaded; if there is a steady drip, choose evenly_layered; if the biggest clarifications cluster in the last quarter, choose back_loaded; if the story is organized into distinct cases/episodes each with its own reveal, choose episodic_or_serial.
scope: global
scope_basis: detection_method
scope_reasoning: The detection_method marks "where the reader learns the most about the underlying situation" across the story — a distribution. Global.
validated_on: fiction
tier_caveat: not one of the paper's 30 core features and must never be described as one. Table 16 does not print it; its human/AI values here were recomputed from the authors' released per-story data, and its weight from our retrained classifier (macro-F1 0.942 on held-out dev).
fiction_detection: Map where the story's explanatory information — stakes, backstory, ontology — actually lands, then choose the pattern. Measured baseline: **back_loaded_key_revelations_near_end, human 66% / AI 48%** (gap +18, human-elevated); evenly_layered_throughout runs the other way (human 32% / AI 49%), and front_loaded is rare in both (human 1% / AI 3%). Moved on 5/10 stories. This is the same feature family as FP-Human-4 (overall revelation pacing → back-loaded), which Table 17 lists as a human fingerprint without values; the baseline here is recomputed and gives it numbers for the first time.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: the passage carrying the largest single clarification; an early passage that either does or does not front-load the situation.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-35 (one cell per content type: literal / analog / n/a)
```

```
id: NC-36
name: Explicit Enumeration vs Enacted Interaction
taxonomy_id: `SOC_REL_004` — "Explicit Enumeration vs Enacted Interaction" (taxonomy.json)
tier: extended (measured, not in Table 16)
importance_rank: NC-36 carries measured importance 0.0124 (`feature_cards.md` Part B), which would place it in the top half of the 30 core cards. Not a paper core feature: it is a taxonomy feature in the classifier's narrative set whose human/AI separation and weight were measured by the validation study, never printed in Table 16.
dimension: SOC
response_type: categorical
question: "Are relationships mainly described through list-like summaries and labels, or through dramatized scene interactions?" (taxonomy.json)
values (exact, taxonomy.json): predominantly_enumerated_or_summarized; mixed_enumerated_and_scenic; predominantly_enacted_in_scenes
baseline (recomputed): biggest-gap option 'predominantly_enacted_in_scenes': human 75% / AI 83% (recomputed from storyscope_features.parquet; see the embedded recomputation notes and `feature_cards.md` Part B)
direction: AI-elevated on the biggest-gap option (predominantly enacted in scenes)
moved_by_rewrites: 2/10 stories in the validation rewrite experiment
detection_method (authors', verbatim): Note whether the narrator frequently pauses to summarize the network ('X is Y's aunt; Z works for W…') versus mostly showing relationships via dialogue and action in scenes. Classify based on which mode dominates the text.
scope: global
scope_basis: detection_method
scope_reasoning: The detection_method classifies "based on which mode dominates the text". Global.
validated_on: fiction
tier_caveat: not one of the paper's 30 core features and must never be described as one. Table 16 does not print it; its human/AI values here were recomputed from the authors' released per-story data, and its weight from our retrained classifier (macro-F1 0.942 on held-out dev).
fiction_detection: Ask how the reader learns who is who: through narratorial summary of the network ("X is Y's aunt; Z works for W"), or through relationships dramatized in scene. Measured baseline: **predominantly_enacted_in_scenes, human 75% / AI 83%** (gap −8); mixed_enumerated_and_scenic runs human-ward (human 23% / AI 17%), and predominantly_enumerated_or_summarized is rare in both (human 2% / AI 1%). Moved on 2/10 stories. The separation here is smaller than most Table 16 rows; its value to the classifier comes from its weight, not from a wide human/AI gap.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: a narratorial passage summarizing relationships; a scene in which a relationship is enacted through dialogue and action.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-36 (one cell per content type: literal / analog / n/a)
```

```
id: NC-37
name: Naming practice for the central character
taxonomy_id: `AGENT_ID_005` — "Naming practice for the central character" (taxonomy.json)
tier: extended (measured, not in Table 16)
importance_rank: NC-37 carries measured importance 0.0123 (`feature_cards.md` Part B), which would place it in the top half of the 30 core cards. Not a paper core feature: it is a taxonomy feature in the classifier's narrative set whose human/AI separation and weight were measured by the validation study, never printed in Table 16.
dimension: AGENT
response_type: categorical
question: "How is the central character primarily identified in the narration?" (taxonomy.json)
values (exact, taxonomy.json): named personal name (e.g., 'Maria', 'Leon'); unnamed but specific 'I/he/she' narrator; role/descriptor used as main identifier (e.g., 'the mother', 'the Anchor'); multiple or shifting identifiers (aliases, titles, different names in different contexts)
baseline (recomputed): biggest-gap option 'named personal name (e.g., 'Maria', 'Leon')': human 70% / AI 80% (recomputed from storyscope_features.parquet; see the embedded recomputation notes and `feature_cards.md` Part B)
direction: AI-elevated on the biggest-gap option (named personal name)
moved_by_rewrites: 0/10 stories in the validation rewrite experiment
detection_method (authors', verbatim): For the story's most central figure, note how they are usually referred to. Choose the single label that best describes the dominant practice over the whole text.
scope: global
scope_basis: detection_method
scope_reasoning: The detection_method chooses "the single label that best describes the dominant practice over the whole text". Global.
validated_on: fiction
tier_caveat: not one of the paper's 30 core features and must never be described as one. Table 16 does not print it; its human/AI values here were recomputed from the authors' released per-story data, and its weight from our retrained classifier (macro-F1 0.942 on held-out dev).
fiction_detection: Note how the story's most central figure is usually referred to and pick the dominant practice. Measured baseline: **named personal name, human 70% / AI 80%** (gap −10); an unnamed but specific I/he/she narrator runs human-ward (human 23% / AI 17%), as does a role or descriptor used as the main identifier (human 4% / AI 2%). Moved on **0/10** stories in the rewrite experiment — like NC-32, untouched value. Compare FP-Gemini-4 (naming practice → named personal name), a per-source fingerprint on the same taxonomy family.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: the first identification of the central character; a later passage confirming the habitual form of reference.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-37 (one cell per content type: literal / analog / n/a)
```

```
id: NC-38
name: Primary Genre Category
taxonomy_id: `SIT_GEN_001` — "Primary Genre Category" (taxonomy.json)
tier: extended (measured, not in Table 16)
importance_rank: NC-38 carries measured importance 0.0121 (`feature_cards.md` Part B), which would place it in the top half of the 30 core cards. Not a paper core feature: it is a taxonomy feature in the classifier's narrative set whose human/AI separation and weight were measured by the validation study, never printed in Table 16.
dimension: SIT
response_type: categorical
question: "What is the story's primary genre classification based on its dominant setting, plot devices, and reader expectations?" (taxonomy.json)
values (exact, taxonomy.json): realist_contemporary; historical_realist; science_fiction; fantasy; horror; mystery_detective; thriller_suspense; romance; comedy_satire; myth_fable_fairy_tale; literary_realist_fiction; multi_genre_blend; nonfictional_mode_pastiche_or_essayistic
baseline (recomputed): biggest-gap option 'historical_realist': human 8% / AI 15% (recomputed from storyscope_features.parquet; see the embedded recomputation notes and `feature_cards.md` Part B)
direction: AI-elevated on the biggest-gap option (historical_realist)
moved_by_rewrites: 2/10 stories in the validation rewrite experiment
detection_method (authors', verbatim): Identify the dominant setting (realistic vs speculative), core plot engine (investigation, romance, quest, etc.), and conventional markers (magic, technology, monsters, clues) to assign the genre that best matches the story's main reader contract.
scope: global
scope_basis: detection_method
scope_reasoning: The detection_method assigns "the genre that best matches the story's main reader contract" from dominant setting, plot engine and conventional markers — a whole-text classification. Global.
validated_on: fiction
tier_caveat: not one of the paper's 30 core features and must never be described as one. Table 16 does not print it; its human/AI values here were recomputed from the authors' released per-story data, and its weight from our retrained classifier (macro-F1 0.942 on held-out dev).
fiction_detection: Name the story's primary genre from its dominant setting, core plot engine and conventional markers. Measured baseline on the biggest-gap option: **historical_realist, human 8% / AI 15%** (gap −6); realist_contemporary is near-even (human 27% / AI 25%) and comedy_satire slightly human-side (human 4% / AI 1%). Moved on 2/10 stories. The gap is small on any single option; the feature earns its place by classifier weight across thirteen values, not by one separating option. Note the paper's own topic analysis found no significant topic-wise differences in detection (H = 4.69, p = 0.46, p. 22) — genre as a *feature* is not the same thing as genre as a *confound*.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: the passage establishing setting and period; the passage that sets the plot engine running.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-38 (one cell per content type: literal / analog / n/a)
```

```
id: NC-39
name: Breadth of focalization across characters
taxonomy_id: `PER_FOC_002` — "Breadth of focalization across characters" (taxonomy.json)
tier: extended (measured, not in Table 16)
importance_rank: NC-39 carries measured importance 0.0116 (`feature_cards.md` Part B), which would place it in the top half of the 30 core cards. Not a paper core feature: it is a taxonomy feature in the classifier's narrative set whose human/AI separation and weight were measured by the validation study, never printed in Table 16.
dimension: PER
response_type: categorical
question: "Across the story, how many distinct characters' inner experiences does the narration give direct access to?" (taxonomy.json)
values (exact, taxonomy.json): single_focal_character; dual_focal_characters; multiple_small_set_3_to_5; many_or_godseye
baseline (recomputed): biggest-gap option 'single_focal_character': human 79% / AI 91% (recomputed from storyscope_features.parquet; see the embedded recomputation notes and `feature_cards.md` Part B)
direction: AI-elevated on the biggest-gap option (single focal character)
moved_by_rewrites: 2/10 stories in the validation rewrite experiment
detection_method (authors', verbatim): List all characters whose thoughts or felt experience are directly rendered (beyond the narrator reporting guesses). Count distinct centers: 1 = single_focal_character; 2 = dual_focal_characters; 3–5 distinct minds = multiple_small_set_3_to_5; more than 5 or a sense of generalised human minds = many_or_godseye.
scope: global
scope_basis: detection_method
scope_reasoning: The detection_method lists "all characters whose thoughts or felt experience are directly rendered" across the story and counts distinct centres. Global.
validated_on: fiction
tier_caveat: not one of the paper's 30 core features and must never be described as one. Table 16 does not print it; its human/AI values here were recomputed from the authors' released per-story data, and its weight from our retrained classifier (macro-F1 0.942 on held-out dev).
fiction_detection: List every character whose thoughts or felt experience the narration renders directly — not the narrator guessing from outside — and count distinct centres: 1, 2, 3–5, or more than five. Measured baseline: **single_focal_character, human 79% / AI 91%** (gap −12); multiple_small_set_3_to_5 is human-side (human 11% / AI 4%), as is dual_focal_characters (human 7% / AI 5%). Moved on 2/10 stories. This is the same taxonomy family as FP-Human-2 (breadth of focalization → single focal), which Table 17 lists without values; note that the fingerprint marks single-focal as *human*-distinctive in the six-way task while this human/AI baseline puts single-focal on the AI side — different tasks, and the paper does not relate them.
locate_by: rate once for the document, then cite the 2–3 spans that most drive the rating. Driving spans are typically: the first interior rendering of each distinct focal character.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-39 (one cell per content type: literal / analog / n/a)
```

```
id: NC-40
name: Ending temporal scope
taxonomy_id: `TMP_ORD_014` — "Ending temporal scope" (taxonomy.json)
tier: extended (measured, not in Table 16)
importance_rank: NC-40 carries measured importance 0.0103 (`feature_cards.md` Part B), which would place it in the top half of the 30 core cards. Not a paper core feature: it is a taxonomy feature in the classifier's narrative set whose human/AI separation and weight were measured by the validation study, never printed in Table 16.
dimension: TMP
response_type: categorical
question: "Relative to the main line of events, where in story time does the narrative end?" (taxonomy.json)
values (exact, taxonomy.json): ends_at_or_just_after_main_climax_no_forward_jump; ends_with_brief_forward_epilogue_or_flashforward; ends_before_external_resolution_mid_arc_or_cliffhanger; ends_at_distant_future_or_far_past_vantage_summarizing_long_term
baseline (recomputed): biggest-gap option 'ends_at_or_just_after_main_climax_no_forward_jump': human 69% / AI 51% (recomputed from storyscope_features.parquet; see the embedded recomputation notes and `feature_cards.md` Part B)
direction: Human-elevated on the biggest-gap option (ends at or just after the climax)
moved_by_rewrites: 3/10 stories in the validation rewrite experiment
detection_method (authors', verbatim): Identify the last narrated scene and determine whether it coincides with the primary external resolution, follows it by a short look ahead, stops earlier while major external consequences are offstage, or jumps far ahead (or back) to survey long-term outcomes from another era.
scope: local
scope_basis: detection_method
scope_reasoning: The detection_method says "Identify the last narrated scene" and place it relative to the main line of events — a bounded terminal passage. Local.
validated_on: fiction
related_cells: NC-04, NC-18, NC-33, NC-40 — **not independent measurements**. In the analog studies these four collapsed onto a single observable, "does the text end on a generalising significance statement?", and shared a firing set across most of the documents they fired on at all (specific document ids deliberately omitted: naming them in a file every rater reads leaks answers into blind studies. Never count them as separate evidence, never aggregate them into a score or a lean count, and where they agree report **one** signal, not three or four. Matrix §0.8 already forbids aggregating measured cells; this line puts the same warning where a rater will see it.
boundary_vs_NC-18: **What this card claims about a closing span:** *where the narration stops relative to the main climax* — at or just after it with no forward jump, after a brief forward epilogue, before the external resolution, or from a distant vantage summarising long-term outcomes. It claims **nothing about how the conflict was settled**; that is NC-18's question alone. Apply both by locating the climax first, then answering the two questions separately. A single closing sentence can carry both properties and can legitimately place the same document on opposite sides — measured case (identifier and span deliberately omitted): NC-18 read the close as a concrete external act, while this card read where the text stops — on a forward reassurance — and scored a forward-looking flourish. Both were correct applications of their own rules. **Governing rule:** where one sentence is the only evidence for both cards, it is one observable — report it once, cite the span once, and never sum NC-40 with NC-18.
tier_caveat: not one of the paper's 30 core features and must never be described as one. Table 16 does not print it; its human/AI values here were recomputed from the authors' released per-story data, and its weight from our retrained classifier (macro-F1 0.942 on held-out dev).
fiction_detection: Read the final scene and place it against the main line of events. Measured baseline: **ends_at_or_just_after_main_climax_no_forward_jump, human 69% / AI 51%** (gap +18, human-elevated); ending at a distant future or far past vantage that summarizes long-term outcomes runs AI-ward (human 25% / AI 38%), as does a brief forward epilogue (human 4% / AI 7%). Moved on 3/10 stories. Same family as FP-Claude-3 and NC-33: a long look forward after the climax is an AI-side ending shape on all three measures.
locate_by: a firing span is the final narrated scene, plus the climax it is measured against. Quote both with their locations.
content_type_behavior: see content_type_matrix.md, section CT-xx, row NC-40 (one cell per content type: literal / analog / n/a)
```

### Core and extended scope tally (NC-01 … NC-40)

| Scope | Cards | Count |
|---|---|---|
| global | NC-01, NC-02, NC-03, NC-05, NC-06, NC-07, NC-08, NC-09, NC-10, NC-11, NC-12, NC-13, NC-14, NC-17, NC-20, NC-24, NC-25, NC-26, NC-27, NC-28, NC-29, NC-30, NC-31, NC-32, NC-34, NC-35, NC-36, NC-37, NC-38, NC-39 | 30 |
| local | NC-04, NC-15, NC-16, NC-18, NC-19, NC-21, NC-22, NC-23, NC-33, NC-40 | 10 |

| Basis | Cards | Count |
|---|---|---|
| detection_method | all 40 (NC-01 … NC-40) | 40 |

Every scope on these 40 cards rests on the authors' own `detection_method` text (`feature_cards.md` for NC-01 … NC-30, `taxonomy.json` for NC-31 … NC-40). It remains a reading of the authors' description, not a scope tag they published, and their pipeline never passes that text to the assigner. NC-05 is global because the detection_method selects functions "consistently prominent across the text". Gate 2 rulings (NC-10) and (NC-25) are preserved and independently confirmed by the detection_method wording.

---

## Part 2 — Fingerprint cards (FP-<source>-<n>)

Source: Table 17 (p. 27) and §5 prose (pp. 8–9) as reproduced in findings §C. Table 17 caption: "Per-source fingerprint features, showing the top 5 (or all, if fewer) ranked by uniqueness ratio. SHAP = mean class SHAP importance; Uniq. = uniqueness ratio vs. next-best class." (findings §C.1). Fingerprints are "identified from the six-way multiclass task. A feature qualifies when its SHAP importance is concentrated in a single source class, and that source's observed values visibly differ from others" (p. 5; findings §A.6).

Per-source counts (p. 25; the only fingerprint totals any file cites, ): Human 32, Claude 26, GPT 11, Gemini 11, DeepSeek 7, Kimi 3. Per-class F1 of the narrative-only six-way model (Table 12, p. 22; findings §C.4): Human 0.89, Claude 0.77, GPT 0.73, Gemini 0.60, DeepSeek 0.57, Kimi 0.55.

Five rows print no "→ option" (FP-Claude-1, FP-Claude-2, FP-DeepSeek-1, FP-DeepSeek-3, FP-DeepSeek-5); they are scale/ordinal features whose whole distribution is the fingerprint, and Table 17 prints no per-source mean or direction for them (findings §C.2). Those cards carry `value: none printed in paper` .

### Source: Human — 32 fingerprints (p. 25); narrative per-class F1 0.89 (Table 12, p. 22)

Section 5 gives no dedicated prose paragraph for human fingerprints; the human profile is characterized through §4.1 and Table 17 rows only (findings §C.3; E1 worklog item 11). Human `fiction_detection` fields below therefore quote no §5 percentages.

```
id: FP-Human-1
source_model: Human
feature: Character introduction → in-dialogue
dimension: AGENT
shap: 0.110
uniqueness_ratio: 21.4
value: option "in-dialogue" (Table 17, p. 27)
scope: local
scope_basis: inferred
scope_fields: nearest: story.agents.major_characters[].role [GLOBAL] (findings §F.2)
scope_reasoning: Same feature as core NC-16; the introduction of a character is a single locatable passage, so the [GLOBAL] agents fields do not settle it. Scope local.
validated_on: fiction
fiction_detection: Locate the first appearance of the central character and check whether the reader meets them through a line of dialogue rather than description, action, thought, or report. This is another option of the same feature as NC-16 (whose AI-elevated option is external description, 52% vs 30%, Table 16, p. 26). Table 17 ranks it first among human fingerprints by uniqueness ratio (21.4); §5 offers no prose or percentage for it.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-Human-2
source_model: Human
feature: Breadth of focalization → single focal
dimension: PER
shap: 0.083
uniqueness_ratio: 15.7
value: option "single focal" (Table 17, p. 27)
scope: global
scope_basis: inferred
scope_fields: nearest: narration.perspective.focalization [LOCAL] ("whose perspective we occupy by scene/section") (findings §F.2)
scope_reasoning: The template records focalization per scene ([LOCAL]), but breadth is the count of distinct focal characters across the whole story — an aggregate. Scope global; the scenes that establish each focal character are the driving spans.
validated_on: fiction
fiction_detection: Track whose perspective the narration occupies scene by scene; the option fires when the whole story stays with one focal character. Table 17 gives uniqueness 15.7, second among human fingerprints; §5 offers no prose or percentage for it.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-Human-3
source_model: Human
feature: Narrator address mode → no direct address
dimension: SIT
shap: 0.069
uniqueness_ratio: 12.6
value: option "no direct address" (Table 17, p. 27)
scope: global
scope_basis: inferred
scope_fields: nearest: narration.perspective.point_of_view [GLOBAL] (findings §F.2)
scope_reasoning: "No direct address" is an absence across the entire text and cannot be pinned to a line. Scope global; where address does occur, those spans (the same spans NC-23 lists) are the driving evidence against the option.
validated_on: fiction
fiction_detection: Read the whole text for any address to the reader; the option fires when there is none. Note the tension the paper leaves uncommented: core NC-23 shows humans addressing the reader more often (Table 16: 0.28 vs 0.07; §4.1: 28% vs 7%, p. 7), while this human fingerprint is the absence of address. The findings file records FP-Human-3 as a different feature in the SIT dimension from NC-23 (findings §B.5); the paper does not discuss the relation. §5 offers no prose or percentage for it.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-Human-4
source_model: Human
feature: Overall revelation pacing → back-loaded
dimension: REV
shap: 0.094
uniqueness_ratio: 7.7
value: option "back-loaded" (Table 17, p. 27)
scope: global
scope_basis: figure-8
scope_fields: discourse.revelation.surprises [GLOBAL] ("what was revealed, and when?") (findings §F.2)
scope_reasoning: "Overall" pacing is the distribution of revelations across the whole text; the surprises field is [GLOBAL].
validated_on: fiction
fiction_detection: Map where the story's key disclosures fall; the option fires when most of them cluster toward the end. Table 17 gives uniqueness 7.7; §5 offers no prose or percentage for it. Compare DeepSeek, which §5 says "front-loads crucial context that other sources leave until later" (p. 9; findings §C.3).
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-Human-5
source_model: Human
feature: Literary ambition → crossover genre
dimension: SIT
shap: 0.116
uniqueness_ratio: 6.8
value: option "crossover genre" (Table 17, p. 27)
scope: global
scope_basis: inferred
scope_fields: nearest: story.plot.narrative_archetype [GLOBAL]; story.events.narrative_schema [GLOBAL] (findings §F.2)
scope_reasoning: No template field asks about literary ambition or genre positioning; the judgment is of the whole work. Scope global.
validated_on: fiction
fiction_detection: Judge the story's positioning as a whole: the option fires when it straddles genres or sits between literary and genre modes rather than inside one. Table 17 gives the highest SHAP among the five human rows (0.116) with uniqueness 6.8; §5 offers no prose or percentage for it.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

### Source: Claude (Claude Sonnet 4.6) — 26 fingerprints (p. 25); narrative per-class F1 0.77 (Table 12, p. 22)

§5, "Claude keeps it cool.": "Claude has the most distinctive narrative profile of the five LLMs. Its stories are defined by restraint: event intensity escalates less than in any other source, and narrative voice is the most uniform." "It favors epilogues and avoids dream sequences, producing careful, consistent stories that favor quiet endings over 'avalanche' endings." (p. 9; findings §C.3) The 62% figure in §5 ("reverent/continuist approach to literary tradition ... 62% of Claude stories vs. 39–56% across other sources", p. 9) attaches to a literary-tradition feature that is not one of the five Table 17 Claude rows; it is not placed on any card below.

```
id: FP-Claude-1
source_model: Claude
feature: Strength of event escalation
dimension: EVT
shap: 0.402
uniqueness_ratio: 22.4
value: none printed in paper (no "→ option"; the whole distribution is the fingerprint — Table 17, p. 27)
scope: global
scope_basis: figure-8
scope_fields: story.plot.plot_arc [GLOBAL] ("rising action -> climax -> falling action"); story.events.sequence [LOCAL] supplies the beats (findings §F.2)
scope_reasoning: Escalation is the trajectory of intensity across the ordered beats of the whole story; the arc field is [GLOBAL].
validated_on: fiction
fiction_detection: Read the sequence of events for how sharply intensity rises toward the climax. §5: Claude stories are "defined by restraint: event intensity escalates less than in any other source" and "favor quiet endings over 'avalanche' endings" (p. 9; findings §C.3); the abstract summarizes "Claude produces notably flat event escalation" (p. 1). Flat escalation is the Claude side. Table 17 gives the highest uniqueness ratio in the whole table (22.4) with SHAP 0.402; no per-source mean is printed.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-Claude-2
source_model: Claude
feature: Event-type diversity
dimension: EVT
shap: 0.491
uniqueness_ratio: 10.7
value: none printed in paper (no "→ option" — Table 17, p. 27)
scope: global
scope_basis: inferred
scope_fields: nearest: story.events.sequence [LOCAL] ("ordered list of concrete beat-level events") (findings §F.2)
scope_reasoning: The sequence field lists events per beat ([LOCAL]), but diversity is a property of the whole set of events. Scope global.
validated_on: fiction
fiction_detection: List the kinds of event the story contains (confrontation, journey, discovery, loss, ritual, and so on) and judge their variety. §5 names no direction or number for this feature beyond the general characterization of Claude stories as "careful, consistent" (p. 9; findings §C.3). Table 17 gives the highest SHAP in the whole table (0.491) with uniqueness 10.7; no per-source mean or direction is printed, so the card cannot say whether Claude is high or low.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-Claude-3
source_model: Claude
feature: Ending temporal scope → epilogue/flashforward
dimension: TMP
shap: 0.096
uniqueness_ratio: 8.9
value: option "epilogue/flashforward" (Table 17, p. 27)
scope: local
scope_basis: figure-8
scope_fields: discourse.temporal_order.time_jumps [LOCAL] ("ellipses or leaps in time/place, with scene references") (findings §F.2)
scope_reasoning: An epilogue or closing flash-forward is a bounded terminal passage marked by a leap in time; the template records such leaps with scene references ([LOCAL]). Scope local.
validated_on: fiction
fiction_detection: Read the ending for a section set after the main action — an epilogue, a jump forward in time, a coda. §5: Claude "favors epilogues" (p. 9); Figure 1's example observation is "C stories write epilogues more often" (Fig. 1, p. 2; findings §C.3). No percentage is printed. Table 17 gives uniqueness 8.9.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-Claude-4
source_model: Claude
feature: Dreams/visions as temporal distortion → no
dimension: TMP
shap: 0.116
uniqueness_ratio: 7.7
value: option "no" (Table 17, p. 27)
scope: global
scope_basis: inferred
scope_fields: nearest: discourse.temporal_order.flashbacks [LOCAL]; time_jumps [LOCAL] (findings §F.2)
scope_reasoning: The option is "no" — an absence across the entire text — which cannot be pinned to a line; the [LOCAL] fields would locate dream sequences only where they exist. Scope global.
validated_on: fiction
fiction_detection: Read the whole text for dream sequences, visions, or hallucinations used to break or bend time; the option fires when there are none. §5: Claude "avoids dream sequences" (p. 9; findings §C.3). No percentage is printed. Table 17 gives uniqueness 7.7.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-Claude-5
source_model: Claude
feature: Setting mood → uncanny/haunted
dimension: SET
shap: 0.059
uniqueness_ratio: 4.6
value: option "uncanny/haunted" (Table 17, p. 27)
scope: global
scope_basis: figure-8
scope_fields: story.setting.atmosphere [GLOBAL] (findings §F.2)
scope_reasoning: Mood of setting is the template's atmosphere field, tagged [GLOBAL].
validated_on: fiction
fiction_detection: Judge the prevailing atmosphere of the story's settings; the option fires when it is uncanny or haunted. §5 does not mention this feature for Claude; the only §5 setting-mood number is Gemini's "88% tagged bleak and oppressive" (p. 9; findings §C.3), a different option for a different source. Table 17 gives the lowest uniqueness of the five Claude rows (4.6).
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

### Source: GPT (GPT-5.4) — 11 fingerprints (p. 25); narrative per-class F1 0.73 (Table 12, p. 22)

§5, "GPT likes to gossip.": "GPT centers on socially-oriented storytelling: gossip and rumor as a plot mechanism (64% vs. 44–55% for other sources), a tendency to frame stories as reflections on events from years or decades ago, and ensemble-heavy social networks matching human levels." "GPT subverts expectations more than other AI (41% vs. 27–36%) and leaves reconciliations ambiguous." (p. 9; findings §C.3)

```
id: FP-GPT-1
source_model: GPT
feature: Role of gossip and rumor → salient
dimension: SOC
shap: 0.200
uniqueness_ratio: 22.1
value: option "salient" (Table 17, p. 27)
scope: global
scope_basis: inferred
scope_fields: nearest: story.social_network.relationships [GLOBAL] (findings §F.2)
scope_reasoning: Salience of gossip as a plot mechanism is a whole-story judgment; the relationships field is [GLOBAL] but concerns bonds, not gossip, so the wording decides. Scope global; the gossip scenes are the driving spans.
validated_on: fiction
fiction_detection: Look for rumor, hearsay, or overheard talk that moves the plot or shapes what characters believe. §5: "gossip and rumor as a plot mechanism (64% vs. 44–55% for other sources)" (p. 9; findings §C.3); the abstract summarizes "GPT likes using gossip as a plot mechanism" (p. 1). Salient gossip is the GPT side. Table 17 gives uniqueness 22.1, second-highest in the table.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-GPT-2
source_model: GPT
feature: Narrator temporal distance → distant retrospective
dimension: TMP
shap: 0.119
uniqueness_ratio: 6.8
value: option "distant retrospective" (Table 17, p. 27)
scope: global
scope_basis: inferred
scope_fields: nearest: discourse.temporal_order.duration [GLOBAL] ("overall time span"); narration.perspective.point_of_view [GLOBAL] (findings §F.2)
scope_reasoning: The narrator's temporal position relative to the events is a stance held across the whole telling; no template field asks about it directly. Scope global.
validated_on: fiction
fiction_detection: Ask from when the narrator speaks: inside the events, shortly after, or from years or decades later. §5: GPT has "a tendency to frame stories as reflections on events from years or decades ago" (p. 9; findings §C.3). Distant retrospection is the GPT side; no percentage is printed. Table 17 gives uniqueness 6.8.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-GPT-3
source_model: GPT
feature: Reader expectation strategy → subverts
dimension: REV
shap: 0.098
uniqueness_ratio: 3.9
value: option "subverts" (Table 17, p. 27)
scope: global
scope_basis: figure-8
scope_fields: discourse.revelation.surprises [GLOBAL]; discourse.revelation.suspense [GLOBAL] (findings §F.2)
scope_reasoning: How the story manages reader expectation is a whole-text strategy; the revelation fields are [GLOBAL].
validated_on: fiction
fiction_detection: Ask whether the story sets up an expectation and then overturns it, or fulfils what it sets up. §5: "GPT subverts expectations more than other AI (41% vs. 27–36%)" (p. 9; findings §C.3). Subversion is the GPT side. Table 17 gives uniqueness 3.9.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-GPT-4
source_model: GPT
feature: Iterative/habitual narration → no
dimension: TMP
shap: 0.144
uniqueness_ratio: 3.2
value: option "no" (Table 17, p. 27)
scope: global
scope_basis: inferred
scope_fields: nearest: discourse.temporal_order.scene_duration [LOCAL] (findings §F.2)
scope_reasoning: The option is "no" — an absence across the entire text. Scope global.
validated_on: fiction
fiction_detection: Read for narration of repeated or habitual action ("every morning she would…", "on Sundays they…"); the option fires when the text contains none. §5 does not mention this feature; no direction or percentage is printed beyond the Table 17 row. Table 17 gives SHAP 0.144, the highest of the five GPT rows, with uniqueness 3.2.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-GPT-5
source_model: GPT
feature: Reconciliation/forgiveness → partial/ambiguous
dimension: SOC
shap: 0.066
uniqueness_ratio: 2.6
value: option "partial/ambiguous" (Table 17, p. 27)
scope: global
scope_basis: figure-8
scope_fields: story.social_network.relationships [GLOBAL] ("enduring bonds, formatted as A-B: relationship type and quality") (findings §F.2)
scope_reasoning: Whether a rupture is reconciled, and how fully, is the end state of a relationship — the template's relationships field, tagged [GLOBAL]. The reconciliation scene(s) are the driving spans.
validated_on: fiction
fiction_detection: Where a relationship has been broken, read how the story leaves it: fully repaired, refused, or partial and ambiguous. §5: GPT "leaves reconciliations ambiguous" (p. 9; findings §C.3). Partial/ambiguous is the GPT side; no percentage is printed. Table 17 gives the lowest uniqueness of the five GPT rows (2.6).
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

### Source: Gemini (Gemini 3 Flash) — 11 fingerprints (p. 25); narrative per-class F1 0.60 (Table 12, p. 22)

§5 groups Gemini with DeepSeek and Kimi as "triplets": "Despite forming a more confused cluster, each still has individual quirks." For Gemini: "Gemini produces the tidiest endings, extended denouements, and the bleakest settings (88% tagged bleak and oppressive)." (p. 9; findings §C.3) None of these three §5 observations corresponds to a Table 17 Gemini row; "setting mood" and "closure" appear among Gemini's "+ 6 more" (below), so the 88% figure is not placed on any card. Per, the abstract's "Gemini defaults to external character description" (p. 1) is not attributed to Gemini on any card: that is core feature NC-16, elevated across all five AI models, with no Gemini-specific number in the paper.

```
id: FP-Gemini-1
source_model: Gemini
feature: Protagonist social trajectory → expands
dimension: SOC
shap: 0.058
uniqueness_ratio: 5.0
value: option "expands" (Table 17, p. 27)
scope: global
scope_basis: figure-8
scope_fields: story.social_network.relationships [GLOBAL] (findings §F.2)
scope_reasoning: A trajectory of the protagonist's social world runs across the whole story; the relationships field is [GLOBAL].
validated_on: fiction
fiction_detection: Compare the protagonist's circle of relationships at the start and end; the option fires when it grows. §5 does not mention this feature for Gemini; no direction or percentage is printed beyond the Table 17 row, which ranks it first among Gemini fingerprints (uniqueness 5.0).
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-Gemini-2
source_model: Gemini
feature: Balance of speech → primarily direct
dimension: PER
shap: 0.119
uniqueness_ratio: 3.6
value: option "primarily direct" (Table 17, p. 27)
scope: global
scope_basis: inferred
scope_fields: nearest: narration.perspective.dialogue_speakers [LOCAL] (findings §F.2)
scope_reasoning: A balance between direct and reported speech is a proportion over the whole text; the per-scene speakers field does not settle it. Scope global.
validated_on: fiction
fiction_detection: Judge whether characters' speech is mostly quoted directly or mostly summarized and reported; the option fires when direct speech dominates. §5 does not mention this feature; no direction or percentage is printed beyond the Table 17 row (SHAP 0.119, the highest of the five Gemini rows; uniqueness 3.6).
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-Gemini-3
source_model: Gemini
feature: Global narrative schema → siege/ordeal
dimension: EVT
shap: 0.037
uniqueness_ratio: 3.5
value: option "siege/ordeal" (Table 17, p. 27)
scope: global
scope_basis: figure-8
scope_fields: story.events.narrative_schema [GLOBAL] ("higher-level pattern such as quest, revenge, or coming-of-age") (findings §F.2)
scope_reasoning: The feature name says "Global", and the template's narrative_schema field is the same object, tagged [GLOBAL].
validated_on: fiction
fiction_detection: Name the story's overall pattern (quest, revenge, coming-of-age, siege, and so on); the option fires when the shape is a siege or ordeal — characters enduring sustained pressure in place. §5 does not mention this feature; no direction or percentage is printed beyond the Table 17 row (SHAP 0.037, uniqueness 3.5).
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-Gemini-4
source_model: Gemini
feature: Naming practice → named personal name
dimension: AGENT
shap: 0.033
uniqueness_ratio: 3.2
value: option "named personal name" (Table 17, p. 27)
scope: global
scope_basis: figure-8
scope_fields: story.agents.major_characters[].name [GLOBAL] ("use the character's full name as-is") (findings §F.2)
scope_reasoning: Whether characters carry personal names is a practice held across the story; the name field is [GLOBAL].
validated_on: fiction
fiction_detection: Check how characters are designated: by personal name, by role or epithet ("the woman", "the doctor"), or left unnamed; the option fires when personal names are used. §5 does not mention this feature; no direction or percentage is printed beyond the Table 17 row (SHAP 0.033, the lowest in the table; uniqueness 3.2).
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-Gemini-5
source_model: Gemini
feature: Global chronological structure → frequent flashbacks
dimension: TMP
shap: 0.060
uniqueness_ratio: 3.1
value: option "frequent flashbacks" (Table 17, p. 27)
scope: global
scope_basis: figure-8
scope_fields: discourse.temporal_order.structure [GLOBAL] ("choose linear, nonlinear, or mixed"); driving spans from discourse.temporal_order.flashbacks [LOCAL] (findings §F.2)
scope_reasoning: The feature name says "Global", and the template's structure field is the same object, tagged [GLOBAL]. Individual flashbacks ([LOCAL]) are the driving spans.
validated_on: fiction
fiction_detection: Characterize the story's overall time structure; the option fires when flashbacks are frequent enough to define it. §5 does not mention this feature for Gemini; no direction or percentage is printed beyond the Table 17 row (uniqueness 3.1). Compare the core human-elevated temporal features NC-25 and NC-27, where humans exceed the all-AI average (Table 16, p. 26).
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

### Source: DeepSeek (DeepSeek V3.2) — 7 fingerprints (p. 25); narrative per-class F1 0.57 (Table 12, p. 22)

§5 ("triplets" paragraph): "DeepSeek front-loads crucial context that other sources leave until later." (p. 9; findings §C.3)

```
id: FP-DeepSeek-1
source_model: DeepSeek
feature: Narrator presence/visibility
dimension: PER
shap: 0.153
uniqueness_ratio: 4.1
value: none printed in paper (no "→ option" — Table 17, p. 27)
scope: global
scope_basis: inferred
scope_fields: nearest: narration.perspective.point_of_view [GLOBAL] (findings §F.2)
scope_reasoning: How visible the narrator is as a presence is a stance across the whole telling; point_of_view is [GLOBAL] but concerns person and limitation, not visibility, so the wording decides. Scope global.
validated_on: fiction
fiction_detection: Judge how much the narrator is felt as a presence — commenting, judging, addressing — versus receding behind the events. §5 does not name this feature for DeepSeek; Table 17 prints no per-source mean or direction, so the card cannot say whether DeepSeek's narrators are more or less visible. Table 17 gives SHAP 0.153, the highest of the five DeepSeek rows, with uniqueness 4.1.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-DeepSeek-2
source_model: DeepSeek
feature: Emotional expression → behavioral cues
dimension: AGENT
shap: 0.069
uniqueness_ratio: 3.6
value: option "behavioral cues" (Table 17, p. 27)
scope: global
scope_basis: inferred
scope_fields: nearest: story.agents.major_characters[].emotion_trajectory [GLOBAL] (findings §F.2)
scope_reasoning: Same feature as core NC-07; the dominant vehicle of emotion is a distribution over the whole text. Scope global.
validated_on: fiction
fiction_detection: Each time a character's emotion reaches the page, note the vehicle; the option fires when outward behavior (gestures, actions, silences) is the dominant vehicle rather than embodied sensation or named feeling. This is a third option of the same feature as NC-07, whose AI-elevated option is embodied (81% vs 38%) and whose human-elevated option is explicit labels (29% vs 8%) (Table 16, p. 26). §5 offers no prose or percentage for the behavioral-cues option; Table 17 gives uniqueness 3.6.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-DeepSeek-3
source_model: DeepSeek
feature: Plot vs. atmosphere orientation
dimension: SIT
shap: 0.117
uniqueness_ratio: 2.9
value: none printed in paper (no "→ option" — Table 17, p. 27)
scope: global
scope_basis: inferred
scope_fields: nearest: story.plot.summary [GLOBAL]; story.setting.atmosphere [GLOBAL] (findings §F.2)
scope_reasoning: The balance between incident and atmosphere is a property of the whole work; no template field asks for it directly. Scope global.
validated_on: fiction
fiction_detection: Judge whether the story is carried chiefly by what happens or by mood and setting. §5 does not name this feature; Table 17 prints no per-source mean or direction, so the card cannot say which way DeepSeek leans. Table 17 gives SHAP 0.117, uniqueness 2.9.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-DeepSeek-4
source_model: DeepSeek
feature: Backstory placement → evenly interleaved
dimension: TMP
shap: 0.096
uniqueness_ratio: 2.7
value: option "evenly interleaved" (Table 17, p. 27)
scope: global
scope_basis: inferred
scope_fields: nearest: discourse.temporal_order.flashbacks [LOCAL] (findings §F.2)
scope_reasoning: Each backstory segment is locatable ([LOCAL]), but "placement" describes the distribution of all of them across the story. Scope global; the backstory segments are the driving spans.
validated_on: fiction
fiction_detection: Mark each passage of backstory and look at where they fall: clustered at the start, saved for late, or spread evenly through the telling; the option fires on even spread. Note that §5's only DeepSeek observation is "DeepSeek front-loads crucial context that other sources leave until later" (p. 9; findings §C.3), while the Table 17 option is "evenly interleaved"; the paper does not relate the two statements, and this card reproduces both without reconciling them. Table 17 gives uniqueness 2.7.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-DeepSeek-5
source_model: DeepSeek
feature: Embedded storytelling scenes
dimension: SIT
shap: 0.078
uniqueness_ratio: 2.2
value: none printed in paper (no "→ option" — Table 17, p. 27)
scope: local
scope_basis: inferred
scope_fields: nearest: story.events.sequence [LOCAL] (findings §F.2)
scope_reasoning: An embedded story — a character telling a tale inside the tale — is a bounded scene with a start and end. Scope local; each instance is quoted with its location.
validated_on: fiction
fiction_detection: Look for scenes in which a character recounts a story, legend, or anecdote to others within the narrative. §5 does not name this feature; Table 17 prints no per-source mean or direction, so the card cannot say whether DeepSeek uses more or fewer such scenes. Table 17 gives the lowest uniqueness of the five DeepSeek rows (2.2).
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

### Source: Kimi (Kimi K2.5) — 3 fingerprints (p. 25); Table 17 shows all three; narrative per-class F1 0.55 (Table 12, p. 22)

§5 ("triplets" paragraph): "Kimi has the fewest fingerprints and lowest F1, sitting at the generic center of the AI distribution with no distinctive narrative choices." (p. 9; findings §C.3) No "+ N more" line exists for Kimi (findings §C.2).

```
id: FP-Kimi-1
source_model: Kimi
feature: Character introduction → in-action event
dimension: AGENT
shap: 0.163
uniqueness_ratio: 3.7
value: option "in-action event" (Table 17, p. 27)
scope: local
scope_basis: inferred
scope_fields: nearest: story.agents.major_characters[].role [GLOBAL] (findings §F.2)
scope_reasoning: Same feature as core NC-16 and FP-Human-1; the introduction is a single locatable passage. Scope local.
validated_on: fiction
fiction_detection: Locate the first appearance of the central character and check whether the reader meets them in the middle of doing something rather than through description, dialogue, thought, or report. This is another option of the same feature as NC-16 (AI-elevated option: external description, 52% vs 30%, Table 16, p. 26) and FP-Human-1 (in-dialogue). §5's only Kimi statement is that it sits "at the generic center of the AI distribution with no distinctive narrative choices" (p. 9); no percentage is printed. Table 17 gives SHAP 0.163, uniqueness 3.7.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-Kimi-2
source_model: Kimi
feature: Narrative entry frame → in medias res
dimension: PLT
shap: 0.035
uniqueness_ratio: 3.0
value: option "in medias res" (Table 17, p. 27)
scope: local
scope_basis: inferred
scope_fields: nearest: story.plot.plot_arc [GLOBAL]; story.events.sequence [LOCAL] (findings §F.2)
scope_reasoning: The entry frame is the opening passage — a bounded span, as with core NC-19. Scope local.
validated_on: fiction
fiction_detection: Read the opening: does the story begin in the middle of action already under way, or with setup before events start? The option fires on the former. §5 offers no prose or percentage for it beyond Kimi's "generic center" characterization (p. 9; findings §C.3). Table 17 gives SHAP 0.035, uniqueness 3.0.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

```
id: FP-Kimi-3
source_model: Kimi
feature: Explicit trait labeling → no
dimension: AGENT
shap: 0.136
uniqueness_ratio: 2.0
value: option "no" (Table 17, p. 27)
scope: global
scope_basis: figure-8
scope_fields: story.agents.major_characters[].attributes [GLOBAL] ("descriptive traits or phrases") (findings §F.2)
scope_reasoning: The option is "no" — an absence of trait labels across the whole text; the template's attributes field, the same object, is [GLOBAL].
validated_on: fiction
fiction_detection: Read for sentences that name a character's traits outright ("she was stubborn", "a generous man"); the option fires when the text never does this and traits emerge only through action and speech. §5 offers no prose or percentage for it beyond Kimi's "generic center" characterization (p. 9; findings §C.3). Table 17 gives uniqueness 2.0, the lowest in the table.
attribution_ceiling: six-way macro-F1 68.4 (Table 3, p. 8); confidence for any single-document attribution is Low
```

### Fingerprint features named in Table 17 without values ("+ N more", p. 27) — `no value in paper`

Names only, transcribed from the Table 17 parentheticals as reproduced in findings §C.2. None has a SHAP, uniqueness ratio, option, or direction in the paper; none receives an FP-id (registry rule). The ellipses are the paper's own — each list is partial.

- **Human** — "+ 27 more": visibility of withholding, atmospheric techniques, subplot density, naming, twist placement, … — `no value in paper` (Table 17, p. 27)
- **Claude** — "+ 21 more": event density, conflict modality, relationship trajectory, heteroglossia, closure, … — `no value in paper` (Table 17, p. 27)
- **GPT** — "+ 6 more": community salience, social emphasis, reader competence, individualization, psych. depth, … — `no value in paper` (Table 17, p. 27)
- **Gemini** — "+ 6 more": secondary char. density, batch intro, authority stance, setting mood, community, closure — `no value in paper` (Table 17, p. 27). The §5 Gemini observations (tidiest endings, extended denouements, "88% tagged bleak and oppressive", p. 9) attach to features in this unvalued list ("setting mood", "closure"), not to any FP-Gemini-n card.
- **DeepSeek** — "+ 2 more": seasons/cyclical time, … — `no value in paper` (Table 17, p. 27)
- **Kimi** — no "+ N more" line; Table 17 shows all three Kimi fingerprints (Table 17, p. 27)

### Fingerprint-card scope tally (inference summary for Gate 2)

| Scope | Cards | Count |
|---|---|---|
| global | FP-Human-2, FP-Human-3, FP-Human-4, FP-Human-5, FP-Claude-1, FP-Claude-2, FP-Claude-4, FP-Claude-5, FP-GPT-1, FP-GPT-2, FP-GPT-3, FP-GPT-4, FP-GPT-5, FP-Gemini-1, FP-Gemini-2, FP-Gemini-3, FP-Gemini-4, FP-Gemini-5, FP-DeepSeek-1, FP-DeepSeek-2, FP-DeepSeek-3, FP-DeepSeek-4, FP-Kimi-3 | 23 |
| local | FP-Human-1, FP-Claude-3, FP-DeepSeek-5, FP-Kimi-1, FP-Kimi-2 | 5 |

| Basis | Cards | Count |
|---|---|---|
| figure-8 | FP-Human-4, FP-Claude-1, FP-Claude-3, FP-Claude-5, FP-GPT-3, FP-GPT-5, FP-Gemini-1, FP-Gemini-3, FP-Gemini-4, FP-Gemini-5, FP-Kimi-3 | 11 |
| inferred | FP-Human-1, FP-Human-2, FP-Human-3, FP-Human-5, FP-Claude-2, FP-Claude-4, FP-GPT-1, FP-GPT-2, FP-GPT-4, FP-Gemini-2, FP-DeepSeek-1, FP-DeepSeek-2, FP-DeepSeek-3, FP-DeepSeek-4, FP-DeepSeek-5, FP-Kimi-1, FP-Kimi-2 | 17 |

---

## Scope notes that apply to every card

1. **Fiction only.** All 68 cards rest on fiction evidence (findings §E.1; the release covers the same corpus; the validation study rated and rewrote fiction only, n = 10, one direction). A card applied to any other content type carries `analog: unvalidated`; that behavior is defined per content type by C2 in `content_type_matrix.md`, never here. Non-fiction analogs remain untested.
2. **The rater reproduces the paper's instrument, not a human standard, and reproduces it imperfectly.** Features were assigned by Gemini 3 Flash; human–human agreement (κ 0.74) was lower than human–model agreement (mean κ 0.84) on 240 items (Table 7, p. 19; findings §E.3). Measured for these cards specifically: **60% exact and 86% within one rung** against the authors' own assignments over 300 blind ratings. A card rating is an LLM rubric judgment, not the paper's classifier and not a human standard.
3. **Core features are the all-AI consensus, not a theory of "AI-ness".** They were selected with an absolute human–AI gap of at least 0.20 and cross-model AI spread of at most 0.35 (p. 20; findings §E.12).
4. **No card carries a detection threshold.** The paper prints means and prevalences, not cut-offs; any threshold a skill applies is that skill's design choice and must be labelled as such (findings §E.2).
5. **Success rate: the paper still has none; the validation study has one, on n = 10.** The paper tests surface editing only (LAMP, 278 Gemini stories, 95.5 → 93.9 macro-F1, p. 8) and never tests structural rewrites against its classifier (findings §E.4). The validation study did: a structural pass driven by these cards moved ten AI stories from mean P(human) 0.057 to 0.607, **5/10 crossing the 0.5 boundary**, 10/10 deltas positive (p = 0.002), with 66 core-feature movements toward the human baseline against 13 away. That is a measured result on ten fiction stories in one direction against our retrained classifier — not a general success rate, and not the paper's classifier. Caveats in validation results Step 5.
6. **The cards do not cover the classifier.** The 30 core cards carry **25%** of total measured importance mass; NC-31 … NC-40 were added because of it, and even with them the coverage is partial. A clean card report is not a claim about what a detector would say.
7. **The extended cards are ours, not the paper's.** NC-31 … NC-40 are taxonomy features with recomputed baselines and measured weights. Table 16 does not print them, and no file may present them as paper core features.
