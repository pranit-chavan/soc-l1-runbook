# RB-03 — Brute-Force Login Attempts

## Trigger Condition

SIEM/identity provider alert fires for repeated failed login attempts against a single account or a single source exceeding the defined threshold (e.g., 10+ failures within 10 minutes), for VPN, email, or application logins.

## Default Severity

**P3 — Medium** (failed attempts only, no successful login). See [severity-matrix.md](../severity-matrix.md).

**Escalation conditions (severity increases to P2):**

- One or more of the failed-attempt bursts is followed by a **successful login**.
- The targeted account has privileged/admin access.
- The source IP is associated with a known-malicious range or matches a threat intel indicator.
- The pattern spans multiple accounts from a single source (password spraying) rather than one account.

## Target Response SLA

2 hours (P3) / 30 minutes if re-assessed to P2 on intake. See [severity-matrix.md](../severity-matrix.md).

## Ownership

L1 SOC Analyst (triage and initial containment). Escalates to L2 per criteria below.

## L1 Investigation Steps

1. Open the alert and record: targeted account(s), source IP(s), number of failed attempts, and time window.
2. Check whether any attempt in the sequence resulted in a **successful authentication**.
3. Check the geolocation and reputation of the source IP against a **threat intel / IP reputation service**.
4. Check whether the source IP or ASN matches the user's normal login pattern (e.g., known corporate VPN egress vs. unfamiliar country).
5. Check whether the same source IP is targeting other accounts in the same time window (spray pattern) via SIEM search.
6. If MFA is enabled on the account, check for any MFA prompt/approval logs corresponding to the failed attempts.
7. Contact the account owner (if a working-hours business account) to confirm whether the login attempts were their own (e.g., forgotten password, locked out) or unrecognized.
8. Determine verdict: **true positive** (malicious brute-force activity) or **false positive** (user error, misconfigured device/service account, expired cached credentials).
9. Document source IP reputation, account owner confirmation (if obtained), and verdict.

## Actions L1 Is Permitted to Take Unaided

- Block the source IP at the firewall/VPN gateway/identity provider if it is a single external IP with no legitimate business use.
- Force a password reset prompt on the account (without disabling it) if the account owner confirms unfamiliarity with the attempts, per standing SOP.
- Advise the account owner to change their password as a precaution.
- Close the ticket as a confirmed false positive with documented evidence (e.g., "user confirmed own failed attempts due to expired password, no compromise").

## Escalation Criteria (L1 → L2)

Escalate immediately if any of the following apply — see [escalation-matrix.md](../escalation-matrix.md) for handoff format:

- A successful login followed the failed-attempt burst (possible account compromise) — requires account disablement, which exceeds L1 authority.
- Targeted account is privileged/admin (domain admin, service account with elevated rights).
- Password-spraying pattern detected against multiple accounts from one source.
- Source IP matches a known-malicious threat intel indicator.
- Account owner cannot be reached within the response SLA window and successful-login risk cannot be ruled out.

## Closure and Documentation Requirements

- Ticket must record: account(s) targeted, source IP(s) and reputation check result, attempt count/window, successful-login check result, and account owner confirmation status.
- If true positive: record containment action taken (IP block, password reset enforced) with timestamps.
- If false positive: record the user-confirmed explanation.
- Ticket closed only after confirming no successful unauthorized login occurred, or handed back from L2 with remediation sign-off.
- Recurring source IPs or account targets logged for trend tracking (see monthly report "top recurring issue").
