# Narrative Check

Narrative Check reviews a piece of writing and points out patterns that can make it feel overly generated, predictable, or mechanically structured.

It gives you a clear report with examples from the text. It does not rewrite your work.

## What it does

- Reviews the full text before making judgments.
- Identifies the type of writing, such as fiction, an essay, research writing, journalism, or documentation.
- Locates each concern with a paragraph number and a short quotation.
- Separates stronger findings from uncertain ones.
- Suggests which structural issues should be addressed first.
- Preserves the original text unchanged.

Narrative Check can be used on fiction and nonfiction, but its strongest research basis is fiction. Its report is an editorial assessment, not proof that a person or an AI wrote the text.

## Install

Download or clone this repository, then keep the entire `narrative-check` folder together.

### Claude

#### Claude on the web or desktop

1. Create a ZIP file containing the `narrative-check` folder.
2. In Claude, open **Customize > Skills**.
3. Select **+**, then **Create skill** and **Upload a skill**.
4. Choose the ZIP file and enable the skill.

Claude requires **Code execution and file creation** to be enabled. See Anthropic's [guide to using skills](https://support.claude.com/en/articles/12512180-use-skills-in-claude) if the Skills option is not visible.

#### Claude Code

Install the folder for your user account:

```bash
mkdir -p ~/.claude/skills
cp -R narrative-check ~/.claude/skills/
```

To make it available only inside one project, copy it to that project's `.claude/skills/` folder instead.

### ChatGPT

Standalone skills are available in the ChatGPT desktop app.

1. Create a ZIP file containing the `narrative-check` folder.
2. Open **Skills** in the ChatGPT desktop sidebar.
3. Use the add or import option and select the ZIP file.
4. Enable the skill when prompted.

See OpenAI's [skills guide](https://developers.openai.com/docs/build-skills) for current availability and interface details.

## Use

Ask Claude or ChatGPT to use the skill and include the text you want reviewed.

Example:

```text
Use narrative-check on the text below. Show me the passages that feel overly generated or mechanically structured, and explain which issues matter most.

[paste your text]
```

In ChatGPT, you can also type `@` and select **narrative-check**. Claude can choose the skill automatically when your request matches its purpose.

## What you receive

The report includes:

- the kind and length of text reviewed;
- an overall assessment with an uncertainty note;
- paragraph-level examples;
- patterns that make the text feel more human or more generated; and
- a prioritized handoff for revision.

## Important limits

- The skill does not determine who wrote a text.
- It does not guarantee the result of any AI detector.
- Short, translated, or mixed-format texts may produce less certain findings.
- It does not rewrite the submitted text. Use Narrative Humanize when you want a revision.

## License

Released under the [MIT License](LICENSE.txt).
