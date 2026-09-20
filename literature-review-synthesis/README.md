# Literature Review & Synthesis

Turn a supplied set of research papers into a traceable synthesis. The skill is
for reading a corpus as a body of evidence, rather than summarizing papers one
at a time.

## What it produces

- thematic literature reviews and field-evolution timelines;
- evidence matrices, knowledge maps, and conceptual frameworks;
- research gaps, hidden questions, untested mechanisms, and future-work
  syntheses;
- agreement/disagreement maps and methodology audits; and
- research agendas or novelty angles, clearly marked as inference.

Every substantive assertion must identify the paper that supports it. When
available, the skill also records a page, section, table, or figure locator so
the reader can check the underlying evidence quickly.

## When to use it

Use it when you have multiple papers on one topic and want a corpus-level
answer: “synthesize these studies,” “find research gaps,” “make an evidence
matrix,” or “what do these researchers disagree about?”

It is not a substitute for a systematic-review protocol, a database search, or
formal risk-of-bias assessment. It only makes claims supported by the papers
provided to the conversation or a user-authorized local corpus.

## Install

Keep the entire `literature-review-synthesis` directory intact.

### Claude Code

```bash
mkdir -p .claude/skills
cp -R literature-review-synthesis .claude/skills/
```

Use `~/.claude/skills/` for a user-wide installation.

### ChatGPT and Codex

Zip the directory and upload it through a supported Skills interface, or place
the directory in your Codex skills location. OpenAI documents the current
format and product availability in its [Skills documentation](https://developers.openai.com/docs/build-skills).

```bash
zip -r literature-review-synthesis.zip literature-review-synthesis
```

## Example prompts

```text
Use literature-review-synthesis on these 18 papers. Create a thematic review,
cite each substantive claim to a paper and page or section when available, and
end with only the unresolved questions the corpus supports.
```

```text
Build an evidence matrix from the PDFs in ./papers. Include an explicit row for
each file that could not be read; do not fill missing fields by inference.
```

```text
Compare the supplied studies' conclusions about remote work and productivity.
Show the evidence on each side, explain design differences that could account
for disagreement, and label proposed future studies as inference.
```

## Expected inputs and outputs

Provide PDFs, paper text, stable links, or a local directory that the assistant
is authorized to read. It will confirm the intended corpus when the count or
scope is unclear. By default, results are returned in the conversation; a file
is written only when you request one or name an output location.

The normal working record is a corpus index with one row per paper. It captures
bibliographic information, research question, theory, sample or dataset,
method, key findings, limitations, future work, and evidence locators. Missing
information is recorded as “not reported,” never guessed.

## Limitations

- A synthesis can be only as complete as the corpus supplied.
- Citation traceability is not independent fact-checking or a claim of study
  quality.
- A proposed hypothesis, mechanism, or study design is an inference, not a
  finding from the corpus.
- For high-stakes, systematic, or publishable reviews, use an appropriate
  protocol and have a domain expert review the result.

## License

Released under the [MIT License](LICENSE.txt).
