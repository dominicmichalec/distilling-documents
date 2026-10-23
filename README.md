# distilling-documents

An [Agent Skill](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) that turns a long or dense document (a report, assessment, contract, paper or board pack) into an executive one-pager that someone can act on in two minutes. Every claim on the page is followed by a short verbatim quote from the source, so the reader can check it.

It's prose only: a `SKILL.md` and one template, no scripts.

## What it does

1. **Finds out who the page is for.** It asks for the audience, the decision and the length in one message. When nobody can answer (an API call or a batch job), it picks the most plausible answers and states them on an **Assumptions** line at the top instead of guessing silently.
2. **Pulls quotes before writing.** It reads the whole document first, looking hardest for later passages that correct an earlier figure. Repeating a superseded number is the worst mistake a one-pager can make.
3. **Drafts to a fixed template** (`ONE-PAGER-TEMPLATE.md`): bottom line, why, numbers that matter, risks and open issues, next step.
4. **Traces every claim** in the form `claim ("exact words from the document")`. A claim it can't quote gets cut or marked as inference, and its own judgement starts with "My reading:".
5. **Checks its draft and fixes it** until a full pass finds nothing wrong.

## Install

**Claude Code:** copy the `distilling-documents` folder into `~/.claude/skills/` (for all your projects) or `.claude/skills/` (for one project).

**Claude apps:** zip the `distilling-documents` folder and upload it as a custom skill in your settings.

**Claude API:** upload the folder with the Skills API. Send every file each time: a new version is a full snapshot, not a diff.

```bash
curl https://api.anthropic.com/v1/skills \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -F "display_name=distilling-documents" \
  -F "files[]=@distilling-documents/SKILL.md;filename=distilling-documents/SKILL.md" \
  -F "files[]=@distilling-documents/ONE-PAGER-TEMPLATE.md;filename=distilling-documents/ONE-PAGER-TEMPLATE.md"
```

Then attach it to a request through `container.skills`, together with the code execution tool. See [Using Agent Skills with the API](https://platform.claude.com/docs/en/build-with-claude/skills-guide) for the request shape. In production, pin a specific version rather than `latest`: custom skills are shared across a workspace, so anyone with a key can push a new version.

## What testing showed

These results come from runs on the Claude API in October 2026, on `claude-sonnet-5-5` and `claude-haiku-5-5`, against the sample document in `evals/`. Each was a single run, so treat them as indications rather than benchmarks.

- **Triggering:** both models used the skill for an explicit one-pager request and for "I have five minutes before the meeting. What do I need to know?", and skipped it for a short email and a translation.
- **Over-triggering on Haiku:** Haiku also used the skill for "Give me a quick summary of this" and "Summarise this in 3 bullets". If that matters to you, this narrower description stopped both while keeping the real triggers (5 for 5 on two runs, Haiku only):

  ```yaml
  description: Distils a long or dense document (report, assessment, contract, paper, board pack) into an executive one-pager for a decision-maker, with every claim traced to a quote from the source. Use when the user asks for an executive summary, one-pager, board brief or leadership TL;DR of a document of more than a few pages. Not for quick or casual summaries, bullet lists, data extraction, short emails, translation, or proofreading.
  ```

- **Length:** the skill asks for about 350 words, and neither version reached that. The current version produced 509 words with 19 quotes on Sonnet, all verbatim.
- **Cost:** attaching the skill added about 680 fixed tokens to a Haiku request. A full run reads `SKILL.md`, then the template, then writes the page, and each step sends the document to the model again. In testing that came to 26,871 input tokens against 4,014 without the skill: $0.083 against $0.031 on Sonnet, plus code execution usage.

## Evals

`evals/evals.json` holds three test prompts against `evals/fixtures/cold-chain-assessment.md`, a synthetic document in which every name and figure is invented:

- an explicit one-pager request, which should use the corrected figures from the appendix rather than the ones in section 3
- an indirect "what do I need to know?", which should still trigger the skill
- a one-line email summary, which should not

Each lists the behaviour to check for. Rerun them before you push a new version.

## License

MIT. See [LICENSE](LICENSE).
