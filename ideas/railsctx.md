# railsctx

`railsctx` is an idea for a Rails-aware context packet compiler for AI coding agents.

The core question:

> Can Rails conventions produce better AI coding context than generic code search?

## Problem

AI coding agents often struggle less because they lack intelligence and more because they receive poor context.

Generic retrieval can find files that mention the right words, but Rails applications have stronger signals than keywords:

- routes point to controller actions
- controller actions reference services, models, jobs, mailers, and views
- request specs describe behavior at the application boundary
- models expose validations, associations, callbacks, and schema constraints
- package systems such as Packwerk define ownership and boundary rules

Most agents do not need the whole app. They need a small, high-signal slice of the app with clear reasons for why each file matters.

## Core idea

Build a CLI that turns a Rails task into a compact **context packet** for an AI coding agent.

Example:

```bash
railsctx feature "Implement billing upgrade for POST /accounts/:id/upgrade"
```

Instead of doing broad semantic search for `billing`, `upgrade`, and `account`, `railsctx` would follow Rails structure:

```text
POST /accounts/:id/upgrade
→ AccountsController#upgrade
→ referenced services/models/jobs
→ relevant request spec
→ package boundary notes
```

The output is not an answer and not an autonomous agent. It is a prepared context artifact that another coding agent can use more effectively.

## What is a context packet?

A context packet is a small, explicit bundle of task-relevant information:

- the task being worked on
- the likely entry point
- the files to inspect first
- short code snippets from those files
- why each file was included
- tests likely worth running
- assumptions and uncertainty
- optional follow-up retrieval if more context is needed

The important property is not just inclusion. It is **provenance**: every file should have a reason.

## Why Rails is a good target

Rails has conventions that make shallow, exact retrieval unusually powerful:

- HTTP routes map to controller actions.
- Controller actions often reveal orchestration.
- Zeitwerk maps constants to file paths.
- Request specs often map to user-visible behavior.
- Active Job, Action Mailer, and views leave recognizable call sites.
- Active Record models expose useful domain constraints through familiar DSLs.
- Packwerk, when present, adds package ownership and boundary information.

This means a useful first version does not need embeddings, a graph database, or a full Ruby call graph.

## v0 scope

The first version should be intentionally small:

```text
route → controller action → referenced constants → nearby request spec → compact markdown packet
```

A v0 command could look like:

```bash
railsctx feature "Implement billing upgrade" --route "POST /accounts/:id/upgrade"
```

And produce a Markdown packet with:

- matched route
- controller/action file and snippet
- obvious referenced constants from the action body
- likely request spec candidates
- tests to run
- uncertainty notes

## Non-goals for v0

Do not start with:

- embeddings
- generic RAG
- full dependency graphs
- autonomous agent behavior
- production trace integrations
- PR history mining
- profiler ingestion
- full Packwerk enforcement
- perfect static analysis of Ruby

The goal is to test whether Rails-aware structural context beats generic retrieval for common coding-agent tasks.

## Design principles

1. **Artifact first**  
   Design the packet the agent receives before designing the index.

2. **Exact Rails anchors beat fuzzy recall**  
   A route, controller action, spec, or stack frame is often more valuable than many keyword matches.

3. **Small by construction**  
   The packet should fit comfortably inside an agent prompt. If context is uncertain, suggest follow-up retrieval instead of dumping files.

4. **Every file needs a reason**  
   `Contains billing` is not enough. `Route target`, `failing spec`, or `constant referenced by controller action` is better.

5. **No false precision**  
   Static Ruby analysis cannot reliably produce a complete call graph for a real Rails app. The tool should present shallow evidence, not pretend to know the entire execution path.

6. **Uncertainty should be explicit**  
   If the route was inferred, a spec was guessed, or a constant was matched by convention only, the packet should say so.

## Example packet shape

```markdown
# railsctx context packet

## Task
Implement billing upgrade for `POST /accounts/:id/upgrade`.

## Entry point
- Route: `POST /accounts/:id/upgrade`
- Controller: `AccountsController#upgrade`
- File: `app/controllers/accounts_controller.rb`

## Files to inspect first

### `app/controllers/accounts_controller.rb`
Why: route target for the requested endpoint.

```ruby
def upgrade
  Billing::Subscriptions.upgrade_account(
    account: @account,
    plan: params.require(:plan)
  )

  SyncBillingAccountJob.perform_later(@account.id)
  render json: { status: "upgraded" }
end
```

### `spec/requests/accounts/upgrade_spec.rb`
Why: likely request spec for the route.

## Tests to run
- `bundle exec rspec spec/requests/accounts/upgrade_spec.rb`

## Uncertainty
- The request spec was inferred by path and should be verified.
- Billing package boundaries were not inspected in v0.

## Retrieve more only if needed
- Billing public API files if the controller delegates to `Billing::*`.
- Job implementation if the side effect behavior is part of the task.
```

## Open questions

- Is route-based context enough to improve coding-agent performance on real Rails tasks?
- What is the smallest packet that still changes agent behavior?
- Should v0 include snippets only, or also file-level summaries?
- How often do Rails conventions fail because of custom routing, metaprogramming, or unconventional service layout?
- How should packet quality be evaluated: task success, reduced agent turns, fewer irrelevant edits, or human judgment?

## Possible next experiment

Manually create context packets for a few real Rails tasks and compare them against generic agent context selection.

Success would mean the Rails-aware packet:

- includes fewer irrelevant files
- surfaces the true entry point faster
- gives the coding agent better tests to run
- reduces unnecessary exploration
- makes uncertainty clearer to the human operator
