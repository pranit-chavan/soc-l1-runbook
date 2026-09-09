# Monthly Security Operations Report

**Client:** Northstar Retail Pvt Ltd
**Reporting Period:** 01–31 August 2026
**Prepared By:** SOC Shift Lead — Ananya Rao
**Distribution:** Client IT/Security POC
**Source:** Compiled from GLPI ticket export; client-facing deliverable maintained in Excel (`SOC-Monthly-Report-2026-08.xlsx`). This file mirrors that workbook for documentation purposes.

---

## Executive Summary

10 tickets were worked this month across all five standard incident categories. 6 of 10 required escalation beyond L1 (confirmed true positives or asset-criticality triggers); 4 closed at L1 as false positives. SLA compliance was **80%** — 2 of 10 tickets breached SLA, both with root causes traced to SOC-side process gaps rather than investigation errors; corrective actions are already in motion for both (see [Escalation & SLA Breach Detail](#escalation--sla-breach-detail)). No incident this month resulted in confirmed data loss or extended service outage.

## Incidents by Category

| Category | Runbook | Tickets | True Positive | False Positive |
|---|---|---|---|---|
| Phishing reported | RB-01 | 2 | 1 | 1 |
| Malware on endpoint | RB-02 | 2 | 2 | 0 |
| Brute-force login | RB-03 | 2 | 1 | 1 |
| EDR agent offline | RB-04 | 2 | 1* | 1 |
| USB / DLP violation | RB-05 | 2 | 1 | 1 |
| **Total** | | **10** | **6** | **4** |

\* INC-2026-08-08: true positive on the underlying offline condition (a real agent failure occurred), false positive on the compromise/tamper concern that triggered escalation. Counted as true positive here since the technical fault was genuine.

## Incidents by Severity (Final, Post-Escalation)

| Severity | Count | % of Total |
|---|---|---|
| P1 — Critical | 1 | 10% |
| P2 — High | 5 | 50% |
| P3 — Medium | 3 | 30% |
| P4 — Low | 1 | 10% |
| **Total** | **10** | **100%** |

## SLA Compliance

| Metric | Value |
|---|---|
| Tickets meeting both response and resolution SLA | 8 / 10 |
| Tickets breaching SLA | 2 / 10 |
| **SLA Compliance Rate** | **80%** |

Both breaches are detailed under [Escalation & SLA Breach Detail](#escalation--sla-breach-detail) below, per client reporting agreement — breaches are reported with root cause, not just flagged.

## Mean Time to Acknowledge (MTTA) and Mean Time to Resolve (MTTR)

| Severity | Tickets | Mean Time to Acknowledge | Mean Time to Resolve |
|---|---|---|---|
| P1 | 1 | 55 min | 4h 45m |
| P2 | 5 | 16 min | 3h 45m |
| P3 | 3 | 41 min | 1h 17m |
| P4 | 1 | 5h 30m | 5h 50m |
| **All tickets (overall)** | **10** | **~59 min** | **~3h 19m** |

P4's MTTA reflects its 8-business-hour response SLA (device-control alert acknowledged same business day, well within target) and is not comparable to the P1–P3 figures on an absolute-time basis.

## Top Recurring Issue

**Personal webmail access enabling malware delivery to corporate endpoints.** Both malware tickets this month (INC-2026-08-03, INC-2026-08-04) originated from macro-enabled attachments downloaded via personal webmail on corporate devices — one was blocked pre-execution, the other executed and became the month's only P1, contributing directly to the SLA breach on INC-2026-08-04.

**Recommendation:** block personal webmail domains at the proxy for corporate endpoints, or enforce attachment sandboxing on all inbound webmail traffic. Referred to client IT for policy decision; SOC will track recurrence next reporting period regardless of outcome.

## Security Tool Health Status

| Tool | Status | Notes |
|---|---|---|
| EDR (endpoint) | Operational, 1 process gap identified | 2 offline events (1 routine connectivity loss, 1 failed silent agent upgrade — see INC-2026-08-08). Scheduled agent upgrades will move to notified maintenance windows going forward. |
| Mail security / gateway | Operational | SPF/DKIM/DMARC enforcement and quarantine actions functioned as expected across both phishing tickets. |
| DLP / device-control | Operational, 1 configuration gap corrected | Finance endpoint group was set to "monitor" instead of "block" for unauthorized removable media (INC-2026-08-10); corrected during the ticket. Recommend an audit of device-control policy across all endpoint groups next cycle. |
| Firewall / VPN / proxy | Operational | IP and domain blocks applied without issue across brute-force and malware tickets. |
| Identity provider / MFA | Operational, client-side gap flagged | MFA enforcement functioned correctly on modern auth paths; legacy IMAP access on `finance.ops` bypassed MFA entirely (INC-2026-08-06), enabling a credential-stuffing success. Referred to client IT — not a SOC-managed control. |
| Ticketing (GLPI) | Operational | No tooling issues affecting ticket tracking or SLA timers this month. |

## Escalation & SLA Breach Detail

**INC-2026-08-04 — Malware executed with C2 beaconing (P1)**
Response SLA breached by 25 minutes; resolution SLA breached by ~45 minutes. Root cause: the sole L1 analyst on the 02:00–03:00 shift-changeover window was single-handling an unrelated ticket when the alert fired, and no automated on-call paging exists for unacknowledged P2+ alerts during that window. Corrective actions opened: automated paging after 15 minutes unacknowledged; mandatory analyst overlap across the changeover window.

**INC-2026-08-08 — Database server EDR agent offline after alert (P2)**
Resolution SLA breached by ~1 hour 5 minutes. Root cause: resolution depended entirely on a client-side infrastructure team confirmation with no interim follow-up checkpoint set, compounded by reduced client staffing due to a local holiday. Corrective actions opened: 2-hour follow-up checkpoint on any ticket blocked on a third-party dependency; backup client contact identified for holiday coverage.

## Next Reporting Period — Carried-Forward Items

- Confirm proxy/sandboxing decision on personal webmail access (client IT).
- Confirm device-control policy audit completed across all endpoint groups.
- Confirm legacy IMAP disabled for non-service accounts (client IT).
- Track recurrence of on-call paging gap fix from INC-2026-08-04.
