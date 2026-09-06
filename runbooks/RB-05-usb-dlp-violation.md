# RB-05 — Unauthorized USB / DLP Policy Violation

## Trigger Condition

DLP or endpoint device-control tool generates an alert for an unauthorized removable media (USB) connection, or a policy violation involving sensitive file movement (e.g., classified file copied to removable media, uploaded to an unsanctioned web service, or emailed externally).

## Default Severity

**P4 — Low** (policy-compliant device connection or minor/first-time hygiene finding). See [severity-matrix.md](../severity-matrix.md).

**Escalation conditions (severity increases to P3, then P2):**

- **P3:** An unauthorized (non-allowlisted) USB device was connected and the device-control tool blocked the action, but the file classification/sensitivity involved is unclear and needs review.
- **P2:** DLP confirms sensitive/classified data (e.g., customer PII, financial records, source code marked confidential) was actually transferred to removable media, an external device, or an external destination before being blocked or noticed.
- **P2:** The same user has a prior confirmed DLP violation on record (repeat offender — policy/HR escalation required in addition to technical response).

## Target Response SLA

8 business hours (P4) / 2 hours if re-assessed to P3 / 30 minutes if re-assessed to P2. See [severity-matrix.md](../severity-matrix.md).

## Ownership

L1 SOC Analyst (triage and evidence collection). Escalates to L2 per criteria below. Confirmed policy violations involving data sensitivity classification or HR action are routed through L2/L3 with the client's data owner or compliance contact.

## L1 Investigation Steps

1. Open the DLP/device-control alert and record: username, hostname, device details (vendor ID/serial if captured), action taken by the tool (blocked/allowed/logged only), and files involved (name, path, classification tag if present).
2. Check whether the device is on the organization's approved/allowlisted device registry.
3. Check whether the DLP action was **preventive** (transfer blocked) or **detective only** (transfer occurred, alert raised after the fact).
4. If files were involved, check the file classification/sensitivity label (e.g., "Confidential," "PII") if the DLP tool tags this.
5. Check the user's role and whether removable media or the destination (e.g., personal cloud storage, personal email) is required for their normal job function.
6. Check the user's DLP violation history in GLPI for repeat occurrences in the last 90 days.
7. Contact the user (or their manager, per SOP) to confirm business justification for the action, if not immediately obvious.
8. Determine verdict: **false positive/compliant** (allowlisted device, no sensitive data, legitimate business use) or **true positive** (policy violation, with severity per sensitivity of data and whether transfer completed).
9. Document findings, classification of data involved, and verdict.

## Actions L1 Is Permitted to Take Unaided

- Confirm the DLP/device-control tool's automatic block action (no reversal without L2/manager approval).
- Close the ticket as compliant if the device is allowlisted and no policy violation occurred.
- Send a standard policy-reminder notice to the user for a first-time, low-sensitivity, blocked (non-completed) violation, per standing SOP.
- Log the event for trend tracking even when closed as routine.

## Escalation Criteria (L1 → L2)

Escalate immediately if any of the following apply — see [escalation-matrix.md](../escalation-matrix.md) for handoff format:

- DLP confirms a completed transfer (not blocked) of sensitive/classified data to removable media or an external destination.
- The user has a prior confirmed DLP violation on record (repeat offender).
- Business justification is unclear or disputed and requires manager/HR involvement.
- The device or destination shows signs of being used for deliberate data exfiltration (e.g., large volume of files, off-hours activity, resignation/termination pending on the account).
- Data classification cannot be determined by L1 and requires the client's data owner or compliance contact to assess sensitivity/impact.

## Closure and Documentation Requirements

- Ticket must record: user, hostname, device details, DLP action taken, files/data involved and classification, and business-justification outcome.
- If compliant: record the allowlist/justification confirmation.
- If true positive: record whether transfer completed, data sensitivity, and any HR/compliance referral made.
- Ticket closed only after documented user/manager acknowledgment (for policy-reminder cases) or handed back from L2 with remediation and HR/compliance sign-off (for confirmed violations).
- Repeat offenders and device/destination patterns logged for trend tracking (see monthly report "top recurring issue").
