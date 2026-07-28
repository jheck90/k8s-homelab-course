# My setup

Copy to `MY-SETUP.md` (gitignored) and fill in. Local notes about your own hardware — not
part of the curriculum, and not something anyone forking this repo should inherit.

## Phase 1 — `kubeadm-scratch/`

The throwaway cluster. Must not share fate with whatever runs your workloads today.

| | |
|---|---|
| Substrate | *(VMs / bare metal / cloud)* |
| Node count | |
| OS + version | |
| Addresses | |
| K8s version | |
| Destroyed on | *(fill in when you throw it away — that date is Phase 1 complete)* |

## Phase 1b — `homelab/`

The real cluster, on Talos or k3s.

| | |
|---|---|
| Distro | |
| Substrate | |
| Node count | |
| Addresses | |
| K8s version | |

## Notes to self

Anything that would be a footgun on rebuild — BIOS settings, network config that isn't
declarative yet, a DHCP reservation you'll forget you made.
