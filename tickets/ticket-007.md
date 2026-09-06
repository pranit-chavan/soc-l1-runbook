# INC-2026-08-07 — EDR Agent Offline, Routine Connectivity Loss (False Positive)

## Runbook Applied

[RB-04 — EDR Agent Offline or Unreachable](../runbooks/RB-04-edr-agent-offline.md)

## Severity

P3 (unchanged)

## Reported By / Source

Automated EDR console alert — agent check-in overdue.

## Affected Asset(s) / User(s)

Host WKS-4456 (10.20.11.56), user Tanvi Bhatt.

## Ticket Lifecycle & Timestamps

| Status | Timestamp | Actor | Notes |
|---|---|---|---|
| New | 2026-08-02 16:10 IST | System | Last check-in 3.5 hours prior |
| Acknowledged | 2026-08-02 17:00 IST | Priya Desai (L1) | Assigned |
| In Progress | 2026-08-02 17:03 IST | Priya Desai (L1) | Connectivity check, user contact |
| Resolved | 2026-08-02 17:35 IST | Priya Desai (L1) | Agent back online |
| Closed | 2026-08-02 18:00 IST | Priya Desai (L1) | Confirmed normal |

## Investigation Notes

1. EDR console: WKS-4456 last check-in 3.5 hours ago; no preceding security alert on this host.
2. Connectivity check: host not reachable on the corporate network (VPN disconnected).
3. No recent policy push, agent update, or uninstall action recorded for this host.
4. No repeat offline history for this host in the past 7 days.
5. User contacted: confirms the laptop was closed/asleep while working from a client site with no VPN connectivity that afternoon.

## Verdict

**False Positive** — routine connectivity loss; no preceding alert, no repeat pattern.

## Actions Taken

- Remote restart attempted; agent reconnected once the user re-established VPN at 17:30.
- Event logged, no further action required.

## SLA Outcome

- Target Response SLA: 2 hours
- Actual Response Time: 50 minutes
- Target Resolution SLA: 24 hours
- Actual Resolution Time: 1 hour 25 minutes
- **Result: Met**

## Closure Notes

Closed after confirming the agent resumed normal check-ins.
