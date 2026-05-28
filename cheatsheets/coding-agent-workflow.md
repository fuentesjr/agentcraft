---
layout: page
title: Coding Agent Workflow Cheatsheet
type: cheatsheet
status: usable
published: true
tags: [coding-agents, workflow, review]
summary: A quick checklist for getting useful work from AI coding agents without losing control.
---

# Coding Agent Workflow Cheatsheet

A quick reference for using AI coding agents on real repo work without turning the session into chaos.

## Before asking the agent

- Know the desired outcome, not just the task name.
- Identify whether the change is local, multi-file, architectural, or risky.
- Say whether the agent should plan first or proceed autonomously.
- Name any hard constraints: no new dependencies, no schema changes, no commits, no external calls.
- If there is a spec, point the agent to it and ask for implementation notes.

## Good task shape

Prefer:

```text
Review the repo and add a small v0 for X. Keep it Markdown-first, avoid new dependencies, and update docs. Plan first if the change crosses multiple files.
```

Avoid:

```text
Make this better.
```

Better prompts include:

- the goal
- the relevant files or area
- constraints
- what counts as done
- whether to commit or not

## During the session

- Let the agent inspect before editing.
- Ask for a plan before cross-cutting changes.
- Push back on broad rewrites or abstractions without concrete need.
- Prefer small working increments over a giant final patch.
- If the agent hits repeated failures, stop and ask for the hypothesis and recommended next move.

## Before accepting changes

Check:

- Did it solve the actual problem?
- Did it preserve existing conventions?
- Did it add unnecessary dependencies, config, or abstractions?
- Did it update relevant docs?
- Did it explain what was verified and what remains unverified?
- Are there new TODOs? If yes, do they have an owner or follow-up?

## Useful agent instruction snippets

```text
Be a constructive challenger. If the request seems overcomplicated, suggest a simpler path.
```

```text
Do not add dependencies or new tooling unless you ask first and explain the tradeoff.
```

```text
For multi-file changes, propose the plan before editing. For local single-file changes, proceed directly.
```

```text
Keep Markdown as the source of truth. Avoid generated artifacts unless explicitly requested.
```

## Failure mode to watch

The biggest risk is not that the agent is incapable. It is that the agent confidently expands the scope, adds polish that was not asked for, or hides uncertainty behind fluent prose.

Keep the loop grounded: inspect, plan, change, verify, summarize.
