# Agentcraft Field Guide

Status: Active v0 proposal.

Agentcraft Field Guide is the active plan for turning Agentcraft into both a personal AI reference library and a public publishing layer, without losing the repo's lab-notebook character.

## Core thesis

> Agentcraft is the workshop. The Field Guide is the shelf of tools that survived contact with reality.

Agentcraft should continue to hold rough thinking, proposals, experiments, failures, and reflections. The Field Guide should surface selected material that is durable enough to revisit, reuse, or share publicly.

```text
Agentcraft = where thinking happens
Field Guide = where durable lessons are surfaced
```

The Field Guide should be Markdown-first, GitHub-hosted, and simple enough to maintain with normal repo workflows.

## Problem

I want one place to accumulate practical AI working knowledge:

- coding-agent workflows
- context engineering patterns
- review loops
- prompt and task templates
- orchestration patterns
- human-in-the-loop systems
- notes on what works, what fails, and why

But there are two different jobs hiding inside that desire:

1. **Personal reference library** — fast, searchable, useful to me while doing real work.
2. **Public publishing layer** — curated, navigable, useful to other people.

If these are merged carelessly, the repo becomes a content junk drawer or every note starts feeling like it needs to be polished for publication. If they are separated too aggressively, the publishing layer drifts away from the actual lab notebook.

The design challenge is to keep one coherent source of truth while making the boundary between rough working notes and polished reference material explicit.

## Goals

- Preserve Agentcraft as a public lab notebook.
- Add a curated Field Guide layer for durable AI references.
- Keep Markdown as the canonical authoring format.
- Host the rendered website on GitHub Pages.
- Make publishing an explicit promotion step.
- Keep the first version boring and maintainable.
- Let AI agents help draft, refactor, cross-link, and polish content without making generated HTML the core architecture.

## Non-goals for v0

Do not start with:

- tutorials
- courses
- a custom web app
- a backend
- a complex content pipeline
- a custom design system
- AI-generated one-off HTML pages as the primary architecture
- heavy branding work
- search
- private notes
- publication pressure on every lab note

Courses can come later as lightweight learning paths: curated sequences of existing pages. They do not belong in v0.

## Settled v0 decisions

### Keep v0 in this repo

The Field Guide should start inside `agentcraft`, not as a separate repository.

Reasons:

- it is naturally a publishing layer for Agentcraft
- the rough-to-reusable workflow stays visible
- there is less synchronization friction
- the site can start small before becoming a separate product

A separate repo may make sense later if the Field Guide becomes a standalone brand, needs a heavier build system, or needs to separate public material from private notes.

### Markdown is canonical

Markdown files are the source of truth. HTML is an output format.

AI agents may help create or improve Markdown, but the project should avoid one-off generated HTML pages that are hard to maintain or edit.

### Use boring GitHub Pages-style publishing first

The v0 publishing architecture should be:

```text
Markdown source → Jekyll/GitHub Pages → rendered website
```

Prefer GitHub Pages and Jekyll because they are native to the hosting target and keep the moving parts small. If Jekyll becomes constraining later, revisit the static-site generator choice after there is real content and real friction.

### Publishing is explicit

Rough notes should not accidentally become Field Guide pages just because they exist in the repo.

A published page should opt in with lightweight metadata or explicit navigation. The exact filtering mechanism can stay simple in v0, but the intent should be clear: not every document is part of the curated guide.

### Tutorials and courses are cut from v0

Start with:

- cheatsheets
- playbooks
- guides
- patterns
- templates

Do not add tutorials or courses yet.

## Content model

Agentcraft keeps its current lab-notebook structure:

```text
ideas/        raw concepts, questions, and bets
proposals/    shaped plans and v0 scopes
experiments/  bounded tests, results, and reflections
patterns/     reusable system designs
playbooks/    practical operating guides
templates/    reusable prompts, checklists, and scaffolds
```

The Field Guide highlights the durable side of the repo:

- **Cheatsheets** — fast reference for commands, concepts, prompts, or workflows.
- **Playbooks** — repeatable operating procedures for practical AI work.
- **Guides** — explanatory documents that teach a concept or approach.
- **Patterns** — distilled reusable designs or workflows.
- **Templates** — reusable prompts, checklists, scaffolds, and forms.

Likely v0 additions:

```text
cheatsheets/  # fast references
guides/       # explanatory/practical docs
```

Existing `patterns/`, `playbooks/`, and `templates/` can serve both the lab notebook and Field Guide when individual documents are promoted.

## Boundary between notebook and guide

The important distinction is maturity and reader intent:

```text
Lab notebook = messy, exploratory, chronological, honest
Field Guide  = curated, navigable, durable, reader-facing
```

Working notes can be rough. Field Guide pages should be clearer, more stable, and more directly useful.

A document can move through this path:

```text
ideas/ → proposals/ → experiments/ → patterns/playbooks/templates/guides/cheatsheets → Field Guide navigation
```

Not everything needs to graduate. Failed experiments are valuable when they explain what was tried, what happened, and what changed as a result.

## Metadata convention

Use minimal frontmatter when a page is intended for the Field Guide:

```yaml
---
title: Context Engineering Cheatsheet
type: cheatsheet
status: usable
published: true
tags: [context-engineering, coding-agents]
---
```

Recommended fields:

- `title` — human-readable page title
- `type` — `cheatsheet`, `playbook`, `guide`, `pattern`, or `template`
- `status` — `draft`, `usable`, or `polished`
- `published` — `true` only when the page is meant for the Field Guide
- `tags` — short topic tags

Avoid over-modeling. Frontmatter exists to support publishing and navigation, not to turn the repo into a CMS.

## Site shape

A minimal rendered site should have:

```text
Home
├── Cheatsheets
├── Playbooks
├── Guides
├── Patterns
└── Templates
```

The homepage should explain the relationship between Agentcraft and the Field Guide:

- Agentcraft is the lab notebook.
- The Field Guide is the curated reference layer.
- The material is practical, field-tested when possible, and honest about uncertainty.

The first site does not need search, complex filters, or interactivity.

## Suggested repo shape for v0 implementation

The implementation can stay close to the current repo layout:

```text
agentcraft/
├── README.md
├── AGENTS.md
├── _config.yml          # future Jekyll config
├── index.md             # future Field Guide homepage
├── cheatsheets/         # new Field Guide content type
├── guides/              # new Field Guide content type
├── patterns/
├── playbooks/
├── templates/
├── ideas/               # lab notebook, not main guide navigation
├── proposals/           # lab notebook, not main guide navigation
└── experiments/         # lab notebook, not main guide navigation
```

Use the root as the GitHub Pages/Jekyll source if that avoids duplicating Markdown content. Exclude or omit rough sections from the main site navigation by default.

If root publishing creates too much friction, revisit a `docs/` source directory or GitHub Actions build later. Do not start there unless the simpler path fails.

## AI agent role

AI agents should help with:

- turning rough notes into clearer guide pages
- proposing content structure
- checking consistency between related pages
- adding relative links
- drafting cheatsheets and playbooks from existing material
- identifying gaps and stale assumptions

AI agents should not:

- polish every rough note by default
- generate elaborate HTML one-offs unless explicitly requested
- add dependencies or build tooling casually
- hide uncertainty or make untested ideas sound proven

## v0 acceptance criteria

A successful v0 should have:

- a clear Field Guide homepage
- a small navigation structure
- at least one useful page in two or more Field Guide content types
- Markdown as the canonical source
- GitHub Pages publishing working
- rough lab-notebook sections preserved and not treated as failures
- `AGENTS.md` updated so future agents preserve the boundary

## Risks

### Content junk drawer

If everything is included, the guide becomes noisy. Countermeasure: make publishing explicit and curate the homepage/navigation.

### Polishing pressure

If every idea feels public-site-ready, the lab notebook gets worse. Countermeasure: preserve rough sections and treat promotion as a separate step.

### Tooling gravity

A static-site generator can become the project. Countermeasure: use boring GitHub Pages/Jekyll first and avoid custom machinery until content demands it.

### AI-generated slop

Agents may over-polish, over-abstract, or generate generic advice. Countermeasure: require concrete examples, uncertainty, and evidence from experiments or actual use.

## Open questions

- Is `Agentcraft Field Guide` the final site name, or should it be `AI Field Manual` / another name?
- Should `cheatsheets/` and `guides/` be added immediately, or only when the first pages are ready?
- Should published pages be selected only by navigation, by `published: true`, or both?
- What are the first two or three pages worth publishing?
- Should the initial GitHub Pages setup use root publishing or a dedicated `docs/` source?

## Fast-track next steps

1. Treat this proposal as the canonical plan for the Field Guide.
2. Update `AGENTS.md` to point future agents here.
3. Add initial `cheatsheets/` and `guides/` directories when the first content is ready.
4. Choose the first two publishable pages.
5. Add the simplest possible Jekyll/GitHub Pages scaffold.
6. Publish v0, then improve only where real friction appears.
