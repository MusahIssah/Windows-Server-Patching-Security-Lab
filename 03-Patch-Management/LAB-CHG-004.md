# LAB-CHG-004

> Publication note: This record retains observations made during the change. Later outcomes are summarized in the [milestone report](../07-Final-Report/README.md). Evidence registers include suggested captures; the [published evidence index](../06-Evidence/README.md) lists the files actually included.

CHANGE RECORD: LAB-CHG-004

Environment: Personal Hyper-V lab
Status: Completed — standalone installation and WSUS reporting verified
Target: SRV02 — Broad group
Update: KB5126655 — October 2026 cumulative update for Windows Server 2022

PURPOSE
Install KB5126655 on SRV02 using the standalone method previously validated on SRV01, and verify the result locally and through WSUS.

BEFORE-CHANGE BASELINE
- Windows Server 2022 Datacenter Evaluation, version 21H2
- OS build: 20348.1970
- KB5030216 installed and verified under LAB-CHG-003
- Domain secure channel: True
- WSUS group: Broad
- WSUS reported KB5126655 as NotApplicable

INVESTIGATION
After the instructed Broad approval, an explicit managed-server scan completed successfully with result code 2 but returned no offered updates.

This reproduced the no-offer symptom previously observed on SRV01. The cause of the October WSUS detection/delivery issue remains unconfirmed.

IMPLEMENTATION
Downloaded KB5126655 from Microsoft Update Catalog and installed the standalone package on SRV02.

Deployment source:
Microsoft Update Catalog — standalone installation.

Successful delivery of this update through WSUS was not demonstrated.

VALIDATION RESULTS
- Get-HotFix lists KB5126655: Passed
- OS build increased from 20348.1970 to 20348.5761: Passed
- Test-ComputerSecureChannel returned True: Passed
- WSUS reports KB5126655 as Installed for SRV02: Passed

WSUS identity:
a893eb2e-619b-4fcf-bbc2-94a3567e2625

REPORTING FOLLOW-UP
WSUS initially retained SRV02’s earlier NotApplicable assessment after local installation.

Performed a managed-server scan and requested upload of queued reporting events using:
wuauclt.exe /reportnow

The last reported-status timestamp advanced from October 16, 2026, 03:44:30 to 04:32:59 in the API output. A subsequent update-state query showed Installed for both SRV01 and SRV02.

The sequence confirms receipt of a newer report; it does not establish that the reporting command alone caused the change.

TIMING
- Exact approval time: Not recorded
- Exact installation start and completion times: Not recorded
- Exact restart time: Not recorded
- Get-HotFix displayed InstalledOn: October 16, 2026

The midnight InstalledOn value does not establish the installation time. API and console timestamps must be interpreted with timezone context.

IMPACT AND RECOVERY
SRV02 was the only target of this change. No application workload was configured, so validation covered the OS, domain trust, and centralized update reporting.

No recovery action was required. The disposable lab member server can be rebuilt and rejoined to the domain if recovery becomes necessary.

EVIDENCE
- SRV02-October-WSUS-No-Offer.png
- SRV02-KB5126655-Verified.png
- Initial WSUS NotApplicable result
- Reporting timestamp before and after the newer upload
- WSUS-October-Both-Servers-Installed.png

OUTCOME
KB5126655 is installed on SRV02 at OS build 20348.5761. Domain trust remains functional, and WSUS reports the update as Installed.

Both managed servers now have verified installation of this selected OS cumulative update.

OPEN ISSUE
October’s original WSUS detection/delivery issue remains unresolved. Standalone remediation and successful reporting do not prove that WSUS delivery is fixed.

COMPLIANCE LIMITATION
This record verifies one selected OS update. It does not establish complete patch compliance, resolution of all vulnerabilities, or formal NIST SP 800-53 compliance.