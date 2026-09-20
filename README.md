# AI Agent Skills

Portable, inspectable skills for Claude, ChatGPT, and Codex. Each skill is a
self-contained directory: keep its `SKILL.md` and any bundled references
together when you install or share it.

## Skills

| Skill | What it does |
| --- | --- |
| [Literature Review & Synthesis](literature-review-synthesis/README.md) | Builds traceable thematic reviews, evidence matrices, gap analyses, critical appraisals, and research agendas from a supplied paper corpus. |
| [Narrative Check](narrative-check/README.md) | Audits structural writing patterns without rewriting the text. |
| [Narrative Humanize](narrative-humanize/README.md) | Revises structure and prose while preserving stated facts and voice. |

## Install a skill

Clone or download this repository, then install the individual skill folder you
want. Do not copy only `SKILL.md`: references are part of the workflow.

### Claude Code

For one project, copy the folder into `.claude/skills/`. For a user-wide
installation, use `~/.claude/skills/` instead.

```bash
git clone https://github.com/rakeshramakrshnan/ai-agent-skills.git
mkdir -p .claude/skills
cp -R ai-agent-skills/literature-review-synthesis .claude/skills/
```

### ChatGPT and Codex

OpenAI skills use the same `SKILL.md` directory format. In a supported ChatGPT
surface, create a ZIP whose top-level entry is the skill directory, then choose
**Skills → Create → Upload**. In Codex, add the directory to your configured
skills location or use the product's Skills interface. Availability and the
exact install flow depend on your account and workspace; see OpenAI's
[Skills documentation](https://developers.openai.com/docs/build-skills) and
[ChatGPT Skills guide](https://help.openai.com/en/articles/20001066).

```bash
cd ai-agent-skills
zip -r literature-review-synthesis.zip literature-review-synthesis
```

## Design principles

- Evidence is traceable to a supplied source; unsupported claims are marked as
  inference or omitted.
- Skills make no external calls or write files unless the active environment
  and the user's request authorize those actions.
- Instructions are product-neutral where possible, with optional UI metadata
  in `agents/openai.yaml` for supported OpenAI surfaces.

## License

Unless a skill states otherwise, this repository is released under the
[MIT License](LICENSE).
