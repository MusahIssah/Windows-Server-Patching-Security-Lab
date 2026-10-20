# LAB-CHG-003

> Publication note: This record retains observations made during the change. Later outcomes are summarized in the [milestone report](../07-Final-Report/README.md). Evidence registers include suggested captures; the [published evidence index](../06-Evidence/README.md) lists the files actually included.

CHANGE RECORD: LAB-CHG-003

Environment: Personal Hyper-V lab
Status: Completed — KB5030216 installation and WSUS reporting verified
Target: SRV02 — Broad group
Update: KB5030216 — September 2023 cumulative update for Windows Server 2022

PURPOSE
Deploy KB5030216, previously tested successfully on SRV01, to SRV02 through WSUS. Demonstrate controlled progression from Pilot to Broad with installation, restart, domain trust, and centralized reporting verification.

BEFORE-CHANGE BASELINE
- Operating system: Windows Server 2022 Datacenter Evaluation
- Version: 21H2
- OS build: 20348.587
- Computer: srv02.ad.northstar.test
- IP address: 192.168.77.102
- Active Directory location: Lab-Servers → Broad
- Applied patch policy: WSUS-Broad
- WSUS group membership: All Computers, Broad
- Updates needed: 77
- Updates installed/not applicable: 14
- Updates with errors: 0
- Updates with unknown status: 0

POLICY AND CONNECTIVITY
- Update and reporting server:
  http://wsus01.ad.northstar.test:8530
- Client-side targeting enabled.
- Target group: Broad
- UseWUServer: 1
- AUOptions: 3 — Auto download and notify for install
- Connection to WSUS01 on TCP port 8530: Successful
- WSUS registration and initial status reporting: Verified

PREPARATION
Earlier scans against Microsoft’s public update servers had populated a pending update queue before the Broad policy applied.

Preserved baseline screenshots and exported the Windows Update log. Renamed the local SoftwareDistribution folder to SoftwareDistribution.before-broad and restarted the update services.

An explicit managed-server scan subsequently succeeded with result code 2 and returned no offered updates before Broad approval. The Settings interface continued displaying the earlier queue, so the controlled deployment used the Windows Update Agent search results.

APPROVAL
- KB5030216 approved for installation in Broad.
- Pilot retained its existing approval.
- All Computers remained Not Approved.
- No installation deadline configured.
- Operator-recorded approval time: 7:40
- Exact approval date and timezone: Not confirmed
- Formal approval workflow: Self-managed personal lab

IMPLEMENTATION
1. Ran a managed-server scan after Broad approval.
2. Confirmed the returned update title identified KB5030216.
3. Downloaded the selected update using the Windows Update Agent through PowerShell.
4. Download result code: 2 — Succeeded.
5. Installed that selected update using the Windows Update Agent.
6. Installation result code: 2 — Succeeded.
7. RebootRequired: True.
8. Restarted SRV02.
9. Verified local installation and domain trust.
10. Queried WSUS and matched SRV02’s computer ID to its reported update state.

DEPLOYMENT METHOD
WSUS approval and managed-server discovery, followed by Windows Update Agent download and installation initiated through PowerShell.

The standalone Microsoft Update Catalog installer was not used for KB5030216 on SRV02.

VALIDATION RESULTS
- Get-HotFix lists KB5030216: Passed
- OS build increased from 20348.587 to 20348.1970: Passed
- Restart and post-restart sign-in: Passed
- Test-ComputerSecureChannel returned True: Passed
- WSUS reports KB5030216 as Installed for SRV02: Passed

WSUS identity used for verification:
- Computer: srv02.ad.northstar.test
- Computer ID: a893eb2e-619b-4fcf-bbc2-94a3567e2625
- KB5030216 state: Installed

Post-installation servicing-stack version:
Not separately recorded for SRV02.

TIMING
- Exact installation start time: Not recorded
- Exact installation completion time: Not recorded
- Exact restart completion time: Not recorded
- Get-HotFix displayed InstalledOn: October 16, 2026

The midnight value shown by Get-HotFix does not establish the exact installation time. Date and time comparisons must account for the lab machines’ timezone settings.

IMPACT
SRV02 required a restart and was temporarily unavailable. No application workload was configured. Validation covered the operating system, domain trust, sign-in, and WSUS reporting.

RECOVERY
Recovery was not required.

Retain SoftwareDistribution.before-broad while its contents remain useful for investigation. If the disposable member server becomes unrecoverable, rebuild it from the installation ISO and rejoin the domain.

EVIDENCE
- Broad Group Policy application and connectivity screenshots
- SRV02-Broad-Registry-Verified.png
- SRV02-Broad-Before-Patching.png
- Before-change OS build screenshot
- Exported SRV02 Windows Update log
- Cache preparation command screenshot
- Managed-server scan showing no offers before approval
- SRV02-KB5030216-Offered.png
- Download result code 2 screenshot
- SRV02-KB5030216-Installed-RestartRequired.png
- SRV02-KB5030216-PostRestart-Verified.png
- SRV02-KB5030216-WSUS-Verified.png

OUTCOME
KB5030216 was successfully deployed through WSUS management to the Broad server after successful Pilot testing. SRV02 reached OS build 20348.1970, retained a functioning domain trust relationship, and reported the update as Installed to WSUS.

All validation requirements for this change are complete.

SEPARATE FOLLOW-UP: OCTOBER UPDATE
A subsequent WSUS query for KB5126655 showed:
- SRV01: Installed
- SRV02: NotApplicable

SRV02’s October applicability issue remains unresolved. October deployment requires a separate change record and investigation. LAB-CHG-003 is complete for KB5030216 only.

COMPLIANCE LIMITATION
This personal lab record demonstrates staged patch deployment and verification. It does not establish current overall patch compliance, vulnerability remediation completeness, or formal NIST SP 800-53 compliance.