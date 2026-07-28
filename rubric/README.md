# rubric/

`k8s-sre-rubric.md` is the standard. Everything else here is a dated self-assessment against it.

Naming: `YYYY-MM-DD-scorecard.md`, from `SCORECARD-TEMPLATE.md`.

Cadence: every 4 weeks, committed. Regression on domains you stopped touching is expected
and is information.

## The evidence rule

Scores are claims, and claims need backing:

- **L1** — self-assessable.
- **L2** — requires a failure you caused *deliberately* and then fixed. Link the postmortem.
- **L3** — requires a produced artifact **and** an unaided verbal defense of the tradeoff.
  Not self-assessable. Link the artifact.

Self-assessment inflation is the main way this program fails quietly. A score without a link
is an aspiration, not a score.
