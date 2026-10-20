# Build and operating notes

These notes summarize the completed build retrospectively. Exact original installation times and per-VM resource allocations were not recorded here.

## Build sequence

1. Created Hyper-V internal networking and host NAT for `192.168.77.0/24`.
2. Installed DC01, assigned `.10`, and created `ad.northstar.test` with AD DS/DNS.
3. Installed and domain-joined WSUS01 at `.20`; deployed WSUS Services, WID connectivity, IIS dependencies, and `C:\WSUS` content storage.
4. Restricted WSUS synchronization to the selected Server 2022 product, classifications, and English content. A later synchronization populated 91 updates.
5. Created Pilot and Broad OUs/groups and joined SRV01 (`.101`) and SRV02 (`.102`).
6. Applied managed-update policies with client-side targeting and controlled approvals.
7. Patched member servers first, then enrolled and patched the infrastructure servers.
8. Added firewall logging, corrected member time-source selection, and verified results.

## Practical observations

- WsusPool stopped during early work and caused HTTP 503 scan failures. Starting the pool restored access; the reason it stopped was not established.
- The Windows Update Settings view sometimes retained an older public-update queue. Managed Windows Update Agent scans were used to identify the approved update set before installation.
- A scan with no offers does not establish complete patch coverage: unapproved or NotApplicable catalog entries may still exist.
- WSUS reports sometimes lagged local installation. Local history, OS build, update identity, and refreshed reports were checked independently.
- Some commands were accidentally duplicated or skipped during manual entry. Errors were corrected before treating output as validation.
- Console selection mode was observed during an installation wait. Exiting it exposed the result, but it was not proven to explain the entire installation duration.
- `Get-HotFix` did not list the .NET bundle identifier. Windows Update history, WSUS state, and the applicable component KB were used instead.

## Operational order

Start DC01 first and allow DNS/domain services to become available before starting dependent servers. Use normal guest shutdown; shut down member servers, then WSUS01, and DC01 last. Do not interrupt active servicing.

Recovery checks are scoped: member trust checks were used on members; selected `dcdiag` tests were used on DC01. Backup restoration was not demonstrated.

See the [DC01 servicing case study](DC01-Servicing-Investigation.md) and [change register](../03-Patch-Management/README.md).
