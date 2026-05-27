# ctxpack

Status: Draft v0 proposal.

`ctxpack` is a deterministic Rails-aware context packet compiler for AI coding agents.

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

Build a CLI that turns an exact Rails anchor into a compact **context packet** for an AI coding agent.

Example workflow:

```bash
bin/rails routes -g upgrade
ctxpack packet accounts#upgrade \
  --name billing_upgrade_accounts_upgrade \
  --task "Implement billing upgrade"
```

`ctxpack` should not replace Rails' existing route discovery tools. Rails already answers "what routes exist?" `ctxpack` starts after the developer has chosen a Rails-native anchor and answers:

> Given this controller action, what compact, evidenced Rails context should an agent receive?

Instead of doing broad semantic search for `billing`, `upgrade`, and `account`, `ctxpack` follows Rails structure:

```text
AccountsController#upgrade
→ controller action snippet
→ referenced constants/services/models/jobs
→ likely request spec candidates
→ package/boundary notes when cheaply detectable
```

The output is not an answer and not an autonomous agent. It is a prepared context artifact that another coding agent can use more effectively.

## Settled v0 direction

The first version should be intentionally small and deterministic:

```text
controller#action → controller action snippet → referenced constants → nearby request spec → compact markdown packet
```

Primary command:

```bash
ctxpack packet accounts#upgrade \
  --name billing_upgrade_accounts_upgrade \
  --task "Implement billing upgrade"
```

By default, the command should save a migration-style context artifact and print its path:

```text
docs/ctxpack/20260527143015_billing_upgrade_accounts_upgrade.md
```

The anchor is an exact Rails controller action, using the same shape shown by `bin/rails routes`.

Possible later extension, only if it stays simple:

```bash
ctxpack packet --helper upgrade_account --task "Implement billing upgrade"
```

But route helper support is not required for v0. The first version should avoid route-string input such as `POST /accounts/:id/upgrade` as the happy path because it creates shell quoting issues and invites route typos.

## What ctxpack should not duplicate

Do not build a custom route browser in v0.

Use Rails for route discovery:

```bash
bin/rails routes -g upgrade
bin/rails routes -c AccountsController
```

Then use `ctxpack` for context compilation:

```bash
ctxpack packet accounts#upgrade \
  --name billing_upgrade_accounts_upgrade \
  --task "Implement billing upgrade"
```

This writes a durable point-in-time context artifact under `docs/ctxpack/` and prints the saved path.

This keeps the responsibility split clear:

```text
Rails:
  discover routes, helpers, and controller actions

ctxpack:
  compile a small, evidenced context packet from a known Rails anchor
```

## What is a context packet?

A context packet is a small, explicit bundle of task-relevant information:

- the task being worked on
- the exact Rails anchor
- the likely entry point
- the files to inspect first
- short code snippets from those files
- why each file was included
- tests likely worth running
- assumptions and uncertainty
- optional follow-up retrieval if more context is needed

The important property is not just inclusion. It is **provenance**: every file should have a reason.

## Determinism

`ctxpack` should make packet construction deterministic by default:

```text
same repo state + same packet inputs = same normalized packet content
```

That means:

- deterministic file ordering
- stable snippet ranges
- templated reason codes and reason text
- no model-generated summaries
- no fuzzy autonomous retrieval
- no hidden agent judgment in packet construction
- no generated timestamps inside packet content

The default artifact filename is the exception: it should use a Rails-migration-style timestamp for chronological ordering and collision resistance. The path is a storage concern, not part of the packet's semantic content. Evals can use `--out` or normalize the output path when checking determinism.

Skills or sub-agents may consume the packet later, but they should not be responsible for constructing the canonical packet.

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

## v0 packet contents

A v0 packet should include:

- the requested task
- the exact `controller#action` anchor
- the controller/action file and snippet
- obvious constants referenced by the action body
- files resolved from those constants when Zeitwerk naming makes that cheap and exact
- likely request spec candidates
- tests to run
- uncertainty notes
- follow-up retrieval suggestions only when more context is needed

The packet should be Markdown because humans and agents are the primary readers.

## Artifact location and naming

Context packets should be saved as named artifacts instead of written to a generic `context.md` file.

They are not disposable scratch files, but they are also not evergreen documentation that should be manually maintained forever. Treat them as durable point-in-time task records: reviewable, linkable from PRs/issues, and superseded by newer packets when the code or task context changes.

Default output directory:

```text
docs/ctxpack/
```

This makes the artifact intentionally durable and keeps it near other project documentation. Projects that do not want to commit generated context packets can ignore this directory or use `--dir`/`--out` for a different location.

Default filename shape:

```text
YYYYMMDDHHMMSS_<context_name>.md
```

The timestamp should match the familiar Rails migration style: chronological, sortable, and collision-resistant. The name should describe the feature, bug, or context, using snake case.

Example:

```bash
ctxpack packet accounts#upgrade \
  --name billing_upgrade_accounts_upgrade \
  --task "Implement billing upgrade"
```

writes:

```text
docs/ctxpack/20260527143015_billing_upgrade_accounts_upgrade.md
```

If `--name` is omitted, derive a snake_case name from the task and anchor:

```bash
ctxpack packet accounts#upgrade --task "Implement billing upgrade"
```

writes something like:

```text
docs/ctxpack/20260527143015_implement_billing_upgrade_accounts_upgrade.md
```

Rules:

- prefer an explicit `--name` for clear feature/bug/context naming
- use snake_case names to resemble Rails migration filenames
- include enough context in the name to avoid vague artifacts like `upgrade.md`
- include a timestamp in the default filename for ordering and collision resistance
- do not include generated timestamps inside packet content
- do not silently overwrite an existing artifact; require `--force` or an explicit `--out`
- allow `--dir` or `--out` for callers that want a different location

## Machine-readable manifest

Markdown should be the main artifact. A structured manifest only earns its keep if it keeps evals simple.

If needed, generate a small manifest from the same internal packet object and save it next to the Markdown packet:

```bash
ctxpack packet accounts#upgrade \
  --name billing_upgrade_accounts_upgrade \
  --task "Implement billing upgrade" \
  --manifest
```

writes:

```text
docs/ctxpack/20260527143015_billing_upgrade_accounts_upgrade.md
docs/ctxpack/20260527143015_billing_upgrade_accounts_upgrade.json
```

The manifest is not a second product surface in v0. It exists so evals can assert stable fields without parsing Markdown prose.

Example manifest fields:

```json
{
  "version": 1,
  "anchor": "accounts#upgrade",
  "entrypoint": {
    "file": "app/controllers/accounts_controller.rb",
    "controller": "AccountsController",
    "action": "upgrade"
  },
  "files": [
    {
      "path": "app/controllers/accounts_controller.rb",
      "reason_code": "controller_action",
      "snippet_ranges": [[24, 39]]
    }
  ],
  "tests": [
    {
      "command": "bundle exec rspec spec/requests/accounts/upgrade_spec.rb",
      "reason_code": "request_spec_candidate"
    }
  ],
  "uncertainty": [
    {
      "code": "spec_inferred_by_path"
    }
  ]
}
```

If evals can use the internal packet object directly, the public `--manifest` flag can wait.

## Simple v0 evals

Evals should be part of v0, but they should stay boring and deterministic.

A fixture case can be a small YAML file:

```yaml
name: accounts_upgrade
command:
  anchor: accounts#upgrade
  task: Implement billing upgrade

expect:
  entrypoint:
    file: app/controllers/accounts_controller.rb
    action: upgrade

  include:
    - path: app/controllers/accounts_controller.rb
      reason_code: controller_action
    - path: spec/requests/accounts/upgrade_spec.rb
      reason_code: request_spec_candidate

  exclude:
    - app/controllers/admin/accounts_controller.rb

  tests:
    - bundle exec rspec spec/requests/accounts/upgrade_spec.rb

  max_files: 6
```

The eval runner should check:

- correct entry point
- required files included
- forbidden files excluded
- expected reason codes present
- expected test commands suggested
- packet stays under file/snippet limits
- running the same command twice with a fixed `--out`, or with output paths normalized, produces the same content hash

Do not use an LLM judge in v0. Every packet bug should become a small deterministic eval case.

## Non-goals for v0

Do not start with:

- embeddings
- generic RAG
- custom `ctxpack routes` command
- interactive route pickers
- freeform route-string parsing as the primary UX
- generic root-level outputs such as `context.md`
- treating packets as disposable `tmp/` scratch files by default
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
   A controller action, route helper, request spec, or stack frame is often more valuable than many keyword matches. v0 should start with exact `controller#action` anchors.

3. **Do not duplicate Rails**  
   Use `bin/rails routes` for route discovery. `ctxpack` should compile context, not become a parallel Rails route UI.

4. **Small by construction**  
   The packet should fit comfortably inside an agent prompt. If context is uncertain, suggest follow-up retrieval instead of dumping files.

5. **Every file needs a reason**  
   `Contains billing` is not enough. `Controller action`, `request spec candidate`, or `constant referenced by action` is better.

6. **No false precision**  
   Static Ruby analysis cannot reliably produce a complete call graph for a real Rails app. The tool should present shallow evidence, not pretend to know the entire execution path.

7. **Uncertainty should be explicit**  
   If a spec was guessed, a constant was matched by convention only, or multiple routes map to the action, the packet should say so.

## Example packet shape

````markdown
# ctxpack context packet

## Task
Implement billing upgrade.

## Anchor
- Action: `accounts#upgrade`
- Controller: `AccountsController#upgrade`
- File: `app/controllers/accounts_controller.rb`

## Files to inspect first

### `app/controllers/accounts_controller.rb`
Why: controller action for the requested anchor.
Reason code: `controller_action`

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
Why: likely request spec for `accounts#upgrade`.
Reason code: `request_spec_candidate`

## Tests to run
- `bundle exec rspec spec/requests/accounts/upgrade_spec.rb`

## Uncertainty
- The request spec was inferred by path and should be verified.
- Route discovery is delegated to Rails; run `bin/rails routes -g upgrade` if the exact endpoint matters.
- Billing package boundaries were not inspected in v0.

## Retrieve more only if needed
- Billing public API files if the controller delegates to `Billing::*`.
- Job implementation if the side effect behavior is part of the task.
````

## Relationship to skills and sub-agents

A skill or sub-agent can be useful as a wrapper around `ctxpack`, for example:

1. run `ctxpack packet accounts#upgrade`
2. read the generated packet
3. use the packet as the starting context for implementation or review

But the skill or sub-agent should not be the canonical packet builder. Keeping packet construction in a deterministic CLI makes the system easier to measure, diff, and improve.

## Open questions

- Is `controller#action` enough for v0, or is exact route-helper support needed early?
- What is the smallest packet that still changes agent behavior?
- Should v0 include snippets only, or also deterministic file-level metadata?
- How often do Rails conventions fail because of custom routing, metaprogramming, or unconventional service layout?
- Which packet limits best predict usefulness: file count, line count, token estimate, or irrelevant-file count?

## Possible next experiment

Build the smallest possible `ctxpack packet <controller#action>` prototype against one or two fixture Rails apps.

Success would mean the Rails-aware packet:

- includes fewer irrelevant files
- surfaces the true entry point faster
- gives the coding agent better tests to run
- reduces unnecessary exploration
- makes uncertainty clearer to the human operator
- produces stable output that can be regression-tested with simple eval cases
