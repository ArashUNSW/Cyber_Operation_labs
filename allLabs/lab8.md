# Lab 08 - Incident Triage and Response Using Elastic

## Estimated Time

90–120 minutes

---

## Lab Purpose

In this lab, you will perform a beginner-friendly incident triage workflow using Elastic and Kibana.

You will start from a simulated alert or saved detection query, identify the affected host and user, review process activity, collect related evidence, create a short timeline, classify the activity, and write an incident ticket.

This lab helps BlueWave Clinic practise how SOC analysts move from an alert to an investigation summary.

---

## How to Use Copy and Type Options

This lab uses **Copy** and **Type** options for every command, query, search term, path, filename, and template that students may need to enter.

### Copy Option

Use the **Copy** option when you want to copy and paste the text directly into the lab environment.

### Type Option

Use the **Type** option when you need to manually type the text.

> [!note]
> The Copy and Type options contain the same command or text. Use one option unless your instructor tells you otherwise.

> [!alert]
> Type commands and queries exactly as shown. Commands and Kibana queries are sensitive to spaces, punctuation, quotation marks, backslashes, and field names.

---

## Learning Objectives

By the end of this lab, you will be able to:

- Explain the purpose of incident triage.
- Open a simulated alert or detection result in Elastic.
- Use Discover to investigate related endpoint activity.
- Identify the affected host, user, process, parent process, and timestamps.
- Search for related child processes.
- Review local endpoint evidence when needed.
- Create an incident timeline.
- Classify activity as expected, suspicious-looking, or requiring escalation.
- Recommend basic containment and recovery actions.
- Create a structured incident ticket.

---

## Scenario

BlueWave Clinic has started collecting Windows and Sysmon events from WIN11-CLIENT.

In Lab 07, the SOC team created simple detection logic for safe simulator activity.

Today, you are acting as a junior SOC analyst. You receive a simulated detection result for endpoint activity involving:

```text
BlueWaveActivitySimulator.exe
```

The activity may include related process activity such as:

```text
whoami
hostname
ipconfig
notepad.exe
calc.exe
```

Your job is to triage the alert and document what happened.

> [!note]
> The simulator is safe educational activity. It is not malware.

> [!note]
> This lab is defensive only.

> [!alert]
> Do not run malware, exploit code, credential theft tools, brute-force tools, password cracking tools, or internet scanning tools.

> [!alert]
> Do not use Elastic Cloud. This course uses self-managed Elastic with the free Basic license only.

---

## Required Machines

| Machine | Used For |
|---|---|
| WIN11-CLIENT | Kibana access, endpoint verification, evidence notes |
| UBUNTU-SOC | Elasticsearch and Kibana services |
| Kibana browser session on WIN11-CLIENT | Incident triage, Discover searches, alerts or saved query review |

---

## Required Files

| File | Location | Purpose |
|---|---|---|
| BlueWaveActivitySimulator.exe | `C:\LabFiles\Simulators` | Safe educational activity generator |
| Lab8-Incident-Ticket.md | `C:\BlueWave\Evidence` | Student-created incident ticket |
| timeline-template.md | `C:\LabFiles\Templates` | Optional timeline template |

---

## Important Paths

### Windows Paths

| Path | Purpose |
|---|---|
| `C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe` | Safe simulator executable |
| `C:\BlueWave\Evidence` | Evidence folder |
| `C:\BlueWave\Evidence\Lab8-Incident-Ticket.md` | Incident ticket created in this lab |
| `C:\BlueWave\SimulatorOutput` | Simulator output folder |
| `C:\BlueWave\SimulatorOutput\activity-note.txt` | Simulator output file |

### Kibana Areas

| Kibana Area | Purpose |
|---|---|
| Discover | Search and investigate events |
| Alerts | Review generated alerts if available |
| Rules | Review detection rules if available |
| Saved query | Use fallback detection workflow if alerts are unavailable |
| Data view selector | Select logs data |
| Time picker | Set investigation time range |
| KQL query bar | Enter triage queries |
| Event details panel | Review process and event fields |

---

## Information You Need Before Starting

You need the following from previous labs:

| Item | Example |
|---|---|
| Ubuntu SOC IP address | `10.1.1.10` |
| Kibana URL | `http://10.1.1.10:5601` |
| Kibana username | Provided by instructor |
| Kibana password | Provided by instructor |
| Windows hostname | `WIN11-CLIENT` |
| Logs data view | `logs-*` |
| Detection rule or saved query | `BlueWave - Simulator Process Activity` |
| Simulator process | `BlueWaveActivitySimulator.exe` |

> [!note]
> Your Ubuntu SOC IP address may be different. Use the actual IP address from your lab environment.

---

## Incident Triage Questions

During this lab, answer these investigation questions:

1. What triggered the alert or detection?
2. Which host was affected?
3. Which user account was involved?
4. What process started the activity?
5. What parent process started the detected process?
6. Were there related child processes?
7. What timestamps define the activity window?
8. Was the activity expected, suspicious-looking, or malicious?
9. What evidence supports your classification?
10. What containment, recovery, or escalation actions are recommended?

---

## Screenshots You Should Capture

Capture screenshots as instructed by your trainer or Skillable platform.

Recommended screenshots:

1. Elastic Agent service status.
2. Sysmon service status.
3. Kibana Alerts page or fallback Discover query.
4. Primary simulator process query result.
5. Open simulator event details.
6. Child process query results.
7. Timeline or related event evidence.
8. Completed incident ticket.
9. Evidence file validation command.

---

## Key Terms

| Term | Meaning |
|---|---|
| Alert | A detection result that requires review |
| Triage | Initial investigation to determine priority and next steps |
| Incident | A security event or group of events that may require response |
| Evidence | Information used to support an investigation finding |
| Timeline | Events placed in time order |
| Affected host | The system involved in an alert or event |
| User context | The account associated with an event |
| Parent process | The process that launched another process |
| Child process | A process launched by another process |
| Containment | Action to limit possible impact |
| Recovery | Steps to return systems to expected operation |
| Escalation | Sending an issue to a senior analyst or response team |

---

# Task 1 - Confirm WIN11-CLIENT Is Available

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Sign in to **WIN11-CLIENT**.
2. Open **Windows PowerShell**.
3. Confirm the hostname.

### Copy

```powershell
hostname
```

### Type

Type this into PowerShell:

```powershell
hostname
```

4. Press **Enter**.
5. Confirm the result.

## Expected Result

The hostname should usually be:

```text
WIN11-CLIENT
```

If your hostname is different, record the exact value.

## Screenshot Checkpoint

Capture a screenshot of the hostname result if required.

## Record in Incident Ticket

### Copy

```text
Affected host:
```

### Type

Type this into your incident ticket, then add the hostname:

```text
Affected host:
```

---

# Task 2 - Confirm the Evidence Folder Exists

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Check the evidence folder.

### Copy

```powershell
Test-Path "C:\BlueWave\Evidence"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence"
```

2. Press **Enter**.
3. If the result is `False`, create the folder.

### Copy

```powershell
New-Item -Path "C:\BlueWave\Evidence" -ItemType Directory -Force
```

### Type

Type this into PowerShell:

```powershell
New-Item -Path "C:\BlueWave\Evidence" -ItemType Directory -Force
```

## Expected Result

The folder check should return:

```text
True
```

## Screenshot Checkpoint

Capture a screenshot if your instructor requires evidence folder validation.

## Record in Incident Ticket

### Copy

```text
Evidence folder confirmed:
```

### Type

Type this into your incident ticket:

```text
Evidence folder confirmed:
```

---

# Task 3 - Confirm Elastic Agent Is Running

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Check the Elastic Agent service.

### Copy

```powershell
Get-Service elastic-agent
```

### Type

Type this into PowerShell:

```powershell
Get-Service elastic-agent
```

2. Press **Enter**.
3. Review the service status.

## Expected Result

The Elastic Agent service should show:

```text
Running
```

Example:

```text
Status   Name            DisplayName
------   ----            -----------
Running  elastic-agent   Elastic Agent
```

> [!note]
> If Elastic Agent is not installed or not running, review Lab 03 or ask your instructor.

## Screenshot Checkpoint

Capture a screenshot showing the Elastic Agent service status.

## Record in Incident Ticket

### Copy

```text
Elastic Agent service status:
```

### Type

Type this into your incident ticket, then add the value:

```text
Elastic Agent service status:
```

---

# Task 4 - Confirm Sysmon Is Running

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Check the Sysmon64 service.

### Copy

```powershell
Get-Service Sysmon64 -ErrorAction SilentlyContinue
```

### Type

Type this into PowerShell:

```powershell
Get-Service Sysmon64 -ErrorAction SilentlyContinue
```

2. Press **Enter**.
3. If no result appears, check the alternate service name.

### Copy

```powershell
Get-Service Sysmon -ErrorAction SilentlyContinue
```

### Type

Type this into PowerShell:

```powershell
Get-Service Sysmon -ErrorAction SilentlyContinue
```

4. Press **Enter**.

## Expected Result

The Sysmon service should show:

```text
Running
```

Example:

```text
Status   Name      DisplayName
------   ----      -----------
Running  Sysmon64  Sysmon64
```

> [!note]
> If Sysmon is not running, review Lab 05 or ask your instructor.

## Screenshot Checkpoint

Capture a screenshot showing Sysmon service status.

## Record in Incident Ticket

### Copy

```text
Sysmon service status:
```

### Type

Type this into your incident ticket, then add the value:

```text
Sysmon service status:
```

---

# Task 5 - Open Kibana from Windows

## Where to Work

Use **WIN11-CLIENT**.

Use a web browser.

## Steps

1. Open the browser on WIN11-CLIENT.
2. Enter the Kibana URL.
3. Replace `<UBUNTU-SOC-IP>` with the actual Ubuntu SOC IP address.

### Copy

```text
http://<UBUNTU-SOC-IP>:5601
```

### Type

Type this into the browser address bar, replacing `<UBUNTU-SOC-IP>` with your Ubuntu SOC IP address:

```text
http://<UBUNTU-SOC-IP>:5601
```

Example:

```text
http://10.1.1.10:5601
```

4. Press **Enter**.
5. Sign in using the lab credentials if prompted.

## Expected Result

Kibana should open successfully.

## Screenshot Checkpoint

Capture a screenshot showing Kibana open if required.

## Record in Incident Ticket

### Copy

```text
Kibana URL used:
Kibana opened successfully:
```

### Type

Type these lines into your incident ticket, then add your values:

```text
Kibana URL used:
Kibana opened successfully:
```

---

# Task 6 - Open Alerts or Use the Fallback Detection Query

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana**.

## Steps

1. Open the Kibana main navigation menu.
2. Search for **Alerts**.

### Copy

```text
Alerts
```

### Type

Type this into the Kibana navigation search:

```text
Alerts
```

3. Open **Alerts** if available.
4. Set the time range to **Last 24 hours**.

### Copy

```text
Last 24 hours
```

### Type

Select or type this time range:

```text
Last 24 hours
```

5. Look for an alert related to the BlueWave simulator rule.

### Copy

```text
BlueWave - Simulator Process Activity
```

### Type

Search or filter for this alert or rule name:

```text
BlueWave - Simulator Process Activity
```

6. If Alerts are not available or no alert appears, use the fallback workflow in Discover.

Fallback query:

### Copy

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

### Type

Type this fallback query into Discover:

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

## Expected Result

One of the following should happen:

```text
An alert is visible in Kibana.
```

or:

```text
No alert is available, so Discover fallback triage is used.
```

> [!note]
> The fallback workflow is acceptable if Alerts or Detection Rules are unavailable.

## Screenshot Checkpoint

Capture a screenshot showing the alert or fallback Discover query.

## Record in Incident Ticket

### Copy

```text
Alert available:
Fallback triage used:
Alert or detection name:
Initial triage query:
```

### Type

Type these lines into your incident ticket, then add your findings:

```text
Alert available:
Fallback triage used:
Alert or detection name:
Initial triage query:
```

---

# Task 7 - Open Kibana Discover

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Open the Kibana main navigation menu.
2. Search for **Discover**.

### Copy

```text
Discover
```

### Type

Type this into the Kibana navigation search:

```text
Discover
```

3. Select **Discover**.
4. Wait for Discover to load.

## Expected Result

The Discover page should open.

## Screenshot Checkpoint

Capture a screenshot showing Discover open.

## Record in Incident Ticket

### Copy

```text
Discover opened:
```

### Type

Type this into your incident ticket:

```text
Discover opened:
```

---

# Task 8 - Select Data View and Time Range

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Locate the data view selector.
2. Select a logs-related data view.

### Copy

```text
logs-*
```

### Type

Look for or select this data view:

```text
logs-*
```

Alternative possible data view:

### Copy

```text
winlogbeat-*
```

### Type

Look for this data view if `logs-*` is not available:

```text
winlogbeat-*
```

3. Set the time range to **Last 24 hours**.

### Copy

```text
Last 24 hours
```

### Type

Select or type this time range:

```text
Last 24 hours
```

4. Select **Update** or **Refresh** if required.

## Expected Result

A logs-related data view should be selected.

The time range should show:

```text
Last 24 hours
```

## Screenshot Checkpoint

Capture a screenshot showing the data view and time range.

## Record in Incident Ticket

### Copy

```text
Data view selected:
Time range used:
```

### Type

Type these lines into your incident ticket, then add values:

```text
Data view selected:
Time range used:
```

---

# Task 9 - Search for the Simulator Process

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Click the Kibana query bar.
2. Search for the simulator process.

### Copy

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

3. Press **Enter** or select **Update**.
4. Review the results.

If no results appear, try:

### Copy

```text
host.name : "WIN11-CLIENT" and process.executable : *BlueWaveActivitySimulator*
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and process.executable : *BlueWaveActivitySimulator*
```

If still no results appear, try:

### Copy

```text
host.name : "WIN11-CLIENT" and message : *BlueWaveActivitySimulator*
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and message : *BlueWaveActivitySimulator*
```

## Expected Result

You should find simulator process activity if the simulator was run in Lab 06 or Lab 07.

If no event appears, record the query attempts.

## Screenshot Checkpoint

Capture a screenshot showing the simulator query result.

## Record in Incident Ticket

### Copy

```text
Simulator process found:
Simulator process query used:
```

### Type

Type these lines into your incident ticket, then add your findings:

```text
Simulator process found:
Simulator process query used:
```

---

# Task 10 - Open the Simulator Event Details

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Select one simulator event from the results.
2. Open the event details.
3. Review the important fields.

Look for:

### Copy

```text
@timestamp
host.name
user.name
process.name
process.executable
process.command_line
process.parent.name
process.parent.executable
event.provider
event.code
winlog.event_id
```

### Type

Look for these fields in the event details:

```text
@timestamp
host.name
user.name
process.name
process.executable
process.command_line
process.parent.name
process.parent.executable
event.provider
event.code
winlog.event_id
```

## Expected Result

You should identify important details about the simulator process.

## Screenshot Checkpoint

Capture a screenshot showing the simulator event details.

## Record in Incident Ticket

### Copy

```text
Detected event timestamp:
Detected host:
Detected user:
Detected process:
Detected process path:
Detected command line:
Detected parent process:
Detected event provider:
Detected event ID:
```

### Type

Type these lines into your incident ticket, then add values:

```text
Detected event timestamp:
Detected host:
Detected user:
Detected process:
Detected process path:
Detected command line:
Detected parent process:
Detected event provider:
Detected event ID:
```

---

# Task 11 - Search for Sysmon Process Creation Events

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for Sysmon Event ID 1 related to the simulator.

### Copy

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon" and event.code : "1" and process.name : "BlueWaveActivitySimulator.exe"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon" and event.code : "1" and process.name : "BlueWaveActivitySimulator.exe"
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try:

### Copy

```text
host.name : "WIN11-CLIENT" and winlog.event_id : 1 and message : *BlueWaveActivitySimulator*
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and winlog.event_id : 1 and message : *BlueWaveActivitySimulator*
```

## Expected Result

You should identify whether the simulator process appears as a Sysmon Process Create event.

> [!note]
> Sysmon Event ID 1 means Process Create.

## Screenshot Checkpoint

Capture a screenshot showing the Sysmon process creation query result.

## Record in Incident Ticket

### Copy

```text
Sysmon Event ID 1 query used:
Sysmon process creation event found:
```

### Type

Type these lines into your incident ticket, then add findings:

```text
Sysmon Event ID 1 query used:
Sysmon process creation event found:
```

---

# Task 12 - Identify the Investigation Time Window

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Review the simulator event timestamp.
2. Estimate the investigation start time.
3. Estimate the investigation end time.
4. Use a time window around the detected event.

Example time window:

### Copy

```text
15 minutes before the detected event through 15 minutes after the detected event
```

### Type

Use this investigation window:

```text
15 minutes before the detected event through 15 minutes after the detected event
```

5. Record the time window.

## Expected Result

You should have a clear time window for related event searches.

## Screenshot Checkpoint

No screenshot is required unless instructed.

## Record in Incident Ticket

### Copy

```text
Investigation start time:
Investigation end time:
Time window reason:
```

### Type

Type these lines into your incident ticket, then add values:

```text
Investigation start time:
Investigation end time:
Time window reason:
```

---

# Task 13 - Search for Related Events on the Same Host

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for all events from WIN11-CLIENT during the investigation time window.

### Copy

```text
host.name : "WIN11-CLIENT"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT"
```

2. Press **Enter** or select **Update**.
3. Review events around the simulator timestamp.
4. Look for related process activity.

## Expected Result

You should see events from WIN11-CLIENT around the investigation time window.

## Screenshot Checkpoint

Capture a screenshot showing related events from the host.

## Record in Incident Ticket

### Copy

```text
Related host events reviewed:
Number or estimate of related events:
```

### Type

Type these lines into your incident ticket, then add findings:

```text
Related host events reviewed:
Number or estimate of related events:
```

---

# Task 14 - Search for Child Process Activity: whoami

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for `whoami` activity.

### Copy

```text
host.name : "WIN11-CLIENT" and process.command_line : *whoami*
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and process.command_line : *whoami*
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try:

### Copy

```text
host.name : "WIN11-CLIENT" and process.name : "whoami.exe"
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and process.name : "whoami.exe"
```

If still no results appear, try:

### Copy

```text
host.name : "WIN11-CLIENT" and message : *whoami*
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and message : *whoami*
```

## Expected Result

You may find `whoami` process activity related to the simulator or test activity.

## Screenshot Checkpoint

Capture a screenshot showing the `whoami` result or query attempt.

## Record in Incident Ticket

### Copy

```text
whoami activity found:
whoami timestamp:
whoami parent process:
whoami command line:
```

### Type

Type these lines into your incident ticket, then add findings:

```text
whoami activity found:
whoami timestamp:
whoami parent process:
whoami command line:
```

---

# Task 15 - Search for Child Process Activity: hostname

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for `hostname` activity.

### Copy

```text
host.name : "WIN11-CLIENT" and process.command_line : *hostname*
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and process.command_line : *hostname*
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try:

### Copy

```text
host.name : "WIN11-CLIENT" and process.name : "hostname.exe"
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and process.name : "hostname.exe"
```

If still no results appear, try:

### Copy

```text
host.name : "WIN11-CLIENT" and message : *hostname*
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and message : *hostname*
```

## Expected Result

You may find `hostname` process activity related to the simulator or test activity.

## Screenshot Checkpoint

Capture a screenshot showing the `hostname` result or query attempt.

## Record in Incident Ticket

### Copy

```text
hostname activity found:
hostname timestamp:
hostname parent process:
hostname command line:
```

### Type

Type these lines into your incident ticket, then add findings:

```text
hostname activity found:
hostname timestamp:
hostname parent process:
hostname command line:
```

---

# Task 16 - Search for Child Process Activity: ipconfig

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for `ipconfig` activity.

### Copy

```text
host.name : "WIN11-CLIENT" and process.command_line : *ipconfig*
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and process.command_line : *ipconfig*
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try:

### Copy

```text
host.name : "WIN11-CLIENT" and process.name : "ipconfig.exe"
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and process.name : "ipconfig.exe"
```

If still no results appear, try:

### Copy

```text
host.name : "WIN11-CLIENT" and message : *ipconfig*
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and message : *ipconfig*
```

## Expected Result

You may find `ipconfig` process activity.

## Screenshot Checkpoint

Capture a screenshot showing the `ipconfig` result or query attempt.

## Record in Incident Ticket

### Copy

```text
ipconfig activity found:
ipconfig timestamp:
ipconfig parent process:
ipconfig command line:
```

### Type

Type these lines into your incident ticket, then add findings:

```text
ipconfig activity found:
ipconfig timestamp:
ipconfig parent process:
ipconfig command line:
```

---

# Task 17 - Search for Child Process Activity: Notepad and Calculator

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for Notepad activity.

### Copy

```text
host.name : "WIN11-CLIENT" and process.name : "notepad.exe"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and process.name : "notepad.exe"
```

2. Press **Enter** or select **Update**.
3. Record the result.
4. Search for Calculator activity.

### Copy

```text
host.name : "WIN11-CLIENT" and process.name : "calc.exe"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and process.name : "calc.exe"
```

5. Press **Enter** or select **Update**.
6. If Calculator does not appear, try:

### Copy

```text
host.name : "WIN11-CLIENT" and message : *calc*
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and message : *calc*
```

## Expected Result

You may find Notepad or Calculator activity related to simulator execution.

> [!note]
> Calculator may appear under a different process name depending on Windows version.

## Screenshot Checkpoint

Capture a screenshot showing Notepad or Calculator activity or query attempts.

## Record in Incident Ticket

### Copy

```text
Notepad activity found:
Notepad timestamp:
Notepad parent process:

Calculator activity found:
Calculator process name observed:
Calculator timestamp:
Calculator parent process:
```

### Type

Type these lines into your incident ticket, then add findings:

```text
Notepad activity found:
Notepad timestamp:
Notepad parent process:

Calculator activity found:
Calculator process name observed:
Calculator timestamp:
Calculator parent process:
```

---

# Task 18 - Search for Simulator Output File Evidence

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell** and **Kibana Discover**.

## Steps

1. Confirm the simulator output folder exists locally.

### Copy

```powershell
Test-Path "C:\BlueWave\SimulatorOutput"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\SimulatorOutput"
```

2. Confirm the simulator output file exists locally.

### Copy

```powershell
Test-Path "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

3. Read the output file.

### Copy

```powershell
Get-Content "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

### Type

Type this into PowerShell:

```powershell
Get-Content "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

4. In Kibana Discover, search for possible file evidence.

### Copy

```text
host.name : "WIN11-CLIENT" and message : *SimulatorOutput*
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and message : *SimulatorOutput*
```

5. If no results appear, try:

### Copy

```text
host.name : "WIN11-CLIENT" and message : *activity-note.txt*
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and message : *activity-note.txt*
```

## Expected Result

The local folder and file should exist if the simulator ran successfully.

File event visibility in Kibana depends on Sysmon configuration.

## Screenshot Checkpoint

Capture a screenshot of local output file evidence and Kibana file evidence query.

## Record in Incident Ticket

### Copy

```text
Simulator output folder exists:
Simulator output file exists:
Simulator output file message:
File evidence found in Kibana:
File evidence query used:
```

### Type

Type these lines into your incident ticket, then add findings:

```text
Simulator output folder exists:
Simulator output file exists:
Simulator output file message:
File evidence found in Kibana:
File evidence query used:
```

---

# Task 19 - Identify Five Indicators

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover** and your incident ticket.

## Steps

1. Review the simulator event.
2. Review child process searches.
3. Review local output file evidence.
4. Identify five indicators.

Examples include:

### Copy

```text
Host: WIN11-CLIENT
Process: BlueWaveActivitySimulator.exe
Output folder: C:\BlueWave\SimulatorOutput
Output file: C:\BlueWave\SimulatorOutput\activity-note.txt
Command: whoami
Command: hostname
Command: ipconfig
Process: notepad.exe
Process: calc.exe
```

### Type

Use these examples to type five indicators into your incident ticket:

```text
Host: WIN11-CLIENT
Process: BlueWaveActivitySimulator.exe
Output folder: C:\BlueWave\SimulatorOutput
Output file: C:\BlueWave\SimulatorOutput\activity-note.txt
Command: whoami
Command: hostname
Command: ipconfig
Process: notepad.exe
Process: calc.exe
```

## Expected Result

You should identify at least five investigation indicators.

## Screenshot Checkpoint

No screenshot is required unless instructed.

## Record in Incident Ticket

### Copy

```text
Indicator 1:
Indicator 2:
Indicator 3:
Indicator 4:
Indicator 5:
```

### Type

Type these lines into your incident ticket, then add five indicators:

```text
Indicator 1:
Indicator 2:
Indicator 3:
Indicator 4:
Indicator 5:
```

---

# Task 20 - Create a Short Incident Timeline

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover** and your incident ticket.

## Steps

1. Review timestamps from the simulator event and related events.
2. Add at least five timeline entries.
3. Include timestamp, event type, process, user, and notes.

Use this structure:

### Copy

```text
Timeline Entry 1:
Time:
Event:
Process:
User:
Notes:

Timeline Entry 2:
Time:
Event:
Process:
User:
Notes:

Timeline Entry 3:
Time:
Event:
Process:
User:
Notes:

Timeline Entry 4:
Time:
Event:
Process:
User:
Notes:

Timeline Entry 5:
Time:
Event:
Process:
User:
Notes:
```

### Type

Type this timeline structure into your incident ticket:

```text
Timeline Entry 1:
Time:
Event:
Process:
User:
Notes:

Timeline Entry 2:
Time:
Event:
Process:
User:
Notes:

Timeline Entry 3:
Time:
Event:
Process:
User:
Notes:

Timeline Entry 4:
Time:
Event:
Process:
User:
Notes:

Timeline Entry 5:
Time:
Event:
Process:
User:
Notes:
```

## Expected Result

Your ticket should contain a basic investigation timeline.

> [!note]
> If fewer than five events are visible, include query attempts and local evidence checks as timeline entries.

## Screenshot Checkpoint

Capture a screenshot showing your completed timeline if required.

---

# Task 21 - Classify the Incident

## Where to Work

Use **WIN11-CLIENT**.

Use your incident ticket.

## Steps

1. Review all evidence.
2. Choose a classification.

Classification options:

### Copy

```text
Expected training activity
Suspicious-looking but authorised
Needs escalation
Malicious
```

### Type

Choose one of these classifications:

```text
Expected training activity
Suspicious-looking but authorised
Needs escalation
Malicious
```

3. For this lab, use the evidence to explain your classification.
4. Remember that the simulator is safe and authorised.

## Expected Result

The expected classification is usually:

```text
Suspicious-looking but authorised
```

or:

```text
Expected training activity
```

> [!note]
> Do not classify the simulator itself as malware.

## Screenshot Checkpoint

No screenshot is required unless instructed.

## Record in Incident Ticket

### Copy

```text
Incident classification:
Classification explanation:
```

### Type

Type these lines into your incident ticket, then add your explanation:

```text
Incident classification:
Classification explanation:
```

---

# Task 22 - Recommend Containment Actions

## Where to Work

Use **WIN11-CLIENT**.

Use your incident ticket.

## Steps

1. Review the incident classification.
2. Decide whether containment is needed.
3. Because this is safe lab activity, recommend a training-safe response.

Suggested containment notes:

### Copy

```text
No emergency containment is required because this activity was authorised lab simulation. If similar activity occurred unexpectedly in production, isolate the host, preserve evidence, and escalate to a senior analyst.
```

### Type

Type this suggested containment note or write your own:

```text
No emergency containment is required because this activity was authorised lab simulation. If similar activity occurred unexpectedly in production, isolate the host, preserve evidence, and escalate to a senior analyst.
```

## Expected Result

Your ticket should include containment guidance.

## Screenshot Checkpoint

No screenshot is required unless instructed.

## Record in Incident Ticket

### Copy

```text
Containment recommendation:
```

### Type

Type this into your incident ticket, then add your recommendation:

```text
Containment recommendation:
```

---

# Task 23 - Recommend Recovery Actions

## Where to Work

Use **WIN11-CLIENT**.

Use your incident ticket.

## Steps

1. Decide whether recovery is required.
2. Because the simulator is safe, no major recovery should be required.
3. Recommend simple cleanup or verification steps.

Suggested recovery notes:

### Copy

```text
No system recovery is required for the approved lab simulator. Confirm Elastic Agent and Sysmon remain running, preserve screenshots and notes, and leave evidence files in C:\BlueWave\Evidence.
```

### Type

Type this suggested recovery note or write your own:

```text
No system recovery is required for the approved lab simulator. Confirm Elastic Agent and Sysmon remain running, preserve screenshots and notes, and leave evidence files in C:\BlueWave\Evidence.
```

## Expected Result

Your ticket should include recovery guidance.

## Screenshot Checkpoint

No screenshot is required unless instructed.

## Record in Incident Ticket

### Copy

```text
Recovery recommendation:
```

### Type

Type this into your incident ticket, then add your recommendation:

```text
Recovery recommendation:
```

---

# Task 24 - Recommend Escalation Actions

## Where to Work

Use **WIN11-CLIENT**.

Use your incident ticket.

## Steps

1. Decide whether escalation is required.
2. Because this is approved lab activity, escalation is usually not required.
3. Include a note explaining when escalation would be appropriate.

Suggested escalation notes:

### Copy

```text
Escalation is not required for approved lab activity. Escalation would be required if the same process activity occurred without authorisation, involved unknown binaries, affected multiple hosts, or included suspicious network connections.
```

### Type

Type this suggested escalation note or write your own:

```text
Escalation is not required for approved lab activity. Escalation would be required if the same process activity occurred without authorisation, involved unknown binaries, affected multiple hosts, or included suspicious network connections.
```

## Expected Result

Your ticket should include escalation guidance.

## Screenshot Checkpoint

No screenshot is required unless instructed.

## Record in Incident Ticket

### Copy

```text
Escalation recommendation:
```

### Type

Type this into your incident ticket, then add your recommendation:

```text
Escalation recommendation:
```

---

# Task 25 - Create the Incident Ticket File

## Where to Work

Use **WIN11-CLIENT**.

Use **Notepad**.

## Steps

1. Open **Notepad**.
2. Copy or type the incident ticket template below.
3. Fill in the missing information using your investigation results.
4. Select **File**.
5. Select **Save As**.
6. Browse to:

### Copy

```text
C:\BlueWave\Evidence
```

### Type

Type this into the Save As location if needed:

```text
C:\BlueWave\Evidence
```

7. Save the file using the required filename.

### Copy

```text
Lab8-Incident-Ticket.md
```

### Type

Type this filename exactly:

```text
Lab8-Incident-Ticket.md
```

## Incident Ticket Template

### Copy

```markdown
# BlueWave Clinic Incident Ticket

## Lab

Lab 08 - Incident Triage and Response Using Elastic

## Student Information

Student Name:
Date:

## 1. Alert Summary

Alert available:
Fallback triage used:
Alert or detection name:
Initial triage query:

## 2. Environment Verification

Affected host:
Evidence folder confirmed:
Elastic Agent service status:
Sysmon service status:
Kibana URL used:
Kibana opened successfully:
Discover opened:
Data view selected:
Time range used:

## 3. Detected Event Details

Simulator process found:
Simulator process query used:
Detected event timestamp:
Detected host:
Detected user:
Detected process:
Detected process path:
Detected command line:
Detected parent process:
Detected event provider:
Detected event ID:
Sysmon Event ID 1 query used:
Sysmon process creation event found:

## 4. Investigation Time Window

Investigation start time:
Investigation end time:
Time window reason:

## 5. Related Host Events

Related host events reviewed:
Number or estimate of related events:

## 6. Child Process Review

whoami activity found:
whoami timestamp:
whoami parent process:
whoami command line:

hostname activity found:
hostname timestamp:
hostname parent process:
hostname command line:

ipconfig activity found:
ipconfig timestamp:
ipconfig parent process:
ipconfig command line:

Notepad activity found:
Notepad timestamp:
Notepad parent process:

Calculator activity found:
Calculator process name observed:
Calculator timestamp:
Calculator parent process:

## 7. File Evidence

Simulator output folder exists:
Simulator output file exists:
Simulator output file message:
File evidence found in Kibana:
File evidence query used:

## 8. Indicators

Indicator 1:
Indicator 2:
Indicator 3:
Indicator 4:
Indicator 5:

## 9. Timeline

Timeline Entry 1:
Time:
Event:
Process:
User:
Notes:

Timeline Entry 2:
Time:
Event:
Process:
User:
Notes:

Timeline Entry 3:
Time:
Event:
Process:
User:
Notes:

Timeline Entry 4:
Time:
Event:
Process:
User:
Notes:

Timeline Entry 5:
Time:
Event:
Process:
User:
Notes:

## 10. Classification

Incident classification:
Classification explanation:

## 11. Response Recommendations

Containment recommendation:
Recovery recommendation:
Escalation recommendation:

## 12. Analyst Summary

Write 4 to 6 sentences summarising what happened, what evidence was reviewed, and what action is recommended.
```

### Type

Type this template into Notepad manually:

```markdown
# BlueWave Clinic Incident Ticket

## Lab

Lab 08 - Incident Triage and Response Using Elastic

## Student Information

Student Name:
Date:

## 1. Alert Summary

Alert available:
Fallback triage used:
Alert or detection name:
Initial triage query:

## 2. Environment Verification

Affected host:
Evidence folder confirmed:
Elastic Agent service status:
Sysmon service status:
Kibana URL used:
Kibana opened successfully:
Discover opened:
Data view selected:
Time range used:

## 3. Detected Event Details

Simulator process found:
Simulator process query used:
Detected event timestamp:
Detected host:
Detected user:
Detected process:
Detected process path:
Detected command line:
Detected parent process:
Detected event provider:
Detected event ID:
Sysmon Event ID 1 query used:
Sysmon process creation event found:

## 4. Investigation Time Window

Investigation start time:
Investigation end time:
Time window reason:

## 5. Related Host Events

Related host events reviewed:
Number or estimate of related events:

## 6. Child Process Review

whoami activity found:
whoami timestamp:
whoami parent process:
whoami command line:

hostname activity found:
hostname timestamp:
hostname parent process:
hostname command line:

ipconfig activity found:
ipconfig timestamp:
ipconfig parent process:
ipconfig command line:

Notepad activity found:
Notepad timestamp:
Notepad parent process:

Calculator activity found:
Calculator process name observed:
Calculator timestamp:
Calculator parent process:

## 7. File Evidence

Simulator output folder exists:
Simulator output file exists:
Simulator output file message:
File evidence found in Kibana:
File evidence query used:

## 8. Indicators

Indicator 1:
Indicator 2:
Indicator 3:
Indicator 4:
Indicator 5:

## 9. Timeline

Timeline Entry 1:
Time:
Event:
Process:
User:
Notes:

Timeline Entry 2:
Time:
Event:
Process:
User:
Notes:

Timeline Entry 3:
Time:
Event:
Process:
User:
Notes:

Timeline Entry 4:
Time:
Event:
Process:
User:
Notes:

Timeline Entry 5:
Time:
Event:
Process:
User:
Notes:

## 10. Classification

Incident classification:
Classification explanation:

## 11. Response Recommendations

Containment recommendation:
Recovery recommendation:
Escalation recommendation:

## 12. Analyst Summary

Write 4 to 6 sentences summarising what happened, what evidence was reviewed, and what action is recommended.
```

## Expected Result

The incident ticket should be saved at:

```text
C:\BlueWave\Evidence\Lab8-Incident-Ticket.md
```

## Screenshot Checkpoint

Capture a screenshot showing the completed incident ticket.

---

# Task 26 - Confirm the Incident Ticket File Exists

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Confirm the incident ticket exists.

### Copy

```powershell
Test-Path "C:\BlueWave\Evidence\Lab8-Incident-Ticket.md"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab8-Incident-Ticket.md"
```

2. Press **Enter**.
3. If the result is `True`, list the file.

### Copy

```powershell
Get-Item "C:\BlueWave\Evidence\Lab8-Incident-Ticket.md"
```

### Type

Type this into PowerShell:

```powershell
Get-Item "C:\BlueWave\Evidence\Lab8-Incident-Ticket.md"
```

## Expected Result

The `Test-Path` command should return:

```text
True
```

The file should be listed.

## Screenshot Checkpoint

Capture a screenshot showing the validation result.

---

# Task 27 - Final Validation

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana**, **PowerShell**, and **File Explorer**.

## Steps

1. Confirm an alert or fallback detection query was reviewed.
2. Confirm the simulator event was searched.
3. Confirm the event details were reviewed.
4. Confirm related child process searches were completed.
5. Confirm five indicators were identified.
6. Confirm a timeline was created.
7. Confirm classification was completed.
8. Confirm containment, recovery, and escalation recommendations were written.
9. Confirm the incident ticket exists.

### Copy

```powershell
Test-Path "C:\BlueWave\Evidence\Lab8-Incident-Ticket.md"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab8-Incident-Ticket.md"
```

## Expected Result

You should have:

```text
Alert or fallback detection reviewed
Affected host identified
User identified if visible
Simulator process reviewed
Related activity searched
Five indicators listed
Timeline completed
Classification completed
Response recommendations written
Lab8-Incident-Ticket.md saved
```

## Screenshot Checkpoint

Capture any final screenshots required by your instructor.

---

# Validation Checklist

Before finishing the lab, confirm each item is complete.

- [ ] I confirmed WIN11-CLIENT is available.
- [ ] I confirmed the evidence folder exists.
- [ ] I confirmed Elastic Agent is running.
- [ ] I confirmed Sysmon is running.
- [ ] I opened Kibana.
- [ ] I reviewed Alerts or used the fallback Discover query.
- [ ] I opened Discover.
- [ ] I selected a logs data view.
- [ ] I set the time range to Last 24 hours.
- [ ] I searched for the simulator process.
- [ ] I opened simulator event details.
- [ ] I searched for Sysmon Event ID 1.
- [ ] I identified the investigation time window.
- [ ] I reviewed related host events.
- [ ] I searched for `whoami`.
- [ ] I searched for `hostname`.
- [ ] I searched for `ipconfig`.
- [ ] I searched for Notepad and Calculator activity.
- [ ] I reviewed local simulator output file evidence.
- [ ] I identified five indicators.
- [ ] I created a short timeline.
- [ ] I classified the incident.
- [ ] I wrote containment guidance.
- [ ] I wrote recovery guidance.
- [ ] I wrote escalation guidance.
- [ ] I created `Lab8-Incident-Ticket.md`.
- [ ] I captured the required screenshots.

---

# Troubleshooting

## Problem: Kibana does not open

Check the Kibana URL.

### Copy

```text
http://<UBUNTU-SOC-IP>:5601
```

### Type

Type this into the browser, replacing `<UBUNTU-SOC-IP>` with your Ubuntu IP address:

```text
http://<UBUNTU-SOC-IP>:5601
```

If Kibana still does not open, confirm Kibana is running or ask your instructor.

---

## Problem: Alerts are not available

Use the fallback Discover query.

### Copy

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

### Type

Type this into Discover:

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

Record:

```text
Alerts unavailable. Fallback Discover triage was used.
```

---

## Problem: No simulator event appears

Try alternate simulator queries.

### Copy

```text
host.name : "WIN11-CLIENT" and process.executable : *BlueWaveActivitySimulator*
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and process.executable : *BlueWaveActivitySimulator*
```

### Copy

```text
host.name : "WIN11-CLIENT" and message : *BlueWaveActivitySimulator*
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and message : *BlueWaveActivitySimulator*
```

Also try expanding the time range.

### Copy

```text
Last 7 days
```

### Type

Select or type this time range:

```text
Last 7 days
```

If the simulator was not run recently, ask your instructor whether to rerun safe simulator activity.

---

## Problem: Sysmon Event ID 1 does not appear

Try alternate fields.

### Copy

```text
host.name : "WIN11-CLIENT" and winlog.event_id : 1 and message : *BlueWaveActivitySimulator*
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and winlog.event_id : 1 and message : *BlueWaveActivitySimulator*
```

Also try:

### Copy

```text
host.name : "WIN11-CLIENT" and event.code : "1"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.code : "1"
```

---

## Problem: Child process events do not appear

Try broader message searches.

### Copy

```text
host.name : "WIN11-CLIENT" and message : *whoami*
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and message : *whoami*
```

### Copy

```text
host.name : "WIN11-CLIENT" and message : *hostname*
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and message : *hostname*
```

### Copy

```text
host.name : "WIN11-CLIENT" and message : *ipconfig*
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and message : *ipconfig*
```

Record the query attempts even if no results appear.

---

## Problem: File evidence does not appear in Kibana

File activity visibility depends on Sysmon configuration.

Confirm local file evidence instead.

### Copy

```powershell
Test-Path "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

Read the file:

### Copy

```powershell
Get-Content "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

### Type

Type this into PowerShell:

```powershell
Get-Content "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

---

## Problem: Field names are different

Elastic field names may vary.

Try alternate fields such as:

### Copy

```text
event.code
winlog.event_id
event.provider
winlog.provider_name
winlog.channel
event.dataset
process.name
process.executable
process.command_line
process.parent.name
process.parent.executable
user.name
message
```

### Type

Look for these fields in event details:

```text
event.code
winlog.event_id
event.provider
winlog.provider_name
winlog.channel
event.dataset
process.name
process.executable
process.command_line
process.parent.name
process.parent.executable
user.name
message
```

---

## Problem: The incident ticket is missing

Check that you saved it as:

### Copy

```text
Lab8-Incident-Ticket.md
```

### Type

Type this filename exactly:

```text
Lab8-Incident-Ticket.md
```

Check that you saved it in:

### Copy

```text
C:\BlueWave\Evidence
```

### Type

Type this path exactly:

```text
C:\BlueWave\Evidence
```

Confirm with:

### Copy

```powershell
Test-Path "C:\BlueWave\Evidence\Lab8-Incident-Ticket.md"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab8-Incident-Ticket.md"
```

Expected result:

```text
True
```

---

# Knowledge Check

Answer the following questions.

1. What is incident triage?
2. What is the difference between an alert and an incident?
3. What process was the main focus of this investigation?
4. What query can search for the simulator process?
5. What host was affected in this lab?
6. Why is the parent process useful during triage?
7. Name three child processes or commands you searched for.
8. Why might file activity not appear in Kibana?
9. Why should the simulator not be classified as malware?
10. What should an analyst include in an incident ticket?

---

# Summary

In this lab, you completed the following tasks:

- Opened Kibana and reviewed an alert or fallback detection query.
- Searched for simulator process activity.
- Opened event details and reviewed important fields.
- Searched for Sysmon Event ID 1.
- Identified the investigation time window.
- Reviewed related host events.
- Searched for child process activity.
- Reviewed local simulator output evidence.
- Identified five indicators.
- Created a short incident timeline.
- Classified the activity.
- Recommended containment, recovery, and escalation actions.
- Created a structured incident ticket.

You are now ready for Lab 09, the final BlueWave Clinic cyber operations capstone.

---

# Deliverables

Submit or retain the following items as directed by your instructor.

| Deliverable | Location |
|---|---|
| Lab 08 incident ticket | `C:\BlueWave\Evidence\Lab8-Incident-Ticket.md` |
| Screenshot of Alerts page or fallback Discover query | Skillable submission area |
| Screenshot of simulator process query | Skillable submission area |
| Screenshot of simulator event details | Skillable submission area |
| Screenshot of Sysmon Event ID 1 query | Skillable submission area |
| Screenshot of child process search results | Skillable submission area |
| Screenshot of simulator output file evidence | Skillable submission area |
| Screenshot of completed incident ticket | Skillable submission area |
| Screenshot of ticket validation command | Skillable submission area |

---

# Instructor Notes

## Expected Knowledge Check Answers

1. Incident triage is the initial review of an alert or event to determine what happened, priority, and next steps.
2. An alert is a detection result. An incident is a larger investigation that may include alerts, evidence, timeline, and response actions.
3. The main process is:

```text
BlueWaveActivitySimulator.exe
```

4. A useful query is:

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

5. The affected host is usually:

```text
WIN11-CLIENT
```

6. Parent process helps explain how the detected process started.
7. Examples include `whoami`, `hostname`, `ipconfig`, `notepad.exe`, and `calc.exe`.
8. File activity visibility depends on Sysmon configuration and collected fields.
9. The simulator is approved safe educational activity and is not malware.
10. An incident ticket should include alert summary, host, user, process, timestamps, evidence, indicators, timeline, classification, and response recommendations.

---

## Expected Evidence File

Students should create:

```text
C:\BlueWave\Evidence\Lab8-Incident-Ticket.md
```

---

## Expected Elastic Queries

Simulator process query:

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

Simulator executable alternate query:

```text
host.name : "WIN11-CLIENT" and process.executable : *BlueWaveActivitySimulator*
```

Simulator message alternate query:

```text
host.name : "WIN11-CLIENT" and message : *BlueWaveActivitySimulator*
```

Sysmon Event ID 1 simulator query:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon" and event.code : "1" and process.name : "BlueWaveActivitySimulator.exe"
```

Alternate Sysmon Event ID 1 query:

```text
host.name : "WIN11-CLIENT" and winlog.event_id : 1 and message : *BlueWaveActivitySimulator*
```

Related host query:

```text
host.name : "WIN11-CLIENT"
```

Child process queries:

```text
host.name : "WIN11-CLIENT" and process.command_line : *whoami*
```

```text
host.name : "WIN11-CLIENT" and process.command_line : *hostname*
```

```text
host.name : "WIN11-CLIENT" and process.command_line : *ipconfig*
```

```text
host.name : "WIN11-CLIENT" and process.name : "notepad.exe"
```

```text
host.name : "WIN11-CLIENT" and process.name : "calc.exe"
```

File evidence queries:

```text
host.name : "WIN11-CLIENT" and message : *SimulatorOutput*
```

```text
host.name : "WIN11-CLIENT" and message : *activity-note.txt*
```

---

## Expected Windows Commands

Check Elastic Agent:

```powershell
Get-Service elastic-agent
```

Check Sysmon:

```powershell
Get-Service Sysmon64 -ErrorAction SilentlyContinue
```

```powershell
Get-Service Sysmon -ErrorAction SilentlyContinue
```

Check simulator output:

```powershell
Test-Path "C:\BlueWave\SimulatorOutput"
```

```powershell
Test-Path "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

Read simulator output:

```powershell
Get-Content "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

Validate incident ticket:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab8-Incident-Ticket.md"
```

---

## Expected Visible Results

Students should be able to show:

- Elastic Agent running.
- Sysmon running.
- Alert or fallback query reviewed.
- Simulator event searched.
- Event details opened.
- Related child process searches attempted.
- Simulator output file checked.
- Five indicators listed.
- Timeline completed.
- Incident ticket saved.

---

## Common Student Mistakes

| Mistake | Instructor Guidance |
|---|---|
| Student cannot find Alerts | Use the Discover fallback workflow |
| Student uses the wrong hostname | Have them confirm with `hostname` |
| Student searches outside the correct time range | Use Last 24 hours or Last 7 days |
| Student does not open event details | Require timestamp, user, process, parent process, and command line |
| Student misses child process searches | Have them search for `whoami`, `hostname`, `ipconfig`, Notepad, and Calculator |
| Student classifies simulator as malware | Remind them it is safe authorised lab activity |
| Student writes no response actions | Require containment, recovery, and escalation recommendations |
| Student saves the ticket with the wrong filename | Require `Lab8-Incident-Ticket.md` |

---

## Grading Guidance

Suggested grading allocation:

| Criteria | Points |
|---|---:|
| Alert or fallback triage started | 10 |
| Simulator event found or searched | 15 |
| Event details reviewed | 15 |
| Related child process searches completed | 15 |
| Indicators identified | 10 |
| Timeline completed | 10 |
| Classification completed | 10 |
| Response recommendations completed | 10 |
| Evidence ticket and screenshots completed | 5 |
| Total | 100 |

Do not penalise students for using the fallback workflow if Alerts are unavailable.

---

## Free Elastic Basic License Reminder

This lab must use:

- Self-managed Elastic.
- Free Elastic Basic license.
- Kibana Discover.
- Alerts if available.
- Discover fallback if Alerts are unavailable.
- Safe simulated endpoint activity only.
- No Elastic Cloud.
- No paid subscriptions.
- No external internet access.

---

## Fallback Option if Alerts Are Not Available

If Alerts are not available, students should:

1. Use Discover.
2. Run the simulator process query.
3. Open matching event details.
4. Search related events.
5. Build a timeline manually.
6. Complete the incident ticket.

Suggested fallback evidence line:

```text
Alerts were unavailable. Incident triage was completed using Discover queries and local endpoint evidence.
```

---

End of Lab 08.
