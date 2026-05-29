# Session handoff skill

Status: raw idea.

A portable agent skill, using the agent skill standard, that makes it easy to generate compact, high-fidelity handoff context documents when moving work to a new AI-agent session.

The handoff document should preserve the relevant state of the work without dragging the whole prior conversation forward.

## Core question

> Can an agent skill reliably compress an active session into a small handoff document that preserves the context a future agent actually needs?

## Short version

The skill generates a handoff file at:

```text
.context/YYYYMMDD-topic-handoff.md
```

The document should help a new session quickly understand:

- what the user is trying to accomplish
- what has already happened
- what decisions were made
- what files, branches, commands, sources, or artifacts matter
- what remains uncertain
- what the next agent should do first

The workflow should be:

```text
messy active session → inferred compact handoff → new session resumes with high-signal context
```

The skill should infer first and ask questions only when important context is missing or ambiguous.

## Why this matters

Long AI-agent sessions accumulate useful context and noise at the same time.

By the time a session needs to be restarted, the important details are often scattered across:

- conversation history
- repo state
- diffs
- commands that passed or failed
- implementation notes
- decisions made in chat
- research links or source references
- partially completed next steps

Copying the full transcript into the next session is expensive and noisy. Starting fresh loses important context. A handoff document is the middle path: compact enough to read, faithful enough to resume safely.

## The core tension

The hard part is:

```text
compact context + maximum fidelity
```

These can feel contradictory, but they are not exactly opposites.

The goal is not to preserve every token. The goal is to preserve every **decision-relevant fact** and enough provenance for a future agent to verify or recover detail.

A good handoff is high-fidelity because it includes:

- exact goals and constraints
- decisions and rationale
- current repo/work state
- file paths and symbols, not vague descriptions
- commands run and outcomes
- unresolved risks and open questions
- next actions in priority order
- pointers to source material instead of pasted bulk

A bad handoff is long but low-fidelity when it includes lots of prose without saying what changed, what matters, or what to do next.

## Target users

This is for humans and agents doing work that may need to continue in another session:

- coding tasks
- design/planning work
- research tasks
- writing/documentation work
- debugging sessions
- multi-agent orchestration
- long-running project work

The skill should handle both coding and non-coding work. It should infer the right handoff shape from the current context, then ask only if the work type or next step is genuinely unclear.

## Skill behavior

### Activation examples

The skill should activate when the user says things like:

- “Create a handoff.”
- “Generate session handoff context.”
- “I need to restart this session.”
- “Write a compact context doc for the next agent.”
- “Summarize where we are so another agent can continue.”

### Default behavior

The skill should:

1. Inspect available context.
2. Infer the work type and topic slug.
3. Inspect repo state when relevant.
4. Generate a compact handoff document under `.context/`.
5. Report the path and any assumptions.
6. Ask follow-up questions only if the missing information would materially reduce handoff quality.

For coding repositories, useful automatic inspection may include:

```bash
git status --short
git branch --show-current
git log --oneline -5
git diff --stat
git diff --name-only
```

It may also inspect relevant files, implementation notes, recent test output, or documentation when those are clearly connected to the task.

For non-coding work, the skill should focus more on:

- goal and audience
- decisions made
- sources consulted
- claims and evidence
- unresolved questions
- next writing/research/planning steps

## Handoff document shape

A useful v0 template:

```markdown
# Handoff: <topic>

Generated: YYYY-MM-DD
Work type: coding | research | planning | writing | mixed
Status: in-progress | blocked | ready-for-review | complete

## Goal

What the user is trying to accomplish.

## Current state

What has already happened and where things stand now.

## Important context

Only the context needed to continue safely.

## Decisions made

- Decision: ...
  Rationale: ...

## Files and artifacts

- `path/to/file` — why it matters

## Commands and verification

- `command` — passed/failed/not run, with short outcome

## Open questions and risks

- ...

## Next steps

1. ...
2. ...
3. ...

## Recovery pointers

Where to look if more detail is needed.
```

The exact sections should adapt to the work. A non-coding research handoff may replace “Files and artifacts” with “Sources and evidence.” A coding handoff should include branch, diff, tests, and relevant file paths.

## Compactness tactics

To keep the document small without losing value:

- Prefer bullet points over narrative.
- Prefer exact file paths, commands, and links over pasted content.
- Include snippets only when the exact text matters.
- Separate facts from assumptions.
- Preserve rationale for decisions, not the whole debate.
- Use “recovery pointers” for detail that a future agent can re-open if needed.
- Include negative context: what was considered and intentionally not done.
- Keep next steps concrete and ordered.

A possible target:

```text
Default handoff: 1–2 pages
Complex handoff: 3–5 pages max, with recovery pointers instead of bulk transcript
```

## Fidelity tactics

To preserve fidelity:

- Record exact branch and repo status when applicable.
- Mention whether the working tree is clean or dirty.
- List changed files and why they matter.
- Include test/check commands and outcomes.
- Record known failed attempts and current hypothesis.
- Keep user constraints explicit.
- Include unresolved risks instead of smoothing them over.
- Link to canonical docs, specs, PRs, issues, or source URLs.

The handoff should make it hard for the next agent to accidentally redo work, reverse a decision, or miss a constraint.

## Naming convention

Default path:

```text
.context/YYYYMMDD-topic-handoff.md
```

Examples:

```text
.context/20260528-agentcraft-field-guide-handoff.md
.context/20260528-rails-auth-refactor-handoff.md
.context/20260528-pricing-research-handoff.md
```

Rules:

- Use the local date unless otherwise configured.
- Use a short kebab-case topic slug.
- If the topic is unclear, infer from the main goal.
- If multiple topics are active, ask which one the handoff is for.
- Do not overwrite an existing handoff without confirmation.

## Relationship to implementation notes

A handoff is not the same as `implementation-notes.md`.

`implementation-notes.md` is a running work log for the current task.

A handoff is a compressed resume packet for a future session.

The skill may read implementation notes and distill them, but it should not blindly copy them.

## Relationship to memory

The handoff document is project-local and task-specific. It should not replace durable cross-session memory.

If the session reveals a durable user preference, project convention, or lesson, the agent may separately suggest recording it in the appropriate memory system. The handoff itself should stay focused on resuming the current work.

## Non-goals for v0

Do not start with:

- a database
- embeddings
- automatic transcript ingestion
- a complex UI
- multi-file handoff bundles
- huge generated archives
- pretending compression is perfect
- hiding uncertainty to make the handoff feel cleaner

The v0 should be a simple skill that writes a clear Markdown document.

## Risks

### Too verbose

If the handoff is too long, the next agent will skim or ignore it. Countermeasure: default to bullets, exact references, and recovery pointers.

### Too lossy

If the handoff omits decisions, failed attempts, or constraints, the next agent may repeat mistakes. Countermeasure: include decisions, rationale, known failures, and open risks.

### False confidence

The skill may infer incorrectly. Countermeasure: ask only when important information is missing or ambiguous, and label assumptions clearly.

### Coding-only bias

The skill may overfit to git/diff workflows. Countermeasure: detect work type and adapt sections for research, writing, planning, or mixed work.

## Open questions

- Should `.context/` be committed by default, or should projects decide whether to ignore it?
- Should the skill maintain a `.context/README.md` explaining the handoff convention?
- Should handoffs include a “copy this into the next session” block at the top?
- Should the skill support multiple compactness levels, such as `brief`, `standard`, and `full`?
- How should it detect and summarize relevant conversation-only decisions that are not reflected in files?
- Should the skill optionally update an existing handoff instead of always creating a new one?

## Possible v0

Build the smallest useful agent skill standard package:

- `SKILL.md` with activation triggers and instructions
- a handoff template
- guidance for repo inspection
- guidance for coding vs non-coding work
- filename convention under `.context/`
- instruction to infer first and ask only on material ambiguity

Success means a new agent can read the generated handoff and continue the work with minimal re-exploration.
