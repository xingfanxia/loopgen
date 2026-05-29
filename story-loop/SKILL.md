---
name: story-loop
description: "Superseded by /loopgen. Shorthand for a STORY-shaped loop: maintains a living product storyboard of user-facing promises — extract, reconcile, implement or verify one focused story per iteration with evidence from prompts, docs, code, and the observed UI. Routes to /loopgen started from the story archetype. Triggers: '/story-loop', 'story loop', 'walk the storyboard', 'verify the product promise across states'."
---

# story-loop → /loopgen (story)

**Superseded by [`/loopgen`](../loopgen/SKILL.md).** One skill now composes all
four loop archetypes from a single primitive vocabulary.

`/story-loop X` is shorthand for **`/loopgen X` started from the `story`
archetype**. loopgen still classifies the task and may diverge where it demands —
recording every divergence in its provenance preamble — so you get the
story-frontier loop you expect, plus hybrid coverage when the task isn't purely
story.

The story loop's actual definition lives in loopgen:

- shape, defaults, forbidden divergences — `loopgen/archetypes/story.md`
- emitted prompt body (the per-iteration playbook) —
  `loopgen/templates/bodies/story-body.md`
