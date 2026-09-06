# RB-04 — EDR Agent Offline or Unreachable

## Trigger Condition

EDR management console reports an agent has not checked in within the defined threshold (e.g., 30+ minutes for servers, 4+ hours for standard endpoints), or shows agent status as "offline"/"unreachable."

## Default Severity

**P3 — Medium** (standard endpoint, first occurrence). See [severity-matrix.md](../severity-matrix.md).

**Escalation conditions (severity increases to P2):**

- The offline asset is a server, domain controller, or other business-critical asset.
- The agent went offline **immediately following** a security alert or suspicious activity on the same host (possible tamper/disable by an attacker).
- The same host has gone offline repeatedly (3+ times in 7 days) — recurring pattern suggesting tampering or misconfiguration rather than routine connectivity loss.
- Multiple hosts go offline simultaneously (possible mass tampering or infrastructure issue).

## Target Response SLA

2 hours (P3) / 30 minutes if re-assessed to P2 on intake. See [severity-matrix.md](../severity-matrix.md).

## Ownership

L1 SOC Analyst (triage and verification). Escalates to L2 per criteria below.

## L1 Investigation Steps

1. Open the EDR console alert and record: hostname, last check-in timestamp, asset type (workstation/server), and offline duration.
2. Check the host's last known activity/telemetry before going offline for any preceding security alerts.
3. Check whether the host appears online/reachable on the network via a basic connectivity check (ping/ICMP or asset management tool, if available) — this distinguishes "host is off/asleep" from "host is up but agent is down."
4. Check EDR console/change logs for any recent policy push, agent update, or uninstall action tied to this host.
5. Check whether this host has a history of repeated offline events in the last 7 days.
6. If the host is reachable, contact the end user or the IT asset owner to confirm the device's physical status (powered off, in maintenance, VPN disconnected, etc.).
7. Determine verdict: **false positive** (routine — device powered off/asleep, in maintenance window, or normal connectivity loss) or **true positive concern** (unexplained offline status, especially following an alert, warranting deeper review).
8. Document findings and verdict.

## Actions L1 Is Permitted to Take Unaided

- Attempt remote agent restart/reconnect via the EDR console's built-in remediation action, if available.
- Contact the end user/IT asset owner to request the device be powered on and connected.
- Close the ticket as a confirmed false positive with documented evidence (e.g., "device confirmed powered off for scheduled maintenance, user contacted").
- Log the offline event for trend tracking even when closed as routine.

## Escalation Criteria (L1 → L2)

Escalate immediately if any of the following apply — see [escalation-matrix.md](../escalation-matrix.md) for handoff format:

- Offline asset is a server or other business-critical system.
- Agent went offline immediately following a security alert on the same host (possible tamper/evasion).
- Host has gone offline repeatedly (3+ times in 7 days) with no clear routine explanation.
- Multiple hosts offline simultaneously.
- Remote restart attempt fails and the end user/asset owner cannot confirm the device's physical status within the response SLA.

## Closure and Documentation Requirements

- Ticket must record: hostname, asset type, offline duration, connectivity check result, and end-user/owner confirmation (if obtained).
- If false positive (routine): record the confirmed reason (maintenance, power-off, connectivity loss).
- If escalated: record what preceding alert (if any) triggered the concern, and the reachability/tamper-check findings passed to L2.
- Ticket closed only after the agent is confirmed back online and reporting normally, or handed back from L2 with sign-off.
- Repeated-offline hosts logged for trend tracking (see monthly report "security tool health status").
