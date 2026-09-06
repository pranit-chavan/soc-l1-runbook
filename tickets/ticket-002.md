# INC-2026-08-02 — Phishing: Credential Entry on Spoofed IT Helpdesk Page

## Runbook Applied

[RB-01 — Phishing Email Reported by User](../runbooks/RB-01-phishing-reported.md)

## Severity

P3 at creation → **P2** (user confirmed entering credentials)

## Reported By / Source

User report via "Report Phishing" mail plugin — Manoj Pillai.

## Affected Asset(s) / User(s)

manoj.pillai@northstar-retail.example, host WKS-2078 (10.20.6.78)

## Ticket Lifecycle & Timestamps

| Status | Timestamp | Actor | Notes |
|---|---|---|---|
| New | 2026-08-07 14:02 IST | System | Auto-created from mail plugin report |
| Acknowledged | 2026-08-07 14:19 IST | Arjun Mehta (L1) | Assigned, review started |
| In Progress | 2026-08-07 14:22 IST | Arjun Mehta (L1) | Header/URL analysis, user interview |
| Escalated | 2026-08-07 15:05 IST | Arjun Mehta (L1) → Vikram Suresh (L2) | User confirmed entering O365 credentials |
| Resolved | 2026-08-07 16:40 IST | Vikram Suresh (L2) | Account containment complete |
| Closed | 2026-08-07 17:15 IST | Vikram Suresh (L2) | User notified, awareness follow-up logged |

## Investigation Notes

1. Email impersonates IT helpdesk: "your password is expiring" with a link to a spoofed login page.
2. Header check: SPF fail; sender domain `it-helpdesk-secure.example` unrelated to the org's actual domain.
3. URL reputation service flags the link domain as newly registered, categorized as phishing.
4. User interview: confirms entering username and password on the linked page; did not complete the follow-up MFA challenge (did not recognize the prompt as legitimate and dismissed it).
5. Auth log review: one failed MFA challenge from external IP `203.0.113.44`, ~10 minutes after the email was sent — consistent with the attacker attempting to reuse the stolen credentials, blocked by MFA.
6. Escalated per criteria: confirmed credential entry on a phishing page.

## Verdict

**True Positive** — credential-harvesting page; password compromised, attacker's login attempt blocked by MFA.

## Actions Taken

- Email quarantined; org-wide search-and-destroy found and removed 3 additional copies in other mailboxes.
- Sender domain blocked at mail gateway.
- Escalated to L2 for account containment (exceeds L1 authority).
- **L2:** forced password reset, revoked all active sessions, reviewed account activity for further suspicious behavior — none found.

## SLA Outcome

- Target Response SLA (re-assessed to P2): 30 minutes
- Actual Response Time: 17 minutes
- Target Resolution SLA (P2): 8 hours
- Actual Resolution Time: 2 hours 38 minutes
- **Result: Met**

## Closure Notes

User notified and briefed on recognizing spoofed login pages. Case flagged to the client's awareness-training coordinator for inclusion in the next phishing-simulation cycle.
