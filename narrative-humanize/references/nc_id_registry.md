# NC-id registry

**Source:** Table 16, StoryScope (Russell et al., COLM 2026, arXiv 2604.03136v6), p. 26. Order is Table 16 order. Table 16 has 33 rows; three features (Reference Explicitness, Emotional Expression, Subplot Integration) appear twice with different option values and are counted once, giving 30 distinct features. Option rows are listed under their feature.

Do not renumber these ids.

## Core features (NC-01 … NC-30)

| NC-id | Feature | Table 16 theme group | Option rows in Table 16 (direction) |
|---|---|---|---|
| NC-01 | Thematic Explicitness & Moralizing | AI-elevated: Thematic over-determination | scale (AI-elevated) |
| NC-02 | Moral / Philosophical Weighting | AI-elevated: Thematic over-determination | scale (AI-elevated) |
| NC-03 | Thematic Unity | AI-elevated: Thematic over-determination | scale (AI-elevated) |
| NC-04 | Narratorial Thematic Commentary | AI-elevated: Thematic over-determination | → yes (AI-elevated) |
| NC-05 | Dialogue Function | AI-elevated: Thematic over-determination | → philosophical debate (AI-elevated) |
| NC-06 | Reference Explicitness | AI-elevated: Thematic over-determination / Human-elevated: Intertextual richness | → implicit echoes (AI-elevated); → balanced mix (Human-elevated) |
| NC-07 | Emotional Expression | AI-elevated: Sensory & embodied performativity / Human-elevated: Narrative diversity | → embodied (AI-elevated); → explicit labels (Human-elevated) |
| NC-08 | Setting as Psychological Mirror | AI-elevated: Sensory & embodied performativity | scale (AI-elevated) |
| NC-09 | Environmental & Ecological Emphasis | AI-elevated: Sensory & embodied performativity | scale (AI-elevated) |
| NC-10 | Sensory Modalities | AI-elevated: Sensory & embodied performativity | → olfactory (AI-elevated) |
| NC-11 | Sensory Density | AI-elevated: Sensory & embodied performativity | scale (AI-elevated) |
| NC-12 | Depth of Interior Access | AI-elevated: Sensory & embodied performativity | scale (AI-elevated) |
| NC-13 | Causal Chain Continuity | AI-elevated: Structural streamlining | scale (AI-elevated) |
| NC-14 | Spatial Granularity | AI-elevated: Structural streamlining | ordinal (AI-elevated) |
| NC-15 | Agency in Resolution | AI-elevated: Structural streamlining | → protagonist choice (AI-elevated) |
| NC-16 | Character Introduction | AI-elevated: Structural streamlining | → external description (AI-elevated) |
| NC-17 | Subplot Integration | AI-elevated: Structural streamlining / Human-elevated: Narrative diversity | → no subplots (AI-elevated); → thematically parallel (Human-elevated) |
| NC-18 | Resolution Mode | AI-elevated: Structural streamlining | → internal understanding (AI-elevated) |
| NC-19 | Opening Spatial Grounding | AI-elevated: Structural streamlining | ordinal (AI-elevated) |
| NC-20 | Pre-Threat Character Investment | AI-elevated: Structural streamlining | scale (AI-elevated) |
| NC-21 | Intertextual Strategy | Human-elevated: Intertextual richness | → explicit named reference (Human-elevated) |
| NC-22 | Fourth-Wall Permeability | Human-elevated: Reader engagement | ordinal (Human-elevated) |
| NC-23 | Direct Reader Address | Human-elevated: Reader engagement | ordinal (Human-elevated) |
| NC-24 | Depth of Recontextualization After Surprise | Human-elevated: Temporal complexity | scale (Human-elevated) |
| NC-25 | Chronological Discontinuity | Human-elevated: Temporal complexity | scale (Human-elevated) |
| NC-26 | Nonlinear Framing for Delayed Disclosure | Human-elevated: Temporal complexity | scale (Human-elevated) |
| NC-27 | Anachrony Intensity | Human-elevated: Temporal complexity | scale (Human-elevated) |
| NC-28 | Location Variety Scope | Human-elevated: Narrative diversity | ordinal (Human-elevated) |
| NC-29 | Dialogue-to-Narration Proportion | Human-elevated: Narrative diversity | scale (Human-elevated) |
| NC-30 | Moral Polarity | Human-elevated: Narrative diversity | → ambivalent/mixed (Human-elevated) |

Means, gaps, response types, and question wording: see the StoryScope paper §B. This registry carries ids and names only.

## Extended tier (NC-31 … NC-40)

**Tier:** `extended (measured, not in Table 16)`. These are **not** Table 16 core features and must never be described as such. They come from the paper's own 304-feature taxonomy (`taxonomy.json`); their human/AI separation and classifier weight were measured by this project because the 30 core cards carry only 25% of the classifier's importance mass (the operating rules ; `feature_cards.md` Part B). Baselines are recomputed from `storyscope_features.parquet`, not printed in the paper.

| NC-id | Feature | taxonomy id |
|---|---|---|
| NC-31 | Modes of conveying the central character's emotions | `AGENT_EMO_002` |
| NC-32 | Dominant Narrative Tense | `TMP_DUR_011` |
| NC-33 | Post-Climax Denouement Length | `PLT_MOR_007` |
| NC-34 | Density of figurative language in character depiction | `AGENT_ATTR_024` |
| NC-35 | Overall Revelation Pacing Pattern | `REV_DIS_001` |
| NC-36 | Explicit Enumeration vs Enacted Interaction | `SOC_REL_004` |
| NC-37 | Naming practice for the central character | `AGENT_ID_005` |
| NC-38 | Primary Genre Category | `SIT_GEN_001` |
| NC-39 | Breadth of focalization across characters | `PER_FOC_002` |
| NC-40 | Proportion of named vs. unnamed characters | `AGENT_ID_003` |

Full cards with measured baselines, importance ranks and the authors' detection methods: `feature_cards.md`.

## Fingerprint features (FP-<source>-<n>)

Ids follow Table 17 (p. 27) row order within each source. Only rows Table 17 names individually receive an id; the "+ N more" parentheticals name features without values and receive no id (they may be cited by name, tagged `no value in paper`).

| FP-id | Source | Feature → option |
|---|---|---|
| FP-Human-1 | Human | Character introduction → in-dialogue |
| FP-Human-2 | Human | Breadth of focalization → single focal |
| FP-Human-3 | Human | Narrator address mode → no direct address |
| FP-Human-4 | Human | Overall revelation pacing → back-loaded |
| FP-Human-5 | Human | Literary ambition → crossover genre |
| FP-Claude-1 | Claude | Strength of event escalation |
| FP-Claude-2 | Claude | Event-type diversity |
| FP-Claude-3 | Claude | Ending temporal scope → epilogue/flashforward |
| FP-Claude-4 | Claude | Dreams/visions as temporal distortion → no |
| FP-Claude-5 | Claude | Setting mood → uncanny/haunted |
| FP-GPT-1 | GPT | Role of gossip and rumor → salient |
| FP-GPT-2 | GPT | Narrator temporal distance → distant retrospective |
| FP-GPT-3 | GPT | Reader expectation strategy → subverts |
| FP-GPT-4 | GPT | Iterative/habitual narration → no |
| FP-GPT-5 | GPT | Reconciliation/forgiveness → partial/ambiguous |
| FP-Gemini-1 | Gemini | Protagonist social trajectory → expands |
| FP-Gemini-2 | Gemini | Balance of speech → primarily direct |
| FP-Gemini-3 | Gemini | Global narrative schema → siege/ordeal |
| FP-Gemini-4 | Gemini | Naming practice → named personal name |
| FP-Gemini-5 | Gemini | Global chronological structure → frequent flashbacks |
| FP-DeepSeek-1 | DeepSeek | Narrator presence/visibility |
| FP-DeepSeek-2 | DeepSeek | Emotional expression → behavioral cues |
| FP-DeepSeek-3 | DeepSeek | Plot vs. atmosphere orientation |
| FP-DeepSeek-4 | DeepSeek | Backstory placement → evenly interleaved |
| FP-DeepSeek-5 | DeepSeek | Embedded storytelling scenes |
| FP-Kimi-1 | Kimi | Character introduction → in-action event |
| FP-Kimi-2 | Kimi | Narrative entry frame → in medias res |
| FP-Kimi-3 | Kimi | Explicit trait labeling → no |

Fingerprint totals per source (p. 25): Human 32, Claude 26, GPT 11, Gemini 11, DeepSeek 7, Kimi 3. Table 17 shows the top 5 by uniqueness ratio (or all, if fewer).
