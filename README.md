# k8s-homelab-course

A 24-week Kubernetes program at ~1 hour/day, structured as a progressive migration of a
homelab from Nomad to Kubernetes. K8s stands up *alongside* Nomad; services move one at a
time; Plex moves last.

Competency target and scoring rules: [`rubric/k8s-sre-rubric.md`](rubric/k8s-sre-rubric.md).
Working agreement for AI assistance in this repo: [`CLAUDE.md`](CLAUDE.md).

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

Re-score every 4 weeks, dated, committed. Regression on untouched domains is expected and is
information, not failure.

L3 is not self-assessable: it needs a produced artifact **and** an unaided verbal defense of
the tradeoff. L2 needs a failure you caused deliberately and then fixed.
