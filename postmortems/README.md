# postmortems/

One per deliberate failure. Phase 3 schedules one per week — a homelab will not fail on its
own often enough to teach anything, so the breakage calendar is the leverage.

`TEMPLATE.md` is the shape. Naming: `YYYY-MM-DD-<failure>.md`.

## Scheduled rotation

| # | Failure | Domain | Date | Status |
|---|---|---|---|---|
| 1 | etcd quorum loss | 6 | | |
| 2 | Expired certificates | 6 | | |
| 3 | Node disk pressure | 2 | | |
| 4 | NetworkPolicy misconfiguration | 3 | | |
| 5 | Admission webhook down | 1 / 5 | | |
| 6 | PVC stuck on a dead node | 4 | | |
| 7 | DNS outage | 3 | | |
| 8 | Resource-limit cascade | 2 | | |

An L2 claim on a domain requires that you caused the failure deliberately and then fixed it.
This table is the evidence trail for that claim — fill in the dates.
