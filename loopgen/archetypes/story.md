# story (archetype)

## Loop shape

A story-frontier loop. Each iteration discovers one user-facing promise, makes
narrow reversible product judgments when needed (logging the alignment cost for
human review), implements or verifies **one focused slice**, captures evidence,
and reconciles the storyboard — then continues. It is not broad QA or a
project-management digest; it biases toward actual story / feature movement.

## Default primitive values

| Axis | Default |
|---|---|
| `target-shape` | `promise-discovery` |
| `halt-shape` | `checkpoint-with-reopen` |
| `artifact-shape` | `storyboard` |
| `convergence-shape` | `capstone-plus-closer` |
| `cadence-shape` | `chapter` |
| `consult-capability` | detect (`tier-0` default) |

`halt-shape: checkpoint-with-reopen` and `convergence-shape:
capstone-plus-closer` are this archetype's defaults (the bodytxt learning) —
which is why bodytxt (a story loop with a `frontier-expanding` target) **sources
them from `story`**, not as brand-new values.

## Forbidden divergences

- `artifact-shape ≠ storyboard` — erases the archetype's identity; the
  storyboard is the durable artifact the whole loop revolves around.
- Treating the storyboard as the source of intent — it is an **index**
  (`primitives/queue-as-second-artifact.md`); re-check authority sources first.

## Failure modes

- **Stale storyboard as authority** — an old row treated as current truth.
  Mitigation: re-check at least one attached authority source before verifying.
- **Re-verifying easy stories** — the frontier-novelty rule: a `verified` story
  is ineligible for re-selection without a recorded trigger.
- **Broad QA masquerade** — exploratory sweeps called story verification.
  Mitigation: the no-ready-story fallback is frontier-prep, not QA.
- **Verifying unaligned candidates** — Mitigation: the Ready Gate
  (intent-ready + proof-ready) before verification.
- **Same-family semantic-cleanup drift** — low-visibility fixes crowd out story
  movement. Mitigation: the same-family drift guard (`last_story_family` +
  `same_family_count`).
- **Missing fixture-mode / unparseable evidence manifest** — Mitigation:
  declare fixture mode before capture; classify `fixture-gap` / `env-gap`.

## Extras

- Emits `docs/storyboard.md` as the required artifact, plus a per-promoted-story
  evidence manifest.
- **Evidence modality tracks the surface class, not the storyboard.** The
  storyboard (the promise index) is surface-agnostic; only its proof modality
  varies: screenshots / DOM for a web frontend, a command transcript + exit code
  for a CLI, contract tests + schema fixtures for an API, terminal snapshots for
  a TUI. Classifying `story` for a non-UI surface is sound; emitting UI-only
  evidence for it is the overclaim to avoid.
- The **Surface Taste Lane** section is conditional — include only when the lane
  is the taste lane.
- Body template: `templates/bodies/story-body.md`.
