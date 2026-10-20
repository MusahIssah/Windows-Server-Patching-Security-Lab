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

## Next phase — Vulnerability assessment

- [ ] Select a scanner and document its version, feed date, and scan scope.
- [ ] Establish credentialed scanning and confirm coverage.
- [ ] Capture baseline findings and prioritize by evidence and impact.
- [ ] Remediate selected findings through recorded changes.
- [ ] Rescan and compare results, retaining unresolved findings.

No scanner-based vulnerability results have been collected for this milestone. Firewall logging is a configuration improvement, not a CVE remediation claim.

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
