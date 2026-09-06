# SOC L1 Operations — Portfolio Documentation Set

This repository reproduces the operational documentation a Tier-1 (L1) SOC analyst produces day-to-day at a managed security services provider (MSSP): runbooks, severity and escalation matrices, worked incident tickets, and a monthly operations report.

## Why this exists

I'm a final-year Computer Engineering student preparing for an L1 SOC Analyst role at an Indian MSSP. I don't have production SOC access, so I can't show real incident data. What I can show is that I understand how an L1 queue actually runs: how a ticket is triggered, investigated, escalated, and closed, and how that work rolls up into SLA numbers a shift lead reports on.

Every artefact here is self-authored and fictional — no real company, no real IP outside RFC 1918 / documentation ranges, no real tool beyond what I've actually used (GLPI for ticketing, Excel for reporting). The point isn't to fake experience. It's to show I can produce the documentation an MSSP already runs on, to the standard it expects, before I've been handed a login.

## Repo structure

```
soc-l1-runbook/
├── README.md                  — this file
├── severity-matrix.md         — P1–P4 definitions, SLAs
├── escalation-matrix.md       — L1 → L2 → L3 handoff criteria
├── runbooks/                  — five L1 investigation runbooks
│   ├── RB-01-phishing-reported.md
│   ├── RB-02-malware-endpoint.md
│   ├── RB-03-bruteforce-login.md
│   ├── RB-04-edr-agent-offline.md
│   └── RB-05-usb-dlp-violation.md
├── tickets/                   — ticket template + 10 worked tickets
│   ├── ticket-template.md
│   └── ticket-001.md … ticket-010.md
└── reports/
    └── monthly-report-2026-08.md
```

## What each artefact demonstrates

| Artefact | Demonstrates |
|---|---|
| **Severity matrix** | Understanding of how MSSPs classify impact against SLA commitments, not just technical severity. |
| **Escalation matrix** | Knowing the boundary of L1 authority — what gets handled at shift level vs. handed to L2/L3, and what context has to travel with the handoff. |
| **Runbooks** | Ability to follow (and write) a repeatable, auditable investigation procedure under time pressure, with a clear line between "L1 can do this" and "L1 must escalate this." |
| **Worked tickets** | Applying the runbooks to plausible scenarios end-to-end through the ticket lifecycle, including honest SLA breaches — a queue with zero breaches doesn't reflect a real operation. |
| **Monthly report** | Translating a month of ticket data into the metrics a SOC manager reports upward: SLA compliance, MTTA/MTTR, recurring issues, tool health. |

## Stack

Markdown + GitHub for documentation, [GLPI](https://glpi-project.org/) as the ticketing/ITSM system of reference, Excel for the monthly report. This matches what a lean Indian MSSP L1 desk typically runs on — no enterprise SOAR/SIEM claimed that I haven't used.

## Status

Work in progress, built section by section. See individual files for the current state of each artefact.
