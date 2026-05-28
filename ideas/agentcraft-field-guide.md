# Agentcraft Field Guide

Status: raw idea.

Agentcraft Field Guide is a proposed GitHub-hosted website for my personal AI cheatsheets, playbooks, guides, patterns, and templates.

The core idea is to make Agentcraft both a working lab notebook and a useful reference library without forcing every note to become polished public content.

## Core question

> Can Agentcraft become both a personal AI reference library and a public publishing layer without losing its lab-notebook character?

## Short version

Agentcraft remains the workshop: rough ideas, proposals, experiments, failures, and evolving notes.

The Field Guide becomes the shelf of durable tools: selected cheatsheets, playbooks, guides, patterns, and templates that are useful enough to revisit or publish.

```text
Agentcraft = where thinking happens
Field Guide = where durable lessons are surfaced
```

The website should be generated from Markdown and hosted with GitHub Pages. Markdown remains the canonical source of truth. HTML is a rendering target, not the primary authoring format.

## Why this is interesting

I want a place where I can quickly find and reuse my own practical AI knowledge:

- coding-agent workflows
- context engineering patterns
- review loops
- prompt and task templates
- orchestration patterns
- human-in-the-loop systems
- notes on what works, what fails, and why

But I also want the best material to be public, navigable, and useful to other people.

This creates two related but distinct jobs:

1. **Personal reference library** — fast, searchable, useful to me.
2. **Publishing layer for Agentcraft** — curated, reader-facing, useful to others.

The bet is that these can share the same Markdown source if the repo keeps clear boundaries between rough working notes and polished published material.

## Relationship to Agentcraft

This should not replace the lab notebook premise. It should reinforce it.

Agentcraft should continue to contain rough and unfinished work:

```text
ideas/        raw concepts, questions, and bets
proposals/    shaped plans and v0 scopes
experiments/  bounded tests, results, and reflections
patterns/     reusable system designs
playbooks/    practical operating guides
templates/    reusable prompts, checklists, and scaffolds
```

The Field Guide would publish or highlight the material that has become durable enough to reuse.

A useful boundary:

```text
Lab notebook = messy, exploratory, chronological, honest
Field guide  = curated, navigable, durable, reader-facing
```

Rough notes should not need to be site-ready. Publishing should be an explicit promotion step, not an ambient pressure on every document.

## Content taxonomy

Start with five content types:

- **Cheatsheets** — fast reference for commands, concepts, prompts, or workflows.
- **Playbooks** — repeatable operating procedures for practical AI work.
- **Guides** — explanatory documents that teach a concept or approach.
- **Patterns** — distilled reusable designs or workflows.
- **Templates** — reusable prompts, checklists, scaffolds, and forms.

Cut for now:

- tutorials
- courses

Those may come later, but they imply more structure and maintenance than the idea needs at v0. If needed, courses can start as lightweight learning paths: curated sequences of existing pages.

## Publishing approach

The default approach should be boring and durable:

```text
Markdown source → static site generator → GitHub Pages
```

Likely v0 choice:

- Markdown as canonical content
- Jekyll or another simple static site generator for rendering
- GitHub Pages for hosting
- repo-local metadata to mark what is published

AI coding agents can help draft, refactor, cross-link, and polish pages, but they should not generate one-off interactive HTML as the primary architecture.

Interactive pages may be useful later, but they should be deliberate enhancements:

- copyable prompt blocks
- expandable examples
- checklists
- small decision trees
- reusable JavaScript components

The base system should stay editable as plain Markdown.

## Possible metadata

If the same repo acts as both lab notebook and published site, documents may eventually need lightweight metadata:

```yaml
---
title: Context Engineering Cheatsheet
type: cheatsheet
status: usable
published: true
tags: [context-engineering, coding-agents]
---
```

This could allow the website to render only selected material while leaving rough notes in place.

A possible status scale:

```text
rough → draft → usable → polished
```

Publishing should probably require an explicit `published: true` rather than inferring from directory alone.

## Same repo or separate repo?

Current leaning: keep v0 inside `agentcraft`.

Reasons:

- the Field Guide is naturally a publishing layer for Agentcraft
- the workflow from rough idea to reusable material stays visible
- there is less sync friction
- the site can start small without becoming a separate product

A separate repo may make sense later if:

- the website becomes its own brand or product
- the build system becomes heavy
- public material needs to be separated from private notes
- Agentcraft becomes too noisy for the published site

For now, the simplest model is:

```text
agentcraft repo = source + lab notebook + published site source
GitHub Pages    = rendered public view
```

## v0 scope

A minimal useful version could be:

- decide the site name and positioning
- add a small set of published sections
- publish a homepage explaining the Field Guide
- expose selected cheatsheets, playbooks, guides, patterns, and templates
- keep rough `ideas/`, `proposals/`, and `experiments/` out of the main published navigation by default
- use Markdown-first authoring
- deploy through GitHub Pages

Possible first sections:

```text
Cheatsheets
Playbooks
Guides
Patterns
Templates
```

The first version does not need search, interactivity, a custom design system, or courses.

## Non-goals for v0

Do not start with:

- tutorials
- courses
- a custom app
- a backend
- complex content pipelines
- AI-generated one-off HTML pages
- heavy branding work
- publication pressure on every lab note
- private notes or sensitive personal material

The goal is to publish useful, durable AI working knowledge while preserving the freedom to think messily in the repo.

## Open questions

- Should the website be called `Agentcraft Field Guide`, `AI Field Manual`, or something else?
- Should published pages live in existing directories or under a dedicated `site/` or `docs/` directory?
- Should frontmatter be added only to published pages or eventually to all substantial documents?
- Is Jekyll good enough, or would another Markdown-first tool fit better?
- What is the smallest initial set of pages worth publishing?
- Should cheatsheets and guides be new top-level directories, or should they map onto existing `patterns/`, `playbooks/`, and `templates/`?

## Next steps

1. Decide whether v0 lives entirely inside this repo.
2. Choose the site name and static-site tool.
3. Define the minimum content metadata.
4. Create a small homepage and navigation structure.
5. Promote one or two existing pieces into publishable Field Guide pages.
6. Keep the lab notebook and Field Guide boundary explicit.
