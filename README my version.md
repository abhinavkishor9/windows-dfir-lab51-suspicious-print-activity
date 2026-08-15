# windows-dfir-lab51-suspicious-print-activity
## Overview

Printing is normally legitimate.

A user may print:

Reports
Emails
Invoices
Screenshots
Source code
Configuration information
Customer records
Security documentation

The problem begins when printing is inconsistent with the user's normal activity or involves information that the user should not be printing.

For example:

User accesses sensitive document
          ↓
Document opened
          ↓
Print command issued
          ↓
Windows Print Spooler
          ↓
Print job created
          ↓
Physical/network printer

From a DFIR perspective, the important question is:

Can we correlate the print job with a particular user, document, process, printer, and time?

Imagine an employee normally works with ordinary business documents.

Suddenly:

09:00  User logs in
09:15  Opens confidential document
09:17  Prints 150 pages
09:20  Copies files to USB
09:25  Logs out

No single event necessarily proves data theft.

But when several activities occur together, the overall behavior becomes much more suspicious.

Therefore:

Print Event
     ≠
Data Theft

Instead:

Print Event
     +
Sensitive Document
     +
Unexpected User
     +
Unusual Time
     +
Large/Repetitive Printing
     +
Other Suspicious Activity
     =
Higher Suspicion

Depending on the available Windows print telemetry, we want to identify things such as:

Print job ID
Document name
User/account
Printer name
Number of pages
Print status
Submission time
Completion time
Client computer
Rendering information

The exact fields available can vary depending on Windows version and logging configuration.

## Lab Objectives

- Understand how Windows handles a print request from an application to a configured printer.
- Identify the Windows components involved in a printing workflow.
- Examine process activity occurring around the time of a print operation.
- Analyze Sysmon Event ID 1 to identify relevant process executions.
- Analyze Sysmon Event ID 3 for communication occurring during the investigation period.
- Examine Windows Security Event ID 4688 for additional process-creation evidence.
- Correlate timestamps from different telemetry sources to reconstruct activity.
- Determine whether observed network connections are related to the printing workflow.
- Identify the limitations of relying on process and network telemetry when investigating printing.
- Separate confirmed observations from assumptions about possible data exposure.
- Develop an evidence-based assessment of whether the printing activity warrants further investigation.


## Investigation Scenario

We will simulate a user printing a document containing fake confidential information.

We will deliberately use harmless test information.

For example:

LAB 51 - CONFIDENTIAL TEST DOCUMENT


Employee: Test User
Department: Security Operations
Document ID: LAB51-001


This document contains simulated confidential information
for Windows DFIR investigation purposes only.

Do not use:

Real passwords
Real API keys
Real company documents
Personal information
Actual confidential files

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


## Limitations

The investigation was performed in a controlled environment and used a harmless test document. The available telemetry primarily consisted of process creation and network activity rather than a complete dedicated print-forensics dataset.

Event availability also depends on Windows configuration and Sysmon configuration. Therefore, absence of a particular event cannot automatically be interpreted as proof that an activity did not occur.



## MITRE ATT&CK Relevance

This investigation can provide supporting context for techniques involving:

- T1059 — Command and Scripting Interpreter
- T1059.001 — PowerShell
- T1049 — System Network Connections Discovery
- T1071 — Application Layer Protocol

These techniques should only be mapped when the actual evidence supports them. The presence of a print operation alone does not establish any specific ATT&CK technique.
