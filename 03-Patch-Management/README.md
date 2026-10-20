# Patch-management change register

Records describe the state at the time of each change. Later records supersede earlier follow-up work; current cross-server results are in the [milestone report](../07-Final-Report/README.md).

| Record | Target and scope | Outcome |
|---|---|---|
| [LAB-CHG-001](LAB-CHG-001.md) | SRV01 intermediate KB5030216 | Completed; build 20348.1970 and SSU 20348.1960 verified |
| [LAB-CHG-002](LAB-CHG-002.md) | SRV01 October OS KB5126655 | Standalone installation verified; original WSUS no-offer cause open |
| [LAB-CHG-003](LAB-CHG-003.md) | SRV02 intermediate KB5030216 | Completed through WSUS after Pilot validation |
| [LAB-CHG-004](LAB-CHG-004.md) | SRV02 October OS KB5126655 | Standalone installation verified; original WSUS no-offer cause open |
| [LAB-CHG-005](LAB-CHG-005.md) | Pilot/Broad October .NET KB5128928 | Completed through WSUS; residual counts later cleared |
| [LAB-CHG-006](../05-Monitoring/LAB-CHG-006.md) | Pilot/Broad firewall logging | Completed; local Domain log generation verified |
| [LAB-CHG-007](LAB-CHG-007.md) | WSUS01 enrollment and October updates | Completed through WSUS; final zero needed/errors |
| [LAB-CHG-008](LAB-CHG-008.md) | DC01 enrollment, time prechecks, staged patching | Completed; rollback retained in history, later WSUS success |
| [LAB-CHG-009](../05-Monitoring/LAB-CHG-009.md) | Three member servers' time source | Completed; automatic domain selection after restarts |

## How deployment was verified

Approval scope and managed discovery were recorded before downloading and installing a selected result set. Installation success was followed by restart, local build/hotfix or component/history checks, member trust or selected DC health tests, and WSUS reporting review.

The final summaries combine Installed and NotApplicable. A decrease in needed counts does not equal the same number of separately installed packages. Empty offers and green dashboards are interpreted within the stated product, classification, approval, and reporting scope.

## Publication notes

- LAB-CHG-001's original TXT stopped at approval. Its publication summary uses saved verification evidence and LAB-CHG-002 to record the subsequently completed intermediate update.
- LAB-CHG-005's 21/70 table is labeled as an intermediate result; its later zero-needed follow-up remains documented separately.
- LAB-CHG-002/003/004/007 include follow-ups that were outstanding at that point in the sequence. Read LAB-CHG-005/008 and the milestone report for subsequent completion.
- [Patch monitoring baseline](../05-Monitoring/Patch-Monitoring-Baseline.md) is an explicitly historical snapshot.
- No new VM commands were executed to create this publication.
