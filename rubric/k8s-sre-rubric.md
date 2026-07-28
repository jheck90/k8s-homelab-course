# Kubernetes Competency Rubric — Senior/Staff SRE

## How to use this

Eight domains, three levels each. Levels are not "how much you've read" — they're what you can do:

- **L1 — Use it.** You can make the thing work on a good day.
- **L2 — Debug it.** You can explain what happens when it breaks, and fix it under time pressure without a tutorial.
- **L3 — Design it.** You can choose between options, defend the tradeoff to a skeptical staff engineer, and set policy others follow. Includes knowing when the answer is "don't."

**Scoring rule:** you don't get to claim L3 on self-assessment alone. L3 requires (a) a produced artifact, and (b) you can explain the tradeoff out loud to another engineer without notes. L2 requires you fixed the failure *after* causing it deliberately.

Re-score every 4 weeks. Expect to regress on domains you stop touching — that's information, not failure.

### Your specific starting position

Coming from Nomad and ECS, some of this is transfer, not learning:

- **Already yours:** bin-packing and scheduling intuition, service discovery concepts, the operational discipline of running orchestrated workloads, declarative job specs, rolling update semantics.
- **Genuinely new:** the reconciliation/controller model, the API server as the extensibility surface (CRDs, admission, operators), the networking model, and RBAC-as-attack-surface.

The failure mode to guard against: learning Kubernetes as "Nomad with more YAML." Spend your disproportionate time on domains 1, 3, 5, and 6.

The counter-advantage: very few K8s-native engineers can articulate *when not to use Kubernetes*. You can, credibly, because you've run production on two alternatives. Don't hide that in interviews or design reviews — it's a staff-level signal, not a gap.

---

## Domain 1 — API & Object Model

The center of gravity. Everything else is built on this.

**L1**
- Read and write core manifests without copy-paste
- Explain the Deployment → ReplicaSet → Pod ownership chain
- Fluent with `kubectl explain`, `kubectl get -o yaml`, label selectors

**L2**
- Explain reconciliation: level-triggered, not edge-triggered, and why that matters for idempotency
- `ownerReferences`, cascading deletion, and debugging an object stuck on a finalizer
- `resourceVersion`, watch semantics, optimistic concurrency, list/watch bookmarks
- Server-side apply and field-manager conflicts (two controllers fighting over one field)

**L3**
- Write a controller with controller-runtime: reconcile loop, requeue strategy, idempotency, backoff
- Design a CRD properly: status subresource, standard conditions, printer columns, validation, versioning + conversion
- API machinery failure modes — especially a mutating webhook with `failurePolicy: Fail` that gates pod creation while its own backing pods are down (the classic self-inflicted total outage)
- Articulate when a CRD is the wrong abstraction and a config file would do

**Evidence artifact:** Write a small operator that replaces a piece of homelab glue you currently do with a Nomad periodic job — e.g. a `MediaBackup` CRD that provisions and tracks Plex metadata backups. It doesn't need to be good. It needs to teach you why reconcile loops get called more than you expect.

---

## Domain 2 — Scheduling & Resource Governance

Your strongest transfer from Nomad. Move through L1/L2 fast; the delta is QoS/eviction and the autoscaler layering.

**L1**
- requests vs limits, nodeSelector, node/pod affinity and anti-affinity

**L2**
- The three QoS classes and the exact eviction ordering under node pressure
- The CPU/memory asymmetry: CPU limits throttle (CFS quota, and the throttling is bursty and invisible in averages), memory limits kill
- Why setting CPU limits often makes latency worse, and when you'd still do it
- PDBs, topology spread constraints, taints/tolerations, PriorityClasses and preemption
- Read a `FailedScheduling` event and know exactly which predicate rejected the pod

**L3**
- Cluster-wide governance: ResourceQuota and LimitRange policy, defensible overcommit ratios, who gets to set priority
- The interaction between HPA, VPA, KEDA, and Cluster Autoscaler/Karpenter — including the feedback loops where two of them fight
- Capacity modeling and cost attribution per tenant
- Bin-pack vs spread as an explicit reliability/cost decision

**Evidence artifact:** Deliberately oversubscribe a node until it evicts. Capture the eviction cascade in metrics, note which pods died and in what order, and write a one-page postmortem explaining why the scheduler made each choice.

---

## Domain 3 — Networking

Where most self-taught K8s knowledge is thinnest, and where interview questions go when they want to find the floor.

**L1**
- Service types, ClusterIP/NodePort/LoadBalancer, Ingress basics, cluster DNS naming

**L2**
- CNI plumbing: veth pairs, IPAM, how a packet actually leaves a pod
- kube-proxy iptables vs IPVS vs eBPF datapath — and the scaling characteristics of each
- EndpointSlices and how they update during a rollout (and the gap that causes 502s)
- The DNS resolution path, `ndots:5`, and the search-domain latency trap
- Headless services, Ingress controller internals
- Can `tcpdump` your way through a pod-to-pod failure without guessing

**L3**
- Choose a CNI with reasoning you'd defend in a design review
- Design a default-deny NetworkPolicy posture that doesn't break DNS or health checks
- Gateway API migration path off Ingress
- Service mesh cost/benefit — including a clear case for declining one
- MTU and encapsulation overhead, multi-cluster connectivity

**Evidence artifact:** Take the homelab to default-deny, namespace by namespace, and restore full function. Document every policy and what broke when you added it. The DNS breakage will teach you more than the rest combined.

---

## Domain 4 — Storage & Stateful Workloads

**L1**
- PV/PVC/StorageClass, mounting volumes, ephemeral vs persistent

**L2**
- CSI architecture: controller plugin vs node plugin, and the attach → mount → publish staging
- StatefulSet identity guarantees, ordered rollout, `volumeClaimTemplates`
- Reclaim policies, volume expansion, and debugging a PVC that won't detach from a dead node
- Why `ReadWriteMany` is a different problem than `ReadWriteOnce`

**L3**
- Backup and restore designed around *tested restores*, not backups (Velero + volume snapshots, with drill timings)
- A defensible position on running databases in-cluster vs out, with the operator-maturity argument
- Topology-aware provisioning and the zone-affinity trap where a pod can never be scheduled again

**Evidence artifact:** Run a real stateful workload — Postgres, or the Plex metadata DB — as a StatefulSet. Then destroy it and restore from backup, timed. Do it twice. The second time should be much faster, and that delta is your MTTR story.

---

## Domain 5 — Security & Multi-tenancy

Your professional wheelhouse. This is where you can reach L3 fastest and where the staff-level gap is widest in most candidates.

**L1**
- ServiceAccounts, Roles/ClusterRoles and their bindings, secrets as env vs mounted volume

**L2**
- RBAC as an escalation graph: who can create pods can usually become cluster-admin; same for `escalate`, `bind`, `impersonate`, and secret-read on a privileged SA
- Pod Security Admission levels (privileged/baseline/restricted) and what each actually blocks
- `securityContext` in depth: runAsNonRoot, dropped capabilities, seccomp, read-only root filesystem
- Workload identity federation instead of long-lived credentials
- Secrets at rest, envelope encryption, external secret operators

**L3**
- Design admission policy with Kyverno or Gatekeeper *including* its failure modes, exception workflow, and rollout strategy (audit → warn → enforce)
- Supply chain: image signing and verification, provenance/attestation, admission-time enforcement
- Namespace-as-tenant vs vcluster vs separate clusters — with the isolation/cost math
- Audit logging that supports detection, not just compliance
- Map controls to a framework and defend the mapping

**Evidence artifact:** Write and enforce five Kyverno policies in the homelab. At least one must catch a real misconfiguration you actually made earlier in this program. Then break the webhook on purpose and watch what happens to your cluster — that lesson is worth the outage.

---

## Domain 6 — Cluster Lifecycle & Operations

**The highest-signal domain for staff interviews and the one homelabs skip entirely.** Do not let a managed distro hide this from you.

**L1**
- Install a cluster, join and drain a node, read component health

**L2**
- etcd operations: backup, restore, defrag, compaction, and recovering from quorum loss
- Certificate rotation and expiry — including the cluster that stops working exactly one year after install
- Version skew policy between API server, kubelet, and controllers; upgrade sequencing
- Cordon/drain when PDBs block you, and how to decide whether to force it
- Read control-plane logs and know which component owns which failure

**L3**
- Fleet upgrade strategy with a tested rollback path
- Blue/green cluster migration as an alternative to in-place upgrades — with the cost/risk comparison
- A real opinion on managed vs self-managed with numbers behind it
- Handling API deprecations across versions at scale without breaking app teams

**Evidence artifact:** Build one cluster by hand (kubeadm, or fully manual) so nothing is a black box — then throw it away. Run the real homelab on Talos or k3s. Then: destroy the cluster's etcd and restore from backup. Then upgrade two minor versions in sequence. Time both.

---

## Domain 7 — Observability & SLOs

**L1**
- `kubectl logs`/`describe`/`events`, a working Prometheus + Grafana stack

**L2**
- What each exporter actually gives you: kube-state-metrics (object state) vs cAdvisor (container resource) vs node-exporter (host) — and why people confuse them
- Control-plane SLIs that matter: apiserver request latency and inflight, etcd fsync duration, scheduler queue depth, kubelet PLEG
- Cardinality management and what a bad label costs you
- A repeatable debugging method for "the cluster is slow" that doesn't start with guessing

**L3**
- Define SLOs for a platform other teams consume, with an error-budget policy that has teeth
- Alert on symptoms, not causes; delete alerts nobody acts on
- Tracing across services, and honest cost/value math on the observability bill

**Evidence artifact:** Define a real SLO — "media playback starts within 3s, 99% of the time" — instrument it end to end, and then let it burn during one of your deliberate break/fix exercises. Watch the error budget drain in real time.

---

## Domain 8 — Delivery & GitOps

**L1**
- `helm install`, `kubectl apply`, basic chart values

**L2**
- Kustomize overlays vs Helm templating — the real tradeoff, not the tribal one
- Argo CD or Flux reconciliation, drift detection, sync waves and hooks
- Secrets in a GitOps world (SOPS, sealed secrets, external operators)
- Authoring a chart others can consume without reading its source

**L3**
- Design the platform contract: what app teams own, what the platform owns, where the seam is
- Progressive delivery (Argo Rollouts/Flagger) with automated rollback triggered by SLO breach
- Multi-environment promotion without environment drift
- A golden path that's genuinely easier than going around it — because that's the only enforcement that works

**Evidence artifact:** The entire homelab reconciled from one Git repo. Prove it: destroy the cluster completely and rebuild from Git alone, with only secrets restored out of band. Time it. That number is your platform's real RTO.

---

## Sequencing at one hour a day

Roughly 24 weeks. The hour splits about 40 minutes hands-on, 20 minutes reading — reverse that ratio and you'll retain far less.

### Phase 1 — Foundations (weeks 1–6)
Domain 6 (L1–L2) and Domain 1 (L1–L2). Build a cluster by hand, then rebuild on Talos/k3s. Learn the object model properly before you migrate anything. Nothing moves off Nomad yet.

### Phase 2 — Migration (weeks 7–14)
Domain 8, then 3, then 2 and 4. Stand K8s up *alongside* Nomad and move one service at a time, starting with the one nobody in the house will miss. Get GitOps in place early so every subsequent migration is cheap. Plex moves last.

For each service, write two sentences: what Nomad did, what Kubernetes needs instead, and why the difference exists. That document becomes your best interview and design-review material.

### Phase 3 — Depth and breakage (weeks 15–24)
Domain 5, then 7, then a second pass to L3 across everything. **One deliberate failure per week, scheduled**, with a written postmortem. A homelab will never fail on its own often enough to teach you anything — the breakage calendar is the single highest-leverage part of this plan.

Suggested failure rotation: etcd quorum loss → expired certs → node disk pressure → NetworkPolicy misconfiguration → admission webhook down → PVC stuck on a dead node → DNS outage → resource-limit cascade.

---

## The cross-cutting staff signal

None of the eight domains capture the thing that actually separates senior from staff. That's:

- Can you say *why not Kubernetes*, specifically, for a given workload?
- Can you name what your platform costs — in dollars, in cognitive load on app teams, in on-call burden?
- Can you design something other engineers adopt without being told to?

Check yourself against those three every re-scoring cycle. They don't have a syllabus.