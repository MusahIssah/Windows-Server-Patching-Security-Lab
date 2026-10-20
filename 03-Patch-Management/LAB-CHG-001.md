# LAB-CHG-001 — SRV01 Intermediate OS Update

**Status:** Completed for KB5030216; October WSUS detection remained unresolved.

This publication summary reconciles the original approval-stage TXT with the later saved verification evidence and LAB-CHG-002. Exact start, completion, and approval timestamps were not recorded.

## Purpose and baseline

Deploy September 2023 cumulative update KB5030216 to SRV01 in Pilot, then reassess whether October KB5126655 is offered. The initial build was 20348.587, with servicing stack 20348.557. Domain trust was functional. WSUS recorded KB5030216 as NotInstalled and October as NotApplicable.

## Implementation and verification

Approved KB5030216 for Pilot while All Computers and Broad remained unapproved at that stage. Installed the update through WSUS management and restarted SRV01.

Verified the KB5030216 hotfix entry, OS build 20348.1970, servicing stack 20348.1960, member-domain secure channel True, and WSUS Installed reporting.

| Evidence | Purpose |
|---|---|
| [Local KB/build validation](../06-Evidence/SRV01-KB5030216-Verified.png) | Installation and resulting build |
| [Servicing stack](../06-Evidence/SRV01-ServicingStack-1960.png) | Intermediate stack verification |
| [WSUS status](../06-Evidence/WSUS-KB5030216-Installed.png) | Central reporting |

## Outcome and limits

The intermediate update completed. October still was not offered by subsequent managed scans, so its standalone deployment and unresolved delivery issue are recorded in [LAB-CHG-002](LAB-CHG-002.md). This sequence does not establish the cause of the original NotApplicable assessment.

The member server was temporarily unavailable during restart. No application workload or backup-restoration test was configured. Recovery was not required; rebuilding the disposable member server was the documented fallback.
