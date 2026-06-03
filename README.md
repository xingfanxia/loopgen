# loopgen

<p align="center"><strong>Prompt Compiler for creating long-running autonomous loops.</strong></p>

Your loop died 10 minutes after you went to sleep.

Not because the task was impossible. It blocked on a decision you could have made before it ever fired, or it declared victory on the first green-looking signal.

loopgen writes the part of the prompt that keeps that from happening.

Give it the thing you're trying to do: close a spec, improve the codebase, push a benchmark, walk a frontend, build a vague idea. It classifies the loop, writes the prompt + state + queue files, resolves the decisions that would stall the run, then hands you one `/goal` line. Paste it into Claude Code or Codex and let it run.

The visible output is intentionally boring:

```text
/goal read loop/PROMPT.md and execute as <loop identity>.
```

![loopgen compiled loop contract](assets/loopgen-hero.jpg)

## Quick Start

Ask for the loop you actually want. Same compiler, different track:

| Track | Ask | What loopgen composes |
|---|---|---|
| Product walkthrough | `/loopgen "walk the onboarding flow"` | A story-shaped loop: keep the product surface contract, reconcile the visible flow with the storyboard. |
| Backend benchmark | `/loopgen "optimize the checkout API load benchmark"` | A frontier-shaped benchmark loop: keep pressure accounting, candidate lineage, traces, and metrics. |

Both emit the same kind of fixed kickoff:

```text
/goal read loop/PROMPT.md and execute as onboarding flow loop.
/goal read loop/PROMPT.md and execute as checkout API load benchmark loop.
```

The stable file shape is the point. Product walkthroughs keep the story surface:

```text
loop/PROMPT.md
loop/STATE.md
docs/storyboard.md
```

The backend benchmark track emits the frontier ledger and benchmark overlay:

```text
loop/PROMPT.md
loop/STATE.md
loop/FINDINGS.md
loop/TRACES.md
loop/METRICS.md
loop/DOMAIN_SPEC.md
loop/BENCHMARK.md
loop/FRONTIER.json
loop/CANDIDATES.jsonl
loop/traces/
```

## Install

It's a skill. Send your agent the repo, or clone it and symlink `loopgen/` into the skill directory (`~/.claude/skills/` for Claude Code, `~/.codex/skills/` for Codex).

## Why This Skill?

Most autonomous prompt workflows behave like one-shot overnight instructions:
the runner gets a large prompt, discovers missing decisions mid-run, then
stalls or invents a stop condition.

loopgen creates a middle layer:

- **Prompt contract** — `loop/PROMPT.md` carries the full re-entrant loop playbook
- **Durable state** — `loop/STATE.md` records classification, frontload, artifacts, and halt scan
- **Queue artifacts** — acceptance inventories, storyboards, ledgers, rubrics, traces, and metrics give the loop somewhere concrete to work
- **Fixed runner pointer** — `/goal read loop/PROMPT.md...` stays the only operator-facing kickoff

The result is a loop that can survive handoff, resume from state, and explain
which contract shaped it.

## Capabilities

| Capability | Description |
|---|---|
| **Archetype classification** | Maps the task to `goal`, `frontier`, `story`, or `greenfield` by primitive values, not vibes. |
| **Hybrid composition** | Keeps the nearest archetype contract and adds active divergences or overlays. |
| **Frontload audit** | Resolves commands, paths, evaluator, scope, and irreversible decisions before the loop fires. |
| **Deterministic artifacts** | Emits the same canonical files for the same loop shape every run. |
| **Benchmark frontier overlay** | Adds domain spec, benchmark, candidate lineage, frontier state, and trace roles when a concrete eval is bound. |
| **Provenance preamble** | Names the primitive, archetype, body, reference, and overlay files that shaped the prompt. |
| **Runner-stable kickoff** | Always emits one `/goal read loop/PROMPT.md...` pointer with no first-iteration instructions baked in. |

## Common Asks

| Ask | Shape |
|---|---|
| `/loopgen "close this spec"` | `goal`: finite acceptance inventory + final verify |
| `/loopgen "walk the onboarding flow"` | `story`: storyboard + surface evidence |
| `/loopgen "optimize the checkout API load benchmark"` | `frontier`: findings ledger + traces + metrics + benchmark overlay |
| `/loopgen "build out this artifact idea from zero"` | `greenfield`: rubric + intent + README |
| `/loopgen "improve frontend snappiness"` | `story` with frontier-expanding evidence add-ons |

## Contract Layout

```text
loop/
├── PROMPT.md             # complete re-entrant loop prompt
├── STATE.md              # durable resume state
├── ACCEPTANCE.md         # goal loops
├── VERIFY.md             # goal loops
├── FINDINGS.md           # frontier loops
├── TRACES.md             # frontier or frontier-expanding loops
├── METRICS.md            # frontier or frontier-expanding loops
├── RUBRIC.md             # greenfield loops
├── INTENT.md             # greenfield loops
├── README.md             # greenfield loops
├── DOMAIN_SPEC.md        # benchmark-frontier overlay
├── BENCHMARK.md          # benchmark-frontier overlay
├── CANDIDATES.jsonl      # benchmark-frontier overlay
├── FRONTIER.json         # benchmark-frontier overlay
└── traces/               # benchmark-frontier overlay

docs/
└── storyboard.md         # story loops
```

### Ownership Model

| Path | Owner | Rule |
|---|---|---|
| `loop/PROMPT.md` | loopgen | Generated loop contract; edit by re-running `/loopgen`. |
| `loop/STATE.md` | loopgen + runner | Seeded by loopgen, updated by the running loop. |
| `loop/ACCEPTANCE.md`, `loop/VERIFY.md` | goal loop | Acceptance source and final-verify transcript. |
| `docs/storyboard.md` | story loop | Product promise index; evidence must not rewrite promises to match behavior. |
| `loop/FINDINGS.md`, `loop/TRACES.md`, `loop/METRICS.md` | frontier loop | Pressure, trace, and metric ledgers. |
| `loop/RUBRIC.md`, `loop/INTENT.md`, `loop/README.md` | greenfield loop | Target, scoring, and artifact orientation. |
| `loop/DOMAIN_SPEC.md`, `loop/BENCHMARK.md`, `loop/CANDIDATES.jsonl`, `loop/FRONTIER.json`, `loop/traces/` | benchmark-frontier overlay | Benchmark roles and candidate lineage. |
| Repo-native paths | project | May be recorded as aliases; they do not replace canonical anchors. |

## Skill Behavior

The bundled `loopgen` skill teaches the model to:

- Never compose from memory; read the required primitives, archetype, body, and overlay references first
- Never silently default on contradictory primitive values
- Always emit canonical artifact files for the active contracts
- Always record `derivation_read_set`, frontload, divergences, overlays, and artifacts in `loop/STATE.md`
- Always make hybrids additive: nearest archetype first, then divergent primitive and overlay contracts
- Always emit the bare `/goal read loop/PROMPT.md and execute as <identity>.` kickoff
- Never put first-iteration setup instructions in the kickoff; bootstrap belongs inside the re-entrant prompt

## Documentation

| Document | What it covers |
|---|---|
| [`loopgen/SKILL.md`](loopgen/SKILL.md) | Compiler contract, phases, artifact/state contracts, and runner kickoff rules. |
| [`loopgen/primitives/`](loopgen/primitives) | Primitive vocabulary: target, halt, artifact, convergence, cadence, frontload, runner, evidence, evaluator, pressure, benchmark overlays. |
| [`loopgen/archetypes/`](loopgen/archetypes) | Defaults and failure modes for `goal`, `frontier`, `story`, and `greenfield`. |
| [`loopgen/templates/composed-prompt.md`](loopgen/templates/composed-prompt.md) | Assembly procedure for emitted prompts. |
| [`loopgen/templates/bodies/`](loopgen/templates/bodies) | Archetype body templates that become `loop/PROMPT.md`. |
| [`loopgen/references/`](loopgen/references) | Oracle, benchmark-frontier, greenfield, review closure, and compatibility references. |

## How it actually works

loopgen is four battle-tested loop-generator skills folded into one compiler. It picks the shape from your intent, fills the blanks, creates canonical state, prompt, and queue files, then hands you the fixed `/goal` kickoff prompt.

### The four archetypes

loopgen classifies for you. When the intent sits between archetypes, it composes the active contracts instead of forcing the task into one box.

| Seed | Archetype | Halts on |
|---|---|---|
| A task with a definition of done | `goal` | `criteria-met`: one final-verify proves the frozen acceptance inventory |
| A frontier to push (autoresearch) | `frontier` | `homeostatic-checkpoint`: five homeostasis axes balanced, no intervention left |
| A product surface to walk through | `story` | `storyboard-converged`: the visible product matches the storyboard |
| An idea to build out from zero | `greenfield` | `stone-converged`: the artifact landed on the user's reframed target |

### How it composes

1. **Frontload audit.** Resolve every uncertainty the loop will need (motive, commands, paths, evaluator, scope) before composing. Unresolved items emit as a frontload preamble so you can close them before the loop fires.
2. **Classify.** Extract the intention's primitive values, score against each archetype's default bundle (weighted-Hamming over the varying axes), pick the nearest. A genuine tie is a hybrid; a contradiction (e.g. a `finite-criteria` target with an `equilibrium` halt) is a classification error → ask, never silently default.
3. **Compose.** Start from the archetype body, apply per-axis divergence patches, and fill the provenance + frontload slots. Overlays such as consult capability and benchmark-frontier are detected during frontload and do not add archetypes or change the weighted classifier.
4. **Emit.** Canonical artifacts plus one self-contained prompt and a fixed `/goal` kick-off.

Canonical artifacts are stable:

| Shape | Files |
|---|---|
| Common | `loop/PROMPT.md`, `loop/STATE.md` |
| `goal` | `loop/ACCEPTANCE.md`, `loop/VERIFY.md` |
| `frontier` | `loop/FINDINGS.md`, `loop/TRACES.md`, `loop/METRICS.md` |
| `story` | `docs/storyboard.md` |
| `greenfield` | `loop/RUBRIC.md`, `loop/INTENT.md`, `loop/README.md` |

Hybrids keep the nearest archetype's files, then add the active divergent or overlay contracts. For example, a story-shaped frontend performance loop keeps `docs/storyboard.md` and adds metric/trace indexes because the frontier-expanding target needs measurable before/after evidence.

The primitive vocabulary (the five axes the classifier scores on: target / halt / artifact / convergence / cadence; plus frontload overlays such as consult capability and benchmark-frontier, which affect composition rather than steering classification), the four archetype definitions, the assembler, and the four emittable bodies all live under [`loopgen/`](loopgen/SKILL.md).

### The archetypes, in full

#### [`goal`](loopgen/archetypes/goal.md)

You have a task with a definition of done. Closes a finite acceptance inventory and halts when one final-verify proves the whole list. A 1-row inventory is fine: one bug with a repro, one review finding, one spec line.

You have a spec with a dozen acceptance lines and you want them all green by morning. `/loopgen` derives a prompt that walks the inventory, fixes each, and halts on `criteria-met` once a single final pass proves the whole list. See [`loopgen/references/oracle-principles.md`](loopgen/references/oracle-principles.md) for the underlying principles (binary oracle, oracle independence, anti-theater).

#### [`frontier`](loopgen/archetypes/frontier.md)

You have a quality frontier to push — better, faster, more robust, higher-scoring — with no finish line, Karpathy's autoresearch style. A held-out benchmark score is one instance; so are latency, test coverage, type-safety, suite health, robustness, or "improve the codebase" once you've named the dimension. Each iteration senses the repo across five homeostasis axes, picks the intervention at the edge between what the artifact can already do and what it can't yet, runs it, scores it, and decides whether the frontier moved — alternating between improving the product and improving the mechanism that judges it. (A *finite* version of the same target — "get coverage to exactly 80% and stop" — has a pass line and is `goal`, not `frontier`; the frontier is the one with no fixed finish. Note: no finish line ≠ no stopping rule — a frontier loop still halts, at equilibrium / plateau / budget, just not at a target number.)

Your benchmark scores 0.71 and you want it to grind and auto-improve — or the test suite is slow and flaky and you want it faster and greener by morning, no fixed target, just better. `/loopgen` halts on `homeostatic-checkpoint` when all five homeostasis axes are in balance and no intervention is available, or `signal-starvation` when N consecutive iterations produce no new failing trace or finding.

Pure frontier records pressure status, pressure debt, checkpoint reason, and next pressure in `loop/FINDINGS.md`, `loop/TRACES.md`, `loop/METRICS.md`, and `loop/STATE.md`. When frontload binds a concrete benchmark/eval/harness object, loopgen adds the `benchmark-frontier` overlay: semantic roles for domain spec, benchmark, candidates, frontier, and traces; candidate lineage; evaluator health; and search/holdout/adversarial pressure before promotion. It is still `frontier`, not a fifth archetype.

#### [`story`](loopgen/archetypes/story.md)

You have a product's user-facing promises to walk through. A web frontend is the best-tooled instance, but a CLI, a TUI, or an API's published contract make promises too — the artifact of interest is the user-facing promise, not its code or its tests, and the evidence tracks the surface: screenshots for a frontend, a command transcript + exit code for a CLI, contract tests + schema for an API.

You shipped onboarding three weeks ago and the storyboard has drifted. Signed-out, signed-in, and post-trial each promise something; you don't know which still hold. `/loopgen` derives a prompt that picks the next story, gathers evidence across prompts, docs, code, and the observed UI, reconciles the row with what's there, and either implements it or records the gap. Halts on `storyboard-converged` when the visible product matches the storyboard within the current scope.

#### [`greenfield`](loopgen/archetypes/greenfield.md)

You have an idea to build out. No spec, no inventory, no evaluator — often an empty repo, but equally a brand-new subsystem inside an existing one. The failure mode is the inverse of `goal`'s: `goal` asks whether the list got closed; greenfield asks whether you ever committed to one.

The seed: "an artifact manager for my spec-driven workflow." You don't know what "it" is yet. `/loopgen` derives a prompt that names the artifact, builds the smallest scenario that proves it, and earns the next addition only when the current one holds. Halts on `stone-converged` when the artifact has landed on the user's reframed target and further iteration has no positive yield. The 11 green-field invariants live in [`loopgen/references/greenfield-invariants.md`](loopgen/references/greenfield-invariants.md).

---

It's a prompt-writer skill with strong opinions about going to sleep. That's all. YMMV.
