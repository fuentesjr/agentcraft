---
layout: default
title: From Lab Notebook to Field Guide
published: true
type: guide
status: usable
tags: [agentcraft, publishing, knowledge-work]
summary: How Agentcraft turns rough working notes into durable public reference material.
---

# From Lab Notebook to Field Guide

Agentcraft has two jobs that should support each other without collapsing into one another.

```text
Agentcraft = where thinking happens
Field Guide = where durable lessons are surfaced
```

The lab notebook is allowed to be messy. The Field Guide should be useful to return to.

## Why keep both?

If everything is a polished article, the repo becomes slow and performative. Rough ideas get filtered too early.

If everything is a rough note, useful lessons become hard to find and reuse.

The answer is not to choose one. The answer is to preserve the boundary.

## The maturity path

A useful piece of work may move through this path:

```text
ideas/ → proposals/ → experiments/ → patterns/playbooks/guides/cheatsheets/templates → Field Guide navigation
```

That path is descriptive, not mandatory. Some ideas should stay rough. Some experiments should fail and stop. Some patterns should never become playbooks.

## What belongs in the lab notebook?

Use the lab notebook for:

- raw questions
- bets and hunches
- early proposals
- experiments and results
- uncertainty
- failed attempts
- working notes that are not ready for reuse

A lab note is successful if it captures the thinking honestly enough that future work can build on it.

## What belongs in the Field Guide?

Use the Field Guide for material that is durable enough to reuse:

- cheatsheets for fast reference
- playbooks for repeatable workflows
- guides for explanations
- patterns for reusable designs
- templates for prompts, checklists, and scaffolds

A Field Guide page should be clear about its maturity. Not everything needs to be polished, but it should be intentionally useful.

## Promotion checklist

Before promoting a note into the Field Guide, ask:

- Is this useful more than once?
- Is the target reader clear?
- Does it contain concrete steps, examples, or decisions?
- Does it distinguish evidence from speculation?
- Does it preserve relevant uncertainty?
- Would a future agent know how to use or extend it?

If the answer is mostly yes, promote it.

## Metadata convention

Published Field Guide pages should use lightweight frontmatter:

```yaml
---
title: Example Page
type: guide
status: usable
published: true
tags: [example]
---
```

Keep metadata boring. It exists to support navigation and publishing, not to turn the repo into a CMS.

## The rule of thumb

Do not make rough notes site-ready by default.

Do make durable lessons easy to find.
