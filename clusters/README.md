# clusters/

Bootstrap configuration, one subdirectory per cluster.

Planned sequence (Domain 6):

1. **`kubeadm-scratch/`** — built by hand so nothing is a black box. Then deliberately thrown
   away. The point is not to run it; the point is that no component stays mysterious.
2. **`homelab/`** — the real cluster, on Talos or k3s. This is what the homelab actually runs on.

## Prerequisite — decide before Week 1

Nodes for `kubeadm-scratch/`. This cluster gets destroyed on purpose, so it must not share
fate with whatever currently runs your workloads. VMs, spare hardware, or cloud instances all
work; three nodes is enough and one is survivable for Phase 1.

Record the decision in `MY-SETUP.md` — copy [`MY-SETUP.example.md`](MY-SETUP.example.md).
That file is gitignored, since node addresses and hardware inventory are yours, not part of
the curriculum.

## Week 1 — the build order

Bring the hand-built control plane up **one component at a time**, and use each gap as an
object-model lesson — a Deployment that sits inert with no controller-manager teaches
reconciliation better than any amount of reading. The full staged table, with the Domain 1
lesson paired to each stage, is in
[`../rubric/k8s-sre-rubric.md`](../rubric/k8s-sre-rubric.md) under Phase 1.

Stop at each stage until you can answer its question without looking it up:

| Stage | Bring up | Question to answer before moving on |
|---|---|---|
| 1 | etcd alone | What does a Kubernetes object look like on disk, and who assigns its `resourceVersion`? |
| 2 | \+ apiserver | You created a Deployment and no Pod exists. Where did the object go, and what — specifically — is *not* running that would have acted on it? |
| 3 | \+ controller-manager | Pods now exist and are Pending forever. Who wrote the `ownerReferences`, and what field is still empty? |
| 4 | \+ scheduler | Scheduling wrote exactly one field. Which one — and what does that imply about replacing the scheduler? |
| 5 | \+ kubelet | Kill a container by hand, outside Kubernetes. Nothing sent an event. Why does it come back? |

Stage 2 is the one that pays for the whole exercise. In Nomad, submitting a job *is* the
scheduling request. Here, submitting is a write to a database and nothing more. Sit in that gap
before you close it.

Write up stage 2 and stage 5 in [`../notes/01-api-object-model/`](../notes/01-api-object-model/)
— those are model corrections, which the notes README rates highest.

A cluster with pieces deliberately missing is a teaching instrument you can only build once.
k3s and Talos are engineered to hide exactly these components, so this is the only window.

Do not let a managed distro hide the control plane from you. Domain 6 is the highest-signal
domain in staff interviews and the one homelabs skip entirely.

**Secrets do not live here.** `talosconfig`, `kubeconfig`, and generated cluster secrets are
gitignored. Commit the declarative inputs, not the credentials they produce.
