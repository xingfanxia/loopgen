# loopgen

One skill that writes the prompt before an autonomous loop runs, so you don't wake up to a loop that died 10 minutes after you went to sleep.

The loop is run by a separate **runner** (`/loop`, `/goal`, gnhf, cocc, ralph). loopgen does not run the loop. It writes the prompt and the invariants. The same emitted prompt runs under any runner.

`/loopgen` reads your task, classifies it to the nearest **archetype**, then composes the emitted prompt from a **primitive vocabulary** — defaulting to the archetype's values and diverging only where the task demands. Every divergence is named in a provenance preamble, so the composition is auditable and a drifting loop can be diagnosed.

## The four archetypes

Pick by what you hand the loop — but you don't have to. `/loopgen` classifies for you, and composes a hybrid when the task sits between archetypes.

| Seed | Archetype | Halts on |
|---|---|---|
| A task with a definition of done | `goal` | `criteria-met` — one final-verify proves the frozen acceptance inventory |
| A benchmark to push | `frontier` | `frontier-exhausted` — five homeostasis axes balanced, no intervention left |
| A frontend to walk through | `story` | `storyboard-converged` — the visible product matches the storyboard |
| An idea to build out | `greenfield` | `stone-converged` — the artifact landed on the user's reframed target |

## Quickstart

```sh
git clone git@github.com:pro-vi/loopgen.git
ln -s "$PWD/loopgen/loopgen" ~/.claude/skills/loopgen   # Claude Code
ln -s "$PWD/loopgen/loopgen" ~/.codex/skills/loopgen    # Codex (optional)

# Optional muscle-memory shims — /frontier-loop, /goal-loop, /story-loop,
# /greenfield-loop each redirect to /loopgen with that archetype pinned:
for s in frontier-loop goal-loop story-loop greenfield-loop; do
  ln -s "$PWD/loopgen/$s" ~/.claude/skills/$s
done
```

Then hand `/loopgen` a task and let it classify, then derive the runnable prompt — one surface, the archetype inferred from the task:

```
/loopgen close the 7 findings in REVIEW.md                            # → goal
/loopgen push reviewer/eval.py past 0.75 on the held-out set          # → frontier
/loopgen verify onboarding across signed-out, signed-in, post-trial   # → story
/loopgen something that reads my Linear inbox and surfaces blockers    # → greenfield
```

The old `/frontier-loop`, `/goal-loop`, `/story-loop`, `/greenfield-loop` still work — each is now a thin shim that calls `/loopgen` with that archetype as the starting default. loopgen still classifies, so a shim invocation can still diverge into a hybrid if the task demands it.

## How it composes

1. **Frontload audit.** Resolve every uncertainty the loop will need (motive, commands, paths, evaluator, scope) before composing. Unresolved items emit as a frontload preamble so you can close them before the loop fires.
2. **Classify.** Extract the task's primitive values, score against each archetype's default bundle (weighted-Hamming over the varying axes), pick the nearest. A genuine tie is a hybrid; a contradiction (e.g. a `finite-criteria` target with an `equilibrium` halt) is a classification error → ask, never silently default.
3. **Compose.** Start from the archetype body, apply per-axis divergence patches, prepend the provenance preamble.
4. **Emit.** One self-contained prompt, ready for any runner.

The primitive vocabulary (six varying axes — target / halt / convergence / artifact / cadence / consult), the four archetype definitions, the assembler, and the four emittable bodies all live under [`loopgen/`](loopgen/SKILL.md).

## The archetypes, in full

### [`goal`](loopgen/archetypes/goal.md)

You have a task with a definition of done. Closes a finite acceptance inventory and halts when one final-verify proves the whole list. A 1-row inventory is fine: one bug with a repro, one review finding, one spec line.

Code review left you with 7 findings in `REVIEW.md` and you want them all green by morning. `/loopgen` derives a prompt that walks the inventory, fixes each, and halts on `criteria-met` once a single final pass proves the whole list. See [`loopgen/references/oracle-principles.md`](loopgen/references/oracle-principles.md) for the underlying principles (binary oracle, oracle independence, anti-theater).

### [`frontier`](loopgen/archetypes/frontier.md)

You have a benchmark to push. Picks the next intervention from the edge between what the artifact can already do and what it can't yet, runs it, scores it, and decides whether the frontier moved.

Your reviewer scores 0.71 on a held-out set and you want it past 0.75 before the team retros tomorrow. `/loopgen` halts on `frontier-exhausted` when all five homeostasis axes are in balance and no intervention is available, or `signal-starvation` when N consecutive iterations produce no new failing trace or finding.

### [`story`](loopgen/archetypes/story.md)

You have a frontend to walk through. The artifact of interest is the user-facing promise of a product, not its code or its tests.

You shipped onboarding three weeks ago and the storyboard has drifted. Signed-out, signed-in, and post-trial each promise something; you don't know which still hold. `/loopgen` derives a prompt that picks the next story, gathers evidence across prompts, docs, code, and the observed UI, reconciles the row with what's there, and either implements it or records the gap. Halts on `storyboard-converged` when the visible product matches the storyboard within the current scope.

### [`greenfield`](loopgen/archetypes/greenfield.md)

You have an idea to build out. Empty repo, no spec, no inventory, no evaluator. The failure mode is the opposite of `goal`: it's not "did I close the list", it's "did I commit to a list at all".

You have a vague intent: "something that surfaces what's blocking me in Linear." You don't know what "it" is yet. `/loopgen` derives a prompt that names the artifact, builds the smallest scenario that proves it, and earns the next addition only when the current one holds. Halts on `stone-converged` when the artifact has landed on the user's reframed target and further iteration has no positive yield. The 11 green-field invariants live in [`loopgen/references/greenfield-invariants.md`](loopgen/references/greenfield-invariants.md).

## Loops that don't block

Every autonomous loop fails the same way: it dies 10 minutes in, blocked on a decision the human could have made before it fired. loopgen is built around three rules to prevent that.

**Frontload at derivation.** Every uncertainty the loop might need (model, command, path, credential, evaluator, scope, fixture) gets resolved up front. If the host has `AskUserQuestion`, ask. If not, print prominently. Otherwise default and record an Alignment Review. Anything left over is a **derivation gap**, and the README that teaches against derivation gaps shouldn't ship with one.

**Judge at iteration.** When the loop hits a judgment-bearing call mid-run, it picks the smallest reversible action consistent with the strongest available source, logs an Alignment Review (problem, options, chosen contract, rollback trigger, review question), and continues. Human review happens after the fact.

**Escalate only the irreducible.** `escalate: <reason>` is reserved for paid APIs without budget caps, public-publish actions, secrets, and product direction with unclear rollback. Everything else is judgment.

Each emitted prompt carries a halt-cause classifier so the user can route a halt back: `derivation-gap` means "you should have asked me this", `genuine-escalate` means "I correctly punted", and `criteria-met` / `frontier-exhausted` / `stone-converged` / `storyboard-converged` mean legitimate completion.

## The promotion bar

A new archetype, primitive, overlay, or structural refactor lands on `main` only with a **dogfooding citation**: a concrete run on a real project that surfaced the failure mode the addition addresses. Frontier-model recommendations and design intuitions don't earn promotion. A real run does.

The capture mechanism is the **Skill Harvest** pattern each archetype body emits as part of its iteration protocol. When a run reveals a generalizable lesson, the loop writes a structured harvest note (target, observed gap, evidence iteration, proposed rule, why it generalizes, suggested patch wording, accidental-encouragement risk). Promotion requires at least one such citation.

This bar already retired `spark-loop` (taxonomy hadn't earned a public slot) and retracted `god-loop` (drafted, then pulled when it failed the same bar). Restoration of either, if real usage ever earns it back, is a `git revert` away.

The bar is empirical, not aesthetic. "Full coverage" is rejected as a design goal. The family grows from observed failure modes, not from theoretical gaps.

## On install

Each skill is a plain directory with a `SKILL.md`. No plugin runtime. Any agent that reads skill folders can use them: Claude Code, Codex, anything compatible. The unified skill is [`loopgen/`](loopgen/SKILL.md); the four named loops are thin shims that redirect to it. The symlink loop in Quickstart is the install path. There is no second one.
