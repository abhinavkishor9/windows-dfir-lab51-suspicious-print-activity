# Windows DFIR Lab 51 — Suspicious Print Activity Investigation

## Overview

This lab investigates a controlled Windows printing scenario from a DFIR and SOC perspective. The objective was to understand how a print-related activity can be investigated by correlating endpoint telemetry rather than treating a single event as proof of malicious behavior. A harmless test document was created and printed in a controlled Windows environment, after which available process, network, and security telemetry was reviewed.

The investigation focused on identifying activity surrounding the print operation and determining whether the observed behavior provided evidence of suspicious information exposure. The available telemetry included Sysmon Event ID 1 for process creation, Sysmon Event ID 3 for network connections, and Windows Security Event ID 4688 for process creation.

## Lab Objectives

- Understand how Windows printing can become relevant to a DFIR investigation.
- Investigate activity surrounding a controlled print operation.
- Identify processes associated with the printing workflow.
- Review Windows Security Event ID 4688.
- Review Sysmon Event ID 1 process creation telemetry.
- Review Sysmon Event ID 3 network connection telemetry.
- Correlate process and network activity with the print timeframe.
- Establish a chronological investigation timeline.
- Identify limitations in the available print-specific telemetry.
- Determine whether the observed activity provides evidence of suspicious behavior.

## Investigation Scenario

A user performs a controlled printing operation involving a test document on a Windows workstation. Because printing can potentially be used to expose or remove sensitive information from an organization, the activity is reviewed from a SOC and DFIR perspective.

The investigation examines the processes running around the print operation, network activity observed during the same timeframe, and Windows Security process creation events. The purpose is to determine whether the surrounding activity is consistent with normal printing behavior or whether additional indicators suggest suspicious activity.

## Lab Environment

- Operating System: Windows
- Investigation Type: Windows DFIR
- Primary Tools:
  - Event Viewer
  - PowerShell
  - Sysmon
  - Windows Security Logs
- Controlled Test Directory:

`C:\PrintActivityLab`

## Evidence Sources

### Windows Security Event ID 4688

Event ID 4688 records process creation activity in the Windows Security log. It was used to identify processes created around the timeframe of the controlled print operation.

### Sysmon Event ID 1

Sysmon Event ID 1 provides process creation telemetry with additional endpoint context. It was reviewed to identify process execution associated with the investigation timeframe.

### Sysmon Event ID 3

Sysmon Event ID 3 records network connection activity. It was reviewed to determine whether network communication occurred around the print activity.

## Investigation Workflow

The investigation followed this general workflow:

1. Created a controlled investigation directory.
2. Created a harmless test document.
3. Collected the document's initial metadata.
4. Calculated the document hash.
5. Checked the Windows Print Spooler service.
6. Reviewed available printers.
7. Checked PrintService logging.
8. Performed a controlled print operation.
9. Recorded the activity timeframe.
10. Reviewed Sysmon Event ID 1.
11. Reviewed Sysmon Event ID 3.
12. Reviewed Windows Security Event ID 4688.
13. Correlated process and network activity with the print timeframe.
14. Built an investigation timeline.
15. Assessed whether the available evidence supported suspicious activity.

## Controlled Document

A harmless test document was used instead of real confidential information.

Example contents:

`LAB 51 - CONFIDENTIAL TEST DOCUMENT`

`Employee: Test User`

`Department: Security Operations`

`Document ID: LAB51-001`

The document was created only for controlled DFIR testing.

## Observed Telemetry

The investigation successfully identified the following telemetry through Event Viewer:

| Source | Event ID | Purpose |
|---|---:|---|
| Sysmon | 1 | Process creation |
| Sysmon | 3 | Network connection |
| Windows Security | 4688 | Process creation |

These events were used to investigate activity surrounding the controlled print operation.

## Key Investigation Findings

- A controlled print activity was performed using a harmless test document.
- Process creation telemetry was available through Sysmon Event ID 1.
- Network connection telemetry was available through Sysmon Event ID 3.
- Windows Security Event ID 4688 provided additional process creation evidence.
- The available telemetry allowed activity around the print timeframe to be investigated.
- No conclusion was made that printing itself constituted data theft.
- Process and network events were treated as supporting evidence rather than direct proof of malicious printing.
- Dedicated print-specific telemetry was not treated as available evidence unless directly observed and documented.

## DFIR Interpretation

The presence of a print-related activity should not automatically be classified as malicious. Printing is a normal Windows operation, and process creation or network events occurring around the same time do not independently prove that information was stolen.

A stronger conclusion requires correlation between the user, document, application, printer, timestamp, and surrounding endpoint activity. Additional evidence such as repeated printing, sensitive document access, unusual user behavior, USB activity, archive creation, or suspicious outbound communication could increase the level of concern.

## Limitations

The investigation was performed in a controlled environment and used a harmless test document. The available telemetry primarily consisted of process creation and network activity rather than a complete dedicated print-forensics dataset.

Event availability also depends on Windows configuration and Sysmon configuration. Therefore, absence of a particular event cannot automatically be interpreted as proof that an activity did not occur.

## Conclusion

The investigation demonstrated how suspicious print activity can be approached through endpoint telemetry and event correlation. Sysmon Event IDs 1 and 3 and Windows Security Event ID 4688 provided useful process and network context around the controlled print operation.

The investigation did not treat the presence of a print operation as proof of data theft. Instead, it established a repeatable DFIR approach for correlating printing-related activity with process execution, network communication, timestamps, and other available evidence.

## Skills Demonstrated

- Windows DFIR
- Event Viewer
- Windows Security Logs
- Sysmon
- Process Creation Analysis
- Network Connection Analysis
- Event Correlation
- Timeline Construction
- Evidence-Based Investigation
- SOC Alert Investigation
- Data Exposure Analysis

## MITRE ATT&CK Relevance

This investigation can provide supporting context for techniques involving:

- T1059 — Command and Scripting Interpreter
- T1059.001 — PowerShell
- T1049 — System Network Connections Discovery
- T1071 — Application Layer Protocol

These techniques should only be mapped when the actual evidence supports them. The presence of a print operation alone does not establish any specific ATT&CK technique.

