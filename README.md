# loopgen

Loopgen is an opt-in Codex/Claude skill for authoring a lightweight autonomous
loop contract. It is useful when a task genuinely needs to survive a long run,
resume, or handoff. It is not a default workflow for ordinary development.

The lite version keeps four things:

- one stable project objective;
- a durable acceptance or discovery ledger;
- validation tied to changed behavior;
- explicit stop and approval boundaries.

It removes mandatory archetype compilation, repeated review rounds, provenance
ceremony, and large fixed artifact sets.

## Use

Start Codex with the opt-in profile, then invoke it explicitly:

```text
codex -p loopgen
```

Inside that session:

```text
/loopgen create a finite delivery loop for the refactor in docs/ACCEPTANCE.md
/loopgen diagnose why loop/PROMPT.md keeps reopening completed work
/loopgen create a bounded frontier loop for the named benchmark
```

The normal output is one compact prompt:

```text
loop/PROMPT.md
```

Loopgen reuses existing acceptance, status, audit, and verification artifacts.
It creates `loop/STATE.md` only when the repository lacks a suitable durable
ledger.

Run the emitted contract separately:

```text
/goal read loop/PROMPT.md
```

## Modes

| Mode | Use | Stop |
|---|---|---|
| Finite delivery (default) | Refactor, migration, audit closure, release milestone, bounded product slice | Acceptance proven or a real blocker/approval boundary is reached |
| Frontier discovery (explicit only) | Benchmark improvement, repeated discovery, exploration against fresh signals | A named bounded checkpoint; candidates go to a separate discovery queue |

A frontier may propose finite delivery work, but it does not share the finite
runner's state or completion rule.

## Process budget

- Focused validation during implementation.
- One self-review of the relevant diff.
- One independent review only for a named risk that merits it.
- A full repository gate once at the final relevant diff when the work is broad,
  high-risk, or explicitly at a merge/release gate.
- No repeated checks while the diff and risk are unchanged.
- No review loop for low, nit, or informational findings.

Long thread age, token totals, and compaction count are diagnostic signals, not
failure conditions. Loss of control means repeated work, acceptance drift,
stale-context mistakes, reopened completed items, missing evidence, or failure
to converge.

## Repository layout

[`loopgen/SKILL.md`](loopgen/SKILL.md) is the active lite skill. The existing
`archetypes/`, `primitives/`, `templates/`, and `references/` directories are
retained as historical design material for the fork; the lite skill does not
load them during normal use.

## Install

Symlink `loopgen/` into the Codex or Claude skills directory. Keep it opt-in: its
frontmatter intentionally excludes broad triggers such as ordinary planning,
debugging, implementation, or vague repository improvement.
