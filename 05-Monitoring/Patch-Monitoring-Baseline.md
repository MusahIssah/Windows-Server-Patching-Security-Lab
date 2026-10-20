# Patch-Monitoring-Baseline

> Publication note: This record retains observations made during the change. Later outcomes are summarized in the [milestone report](../07-Final-Report/README.md). Evidence registers include suggested captures; the [published evidence index](../06-Evidence/README.md) lists the files actually included. The 22-needed/69-installed counts are historical baseline observations, superseded by the later final summaries.

PATCH MONITORING BASELINE

Environment: Personal Hyper-V lab
Scope: SRV01 and SRV02; selected WSUS products and classifications only

Verified OS update:
KB5126655 installed on both servers.
Verified OS build: 20348.5761 on both servers.

Reported update counts:
SRV01: 22 needed; 69 installed/not applicable; 0 errors; 0 unknown.
SRV02: 22 needed; 69 installed/not applicable; 0 errors; 0 unknown.

Last status reports shown in the WSUS console:
SRV01: October 15, 2026, 6:11 PM
SRV02: October 15, 2026, 9:32 PM
These are the displayed report times, not the screenshot capture time. Preserve the console’s timezone context.

Progress:
Each server’s needed count decreased from 77 to 22.
This does not represent 55 separately installed updates.

Limitations:
Installed and Not Applicable are combined in the summary.
Needed updates may include superseded entries.
Counts do not cover all software or all vulnerabilities.
Report freshness must be checked during each monitoring review.
DC01 and WSUS01 are outside this two-server assessment.

Open issue:
October’s OS update required standalone installation on both targets. WSUS subsequently reported Installed, but the original detection/delivery issue remains unresolved.

Next actions:
Identify the 22 remaining needed updates.
Review applicability, supersedence, and approvals.
Test selected remediation on Pilot before Broad.