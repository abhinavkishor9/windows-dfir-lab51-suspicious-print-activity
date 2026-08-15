# Timeline — Lab 51 Suspicious Print Activity Investigation

## Timeline Purpose

This timeline records the sequence of actions and evidence reviewed during the controlled suspicious print activity investigation.

All timestamps should be replaced with the actual timestamps observed during the lab.

## Investigation Timeline

| Time | Source | Event | Significance |
|---|---|---|---|
| T0 | File System | Test document created | Establishes the controlled investigation artifact |
| T1 | File System | Document metadata collected | Establishes initial file baseline |
| T2 | PowerShell | SHA-256 hash calculated | Provides evidence identifier for the original document |
| T3 | Service Control | Print Spooler checked | Confirms printing infrastructure state |
| T4 | Printer Configuration | Available printers enumerated | Identifies available print destinations |
| T5 | Event Viewer | PrintService logging reviewed | Determines available print-related telemetry |
| T6 | Print Activity | Controlled document printed | Generates the activity under investigation |
| T7 | Sysmon Event ID 1 | Process creation observed | Provides process execution context |
| T8 | Sysmon Event ID 3 | Network connection observed | Provides network activity context |
| T9 | Security Event ID 4688 | Process creation observed | Provides Windows Security process evidence |
| T10 | File System | Printed output examined | Provides evidence of the controlled print result |
| T11 | PowerShell | Printed artifact hash calculated | Establishes evidence identifier for output |
| T12 | DFIR Analysis | Events correlated | Determines relationships between observations |
| T13 | DFIR Analysis | Investigation timeline completed | Establishes chronological sequence |

## Event Correlation

The investigation followed this general sequence:

`Test Document`

`↓`

`Controlled Print Activity`

`↓`

`Process Creation`

`↓`

`Network Activity`

`↓`

`Windows Security Process Creation`

`↓`

`Evidence Correlation`

`↓`

`Final Assessment`

## Confirmed Telemetry

The following events were observed during the investigation:

| Telemetry Source | Event ID | Observation |
|---|---:|---|
| Sysmon | 1 | Process creation telemetry available |
| Sysmon | 3 | Network connection telemetry available |
| Windows Security | 4688 | Process creation telemetry available |

## Timeline Interpretation

The observed process creation events provide information about processes executing around the investigation timeframe. Sysmon Event ID 3 provides network communication context, while Security Event ID 4688 provides additional process creation evidence.

These events can be correlated with the controlled print activity, but they should not automatically be interpreted as proof that a process performed malicious printing or that document contents were transmitted.

## Evidence Gaps

The investigation should explicitly record any missing information, including:

- Missing or incomplete PrintService events
- Missing document name in print telemetry
- Missing user attribution
- Missing printer attribution
- Missing page-count information
- Missing network destination information
- Missing evidence of actual data transmission

These gaps reduce the confidence of any conclusion about malicious printing.

## Final Timeline Assessment

The investigation successfully established a controlled print activity and identified supporting endpoint telemetry through Sysmon Event IDs 1 and 3 and Windows Security Event ID 4688.

The available evidence supports investigation of the activity surrounding the print operation but does not independently establish data theft or malicious intent.

## Analyst Note

The timeline should always use the actual timestamps from Event Viewer and PowerShell rather than the example T0-T13 labels. The example labels represent the logical sequence of the investigation and are not intended to replace the original evidence timestamps.
