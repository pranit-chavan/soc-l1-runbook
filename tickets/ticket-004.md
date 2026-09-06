# INC-2026-08-04 — Malware Executed with C2 Beaconing on Application Server (SLA BREACH)

## Runbook Applied

[RB-02 — Malware Detected on Endpoint](../runbooks/RB-02-malware-endpoint.md)

## Severity

P2 at creation → **P1** (business-critical server, active C2 beaconing)

## Reported By / Source

Automated EDR alert.

## Affected Asset(s) / User(s)

Host SVR-APP07 (10.10.2.7) — production application server.

## Ticket Lifecycle & Timestamps

| Status | Timestamp | Actor | Notes |
|---|---|---|---|
| New | 2026-08-14 02:10 IST | System | EDR detection-only alert (night shift) |
| Acknowledged | 2026-08-14 03:05 IST | Rohan Iyer (L1) | **55 min after creation — response SLA breached** |
| In Progress | 2026-08-14 03:07 IST | Rohan Iyer (L1) | Host isolated immediately on confirming execution |
| Escalated | 2026-08-14 03:40 IST | Rohan Iyer (L1) → Vikram Suresh (L2) + Ananya Rao (Shift Lead) | Server asset + confirmed C2 beaconing; incident bridge opened |
| Resolved | 2026-08-14 06:55 IST | Vikram Suresh (L2), Deepak Menon (IR Lead) | Contained, scope confirmed single-host |
| Closed | 2026-08-14 10:30 IST | Vikram Suresh (L2) | Client notified, post-incident summary sent |

## Investigation Notes

1. EDR alert on SVR-APP07: PowerShell process spawned from an Office document macro — **detection-only** (not blocked; flagged after execution).
2. Hash and PowerShell script submitted to sandbox service: confirmed malware downloader with C2 beaconing behavior.
3. Process tree shows repeated outbound connections to `198.51.100.23`.
4. EDR search across environment: no other hosts show the same hash or C2 indicator at time of check — contained to this host.
5. Host is a production application server — business-critical, escalation criteria met on asset class alone.
6. Persistence check: a scheduled task created for beacon re-execution was found and later removed by L2.

## Verdict

**True Positive** — malware executed with active C2 beaconing on a business-critical server.

## Actions Taken

- Host isolated via EDR by L1 immediately upon confirming execution (03:10).
- C2 domain blocked at proxy/firewall by L2.
- Scheduled-task persistence mechanism removed.
- IR Lead engaged for scope confirmation; review of the 04:00–06:30 window found no evidence of lateral movement.

## SLA Outcome

- Target Response SLA (P2 at creation): 30 minutes
- Actual Response Time: 55 minutes
- **Response SLA: Breached**
- Target Resolution SLA (re-assessed to P1 at 03:40): 4 hours from creation (target ~06:10)
- Actual Resolution Time: 06:55 (4 hours 45 minutes from creation)
- **Resolution SLA: Breached** (by ~45 minutes)

**Root cause (honest):** the sole L1 analyst on duty was covering the 02:00–03:00 shift-changeover gap alone — the outgoing analyst had already logged off and the incoming analyst arrived late — and was mid-triage on an unrelated P3 ticket when this alert fired. No secondary on-call analyst is currently paged automatically for P2+ alerts arriving during a shift-changeover window; the alert sat in the queue until the analyst cleared their existing task. This is a shift-coverage process gap, not an analyst skill or tooling failure.

## Closure Notes

Client notified the same morning with a full timeline and remediation summary. Post-incident review opened two action items: (1) automated on-call paging for any P1/P2 alert unacknowledged after 15 minutes, and (2) a hard overlap requirement between outgoing and incoming night-shift analysts.
