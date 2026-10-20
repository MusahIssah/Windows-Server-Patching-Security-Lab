# Local monitoring baseline

| Record | Demonstrated result |
|---|---|
| [LAB-CHG-006 — Firewall logging](LAB-CHG-006.md) | Group Policy applied to Pilot/Broad; actual Domain-profile log entries verified |
| [LAB-CHG-009 — Domain time](LAB-CHG-009.md) | Three member servers selected DC01 automatically after service/computer restarts |
| [Historical patch snapshot](Patch-Monitoring-Baseline.md) | Earlier two-server 22/69 reporting baseline; superseded by final results |

DC01's startup DNS-resolution failure and automatic NTP recovery are documented in [LAB-CHG-008](../03-Patch-Management/LAB-CHG-008.md).

Local log production is demonstrated. Central collection, alerts, retention design, and a recurring review process remain planned. Private/Public firewall settings were inspected but not exercised by changing network profiles. SRV02's saved runtime excerpt shows ALLOW entries; a controlled DROP test was not performed there.
