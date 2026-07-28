# clusters/

Bootstrap configuration, one subdirectory per cluster.

Planned sequence (Domain 6):

1. **`kubeadm-scratch/`** — built by hand so nothing is a black box. Then deliberately thrown
   away. The point is not to run it; the point is that no component stays mysterious.
2. **`homelab/`** — the real cluster, on Talos or k3s. This is what the homelab actually runs on.

Bring the hand-built control plane up **one component at a time**, and use each gap as an
object-model lesson — a Deployment that sits inert with no controller-manager teaches
reconciliation better than any amount of reading. The staged table is in
[`../rubric/k8s-sre-rubric.md`](../rubric/k8s-sre-rubric.md) under Phase 1.

A cluster with pieces deliberately missing is a teaching instrument you can only build once.
k3s and Talos are engineered to hide exactly these components, so this is the only window.

Do not let a managed distro hide the control plane from you. Domain 6 is the highest-signal
domain in staff interviews and the one homelabs skip entirely.

**Secrets do not live here.** `talosconfig`, `kubeconfig`, and generated cluster secrets are
gitignored. Commit the declarative inputs, not the credentials they produce.
