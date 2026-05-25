# railsctx v0 context packet experiment

## Status

Planned

## Source idea

See [`../ideas/railsctx.md`](../ideas/railsctx.md).

## Hypothesis

A Rails-aware context packet built from route, controller, and spec conventions gives an AI coding agent better starting context than generic repository search.

Specifically, it should help the agent:

- find the real entry point faster
- inspect fewer irrelevant files
- identify the most useful test sooner
- understand uncertainty instead of assuming a complete call graph
- avoid broad, low-signal keyword-driven exploration

## Why this matters

AI coding agents are often bottlenecked by context selection. In Rails apps, generic search can over-select files that merely share domain terms like `account`, `billing`, or `upgrade`.

Rails has stronger structural anchors:

```text
route → controller action → referenced constants → likely request spec
```

This experiment tests whether that shallow Rails-aware path is already enough to produce a useful packet.

## Experiment design

This experiment should be manual-first. Before building a CLI, manually create context packets for a small number of Rails tasks and compare them against generic context selection.

## Test cases

Use 2–3 Rails tasks. They can come from a real app, a toy app, or a representative fixture app.

Good task shapes:

1. Feature work from a route
   - Example: `Implement billing upgrade for POST /accounts/:id/upgrade`
2. Bug fix from a failing request spec
   - Example: `Fix spec/requests/accounts/upgrade_spec.rb`
3. Small behavior change in a controller/service path
   - Example: `Send confirmation email after account upgrade`

## v0 input shape

The minimal input should be explicit enough to avoid guessing the seed:

```bash
railsctx feature "Implement billing upgrade" --route "POST /accounts/:id/upgrade"
```

For the manual experiment, the equivalent input is:

```text
Task: Implement billing upgrade
Route: POST /accounts/:id/upgrade
```

## v0 packet shape

Each manually generated packet should include:

- task
- matched route
- controller/action file and snippet
- referenced constants from the action body
- likely request spec candidates
- tests to run
- uncertainty notes
- follow-up retrieval suggestions

## Packet template

```markdown
# Context packet: <task slug>

## Task
<one-sentence task description>

## Entry point
- Route: `<HTTP verb> <path>`
- Controller action: `<Controller>#<action>`
- File: `<path>`

## Files to inspect first

### `<path>`
Why: <specific reason>

```ruby
<short relevant snippet>
```

## Referenced constants
- `<Constant>` — <why it matters / confidence>

## Likely tests
- `<test command>` — <reason>

## Uncertainty
- <what is inferred, missing, or needs verification>

## Retrieve more only if needed
- <follow-up context request> — <when to retrieve it>
```

## Baseline comparison

For each task, compare the Rails-aware packet against a generic search approach.

Generic search baseline examples:

```bash
rg "billing|upgrade|account"
find app spec -iname "*account*"
find app spec -iname "*billing*"
```

Record:

- files found by generic search
- files included in the packet
- files that were useful
- files that were distracting
- important files either approach missed

## Success criteria

The Rails-aware packet is successful if it:

- identifies the true entry point
- includes the primary controller/action snippet
- includes or suggests the most relevant request spec
- includes fewer irrelevant files than generic search
- gives the coding agent an obvious first test command
- clearly labels inferred or uncertain context

## Failure criteria

The approach is weak if:

- the route cannot be resolved reliably
- the controller action does not reveal useful next files
- the likely spec cannot be found or guessed
- generic search finds the right context just as quickly
- the packet omits an essential file needed for the task
- the packet creates false confidence about an incomplete execution path

## Measurements

For each task, record:

| Metric | Rails-aware packet | Generic search |
|---|---:|---:|
| Total files suggested | | |
| Useful files suggested | | |
| Distracting files suggested | | |
| True entry point found? | | |
| Relevant test found? | | |
| Uncertainty documented? | | |

Optional qualitative notes:

- Did the packet make the next action obvious?
- Did it reduce exploratory file reads?
- Did it prevent irrelevant edits?
- Did it expose package or architectural boundaries?

## Expected result

The expected result is not a perfect context packet. The expected result is evidence that even a shallow Rails-aware slice can outperform broad keyword retrieval for common Rails coding-agent tasks.

If this holds, the next step is to automate the smallest useful version:

```text
route lookup → controller snippet → referenced constants → likely request spec → markdown renderer
```

## Next step after experiment

If 2–3 manual packets are useful, create a minimal CLI prototype that supports one command:

```bash
railsctx feature "<task>" --route "<VERB> <PATH>" --md
```

The prototype should produce Markdown only. JSON schema, caching, Packwerk, runtime traces, and richer indexing can wait.
