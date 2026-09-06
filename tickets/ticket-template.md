# Ticket Template

Fields mirror a GLPI incident ticket as used by the SOC. Every worked ticket in this folder follows this exact structure.

---

## Ticket ID

`INC-YYYY-MM-##` (e.g., `INC-2026-08-01`)

## Title

Short one-line summary of the incident.

## Runbook Applied

Link to the runbook used, e.g. [RB-01 — Phishing Email Reported by User](../runbooks/RB-01-phishing-reported.md)

## Severity

Initial severity at creation, and final severity if re-assessed (per [severity-matrix.md](../severity-matrix.md)).

## Reported By / Source

How the ticket originated (user report, automated alert/tool name-category, service desk).

## Affected Asset(s) / User(s)

Hostname/IP (RFC 1918 range only) and/or username (fictional).

## Ticket Lifecycle & Timestamps

| Status | Timestamp | Actor | Notes |
|---|---|---|---|
| New | | | Ticket created |
| Acknowledged | | | L1 analyst assigned/acknowledged |
| In Progress | | | Investigation underway |
| Escalated *(if applicable)* | | | Handed to L2/L3, reason noted |
| Resolved | | | Verdict reached, actions completed |
| Closed | | | Reporter/client notified, ticket closed |

## Investigation Notes

Numbered findings from applying the runbook's investigation steps.

## Verdict

**True Positive** / **False Positive** — with one-line justification.

## Actions Taken

List of containment/remediation actions applied, with timestamps.

## SLA Outcome

- Target Response SLA: _(from severity matrix)_
- Actual Response Time: _(Acknowledged − New)_
- Target Resolution SLA: _(from severity matrix)_
- Actual Resolution Time: _(Resolved − New)_
- **Result:** Met / Breached — if breached, honest root-cause note.

## Closure Notes

Final documentation per the runbook's closure requirements; confirmation reporter/client was notified.
