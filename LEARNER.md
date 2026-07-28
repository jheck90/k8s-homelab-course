# LEARNER.md

Who is running this program, and what they already know. `CLAUDE.md` reads this to calibrate —
without it, the teaching contract has no idea which analogies land and which concepts are
genuinely new.

**Forking this repo? Replace this file entirely.** Everything below describes one specific
person. It's kept filled in rather than blanked because a worked example shows what a useful
answer looks like — vague self-description here produces vague help everywhere else.

---

## Current learner

Senior infrastructure engineer, individual contributor. Background in DevSecOps and automation,
with a security and compliance-heavy day job. Working toward platform/SRE depth.

**Prior orchestrators — the transfer channel:** Nomad and ECS. Extensive production experience
with both. Homelab currently runs entirely on Nomad. Also comfortable with NixOS, Terraform/IaC,
self-hosted GitLab.

**The gap this repo closes:** Kubernetes. Strong orchestration fundamentals, thin K8s specifics.

This distinction matters enormously. Not a beginner learning orchestration — an expert in two
other orchestrators learning a third. Treat accordingly.

**Constraint:** ~1 hour/day. The homelab serves a household; Plex uptime is a real constraint,
which is why it migrates last.

**Strongest domain going in:** 5 (security/multi-tenancy) — professional wheelhouse, fastest
path to L3.

**Weakest:** 1 (API/object model) and 3 (networking) — genuinely new, no transfer.

---

## If you're filling this in for yourself

Answer these four. They're what the teaching contract actually consumes:

1. **What have you run in production?** Name the orchestrators, config management, and clouds.
   Every analogy in this repo is anchored to something you already operate — with no answer
   here, you get generic tutorial prose.
2. **What's genuinely new vs. transfer?** Be honest about which. Learning something you half-know
   as if it's new is slow; learning something new as if it's familiar produces outages.
3. **What's your time budget, and what can't break?** The migration order falls out of this.
   Something in your lab has users who will complain. That one moves last.
4. **Which rubric domain is your professional day job?** That's your fastest route to an L3
   and your best early evidence artifact.
