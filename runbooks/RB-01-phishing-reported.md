# RB-01 — Phishing Email Reported by User

## Trigger Condition

A user forwards or reports a suspicious email (via the "Report Phishing" mail plugin, a forward to the SOC mailbox, or a GLPI ticket raised by the service desk on the user's behalf).

## Default Severity

**P3 — Medium** (pending verdict). See [severity-matrix.md](../severity-matrix.md).

**Escalation conditions (severity increases to P2):**

- User confirms they clicked a link and/or entered credentials on the linked page.
- The email was sent to multiple users (mass campaign) rather than a single recipient.
- The email impersonates internal IT/HR/Finance and requests credentials, MFA codes, or payment action (BEC-style).
- Sender domain or URL matches an IOC already flagged elsewhere in the environment this month.

## Target Response SLA

2 hours (P3) / 30 minutes if re-assessed to P2 on intake. See [severity-matrix.md](../severity-matrix.md).

## Ownership

L1 SOC Analyst (triage and verdict). Escalates to L2 per criteria below.

## L1 Investigation Steps

1. Open the reported email in a safe/isolated mail view (never open links or attachments directly on a live endpoint).
2. Record sender address, display name, reply-to address, and originating IP from mail headers.
3. Check SPF/DKIM/DMARC authentication results in the mail headers.
4. Extract all URLs and attachment hashes from the email.
5. Submit extracted URLs to a **URL reputation service** and record the verdict.
6. Submit attachment hashes (if any) to a **file reputation / sandbox service** and record the verdict.
7. Check whether the same sender address or URL has triggered other tickets in GLPI in the last 30 days.
8. Interview the reporting user (via ticket comment or call): did they click any link, open any attachment, or enter any credentials?
9. If credential entry is suspected, check the authentication logs for the user's account for logins from unfamiliar locations/IPs since the email was received.
10. Determine verdict: **true positive** (malicious) or **false positive** (legitimate/marketing/misidentified).
11. Document the verdict and evidence in the ticket before proceeding to closure actions.

## Actions L1 Is Permitted to Take Unaided

- Quarantine or delete the reported email from the recipient's mailbox.
- Search and remove matching copies of the same email from other mailboxes in the same domain (search-and-destroy), if the mail security tool supports org-wide search.
- Block the sender address and/or malicious URL at the mail gateway / web proxy, if a single indicator and org policy permits L1-level blocklist entries.
- Advise the reporting user on next steps (e.g., "no action needed, this was legitimate" or "please change your password as a precaution" — see escalation criteria before advising a password reset is mandatory).
- Close the ticket as a confirmed false positive with documented evidence.

## Escalation Criteria (L1 → L2)

Escalate immediately if any of the following apply — see [escalation-matrix.md](../escalation-matrix.md) for handoff format:

- User confirms credential entry or MFA approval on a phishing page (severity → P2; account compromise workflow required).
- Evidence of a mass campaign (same email to 5+ users) rather than a single targeted report.
- BEC-style email requesting a financial transaction or payroll/bank detail change.
- Sandbox/reputation checks return inconclusive or ambiguous results and L1 cannot reach a confident verdict.
- Authentication logs show suspicious login activity on the affected account following the reported email.

## Closure and Documentation Requirements

- Ticket must record: sender details, header authentication results, URL/attachment reputation results, user interview outcome, and final verdict.
- If true positive: record all remediation actions taken (quarantine, org-wide search-and-destroy count, blocks applied) with timestamps.
- If false positive: record the reason (e.g., "legitimate marketing email from known vendor domain, SPF/DKIM pass, no malicious indicators").
- Ticket closed only after reporting user is notified of the outcome.
- Verdict and IOC (if any) logged for cross-reference against future reports of the same sender/URL.
