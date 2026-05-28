---
layout: default
title: Rails AI Coding Agents Cheatsheet
type: cheatsheet
status: usable
published: true
tags: [rails, coding-agents, ai-workflows]
summary: A Rails-focused quick reference for safely using AI coding agents in real codebases.
---

# Rails AI Coding Agents Cheatsheet

A quick Rails-specific reference for using AI coding agents without handing them the keys to the app.

## Core rule

The agent is:

- fast
- useful
- often plausible
- not accountable

The engineer still owns:

- problem framing
- Rails-specific correctness
- authorization and data safety
- migration and rollout safety
- final review confidence

When the task is ambiguous, architectural, or risky, slow down and make the agent plan first.

## Default loop

Use this for most non-trivial Rails work:

1. **Question-only orientation** — understand the current flow before edits.
2. **Planning-only proposal** — ask for files, risks, tests, and rollout concerns.
3. **One small implementation step** — do not let the agent sprawl.
4. **Run narrow tests/checks** — prefer the smallest meaningful verification.
5. **Review the diff** — check behavior, scope, and Rails conventions.
6. **Repeat or reset** — continue only while the agent stays grounded.

The loop is simple:

```text
orient → plan → small edit → test → review → repeat
```

## Prompt modes

### Question-only

Use when entering unfamiliar code or checking current behavior.

```text
Just a question. Do not edit files.
Explain how invoice authorization currently works.
Point me to the controller, policy, query object, and specs.
```

### Planning-only

Use before multi-file, risky, or ambiguous changes.

```text
Planning only. No edits yet.
Review the ticket and propose the safest Rails implementation plan.
Call out auth risk, migration risk, callback/transaction risk, and test strategy.
```

### One-step implementation

Use after approving a plan.

```text
Implement only step 1 from the approved plan.
After editing, run the narrowest relevant test set.
Then summarize what changed and any failures.
```

### Scope validation

Use when the ticket is easy to over-interpret.

```text
Before editing, restate the exact scope in 5 bullets.
Also list 3 things you will not change.
```

### Diff review

Use before final human review.

```text
Run git diff and summarize:
- what changed,
- what remains incomplete,
- any cleanup needed,
- and any assumptions you made.
```

### Reset

Use when the session drifts.

```text
Start fresh.
Do not reuse prior assumptions.
This is planning-only.
Here is the exact goal, scope, and files.
First explain the current behavior before proposing edits.
```

## What to give the agent

Minimum useful context:

- business goal
- Rails app shape: monolith, engines, services, jobs, etc.
- relevant files or directories
- existing patterns to follow
- constraints and non-goals
- required tests
- risks to avoid

Strong prompt shape:

```text
We are in a Rails monolith with controllers, service objects, Sidekiq jobs, Pundit, and RSpec.
Task: allow admins to suspend a subscription.
Relevant files:
- app/controllers/admin/subscriptions_controller.rb
- app/policies/subscription_policy.rb
- app/services/subscriptions/
- spec/requests/admin/subscriptions_spec.rb
Constraints:
- Follow existing service-object conventions.
- Do not add a new abstraction unless necessary.
- Preserve audit logging.
- Avoid hidden callback behavior.
- Add focused request and service specs.
First, explain the current flow.
Then propose the smallest safe implementation.
```

Weak prompt:

```text
Add subscription suspension to Rails app.
```

The weak version invites guessing. The strong version gives the agent an operating environment.

## Rails low-trust zones

Slow down and verify aggressively when work touches:

- authorization and policies
- payments and billing
- migrations and backfills
- callbacks and validations
- transactions and locking
- background job retries
- caching correctness
- cross-service contracts
- incident debugging

These are not “never use AI” areas. They are “plan first, narrow scope, and verify” areas.

## Rails examples

### Fat controller refactor

Bad:

```text
Clean up this controller.
```

Better:

```text
Planning only. Do not edit files.
Review Admin::OrdersController#create.
Identify business logic that could move, but preserve policy checks, audit logging, and transaction boundaries.
Propose a two-step refactor with tests.
```

Human review focus:

- authorization order
- side effects
- transaction boundaries
- unnecessary abstractions

### Callback danger

Use this before refactoring model-heavy behavior:

```text
Before proposing a refactor, list all callbacks, validations, and after-commit behavior touched by this model.
Do not change behavior yet.
```

Human review focus:

- hidden email sends
- job enqueue timing
- audit events
- transaction semantics

### Migration safety

Bad:

```text
Generate a migration to backfill status on users.
```

Better:

```text
Planning only.
We need to add `account_status` to users and backfill existing rows.
Assume this table is large and hot.
Propose a safe Rails migration and rollout plan.
Call out locking, batching, deploy order, and rollback concerns.
```

Human review focus:

- concurrent indexes
- batched backfills
- staged deploy order
- defaults and null constraints
- rollback plan

## Verification checklist

Before accepting agent changes, check:

- Is the scope correct?
- Did it change more files than necessary?
- Did it follow Rails and team conventions?
- Did it preserve authorization and policy behavior?
- Did it introduce callback or transaction risk?
- Did it add meaningful tests, not test-shaped comfort?
- Did it avoid unnecessary dependencies and abstractions?
- Is the diff small enough to defend in review?
- Did the agent state what was verified and what remains unverified?

## High-value uses

Agents are most useful when the work is real but bounded:

- codebase exploration
- spec drafting
- small refactors
- repetitive test coverage
- log or stack-trace analysis
- first-pass query review
- boilerplate around service objects, serializers, and policies
- converting explicit TODOs into focused edits

Example:

```text
Implement all `#TODO(agent)` comments in the touched files only.
Follow existing service object and RSpec conventions.
After editing, summarize each TODO you completed.
```

## Signs the agent is off track

Reset or narrow the session when it:

- invents abstractions the codebase does not use
- ignores named files and edits unrelated areas
- answers questions by changing code
- writes tests that only confirm its assumptions
- reintroduces the same mistake
- sounds confident but cannot explain the current flow

Do not keep feeding a drifting context forever. Start a narrower session.

## Team rule of thumb

Good team-level agent rules are explicit, demonstrative, and checked in.

Include:

- preferred service object shape
- authorization conventions
- migration safety rules
- background job naming and retry expectations
- required tests for common change types
- examples of good and bad prompts

If engineers keep correcting the same agent mistake, write the rule down.

## One-page refresh

If short on time, remember:

- Ask for a plan before code.
- Use question-only mode for unfamiliar areas.
- Give files, constraints, and patterns.
- One logical step beats one-shot generation.
- Review each diff before review debt piles up.
- Slow down around auth, migrations, callbacks, and contracts.
- Require real tests, not happy-path theater.
- Reset when drift becomes obvious.
- Final responsibility stays with the engineer.

## References

- [Forgecode: AI agent best practices](https://forgecode.dev/blog/ai-agent-best-practices/)
- [Google Cloud: Five best practices for using AI coding assistants](https://cloud.google.com/blog/topics/developers-practitioners/five-best-practices-for-using-ai-coding-assistants)
- [Augment Code: Best practices for using AI coding agents](https://www.augmentcode.com/blog/best-practices-for-using-ai-coding-agents)
- [NextLink Labs: Setting up your Ruby on Rails monolith for AI development](https://nextlinklabs.com/resources/insights/setting-up-your-ruby-on-rails-monolith-for-ai-development)
- [Stack Overflow: Coding guidelines for AI agents and people too](https://stackoverflow.blog/2026/03/26/coding-guidelines-for-ai-agents-and-people-too/)
- [Microsoft Learn: AI app/agent best practices](https://learn.microsoft.com/en-us/partner-center/marketplace-offers/artificial-intelligence-app-agent-best-practices)
