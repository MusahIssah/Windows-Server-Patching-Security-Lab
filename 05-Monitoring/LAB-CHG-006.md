# LAB-CHG-006 — Enable Windows Firewall Logging

> Publication note: This record retains observations made during the change. Later outcomes are summarized in the [milestone report](../07-Final-Report/README.md). Evidence registers include suggested captures; the [published evidence index](../06-Evidence/README.md) lists the files actually included.

Status: Completed — configuration applied to 2 of 2 targets
Implementation date: October 16, 2026
Environment: ad.northstar.test
Group Policy: Firewall-Logging-Baseline

## Purpose and Scope

Enable Windows Firewall logging on SRV01 (Pilot) and SRV02 (Broad) to support network investigation and monitoring.

DC01 was used to manage the policy. DC01 and WSUS01 were outside the deployment scope.

## Baseline Finding

Effective firewall settings on both targets showed:

- Domain, Private and Public profiles enabled.
- Default inbound action: Block.
- Default outbound action: Allow.
- Dropped-packet logging: False.
- Successful-connection logging: False.
- Maximum log size: 4,096 KB.

The finding was a monitoring configuration gap. No exploitable vulnerability was established by these checks.

Both targets ran Windows Server 2022 Datacenter Evaluation, version 21H2, build 20348.5761. Defender service, antivirus and real-time protection were enabled.

## Implementation

Created Firewall-Logging-Baseline and linked it to the Pilot OU.

Configured all three firewall profiles with:

- Log dropped packets: Yes.
- Log successful connections: Yes.
- Maximum log size: 20,480 KB.
- Separate log files under %SystemRoot%\System32\LogFiles\Firewall:
  - Domain: pfirewall_Domain.log
  - Private: pfirewall_Private.log
  - Public: pfirewall_Public.log

Applied computer policy on SRV01 and verified the effective settings and log contents.

After Pilot validation, linked the existing GPO to the Broad OU, applied computer policy on SRV02, and repeated verification.

## Validation Results

| Check | SRV01 — Pilot | SRV02 — Broad |
|---|---|---|
| Computer policy update | Successful | Successful |
| Logging enabled on all profiles | Confirmed | Confirmed |
| Separate filenames and 20,480 KB limits | Confirmed | Confirmed |
| Active network category | DomainAuthenticated | DomainAuthenticated |
| Domain log contains traffic records | Confirmed | Confirmed |
| ALLOW entries observed | Yes | Yes |
| DROP entries observed | Yes | Not observed in captured excerpt |

Configuration and active-profile log writing were verified on both targets. Private and Public profile operation was not tested by changing network categories. A controlled dropped-packet test was not performed on SRV02.

## Evidence

Retain the following screenshots in 06-Evidence:

- SRV01-Firewall-Logging-Before.png
- SRV02-Firewall-Logging-Before.png
- Firewall-Logging-GPO-Settings.png
- SRV01-Firewall-Logging-Pilot-Applied.png
- SRV01-Firewall-Logging-Pilot-Verified.png
- SRV02-Firewall-Logging-Broad-Applied.png
- SRV02-Firewall-Logging-Broad-Traffic.png
- SRV02-Network-Profile-Verified.png

## Rollback Record

No rollback was required or performed.

Any future rollback should restore the recorded prior logging settings through managed policy and verify the effective result on each target. Removing the GPO link alone should not be treated as proof of restoration.

## Outcome and Follow-up

The firewall logging configuration gap was addressed on both target servers through a Pilot-then-Broad deployment.

Local log generation is verified. Central collection, retention beyond the configured file limits, alerting, and routine review remain future monitoring work.

This change does not constitute a full vulnerability assessment or establish formal security compliance.