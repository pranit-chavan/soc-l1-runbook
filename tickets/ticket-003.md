# INC-2026-08-03 — Malware Blocked Pre-Execution (Macro Downloader)

## Runbook Applied

[RB-02 — Malware Detected on Endpoint](../runbooks/RB-02-malware-endpoint.md)

## Severity

P2 (unchanged — blocked pre-execution, single host)

## Reported By / Source

Automated EDR alert.

## Affected Asset(s) / User(s)

Host WKS-3311 (10.20.9.11), user Sneha Joshi.

## Ticket Lifecycle & Timestamps

| Status | Timestamp | Actor | Notes |
|---|---|---|---|
| New | 2026-08-10 03:22 IST | System | EDR quarantine alert (night shift) |
| Acknowledged | 2026-08-10 03:29 IST | Kavya Nair (L1) | Assigned |
| In Progress | 2026-08-10 03:31 IST | Kavya Nair (L1) | Hash/sandbox lookup, process tree review |
| Resolved | 2026-08-10 03:58 IST | Kavya Nair (L1) | Verdict reached, scan clean |
| Closed | 2026-08-10 08:15 IST | Kavya Nair (L1) | User briefed at shift handover |

## Investigation Notes

1. EDR alert: `Trojan.GenericKD` detected in `invoice_0847.xlsm`, downloaded via personal webmail attachment. Action = **Quarantined** (prevented pre-execution).
2. Hash submitted to file reputation/sandbox service: confirmed malicious macro downloader.
3. Process tree: no child processes spawned — execution did not occur.
4. Searched EDR for the same hash across the environment: no other hosts affected.
5. Delivery vector: personal webmail accessed on a corporate laptop, macro-enabled spreadsheet attachment.
6. EDR agent online, signature version current.

## Verdict

**True Positive** — malicious macro downloader, prevented before execution by EDR.

## Actions Taken

- Quarantine action confirmed (no reversal needed).
- On-demand full scan run on the host — clean.
- User advised to stop accessing personal webmail on the corporate device; policy reminder issued.

## SLA Outcome

- Target Response SLA: 30 minutes
- Actual Response Time: 7 minutes
- Target Resolution SLA: 8 hours
- Actual Resolution Time: 36 minutes
- **Result: Met**

## Closure Notes

Host confirmed clean via scan. Ticket closed after user was briefed during morning shift handover.
