# INC-2026-08-08 — Database Server EDR Agent Offline After Alert (SLA BREACH)

## Runbook Applied

[RB-04 — EDR Agent Offline or Unreachable](../runbooks/RB-04-edr-agent-offline.md)

## Severity

P3 at creation → **P2** (business-critical server, offline immediately following an alert)

## Reported By / Source

Automated EDR console alert — agent unreachable.

## Affected Asset(s) / User(s)

Host SVR-DB02 (10.10.1.12) — production database server, unattended server-room asset.

## Ticket Lifecycle & Timestamps

| Status | Timestamp | Actor | Notes |
|---|---|---|---|
| New | 2026-08-22 13:05 IST | System | Agent unreachable, 6 min after a suspicious-process alert |
| Acknowledged | 2026-08-22 13:20 IST | Rohan Iyer (L1) | Assigned |
| In Progress | 2026-08-22 13:22 IST | Rohan Iyer (L1) | Connectivity + tamper check |
| Escalated | 2026-08-22 13:50 IST | Rohan Iyer (L1) → Vikram Suresh (L2) | Server asset + offline immediately after an alert |
| Resolved | 2026-08-22 22:10 IST | Vikram Suresh (L2), Deepak Menon (IR Lead) | Root cause confirmed: failed agent upgrade |
| Closed | 2026-08-23 09:15 IST | Vikram Suresh (L2) | Client notified, maintenance process updated |

## Investigation Notes

1. EDR console: SVR-DB02 last check-in at 13:05, immediately preceded by a "suspicious process" alert at 12:59 (separately reviewed and confirmed benign backup-agent behavior).
2. Connectivity check: host still reachable via ping — the host itself is up, only the EDR agent is unreachable, raising a tamper concern.
3. No user available to confirm physical status directly (unattended server-room asset); request routed to the client's infrastructure team.
4. Escalated immediately per criteria: server asset + offline event immediately following an alert meets the escalation trigger regardless of the alert's own severity.
5. L2 requested the client's infrastructure team confirm server status. Response depended entirely on the client's on-call admin, who was reached only after several hours — a client-side holiday reduced admin availability that day.
6. Once reached (21:40), the client admin confirmed a scheduled EDR agent version upgrade had failed silently mid-installation, stopping the agent service — not tampering. Agent was manually restarted and resumed reporting by 22:05.

## Verdict

**True Positive** on the offline condition itself (a real service failure occurred, this was not a false alarm) — **False Positive** on the tamper/compromise concern that drove the escalation.

## Actions Taken

- L1 escalated per criteria on asset class and timing alone.
- L2/L3 coordinated with the client's infrastructure team for remote confirmation.
- Agent manually restarted; future EDR upgrades on this asset class to run under a notified maintenance window.

## SLA Outcome

- Target Response SLA (P3 at creation): 2 hours
- Actual Response Time: 15 minutes
- **Response SLA: Met**
- Target Resolution SLA (re-assessed to P2 at 13:50): 8 hours from creation (target ~21:05)
- Actual Resolution Time: 22:10 (~9 hours 5 minutes from creation)
- **Resolution SLA: Breached** (by ~1 hour 5 minutes)

**Root cause (honest):** resolution depended entirely on a third-party (client infrastructure team) confirmation, and no interim follow-up checkpoint was set when that confirmation didn't arrive within a reasonable window — L2 waited on a single outbound call/email rather than escalating the delay itself. The wait coincided with reduced client-side staffing due to a local holiday, which was not accounted for in the notification plan. This is a process gap in how the SOC tracks tickets blocked on external dependencies, not an investigation error.

## Closure Notes

Client notified of the failed upgrade and the resulting delay. Two follow-ups opened: (1) a 2-hour reminder checkpoint on any ticket blocked on a client/third-party dependency, and (2) identification of a backup client contact for holiday coverage. Scheduled EDR upgrades will now use maintenance-window notifications going forward.
