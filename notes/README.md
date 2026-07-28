# notes/

Concept notes, one subdirectory per rubric domain.

```
01-api-object-model/
02-scheduling/
03-networking/
04-storage/
05-security/
06-cluster-lifecycle/
07-observability/
08-delivery-gitops/
```

Notes worth keeping have one of these shapes:

- **A model correction.** You believed X, X was wrong, here's what's actually true and why the
  wrong version was plausible. These are the highest-value notes in the directory.
- **A Nomad/ECS analogy and its break point.** If it also describes a migrated service, it
  belongs in `migrations/` instead.
- **A command that revealed something** you couldn't see another way.

The first notes come from Week 1, in `01-api-object-model/` — see the stage table in
[`../clusters/README.md`](../clusters/README.md). Stages 2 and 5 are model corrections, and
model corrections are the notes worth having.

Notes that just restate the upstream docs are not worth writing — link the docs instead. The
reason to write is that you had to *derive* something.
