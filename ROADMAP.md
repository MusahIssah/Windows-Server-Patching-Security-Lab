# Project roadmap

## v0.1 — Foundation, patching, and local monitoring baseline

- [x] Hyper-V internal network and NAT.
- [x] AD DS/DNS domain and server roles.
- [x] WSUS products, classifications, groups, and client policies.
- [x] Pilot and Broad deployments with local and WSUS validation.
- [x] Infrastructure enrollment and selected updates.
- [x] DC01 rollback investigation and successful later deployment.
- [x] Firewall logging on SRV01 and SRV02.
- [x] Domain time selection and member-server restart verification.
- [x] Document known limitations and open observations.

## Phase 4 — Vulnerability assessment (in progress)

- [x] Record Nessus 10.12.4/build 20038, feed 202609212044, and scan scope.
- [x] Establish and verify credentialed scanning on SRV01 and SRV02.
- [x] Capture baseline findings and validate three selected fixes on Pilot.
- [x] Deploy the tested hardening GPO to Broad and verify resultant policy.
- [x] Capture credentialed rescans with the selected findings no longer detected.
- [x] Assess WSUS01; validate selected remediation, console connection, and new synchronization.
- [x] Establish an appropriate DC01 credential plan and assess DC01.
- [x] Validate WSUS01 through a separate computer-filtered hardening GPO and credentialed rescan.
- [x] Validate two selected DC01 remediations separately and retain the GRC tracking workbook.
- [ ] Test new client update delivery after WSUS01 hardening.
- [ ] Test recovery and representative application compatibility.

See the [Phase 4 report](04-Vulnerability-Remediation/README.md). Evidence is screenshots; raw scan export was unavailable. WSUS01 is documented in [LAB-CHG-011](04-Vulnerability-Remediation/LAB-CHG-011-WSUS01-Hardening.md). [DC01 remediation and the Excel register](04-Vulnerability-Remediation/LAB-CHG-012-013-DC01-Hardening.md) are documented. Selected findings are validated on all four targets; the unchecked validation tasks remain outstanding.

## Later phase — Central monitoring

- [ ] Collect selected security, firewall, WSUS, and time-service events centrally.
- [ ] Define retention and recurring review responsibilities.
- [ ] Test alerts using controlled events and record expected/observed outcomes.
- [ ] Observe additional cold starts and measure DC01 DNS/time recovery.
- [ ] Define checks for report freshness, update failures, and time-source drift.

## Later phase — Configuration Manager / MECM

- [ ] Plan resources, supported prerequisites, and isolated deployment scope.
- [ ] Build and validate the management infrastructure.
- [ ] Demonstrate client onboarding and update deployment.
- [ ] Compare evidence and operational workflow with the WSUS phase.

## Open investigations

| Observation | Current position |
|---|---|
| SRV01/SRV02 October OS update not offered through WSUS | Standalone installation verified; original detection cause unresolved |
| DC01 standalone October rollback in tape-driver processing | Later WSUS deployment succeeded after September; underlying cause unresolved |
| DC01 external NTP hostname unavailable at startup | Event 134 captured; automatic recovery verified once; initial DNS cause unconfirmed |
| Full recovery capability | Update rollback observed, but backup restoration not tested |

Future completion claims require new evidence. Planned phases have no simulated findings or placeholder success results.
