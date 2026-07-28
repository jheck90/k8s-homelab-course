# k8s-homelab-course

A 24-week Kubernetes program at ~1 hour/day, structured as a progressive migration of a
homelab from Nomad to Kubernetes. K8s stands up *alongside* Nomad; services move one at a
time; Plex moves last.

Competency target and scoring rules: [`rubric/k8s-sre-rubric.md`](rubric/k8s-sre-rubric.md).
Working agreement for AI assistance in this repo: [`CLAUDE.md`](CLAUDE.md).
Who's running it and what they already know: [`LEARNER.md`](LEARNER.md).

**Forking this to learn from?** Rewrite `LEARNER.md` first — the teaching contract calibrates
off it, and it currently describes someone else. Then `rm -rf rubric/my-scores/*.md notes/*/*.md
migrations/*.md postmortems/2*.md` for a blank slate, or leave them as worked examples.

## Start here

**Day 1 — baseline scorecard.** Copy `rubric/SCORECARD-TEMPLATE.md` to
`rubric/my-scores/YYYY-MM-DD-scorecard.md` and score all eight domains *before* doing any
work. It will feel pointless and most of it will be L0. That's the point: the 4-week re-score
is meaningless without a zero point, and honest scoring is easiest when there's nothing to
defend yet.

**Day 1 — the one decision that blocks everything.** Phase 1 needs hardware for a hand-built
cluster that gets deliberately thrown away, running *alongside* the existing Nomad homelab. VMs,
spare hardware, or cloud instances — but decide it now, because Week 1 can't start without
nodes. Record the choice in `clusters/MY-SETUP.md` (copy `clusters/MY-SETUP.example.md`).

**Week 1 — first build.** `clusters/kubeadm-scratch/`, one control-plane component at a time,
per the sequence in [`clusters/README.md`](clusters/README.md). Stop after etcd + the API
server. Do not add the controller-manager yet; the gap is the lesson.

**When you get stuck,** ask for a hint rather than an answer — the escalation ladder is in
[`CLAUDE.md`](CLAUDE.md). A working manifest you didn't reason through is worth nothing in six
months.

## Layout

| Path | Contents |
|---|---|
| `rubric/` | The rubric, plus dated self-assessments |
| `clusters/` | Bootstrap configs — kubeadm build first, then Talos/k3s |
| `manifests/` | GitOps source of truth (Argo or Flux) |
| `migrations/` | One file per service: Nomad spec, K8s equivalent, what differs and why |
| `postmortems/` | One per deliberate failure (Phase 3) |
| `notes/` | Concept notes, organized by rubric domain |
| `.claude/skills/` | Project-scoped skills |

`migrations/` and `postmortems/` are the highest-value directories here. Everything else is
scaffolding around them.

## Phases

- **1 — Foundations (wk 1–6):** Domain 6 and Domain 1 to L2. Build a cluster by hand, then
  rebuild on Talos/k3s. Nothing moves off Nomad yet.
- **2 — Migration (wk 7–14):** Domain 8, then 3, then 2 and 4. GitOps early so every later
  migration is cheap.
- **3 — Depth and breakage (wk 15–24):** Domain 5, then 7, then a second pass to L3. One
  deliberate failure per week, scheduled, each with a written postmortem.

## Progress rule

Re-score every 4 weeks, dated, into `rubric/my-scores/` (gitignored — see
[`rubric/README.md`](rubric/README.md)). Regression on untouched domains is expected and is
information, not failure.

L3 is not self-assessable: it needs a produced artifact **and** an unaided verbal defense of
the tradeoff. L2 needs a failure you caused deliberately and then fixed.
