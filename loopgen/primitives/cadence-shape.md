# cadence-shape (axis primitive)

## Purpose

The human-checkpoint / expensive-check rhythm. Weight 1 — rarely flips the
nearest-archetype pick, but it shapes when consults fire and how complete the
frontload must be.

## Values

- `sync` — cheap channel every iteration; human / expensive check at completion.
- `checkpoint-gated` — expensive outer channel or user-look gate every N
  iterations (the frontier quiet-signal checkpoint; the green-field ~50-iter
  user-look).
- `chapter` — work batched into narrative chapters, each ending with a closer /
  review milestone (the bodytxt overnight-chapter shape).
- `deferred-fire-and-forget` — fully autonomous overnight; summarize only at the
  end.

## Detection heuristics

| Task phrasing | Value |
|---|---|
| terminal / sync target | `sync` |
| "run overnight, check periodically" | `checkpoint-gated` |
| "chapters / episodes / sections" | `chapter` |
| "run all night unattended, summarize in the morning" | `deferred-fire-and-forget` |

## Archetype defaults

| Archetype | Default |
|---|---|
| frontier | `checkpoint-gated` |
| goal | `sync` |
| story | `chapter` |
| greenfield | `checkpoint-gated` |

## Composition rules

- Low weight: a cadence mismatch alone does not re-classify the archetype; it
  becomes a divergence on the nearest archetype.
- Pairs with `consult-capability`: a chapter or checkpoint boundary is the
  natural place to schedule a consult when tier ≥ 2.
- `deferred-fire-and-forget` raises the bar on `frontload-audit` completeness —
  no human is awake to answer a derivation gap, so every uncertainty must be
  resolved or defaulted, never left open.
