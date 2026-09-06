# INC-2026-08-09 — Approved USB Device Connection (Compliant)

## Runbook Applied

[RB-05 — Unauthorized USB / DLP Policy Violation](../runbooks/RB-05-usb-dlp-violation.md)

## Severity

P4 (unchanged)

## Reported By / Source

Automated device-control alert.

## Affected Asset(s) / User(s)

Host WKS-5521 (10.20.14.21), user Karan Malhotra (field sales).

## Ticket Lifecycle & Timestamps

| Status | Timestamp | Actor | Notes |
|---|---|---|---|
| New | 2026-08-11 10:00 IST | System | USB mass storage device connected |
| Acknowledged | 2026-08-11 15:30 IST | Priya Desai (L1) | Within 8 business-hour SLA |
| In Progress | 2026-08-11 15:32 IST | Priya Desai (L1) | Device registry check |
| Resolved | 2026-08-11 15:50 IST | Priya Desai (L1) | Confirmed compliant |
| Closed | 2026-08-11 16:10 IST | Priya Desai (L1) | Logged, no contact needed |

## Investigation Notes

1. Device-control alert: USB mass storage device connected; vendor/serial matches an entry on the approved device registry (encrypted, company-issued drive).
2. Action taken by the tool: **Allowed** (compliant device).
3. No files flagged as sensitive/classified in the accompanying log.
4. User's role (field sales) legitimately uses an encrypted USB drive for offline catalog files.
5. No prior violations on record for this user.

## Verdict

**False Positive / Compliant** — allowlisted device, no policy violation.

## Actions Taken

- Closed as compliant; no user contact required beyond the standard log entry.

## SLA Outcome

- Target Response SLA: 8 business hours
- Actual Response Time: 5.5 hours
- Target Resolution SLA: 5 business days
- Actual Resolution Time: same business day
- **Result: Met**

## Closure Notes

Logged for routine device-usage trend tracking. No further action.
