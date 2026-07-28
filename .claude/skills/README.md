# .claude/skills/

Project-scoped skills, committed. Personal ones go in `~/.claude/skills/`.

## The governing rule

Most off-the-shelf Kubernetes skills are actively wrong for this repo during Phases 1–2. They
exist to stop LLMs hallucinating manifests, so they generate production-ready YAML with correct
security contexts, limits, and probes — precisely the behavior this repo is designed to prevent.
A correct manifest you didn't reason through is worth nothing in six months.

Teaching skills now. Kubernetes skills after the migration, when the mode shifts from learning
to operating.

## To author here

1. **`nomad-to-k8s`** — the translation layer, built incrementally as `migrations/` fills up.
   Each entry: the Nomad or ECS construct, the K8s equivalent, and where the analogy breaks.
   The single most useful artifact in the program, and no off-the-shelf skill can contain it
   because it's specific to mental models you already have.
2. **`hint-ladder`** — the escalation rules from `CLAUDE.md`, as an enforceable skill rather
   than prose an agent may drift from. Survives context compaction; prose doesn't.
3. **`rubric-score`** — loads `rubric/k8s-sre-rubric.md`, interrogates claimed levels against
   the evidence rule, writes a dated scorecard.

Scaffold with the official `skill-creator` skill.

## Before installing anything from a community marketplace

A skill is arbitrary instructions an agent will follow — prompt injection, credential access,
and dangerous-command surface. Read the SKILL.md first, same as any dependency. Advertised
security scanning is a signal, not a guarantee.
