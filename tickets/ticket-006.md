# INC-2026-08-06 — Credential-Stuffing Success Against Legacy IMAP Access

## Runbook Applied

[RB-03 — Brute-Force Login Attempts](../runbooks/RB-03-bruteforce-login.md)

## Severity

P3 at creation → **P2** (successful login followed a failed-attempt burst)

## Reported By / Source

Automated SIEM alert — repeated failed logins followed by a success.

## Affected Asset(s) / User(s)

finance.ops@northstar-retail.example (finance department mailbox), external source IP `203.0.113.87`.

## Ticket Lifecycle & Timestamps

| Status | Timestamp | Actor | Notes |
|---|---|---|---|
| New | 2026-08-19 22:40 IST | System | 22 failed logins in 12 min, then 1 success |
| Acknowledged | 2026-08-19 22:58 IST | Simran Kaur (L1) | Assigned (night shift) |
| In Progress | 2026-08-19 23:00 IST | Simran Kaur (L1) | IP reputation, MFA log review |
| Escalated | 2026-08-19 23:22 IST | Simran Kaur (L1) → Vikram Suresh (L2) | Successful login after brute-force burst |
| Resolved | 2026-08-20 00:15 IST | Vikram Suresh (L2) | Account disabled, sessions revoked, reset issued |
| Closed | 2026-08-20 09:00 IST | Vikram Suresh (L2) | Client notified of protocol gap |

## Investigation Notes

1. Alert: 22 failed logins against `finance.ops` within 12 minutes from external IP `203.0.113.87`, followed by one successful login.
2. IP reputation service flags `203.0.113.87` as associated with known credential-stuffing infrastructure.
3. Login geolocation inconsistent with the account's normal usage country.
4. Same source IP attempted 2 other accounts in the same window — a limited spray pattern, noted for L2.
5. MFA logs: no MFA challenge was recorded after the successful password-only login — the account was accessed via a legacy IMAP connection that bypasses the org's MFA enforcement policy.
6. Escalated immediately per criteria: successful login following a brute-force burst requires account disablement, which exceeds L1 authority.

## Verdict

**True Positive** — credential-stuffing success exploiting a legacy protocol path that bypasses MFA.

## Actions Taken

- L1 blocked the source IP at the VPN/firewall immediately.
- Escalated to L2, who disabled the account, revoked active sessions, and forced a password reset.
- IR review flagged the legacy IMAP access path lacking MFA enforcement as a systemic gap and referred it to the client's IT team.

## SLA Outcome

- Target Response SLA (P3 at creation): 2 hours
- Actual Response Time: 18 minutes
- Target Resolution SLA (re-assessed to P2 at 23:22): 8 hours from creation
- Actual Resolution Time: 1 hour 35 minutes from creation
- **Result: Met**

## Closure Notes

Client notified of the account compromise and the legacy-protocol gap. Recommendation issued for the client's IT team to disable IMAP access for non-service accounts org-wide — tracked as a recommendation, not a SOC-executed action.
