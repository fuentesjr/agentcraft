# AGENTS.md

Guidance for AI agents working in this repository.

## What this repo is

Agentcraft is a public lab notebook for developing practical strategies, workflows, and systems for working with AI agents. Most work here is Markdown: ideas, proposals, experiments, patterns, playbooks, templates, and reflections.

The goal is not to collect clever prompts. The goal is to understand which systems reliably improve judgment, focus, and execution.

## Operating principles

- Prefer simple, practical systems over elaborate frameworks.
- Be a constructive challenger: question weak assumptions, but offer concrete alternatives.
- Preserve the lab-notebook nature of the repo. Rough thinking is allowed when it is clearly labeled.
- Separate evidence from speculation. If something has not been tested, say so.
- Avoid hype, false certainty, and polished AI-sounding filler.
- This is a public repo; do not add secrets, private client details, or unsanitized personal information.

## Repository map

- `ideas/` — raw concepts, questions, bets, and early sketches.
- `proposals/` — canonical plans for ideas being shaped into concrete experiments, tools, or systems.
- `experiments/` — bounded tests, results, and reflections.
- `patterns/` — distilled workflows and system designs that appear reusable.
- `playbooks/` — practical operating guides for repeatable workflows.
- `templates/` — reusable prompts, checklists, scaffolds, and supporting materials.

Before editing, read `README.md` and the `README.md` in the relevant directory.

## Where new content belongs

Use the maturity of the work to choose a location:

```text
ideas/ → proposals/ → experiments/ → patterns/ → playbooks/
```

Not everything needs to graduate. Failed experiments are useful when they explain what was tried, what happened, and what changed as a result.

For early-stage software or product ideas, prefer one canonical proposal document in `proposals/` that keeps the idea, motivation, design decisions, v0 scope, eval plan, open questions, and next steps together. Split into separate docs only when sections have distinct lifecycles or the idea becomes its own project.

When promoting or superseding material, leave a short status note and relative link from the older document to the newer canonical location.

## Writing standards

- Use clear Markdown with descriptive headings.
- Preserve the existing direct, reflective voice.
- Use short lowercase filenames without spaces, such as `ctxpack.md`.
- Prefer concrete examples, commands, checklists, and decision records over abstract advice.
- Include non-goals and rejected alternatives when they clarify scope.
- Do not over-template small notes. Add structure only when it improves readability or future reuse.
- Use relative links for repo-local references.

Substantial documents should usually answer:

- What question, problem, or hypothesis is this about?
- Why does it matter?
- What is the smallest useful version?
- What has been tried or observed?
- What are the tradeoffs, uncertainty, and non-goals?
- What should happen next?

## Experiments, patterns, and playbooks

For experiments, include enough detail to make the result interpretable:

- hypothesis or core question
- setup and procedure
- result or observation
- interpretation
- what changed as a result

Only move material into `patterns/` or `playbooks/` when it has become reusable. If it is promising but not proven, label it as a candidate pattern or draft playbook.

Templates should say when to use them and what inputs they expect.

## Operational guidance

- This repo currently has no build, test, or formatter harness.
- For Markdown-only changes, verify headings, links, and examples manually; run targeted checks only when useful.
- Do not add dependencies, formatters, generated artifacts, or automation unless explicitly asked.
- Do not commit, push, open PRs, delete or rename non-generated files, or change config outside the repo unless explicitly asked.
- For multi-file, structural, or cross-cutting changes, propose the plan before editing. Local single-file changes can proceed directly.
