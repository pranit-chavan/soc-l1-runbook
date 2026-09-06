# Severity Matrix

Defines P1–P4 severity levels used across all runbooks and tickets in this repository. Severity is assigned by the L1 analyst at ticket creation based on **business impact and scope**, not just technical indicators, and can be revised by L2/L3 on review.

SLA clock starts at ticket creation (`New` status) in GLPI and runs on a 24x7 shift model, standard for an Indian MSSP handling multi-client coverage across time zones.

| Severity | Definition | Example Incidents | Response SLA | Resolution SLA |
|---|---|---|---|---|
| **P1 — Critical** | Confirmed active compromise, ongoing data loss, or a service-down condition affecting business-critical systems. Immediate financial, regulatory, or reputational impact. | Ransomware encryption in progress; confirmed domain admin account compromise; active data exfiltration; critical production server down due to attack. | 15 minutes | 4 hours |
| **P2 — High** | Confirmed malicious activity contained to a single host/user with no evidence of lateral movement, or a high-confidence indicator requiring urgent containment. | Malware confirmed executed on a single endpoint; successful brute-force login followed by anomalous activity; EDR agent offline on a server class asset for >30 minutes. | 30 minutes | 8 hours |
| **P3 — Medium** | Suspicious activity requiring investigation, not yet confirmed malicious, with limited or no immediate business impact. | User-reported phishing email pending verdict; repeated failed logins without success; EDR agent offline on a standard endpoint; DLP alert on file movement pending classification. | 2 hours | 24 hours |
| **P4 — Low** | Informational, policy, or hygiene-related finding with no active threat indicated. | Blocked phishing email (no user interaction); one-off failed login within normal threshold; expired/outdated EDR signature version; USB device connected but policy-compliant. | 8 business hours (next shift handover) | 5 business days |

## Notes on application

- **Response SLA** = time from ticket creation to the ticket moving to `Acknowledged` status by an assigned analyst.
- **Resolution SLA** = time from ticket creation to `Resolved` status (verdict reached and containment/remediation actions, if any, completed). `Closed` (client/stakeholder sign-off) is tracked separately and does not count against resolution SLA.
- Severity can escalate mid-investigation (e.g., a P3 phishing report where the user confirms they entered credentials becomes P2). The runbook for each incident type specifies its own escalation triggers — see individual runbook files.
- P1 and P2 tickets require L1 to notify the shift lead / L2 on-call immediately per the [escalation matrix](escalation-matrix.md), regardless of whether L1 believes they can contain it.
- SLA timers pause only for confirmed **false positive** verdicts reached within the response window, not for "pending client response" — an MSSP is still accountable for its own investigation timeline even if the client is slow to reply.
