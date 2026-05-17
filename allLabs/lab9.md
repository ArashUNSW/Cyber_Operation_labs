# Lab 09 - Final Elastic Cyber Operations Capstone

## Estimated Time

120–180 minutes

---

## Lab Purpose

In this final capstone lab, you will complete an end-to-end BlueWave Clinic cyber operations investigation using Elastic and Kibana.

You will start from a case briefing, investigate endpoint activity, identify the affected system, review process and user activity, collect indicators, build a timeline, classify the incident, and write a final incident report.

This lab combines skills from Labs 01 through 08.

---

## How to Use Copy or Type Inputs

This lab keeps the Skillable **Copy or Type** requirement without repeating the same command twice.

For each command, query, search term, path, filename, URL, or template, you will see one block named:

```text
Student Input - Copy or Type
```

You may either:

- Copy the text directly into the lab environment.
- Type the same text manually.

> [!note]
> Copy or type the text exactly as shown. You only need to use one method.

> [!alert]
> Commands, queries, paths, quotation marks, slashes, backslashes, and spaces must match the instructions exactly.

---

## Learning Objectives

By the end of this capstone lab, you will be able to:

- Interpret a cyber operations case briefing.
- Verify endpoint telemetry collection.
- Use Kibana Discover to investigate Windows and Sysmon events.
- Identify the affected host, user, process, parent process, and timestamps.
- Identify related child processes and command-line activity.
- Collect at least five investigation indicators.
- Build an incident timeline.
- Write an executive summary for non-technical stakeholders.
- Write a technical incident report for SOC handoff.
- Recommend containment, recovery, and escalation actions.
- Validate and save final deliverables.

---

## Scenario

BlueWave Clinic has completed several weeks of SOC onboarding and Elastic training.

The SOC team has now assigned you a final capstone investigation.

A detection or case briefing indicates that unusual but safe lab activity occurred on the Windows endpoint.

Your job is to complete a full analyst workflow:

1. Read the case briefing.
2. Confirm endpoint telemetry is available.
3. Search Elastic for relevant events.
4. Review Windows and Sysmon evidence.
5. Identify affected host and user.
6. Identify processes and related child processes.
7. Build a timeline.
8. Write a final incident report.
9. Recommend response actions.

The investigation is based on safe educational activity and prepared lab evidence.

> [!note]
> This capstone is defensive only.

> [!note]
> The BlueWave Activity Simulator is safe educational activity and is not malware.

> [!alert]
> Do not run malware, exploit code, credential theft tools, brute-force tools, password cracking tools, or internet scanning tools.

> [!alert]
> Do not use Elastic Cloud. This course uses self-managed Elastic with the free Basic license only.

---

## Required Machines

| Machine | Used For |
|---|---|
| WIN11-CLIENT | Kibana access, endpoint verification, evidence review, final report |
| UBUNTU-SOC | Elasticsearch, Kibana, and optional capstone evidence repository |

---

## Required Files

| File | Location | Purpose |
|---|---|---|
| BlueWaveActivitySimulator.exe | `C:\LabFiles\Simulators` | Safe educational activity generator, if live activity is needed |
| capstone-briefing.txt | `C:\LabFiles\Logs` or `/home/student/labfiles/capstone` | Capstone case briefing, if provided |
| capstone-events.csv | `C:\LabFiles\Logs` or `/home/student/labfiles/capstone` | Optional prepared fallback evidence |
| Lab9-Final-Incident-Report.md | `C:\BlueWave\Evidence` | Final capstone report created by the student |

> [!note]
> If prepared capstone files are not present, use live Elastic events from previous simulator and Sysmon labs.

---

## Important Paths

### Windows Paths

| Path | Purpose |
|---|---|
| `C:\BlueWave\Evidence` | Evidence and report folder |
| `C:\BlueWave\Evidence\Lab9-Final-Incident-Report.md` | Final report |
| `C:\LabFiles\Logs` | Optional prepared logs |
| `C:\LabFiles\Logs\capstone-briefing.txt` | Optional Windows capstone briefing |
| `C:\LabFiles\Logs\capstone-events.csv` | Optional Windows capstone evidence |
| `C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe` | Safe simulator |
| `C:\BlueWave\SimulatorOutput` | Simulator output folder |
| `C:\BlueWave\SimulatorOutput\activity-note.txt` | Simulator output note |

### Ubuntu Paths

| Path | Purpose |
|---|---|
| `/home/student/labfiles/capstone` | Optional capstone evidence folder |
| `/home/student/labfiles/capstone/capstone-briefing.txt` | Optional Ubuntu capstone briefing |
| `/home/student/labfiles/capstone/capstone-events.csv` | Optional Ubuntu capstone evidence |
| `/home/student/bluewave/evidence` | Ubuntu evidence folder |

### Kibana Areas

| Kibana Area | Purpose |
|---|---|
| Discover | Main investigation workspace |
| Alerts | Review detection results if available |
| Rules | Review detection rule context if available |
| Data view selector | Select logs data |
| Time picker | Set investigation time range |
| KQL query bar | Search host, process, user, and event fields |
| Event details panel | Review technical event evidence |

---

## Capstone Investigation Questions

Answer these questions during the lab:

1. What started the investigation?
2. What host was affected?
3. What user account was involved?
4. What process was most important to the investigation?
5. What parent process launched it?
6. What child processes or commands were related?
7. What timestamps define the activity window?
8. What indicators support the finding?
9. Was the activity expected, suspicious-looking, or malicious?
10. What response actions should BlueWave Clinic take?

---

## Screenshots You Should Capture

Capture screenshots as instructed by your trainer or Skillable platform.

Recommended screenshots:

1. Evidence folder validation.
2. Elastic Agent service status.
3. Sysmon service status.
4. Kibana Discover with selected data view and time range.
5. Primary investigation query result.
6. Open event details for the key event.
7. Child process query results.
8. File/output evidence result.
9. Timeline or report content.
10. Final report validation command.

---

## Key Terms

| Term | Meaning |
|---|---|
| Capstone | Final lab that combines multiple skills |
| Case briefing | Initial information that starts an investigation |
| Indicator | A useful clue such as host, process, command, path, user, or timestamp |
| Timeline | Events arranged in time order |
| Executive summary | Short non-technical summary for leadership |
| Technical summary | Detailed analyst summary for SOC handoff |
| Containment | Steps to limit possible impact |
| Recovery | Steps to return systems to normal |
| Escalation | Moving an issue to senior analysts or response teams |
| False positive | Detection that does not represent harmful activity |
| Authorised activity | Activity approved for the lab or organisation |

---

# Task 1 - Confirm WIN11-CLIENT Is Available

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Sign in to **WIN11-CLIENT**.
2. Open **Windows PowerShell**.
3. Confirm the hostname.

### Student Input - Copy or Type

```powershell
hostname
```

4. Press **Enter**.
5. Record the hostname.

## Expected Result

The hostname should usually be:

```text
WIN11-CLIENT
```

If your hostname is different, record the exact value.

## Screenshot Checkpoint

Capture a screenshot of the hostname result if required.

## Record in Final Report

### Student Input - Copy or Type

```text
Affected host:
```

---

# Task 2 - Confirm the Windows Evidence Folder Exists

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Check the evidence folder.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\Evidence"
```

2. Press **Enter**.
3. If the result is `False`, create the folder.

### Student Input - Copy or Type

```powershell
New-Item -Path "C:\BlueWave\Evidence" -ItemType Directory -Force
```

4. Confirm the folder contents.

### Student Input - Copy or Type

```powershell
Get-ChildItem "C:\BlueWave\Evidence"
```

## Expected Result

The folder check should return:

```text
True
```

## Screenshot Checkpoint

Capture a screenshot showing the evidence folder validation.

## Record in Final Report

### Student Input - Copy or Type

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

### Student Input - Copy or Type

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

## Record in Final Report

### Student Input - Copy or Type

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

### Student Input - Copy or Type

```powershell
Get-Service Sysmon64 -ErrorAction SilentlyContinue
```

2. Press **Enter**.
3. If no result appears, check the alternate service name.

### Student Input - Copy or Type

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

## Record in Final Report

### Student Input - Copy or Type

```text
Sysmon service status:
```

---

# Task 5 - Review the Capstone Briefing on Windows

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Check whether a Windows capstone briefing file exists.

### Student Input - Copy or Type

```powershell
Test-Path "C:\LabFiles\Logs\capstone-briefing.txt"
```

2. Press **Enter**.
3. If the result is `True`, display the briefing.

### Student Input - Copy or Type

```powershell
Get-Content "C:\LabFiles\Logs\capstone-briefing.txt"
```

4. If the briefing file does not exist, continue to the next task and use the default capstone briefing below.

## Default Capstone Briefing

### Student Input - Copy or Type

```text
BlueWave Clinic SOC received a detection related to process activity on WIN11-CLIENT. The activity may include BlueWaveActivitySimulator.exe and related commands such as whoami, hostname, and ipconfig. The analyst must determine what occurred, identify the affected user and process relationships, collect indicators, build a timeline, and write a final incident report.
```

## Expected Result

You should have a case briefing to begin the investigation.

## Screenshot Checkpoint

Capture a screenshot showing the briefing or the missing file check.

## Record in Final Report

### Student Input - Copy or Type

```text
Case briefing source:
Case briefing summary:
```

---

# Task 6 - Review Optional Capstone Files on Ubuntu

## Where to Work

Use **UBUNTU-SOC**.

Use **Terminal**.

## Steps

1. Open **UBUNTU-SOC**.
2. Open **Terminal**.
3. Check the capstone folder.

### Student Input - Copy or Type

```bash
ls -la /home/student/labfiles/capstone
```

4. Press **Enter**.
5. If a briefing file exists, display it.

### Student Input - Copy or Type

```bash
cat /home/student/labfiles/capstone/capstone-briefing.txt
```

6. If an event file exists, list it.

### Student Input - Copy or Type

```bash
ls -l /home/student/labfiles/capstone/capstone-events.csv
```

## Expected Result

The Ubuntu capstone folder may contain prepared evidence files.

If files are not present, continue with live Elastic investigation.

## Screenshot Checkpoint

Capture a screenshot if capstone files are present.

## Record in Final Report

### Student Input - Copy or Type

```text
Ubuntu capstone folder checked:
Ubuntu capstone briefing found:
Ubuntu capstone event file found:
```

---

# Task 7 - Open Kibana from Windows

## Where to Work

Use **WIN11-CLIENT**.

Use a web browser.

## Steps

1. Open the browser on WIN11-CLIENT.
2. Enter the Kibana URL.
3. Replace `<UBUNTU-SOC-IP>` with the actual Ubuntu SOC IP address.

### Student Input - Copy or Type

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

## Record in Final Report

### Student Input - Copy or Type

```text
Kibana URL used:
Kibana opened successfully:
```

---

# Task 8 - Open Kibana Discover

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Open the Kibana main navigation menu.
2. Search for **Discover**.

### Student Input - Copy or Type

```text
Discover
```

3. Select **Discover**.
4. Wait for Discover to load.

## Expected Result

The Discover page should open.

## Screenshot Checkpoint

Capture a screenshot showing Discover open.

## Record in Final Report

### Student Input - Copy or Type

```text
Discover opened:
```

---

# Task 9 - Select Data View and Time Range

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Locate the data view selector.
2. Select a logs-related data view.

### Student Input - Copy or Type

```text
logs-*
```

Alternative possible data view:

### Student Input - Copy or Type

```text
winlogbeat-*
```

3. Set the time range to **Last 7 days** for the capstone.

### Student Input - Copy or Type

```text
Last 7 days
```

4. Select **Update** or **Refresh** if required.

## Expected Result

A logs-related data view should be selected.

The time range should show:

```text
Last 7 days
```

> [!note]
> The final capstone uses a wider time range to help find previous lab activity.

## Screenshot Checkpoint

Capture a screenshot showing the data view and time range.

## Record in Final Report

### Student Input - Copy or Type

```text
Data view selected:
Time range used:
```

---

# Task 10 - Search for Events from WIN11-CLIENT

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for events from WIN11-CLIENT.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT"
```

2. Press **Enter** or select **Update**.
3. Review the result count and event table.

If no results appear, try:

### Student Input - Copy or Type

```text
agent.name : "WIN11-CLIENT"
```

If still no results appear, try:

### Student Input - Copy or Type

```text
message : *WIN11-CLIENT*
```

## Expected Result

Events from WIN11-CLIENT should appear.

## Screenshot Checkpoint

Capture a screenshot showing events from WIN11-CLIENT.

## Record in Final Report

### Student Input - Copy or Type

```text
Host event query used:
Events from affected host found:
```

---

# Task 11 - Search for the Primary Capstone Process

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for the primary capstone process.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.executable : *BlueWaveActivitySimulator*
```

If still no results appear, try:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *BlueWaveActivitySimulator*
```

## Expected Result

You should find BlueWave Activity Simulator events if it was run in previous labs or during the capstone.

> [!note]
> The simulator is safe educational activity and is not malware.

## Screenshot Checkpoint

Capture a screenshot showing the primary process query.

## Record in Final Report

### Student Input - Copy or Type

```text
Primary process query used:
Primary process found:
Primary process name:
```

---

# Task 12 - Open the Key Event Details

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Select one key event from the primary process query results.
2. Open the event details.
3. Review important fields.

Look for:

### Student Input - Copy or Type

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

You should identify the key event timestamp, host, user, process, command line, parent process, provider, and event ID.

## Screenshot Checkpoint

Capture a screenshot showing the key event details.

## Record in Final Report

### Student Input - Copy or Type

```text
Key event timestamp:
Affected host:
Affected user:
Key process:
Key process path:
Key command line:
Parent process:
Parent process path:
Event provider:
Event ID:
```

---

# Task 13 - Search for Sysmon Event ID 1

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for Sysmon process creation events for the primary process.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon" and event.code : "1" and process.name : "BlueWaveActivitySimulator.exe"
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and winlog.event_id : 1 and message : *BlueWaveActivitySimulator*
```

If still no results appear, search all Sysmon process creation events:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and event.code : "1"
```

## Expected Result

You should identify whether the key event is a Sysmon Event ID 1 process creation event.

## Screenshot Checkpoint

Capture a screenshot showing the Sysmon Event ID 1 query.

## Record in Final Report

### Student Input - Copy or Type

```text
Sysmon Event ID 1 query used:
Sysmon process creation event found:
```

---

# Task 14 - Search for Related whoami Activity

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for `whoami` activity.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.command_line : *whoami*
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "whoami.exe"
```

If still no results appear, try:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *whoami*
```

## Expected Result

You may find `whoami` process activity related to the case.

## Screenshot Checkpoint

Capture a screenshot showing the `whoami` result or query attempt.

## Record in Final Report

### Student Input - Copy or Type

```text
whoami activity found:
whoami timestamp:
whoami user:
whoami parent process:
whoami command line:
```

---

# Task 15 - Search for Related hostname Activity

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for `hostname` activity.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.command_line : *hostname*
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "hostname.exe"
```

If still no results appear, try:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *hostname*
```

## Expected Result

You may find `hostname` process activity related to the case.

## Screenshot Checkpoint

Capture a screenshot showing the `hostname` result or query attempt.

## Record in Final Report

### Student Input - Copy or Type

```text
hostname activity found:
hostname timestamp:
hostname user:
hostname parent process:
hostname command line:
```

---

# Task 16 - Search for Related ipconfig Activity

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for `ipconfig` activity.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.command_line : *ipconfig*
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "ipconfig.exe"
```

If still no results appear, try:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *ipconfig*
```

## Expected Result

You may find `ipconfig` process activity related to the case.

## Screenshot Checkpoint

Capture a screenshot showing the `ipconfig` result or query attempt.

## Record in Final Report

### Student Input - Copy or Type

```text
ipconfig activity found:
ipconfig timestamp:
ipconfig user:
ipconfig parent process:
ipconfig command line:
```

---

# Task 17 - Search for Notepad and Calculator Activity

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for Notepad activity.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "notepad.exe"
```

2. Press **Enter** or select **Update**.
3. Record the result.
4. Search for Calculator activity.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "calc.exe"
```

5. Press **Enter** or select **Update**.
6. If Calculator does not appear, try:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *calc*
```

## Expected Result

You may find Notepad or Calculator activity related to the case.

> [!note]
> Calculator may appear under a different process name depending on Windows version.

## Screenshot Checkpoint

Capture a screenshot showing Notepad or Calculator activity or query attempts.

## Record in Final Report

### Student Input - Copy or Type

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

# Task 18 - Review Local Simulator Output Evidence

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Check whether the simulator output folder exists.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\SimulatorOutput"
```

2. Press **Enter**.
3. Check whether the simulator output file exists.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

4. Press **Enter**.
5. If the file exists, read it.

### Student Input - Copy or Type

```powershell
Get-Content "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

## Expected Result

If simulator activity occurred, the output folder and file should exist.

The output file may contain:

```text
BlueWave simulator activity completed
```

## Screenshot Checkpoint

Capture a screenshot showing the local simulator output evidence.

## Record in Final Report

### Student Input - Copy or Type

```text
Simulator output folder exists:
Simulator output file exists:
Simulator output file message:
```

---

# Task 19 - Search for File Evidence in Kibana

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for the simulator output folder.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *SimulatorOutput*
```

2. Press **Enter** or select **Update**.
3. If no results appear, search for the output file.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *activity-note.txt*
```

4. If no results appear, search for BlueWave.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *BlueWave*
```

## Expected Result

You may find file-related or message-related evidence.

> [!note]
> File activity visibility depends on Sysmon configuration. Local file evidence is acceptable if Kibana file events are not visible.

## Screenshot Checkpoint

Capture a screenshot showing the file evidence query.

## Record in Final Report

### Student Input - Copy or Type

```text
File evidence found in Kibana:
File evidence query used:
File evidence result:
```

---

# Task 20 - Search for Windows Security Events Around the Activity

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for Security events from the affected host.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Security-Auditing"
```

2. Press **Enter** or select **Update**.
3. Review events around the investigation time window.

If no results appear, try:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Security"
```

## Expected Result

You may find Security events from the affected host.

## Screenshot Checkpoint

Capture a screenshot showing Security event search results or query attempt.

## Record in Final Report

### Student Input - Copy or Type

```text
Security events reviewed:
Security query used:
Relevant Security event found:
Relevant Security event ID:
```

---

# Task 21 - Identify at Least Five Indicators

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**, PowerShell output, and your final report.

## Steps

1. Review all evidence collected.
2. Identify at least five indicators.
3. Indicators may include host, user, process, command line, parent process, file path, or event ID.

Suggested indicators:

### Student Input - Copy or Type

```text
Host: WIN11-CLIENT
Process: BlueWaveActivitySimulator.exe
File path: C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe
Output folder: C:\BlueWave\SimulatorOutput
Output file: C:\BlueWave\SimulatorOutput\activity-note.txt
Command: whoami
Command: hostname
Command: ipconfig
Process: notepad.exe
Process: calc.exe
Event provider: Microsoft-Windows-Sysmon
Event ID: 1
```

## Expected Result

Your report should contain at least five indicators.

## Screenshot Checkpoint

No screenshot is required unless instructed.

## Record in Final Report

### Student Input - Copy or Type

```text
Indicator 1:
Indicator 2:
Indicator 3:
Indicator 4:
Indicator 5:
Additional indicators:
```

---

# Task 22 - Build the Final Timeline

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover** and your final report.

## Steps

1. Review event timestamps.
2. Add at least six timeline entries.
3. Include local evidence checks if needed.
4. Include timestamp, source, event, process, user, and notes.

Timeline structure:

### Student Input - Copy or Type

```text
Timeline Entry 1
Time:
Source:
Event:
Host:
User:
Process:
Parent process:
Notes:

Timeline Entry 2
Time:
Source:
Event:
Host:
User:
Process:
Parent process:
Notes:

Timeline Entry 3
Time:
Source:
Event:
Host:
User:
Process:
Parent process:
Notes:

Timeline Entry 4
Time:
Source:
Event:
Host:
User:
Process:
Parent process:
Notes:

Timeline Entry 5
Time:
Source:
Event:
Host:
User:
Process:
Parent process:
Notes:

Timeline Entry 6
Time:
Source:
Event:
Host:
User:
Process:
Parent process:
Notes:
```

## Expected Result

Your final report should include a timeline with at least six entries.

> [!note]
> If fewer than six Elastic events are visible, include local checks or query attempts as timeline entries.

## Screenshot Checkpoint

Capture a screenshot of your timeline if required.

---

# Task 23 - Classify the Capstone Activity

## Where to Work

Use **WIN11-CLIENT**.

Use your final report.

## Steps

1. Review the case briefing.
2. Review the key process.
3. Review child process and file evidence.
4. Choose a classification.

Classification options:

### Student Input - Copy or Type

```text
Expected training activity
Suspicious-looking but authorised
Needs escalation
Malicious
```

5. Explain your classification.

## Expected Result

For the BlueWave simulator scenario, the expected classification is usually:

```text
Suspicious-looking but authorised
```

or:

```text
Expected training activity
```

> [!alert]
> Do not classify the safe simulator itself as malware.

## Screenshot Checkpoint

No screenshot is required unless instructed.

## Record in Final Report

### Student Input - Copy or Type

```text
Final classification:
Classification rationale:
```

---

# Task 24 - Write the Executive Summary

## Where to Work

Use **WIN11-CLIENT**.

Use your final report.

## Steps

1. Write a short non-technical executive summary.
2. Keep it to 4 to 6 sentences.
3. Include what happened, whether the activity was authorised, and recommended next steps.

Suggested executive summary structure:

### Student Input - Copy or Type

```text
BlueWave Clinic reviewed endpoint activity on WIN11-CLIENT after a detection identified simulator-related process activity. The activity involved the approved BlueWave Activity Simulator and related command activity used for SOC training. The investigation reviewed Elastic events, Sysmon process creation data, and local endpoint evidence. The activity was classified as authorised training activity, although it appeared suspicious enough to require analyst review. No emergency containment is required, but the evidence should be retained and similar unexpected activity in production should be escalated.
```

## Expected Result

Your final report should include an executive summary.

## Screenshot Checkpoint

No screenshot is required unless instructed.

## Record in Final Report

### Student Input - Copy or Type

```text
Executive summary:
```

---

# Task 25 - Write the Technical Summary

## Where to Work

Use **WIN11-CLIENT**.

Use your final report.

## Steps

1. Write a technical summary for SOC handoff.
2. Include:
   - affected host
   - user
   - key process
   - parent process
   - child processes
   - event provider
   - event ID
   - evidence sources
   - timeline summary

Technical summary structure:

### Student Input - Copy or Type

```text
The affected endpoint was WIN11-CLIENT. The key process reviewed was BlueWaveActivitySimulator.exe. The investigation used Kibana Discover, Sysmon process creation events, Windows event data, and local endpoint file checks. The key event was reviewed for timestamp, user, process path, command line, parent process, event provider, and event ID. Related activity searches included whoami, hostname, ipconfig, Notepad, and Calculator. Evidence supports that the activity was authorised lab simulation rather than malicious activity.
```

## Expected Result

Your final report should include a technical summary.

## Screenshot Checkpoint

No screenshot is required unless instructed.

## Record in Final Report

### Student Input - Copy or Type

```text
Technical summary:
```

---

# Task 26 - Recommend Containment Actions

## Where to Work

Use **WIN11-CLIENT**.

Use your final report.

## Steps

1. Review the classification.
2. Recommend containment actions.
3. Because this is safe authorised lab activity, emergency containment is not required.

Suggested containment recommendation:

### Student Input - Copy or Type

```text
No emergency containment is required because the activity was approved lab simulation. If similar activity occurred unexpectedly in production, isolate the affected endpoint, preserve Elastic and endpoint evidence, and escalate to a senior analyst.
```

## Expected Result

Your final report should include containment guidance.

## Screenshot Checkpoint

No screenshot is required unless instructed.

## Record in Final Report

### Student Input - Copy or Type

```text
Containment recommendation:
```

---

# Task 27 - Recommend Recovery Actions

## Where to Work

Use **WIN11-CLIENT**.

Use your final report.

## Steps

1. Recommend recovery actions.
2. Because the activity is approved lab simulation, no major recovery is required.

Suggested recovery recommendation:

### Student Input - Copy or Type

```text
No system recovery is required for the approved simulator activity. Confirm Elastic Agent and Sysmon remain running, retain the final report and screenshots, and leave evidence files in C:\BlueWave\Evidence for grading and review.
```

## Expected Result

Your final report should include recovery guidance.

## Screenshot Checkpoint

No screenshot is required unless instructed.

## Record in Final Report

### Student Input - Copy or Type

```text
Recovery recommendation:
```

---

# Task 28 - Recommend Escalation Actions

## Where to Work

Use **WIN11-CLIENT**.

Use your final report.

## Steps

1. Decide whether escalation is required.
2. Because this is authorised lab activity, escalation is usually not required.
3. Explain when escalation would be required.

Suggested escalation recommendation:

### Student Input - Copy or Type

```text
Escalation is not required for approved lab activity. Escalation would be required if this activity occurred without authorisation, involved an unknown executable, affected multiple hosts, included unusual network connections, or showed signs of credential misuse.
```

## Expected Result

Your final report should include escalation guidance.

## Screenshot Checkpoint

No screenshot is required unless instructed.

## Record in Final Report

### Student Input - Copy or Type

```text
Escalation recommendation:
```

---

# Task 29 - Create the Final Incident Report

## Where to Work

Use **WIN11-CLIENT**.

Use **Notepad**.

## Steps

1. Open **Notepad**.
2. Copy or type the final report template below.
3. Fill in all sections using your investigation evidence.
4. Select **File**.
5. Select **Save As**.
6. Browse to:

### Student Input - Copy or Type

```text
C:\BlueWave\Evidence
```

7. Save the file using the required filename.

### Student Input - Copy or Type

```text
Lab9-Final-Incident-Report.md
```

## Final Report Template

### Student Input - Copy or Type

```markdown
# BlueWave Clinic Final Incident Report

## Lab

Lab 09 - Final Elastic Cyber Operations Capstone

## Student Information

Student Name:
Date:

## 1. Executive Summary

Executive summary:

## 2. Case Briefing

Case briefing source:
Case briefing summary:
Ubuntu capstone folder checked:
Ubuntu capstone briefing found:
Ubuntu capstone event file found:

## 3. Environment Verification

Affected host:
Evidence folder confirmed:
Elastic Agent service status:
Sysmon service status:
Kibana URL used:
Kibana opened successfully:
Discover opened:
Data view selected:
Time range used:

## 4. Key Event Details

Host event query used:
Events from affected host found:
Primary process query used:
Primary process found:
Primary process name:
Key event timestamp:
Affected user:
Key process:
Key process path:
Key command line:
Parent process:
Parent process path:
Event provider:
Event ID:
Sysmon Event ID 1 query used:
Sysmon process creation event found:

## 5. Related Activity

whoami activity found:
whoami timestamp:
whoami user:
whoami parent process:
whoami command line:

hostname activity found:
hostname timestamp:
hostname user:
hostname parent process:
hostname command line:

ipconfig activity found:
ipconfig timestamp:
ipconfig user:
ipconfig parent process:
ipconfig command line:

Notepad activity found:
Notepad timestamp:
Notepad parent process:

Calculator activity found:
Calculator process name observed:
Calculator timestamp:
Calculator parent process:

## 6. File and Local Evidence

Simulator output folder exists:
Simulator output file exists:
Simulator output file message:
File evidence found in Kibana:
File evidence query used:
File evidence result:

## 7. Windows Security Review

Security events reviewed:
Security query used:
Relevant Security event found:
Relevant Security event ID:

## 8. Indicators

Indicator 1:
Indicator 2:
Indicator 3:
Indicator 4:
Indicator 5:
Additional indicators:

## 9. Timeline

Timeline Entry 1
Time:
Source:
Event:
Host:
User:
Process:
Parent process:
Notes:

Timeline Entry 2
Time:
Source:
Event:
Host:
User:
Process:
Parent process:
Notes:

Timeline Entry 3
Time:
Source:
Event:
Host:
User:
Process:
Parent process:
Notes:

Timeline Entry 4
Time:
Source:
Event:
Host:
User:
Process:
Parent process:
Notes:

Timeline Entry 5
Time:
Source:
Event:
Host:
User:
Process:
Parent process:
Notes:

Timeline Entry 6
Time:
Source:
Event:
Host:
User:
Process:
Parent process:
Notes:

## 10. Classification

Final classification:
Classification rationale:

## 11. Technical Summary

Technical summary:

## 12. Response Recommendations

Containment recommendation:
Recovery recommendation:
Escalation recommendation:

## 13. Final Analyst Notes

Write 4 to 6 sentences explaining what you learned during the capstone investigation and how Elastic helped the investigation.
```

## Expected Result

The final report should be saved at:

```text
C:\BlueWave\Evidence\Lab9-Final-Incident-Report.md
```

## Screenshot Checkpoint

Capture a screenshot showing the completed final report.

---

# Task 30 - Confirm the Final Report Exists

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Confirm the final report exists.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\Evidence\Lab9-Final-Incident-Report.md"
```

2. Press **Enter**.
3. If the result is `True`, list the file.

### Student Input - Copy or Type

```powershell
Get-Item "C:\BlueWave\Evidence\Lab9-Final-Incident-Report.md"
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

# Task 31 - Final Capstone Validation

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana**, **PowerShell**, and **File Explorer**.

## Steps

1. Confirm the case briefing was reviewed.
2. Confirm telemetry services were checked.
3. Confirm Discover was used.
4. Confirm the affected host was identified.
5. Confirm the key process was investigated.
6. Confirm key event details were reviewed.
7. Confirm related activity searches were completed.
8. Confirm at least five indicators were listed.
9. Confirm at least six timeline entries were created.
10. Confirm executive and technical summaries were written.
11. Confirm containment, recovery, and escalation recommendations were written.
12. Confirm the final report exists.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\Evidence\Lab9-Final-Incident-Report.md"
```

## Expected Result

You should have:

```text
Case briefing reviewed
Elastic Agent and Sysmon checked
Kibana Discover used
Affected host identified
Affected user identified if visible
Key process reviewed
Parent process reviewed
Child process searches completed
Five or more indicators listed
Six or more timeline entries created
Executive summary written
Technical summary written
Response recommendations written
Lab9-Final-Incident-Report.md saved
```

## Screenshot Checkpoint

Capture any final screenshots required by your instructor.

---

# Validation Checklist

Before finishing the capstone, confirm each item is complete.

- [ ] I confirmed WIN11-CLIENT is available.
- [ ] I confirmed the evidence folder exists.
- [ ] I confirmed Elastic Agent is running.
- [ ] I confirmed Sysmon is running.
- [ ] I reviewed the capstone briefing or default briefing.
- [ ] I checked optional Ubuntu capstone files.
- [ ] I opened Kibana.
- [ ] I opened Discover.
- [ ] I selected a logs data view.
- [ ] I set the time range to Last 7 days.
- [ ] I searched for events from WIN11-CLIENT.
- [ ] I searched for the primary capstone process.
- [ ] I opened the key event details.
- [ ] I searched for Sysmon Event ID 1.
- [ ] I searched for `whoami`.
- [ ] I searched for `hostname`.
- [ ] I searched for `ipconfig`.
- [ ] I searched for Notepad and Calculator activity.
- [ ] I reviewed local simulator output evidence.
- [ ] I searched for file evidence in Kibana.
- [ ] I reviewed Windows Security events.
- [ ] I identified at least five indicators.
- [ ] I built a timeline with at least six entries.
- [ ] I classified the capstone activity.
- [ ] I wrote an executive summary.
- [ ] I wrote a technical summary.
- [ ] I wrote containment guidance.
- [ ] I wrote recovery guidance.
- [ ] I wrote escalation guidance.
- [ ] I created `Lab9-Final-Incident-Report.md`.
- [ ] I captured required screenshots.

---

# Troubleshooting

## Problem: Kibana does not open

Check the Kibana URL.

### Student Input - Copy or Type

```text
http://<UBUNTU-SOC-IP>:5601
```

If Kibana still does not open, confirm Kibana is running or ask your instructor.

---

## Problem: No capstone briefing file exists

Use the default briefing in Task 5.

Record:

```text
No prepared briefing file was found. Default capstone briefing was used.
```

---

## Problem: No events appear from WIN11-CLIENT

Try alternate host queries.

### Student Input - Copy or Type

```text
agent.name : "WIN11-CLIENT"
```

### Student Input - Copy or Type

```text
message : *WIN11-CLIENT*
```

Also expand the time range.

### Student Input - Copy or Type

```text
Last 30 days
```

---

## Problem: The simulator process is not found

Try alternate simulator queries.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.executable : *BlueWaveActivitySimulator*
```

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *BlueWaveActivitySimulator*
```

If instructed, generate safe activity by running the simulator:

### Student Input - Copy or Type

```powershell
Start-Process -FilePath "C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe"
```

> [!note]
> Only rerun the simulator if your instructor allows it.

---

## Problem: Sysmon Event ID 1 does not appear

Try alternate fields.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and winlog.event_id : 1
```

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and event.code : "1"
```

---

## Problem: Child process activity does not appear

Try broader message searches.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *whoami*
```

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *hostname*
```

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *ipconfig*
```

Record the query attempts even if no results appear.

---

## Problem: File evidence does not appear in Kibana

File activity visibility depends on Sysmon configuration.

Confirm local file evidence instead.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

Read the file:

### Student Input - Copy or Type

```powershell
Get-Content "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

---

## Problem: Field names are different

Elastic field names may vary.

Try alternate fields such as:

### Student Input - Copy or Type

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

## Problem: The final report is missing

Check that you saved it as:

### Student Input - Copy or Type

```text
Lab9-Final-Incident-Report.md
```

Check that you saved it in:

### Student Input - Copy or Type

```text
C:\BlueWave\Evidence
```

Confirm with:

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\Evidence\Lab9-Final-Incident-Report.md"
```

Expected result:

```text
True
```

---

# Knowledge Check

Answer the following questions.

1. What is the purpose of a capstone lab?
2. What Kibana page did you use for most of the investigation?
3. What host was the main focus of the investigation?
4. What process was the primary focus of the capstone?
5. What does Sysmon Event ID 1 represent?
6. Why is parent process information important?
7. Name three related commands or child processes you searched for.
8. What are indicators in an investigation?
9. What is the difference between an executive summary and a technical summary?
10. Why should the approved simulator activity not be classified as malware?

---

# Summary

In this capstone lab, you completed the following tasks:

- Reviewed the capstone case briefing.
- Verified Elastic Agent and Sysmon.
- Opened Kibana Discover.
- Searched for events from WIN11-CLIENT.
- Investigated the primary process.
- Opened key event details.
- Reviewed Sysmon process creation evidence.
- Searched for related command and child process activity.
- Reviewed local and Kibana file evidence.
- Reviewed Windows Security events.
- Identified indicators.
- Built a timeline.
- Classified the activity.
- Wrote executive and technical summaries.
- Recommended containment, recovery, and escalation actions.
- Created a final incident report.

This completes the BlueWave Clinic Cyber Operations with Elastic lab series.

---

# Deliverables

Submit or retain the following items as directed by your instructor.

| Deliverable | Location |
|---|---|
| Final incident report | `C:\BlueWave\Evidence\Lab9-Final-Incident-Report.md` |
| Screenshot of evidence folder validation | Skillable submission area |
| Screenshot of Elastic Agent service status | Skillable submission area |
| Screenshot of Sysmon service status | Skillable submission area |
| Screenshot of Discover with data view and time range | Skillable submission area |
| Screenshot of primary process query | Skillable submission area |
| Screenshot of key event details | Skillable submission area |
| Screenshot of Sysmon Event ID 1 query | Skillable submission area |
| Screenshot of related child process query | Skillable submission area |
| Screenshot of local or Kibana file evidence | Skillable submission area |
| Screenshot of completed final report | Skillable submission area |
| Screenshot of final report validation command | Skillable submission area |

---

# Instructor Notes

## Expected Knowledge Check Answers

1. A capstone lab combines skills from previous labs into a complete investigation.
2. Kibana Discover is used for most of the investigation.
3. The main host is usually:

```text
WIN11-CLIENT
```

4. The primary process is usually:

```text
BlueWaveActivitySimulator.exe
```

5. Sysmon Event ID 1 represents Process Create.
6. Parent process information helps explain how a process started.
7. Examples include `whoami`, `hostname`, `ipconfig`, `notepad.exe`, and `calc.exe`.
8. Indicators are useful clues such as hostnames, usernames, process names, file paths, event IDs, command lines, and timestamps.
9. An executive summary is short and non-technical. A technical summary includes detailed evidence for analysts.
10. The simulator is approved safe educational activity and is not malware.

---

## Expected Final Report

Students should create:

```text
C:\BlueWave\Evidence\Lab9-Final-Incident-Report.md
```

---

## Expected Elastic Queries

Host query:

```text
host.name : "WIN11-CLIENT"
```

Alternate host queries:

```text
agent.name : "WIN11-CLIENT"
```

```text
message : *WIN11-CLIENT*
```

Primary process query:

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

Alternate primary process queries:

```text
host.name : "WIN11-CLIENT" and process.executable : *BlueWaveActivitySimulator*
```

```text
host.name : "WIN11-CLIENT" and message : *BlueWaveActivitySimulator*
```

Sysmon Event ID 1 query:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon" and event.code : "1" and process.name : "BlueWaveActivitySimulator.exe"
```

Alternate Sysmon Event ID 1 query:

```text
host.name : "WIN11-CLIENT" and winlog.event_id : 1 and message : *BlueWaveActivitySimulator*
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

```text
host.name : "WIN11-CLIENT" and message : *BlueWave*
```

Security event queries:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Security-Auditing"
```

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Security"
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

Validate final report:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab9-Final-Incident-Report.md"
```

---

## Expected Indicators

Students may include:

```text
WIN11-CLIENT
BlueWaveActivitySimulator.exe
C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe
C:\BlueWave\SimulatorOutput
C:\BlueWave\SimulatorOutput\activity-note.txt
whoami
hostname
ipconfig
notepad.exe
calc.exe
Microsoft-Windows-Sysmon
Event ID 1
```

---

## Expected Classification

Acceptable classifications include:

```text
Expected training activity
```

or:

```text
Suspicious-looking but authorised
```

Students should not classify the approved simulator as malware.

---

## Common Student Mistakes

| Mistake | Instructor Guidance |
|---|---|
| Student cannot find capstone briefing file | Have them use the default briefing |
| Student uses too narrow a time range | Use Last 7 days or Last 30 days |
| Student searches only one field | Have them try process.name, process.executable, and message |
| Student does not open event details | Require timestamp, user, process, parent process, and event ID |
| Student misses related commands | Require searches for whoami, hostname, and ipconfig |
| Student lists fewer than five indicators | Have them use host, process, path, command, and event ID |
| Student writes only technical detail in executive summary | Ask for short non-technical summary |
| Student classifies simulator as malware | Remind them it is approved safe lab activity |
| Student omits response recommendations | Require containment, recovery, and escalation notes |
| Student saves report with wrong filename | Require `Lab9-Final-Incident-Report.md` |

---

## Grading Guidance

Suggested grading allocation:

| Criteria | Points |
|---|---:|
| Environment verification completed | 10 |
| Case briefing reviewed | 5 |
| Kibana Discover investigation completed | 15 |
| Key event details identified | 15 |
| Related activity searches completed | 15 |
| Five indicators identified | 10 |
| Timeline completed | 10 |
| Executive and technical summaries completed | 10 |
| Response recommendations completed | 5 |
| Final report and screenshots completed | 5 |
| Total | 100 |

---

## Free Elastic Basic License Reminder

This lab must use:

- Self-managed Elastic.
- Free Elastic Basic license.
- Kibana Discover.
- Windows and Sysmon event collection.
- Safe simulated endpoint activity only.
- Optional prepared capstone evidence only.
- No Elastic Cloud.
- No paid subscriptions.
- No external internet access.

---

## Fallback Option if Live Events Are Unavailable

If live events are unavailable, students may use prepared evidence files if present:

```text
C:\LabFiles\Logs\capstone-events.csv
```

or:

```text
/home/student/labfiles/capstone/capstone-events.csv
```

Suggested fallback evidence line:

```text
Live Elastic events were unavailable. Prepared capstone evidence was reviewed and documented.
```

The preferred method is live Kibana Discover evidence when available.

---

End of Lab 09.
