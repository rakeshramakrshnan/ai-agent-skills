# Narrative Humanize

Narrative Humanize revises writing that feels overly generated or mechanically structured while protecting its meaning, facts, citations, and intended voice.

It works on the organization and flow of the piece first, then improves the wording. The result is a revised text accompanied by a clear summary of meaningful changes.

## What it does

- Finds structural patterns that make writing feel predictable or over-explained.
- Revises pacing, emphasis, transitions, and other narrative choices where appropriate.
- Improves stiff or repetitive wording after the structural revision.
- Preserves names, numbers, claims, quotations, citations, and other factual content.
- Checks the finished version against the original.
- Explains what changed and where.

The skill adjusts its approach for fiction, essays, research writing, journalism, documentation, and other common forms. It does not apply fiction techniques where they would be inappropriate.

## Install

Download or clone this repository, then keep the entire `narrative-humanize` folder together.

### Claude

#### Claude on the web or desktop

1. Create a ZIP file containing the `narrative-humanize` folder.
2. In Claude, open **Customize > Skills**.
3. Select **+**, then **Create skill** and **Upload a skill**.
4. Choose the ZIP file and enable the skill.

Claude requires **Code execution and file creation** to be enabled. See Anthropic's [guide to using skills](https://support.claude.com/en/articles/12512180-use-skills-in-claude) if the Skills option is not visible.

#### Claude Code

Install the folder for your user account:

```bash
mkdir -p ~/.claude/skills
cp -R narrative-humanize ~/.claude/skills/
```

To make it available only inside one project, copy it to that project's `.claude/skills/` folder instead.

### ChatGPT

Standalone skills are available in the ChatGPT desktop app.

1. Create a ZIP file containing the `narrative-humanize` folder.
2. Open **Skills** in the ChatGPT desktop sidebar.
3. Use the add or import option and select the ZIP file.
4. Enable the skill when prompted.

See OpenAI's [skills guide](https://developers.openai.com/docs/build-skills) for current availability and interface details.

## Use

Ask Claude or ChatGPT to use the skill and include the text you want revised.

Example:

```text
Use narrative-humanize to revise the text below. Preserve every fact and citation, keep the tone professional, and show me a summary of the important changes.

[paste your text]
```

If you already have a Narrative Check report, include it with the text. If you do not, Narrative Humanize can review the text before revising it.

In ChatGPT, you can also type `@` and select **narrative-humanize**. Claude can choose the skill automatically when your request matches its purpose.

## What you receive

The output includes:

- the revised text;
- a concise record of structural changes;
- confirmation that facts were preserved; and
- a before-and-after assessment of the main writing patterns.

## Important limits

- The skill never promises to evade AI detection.
- It does not invent sources, quotations, facts, or figures.
- It cannot verify whether a claim in the original text is true unless supporting evidence is provided.
- The final result should still receive normal author or editor review before publication.

## License

Released under the [MIT License](LICENSE.txt).
