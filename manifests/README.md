# manifests/

GitOps source of truth. Argo CD or Flux reconciles the cluster from here.

Established early (Phase 2, Domain 8) on purpose: once this is in place, every subsequent
service migration is cheap. Standing it up late means paying the migration cost twice.

## The Domain 8 evidence artifact

The whole homelab reconciled from this directory. Prove it the only way that counts: destroy
the cluster completely and rebuild from Git alone, with only secrets restored out of band.

Time it. That number is the platform's real RTO — and it's a far better interview answer than
"we use GitOps."

## Secrets

Encrypted at rest in Git (SOPS or sealed secrets) or pulled by an external secrets operator.
Plaintext secrets are gitignored, but do not rely on that as the control.
