# LAB-CHG-007 — WSUS01 Infrastructure Enrollment and October Updates

> Publication note: This record retains observations made during the change. Later outcomes are summarized in the [milestone report](../07-Final-Report/README.md). Evidence registers include suggested captures; the [published evidence index](../06-Evidence/README.md) lists the files actually included. DC01 work described as pending here was subsequently completed in [LAB-CHG-008](LAB-CHG-008.md).

Status: Completed
Environment: ad.northstar.test
Target: WSUS01 — 192.168.77.20
Deployment group: Infrastructure
Implementation: October 16, 2026
Final reporting verification: October 19, 2026

## 1. Purpose and Scope

Enroll WSUS01 as a managed WSUS client and deploy:

- KB5126655 — October 2026 cumulative operating-system update.
- KB5128928 — October 2026 cumulative .NET Framework update.

DC01, SRV01 and SRV02 were outside this change’s deployment scope.

## 2. Initial Assessment

WSUS01 initially reported build 20348.587, although its hotfix inventory included newer updates.

Package inspection identified pending servicing operations, including:

- OS package 20348.5622: InstallPending.
- Previous OS package 20348.587: UninstallPending.
- Two .NET packages: InstallPending.

A normal restart completed the pending OS transition. Subsequent inspection confirmed build 20348.5622, with the newer OS package Installed and the previous OS package Superseded.

One previously pending .NET package was absent from the subsequent listing; its individual outcome was not established by that check.

Further validation confirmed:

- No packages returned by the pending-state query.
- Windows Update RebootRequired: False.
- WSUS, IIS and WID services running.
- Domain secure channel: True.
- WSUS console accessible.

## 3. Infrastructure Enrollment

Initial policy inspection showed AUOptions 3, but no configured WUServer, WUStatusServer or UseWUServer values.

Created the Infrastructure computer group in WSUS.

Created WSUS-Infrastructure Group Policy and linked it to the Management OU containing WSUS01.

Configured:

| Setting | Value |
|---|---|
| Update service | http://wsus01.ad.northstar.test:8530 |
| Statistics server | http://wsus01.ad.northstar.test:8530 |
| Client-side target group | Infrastructure |
| TargetGroupEnabled | 1 |
| UseWUServer | 1 |
| AUOptions | 3 — Auto download and notify for install |

Applied computer policy and verified the registry values.

A managed scan succeeded, and WSUS01 registered in Infrastructure.

## 4. Before-Deployment Counts

| Metric | Count |
|---|---:|
| Updates needed | 23 |
| Installed/not applicable | 68 |
| Updates with errors | 0 |
| Updates with no status | 0 |

Pre-deployment operating-system build: 20348.5622.

## 5. Deployment

Approved KB5126655 and KB5128928 for Infrastructure, with no deadlines.

A managed scan returned exactly the two selected updates.

Download validation:

- ResultCode: 2 — Succeeded.
- IsDownloaded: True for both updates.

Installation validation:

- ResultCode: 2 — Succeeded.
- RebootRequired: True.

Restarted WSUS01 normally while DC01 remained running.

Both selected updates were offered, downloaded and installed through WSUS.

## 6. Post-Restart Validation

| Check | Result |
|---|---|
| Operating-system build | 20348.5761 |
| WsusService | Running |
| W3SVC | Running |
| WID database service | Running |
| Domain secure channel | True |
| Windows Update RebootRequired | False |
| KB5126655 installation history | Operation 1, ResultCode 2, HResult 0 |
| KB5128928 installation history | Operation 1, ResultCode 2, HResult 0 |

## 7. Reporting Progression

WSUS initially displayed 1 update with errors, 22 needed, and 68 installed/not applicable despite successful local installation history.

A later view showed 0 errors, 21 needed, and 70 installed/not applicable.

On October 19, a fresh managed scan succeeded and offered no updates. After allowing time for reporting and refreshing the console, WSUS01 showed:

| Metric | Final count |
|---|---:|
| Updates needed | 0 |
| Installed/not applicable | 91 |
| Updates with errors | 0 |
| Updates with no status | 0 |
| Installed/not applicable percentage | 100% |

The earlier error and residual needed counts cleared without additional installations or approval changes during the final reporting review. The specific update behind the transient reported error and its precise cause were not established.

The displayed 100% applies to the current 91-update WSUS reporting scope.

## 8. Evidence

Retain the following screenshots in 06-Evidence:

- WSUS01-Packages-Pending-Restart.png
- WSUS01-Post-Restart-Build-and-Packages.png
- WSUS01-Post-Restart-Console.png
- WSUS01-Post-Restart-Services-and-Trust.png
- WSUS01-No-Pending-Packages.png
- WSUS01-No-Update-Restart-Required.png
- WSUS01-Infrastructure-Policy-Applied.png
- WSUS01-Infrastructure-Initial-Scan.png
- WSUS01-Infrastructure-Before-Deployment.png
- WSUS01-October-OS-Needed.png
- WSUS01-October-NET-Needed.png
- WSUS01-October-Updates-Offered.png
- WSUS01-October-Updates-Downloaded.png
- WSUS01-October-Updates-Installed-Restart-Required.png
- WSUS01-October-Post-Restart-Validation.png
- WSUS01-October-Update-History.png
- WSUS01-Post-Deployment-Reported-Error.png
- WSUS01-Reported-Error-Cleared.png
- WSUS01-Post-Deployment-Managed-Scan.png
- WSUS01-Infrastructure-Final-Zero-Needed.png

## 9. Rollback Record

No rollback was required or performed. Update removal and recovery procedures were not tested during this change.

## 10. Outcome and Remaining Work

WSUS01 is enrolled in Infrastructure and has completed installation of the selected October OS and .NET updates. Post-restart operational checks passed, and final WSUS reporting shows zero needed updates and zero errors.

DC01 patching remains separate and outstanding.

Successful WSUS delivery of the October OS update to WSUS01 does not establish the cause or resolution of the earlier delivery issue on SRV01 and SRV02.