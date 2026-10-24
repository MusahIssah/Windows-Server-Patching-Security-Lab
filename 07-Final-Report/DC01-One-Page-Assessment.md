# DC01 Security Remediation Assessment

Personal lab assessment | Evidence date October 23, 2026 | Prepared by Musah Issah

## Assessment conclusion

Evidence supports closure of two selected DC01 configuration findings following documented changes and authenticated verification scans on October 23, 2026. The result demonstrates a traceable remediation process within this lab assessment, rather than an overall compliance determination.

## Business purpose and scope

DC01 is the Windows Server 2022 domain controller at 192.168.77.10 for ad.northstar.test. It supports authentication and name resolution. In a business environment, compromise or disruption could affect access to systems and business operations; no actual business loss was measured in this lab. The objective was to identify, address, and verify the two selected weaknesses.

## Assessment approach

The initial scan accepted SMB credentials but could not complete credentialed checks because of access limitations. That scan was not treated as a complete baseline. A subsequent privileged scan reported Auth Pass and credentialed checks yes. Baseline findings were recorded, a DC01-filtered Group Policy Object was applied, registry settings were checked, and authenticated rescans were reviewed after restarts.

## Finding and evidence traceability

**DC01-VULN-001 | WinVerifyTrust | Nessus High | CVSS 3.0 8.8.** LAB-CHG-012 set EnableCertPaddingCheck to 1 in both registry locations. EV-003 records the baseline; EV-005 and EV-006 support GPO application and registry verification; EV-008 supports no longer detected status in the authenticated rescan. EV-007 is the operator-reported post-restart health check.

**DC01-VULN-002 | Intel BHI configuration | Nessus Low | CVSS 3.0 6.5.** LAB-CHG-013 set FeatureSettingsOverride to 8388608 and its mask to 3. EV-004 and EV-009 record the baseline; EV-010 and EV-011 support implementation; EV-012 supports final authenticated verification. EV-013 is the operator-reported post-restart health check.

## Evidence quality and limitations

The repository retains 50 screenshots. Evidence IDs identify supporting records, Finding IDs identify the weaknesses, and Change IDs explain the remediation. Post-restart service and Active Directory checks, and removal of the saved privileged scan credential, were operator confirmed without retained screenshots. The operator implemented and verified the changes; no independent approval was recorded. Physical host protection, recovery testing, and full application compatibility were not established. An earlier unverified SMB finding remains a historical discrepancy, not a closed finding.

## Conclusion and next steps

Retain both selected findings as closed within the documented scan scope and preserve their original scores. Review future scan results for recurrence. For a business assessment, obtain an independent review, agree remediation timeframes, and define ongoing monitoring before using this record to support a broader risk or authorization decision.

## Source records

- [Tracking workbook](https://github.com/MusahIssah/Windows-Server-Patching-Security-Lab/blob/main/04-Vulnerability-Remediation/DC01-Vulnerability-Tracker.xlsx)
- [Change record 012 and 013](https://github.com/MusahIssah/Windows-Server-Patching-Security-Lab/blob/main/04-Vulnerability-Remediation/LAB-CHG-012-013-DC01-Hardening.md)
- [Evidence index and screenshots](https://github.com/MusahIssah/Windows-Server-Patching-Security-Lab/tree/main/06-Evidence/Phase-4-Vulnerability-Assessment/DC01)
