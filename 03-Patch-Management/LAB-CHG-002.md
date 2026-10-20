# LAB-CHG-002

> Publication note: This record retains observations made during the change. Later outcomes are summarized in the [milestone report](../07-Final-Report/README.md). Evidence registers include suggested captures; the [published evidence index](../06-Evidence/README.md) lists the files actually included.

CHANGE RECORD: LAB-CHG-002

Environment: Personal Hyper-V lab
Status: Installation and reporting verified; original WSUS delivery issue remains open
Target: SRV01 — Pilot server
Update: KB5126655 — October 2026 cumulative update for Windows Server 2022

PURPOSE
Install the October cumulative update and investigate why WSUS did not offer it to SRV01.

BEFORE-CHANGE BASELINE
- Operating system: Windows Server 2022 Datacenter Evaluation, version 21H2
- OS build: 20348.1970
- Installed servicing stack: 20348.1960
- Previous update KB5030216 verified locally and through WSUS.
- Domain secure channel: True

WSUS CONFIGURATION
- Update server: WSUS01.ad.northstar.test
- Update service address: http://wsus01.ad.northstar.test:8530
- Target group: Pilot
- KB5126655 approved for Pilot.
- Broad remained Not Approved.
- WSUS update state: Ready
- Exact approval time: Not recorded

INVESTIGATION
1. WSUS initially reported KB5126655 as NotApplicable.
2. Confirmed the update was absent from the local Get-HotFix results.
3. Installed intermediate update KB5030216 under LAB-CHG-001.
4. Verified OS build 20348.1970 and servicing stack 20348.1960.
5. Confirmed October’s update was approved and Ready in WSUS.
6. Client scans completed successfully but offered no updates.
7. Reviewed Windows Update logs. Historical servicing-stack errors were present, but their association with KB5126655 was not established.
8. Refreshed the local update cache, retaining SoftwareDistribution.lab-backup.
9. A fresh scan still offered no updates.
10. The WSUS prerequisite relationship query returned no entries.

IMPLEMENTATION
Downloaded the standalone KB5126655 package from Microsoft Update Catalog and installed it locally on SRV01.

Deployment source:
Microsoft Update Catalog — standalone installation.
Installation through WSUS was not demonstrated for this update.

VALIDATION RESULTS
- Get-HotFix lists KB5126655: Passed
- OS build changed from 20348.1970 to 20348.5761: Passed
- Test-ComputerSecureChannel returned True: Passed
- WSUS API reports KB5126655 as Installed: Passed
- Post-installation servicing-stack version: Not separately recorded
- Restart completion time: Not recorded
- Exact installation start and completion times: Not recorded

TIMESTAMP NOTE
Get-HotFix displayed an InstalledOn date of October 16, 2026, with a midnight time value. This does not establish the exact installation time. Lab screenshots previously showed UTC−07:00; preserve timezone context when comparing records.

IMPACT
SRV01 was the only deployment target. No application workload was configured, so validation covered the operating system, domain trust, and update reporting rather than application functionality.

RECOVERY APPROACH
Capture installation errors before further changes. If the disposable lab member server becomes unrecoverable, rebuild it from the ISO and rejoin the domain. Recovery was not required.

EVIDENCE
- Before-change OS build and servicing-stack screenshots
- WSUS NotApplicable query result
- WSUS approval and Ready-state evidence
- WindowsUpdate.log
- WindowsUpdate-after-reset.log
- SRV01-KB5126655-Verified.png
- WSUS-KB5126655-Installed.png

OUTCOME
KB5126655 installation is verified locally on SRV01, and WSUS subsequently reported it as Installed. The OS build is 20348.5761 and domain trust remains functional.

OPEN ISSUE AND FOLLOW-UP
The cause of the original WSUS detection/delivery issue remains unconfirmed. Successful standalone installation and WSUS reporting do not prove that WSUS delivery is fixed.

Configure SRV02’s Broad policy, capture its initial baseline, and test controlled WSUS deployment to determine whether the issue reproduces on another server.

This record verifies one OS update deployment and does not establish overall patch or vulnerability compliance.