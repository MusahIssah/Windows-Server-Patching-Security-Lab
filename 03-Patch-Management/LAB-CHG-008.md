# LAB-CHG-008 — DC01 Infrastructure Enrollment and Patching

> Publication note: This record retains observations made during the change. Later outcomes are summarized in the [milestone report](../07-Final-Report/README.md). Evidence registers include suggested captures; the [published evidence index](../06-Evidence/README.md) lists the files actually included.

**Status:** Completed for selected updates  
**Follow-up:** Startup DNS-resolution delay remains documented; automatic time recovery verified  
**Environment:** ad.northstar.test  
**Target:** DC01 — 192.168.77.10  
**Roles:** Active Directory Domain Services, DNS, PDC Emulator  
**WSUS group:** Infrastructure  
**Follow-up verification date:** October 20, 2026

## 1. Purpose and Scope

Enroll DC01 in managed WSUS deployment and install the selected October operating-system and .NET updates. Validate domain-controller health after restarts, investigate installation failures, and assess time synchronization.

DC01 was the deployment target. WSUS01 provided update services.

## 2. Initial Baseline

- Windows Server 2022 Datacenter Evaluation, version 21H2.
- Operating-system build: 20348.587.
- Windows Update restart required: False.

| Initial WSUS metric | Count |
|---|---:|
| Updates needed | 77 |
| Installed/not applicable | 14 |
| Updates with errors | 0 |
| Updates with no status | 0 |

The initial Advertising test failed because DC01 was not advertising as a time server. The selected SYSVOL and Services tests reported no failures.

## 3. Time-Service Configuration

Confirmed that DC01 holds the PDC Emulator role.

Initial inspection showed Windows Time running, but reporting an unsynchronized state with the Hyper-V time provider selected.

Configured:

- External peer: time.windows.com,0x8.
- Synchronization type: NTP.
- Reliable time-server setting enabled.
- Hyper-V Time synchronization integration disabled for DC01.

Manual resynchronization subsequently confirmed the external source with Leap Indicator 0. The selected Advertising, SYSVOL and Services tests then reported no failures.

During subsequent patching restarts, DC01 initially displayed Local CMOS Clock. Manual resynchronization restored the external source. Automatic recovery had not yet been observed during those checks.

### October 20 Startup Investigation

DC01 started at 7:50:37 AM, as displayed in the guest. At approximately six minutes after startup, it still reported Local CMOS Clock despite retaining its external NTP configuration.

Time-Service Event 134 recorded:

- DNS resolution failure for time.windows.com.
- Error 0x80072AF9 — No such host is known.
- An automatic retry scheduled after 15 minutes.

The configured peer was Pending.

Subsequent checks confirmed:

- time.windows.com resolved successfully.
- DC01’s IPv4 DNS server was 192.168.77.10.
- Windows Time synchronized automatically without a manual resync.
- Successful synchronization was recorded at 8:13:32 AM.
- Source: time.windows.com,0x8.
- Leap Indicator: 0.
- Peer state: Active.

This establishes automatic recovery for the observed startup. The earlier assumption that manual resynchronization was required is superseded by this evidence.

The immediate startup failure was DNS resolution. The underlying reason DNS was initially unavailable was not established. No further configuration changes were made during this follow-up.

## 4. WSUS Enrollment

Linked the existing WSUS-Infrastructure GPO to the Domain Controllers OU. DC01 remained in its existing OU.

Applied computer policy and verified:

| Setting | Value |
|---|---|
| WUServer | http://wsus01.ad.northstar.test:8530 |
| WUStatusServer | http://wsus01.ad.northstar.test:8530 |
| TargetGroupEnabled | 1 |
| TargetGroup | Infrastructure |
| UseWUServer | 1 |
| AUOptions | 3 — Auto download and notify for install |

DC01 registered successfully in Infrastructure.

## 5. Deployment and Troubleshooting

### October .NET — KB5128928

The managed scan offered KB5128928.

- Download ResultCode: 2 — Succeeded.
- Installation ResultCode: 2 — Succeeded.
- Restart required: True.
- Restart completed.
- Local component KB5127687 confirmed installed.
- Update history confirmed successful installation with HResult 0.

### Intermediate OS — KB5030216

October OS update KB5126655 initially reported NotApplicable and was not offered by managed scans.

Installed KB5030216 through WSUS and restarted.

Verified:

- Operating-system build: 20348.1970.
- KB5030216 hotfix entry present.
- Selected domain-controller tests reported no failures.
- Windows Update restart required: False.

October’s OS update remained unoffered.

### Failed Standalone October Attempt

Ran the Microsoft Update Catalog package for KB5126655 and restarted.

WUSA initially reported success and a restart requirement. However, post-restart servicing failed with 0x80070490, and DC01 remained on build 20348.1970.

CBS and SetupAPI logs established:

- Newer tape.inf processing was recorded before restart.
- Startup servicing could not locate the newer tape-driver update in its Driver Store transaction.
- Driver processing failed with ERROR_NOT_FOUND.
- Windows servicing rolled back the update attempt.

Additional investigation found:

- DISM ScanHealth detected no component-store corruption.
- Old and new tape.inf files existed in WinSxS and Driver Store folders.
- Matching versions had identical SHA-256 hashes.
- File presence and matching hashes did not establish complete driver registration.
- A targeted PnPUtil import was rejected for missing signature information.
- PnPUtil reported zero packages added.

No signature-enforcement bypass, manual driver-file deletion, or servicing-registry repair was performed.

The precise underlying cause of the driver transaction failure was not established. The diagnostic archive was preserved.

### September OS — KB5122882

Approved KB5122882 for Infrastructure with no deadline.

The managed scan offered the update. Download and installation returned ResultCode 2, followed by a required restart.

Post-restart verification confirmed:

- Operating-system build: 20348.5622.
- KB5122882 hotfix entry present.
- Selected domain-controller tests reported no failures.
- Windows Update restart required: False.

### Successful October OS Retry — KB5126655

After September installation, a managed scan offered KB5126655.

Downloaded and installed the update through WSUS:

- Download ResultCode: 2.
- IsDownloaded: True.
- Installation ResultCode: 2.
- Restart required: True.

The PowerShell window was observed in text-selection mode during the installation wait. After exiting selection mode, the installation result became available. Selection mode was not established as the sole cause of the elapsed installation time.

Restarted DC01 and confirmed that October finalized successfully.

## 6. Final Validation

| Check | Result |
|---|---|
| Operating-system build | 20348.5761 |
| KB5126655 hotfix entry | Present |
| Latest KB5126655 history | ResultCode 2, HResult 0 |
| KB5122882 history | ResultCode 2, HResult 0 |
| KB5128928 history | ResultCode 2, HResult 0 |
| Selected Advertising, SYSVOL and Services tests | No failures reported |
| Windows Update restart required | False |
| External time synchronization | Verified |
| Automatic time recovery after startup | Verified on October 20 |

Update history retains the earlier failed standalone KB5126655 attempt. The later successful entry and post-restart build distinguish the completed deployment from that failure.

## 7. Final WSUS Counts

| Metric | Count |
|---|---:|
| Updates needed | 0 |
| Installed/not applicable | 91 |
| Updates with errors | 0 |
| Updates with no status | 0 |
| Installed/not applicable percentage | 100% |

The displayed 100% applies to the current 91-update WSUS reporting scope. It does not establish overall security compliance.

## 8. Evidence

Retain the baseline, policy, scan, download, installation, restart-validation and troubleshooting evidence in 06-Evidence.

Key final evidence:

- DC01-KB5126655-WSUS-Post-Restart-Verified.png
- DC01-Update-History-Final.png
- DC01-Infrastructure-Final-Zero-Needed.png
- DC01-Update-Diagnostics.zip

Time-service investigation evidence:

- DC01-Time-Startup-Baseline-01.png
- DC01-Time-Startup-Baseline-02.png
- DC01-Time-Startup-DNS-Failure.png
- DC01-Time-Automatic-Recovery-Verified.png

Preserve earlier screenshots showing Local CMOS Clock and manual resynchronization as historical evidence.

## 9. Rollback and Recovery Record

Windows servicing rolled back the failed standalone October attempt. No manual OS rollback was performed.

The PnPUtil staging attempt added zero packages. No manual driver-store or registry repair was performed.

Backup restoration was not tested during this change.

## 10. Outcome and Remaining Observations

The selected October OS and .NET updates are installed and verified on DC01. Final October OS delivery succeeded through WSUS after September KB5122882 was installed.

This observed sequence does not establish September as a universal prerequisite or prove the underlying cause of the earlier installation failure.

Automatic external time synchronization recovered after a startup DNS-resolution failure on October 20. No manual resync was required during that verification.

Remaining observations:

- Monitor future startups for recurrence and duration of the DNS/time synchronization delay.
- The underlying cause of the initial startup DNS failure remains unconfirmed.
- Preserve the October driver-processing failure evidence; its precise cause remains unresolved.
- Continue tracking the earlier October OS delivery issues on SRV01 and SRV02 separately.

**Final outcome: DC01 patch deployment completed, final WSUS counts clear, and automatic time recovery demonstrated.**