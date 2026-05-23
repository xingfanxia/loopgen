# Frontier-model review — 2026-05-22

Five ChatGPT Pro (extended) reviews of the loop skills: one adversarial design
review per skill, plus one cohesive cross-family review. This file triages the
findings — what was applied directly, and what needs a design decision.

## Cross-cutting patterns

1. **Advisory → enforceable.** Every skill names the right concept but leaves
   it to the loop's judgment. A long-lived autonomous loop exploits that. The
   recurring fix: hard gates with required fields / machine-checkable closure.
2. **Stale-target re-grounding.** Loops self-certify with self-authored
   artifacts (ledger prose, JSON they wrote). Strong signal must be external
   or machine-derived; self-authored memory cannot reset a starvation detector
   or stand in for product intent.
3. **Boundary integrity.** Each loop can quietly become the wrong loop type
   (Spark drifting into frontier search; frontier closure-mode competing with
   Spark). Loops need explicit "wrong loop — reroute" tripwires.
4. **Two real contradictions** (now fixed) — see Applied.

## Applied (this commit)

Surgical, unambiguous fixes — bugs, public hygiene, and the clearest loophole
patches. No skill changed identity or structure.

| Skill | Change |
|---|---|
| frontier-loop | **Bug:** ramp trigger said stages 3–6, but ramp-exit needs only 1–5 — a loop could be told "still ramping" and "must exit" at once. Trigger is now stages 1–5; stage 6 absence is post-ramp evaluator debt. Added a mandatory `ramp-complete` exit. |
| frontier-loop | Quiet-signal counter now resets only on *strong* signal (external finding, typed trace, oracle verdict, metric move) — not on self-authored ledger prose. Closes the "narrate new work to stay alive" loophole. |
| greenfield-loop | **Bug:** emit step 5 told the loop to render iteration-0 output before the rubric existed, contradicting invariant 1 (evaluator scaffolding precedes artifact). Step 5 is now evaluator-first. |
| greenfield-loop | Doc drift fixed: "10 invariants" → 11; "Three files" → four (README was already emitted). |
| greenfield-loop | **Public hygiene:** removed a private absolute path from Provenance. |
| spark-loop | Closure-fit classification tightened: requires a *discriminative* proof channel and an *admissible concrete anchor* (not a generic seam class); bootstrap budget is one move for the whole run. |
| story-loop | **Public hygiene:** "xBanker-specific" → "project-specific". |
| story-loop | New Forbidden Move: the storyboard is not product intent — re-check an authority source before verifying/promoting an existing story. |

## Second pass — story-loop restructure + family alignment (2026-05-22)

A targeted follow-up applied two structural calls from the deferred list,
on top of the first-pass fixes above.

### story-loop is now a prompt-generator

Previously an operational workflow (the assistant ran steps 0–7 directly).
Now mirrors frontier/spark:

- `story-loop/SKILL.md` is a derivation procedure (inspect repo → pick
  lane/surface → choose storyboard format → fill template → emit).
- `story-loop/references/story-prompt-template.md` (new) holds the
  per-iteration playbook — every section from the old Workflow §0–7,
  Status Model, Core Objects, Forbidden Moves, and Minimal Output is
  preserved verbatim.
- Emits `loop/PROMPT.md` + `docs/storyboard.{md,yaml}` + `loop/STATE.md`.

### All four skills are runner-agnostic + judgment-default

The owner's framing: `/loop`, `/goal`, and external harnesses (gnhf,
cocc, ralph) are the same thing minus a small invocation difference. The
same prompt now runs under any of them. Two shared blocks (duplicated per
skill to survive the per-skill symlink install model):

- **Runner contract** — names the runner abstraction explicitly, declares
  two logical signals (`stop-and-summarize` for halt, `escalate: <reason>`
  for rare irreversible blockers), and disowns external ceilings as a
  runner concern. Inserted into both `*-prompt-template.md` files and the
  emitted story-loop prompt.
- **Judgment default** — adopts story-loop's pattern family-wide: when
  the iteration hits a taste-based or inferred call, prefer the narrow
  reversible choice + log an Alignment Review (problem · options ·
  chosen contract · alignment cost · rollback trigger · review question)
  and continue. Escalate only for the genuinely irreversible.

greenfield-loop's invariant 7 (AWAIT mode) was the most polling-shaped
piece — rewritten as "Judgment default + bounded escalate", preserving
the narrow trigger list (paid APIs without budget caps, public-publish,
secrets) and the 2-consecutive-same-question saturation rule.

frontier-loop and spark-loop's framing was lightly decoupled from
specific runners; their templates were already runner-neutral.

## Third pass — 19 hand-tunes (2026-05-22)

The deferred per-skill enhancements were triaged and applied (cash-out
softened to parameterized `{{CASH_OUT_N}}`, default 3; story-loop's
nested-YAML forms slimmed into terse single-level contracts matching the
family's voice).

**frontier-loop** — stage closure cards · repo-specific frontier vector ·
anchor lifecycle (stale-anchor depletion) · cash-out discipline (softened).

**greenfield-loop** — score-locked ramp state machine · role-protected
phase gates (loop / user / external ownership) · `INTENT.md` competing
target hypotheses · blind audience-comprehension tests · rubric versioning
+ score quarantine · blind adversarial consult protocol · consult
availability detection at derivation time (front-loads the limitation when
no consult channel is present).

**spark-loop** — in-prompt frontier tripwires · wrong-loop checkpoint ·
FIXED ≠ CLOSED closure discipline · constrained
`claim-falsification` / `probe-and-preserve` seam families · intro
tightened with plain language (no "seam closure" jargon at the top).

**story-loop** — explicit Bootstrap Mode · Ready Gate (intent-ready +
proof-ready split) · non-QA "no ready story" fallback · frontier novelty
rule · current-surface promise scan.

## Fourth pass — frontloading discipline as the family-level principle (2026-05-22)

Observation in conversation: the biggest concrete failure mode of these
loops is *self-terminating at 3 AM blocked on a decision the user could
have made before sleeping*. The patches above address it piecewise. This
pass names the unifying principle.

**The trinity:**

1. **Derivation = decide as much as possible.** Frontload every
   uncertainty.
2. **Iteration = narrow reversible judgment + Alignment Review.**
3. **Escalate = rare, irreversible only.**

If derivation does its job, iteration almost never blocks; the loop runs
through the night.

**What was added** to each of the four skills:

- **`## Frontloading discipline`** — a shared block (duplicated per skill
  to survive the symlink install model) naming the principle and the
  resolve / default / escalate-mark choices. The discipline explicitly
  privileges **AskUserQuestion** when the host provides it; otherwise
  printing the uncertainty prominently in the derivation response is the
  expected behaviour.
- **Pre-flight: Frontload audit** — a skill-specific checklist that runs
  *before* §1 of the derivation procedure. For each item the skill
  resolves / defaults / escalate-marks, then records the result in
  `loop/STATE.md` under `frontload:` and prepends a frontload preamble
  to the emitted `loop/PROMPT.md`. The user sees, before sleeping,
  exactly what was resolved, what was defaulted, and what's an escalate
  candidate.
- **Halt-cause classifier** in the emitted prompt: every `stop-and-summarize`
  or `escalate:` is labelled (`derivation-gap` · `genuine-escalate` ·
  legitimate-completion · `signal-starvation` · `wrong-loop`).
  `derivation-gap` is the feedback signal — it tells the user the
  Frontload audit missed something; the next derivation closes it.

The README now opens with the same principle so anyone cloning the repo
reads it before the skill files.

## Fifth pass — goal-loop landed as /oracle-design distillation (2026-05-23)

`goal-loop` exists. It's the family-voice distillation of `/oracle-design`,
the heavyweight ancestor (47KB SKILL.md + case studies + references). Kept:

- the **four oracle principles** (binary · independence · consumer-side ·
  anti-theater / FIXED ≠ CLOSED),
- a **slim subset of the 18 invariants** (the ones about *verifier
  honesty*, not multi-agent harness mechanics),
- the **PASS_PENDING_FINAL → PASS** state distinction (one verifier
  passing isn't terminal; the final-verify must prove the whole inventory
  in one repo state),
- **Oracle Change Notes** inline in `loop/STATE.md` (no separate
  oracle-change-cards file),
- the anti-patterns table (trimmed).

Dropped (syntax-heavy bureaucracy `/loop` and `/goal` make unnecessary):

- multi-agent `PROMPT_build.md` + `PROMPT_acceptance.md` split (default;
  available as `loop/PROMPT_verify.md` for high-stakes runs),
- `loop.sh` runnable harness (runner contract replaces it),
- Phase + TICKET-ID decomposition (flat acceptance inventory),
- `[lane:acceptance]` / `[expect:fail]` tags (prose rules),
- Motivation Trace / Risk-to-Oracle Compilation as separate derivation
  steps (folded into the verifier-design step),
- Representation Boundary Analysis (too specialized).

**Files**: `goal-loop/SKILL.md`,
`goal-loop/references/goal-prompt-template.md`,
`goal-loop/references/oracle-principles.md`. Total ~700 lines vs
`/oracle-design`'s 47KB.

**`/architect` compose**: goal-loop's derivation §1 names `/architect`
blueprints as a first-class criteria source. A U-ID becomes an AC-ID, a
test scenario becomes `pass_evidence` + `fail_evidence`, the decisive
choice becomes `authority`. The only new work goal-loop does on top of
`/architect` output is wire the `verifier` commands and define the
`final-verify`.

## Still deferred

Family-structural calls still need human judgment:

- **Shared `references/loop-family-router.md` + `shared-loop-contract.md`.**
  A truly shared file dangles under per-skill symlink install; this pass
  resolved to per-skill duplication of the short Runner-contract /
  Judgment-default blocks. A router would need either heavy per-skill
  duplication or an installer that brings the family together.
- **Rename `spark-loop`.** The word "spark" doesn't telegraph
  "closing-shaped"; the intro was tightened in plain language but the
  name itself remains. Renaming is a public-API change; deferred as its
  own decision.

The full review text for each skill is preserved in the ChatGPT Pro
threads.
