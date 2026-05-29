# target-shape (axis primitive)

## Purpose

How the loop's optimization target is defined. The single most load-bearing
axis (weight 3): it decides whether the loop is terminal or open-ended, and it
gates the forbidden-divergence and contradiction checks.

## Values

- `finite-criteria` — a frozen, machine-checkable list with a terminal pass
  line. "Done" is a known set of things that must be true at once.
- `frontier-expanding` — a quality frontier that moves (better / more robust /
  higher score) with no terminal pass line.
- `promise-discovery` — user-facing product promises discovered and verified
  one at a time, before a finite implementation target exists.
- `discovery-reframing` — the target itself is undefined and phase-shifts as
  intent reveals ("X-adjacent, I'll know it when I see it").

## Detection heuristics

| Task phrasing | Value |
|---|---|
| "make these criteria pass", "implement this spec", "close this checklist" | `finite-criteria` |
| "make it better / faster / more robust", "raise the score", no pass line | `frontier-expanding` |
| "verify the product does what it promises", "which stories are real" | `promise-discovery` |
| "build me something X-adjacent", "discover the right shape" | `discovery-reframing` |

## Archetype defaults

| Archetype | Default |
|---|---|
| frontier | `frontier-expanding` |
| goal | `finite-criteria` |
| story | `promise-discovery` |
| greenfield | `discovery-reframing` |

## Composition rules

- `finite-criteria` forces `halt-shape: terminal`, `convergence-shape:
  criteria-completion`, and `artifact-shape: acceptance-inventory`.
- `finite-criteria` is **forbidden** for `frontier` and `greenfield` — a fixed
  finite target is goal-shaped; route to `goal`.
- `frontier-expanding` and `discovery-reframing` are **forbidden** for `goal` —
  a moving target with no pass line is not goal-shaped.
- **Contradiction** (classification error → ask the user, no silent default):
  `finite-criteria` with `halt-shape: equilibrium`/`manual-gated`, or with
  `convergence-shape: frontier-exhaustion`.
