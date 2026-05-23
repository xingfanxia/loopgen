# loopy

Prompt generators for `/goal` and `/loop`.

`loopy` is a small family of Claude Code skills that **derive repo-specific
prompts for autonomous improvement loops**. The skills do not run the loop —
they author the prompt (and the invariants) that an external harness or
`/loop` then executes.

## Design principle: loops that don't block

The biggest failure mode of an autonomous loop is *self-terminating at 3 AM
blocked on a decision the user could have made before the loop fired*. All
four skills share one principle to prevent that:

- **Frontload at derivation.** For every uncertainty the loop might need
  (model, command, path, credential, evaluator, scope, fixture, …),
  *resolve it now* (AskUserQuestion if the host has the tool, otherwise
  print prominently), *default + Alignment Review*, or *mark as escalate
  candidate*. Anything left over is a **derivation gap** — a future block.
- **Judge at iteration.** When the loop *does* hit a judgment-bearing call,
  it picks the smallest reversible action consistent with the strongest
  available source, records an Alignment Review (problem · options ·
  chosen contract · rollback trigger · review question), and continues.
  Human review happens after the fact.
- **Escalate only the irreversible.** `escalate: <reason>` is reserved for
  paid APIs without budget caps, public-publish actions, secrets, and
  product direction with unclear rollback. Everything else is judgment.

Each emitted prompt also carries a **halt-cause classifier** so the user
can route a halt back: `derivation-gap` means "you should have asked me
this", `genuine-escalate` means "I correctly punted", `frontier-exhausted`
/ `stone-converged` / `seams-sealed` / `storyboard-converged` mean
legitimate completion.

## The skills

| Skill | Derives a loop that… | Use when |
|---|---|---|
| `frontier-loop` | moves a repo's evidence-backed capability frontier | an artifact/evaluator exists and should get better |
| `greenfield-loop` | discovers what to build when nothing is defined | empty repo, vague "build me something X-adjacent" intent |
| `spark-loop` | seals one operator-facing seam per iteration | a known, closure-shaped seam with a cheap proof surface |
| `story-loop` | maintains a living product storyboard | discovering / verifying user-facing product promises |

Rough routing: **target undefined →** `greenfield-loop` · **move an existing
frontier →** `frontier-loop` · **close one known seam →** `spark-loop` ·
**product promises →** `story-loop`.

## Install

Clone, then symlink the skills into your agent's skills directory:

```sh
git clone git@github.com:pro-vi/loopy.git
for s in frontier-loop greenfield-loop spark-loop story-loop; do
  ln -s "$PWD/loopy/$s" ~/.claude/skills/$s      # Claude Code
  ln -s "$PWD/loopy/$s" ~/.codex/skills/$s       # Codex (optional)
done
```

Each skill is a plain directory with a `SKILL.md` — no plugin runtime, so it
works with any agent that reads skill folders.

## Notes

See [`REVIEW.md`](REVIEW.md) for the most recent frontier-model design review
of these skills and what changed because of it.
