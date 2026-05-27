# ctxpack

`ctxpack` is a proposal for a deterministic, Rails-aware context packet compiler for AI coding agents.

Status: promoted from raw idea to v0 proposal. The canonical plan now lives in [`../proposals/ctxpack.md`](../proposals/ctxpack.md).

## Core question

> Can Rails conventions produce better AI coding context than generic code search?

## Short version

Given an exact Rails anchor such as `accounts#upgrade`, `ctxpack` produces a compact, evidenced Markdown context packet for an AI coding agent.

The key bet is that Rails structure can select better initial context than broad keyword search:

```text
controller#action → action snippet → referenced constants → likely request spec → context packet
```

## Current direction

- Build a deterministic CLI, not a skill or autonomous agent.
- Use Rails' own tools, such as `bin/rails routes`, for route discovery.
- Start v0 with exact `controller#action` anchors.
- Save durable point-in-time artifacts under `docs/ctxpack/` using Rails-migration-style filenames.
- Keep evals simple, deterministic, and non-LLM-judged.

See the proposal for the full v0 plan.
