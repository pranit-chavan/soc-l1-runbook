# INC-2026-08-05 — Failed VPN Logins, Cached Old Credentials (False Positive)

## Runbook Applied

[RB-03 — Brute-Force Login Attempts](../runbooks/RB-03-bruteforce-login.md)

## Severity

P3 (unchanged)

## Reported By / Source

Automated SIEM alert — repeated failed VPN logins.

## Affected Asset(s) / User(s)

deepika.rao@northstar-retail.example, source host WKS-7790 (10.20.1.9, internal).

## Ticket Lifecycle & Timestamps

| Status | Timestamp | Actor | Notes |
|---|---|---|---|
| New | 2026-08-05 11:20 IST | System | Threshold alert: 14 failures in 8 min |
| Acknowledged | 2026-08-05 12:05 IST | Rohan Iyer (L1) | Assigned |
| In Progress | 2026-08-05 12:08 IST | Rohan Iyer (L1) | Source/account review, user contact |
| Resolved | 2026-08-05 12:40 IST | Rohan Iyer (L1) | Verdict reached |
| Closed | 2026-08-05 13:00 IST | Rohan Iyer (L1) | User confirmed fix |

## Investigation Notes

1. Alert: 14 failed VPN logins for `deepika.rao` within 8 minutes; source IP `10.20.1.9` — internal, matches the user's own assigned workstation.
2. No successful login within the burst.
3. Source IP is internal and targets only this one account — not a spray pattern.
4. User contacted directly: confirms she had just reset her password, and her VPN client had cached the old password, auto-retrying it on each connection attempt.
5. MFA logs: no MFA prompts issued — failures occurred at the password stage, consistent with the user's explanation.

## Verdict

**False Positive** — user error; cached VPN credentials retried the old password after a routine reset.

## Actions Taken

- Advised user to clear saved credentials in the VPN client and re-enter the new password.
- No IP block applied — internal source, confirmed legitimate user.

## SLA Outcome

- Target Response SLA: 2 hours
- Actual Response Time: 45 minutes
- Target Resolution SLA: 24 hours
- Actual Resolution Time: 1 hour 20 minutes
- **Result: Met**

## Closure Notes

User confirmed successful VPN login after clearing cached credentials. Ticket closed.
