# halt-cause-classifier (shared primitive)

## Purpose

When the loop emits `stop-and-summarize` or `escalate: <reason>`, it labels the
cause so the user — and the next derivation pass — can route it back. Unifies
the four legacy classifiers.

## Include when

Every composed prompt, under "Halt conditions." Emit the shared causes plus the
nearest archetype's terminal cause (a divergence may add a second archetype's
terminal cause).

## Shared causes (in every prompt)

- `derivation-gap` — blocked on something derivation could have asked for
  (model identity, budget cap, secret, path, fixture, …). **The feedback
  signal**: it tells the user the frontload checklist was incomplete; close it
  next run.
- `genuine-escalate` — irreversible / external / authority-needed (paid API
  budget, public-publish, secrets, product direction with unclear rollback,
  source conflict between authoritative-current sources).
- `signal-starvation` — quiet region: no new strong evidence (no typed trace,
  no reviewed finding, no metric movement, no user reframe) for the configured
  stretch; the quiet-signal checkpoint fired.
- `wrong-loop` — the work belongs in a different archetype (see reroute table).

## Archetype-terminal causes

| Archetype | Terminal cause(s) |
|---|---|
| frontier | `frontier-exhausted` |
| goal | `criteria-met` (success) · `partial-deadlock` · `oracle-drift` |
| story | `storyboard-converged` |
| greenfield | `stone-converged` |

## wrong-loop reroute targets

- finite checklist with a pass line → `goal`
- open-ended "make it better" with no pass line → `frontier`
- product-promise discovery / reconciliation → `story`
- target / artifact / evaluator undefined → `greenfield`

## Composition notes

- The terminal cause matches the archetype's `convergence-shape`.
- `derivation-gap` is what makes the `frontload-audit` self-improving across
  runs.
