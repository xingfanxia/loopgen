# Runner Contract (shared primitive)

**Purpose.** Declares that the emitted prompt is runner-agnostic — `/loop`,
`/goal`, and external harnesses (gnhf, cocc, ralph) all re-invoke it; they
differ only in invocation. Establishes the four things the prompt assumes
about its host and the boundary between a runner ceiling and a repository
failure.

**Include when.** Every composed prompt, every archetype, unconditionally.
This block is universal.

**Placeholders.** None — substituted verbatim.

**Source of truth.** Byte-identical to the `## Runner contract` block in the
retired `frontier-loop` / `goal-loop` / `story-loop` prompt templates
(verified at hoist time). After retirement, this file is canonical.

---

## Runner contract

This prompt is runner-agnostic. A *runner* re-invokes this prompt
iteratively; `/loop`, `/goal`, and external harnesses (gnhf, cocc, ralph)
are all runners. The prompt assumes only:

1. Iterative re-invocation — you are one iteration.
2. File-persisted state — durable progress lives in named files, not memory.
3. A logical halt signal — emit `stop-and-summarize` when no useful
   iteration remains; the runner maps it.
4. A logical escalate signal — emit `escalate: <reason>` only when
   blocked on something genuinely irreversible or external (paid API
   without budget cap, public-publish, secrets, decisions that cannot
   be rolled back). Reversible judgment is not escalation — see the
   judgment default.

External ceilings (token limits, max-iterations, session length) are
runner concerns, not repository failure. Preserve the worktree and
summarize unresolved work for the next run.
