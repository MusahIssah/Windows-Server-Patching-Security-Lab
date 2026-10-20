# LAB-CHG-005 — October .NET Update Deployment

> Publication note: This record retains observations made during the change. Later outcomes are summarized in the [milestone report](../07-Final-Report/README.md). Evidence registers include suggested captures; the [published evidence index](../06-Evidence/README.md) lists the files actually included.

**Status:** Completed — selected update installed on 2 of 2 target servers  
**Environment:** NORTHSTAR lab — ad.northstar.test  
**Deployment platform:** WSUS01  
**Target update:** KB5128928 — October 2026 Cumulative Update for .NET Framework 3.5, 4.8 and 4.8.1 for Microsoft server operating system version 21H2 for x64

## 1. Purpose and Scope

Deploy the selected October .NET update through WSUS using a Pilot deployment followed by Broad deployment.

| Target | IP address | Deployment group |
|---|---|---|
| SRV01 | 192.168.77.101 | Pilot |
| SRV02 | 192.168.77.102 | Broad |

Both targets run Windows Server 2022 Datacenter Evaluation. DC01 and WSUS01 are outside this change’s deployment scope.

## 2. Before Deployment

WSUS identified KB5128928 as needed by both target servers. Managed Windows Update scans subsequently identified the update on each target before installation.

The recorded WSUS baseline for each server was:

| Metric | Before deployment |
|---|---:|
| Updates needed | 22 |
| Updates installed/not applicable | 69 |
| Updates with errors | 0 |
| Updates with no status | 0 |
| Installed/not applicable percentage | 76% |

These figures describe the WSUS catalog status for each server, rather than an overall security compliance score.

## 3. Implementation

### Pilot — SRV01

1. Approved KB5128928 for the Pilot group.
2. Performed a managed Windows Update scan and confirmed the selected update.
3. Downloaded the update successfully: ResultCode 2.
4. Installed the update successfully: ResultCode 2.
5. Confirmed that installation required a restart.
6. Restarted SRV01 and performed validation.
7. Confirmed that WSUS reported the update as Installed before proceeding with Broad.

### Broad — SRV02

1. Approved KB5128928 for the Broad group after Pilot validation.
2. Performed a managed Windows Update scan and confirmed the selected update.
3. Downloaded the update successfully: ResultCode 2.
4. Installed the update successfully: ResultCode 2.
5. Confirmed that installation required a restart.
6. Restarted SRV02 and performed validation.
7. Confirmed that WSUS reported the update as Installed.

The selected .NET update was delivered through WSUS on both targets.

## 4. Post-Implementation Validation

| Validation | SRV01 — Pilot | SRV02 — Broad |
|---|---|---|
| Installation ResultCode | 2 — Succeeded | 2 — Succeeded |
| Restart required by installer | True | True |
| Restart performed | Yes | Yes |
| Domain secure channel | True | True |
| Windows Update history for KB5128928 | ResultCode 2, HResult 0 | ResultCode 2, HResult 0 |
| WSUS status for KB5128928 | Installed | Installed |
| Local October component KB5127687 | Installed | Installed |

No application workload was configured for application-specific .NET testing. Validation covered installation, restart, domain trust, update history, and WSUS reporting.

### Component Verification

A lookup for the bundle identifier KB5128928 using Get-HotFix returned no entry on SRV01. Additional verification used Windows Update history, WSUS installation status, and the component packages identified in WSUS metadata.

| Bundle | Component files identified |
|---|---|
| September KB5126149 | KB5126050 — NDP48; KB5126422 — NDP481 |
| October KB5128928 | KB5127687 — NDP48; KB5127693 — NDP481 |

Get-HotFix confirmed October component KB5127687 installed on both SRV01 and SRV02. KB5127693 did not appear in those query results.

### Intermediate Recorded Counts

| Metric | SRV01 | SRV02 |
|---|---:|---:|
| Updates needed | 21 | 21 |
| Updates installed/not applicable | 70 | 70 |
| Updates with errors | 0 | 0 |
| Updates with no status | 0 | 0 |
| Installed/not applicable percentage | 77% | 77% |

For each server, the needed count decreased from 22 to 21, and the installed/not applicable count increased from 69 to 70.

### Residual WSUS Status / Follow-up Items

Following the managed scan and subsequent WSUS refresh, both SRV01 and SRV02 report 0 updates needed, 91 installed/not applicable, 0 errors, and 0 updates with no status.

The previously observed residual needed counts have cleared in the refreshed computer summaries. No additional installations, approvals, or declines were performed during this follow-up review. The precise cause of the earlier reporting discrepancy was not established.

The displayed 100% applies to the current 91-update WSUS reporting scope. It does not establish overall security compliance.

The separate October operating-system update delivery issue tracked under LAB-CHG-002 and LAB-CHG-004 remains unresolved.

## 5. Issues and Observations

- WSUS reporting initially lagged behind local installation results. Later queries confirmed Installed on both targets.
- The bundle identifier alone was insufficient for local Get-HotFix verification. Component KB5127687 provided additional local evidence.
- The earlier October operating-system update delivery issue tracked under LAB-CHG-002 and LAB-CHG-004 remains separate and unresolved. This successful .NET deployment does not establish that the earlier issue was fixed.

## 6. Rollback Record

No rollback was required or performed. Rollback capability and package removal were not tested as part of this change.

## 7. Evidence Register

Retain screenshots showing:

1. KB5128928 needed before deployment.
2. Pilot download and installation results.
3. Pilot update history and WSUS Installed status.
4. Broad download and installation results.
5. Broad update history and successful secure-channel test.
6. WSUS reporting KB5128928 Installed for both computer targets.
7. Final WSUS counts for each server.
8. September and October component package filenames.
9. Local KB5127687 installation on each server.
10. Residual September KB5126149 status on both targets.

Suggested evidence filenames:

- NET-KB5128928-Before-Deployment.png
- NET-September-October-Component-Packages.png
- SRV01-NET-October-Component-Verified.png
- SRV02-NET-October-Component-Verified.png
- NET-KB5126149-Residual-Status.png

## 8. Final Outcome

KB5128928 was successfully deployed through WSUS to SRV01 in Pilot and then SRV02 in Broad. Both targets completed restart and domain-trust validation, and both report Installed for the selected update in WSUS.

**Deployment result: 2 of 2 targets successful.**

**Change status: Completed. Later refreshed summaries show 0 needed and 91 installed/not applicable on both targets. The separate OS detection issue remains unresolved.**