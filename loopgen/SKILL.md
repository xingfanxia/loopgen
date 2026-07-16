---
name: loopgen
description: "Opt-in only: create or repair a lightweight autonomous loop prompt for a repository. Use when the user explicitly asks for /loopgen, an overnight/autonomous loop, or diagnosis of a drifting loop. Supports finite delivery and explicitly requested frontier discovery. Reuses existing acceptance/status/verification artifacts and avoids mandatory ceremony. Do not auto-trigger for ordinary implementation, planning, debugging, review, or vague 'improve this repo' requests."
---

# Loopgen Lite

Create the smallest durable loop contract that can keep a long task aligned.
Loopgen authors the loop; it does not run it.

The default is a finite delivery loop. Frontier discovery is opt-in and must
have a named signal, bounded checkpoint, and separate discovery queue.

## Principles

- Preserve one project-level Goal when the objective remains stable. Milestones
  and PRs do not require new Goals.
- Manage long work through a durable milestone or acceptance ledger, not chat
  history alone.
- Reuse repo-native sources of truth. Do not duplicate a PRD, audit report,
  acceptance checklist, status file, or verification log merely to fit a
  template.
- Validation must prove changed behavior or completion. Review must target a
  concrete risk. Omit ceremony that cannot change a decision or catch a defect.
- Keep discovery and finite delivery separate. A frontier may propose work; it
  may not silently turn a bounded acceptance loop into an infinite runner.
- Do not prescribe agent count, model, review rounds, commit cadence, or thread
  rotation unless the task actually depends on them.

## Choose The Runner First

Do not turn every repeated action into an autonomous loop.

- Use an ordinary turn for short, exploratory, or one-off work where the user
  still owns the next check.
- Use `/goal` for bounded multi-turn work with a stable objective and
  verifiable completion. Point it at `loop/PROMPT.md` only when the durable
  contract is too large for the goal itself.
- Use a scheduled continuation when work must revisit external state and prior
  task context matters. Use standalone scheduled runs when each input window is
  independent.
- Use an unattended mutating routine only after the same prompt, tools, model
  route, and permissions succeed on a representative manual slice. Prefer
  idempotent actions and an isolated worktree for repository mutations.

If an ordinary turn or direct `/goal` is sufficient, report that decision and
do not create loop artifacts. For scheduled work, define the cadence from how
often the source can change, the freshness/delta and no-op checks, and separate
the stop condition for one run from retirement of the recurring routine.

## Modes

Choose one mode decisively.

### Finite delivery (default)

Use when success can be stated as a bounded acceptance set: a refactor,
checklist, audit closure, migration, release milestone, or defined product
slice.

Completion is terminal: all in-scope acceptance items are proven, required
validation passes, and remaining items are explicitly out of scope or blocked.

### Frontier discovery (explicit only)

Use only when the user explicitly wants ongoing exploration, benchmark
improvement, product discovery, or repeated audit against fresh signals.

Require all three:

1. a named signal or evaluator;
2. a bounded checkpoint (time, iteration, cost, or milestone);
3. a discovery queue separate from any finite delivery acceptance ledger.

The checkpoint ends the current run. It may recommend reopening later; it is
not an instruction to run forever.

If a request mixes a finite checklist with an open-ended frontier, emit two
contracts or keep the frontier as a read-only candidate generator. Never share
one runner state or one completion rule between them.

## Authoring flow

1. Inspect the repository and the named task source.
2. Find existing acceptance, plan, status, audit, and verification artifacts.
3. Choose the runner, then decide finite delivery or explicit frontier
   discovery.
4. Bind the objective, trigger when applicable, scope boundary, evidence source,
   validation commands, approval boundary, and stop rules.
5. Write or revise `loop/PROMPT.md`.
6. Reuse an existing durable ledger. Create `loop/STATE.md` only when no
   suitable state or milestone ledger exists.
7. Read the emitted prompt once as a cold-start runner. Remove ambiguity,
   duplication, and checks that do not affect acceptance.

Ask at most one focused question, and only when a missing value would materially
change scope, authorization, or the definition of done. Otherwise make the
smallest safe assumption and record it.

## Required prompt contract

`loop/PROMPT.md` should normally fit within 80–160 lines and contain only the
sections below.

### Objective

State the user-visible outcome in one paragraph. Preserve the project-level
objective; name the current milestone separately.

### Runner and trigger

Name the selected runner and what starts one run. For scheduled work, also name
the cadence, one run's input window, freshness/delta checkpoint, no-op result,
and the separate condition that retires the recurring routine.

### Sources of truth

Link the existing artifacts that define requirements, acceptance, current
state, and validation. Mark one artifact as authoritative for each concern.
Do not copy their contents into the prompt.

### Scope and authority

Name what the runner may inspect, edit, and validate without asking. Require
confirmation for external writes, destructive actions, purchases, or material
scope expansion.

### Iteration

Each iteration:

1. read the current acceptance/state ledger and relevant diff;
2. choose the highest-value actionable item that advances the current
   milestone;
3. implement the smallest coherent slice;
4. run the most focused validation that can disprove the change;
5. update acceptance/state with result and evidence;
6. decide whether to continue, verify the milestone, or stop.

For a scheduled runner, each run must first check source freshness and the
actionable delta. Exit cheaply when nothing changed, and do not repeat a
mutation for the same source item or checkpoint.

Independent reads may be parallelized. Dependent work stays sequential. Spawn
agents only when there is real parallel work or independent-evidence value;
pass a bounded task brief instead of full history by default.

### Review and validation budget

- Self-review the relevant diff once before marking an item done.
- Add one focused independent review only for a named correctness, data,
  contract, architecture, or regression risk.
- Use adversarial/specialist review only for auth, permissions, secrets,
  private data, payments, persistence migrations, concurrency, or public
  protocols.
- Run focused tests during implementation. Run a full repository gate once at
  the final relevant diff only when the change is broad/high-risk or the user
  requested a merge/release gate.
- Reuse valid evidence while the diff is unchanged. Do not repeat review or
  validation solely to clear low, nit, or informational findings.
- If validation cannot run, record why and the best available substitute.

### State update

Use the repo's existing ledger if possible. When `loop/STATE.md` is necessary,
keep only:

```yaml
objective: stable project outcome
mode: finite-delivery | frontier-discovery
acceptance_source: path or URL
current_milestone: bounded milestone
completed: []
evidence: []
open: []
decisions: []
failed_attempts: []
next_action: one concrete action
stop_reason: null
```

Update it at milestone changes or material findings, not after routine tool
calls. Do not store transcript summaries or duplicate the acceptance source.

### Stop rules

Stop and report when one of these is true:

- finite delivery acceptance is proven;
- the explicit frontier checkpoint is reached;
- required evidence or authorization is missing;
- the next action would expand scope materially;
- the current loop shape is wrong for the work;
- repeated attempts show a genuine blocker.

For recurring routines, distinguish these outcomes: a run may exit because
there is no fresh actionable delta while the routine remains active; retire or
cancel the routine only at its explicit external terminal condition or on user
request.

Before stopping for a blocker, check the remaining in-scope acceptance set for
another actionable item. Report completed work, evidence, open items, blocker,
and the smallest next decision.

Do not treat elapsed days, cumulative tokens, compactions, or thread age alone
as loss of control. Diagnose loss of control from repeated work, acceptance
drift, reopening completed items, stale-context mistakes, missing evidence, or
failure to converge.

## Diagnostic mode

When repairing an existing loop, inspect its prompt, state, acceptance source,
and recent evidence. Return a short diagnosis before editing:

- finite work using equilibrium/infinite stop semantics;
- discovery and delivery sharing runner state;
- duplicated or conflicting sources of truth;
- completed items repeatedly reopened;
- validation/review repeated without new diff or risk;
- state dominated by transcript history instead of decisions and evidence;
- missing authority, evidence, or terminal condition.

Make the smallest repair. Preserve valid artifacts and evidence. Do not
regenerate the whole loop merely because an older template has more sections.

## Output

Return:

1. runner, mode, and one-sentence rationale;
2. files created or revised;
3. reused sources of truth;
4. assumptions or blockers;
5. the exact runner invocation or schedule draft, normally `/goal read loop/PROMPT.md`.

Do not start a goal or create a scheduled task unless the user separately asks
to run or schedule it.
