# Lab 06 - Analysing Simulated Endpoint Activity

## Estimated Time

90–120 minutes

---

## Lab Purpose

In this lab, you will run a safe educational activity generator on WIN11-CLIENT and investigate the events it creates.

You will verify simulator output on the Windows endpoint, review local Windows and Sysmon evidence, search for simulator activity in Kibana, identify related child processes, and create an event timeline.

This lab helps BlueWave Clinic practise endpoint investigation using safe simulated activity.

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

By the end of this lab, you will be able to:

- Explain the purpose of the BlueWave Activity Simulator.
- Verify that the simulator file exists on WIN11-CLIENT.
- Run the simulator safely.
- Confirm the simulator output folder and output file were created.
- Review local endpoint evidence in Windows and Sysmon logs.
- Search for simulator activity in Kibana Discover.
- Identify the simulator process.
- Identify child processes created by the simulator.
- Identify the user account and timestamps associated with activity.
- Build an investigation timeline.
- Decide whether the activity appears normal, suspicious-looking, or malicious.
- Document investigation findings.

---

## Scenario

BlueWave Clinic wants junior analysts to practise investigating endpoint activity without using real malware or unsafe tools.

The instructor has provided a safe educational activity generator named:

```text
BlueWaveActivitySimulator.exe
```

The simulator is used only to create harmless Windows activity for SOC analysis.

It may perform safe actions such as:

- Create a local folder.
- Create a local text file.
- Start Notepad.
- Start Calculator.
- Run `whoami`.
- Run `hostname`.
- Run `ipconfig`.
- Make a local HTTP request to the Ubuntu web server if configured.
- Write a harmless completion message.

Your job is to run the simulator and investigate the resulting endpoint events in Kibana.

> [!note]
> The simulator is not malware.

> [!note]
> The simulator is a safe educational activity generator.

> [!alert]
> Do not rename the simulator, modify it, reverse engineer it, or use it outside the lab environment.

> [!alert]
> Do not run offensive tools, malware, exploit code, credential theft tools, password cracking tools, brute-force tools, or internet scanning tools.

---

## Required Machines

| Machine | Used For |
|---|---|
| WIN11-CLIENT | Run the safe simulator, verify local output, review evidence, create notes |
| UBUNTU-SOC | Elasticsearch and Kibana services |
| Kibana browser session on WIN11-CLIENT | Search and analyse simulator-related events |

---

## Required Files

| File | Location | Purpose |
|---|---|---|
| BlueWaveActivitySimulator.exe | `C:\LabFiles\Simulators` | Safe educational activity generator |
| timeline-template.md | `C:\LabFiles\Templates` | Optional event timeline template |
| Lab06-Simulator-Investigation.txt | `C:\BlueWave\Evidence` | Student-created investigation notes |
| Lab06-Simulator-Timeline.txt | `C:\BlueWave\Evidence` | Student-created simulator timeline |

---

## Important Paths

### Windows Paths

| Path | Purpose |
|---|---|
| `C:\LabFiles\Simulators` | Simulator folder |
| `C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe` | Safe simulator executable |
| `C:\BlueWave` | BlueWave working folder |
| `C:\BlueWave\Evidence` | Evidence folder |
| `C:\BlueWave\SimulatorOutput` | Simulator output folder |
| `C:\BlueWave\SimulatorOutput\activity-note.txt` | Simulator output note |
| `C:\BlueWave\Evidence\Lab06-Simulator-Investigation.txt` | Lab 06 investigation notes |
| `C:\BlueWave\Evidence\Lab06-Simulator-Timeline.txt` | Lab 06 timeline |

### Windows Event Log Path

| Log | Purpose |
|---|---|
| `Applications and Services Logs > Microsoft > Windows > Sysmon > Operational` | Local Sysmon process activity |
| `Windows Logs > Application` | Local application-related events |
| `Windows Logs > System` | Local system-related events |

### Kibana Areas

| Kibana Area | Purpose |
|---|---|
| Discover | Search simulator-related events |
| Data view selector | Select logs data |
| Time picker | Set investigation time range |
| KQL query bar | Enter simulator and process queries |
| Event details panel | Review process fields |

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
| Windows evidence folder | `C:\BlueWave\Evidence` |
| Logs data view | `logs-*` |
| Sysmon status | Running |
| Elastic Agent status | Running |

> [!note]
> Your Ubuntu SOC IP address may be different. Use the actual IP address from your lab environment.

---

## Screenshots You Should Capture

Capture screenshots as instructed by your trainer or Skillable platform.

Recommended screenshots:

1. Simulator file in `C:\LabFiles\Simulators`.
2. PowerShell command used to run the simulator.
3. Simulator output folder.
4. `activity-note.txt` contents.
5. Recent local Sysmon events after simulator execution.
6. Kibana Discover showing simulator process activity.
7. Kibana Discover showing child process activity.
8. Kibana event details for the simulator process.
9. Completed simulator timeline.
10. Completed Lab 06 investigation notes file.

---

## Key Terms

| Term | Meaning |
|---|---|
| Simulator | A safe educational program used to create harmless lab activity |
| Endpoint activity | Actions that occur on a workstation or server |
| Process | A running program |
| Parent process | The process that starts another process |
| Child process | A process started by another process |
| Command line | The command and arguments used to start a process |
| Sysmon Event ID 1 | Sysmon Process Create event |
| Event timeline | A list of events arranged by time |
| Indicator | A clue or data point useful during investigation |
| Triage | The first review of an event to decide what action is needed |
| Suspicious-looking | Activity that deserves review but is not automatically malicious |
| Malicious | Activity that is harmful or unauthorised |

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
5. Confirm the result.

## Expected Result

The hostname should usually be:

```text
WIN11-CLIENT
```

If your hostname is different, record the exact value.

## Screenshot Checkpoint

Capture a screenshot of the hostname result if required.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Windows hostname:
```

---

# Task 2 - Confirm the Evidence Folder Exists

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. In PowerShell, check the evidence folder.

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

## Expected Result

The folder check should return:

```text
True
```

## Screenshot Checkpoint

Capture a screenshot if your instructor requires evidence folder validation.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Windows evidence folder confirmed:
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

## Record in Evidence Notes

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

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Sysmon service status:
```

---

# Task 5 - Review the Simulator Safety Statement

## Where to Work

Use **WIN11-CLIENT**.

Use this lab document.

## Steps

1. Read the simulator description carefully.
2. Confirm that the simulator is safe and educational.
3. Confirm that the simulator must only be used inside this lab.
4. Do not modify the simulator.
5. Do not copy the simulator outside the lab environment.

## Simulator Safety Statement

### Student Input - Copy or Type

```text
BlueWaveActivitySimulator.exe is a safe educational activity generator used to create harmless Windows events for SOC analysis. It is not malware. It must only be used inside the lab environment.
```

## Expected Result

You should understand that the simulator is safe, controlled, and defensive.

## Screenshot Checkpoint

No screenshot is required for this task unless instructed.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Simulator safety statement reviewed:
Simulator described as malware:
```

Example:

```text
Simulator safety statement reviewed: Yes
Simulator described as malware: No
```

---

# Task 6 - Locate the Simulator File

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Check whether the simulator folder exists.

### Student Input - Copy or Type

```powershell
Test-Path "C:\LabFiles\Simulators"
```

2. Press **Enter**.
3. List the simulator folder.

### Student Input - Copy or Type

```powershell
Get-ChildItem "C:\LabFiles\Simulators"
```

4. Press **Enter**.
5. Look for the simulator file.

### Student Input - Copy or Type

```text
BlueWaveActivitySimulator.exe
```

6. Confirm the full simulator path.

### Student Input - Copy or Type

```powershell
Test-Path "C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe"
```

7. Press **Enter**.

## Expected Result

The simulator should exist at:

```text
C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe
```

The `Test-Path` command should return:

```text
True
```

## Screenshot Checkpoint

Capture a screenshot showing the simulator file in the folder.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Simulator file found:
Simulator file path:
```

Example:

```text
Simulator file found: Yes
Simulator file path: C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe
```

---

# Task 7 - Record the Current Time Before Running the Simulator

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Record the current system time.

### Student Input - Copy or Type

```powershell
Get-Date
```

2. Press **Enter**.
3. Record the displayed time.

## Expected Result

PowerShell should display the current date and time.

Example:

```text
Friday, 15 May 2026 10:30:00 AM
```

> [!note]
> This timestamp helps you search for events around the time the simulator ran.

## Screenshot Checkpoint

Capture a screenshot showing the time before simulator execution.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Simulator pre-run timestamp:
```

---

# Task 8 - Run the BlueWave Activity Simulator

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Make sure you are on WIN11-CLIENT.
2. Make sure the simulator file exists.
3. Run the simulator.

### Student Input - Copy or Type

```powershell
Start-Process -FilePath "C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe"
```

4. Press **Enter**.
5. Wait for the simulator to complete.
6. If Notepad or Calculator opens, that is expected.
7. Do not stop Elastic Agent or Sysmon.
8. Do not modify system security settings.

## Expected Result

The simulator should run normally.

It may create a folder and file, start safe applications, run simple commands, and exit normally.

Expected safe activity may include:

```text
C:\BlueWave\SimulatorOutput
C:\BlueWave\SimulatorOutput\activity-note.txt
notepad.exe
calc.exe
whoami
hostname
ipconfig
```

> [!note]
> The exact visible behaviour depends on how the simulator was prepared by the lab builder.

## Screenshot Checkpoint

Capture a screenshot showing the PowerShell command used to run the simulator.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Simulator run command:
Simulator executed:
Visible activity observed:
```

---

# Task 9 - Record the Current Time After Running the Simulator

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Record the current system time after the simulator runs.

### Student Input - Copy or Type

```powershell
Get-Date
```

2. Press **Enter**.
3. Record the displayed time.

## Expected Result

PowerShell should display the current date and time.

This timestamp marks the end of the simulator activity window.

## Screenshot Checkpoint

Capture a screenshot showing the time after simulator execution.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Simulator post-run timestamp:
```

---

# Task 10 - Confirm the Simulator Output Folder Was Created

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
3. List the BlueWave folder contents.

### Student Input - Copy or Type

```powershell
Get-ChildItem "C:\BlueWave"
```

4. Press **Enter**.

## Expected Result

The folder check should return:

```text
True
```

You should see:

```text
SimulatorOutput
```

## Screenshot Checkpoint

Capture a screenshot showing the simulator output folder.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Simulator output folder created:
Simulator output folder path:
```

---

# Task 11 - Confirm the Simulator Output File Was Created

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Check whether the simulator note file exists.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

2. Press **Enter**.
3. Display the file contents.

### Student Input - Copy or Type

```powershell
Get-Content "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

4. Press **Enter**.

## Expected Result

The file check should return:

```text
True
```

The file should contain a harmless message such as:

```text
BlueWave simulator activity completed
```

> [!note]
> The exact message may vary, but it should be harmless.

## Screenshot Checkpoint

Capture a screenshot showing the output file contents.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Simulator output file created:
Simulator output file path:
Simulator output file message:
```

---

# Task 12 - Review Recent Sysmon Events Locally

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Query recent Sysmon events.

### Student Input - Copy or Type

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 20 | Select-Object TimeCreated, Id, ProviderName, Message
```

2. Press **Enter**.
3. Look for events near the simulator execution time.
4. Look for Event ID 1 if visible.

## Expected Result

You should see recent Sysmon events.

Some events may include:

```text
Id: 1
ProviderName: Microsoft-Windows-Sysmon
```

> [!note]
> Sysmon Event ID 1 is Process Create.

## Screenshot Checkpoint

Capture a screenshot showing recent Sysmon events after simulator execution.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Recent Sysmon events after simulator:
Sysmon Event ID 1 visible after simulator:
```

---

# Task 13 - Open Event Viewer and Review Sysmon Operational Events

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows Event Viewer**.

## Steps

1. Select the **Windows Start** menu.
2. Search for **Event Viewer**.

### Student Input - Copy or Type

```text
Event Viewer
```

3. Open **Event Viewer**.
4. Navigate to the Sysmon Operational log.

### Student Input - Copy or Type

```text
Applications and Services Logs > Microsoft > Windows > Sysmon > Operational
```

5. Look for recent events near the simulator execution time.
6. Select one recent event.
7. Review the event details.

## Expected Result

You should see recent Sysmon events.

Some events may be process creation events.

## Screenshot Checkpoint

Capture a screenshot showing a recent Sysmon event in Event Viewer.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Event Viewer Sysmon log reviewed:
Recent Sysmon event ID observed:
Recent Sysmon event time:
```

---

# Task 14 - Open Kibana from Windows

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

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Kibana URL used:
Kibana opened successfully:
```

---

# Task 15 - Open Kibana Discover

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana** in the browser.

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

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Discover opened:
```

---

# Task 16 - Select the Logs Data View and Time Range

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Locate the data view selector in Discover.
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

3. Set the time range to **Last 24 hours**.

### Student Input - Copy or Type

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

> [!hint]
> If no events appear later, try expanding the time range to `Last 7 days`.

## Screenshot Checkpoint

Capture a screenshot showing the selected data view and time range.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Data view selected:
Time range used:
```

---

# Task 17 - Search for Events from WIN11-CLIENT

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Click the Kibana query bar.
2. Enter the host query.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT"
```

3. Press **Enter** or select **Update**.
4. Review the results.

If no results appear, try this alternate query:

### Student Input - Copy or Type

```text
agent.name : "WIN11-CLIENT"
```

## Expected Result

Events from WIN11-CLIENT should appear in Discover.

## Screenshot Checkpoint

Capture a screenshot showing events from WIN11-CLIENT.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Host query used:
Events from WIN11-CLIENT found:
```

---

# Task 18 - Search for the Simulator Process

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for the simulator process by name.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try the process executable field:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.executable : *BlueWaveActivitySimulator*
```

If still no results appear, try a broader message search:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *BlueWaveActivitySimulator*
```

## Expected Result

You should find the simulator process event.

Expected process name:

```text
BlueWaveActivitySimulator.exe
```

## Screenshot Checkpoint

Capture a screenshot showing the simulator process in Kibana.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Simulator process query used:
Simulator process found:
Simulator process timestamp:
Simulator process user:
Simulator process parent process:
Simulator process command line:
```

---

# Task 19 - Search for Simulator Sysmon Event ID 1

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for the simulator process as a Sysmon process creation event.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon" and event.code : "1" and process.name : "BlueWaveActivitySimulator.exe"
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try this alternate event ID field:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and winlog.event_id : 1 and message : *BlueWaveActivitySimulator*
```

## Expected Result

You should find a Sysmon Event ID 1 process creation event for the simulator.

> [!note]
> Sysmon Event ID 1 means Process Create.

## Screenshot Checkpoint

Capture a screenshot showing the simulator Sysmon Event ID 1 event.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Simulator Sysmon Event ID 1 query used:
Simulator Sysmon Event ID 1 found:
```

---

# Task 20 - Search for Child Process Activity: Notepad

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for Notepad process activity.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "notepad.exe"
```

2. Press **Enter** or select **Update**.
3. Review the parent process if visible.

If no results appear, try this broader query:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *notepad*
```

## Expected Result

You may find Notepad activity.

Expected process name:

```text
notepad.exe
```

If the simulator launched Notepad, the parent process may be:

```text
BlueWaveActivitySimulator.exe
```

or another process depending on the lab design.

## Screenshot Checkpoint

Capture a screenshot showing Notepad process activity or the query attempt.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Notepad child process found:
Notepad timestamp:
Notepad parent process:
Notepad command line:
```

---

# Task 21 - Search for Child Process Activity: Calculator

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for Calculator process activity.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "calc.exe"
```

2. Press **Enter** or select **Update**.
3. Review the parent process if visible.

If no results appear, try this alternate query:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : *Calculator*
```

If still no results appear, try this broader query:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *calc*
```

## Expected Result

You may find Calculator-related activity.

Possible process names include:

```text
calc.exe
CalculatorApp.exe
```

> [!note]
> Calculator may appear differently depending on the Windows version.

## Screenshot Checkpoint

Capture a screenshot showing Calculator process activity or the query attempt.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Calculator child process found:
Calculator process name observed:
Calculator timestamp:
Calculator parent process:
Calculator command line:
```

---

# Task 22 - Search for Child Process Activity: whoami

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for `whoami` command-line activity.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.command_line : *whoami*
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try this alternate query:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "whoami.exe"
```

If still no results appear, try this broader query:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *whoami*
```

## Expected Result

You may find `whoami` activity.

Expected process name:

```text
whoami.exe
```

## Screenshot Checkpoint

Capture a screenshot showing `whoami` process activity or the query attempt.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
whoami child process found:
whoami timestamp:
whoami parent process:
whoami command line:
whoami user:
```

---

# Task 23 - Search for Child Process Activity: hostname

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for `hostname` command-line activity.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.command_line : *hostname*
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try this alternate query:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "hostname.exe"
```

If still no results appear, try this broader query:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *hostname*
```

## Expected Result

You may find `hostname` activity.

Expected process name:

```text
hostname.exe
```

## Screenshot Checkpoint

Capture a screenshot showing `hostname` process activity or the query attempt.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
hostname child process found:
hostname timestamp:
hostname parent process:
hostname command line:
hostname user:
```

---

# Task 24 - Search for Child Process Activity: ipconfig

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for `ipconfig` command-line activity.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.command_line : *ipconfig*
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try this alternate query:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "ipconfig.exe"
```

If still no results appear, try this broader query:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *ipconfig*
```

## Expected Result

You may find `ipconfig` activity.

Expected process name:

```text
ipconfig.exe
```

## Screenshot Checkpoint

Capture a screenshot showing `ipconfig` process activity or the query attempt.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
ipconfig child process found:
ipconfig timestamp:
ipconfig parent process:
ipconfig command line:
ipconfig user:
```

---

# Task 25 - Search for BlueWave Output File Activity

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for the simulator output path.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *SimulatorOutput*
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try searching for the output file name:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *activity-note.txt*
```

If still no results appear, try searching for the BlueWave keyword:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *BlueWave*
```

## Expected Result

You may find events referencing:

```text
C:\BlueWave\SimulatorOutput
activity-note.txt
BlueWave
```

> [!note]
> File creation visibility depends on Sysmon configuration. It is acceptable if file activity does not appear, as long as process activity is visible.

## Screenshot Checkpoint

Capture a screenshot showing file-related activity or the query attempt.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Simulator output file activity found in Kibana:
File activity query used:
File activity result:
```

---

# Task 26 - Search for Possible Local HTTP Activity

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for possible HTTP activity related to the simulator.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *http*
```

2. Press **Enter** or select **Update**.
3. Review the results.

If the Ubuntu SOC IP is known, try searching for it.

Replace `<UBUNTU-SOC-IP>` with the Ubuntu IP address.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *<UBUNTU-SOC-IP>*
```

Example:

```text
host.name : "WIN11-CLIENT" and message : *10.1.1.10*
```

## Expected Result

You may or may not find local HTTP activity.

This depends on how the simulator and lab network were prepared.

> [!note]
> If no HTTP activity appears, record that no HTTP-related event was found.

## Screenshot Checkpoint

Capture a screenshot of the HTTP activity search or result if required.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Possible HTTP activity searched:
HTTP activity found:
HTTP activity query used:
```

---

# Task 27 - Open the Simulator Process Event Details

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Run the simulator process query again.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

2. Press **Enter** or select **Update**.
3. Select the simulator event from the results table.
4. Open the event details.
5. Review the fields.

Look for these fields:

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
```

## Expected Result

You should be able to identify key details for the simulator process.

## Screenshot Checkpoint

Capture a screenshot showing the simulator event details.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Selected simulator event timestamp:
Selected simulator process name:
Selected simulator process path:
Selected simulator command line:
Selected simulator parent process:
Selected simulator username:
Selected simulator event ID:
```

---

# Task 28 - Decide How to Classify the Activity

## Where to Work

Use **WIN11-CLIENT**.

Use your evidence notes.

## Steps

1. Review the simulator process.
2. Review any child processes found.
3. Review the output folder and output file.
4. Decide whether the activity is:

### Student Input - Copy or Type

```text
Normal
Suspicious-looking
Malicious
```

5. Write a short explanation.

## Expected Result

For this lab, the expected classification is usually:

```text
Suspicious-looking but not malicious
```

Reason:

```text
The simulator intentionally creates activity that analysts should investigate, but it is safe and educational.
```

> [!note]
> The simulator is not malware. It is safe lab activity.

## Screenshot Checkpoint

No screenshot is required for this task unless instructed.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Activity classification:
Classification explanation:
```

---

# Task 29 - Create the Lab 06 Simulator Timeline

## Where to Work

Use **WIN11-CLIENT**.

Use **Notepad**.

## Steps

1. Open **Notepad**.
2. Copy or type the simulator timeline template below.
3. Fill in the timeline using evidence from PowerShell, Event Viewer, and Kibana.
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
Lab06-Simulator-Timeline.txt
```

## Timeline Template

### Student Input - Copy or Type

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 06 - Simulator Event Timeline

Student Name:
Date:

Investigation Window

Simulator pre-run timestamp:
Simulator post-run timestamp:

Timeline Entry 1
Timestamp:
Source:
Host:
User:
Event type:
Process name:
Command line:
Parent process:
Event ID:
Notes:

Timeline Entry 2
Timestamp:
Source:
Host:
User:
Event type:
Process name:
Command line:
Parent process:
Event ID:
Notes:

Timeline Entry 3
Timestamp:
Source:
Host:
User:
Event type:
Process name:
Command line:
Parent process:
Event ID:
Notes:

Timeline Entry 4
Timestamp:
Source:
Host:
User:
Event type:
Process name:
Command line:
Parent process:
Event ID:
Notes:

Timeline Entry 5
Timestamp:
Source:
Host:
User:
Event type:
Process name:
Command line:
Parent process:
Event ID:
Notes:

Timeline Summary
Write 3 to 5 sentences explaining the order of events and what the simulator did.
```

## Expected Result

The timeline file should be saved at:

```text
C:\BlueWave\Evidence\Lab06-Simulator-Timeline.txt
```

## Screenshot Checkpoint

Capture a screenshot showing the completed simulator timeline.

---

# Task 30 - Create the Lab 06 Investigation Notes File

## Where to Work

Use **WIN11-CLIENT**.

Use **Notepad**.

## Steps

1. Open **Notepad**.
2. Copy or type the investigation notes template below.
3. Fill in the missing information using your lab results.
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
Lab06-Simulator-Investigation.txt
```

## Investigation Notes Template

### Student Input - Copy or Type

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 06 - Analysing Simulated Endpoint Activity

Student Name:
Date:

1. Environment Verification

Windows hostname:
Windows evidence folder confirmed:
Elastic Agent service status:
Sysmon service status:

2. Simulator Safety

Simulator safety statement reviewed:
Simulator described as malware:

3. Simulator File Verification

Simulator file found:
Simulator file path:
Simulator pre-run timestamp:
Simulator run command:
Simulator executed:
Visible activity observed:
Simulator post-run timestamp:

4. Simulator Output

Simulator output folder created:
Simulator output folder path:
Simulator output file created:
Simulator output file path:
Simulator output file message:

5. Local Event Review

Recent Sysmon events after simulator:
Sysmon Event ID 1 visible after simulator:
Event Viewer Sysmon log reviewed:
Recent Sysmon event ID observed:
Recent Sysmon event time:

6. Kibana Setup

Kibana URL used:
Kibana opened successfully:
Discover opened:
Data view selected:
Time range used:
Host query used:
Events from WIN11-CLIENT found:

7. Simulator Process Investigation

Simulator process query used:
Simulator process found:
Simulator process timestamp:
Simulator process user:
Simulator process parent process:
Simulator process command line:
Simulator Sysmon Event ID 1 query used:
Simulator Sysmon Event ID 1 found:

8. Child Process Investigation

Notepad child process found:
Notepad timestamp:
Notepad parent process:
Notepad command line:

Calculator child process found:
Calculator process name observed:
Calculator timestamp:
Calculator parent process:
Calculator command line:

whoami child process found:
whoami timestamp:
whoami parent process:
whoami command line:
whoami user:

hostname child process found:
hostname timestamp:
hostname parent process:
hostname command line:
hostname user:

ipconfig child process found:
ipconfig timestamp:
ipconfig parent process:
ipconfig command line:
ipconfig user:

9. File and Network Activity

Simulator output file activity found in Kibana:
File activity query used:
File activity result:
Possible HTTP activity searched:
HTTP activity found:
HTTP activity query used:

10. Selected Simulator Event

Selected simulator event timestamp:
Selected simulator process name:
Selected simulator process path:
Selected simulator command line:
Selected simulator parent process:
Selected simulator username:
Selected simulator event ID:

11. Assessment

Activity classification:
Classification explanation:

12. Lab Summary

Write 3 to 5 sentences explaining what you found during the simulator investigation.
```

## Expected Result

The investigation notes file should be saved at:

```text
C:\BlueWave\Evidence\Lab06-Simulator-Investigation.txt
```

## Screenshot Checkpoint

Capture a screenshot showing the completed investigation notes file.

---

# Task 31 - Confirm Lab 06 Evidence Files Exist

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Confirm the timeline file exists.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\Evidence\Lab06-Simulator-Timeline.txt"
```

2. Press **Enter**.
3. Confirm the investigation notes file exists.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\Evidence\Lab06-Simulator-Investigation.txt"
```

4. Press **Enter**.

## Expected Result

Both commands should return:

```text
True
```

## Screenshot Checkpoint

Capture a screenshot showing both validation commands returning `True`.

---

# Task 32 - Final Validation

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana**, **PowerShell**, and **File Explorer**.

## Steps

1. Confirm the simulator file exists.
2. Confirm the simulator was run.
3. Confirm the simulator output folder exists.
4. Confirm the simulator output file exists.
5. Confirm local Sysmon events were reviewed.
6. Confirm Kibana was used to search for the simulator.
7. Confirm child process searches were completed.
8. Confirm the timeline file exists.
9. Confirm the investigation notes file exists.

## Expected Result

You should have:

```text
Simulator file verified
Simulator executed
Simulator output folder created
Simulator output file created
Local Sysmon evidence reviewed
Simulator process found or searched in Kibana
Child process activity found or searched
Timeline completed
Investigation notes completed
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
- [ ] I reviewed the simulator safety statement.
- [ ] I confirmed the simulator file exists.
- [ ] I recorded the pre-run timestamp.
- [ ] I ran the simulator.
- [ ] I recorded the post-run timestamp.
- [ ] I confirmed the output folder was created.
- [ ] I confirmed the output file was created.
- [ ] I reviewed the output file contents.
- [ ] I reviewed recent local Sysmon events.
- [ ] I opened Event Viewer and reviewed Sysmon events.
- [ ] I opened Kibana.
- [ ] I opened Discover.
- [ ] I selected a logs data view.
- [ ] I set the time range to Last 24 hours.
- [ ] I searched for events from WIN11-CLIENT.
- [ ] I searched for the simulator process.
- [ ] I searched for simulator Sysmon Event ID 1.
- [ ] I searched for Notepad child process activity.
- [ ] I searched for Calculator child process activity.
- [ ] I searched for `whoami` child process activity.
- [ ] I searched for `hostname` child process activity.
- [ ] I searched for `ipconfig` child process activity.
- [ ] I searched for simulator output file activity.
- [ ] I searched for possible local HTTP activity.
- [ ] I opened the simulator process event details.
- [ ] I classified the activity.
- [ ] I created `Lab06-Simulator-Timeline.txt`.
- [ ] I created `Lab06-Simulator-Investigation.txt`.
- [ ] I captured the required screenshots.

---

# Troubleshooting

## Problem: The simulator file is missing

Check the simulator folder.

### Student Input - Copy or Type

```powershell
Get-ChildItem "C:\LabFiles\Simulators"
```

Look for:

```text
BlueWaveActivitySimulator.exe
```

If the file is missing, notify your instructor.

Do not download or create a replacement simulator.

---

## Problem: The simulator does not run

Check the exact path.

### Student Input - Copy or Type

```powershell
Test-Path "C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe"
```

Expected result:

```text
True
```

Try running it again:

### Student Input - Copy or Type

```powershell
Start-Process -FilePath "C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe"
```

If it still does not run, ask your instructor.

---

## Problem: The output folder was not created

Check the output folder path.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\SimulatorOutput"
```

If the result is `False`, check whether the simulator ran successfully.

Ask your instructor before manually creating the folder.

---

## Problem: The output file was not created

Check the output file path.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

If the result is `False`, record the issue and ask your instructor.

---

## Problem: Elastic Agent is not running

Check the Elastic Agent service.

### Student Input - Copy or Type

```powershell
Get-Service elastic-agent
```

If the service is not running, review Lab 03 or ask your instructor.

---

## Problem: Sysmon is not running

Check both possible Sysmon service names.

### Student Input - Copy or Type

```powershell
Get-Service Sysmon64 -ErrorAction SilentlyContinue
```

### Student Input - Copy or Type

```powershell
Get-Service Sysmon -ErrorAction SilentlyContinue
```

If Sysmon is not running, review Lab 05 or ask your instructor.

---

## Problem: No recent Sysmon events appear locally

Try this command again:

### Student Input - Copy or Type

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 20 | Select-Object TimeCreated, Id, ProviderName, Message
```

If no events appear, ask your instructor to verify Sysmon configuration.

---

## Problem: Kibana does not open

Check the Kibana URL.

### Student Input - Copy or Type

```text
http://<UBUNTU-SOC-IP>:5601
```

If Kibana still does not open, confirm Kibana is running or ask your instructor.

---

## Problem: No simulator events appear in Kibana

First check the time range.

### Student Input - Copy or Type

```text
Last 24 hours
```

Try expanding to:

### Student Input - Copy or Type

```text
Last 7 days
```

Try alternate simulator queries:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.executable : *BlueWaveActivitySimulator*
```

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *BlueWaveActivitySimulator*
```

---

## Problem: Child processes do not appear

Try broader searches.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *notepad*
```

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *calc*
```

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

---

## Problem: File activity does not appear in Kibana

File creation visibility depends on Sysmon configuration.

Confirm the file exists locally:

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

Then record that the local file exists even if the file event is not visible in Kibana.

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

## Problem: Evidence files are missing

Check the evidence folder.

### Student Input - Copy or Type

```powershell
Get-ChildItem "C:\BlueWave\Evidence"
```

Confirm these files exist:

```text
Lab06-Simulator-Investigation.txt
Lab06-Simulator-Timeline.txt
```

If missing, recreate them using Task 29 and Task 30.

---

# Knowledge Check

Answer the following questions.

1. What is the purpose of `BlueWaveActivitySimulator.exe`?
2. Is the simulator malware?
3. What folder should the simulator create?
4. What file should the simulator create?
5. What Kibana query can search for the simulator process by name?
6. What Sysmon Event ID represents process creation?
7. Why is the parent process useful during an investigation?
8. Name three child processes or commands you searched for in this lab.
9. Why might local file activity not appear in Kibana?
10. Why should the simulator activity be classified as suspicious-looking but not malicious?

---

# Summary

In this lab, you completed the following tasks:

- Reviewed the simulator safety statement.
- Verified the simulator file exists.
- Ran the safe BlueWave Activity Simulator.
- Confirmed the simulator output folder was created.
- Confirmed the simulator output file was created.
- Reviewed local Sysmon events.
- Opened Kibana Discover.
- Searched for the simulator process.
- Searched for Sysmon Event ID 1.
- Searched for child process activity.
- Reviewed simulator event details.
- Classified the activity.
- Created a simulator timeline.
- Created investigation notes.

You are now ready for Lab 07, where you will create simple Elastic detection logic for suspicious-looking simulated activity.

---

# Deliverables

Submit or retain the following items as directed by your instructor.

| Deliverable | Location |
|---|---|
| Lab 06 simulator investigation notes | `C:\BlueWave\Evidence\Lab06-Simulator-Investigation.txt` |
| Lab 06 simulator timeline | `C:\BlueWave\Evidence\Lab06-Simulator-Timeline.txt` |
| Screenshot of simulator file in `C:\LabFiles\Simulators` | Skillable submission area |
| Screenshot of simulator run command | Skillable submission area |
| Screenshot of simulator output folder | Skillable submission area |
| Screenshot of `activity-note.txt` contents | Skillable submission area |
| Screenshot of recent local Sysmon events | Skillable submission area |
| Screenshot of simulator process in Kibana | Skillable submission area |
| Screenshot of simulator Sysmon Event ID 1 | Skillable submission area |
| Screenshot of child process activity | Skillable submission area |
| Screenshot of selected simulator event details | Skillable submission area |
| Screenshot of completed simulator timeline | Skillable submission area |
| Screenshot of completed investigation notes | Skillable submission area |

---

# Instructor Notes

## Expected Knowledge Check Answers

1. The simulator creates safe, harmless endpoint activity for SOC analysis.
2. No. The simulator is not malware.
3. The simulator should create:

```text
C:\BlueWave\SimulatorOutput
```

4. The simulator should create:

```text
C:\BlueWave\SimulatorOutput\activity-note.txt
```

5. A useful query is:

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

6. Sysmon Event ID 1 represents Process Create.
7. The parent process helps explain what started another process.
8. Examples include `notepad.exe`, `calc.exe`, `whoami.exe`, `hostname.exe`, and `ipconfig.exe`.
9. File activity visibility depends on Sysmon configuration and the fields collected by Elastic.
10. The activity is designed to look investigation-worthy but is known safe lab activity.

---

## Expected Evidence Files

Students should create:

```text
C:\BlueWave\Evidence\Lab06-Simulator-Investigation.txt
```

and:

```text
C:\BlueWave\Evidence\Lab06-Simulator-Timeline.txt
```

---

## Expected Windows Commands

Verify simulator file:

```powershell
Test-Path "C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe"
```

Run simulator:

```powershell
Start-Process -FilePath "C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe"
```

Check output folder:

```powershell
Test-Path "C:\BlueWave\SimulatorOutput"
```

Check output file:

```powershell
Test-Path "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

Read output file:

```powershell
Get-Content "C:\BlueWave\SimulatorOutput\activity-note.txt"
```

Review recent Sysmon events:

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 20 | Select-Object TimeCreated, Id, ProviderName, Message
```

---

## Expected Elastic Queries

Host query:

```text
host.name : "WIN11-CLIENT"
```

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

Simulator Sysmon Event ID 1 query:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon" and event.code : "1" and process.name : "BlueWaveActivitySimulator.exe"
```

Alternate simulator Event ID 1 query:

```text
host.name : "WIN11-CLIENT" and winlog.event_id : 1 and message : *BlueWaveActivitySimulator*
```

Child process queries:

```text
host.name : "WIN11-CLIENT" and process.name : "notepad.exe"
```

```text
host.name : "WIN11-CLIENT" and process.name : "calc.exe"
```

```text
host.name : "WIN11-CLIENT" and process.command_line : *whoami*
```

```text
host.name : "WIN11-CLIENT" and process.command_line : *hostname*
```

```text
host.name : "WIN11-CLIENT" and process.command_line : *ipconfig*
```

File activity queries:

```text
host.name : "WIN11-CLIENT" and message : *SimulatorOutput*
```

```text
host.name : "WIN11-CLIENT" and message : *activity-note.txt*
```

```text
host.name : "WIN11-CLIENT" and message : *BlueWave*
```

---

## Expected Visible Results

Students should be able to show:

- Simulator file exists.
- Simulator was run.
- Simulator output folder exists.
- Simulator output file exists.
- Simulator output file contains a harmless message.
- Local Sysmon events exist after simulator execution.
- Kibana searches were performed.
- Simulator process was found or query attempts were documented.
- Child process activity was found or query attempts were documented.
- Timeline was completed.
- Investigation notes were completed.

---

## Common Student Mistakes

| Mistake | Instructor Guidance |
|---|---|
| Student describes the simulator as malware | Remind them it is safe educational activity |
| Student runs the simulator from the wrong path | Have them verify `C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe` |
| Student forgets pre-run and post-run timestamps | Have them run `Get-Date` and use approximate times |
| Student expects every child process to appear | Explain that visibility depends on configuration and simulator behaviour |
| Student searches only one field | Have them try `process.name`, `process.executable`, and `message` |
| Student ignores parent process | Remind them parent process is important for investigation context |
| Student cannot find file activity | Explain that file activity may not be collected by the current Sysmon config |
| Student marks activity as malicious | Remind them this is safe, authorised lab activity |

---

## Grading Guidance

Suggested grading allocation:

| Criteria | Points |
|---|---:|
| Simulator file verified | 10 |
| Simulator executed safely | 10 |
| Output folder and file verified | 15 |
| Local Sysmon evidence reviewed | 10 |
| Simulator searched in Kibana | 15 |
| Child process searches completed | 15 |
| Event details reviewed | 10 |
| Activity classification completed | 5 |
| Timeline completed | 5 |
| Notes and screenshots completed | 5 |
| Total | 100 |

Do not heavily penalise students if specific child processes do not appear, as long as they use the correct queries, try alternates, and document their findings.

---

## Free Elastic Basic License Reminder

This lab must use:

- Self-managed Elastic.
- Free Elastic Basic license.
- Kibana Discover.
- Windows and Sysmon event collection.
- Safe simulated endpoint activity only.
- No Elastic Cloud.
- No paid subscriptions.
- No external internet access.

---

## Fallback Option if Live Simulator Logs Are Unavailable

If live simulator logs are unavailable, students should still verify:

```text
C:\BlueWave\SimulatorOutput
```

and:

```text
C:\BlueWave\SimulatorOutput\activity-note.txt
```

Suggested fallback evidence line:

```text
Live simulator events were not visible in Kibana. Local simulator output and Sysmon/Event Viewer checks were completed.
```

The preferred method is always live Kibana evidence when available.

---

End of Lab 06.
