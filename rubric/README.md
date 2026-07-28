# rubric/

`k8s-sre-rubric.md` is the standard. `SCORECARD-TEMPLATE.md` is the shape of an assessment
against it. Both are shareable — clone this repo and they apply to you unchanged.

Completed scorecards go in [`my-scores/`](my-scores/), which is gitignored. Naming:
`YYYY-MM-DD-scorecard.md`.

**The first one is day one, before any work.** Mostly L0, and it will feel like a formality —
but every later score is a delta against it, and a program with no zero point can't show
movement. It's also the only scorecard with nothing to defend, which makes it the most honest
one you'll write.

Cadence: every 4 weeks, dated, never skipped. Regression on domains you stopped touching is
expected and is information.

Since these aren't committed, git history no longer enforces the dating for you — a scorecard
can be quietly backdated or rewritten. Write them on the day, in one sitting, and don't revise
an old one after the fact.

## The evidence rule

Scores are claims, and claims need backing:

- **L1** — self-assessable.
- **L2** — requires a failure you caused *deliberately* and then fixed. Link the postmortem.
- **L3** — requires a produced artifact **and** an unaided verbal defense of the tradeoff.
  Not self-assessable. Link the artifact.

Self-assessment inflation is the main way this program fails quietly. A score without a link
is an aspiration, not a score.
