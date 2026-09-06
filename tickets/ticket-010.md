# INC-2026-08-10 — Unauthorized Transfer of Confidential Financial File to USB

## Runbook Applied

[RB-05 — Unauthorized USB / DLP Policy Violation](../runbooks/RB-05-usb-dlp-violation.md)

## Severity

P4 at creation → **P2** (confirmed transfer of classified data)

## Reported By / Source

Automated DLP alert.

## Affected Asset(s) / User(s)

Host WKS-6688 (10.20.18.88), user Alok Sharma (finance department).

## Ticket Lifecycle & Timestamps

| Status | Timestamp | Actor | Notes |
|---|---|---|---|
| New | 2026-08-27 09:30 IST | System | Confidential file copied to non-allowlisted USB |
| Acknowledged | 2026-08-27 09:52 IST | Kavya Nair (L1) | Assigned |
| In Progress | 2026-08-27 09:55 IST | Kavya Nair (L1) | Classification and device check |
| Escalated | 2026-08-27 10:40 IST | Kavya Nair (L1) → Vikram Suresh (L2) | Confirmed completed transfer of classified data |
| Resolved | 2026-08-27 14:20 IST | Vikram Suresh (L2), client HR/compliance contact | Device recovered, acknowledgment obtained |
| Closed | 2026-08-28 11:00 IST | Vikram Suresh (L2) | Policy fix confirmed |

## Investigation Notes

1. DLP alert: file `Q2_Financial_Consolidation_CONFIDENTIAL.xlsx` copied to a non-allowlisted USB device. Action = **detected only** — the transfer completed because the device-control policy for this endpoint group was set to "monitor," not "block" (a configuration gap, not a runbook gap).
2. File carries a "Confidential" classification tag in the DLP tool.
3. Device is not on the approved registry; serial number logged for reference.
4. User's role (finance) has legitimate access to the file itself, but no approved business justification for transferring consolidated financials off-network via USB.
5. No prior DLP violations on record for this user.
6. Escalated per criteria: confirmed completed transfer of classified data.

## Verdict

**True Positive** — unauthorized transfer of confidential financial data to an unapproved removable device.

## Actions Taken

- L1 flagged the device-control policy gap (monitor vs. block) for the finance endpoint group and referred it to L2 alongside the ticket.
- L2 contacted the user's manager and the client's HR/compliance contact per SOP.
- User was asked to surrender the device; forensic timestamp review found no evidence of further distribution.
- Formal written acknowledgment obtained from the user.
- Device-control policy for the finance endpoint group changed from "monitor" to "block" for unauthorized removable media.

## SLA Outcome

- Target Response SLA (P4 at creation): 8 business hours
- Actual Response Time: 22 minutes
- Target Resolution SLA (re-assessed to P2 at 10:40): 8 hours from creation
- Actual Resolution Time: 4 hours 50 minutes from creation
- **Result: Met**

## Closure Notes

Client HR/compliance handled the personnel-side outcome separately and directly with the user. SOC ticket closed after confirming the device-control policy change was applied and no further data exposure was found.
