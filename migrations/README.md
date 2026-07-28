# migrations/

One file per service moved off Nomad. `TEMPLATE.md` is the shape.

The rule: two sentences minimum per service — what Nomad did, what Kubernetes needs instead,
and **why the difference exists**. The third part is the one that's worth anything later.

Where the Nomad/ECS analogy *breaks* is the payload of this directory. An analogy that's 90%
right is more dangerous than none, because it gets trusted into an outage. Write down the
break every time you find one.

Naming: `<service>.md` — e.g. `gitea.md`, `paperless.md`, `plex.md`.

Migration order is deliberate: start with whatever nobody in the house will miss. Plex last.
