# LAB-CHG-009 — Domain Time Synchronization

> Publication note: This record retains observations made during the change. Later outcomes are summarized in the [milestone report](../07-Final-Report/README.md). Evidence registers include suggested captures; the [published evidence index](../06-Evidence/README.md) lists the files actually included.

Status: Completed
Date: October 20, 2026
Environment: ad.northstar.test
Targets: WSUS01, SRV01 and SRV02

## Purpose

Configure the three member servers to select domain time and verify automatic synchronization after service and computer restarts.

## Baseline

All three member servers selected VM IC Time Synchronization Provider and reported Leap Indicator 3 — not synchronized.

WSUS01 used NT5DS and had DC01 as an active peer. Time-Service events confirmed receipt of valid time data from DC01, while another event showed selection of the Hyper-V time provider.

SRV01 also reported DC01 as an active peer. These observations did not establish a large clock offset or a current inability to contact DC01.

## Implementation

On WSUS01, then SRV01, then SRV02, changed:

Registry path:
HKLM\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider

Value:
Enabled — changed from 1 to 0

Restarted Windows Time on each server and allowed automatic synchronization. No manual resync was used during the member-server validation.

The Hyper-V Manager integration checkbox was left unchanged on these three member servers.

## Verification

After the service restart, each target selected DC01.ad.northstar.test, source address 192.168.77.10, and reported Leap Indicator 0.

Restarted the member servers individually while DC01 remained running and synchronized.

| Target | Source after computer restart | Leap Indicator |
|---|---|---|
| SRV01 | DC01.ad.northstar.test | 0 — no warning |
| SRV02 | DC01.ad.northstar.test | 0 — no warning |
| WSUS01 | DC01.ad.northstar.test | 0 — no warning |

Automatic domain-time selection persisted across the tested restarts.

## DC01 Dependency

DC01 obtains time from time.windows.com.

A separate startup investigation recorded DNS-resolution failure for that external source, followed by successful automatic recovery without manual resynchronization.

DC01’s configuration and startup investigation remain documented in LAB-CHG-008. No additional DC01 configuration changes were made under this change.

## Evidence

- WSUS01-Time-Baseline.png
- WSUS01-Time-Provider-Events-01.png
- WSUS01-Time-Provider-Events-02.png
- WSUS01-Domain-Time-Verified.png
- SRV01-Time-Baseline.png
- SRV01-Domain-Time-Peer-Before.png
- SRV02-Time-Baseline.png
- SRV01-Domain-Time-Verified.png
- SRV02-Domain-Time-Verified.png
- SRV01-Domain-Time-After-Restart.png
- SRV02-Domain-Time-After-Restart.png
- WSUS01-Domain-Time-After-Restart.png

## Rollback

No rollback was required or performed.

To restore the previous member-server provider setting, set VMICTimeProvider Enabled to 1 and restart Windows Time. Source selection and synchronization would require verification afterward.

## Outcome and Limitations

Verified hierarchy:

time.windows.com → DC01 → WSUS01, SRV01, SRV02

The three member servers now select domain time automatically after the tested restarts.

This establishes source selection and synchronization status for the observed tests. It does not establish a precision-time guarantee or long-term availability.

Continue monitoring DC01’s startup DNS-resolution delay.