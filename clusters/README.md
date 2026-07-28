# clusters/

Bootstrap configuration, one subdirectory per cluster.

Planned sequence (Domain 6):

1. **`kubeadm-scratch/`** — built by hand so nothing is a black box. Then deliberately thrown
   away. The point is not to run it; the point is that no component stays mysterious.
2. **`homelab/`** — the real cluster, on Talos or k3s. This is what the homelab actually runs on.

Do not let a managed distro hide the control plane from you. Domain 6 is the highest-signal
domain in staff interviews and the one homelabs skip entirely.

**Secrets do not live here.** `talosconfig`, `kubeconfig`, and generated cluster secrets are
gitignored. Commit the declarative inputs, not the credentials they produce.
