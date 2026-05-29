---
name: goal-loop
description: "Superseded by /loopgen. Shorthand for a GOAL-shaped loop: a terminal/oracle loop that closes a finite acceptance inventory with per-criterion verifiers and one final-verify, halting when all pass without weakening the criteria. Routes to /loopgen started from the goal archetype. Triggers: '/goal-loop', 'goal loop', 'close the findings in X', 'fix the bugs by morning', 'definition of done'."
---

# goal-loop → /loopgen (goal)

**Superseded by [`/loopgen`](../loopgen/SKILL.md).** One skill now composes all
four loop archetypes from a single primitive vocabulary.

`/goal-loop X` is shorthand for **`/loopgen X` started from the `goal`
archetype**. loopgen still classifies the task and may diverge where it demands —
recording every divergence in its provenance preamble — so you get the terminal
oracle loop you expect, plus hybrid coverage when the task isn't purely goal.

The goal loop's actual definition lives in loopgen:

- shape, defaults, forbidden divergences — `loopgen/archetypes/goal.md`
- emitted prompt body — `loopgen/templates/bodies/goal-body.md`
- oracle principles (binary oracle, oracle independence, anti-theater) —
  `loopgen/references/oracle-principles.md`
