# INC-2026-08-01 — Phishing Email Reported (Vendor Marketing, False Positive)

## Runbook Applied

[RB-01 — Phishing Email Reported by User](../runbooks/RB-01-phishing-reported.md)

## Severity

P3 (unchanged)

## Reported By / Source

User report via "Report Phishing" mail plugin — Rina Kapoor.

## Affected Asset(s) / User(s)

rina.kapoor@northstar-retail.example, host WKS-1042 (10.20.4.42)

## Ticket Lifecycle & Timestamps

| Status | Timestamp | Actor | Notes |
|---|---|---|---|
| New | 2026-08-03 09:14 IST | System | Auto-created from mail plugin report |
| Acknowledged | 2026-08-03 09:41 IST | Priya Desai (L1) | Assigned, review started |
| In Progress | 2026-08-03 09:45 IST | Priya Desai (L1) | Header and URL analysis underway |
| Resolved | 2026-08-03 10:20 IST | Priya Desai (L1) | Verdict reached |
| Closed | 2026-08-03 11:00 IST | Priya Desai (L1) | Reporter notified |

## Investigation Notes

1. Email from `no-reply@northstar-vendor-updates.example` promoting a webinar sign-up.
2. SPF/DKIM/DMARC all pass; sender domain matches a vendor with prior legitimate correspondence on file.
3. URL reputation service: clean, resolves to the vendor's registered marketing domain.
4. No attachments present.
5. Sender not matched against any prior GLPI ticket.
6. User interview: did not click any link, reported out of caution per awareness training.

## Verdict

**False Positive** — legitimate vendor marketing email; authentication checks pass and URL reputation is clean.

## Actions Taken

- Advised reporting user no action needed.
- No block applied — sender is a known legitimate vendor.

## SLA Outcome

- Target Response SLA: 2 hours
- Actual Response Time: 27 minutes
- Target Resolution SLA: 24 hours
- Actual Resolution Time: 1 hour 6 minutes
- **Result: Met**

## Closure Notes

User notified of outcome via ticket comment. No further action; case logged for baseline sender reputation reference.
