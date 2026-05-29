# frontload-audit (shared primitive)

## Purpose

The universal pre-flight every archetype runs before composing. Loops fail when
they self-terminate blocked on a decision the user could have made before the
loop fired; the cure is to **frontload every uncertainty** so iteration runs
through the night without blocking.

## Include when

Phase 1, always. Its output — the frontload preamble — is prepended to every
composed prompt.

## The resolve / default / escalate procedure

For each checklist item, do exactly one of:

1. **Resolve now** — if the host has an AskUserQuestion-style tool, use it;
   otherwise print the uncertainty prominently so the user can answer before
   launching.
2. **Default + Alignment Review** — pick the smallest reversible default and
   record problem · options · chosen contract · rollback trigger. Reversible at
   iteration time.
3. **Escalate-mark** — only for the truly irreversible (paid APIs without budget
   caps, public-publish, secrets, product direction with unclear rollback).

## Universal checklist (shared by all archetypes)

- **Motive** — one-sentence goal.
- **Evidence surface** — build / test / run commands; artifact / ledger /
  findings paths.
- **Scope manifest** — allowed / forbidden globs, or `none`.
- **Known false-green zones** — tests / suites that pass without validating.
- **Forbidden shortcuts** — `--no-verify`, mocked integration, assertion-free
  fixtures, snapshot refresh without semantic proof.
- **Artifact locations** — where the queue, traces, and metrics live.
- **Consult availability** — detect the `consult-capability` tier here.
- **Parameters / thresholds** — quiet-signal N, stuck-attempt N, cash-out N.

Each archetype adds its own items (named in `archetypes/*.md`): frontier —
evaluator tier, ramp stages, two reward channels, frontier vector; goal —
criteria source, authority order, final-verify, dependency topology; story —
lane, surface class, storyboard path, fixtures, app URL; greenfield — target
adjacency, INTENT hypotheses, model identity, paid APIs, preloop checklist.

## Derivation-gap concept

Anything neither resolved, defaulted, nor escalate-marked is a **derivation
gap** — a future block waiting to happen. The emitted prompt's
`halt-cause-classifier` flags `derivation-gap` halts so the next derivation
pass closes them.

## Composition notes

- Output: record under `loop/STATE.md` `frontload:` and prepend a frontload
  preamble to `loop/PROMPT.md` naming exactly what was resolved / defaulted /
  left open.
- `cadence-shape: deferred-fire-and-forget` raises the completeness bar — no
  human is awake to answer a gap.
