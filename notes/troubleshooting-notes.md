# Troubleshooting Notes — Lab 51 Suspicious Print Activity

## 1. PrintService Log Not Showing Expected Events

### Problem

The PrintService Operational log may not contain the expected print-related events.

### Possible Causes

- Operational logging was disabled.
- The log was enabled only after the print operation.
- The printer did not generate the expected telemetry.
- The Windows configuration differs from the expected configuration.
- Events were removed or overwritten.

### Check

Run:

`Get-WinEvent -ListLog *PrintService* | Select-Object LogName, IsEnabled, RecordCount`

### Resolution

If the Operational log exists but is disabled, enable it before repeating the controlled print activity:

`wevtutil set-log "Microsoft-Windows-PrintService/Operational" /enabled:true`

Then verify:

`Get-WinEvent -ListLog "Microsoft-Windows-PrintService/Operational" | Select-Object LogName, IsEnabled, RecordCount`

### DFIR Note

If the log was disabled during the original activity, do not claim that the absence of events proves that no printing occurred.

---

## 2. Print Spooler Service Not Running

### Problem

Printing may fail if the Print Spooler service is stopped.

### Check

`Get-Service Spooler`

### Resolution

For a controlled lab environment:

`Start-Service Spooler`

Then verify:

`Get-Service Spooler`

### DFIR Note

Record the original service state before making changes.

---

## 3. No Printer Available

### Problem

`Get-Printer` does not show an available printer.

### Check

`Get-Printer`

### Possible Causes

- No printer is installed.
- A virtual printer is unavailable.
- The printer configuration is incomplete.
- The Print Spooler service is not running.

### Resolution

For a controlled lab, Microsoft Print to PDF can be used when available.

---

## 4. Sysmon Event ID 1 Not Found

### Problem

No Sysmon Event ID 1 events are returned.

### Check

`Get-WinEvent -FilterHashtable @{LogName="Microsoft-Windows-Sysmon/Operational"; Id=1} -MaxEvents 20`

### Possible Causes

- Sysmon is not installed.
- The Sysmon service is not running.
- The Sysmon configuration is not generating the expected events.
- The event log was cleared.

### DFIR Note

Do not modify the Sysmon configuration simply to manufacture evidence. First determine whether the event was genuinely unavailable.

---

## 5. Sysmon Event ID 3 Not Found

### Problem

No Event ID 3 network events are available.

### Possible Causes

- Network monitoring is not enabled by the Sysmon configuration.
- No qualifying network connection occurred.
- The activity used a local or virtual printer.
- Events were overwritten.

### Check

`Get-WinEvent -FilterHashtable @{LogName="Microsoft-Windows-Sysmon/Operational"; Id=3} -MaxEvents 20`

### DFIR Note

No Event ID 3 does not prove that no network communication occurred.

---

## 6. Windows Security Event ID 4688 Not Found

### Problem

Event ID 4688 cannot be located.

### Check

`Get-WinEvent -FilterHashtable @{LogName="Security"; Id=4688} -MaxEvents 20`

### Possible Causes

- Process creation auditing is not enabled.
- The Security log does not contain the required timeframe.
- Events have been overwritten.
- Insufficient permissions were used to query the Security log.

### DFIR Note

Event ID 4688 is useful for process correlation but should not be treated as a dedicated print event.

---

## 7. Permission Error While Reading Security Logs

### Problem

PowerShell returns an access or permission-related error when querying the Security log.

### Resolution

Open PowerShell using:

`Run as Administrator`

Then repeat the query.

---

## 8. Document Name Does Not Appear in Print Events

### Problem

The document name cannot be found in the available PrintService events.

### Possible Causes

- The event does not contain the document name.
- The wrong PrintService channel was queried.
- Logging was not enabled before the activity.
- The print workflow did not generate the expected event.

### Resolution

Search the PrintService log by timestamp rather than relying only on the document name.

Example:

`Get-WinEvent -LogName "Microsoft-Windows-PrintService/Operational" -MaxEvents 100 | Select-Object TimeCreated, Id, Message`

### DFIR Note

Absence of the document name is a telemetry limitation, not proof that the document was not printed.

---

## 9. Network Event Appears During Printing

### Problem

Sysmon Event ID 3 shows a network connection around the print timeframe.

### Interpretation

Do not immediately conclude that document contents were transmitted.

First determine:

- Destination IP
- Destination port
- Protocol
- Process
- Timestamp
- Whether the destination corresponds to the printer
- Whether the connection is expected

### DFIR Principle

`Network connection ≠ Data exfiltration`

Additional evidence is required to establish data transfer.

---

## 10. Multiple Process Events Appear

### Problem

Several Event ID 1 and 4688 events occur around the print operation.

### Resolution

Build a timeline.

Compare:

- Process creation time
- Print activity time
- Parent process
- Process image
- Command line
- Network activity

Do not assume that every process near the print timestamp is related to printing.

---

## 11. Print Activity Cannot Be Directly Proven

### Problem

Available telemetry shows process and network activity but does not contain a definitive print event.

### Resolution

Document exactly what was observed.

For example:

`Sysmon Event ID 1 was observed.`

`Sysmon Event ID 3 was observed.`

`Security Event ID 4688 was observed.`

Then state that these events provide supporting endpoint context rather than direct proof of a print job.

### DFIR Principle

Evidence should be reported according to what it actually proves.

---

## Final Troubleshooting Lesson

The main troubleshooting lesson from this lab is that Windows telemetry is highly dependent on configuration. A SOC analyst should verify which logs and events are actually available before building conclusions around them.

Missing telemetry should be documented as a limitation rather than silently replaced with assumptions.
