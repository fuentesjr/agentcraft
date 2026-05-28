# Agentcraft

A public lab notebook for developing practical strategies, workflows, and systems for working with AI agents.

This repository collects my experiments, patterns, playbooks, guides, cheatsheets, and reflections on AI engineering — especially the parts that sit between raw model capability and real-world usefulness: context design, review loops, orchestration, human-in-the-loop workflows, and agentic system design.

It also hosts the **Agentcraft Field Guide**: a Markdown-first, GitHub Pages publishing layer for the most durable and reusable material.

## Why this exists

There is still a huge amount of unexplored surface area in how we work with AI.

Not just prompts, but systems. Workflows. Interfaces. Review loops. Context strategies. Orchestration patterns. The small decisions that make the difference between AI feeling impressive and AI being genuinely useful.

Agentcraft is where I collect, test, and refine those ideas.

Some ideas will be rough. Some will be tested. Some will turn into repeatable playbooks. Others will fail. That is part of the point: this repo is a place to think in public, document what I am learning, and build a library of patterns for higher-leverage human-AI collaboration.

## What is in here

This repo is a mix of:

- Exploratory ideas
- Proposals and v0 plans for ideas being shaped
- Experiments and results
- Reusable patterns
- Playbooks and templates
- Cheatsheets and guides
- Notes on what works, what does not, and when

The most reusable material should eventually live in `cheatsheets/`, `guides/`, `patterns/`, `playbooks/`, and `templates/`. Earlier-stage thinking belongs in `ideas/`, `proposals/`, and `experiments/`.

## Agentcraft Field Guide

Agentcraft is the workshop: rough ideas, proposals, experiments, failures, and evolving notes.

The Field Guide is the shelf: durable cheatsheets, playbooks, guides, patterns, and templates that are useful enough to revisit, reuse, or share.

```text
Agentcraft = where thinking happens
Field Guide = where durable lessons are surfaced
```

Publishing is an explicit promotion step. Rough notes do not need to be site-ready by default.

## How ideas evolve

The goal is not to collect clever prompts. The goal is to understand which systems reliably improve judgment, focus, and execution.

A typical idea may move through:

```text
ideas/ → proposals/ → experiments/ → patterns/playbooks/guides/cheatsheets/templates → Field Guide navigation
```

`proposals/` is for canonical plans that are more mature than raw ideas but not necessarily ready to become standalone repositories. Not everything graduates. Failed experiments are useful when they explain what was tried, what happened, and what changed as a result.

## Areas I care about

A few recurring themes in this repo:

- Context engineering
- Agentic workflows
- Orchestrator patterns
- PR review processes
- Human-in-the-loop systems
- Designing for leverage, not just automation

## How to read this repo

This repo is intentionally a mix of rough and refined material:

- `ideas/` contains early-stage questions, bets, and concepts.
- `proposals/` contains canonical plans for ideas being shaped into concrete experiments, tools, or systems.
- `experiments/` contains bounded tests, results, and reflections.
- `cheatsheets/` contains fast references for commands, concepts, prompts, and workflows.
- `guides/` contains explanatory and practical docs.
- `patterns/` contains distilled workflows and system designs.
- `playbooks/` contains practical operating guides.
- `templates/` contains reusable prompts, checklists, and scaffolding.

If you want the most polished material, start with the Field Guide homepage, `cheatsheets/`, `guides/`, `patterns/`, and `playbooks/`. If you want to see the thinking process, start with `ideas/`, `proposals/`, and `experiments/`.

## Local site check

The Field Guide is a small Jekyll/GitHub Pages site. To verify the rendered site locally without writing generated files into the repo:

```bash
jekyll build --destination /tmp/agentcraft-field-guide-site
```

## Working structure

```text
agentcraft/
├── README.md
├── index.md        # Field Guide homepage
├── _config.yml     # GitHub Pages / Jekyll config
├── cheatsheets/    # Fast references
├── guides/         # Explanatory and practical docs
├── ideas/          # Raw concepts, questions, and bets
├── proposals/      # Canonical plans for ideas being shaped
├── experiments/    # Bounded tests, results, and reflections
├── patterns/       # Repeatable workflows and system designs
├── playbooks/      # Practical operating guides
└── templates/      # Prompts, checklists, and scaffolds
```

This structure is a starting point, not a contract. It should evolve as the work becomes clearer.

## What this demonstrates

This repo is also a record of how I approach AI engineering: forming hypotheses, testing workflows, documenting tradeoffs, and turning useful discoveries into repeatable systems.

The bias here is not toward automation for its own sake. The bias is toward systems that improve human judgment, reduce coordination drag, and create leverage without hiding responsibility.

## The goal

My goal is to build a practical body of work around using AI well — especially in ways that improve judgment, focus, and execution.

Over time, I want this repo to become a collection of tested strategies for building and working with agentic systems in the real world.
