# Timeline 
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

