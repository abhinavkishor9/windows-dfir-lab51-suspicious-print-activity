# Investigation Notes — Lab 51 Suspicious Print Activity

## Investigation Overview

This investigation examined a controlled Windows printing activity to determine what endpoint evidence could be correlated with the operation. The investigation did not assume that printing was malicious. Instead, process creation, network activity, and Windows Security telemetry were reviewed around the timeframe of the controlled print operation.

## Test Activity

A harmless test document was created inside:

`C:\PrintActivityLab`

The document contained simulated confidential information intended only for the investigation.

The document was then used in a controlled printing operation.

## Initial Evidence Collection

The investigation began by recording basic information about the test document.

Collected information included:

- File name
- File path
- File size
- Creation time
- Last modification time
- Last access time
- SHA-256 hash

The purpose was to establish a baseline before investigating the print activity.

## Print Spooler Check

The Windows Print Spooler service was checked before performing the controlled print operation.

Command used:

`Get-Service Spooler`

The service status was reviewed to confirm whether Windows printing functionality was available.

## Printer Enumeration

Available printers were reviewed using:

`Get-Printer`

This was used to identify the printer or virtual printer available on the system.

## PrintService Investigation

The Windows PrintService logging configuration was checked through Event Viewer and PowerShell.

The following area was reviewed:

`Applications and Services Logs > Microsoft > Windows > PrintService`

The investigation checked whether the Operational log was available and enabled.

Print-specific telemetry was not assumed to exist unless it was directly observed.

## Process Creation Evidence

### Sysmon Event ID 1

Sysmon Event ID 1 was observed through Event Viewer.

This event was used to investigate processes created around the print activity.

Relevant fields considered during the investigation included:

- Timestamp
- Process ID
- Image
- Command Line
- Parent Image
- User
- Process GUID

The purpose was to determine which processes were active around the time of the printing operation.

## Network Evidence

### Sysmon Event ID 3

Sysmon Event ID 3 was observed through Event Viewer.

The event was reviewed for network connections occurring around the print activity.

Relevant fields included:

- Timestamp
- Source IP
- Destination IP
- Destination Port
- Protocol
- Process ID
- Process Image

A network connection was treated only as evidence of communication. It was not automatically interpreted as evidence that document contents were transmitted.

## Windows Security Evidence

### Event ID 4688

Windows Security Event ID 4688 was observed through Event Viewer.

The event was used as additional process creation evidence.

Relevant information included:

- New Process Name
- Creator Process Name
- Process ID
- Command Line
- Account information
- Timestamp

The event was correlated with Sysmon process creation telemetry where possible.

## Event Correlation

The main correlation objective was to compare events occurring around the same timeframe.

The investigation followed this model:

`Controlled Print Activity`

`↓`

`Process Creation`

`↓`

`Network Activity`

`↓`

`Windows Security Process Creation`

`↓`

`Timeline Correlation`

The purpose was to determine whether the observed events formed a meaningful sequence.

## Evidence Assessment

The presence of Sysmon Event ID 1, Sysmon Event ID 3, and Security Event ID 4688 demonstrated that useful endpoint telemetry was available during the investigation.

However, these events do not independently prove that a print operation resulted in data theft.

The process events establish execution.

The network events establish communication.

Additional evidence would be required to demonstrate that sensitive document contents were transmitted or intentionally removed.

## Suspicion Assessment

The following questions were considered:

- Was the user expected to perform the print?
- Was the document sensitive?
- Was the printing activity unusually large?
- Was the activity performed at an unusual time?
- Was an unexpected application involved?
- Was an unexpected printer involved?
- Was suspicious network activity observed?
- Did additional suspicious endpoint activity occur afterward?

The answers to these questions should be based on collected evidence rather than assumptions.

## Key Findings

1. A controlled print operation was performed.
2. Sysmon Event ID 1 was available for process investigation.
3. Sysmon Event ID 3 was available for network investigation.
4. Windows Security Event ID 4688 was available for process creation analysis.
5. Process and network telemetry could be correlated with the investigation timeframe.
6. The available events did not independently prove data theft.
7. Additional evidence would be required for a high-confidence malicious classification.

## Investigation Limitations

The investigation was performed using simulated information and a controlled environment.

The available evidence did not constitute a complete print-forensics dataset.

The absence of a specific print event should not be interpreted as proof that printing did not occur.

## Analyst Conclusion

The investigation demonstrated that process and network telemetry can provide useful supporting evidence when investigating suspicious printing behavior. Sysmon Event IDs 1 and 3 and Windows Security Event ID 4688 allowed the activity surrounding the controlled print operation to be reconstructed.

The available evidence was sufficient to investigate the surrounding endpoint activity but was not, by itself, sufficient to classify the print operation as data theft.
