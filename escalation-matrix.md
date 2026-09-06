# Escalation Matrix

Defines when a ticket moves from L1 to L2, and from L2 to L3, what must travel with the handoff, and who is notified at each step. This matrix is referenced by every runbook's "Escalation Criteria" section rather than repeated in full there.

## Escalation tiers

| Tier | Role | Typical scope |
|---|---|---|
| **L1** | Tier-1 SOC Analyst | Triage, initial investigation, verdict on clear-cut cases, containment actions explicitly permitted by the runbook. |
| **L2** | Tier-2 / Senior SOC Analyst | Deeper investigation, cross-host/cross-log correlation, containment actions beyond L1 authority (account disablement, network isolation of a single host, malware sample handling). |
| **L3** | Incident Response Lead / SOC Manager | Multi-host or confirmed breach scenarios, forensic evidence handling, client executive communication, regulatory/legal coordination, decisions with contractual or financial impact. |

## L1 → L2

**Triggers:**

- Ticket severity is **P1** or **P2** at creation or after re-assessment.
- The relevant runbook's investigation steps are exhausted and the verdict is still inconclusive.
- Confirmed **true positive** requiring a containment action outside the "actions L1 is permitted to take unaided" list in the runbook (e.g., disabling an AD account, isolating a host from the network, blocking at the firewall/proxy beyond a single indicator).
- Response or resolution SLA is at risk of breach (a fixed buffer before the SLA deadline, per shift SOP — commonly 25% of remaining SLA window) and L1 cannot close it out in time.
- Same IOC, source IP, or user account triggers a **third ticket within 24 hours** (recurring pattern beyond a single-incident runbook's scope).
- Client explicitly requests analyst-level (non-L1) contact on the ticket.

**Information that must travel with the handoff:**

- Ticket ID, severity, and current status/timestamps (created, acknowledged).
- Runbook applied and which investigation steps were completed, with results of each.
- All IOCs identified (hashes, IPs, URLs, sender addresses, usernames) and their reputation-lookup results.
- Affected asset(s) and user(s), with hostname/IP (internal ranges only) and business criticality if known.
- Actions already taken (e.g., email quarantined, host network-isolated is NOT yet done — flag explicitly what has and hasn't been actioned).
- L1's working verdict and confidence level, and the specific reason escalation is required (which trigger above applies).

**Who is notified:**

- Ticket reassigned to the on-call L2 analyst in GLPI with escalation reason in the ticket notes.
- P1/P2: L1 also notifies the **Shift Lead** directly (call/chat per shift SOP) in addition to the GLPI reassignment — do not rely on queue visibility alone for time-critical tickets.
- P3/P4: GLPI reassignment is sufficient; no separate call required.

## L2 → L3

**Triggers:**

- Confirmed compromise spanning **multiple hosts or user accounts** (lateral movement evidence).
- Confirmed ransomware activity, active data exfiltration, or destructive malware behavior.
- Containment requires action beyond L2 authority: network segment isolation, disabling privileged/service accounts, engaging the client's executive or legal contact, or public-facing service takedown.
- Evidence handling requires forensic imaging or chain-of-custody procedures.
- Root cause is a previously unseen (not covered by existing runbooks) attack technique or malware family.
- Client relationship impact: the incident is likely to trigger a contractual SLA penalty, regulatory notification (e.g., breach disclosure), or media/reputational exposure.

**Information that must travel with the handoff:**

- Everything passed at L1→L2, plus:
- L2's additional investigation findings and correlated events across hosts/timeframes.
- Full incident timeline with timestamps (first indicator → detection → containment actions to date).
- Scope assessment: number of hosts/accounts/data assets believed affected, and confidence in that scope being complete.
- Any client communication already sent, and what the client has been told so far.
- Recommended next action (e.g., "isolate VLAN X", "initiate forensic imaging on host Y") with justification.

**Who is notified:**

- P1 confirmed breach/ransomware: L2 initiates the incident bridge/call per shift SOP, brings in the **Incident Response Lead / SOC Manager** and notifies the **Shift Lead** and **Client SPOC** in parallel — this is not a queue reassignment, it's an active handoff.
- P2 requiring L3 input: ticket reassigned in GLPI to L3 queue with a direct notification (call/chat) to the Incident Response Lead; Shift Lead cc'd.
- Client SPOC notification timing and content for confirmed incidents follows the client's contractual notification SLA, coordinated by L3 — L1/L2 do not contact the client directly on confirmed P1 incidents beyond the initial acknowledgment already covered by the runbook.

## Notes

- Escalation is not a failure state — a runbook that routes a scenario to L2/L3 is working as designed. Tickets are still tracked for MTTA/MTTR against the assigned tier's SLA contribution, not penalized for the handoff itself.
- Under-escalation (an analyst sitting on a P1/P2 or a confirmed true positive without escalating) is treated as a process deviation and reviewed in shift handover notes.
- De-escalation is possible: if L2/L3 review determines a ticket was over-escalated (e.g., confirmed false positive), it can be handed back to L1 for closure documentation, with the reviewing tier's findings recorded in the ticket.
