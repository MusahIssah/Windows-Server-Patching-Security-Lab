# Project milestone report

This is an interim report for the completed phases, not the final report for all planned project work.

## Verified results

| System | Role/group | Final verified OS build | Final recorded WSUS summary |
|---|---|---|---|
| SRV01 | Pilot | 20348.5761 | 0 needed; 91 installed/not applicable; 0 errors; 0 unknown |
| SRV02 | Broad | 20348.5761 | 0 needed; 91 installed/not applicable; 0 errors; 0 unknown |
| WSUS01 | Infrastructure | 20348.5761 | 0 needed; 91 installed/not applicable; 0 errors; 0 unknown |
| DC01 | Infrastructure / AD DS / DNS | 20348.5761 | 0 needed; 91 installed/not applicable; 0 errors; 0 unknown |

These observations were recorded at different points in the work. They are not a simultaneous new scan or a guarantee about updates outside the 91-item catalog.

## Delivery and validation

- Pilot/Broad intermediate OS and October .NET deployments used WSUS.
- Pilot/Broad October OS installations used standalone Catalog packages; WSUS later reported Installed.
- WSUS01's October OS and .NET deployments used WSUS.
- DC01's October .NET, intermediate OS updates, and final successful October OS deployment used WSUS. Its earlier standalone October attempt rolled back.
- Local build, hotfix/component/history evidence and centralized reports were treated as separate checks.
- Selected DC health tests and member-domain trust checks passed within their stated scope.

## Monitoring and time

SRV01 and SRV02 applied a firewall logging GPO with allowed/dropped traffic enabled, separate profile files, and 20,480 KB file limits. Runtime Domain log generation was demonstrated.

WSUS01, SRV01, and SRV02 selected DC01 after disabling their Windows Time VM provider. All three retained automatic domain-time selection after the tested restarts. DC01 used external NTP and recovered automatically after a startup DNS-resolution failure on the observed October 20 startup.

## Technical lessons

Pre-restart success does not prove an update finalized. DC01's rollback was found by checking the post-restart build and logs. Matching INF files and a clean DISM scan did not prove complete driver transaction integrity. A later staged deployment succeeded without establishing a universal prerequisite or the original root cause.

An active time peer does not prove it is the selected source. Likewise, an empty managed-update scan does not prove complete patch coverage. Freshness and scope matter when interpreting both.

## Phase 4 — selected member-server findings validated

On October 21, 2026, credentialed Nessus scans identified certificate-padding, SMB-signing, and Intel BHI configuration findings on both member servers. Individual fixes were tested on SRV01, captured in LAB - Server Security Hardening, verified through resultant Group Policy, and rolled out to SRV02/Broad. Final scans showed Informational results, Auth: Pass, and credentialed checks via SMB. The selected findings were no longer detected within the recorded policy/feed scope.

Scanner: 10.12.4/build 20038; feed: 202609212044. Raw export was unavailable; screenshots support the record. Host/firmware protection, full application compatibility, and recovery were not established. See the [assessment, LAB-CHG-010, and evidence](../04-Vulnerability-Remediation/README.md).

## Remaining work

DC01 and WSUS01 vulnerability assessment, central monitoring/alerting, Configuration Manager, and backup-restoration testing remain outstanding. The original Pilot/Broad October detection issue, the precise DC01 driver rollback cause, and the initial startup DNS failure cause remain unconfirmed. See [ROADMAP.md](../ROADMAP.md).
