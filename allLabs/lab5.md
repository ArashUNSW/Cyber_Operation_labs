# Lab 05 - Collecting Sysmon Process Activity

## Estimated Time

90–120 minutes

---

## Lab Purpose

In this lab, you will collect and review Sysmon process activity from WIN11-CLIENT.

You will confirm that Sysmon is installed, confirm that the Sysmon Operational log exists, generate safe process activity, search for Sysmon Event ID 1 in Kibana, and create a simple process activity timeline.

This lab helps BlueWave Clinic improve endpoint visibility beyond standard Windows Event Logs.

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
> Type commands and queries exactly as shown. Commands and Kibana queries are sensitive to spaces, punctuation, quotation marks, and field names.

---

## Learning Objectives

By the end of this lab, you will be able to:

- Explain why Sysmon is useful for endpoint monitoring.
- Confirm whether Sysmon is installed on WIN11-CLIENT.
- Confirm that the Sysmon Operational log exists.
- Generate safe process activity on Windows.
- Search for Sysmon events in Kibana Discover.
- Identify Sysmon Event ID 1 process creation events.
- Identify process name, parent process, command line, username, and timestamp.
- Build a simple process activity timeline.
- Save evidence for later investigation labs.

---

## Scenario

BlueWave Clinic has started collecting Windows Event Logs in Elastic.

The SOC team now wants better visibility into process activity on the Windows endpoint.

Standard Windows Event Logs provide useful information, but Sysmon can provide more detailed activity such as process creation, parent process, command line, and executable path.

As a junior cyber operations analyst, you will confirm Sysmon is available and use it to analyse safe activity generated on WIN11-CLIENT.

You will generate harmless activity by opening Notepad, opening Calculator, and running simple commands such as `whoami`, `hostname`, and `ipconfig`.

Then you will search for those events in Kibana.

> [!note]
> This lab is defensive only. You will not run malware, exploit code, credential theft tools, password cracking tools, or attack tools.

> [!note]
> The simulator is not required in this lab. The simulator is introduced in Lab 06.

> [!alert]
> Do not download Sysmon or any tools from the internet. Use only the preloaded lab files.

---

## Required Machines

| Machine | Used For |
|---|---|
| WIN11-CLIENT | Sysmon verification, process activity generation, Event Viewer, evidence notes |
| UBUNTU-SOC | Elasticsearch and Kibana services |
| Kibana browser session on WIN11-CLIENT | Searching Sysmon events in Elastic |

---

## Required Files

| File | Location | Purpose |
|---|---|---|
| Sysmon64.exe | `C:\LabFiles\Tools` | Preloaded Sysmon executable |
| sysmon-bluewave.xml | `C:\LabFiles\Tools` | Safe Sysmon configuration file |
| sample-sysmon-events.csv | `C:\LabFiles\Logs` | Optional fallback sample Sysmon events |
| timeline-template.md | `C:\LabFiles\Templates` | Optional process timeline template |
| Lab05-Sysmon-Process-Timeline.txt | `C:\BlueWave\Evidence` | Student-created process timeline |
| Lab05-Sysmon-Notes.txt | `C:\BlueWave\Evidence` | Student-created lab notes |

---

## Important Paths

### Windows Paths

| Path | Purpose |
|---|---|
| `C:\LabFiles\Tools` | Location of Sysmon files |
| `C:\LabFiles\Tools\Sysmon64.exe` | Sysmon executable |
| `C:\LabFiles\Tools\sysmon-bluewave.xml` | Safe Sysmon configuration |
| `C:\LabFiles\Logs\sample-sysmon-events.csv` | Optional fallback Sysmon sample data |
| `C:\LabFiles\Templates\timeline-template.md` | Optional timeline template |
| `C:\BlueWave\Evidence` | Evidence folder |
| `C:\BlueWave\Evidence\Lab05-Sysmon-Notes.txt` | Lab 05 notes file |
| `C:\BlueWave\Evidence\Lab05-Sysmon-Process-Timeline.txt` | Lab 05 timeline file |

### Windows Event Log Path

| Log | Purpose |
|---|---|
| `Applications and Services Logs > Microsoft > Windows > Sysmon > Operational` | Local Sysmon event log |

### Kibana Areas

| Kibana Area | Purpose |
|---|---|
| Discover | Search Sysmon events |
| Data view selector | Select logs data |
| Time picker | Set event search range |
| KQL query bar | Enter Sysmon queries |
| Event details panel | Review fields for process activity |

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

> [!note]
> Your Ubuntu SOC IP address may be different. Use the actual IP address from your lab environment.

---

## Screenshots You Should Capture

Capture screenshots as instructed by your trainer or Skillable platform.

Recommended screenshots:

1. Sysmon service status on WIN11-CLIENT.
2. Sysmon Operational log in Event Viewer.
3. Safe process activity commands in PowerShell.
4. Kibana Discover showing Sysmon events.
5. Kibana Discover showing Sysmon Event ID 1.
6. Kibana showing `notepad.exe` or `calc.exe`.
7. Kibana showing `whoami`, `hostname`, or `ipconfig`.
8. Open Kibana event details showing process fields.
9. Completed process activity timeline.
10. Completed Lab 05 notes file.

---

## Key Terms

| Term | Meaning |
|---|---|
| Sysmon | Microsoft Sysinternals tool that records detailed Windows system activity |
| Sysmon Operational log | Windows event log where Sysmon events are stored |
| Process | A running program |
| Process creation | The start of a new process |
| Parent process | The process that started another process |
| Child process | A process started by another process |
| Command line | The command and arguments used to start a process |
| Event ID | A number that identifies the event type |
| Sysmon Event ID 1 | Sysmon Process Create event |
| Timestamp | The date and time an event occurred |
| KQL | Kibana Query Language |
| Timeline | A list of events arranged in time order |

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

## Record in Evidence Notes

### Copy

```text
Windows hostname:
```

### Type

Type this into your evidence notes, then add the hostname:

```text
Windows hostname:
```

---

# Task 2 - Confirm the Windows Evidence Folder Exists

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. In PowerShell, check the evidence folder.

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

The evidence folder check should return:

```text
True
```

## Screenshot Checkpoint

Capture a screenshot if your instructor requires evidence folder validation.

## Record in Evidence Notes

### Copy

```text
Windows evidence folder confirmed:
```

### Type

Type this into your evidence notes:

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

## Record in Evidence Notes

### Copy

```text
Elastic Agent service status:
```

### Type

Type this into your evidence notes, then add the value:

```text
Elastic Agent service status:
```

---

# Task 4 - Locate the Sysmon Files

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Check the Tools folder.

### Copy

```powershell
Test-Path "C:\LabFiles\Tools"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\LabFiles\Tools"
```

2. Press **Enter**.
3. List the files in the Tools folder.

### Copy

```powershell
Get-ChildItem "C:\LabFiles\Tools"
```

### Type

Type this into PowerShell:

```powershell
Get-ChildItem "C:\LabFiles\Tools"
```

4. Press **Enter**.
5. Look for the Sysmon executable.

### Copy

```text
Sysmon64.exe
```

### Type

Look for this filename:

```text
Sysmon64.exe
```

6. Look for the Sysmon configuration file.

### Copy

```text
sysmon-bluewave.xml
```

### Type

Look for this filename:

```text
sysmon-bluewave.xml
```

## Expected Result

You should see the Sysmon files in:

```text
C:\LabFiles\Tools
```

Expected files:

```text
Sysmon64.exe
sysmon-bluewave.xml
```

> [!alert]
> Do not download Sysmon from the internet. Use the preloaded files only.

## Screenshot Checkpoint

Capture a screenshot showing the Sysmon files in `C:\LabFiles\Tools`.

## Record in Evidence Notes

### Copy

```text
Sysmon executable found:
Sysmon configuration found:
Sysmon files path:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Sysmon executable found:
Sysmon configuration found:
Sysmon files path:
```

---

# Task 5 - Check Whether Sysmon Is Installed

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Check for the Sysmon service.

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
3. If no result appears, try this alternate service name.

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
5. Record whether Sysmon is installed.

## Expected Result

If Sysmon is installed, you may see:

```text
Status   Name      DisplayName
------   ----      -----------
Running  Sysmon64  Sysmon64
```

or:

```text
Status   Name    DisplayName
------   ----    -----------
Running  Sysmon  Sysmon
```

If no result appears, Sysmon may not be installed yet.

> [!note]
> Your instructor may have already installed Sysmon before the lab. If Sysmon is installed, do not reinstall it unless instructed.

## Screenshot Checkpoint

Capture a screenshot showing the Sysmon service status or the empty result.

## Record in Evidence Notes

### Copy

```text
Sysmon service found:
Sysmon service status:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Sysmon service found:
Sysmon service status:
```

---

# Task 6 - Install Sysmon Only If Required

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell as Administrator**.

## Steps

Complete this task only if:

- Sysmon is not installed, or
- your instructor tells you to install Sysmon during the lab.

If Sysmon is already installed, skip to Task 7.

1. Close normal PowerShell if needed.
2. Select the **Windows Start** menu.
3. Search for **PowerShell**.

### Copy

```text
PowerShell
```

### Type

Type this into the Windows search box:

```text
PowerShell
```

4. Right-click **Windows PowerShell**.
5. Select **Run as administrator**.
6. If prompted by User Account Control, select **Yes**.
7. Change to the Tools folder.

### Copy

```powershell
Set-Location "C:\LabFiles\Tools"
```

### Type

Type this into Administrator PowerShell:

```powershell
Set-Location "C:\LabFiles\Tools"
```

8. Press **Enter**.
9. Install Sysmon with the safe BlueWave configuration.

### Copy

```powershell
.\Sysmon64.exe -accepteula -i .\sysmon-bluewave.xml
```

### Type

Type this into Administrator PowerShell:

```powershell
.\Sysmon64.exe -accepteula -i .\sysmon-bluewave.xml
```

10. Press **Enter**.
11. Wait for the installation to complete.

## Expected Result

You should see output indicating that Sysmon was installed.

Example:

```text
Sysmon installed.
SysmonDrv installed.
Starting SysmonDrv.
Sysmon64 started.
```

> [!alert]
> Only use the provided `sysmon-bluewave.xml` configuration. Do not use a configuration from the internet.

> [!note]
> If Sysmon is already installed, do not reinstall it unless instructed.

## Screenshot Checkpoint

Capture a screenshot showing Sysmon installation output if you installed it.

## Record in Evidence Notes

### Copy

```text
Sysmon installation performed:
Sysmon installation result:
```

### Type

Type these lines into your evidence notes, then add your result:

```text
Sysmon installation performed:
Sysmon installation result:
```

---

# Task 7 - Confirm Sysmon Is Running

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

## Screenshot Checkpoint

Capture a screenshot showing Sysmon running.

## Record in Evidence Notes

### Copy

```text
Sysmon running after verification:
```

### Type

Type this into your evidence notes, then add your result:

```text
Sysmon running after verification:
```

---

# Task 8 - Confirm the Sysmon Operational Log Exists

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Check for the Sysmon Operational log.

### Copy

```powershell
Get-WinEvent -ListLog "Microsoft-Windows-Sysmon/Operational"
```

### Type

Type this into PowerShell:

```powershell
Get-WinEvent -ListLog "Microsoft-Windows-Sysmon/Operational"
```

2. Press **Enter**.
3. Review the result.

## Expected Result

You should see details for the Sysmon Operational log.

Example:

```text
LogName                             RecordCount IsEnabled
-------                             ----------- ---------
Microsoft-Windows-Sysmon/Operational       100 True
```

> [!note]
> The record count will vary.

## Screenshot Checkpoint

Capture a screenshot showing the Sysmon Operational log exists.

## Record in Evidence Notes

### Copy

```text
Sysmon Operational log exists:
Sysmon Operational log enabled:
Sysmon Operational record count:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Sysmon Operational log exists:
Sysmon Operational log enabled:
Sysmon Operational record count:
```

---

# Task 9 - Open the Sysmon Operational Log in Event Viewer

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows Event Viewer**.

## Steps

1. Select the **Windows Start** menu.
2. Search for **Event Viewer**.

### Copy

```text
Event Viewer
```

### Type

Type this into the Windows search box:

```text
Event Viewer
```

3. Open **Event Viewer**.
4. In the left pane, expand **Applications and Services Logs**.
5. Expand **Microsoft**.
6. Expand **Windows**.
7. Expand **Sysmon**.
8. Select **Operational**.

### Copy

```text
Applications and Services Logs > Microsoft > Windows > Sysmon > Operational
```

### Type

Navigate to this path in Event Viewer:

```text
Applications and Services Logs > Microsoft > Windows > Sysmon > Operational
```

9. Select a recent event if events are visible.

## Expected Result

The Sysmon Operational log should be visible.

You may see Sysmon events listed.

## Screenshot Checkpoint

Capture a screenshot showing the Sysmon Operational log in Event Viewer.

## Record in Evidence Notes

### Copy

```text
Sysmon Operational log visible in Event Viewer:
Recent Sysmon events visible:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Sysmon Operational log visible in Event Viewer:
Recent Sysmon events visible:
```

---

# Task 10 - Generate Safe Process Activity: Open Notepad

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Return to PowerShell.
2. Start Notepad.

### Copy

```powershell
Start-Process notepad.exe
```

### Type

Type this into PowerShell:

```powershell
Start-Process notepad.exe
```

3. Press **Enter**.
4. Confirm Notepad opens.
5. Close Notepad after it opens, or leave it open if your instructor allows.

## Expected Result

Notepad should open.

Sysmon should record process creation activity for:

```text
notepad.exe
```

## Screenshot Checkpoint

Capture a screenshot showing Notepad open if required.

## Record in Evidence Notes

### Copy

```text
Generated activity: notepad.exe
Activity time:
```

### Type

Type these lines into your evidence notes, then add the approximate time:

```text
Generated activity: notepad.exe
Activity time:
```

---

# Task 11 - Generate Safe Process Activity: Open Calculator

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Start Calculator.

### Copy

```powershell
Start-Process calc.exe
```

### Type

Type this into PowerShell:

```powershell
Start-Process calc.exe
```

2. Press **Enter**.
3. Confirm Calculator opens.
4. Close Calculator after it opens, or leave it open if your instructor allows.

## Expected Result

Calculator should open.

Sysmon should record process creation activity for:

```text
calc.exe
```

> [!note]
> On some Windows versions, Calculator may appear as `CalculatorApp.exe` or may launch through an app host process. Record what you observe in Kibana.

## Screenshot Checkpoint

Capture a screenshot showing Calculator open if required.

## Record in Evidence Notes

### Copy

```text
Generated activity: calc.exe
Activity time:
```

### Type

Type these lines into your evidence notes, then add the approximate time:

```text
Generated activity: calc.exe
Activity time:
```

---

# Task 12 - Generate Safe Process Activity: Run whoami

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Run the `whoami` command.

### Copy

```powershell
whoami
```

### Type

Type this into PowerShell:

```powershell
whoami
```

2. Press **Enter**.
3. Record the output.

## Expected Result

The output may look similar to:

```text
win11-client\student
```

Sysmon should record process activity related to command execution.

## Screenshot Checkpoint

Capture a screenshot showing the `whoami` command and output.

## Record in Evidence Notes

### Copy

```text
Generated activity: whoami
whoami output:
Activity time:
```

### Type

Type these lines into your evidence notes, then add your values:

```text
Generated activity: whoami
whoami output:
Activity time:
```

---

# Task 13 - Generate Safe Process Activity: Run hostname

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Run the `hostname` command.

### Copy

```powershell
hostname
```

### Type

Type this into PowerShell:

```powershell
hostname
```

2. Press **Enter**.
3. Record the output.

## Expected Result

The output should usually be:

```text
WIN11-CLIENT
```

Sysmon should record process activity related to command execution.

## Screenshot Checkpoint

Capture a screenshot showing the `hostname` command and output.

## Record in Evidence Notes

### Copy

```text
Generated activity: hostname
hostname output:
Activity time:
```

### Type

Type these lines into your evidence notes, then add your values:

```text
Generated activity: hostname
hostname output:
Activity time:
```

---

# Task 14 - Generate Safe Process Activity: Run ipconfig

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Run the `ipconfig` command.

### Copy

```powershell
ipconfig
```

### Type

Type this into PowerShell:

```powershell
ipconfig
```

2. Press **Enter**.
3. Review the output.
4. Record the approximate time.

## Expected Result

The output should show Windows network adapter information.

Sysmon should record process activity related to command execution.

## Screenshot Checkpoint

Capture a screenshot showing the `ipconfig` command and output.

## Record in Evidence Notes

### Copy

```text
Generated activity: ipconfig
Activity time:
```

### Type

Type these lines into your evidence notes, then add the approximate time:

```text
Generated activity: ipconfig
Activity time:
```

---

# Task 15 - Confirm Recent Sysmon Events Locally

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Query recent Sysmon events locally.

### Copy

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 10 | Select-Object TimeCreated, Id, ProviderName, Message
```

### Type

Type this into PowerShell:

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 10 | Select-Object TimeCreated, Id, ProviderName, Message
```

2. Press **Enter**.
3. Review the recent Sysmon events.
4. Look for Event ID 1 if visible.

## Expected Result

You should see recent Sysmon events.

Some events may have:

```text
Id: 1
ProviderName: Microsoft-Windows-Sysmon
```

> [!note]
> Event ID 1 means Process Create.

## Screenshot Checkpoint

Capture a screenshot showing recent Sysmon events in PowerShell.

## Record in Evidence Notes

### Copy

```text
Recent Sysmon events visible locally:
Sysmon Event ID 1 visible locally:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Recent Sysmon events visible locally:
Sysmon Event ID 1 visible locally:
```

---

# Task 16 - Open Kibana from Windows

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

Capture a screenshot showing that Kibana is open if required.

## Record in Evidence Notes

### Copy

```text
Kibana URL used:
Kibana opened successfully:
```

### Type

Type these lines into your evidence notes, then add your values:

```text
Kibana URL used:
Kibana opened successfully:
```

---

# Task 17 - Open Kibana Discover

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana** in the browser.

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

## Record in Evidence Notes

### Copy

```text
Discover opened:
```

### Type

Type this into your evidence notes:

```text
Discover opened:
```

---

# Task 18 - Select the Logs Data View and Time Range

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Locate the data view selector in Discover.
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

Alternative possible data views:

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

> [!hint]
> If no events appear later, try expanding the time range to `Last 7 days`.

## Screenshot Checkpoint

Capture a screenshot showing the selected data view and time range.

## Record in Evidence Notes

### Copy

```text
Data view selected:
Time range used:
```

### Type

Type these lines into your evidence notes, then add your values:

```text
Data view selected:
Time range used:
```

---

# Task 19 - Search for Events from WIN11-CLIENT

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Click the Kibana query bar.
2. Enter the host query.

### Copy

```text
host.name : "WIN11-CLIENT"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT"
```

3. Press **Enter** or select **Update**.
4. Review the event results.

If no events appear, try this alternate query:

### Copy

```text
agent.name : "WIN11-CLIENT"
```

### Type

Type this alternate query into the Kibana query bar:

```text
agent.name : "WIN11-CLIENT"
```

## Expected Result

Events from WIN11-CLIENT should appear in Discover.

## Screenshot Checkpoint

Capture a screenshot showing events from WIN11-CLIENT.

## Record in Evidence Notes

### Copy

```text
Host query used:
Events from WIN11-CLIENT found:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Host query used:
Events from WIN11-CLIENT found:
```

---

# Task 20 - Search for Sysmon Events

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for Sysmon events using the provider field.

### Copy

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon"
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try the Sysmon dataset query:

### Copy

```text
host.name : "WIN11-CLIENT" and event.dataset : "windows.sysmon_operational"
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.dataset : "windows.sysmon_operational"
```

If still no results appear, try the channel query:

### Copy

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Microsoft-Windows-Sysmon/Operational"
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Microsoft-Windows-Sysmon/Operational"
```

## Expected Result

Sysmon events should appear in Kibana Discover.

You may see fields such as:

```text
event.provider
event.dataset
winlog.channel
event.code
process.name
process.command_line
process.parent.name
user.name
```

## Screenshot Checkpoint

Capture a screenshot showing Sysmon events in Discover.

## Record in Evidence Notes

### Copy

```text
Sysmon query used:
Sysmon events found in Kibana:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Sysmon query used:
Sysmon events found in Kibana:
```

---

# Task 21 - Search for Sysmon Event ID 1 Process Creation Events

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for Sysmon Event ID 1.

### Copy

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon" and event.code : "1"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon" and event.code : "1"
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try the alternate event ID field:

### Copy

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon" and winlog.event_id : 1
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon" and winlog.event_id : 1
```

If still no results appear, try the dataset version:

### Copy

```text
host.name : "WIN11-CLIENT" and event.dataset : "windows.sysmon_operational" and event.code : "1"
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.dataset : "windows.sysmon_operational" and event.code : "1"
```

## Expected Result

You should see Sysmon Event ID 1 events.

Sysmon Event ID 1 means:

```text
Process Create
```

## Screenshot Checkpoint

Capture a screenshot showing Sysmon Event ID 1 process creation events.

## Record in Evidence Notes

### Copy

```text
Sysmon Event ID 1 query used:
Sysmon Event ID 1 events found:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Sysmon Event ID 1 query used:
Sysmon Event ID 1 events found:
```

---

# Task 22 - Add Useful Process Fields in Discover

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. In Discover, locate the field list.
2. Add or view the timestamp field.

### Copy

```text
@timestamp
```

### Type

Search for or add this field:

```text
@timestamp
```

3. Add or view the process name field.

### Copy

```text
process.name
```

### Type

Search for or add this field:

```text
process.name
```

4. Add or view the process executable field.

### Copy

```text
process.executable
```

### Type

Search for or add this field:

```text
process.executable
```

5. Add or view the command line field.

### Copy

```text
process.command_line
```

### Type

Search for or add this field:

```text
process.command_line
```

6. Add or view the parent process name field.

### Copy

```text
process.parent.name
```

### Type

Search for or add this field:

```text
process.parent.name
```

7. Add or view the username field.

### Copy

```text
user.name
```

### Type

Search for or add this field:

```text
user.name
```

8. Add or view the event code field.

### Copy

```text
event.code
```

### Type

Search for or add this field:

```text
event.code
```

## Expected Result

The Discover table should show useful process investigation fields.

Recommended fields:

```text
@timestamp
process.name
process.executable
process.command_line
process.parent.name
user.name
event.code
```

> [!note]
> Some fields may be missing depending on the Elastic integration and event format. If a field is not visible, record that it was not available.

## Screenshot Checkpoint

Capture a screenshot showing process-related fields in Discover.

## Record in Evidence Notes

### Copy

```text
Process fields added or reviewed:
Missing fields, if any:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Process fields added or reviewed:
Missing fields, if any:
```

---

# Task 23 - Search for Notepad Process Activity

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for Notepad process activity.

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
3. Review the results.

If no results appear, try this alternate query:

### Copy

```text
host.name : "WIN11-CLIENT" and process.executable : *notepad*
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and process.executable : *notepad*
```

If still no results appear, try this broader query:

### Copy

```text
host.name : "WIN11-CLIENT" and message : *notepad*
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and message : *notepad*
```

## Expected Result

You should find process activity related to:

```text
notepad.exe
```

## Screenshot Checkpoint

Capture a screenshot showing the Notepad process event.

## Record in Evidence Notes

### Copy

```text
Notepad query used:
Notepad event found:
Notepad timestamp:
Notepad parent process:
Notepad command line:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Notepad query used:
Notepad event found:
Notepad timestamp:
Notepad parent process:
Notepad command line:
```

---

# Task 24 - Search for Calculator Process Activity

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for Calculator process activity.

### Copy

```text
host.name : "WIN11-CLIENT" and process.name : "calc.exe"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and process.name : "calc.exe"
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try this alternate query:

### Copy

```text
host.name : "WIN11-CLIENT" and process.name : *Calculator*
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and process.name : *Calculator*
```

If still no results appear, try this broader query:

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

You may find process activity related to:

```text
calc.exe
```

or:

```text
CalculatorApp.exe
```

or another Windows app host process.

> [!note]
> Calculator may appear differently depending on the Windows version.

## Screenshot Checkpoint

Capture a screenshot showing the Calculator-related process event or query attempt.

## Record in Evidence Notes

### Copy

```text
Calculator query used:
Calculator event found:
Calculator process name observed:
Calculator timestamp:
Calculator parent process:
Calculator command line:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Calculator query used:
Calculator event found:
Calculator process name observed:
Calculator timestamp:
Calculator parent process:
Calculator command line:
```

---

# Task 25 - Search for whoami Command Activity

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for `whoami` in process command lines.

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

If no results appear, try this alternate query:

### Copy

```text
host.name : "WIN11-CLIENT" and process.name : "whoami.exe"
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and process.name : "whoami.exe"
```

If still no results appear, try this broader query:

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

You should find activity related to:

```text
whoami
```

## Screenshot Checkpoint

Capture a screenshot showing the `whoami` process event.

## Record in Evidence Notes

### Copy

```text
whoami query used:
whoami event found:
whoami timestamp:
whoami parent process:
whoami command line:
whoami user:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
whoami query used:
whoami event found:
whoami timestamp:
whoami parent process:
whoami command line:
whoami user:
```

---

# Task 26 - Search for hostname Command Activity

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for `hostname` in process command lines.

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

If no results appear, try this alternate query:

### Copy

```text
host.name : "WIN11-CLIENT" and process.name : "hostname.exe"
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and process.name : "hostname.exe"
```

If still no results appear, try this broader query:

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

You should find activity related to:

```text
hostname
```

## Screenshot Checkpoint

Capture a screenshot showing the `hostname` process event.

## Record in Evidence Notes

### Copy

```text
hostname query used:
hostname event found:
hostname timestamp:
hostname parent process:
hostname command line:
hostname user:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
hostname query used:
hostname event found:
hostname timestamp:
hostname parent process:
hostname command line:
hostname user:
```

---

# Task 27 - Search for ipconfig Command Activity

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Search for `ipconfig` in process command lines.

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

If no results appear, try this alternate query:

### Copy

```text
host.name : "WIN11-CLIENT" and process.name : "ipconfig.exe"
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and process.name : "ipconfig.exe"
```

If still no results appear, try this broader query:

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

You should find activity related to:

```text
ipconfig
```

## Screenshot Checkpoint

Capture a screenshot showing the `ipconfig` process event.

## Record in Evidence Notes

### Copy

```text
ipconfig query used:
ipconfig event found:
ipconfig timestamp:
ipconfig parent process:
ipconfig command line:
ipconfig user:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
ipconfig query used:
ipconfig event found:
ipconfig timestamp:
ipconfig parent process:
ipconfig command line:
ipconfig user:
```

---

# Task 28 - Open One Sysmon Event and Review Details

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Run a query that returns Sysmon Event ID 1 events.

### Copy

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon" and event.code : "1"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon" and event.code : "1"
```

2. Press **Enter** or select **Update**.
3. Select one event from the results table.
4. Open the event details.
5. Review the process fields.

Look for these fields:

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
```

## Expected Result

You should be able to identify important process details from one Sysmon event.

## Screenshot Checkpoint

Capture a screenshot showing the open Sysmon event details.

## Record in Evidence Notes

### Copy

```text
Selected Sysmon event timestamp:
Selected Sysmon event process name:
Selected Sysmon event process path:
Selected Sysmon event command line:
Selected Sysmon event parent process:
Selected Sysmon event username:
Selected Sysmon event ID:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Selected Sysmon event timestamp:
Selected Sysmon event process name:
Selected Sysmon event process path:
Selected Sysmon event command line:
Selected Sysmon event parent process:
Selected Sysmon event username:
Selected Sysmon event ID:
```

---

# Task 29 - Create the Lab 05 Process Timeline File

## Where to Work

Use **WIN11-CLIENT**.

Use **Notepad**.

## Steps

1. Open **Notepad**.
2. Copy or type the process timeline template below.
3. Fill in details from the events you found in Kibana.
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
Lab05-Sysmon-Process-Timeline.txt
```

### Type

Type this filename exactly:

```text
Lab05-Sysmon-Process-Timeline.txt
```

## Process Timeline Template

### Copy

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 05 - Sysmon Process Activity Timeline

Student Name:
Date:

Timeline Entry 1
Timestamp:
Host:
User:
Process name:
Process path:
Command line:
Parent process:
Event provider:
Event ID:
Notes:

Timeline Entry 2
Timestamp:
Host:
User:
Process name:
Process path:
Command line:
Parent process:
Event provider:
Event ID:
Notes:

Timeline Entry 3
Timestamp:
Host:
User:
Process name:
Process path:
Command line:
Parent process:
Event provider:
Event ID:
Notes:

Timeline Entry 4
Timestamp:
Host:
User:
Process name:
Process path:
Command line:
Parent process:
Event provider:
Event ID:
Notes:

Timeline Entry 5
Timestamp:
Host:
User:
Process name:
Process path:
Command line:
Parent process:
Event provider:
Event ID:
Notes:

Timeline Summary
Write 3 to 5 sentences explaining what process activity you observed and why Sysmon Event ID 1 is useful.
```

### Type

Type this template into Notepad manually:

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 05 - Sysmon Process Activity Timeline

Student Name:
Date:

Timeline Entry 1
Timestamp:
Host:
User:
Process name:
Process path:
Command line:
Parent process:
Event provider:
Event ID:
Notes:

Timeline Entry 2
Timestamp:
Host:
User:
Process name:
Process path:
Command line:
Parent process:
Event provider:
Event ID:
Notes:

Timeline Entry 3
Timestamp:
Host:
User:
Process name:
Process path:
Command line:
Parent process:
Event provider:
Event ID:
Notes:

Timeline Entry 4
Timestamp:
Host:
User:
Process name:
Process path:
Command line:
Parent process:
Event provider:
Event ID:
Notes:

Timeline Entry 5
Timestamp:
Host:
User:
Process name:
Process path:
Command line:
Parent process:
Event provider:
Event ID:
Notes:

Timeline Summary
Write 3 to 5 sentences explaining what process activity you observed and why Sysmon Event ID 1 is useful.
```

## Expected Result

The process timeline file should be saved at:

```text
C:\BlueWave\Evidence\Lab05-Sysmon-Process-Timeline.txt
```

## Screenshot Checkpoint

Capture a screenshot showing the completed process timeline.

---

# Task 30 - Create the Lab 05 Notes File

## Where to Work

Use **WIN11-CLIENT**.

Use **Notepad**.

## Steps

1. Open **Notepad**.
2. Copy or type the Lab 05 notes template below.
3. Fill in the missing information using your lab results.
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
Lab05-Sysmon-Notes.txt
```

### Type

Type this filename exactly:

```text
Lab05-Sysmon-Notes.txt
```

## Lab Notes Template

### Copy

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 05 - Collecting Sysmon Process Activity

Student Name:
Date:

1. Environment Verification

Windows hostname:
Windows evidence folder confirmed:
Elastic Agent service status:

2. Sysmon Verification

Sysmon executable found:
Sysmon configuration found:
Sysmon files path:
Sysmon service found:
Sysmon service status:
Sysmon installation performed:
Sysmon installation result:
Sysmon running after verification:
Sysmon Operational log exists:
Sysmon Operational log enabled:
Sysmon Operational record count:
Sysmon Operational log visible in Event Viewer:
Recent Sysmon events visible:
Recent Sysmon events visible locally:
Sysmon Event ID 1 visible locally:

3. Safe Activity Generated

Generated activity: notepad.exe
Activity time:

Generated activity: calc.exe
Activity time:

Generated activity: whoami
whoami output:
Activity time:

Generated activity: hostname
hostname output:
Activity time:

Generated activity: ipconfig
Activity time:

4. Kibana Verification

Kibana URL used:
Kibana opened successfully:
Discover opened:
Data view selected:
Time range used:
Host query used:
Events from WIN11-CLIENT found:
Sysmon query used:
Sysmon events found in Kibana:
Sysmon Event ID 1 query used:
Sysmon Event ID 1 events found:
Process fields added or reviewed:
Missing fields, if any:

5. Process Searches

Notepad query used:
Notepad event found:
Notepad timestamp:
Notepad parent process:
Notepad command line:

Calculator query used:
Calculator event found:
Calculator process name observed:
Calculator timestamp:
Calculator parent process:
Calculator command line:

whoami query used:
whoami event found:
whoami timestamp:
whoami parent process:
whoami command line:
whoami user:

hostname query used:
hostname event found:
hostname timestamp:
hostname parent process:
hostname command line:
hostname user:

ipconfig query used:
ipconfig event found:
ipconfig timestamp:
ipconfig parent process:
ipconfig command line:
ipconfig user:

6. Selected Sysmon Event

Selected Sysmon event timestamp:
Selected Sysmon event process name:
Selected Sysmon event process path:
Selected Sysmon event command line:
Selected Sysmon event parent process:
Selected Sysmon event username:
Selected Sysmon event ID:

7. Lab Summary

Write 3 to 5 sentences explaining what you learned about Sysmon, process creation events, and Kibana analysis.
```

### Type

Type this template into Notepad manually:

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 05 - Collecting Sysmon Process Activity

Student Name:
Date:

1. Environment Verification

Windows hostname:
Windows evidence folder confirmed:
Elastic Agent service status:

2. Sysmon Verification

Sysmon executable found:
Sysmon configuration found:
Sysmon files path:
Sysmon service found:
Sysmon service status:
Sysmon installation performed:
Sysmon installation result:
Sysmon running after verification:
Sysmon Operational log exists:
Sysmon Operational log enabled:
Sysmon Operational record count:
Sysmon Operational log visible in Event Viewer:
Recent Sysmon events visible:
Recent Sysmon events visible locally:
Sysmon Event ID 1 visible locally:

3. Safe Activity Generated

Generated activity: notepad.exe
Activity time:

Generated activity: calc.exe
Activity time:

Generated activity: whoami
whoami output:
Activity time:

Generated activity: hostname
hostname output:
Activity time:

Generated activity: ipconfig
Activity time:

4. Kibana Verification

Kibana URL used:
Kibana opened successfully:
Discover opened:
Data view selected:
Time range used:
Host query used:
Events from WIN11-CLIENT found:
Sysmon query used:
Sysmon events found in Kibana:
Sysmon Event ID 1 query used:
Sysmon Event ID 1 events found:
Process fields added or reviewed:
Missing fields, if any:

5. Process Searches

Notepad query used:
Notepad event found:
Notepad timestamp:
Notepad parent process:
Notepad command line:

Calculator query used:
Calculator event found:
Calculator process name observed:
Calculator timestamp:
Calculator parent process:
Calculator command line:

whoami query used:
whoami event found:
whoami timestamp:
whoami parent process:
whoami command line:
whoami user:

hostname query used:
hostname event found:
hostname timestamp:
hostname parent process:
hostname command line:
hostname user:

ipconfig query used:
ipconfig event found:
ipconfig timestamp:
ipconfig parent process:
ipconfig command line:
ipconfig user:

6. Selected Sysmon Event

Selected Sysmon event timestamp:
Selected Sysmon event process name:
Selected Sysmon event process path:
Selected Sysmon event command line:
Selected Sysmon event parent process:
Selected Sysmon event username:
Selected Sysmon event ID:

7. Lab Summary

Write 3 to 5 sentences explaining what you learned about Sysmon, process creation events, and Kibana analysis.
```

## Expected Result

The notes file should be saved at:

```text
C:\BlueWave\Evidence\Lab05-Sysmon-Notes.txt
```

## Screenshot Checkpoint

Capture a screenshot showing the completed Lab 05 notes file.

---

# Task 31 - Confirm the Lab 05 Evidence Files Exist

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Confirm the process timeline file exists.

### Copy

```powershell
Test-Path "C:\BlueWave\Evidence\Lab05-Sysmon-Process-Timeline.txt"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab05-Sysmon-Process-Timeline.txt"
```

2. Press **Enter**.
3. Confirm the notes file exists.

### Copy

```powershell
Test-Path "C:\BlueWave\Evidence\Lab05-Sysmon-Notes.txt"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab05-Sysmon-Notes.txt"
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

1. Confirm Sysmon is running.
2. Confirm the Sysmon Operational log exists.
3. Confirm safe process activity was generated.
4. Confirm Sysmon events are visible in Kibana.
5. Confirm Sysmon Event ID 1 events are visible.
6. Confirm at least three process activities were searched in Kibana.
7. Confirm the process timeline file exists.
8. Confirm the notes file exists.

## Expected Result

You should have:

```text
Sysmon running
Sysmon Operational log visible
Safe process activity generated
Sysmon Event ID 1 visible in Kibana
Process details reviewed
Timeline completed
Lab05-Sysmon-Notes.txt saved
Lab05-Sysmon-Process-Timeline.txt saved
```

## Screenshot Checkpoint

Capture any final screenshots required by your instructor.

---

# Validation Checklist

Before finishing the lab, confirm each item is complete.

- [ ] I confirmed WIN11-CLIENT is available.
- [ ] I confirmed the Windows evidence folder exists.
- [ ] I confirmed Elastic Agent is running.
- [ ] I found `Sysmon64.exe`.
- [ ] I found `sysmon-bluewave.xml`.
- [ ] I checked whether Sysmon is installed.
- [ ] I installed Sysmon only if required.
- [ ] I confirmed Sysmon is running.
- [ ] I confirmed the Sysmon Operational log exists.
- [ ] I opened the Sysmon Operational log in Event Viewer.
- [ ] I opened Notepad.
- [ ] I opened Calculator.
- [ ] I ran `whoami`.
- [ ] I ran `hostname`.
- [ ] I ran `ipconfig`.
- [ ] I confirmed recent Sysmon events locally.
- [ ] I opened Kibana.
- [ ] I opened Discover.
- [ ] I selected a logs data view.
- [ ] I set the time range to Last 24 hours.
- [ ] I searched for events from WIN11-CLIENT.
- [ ] I searched for Sysmon events.
- [ ] I searched for Sysmon Event ID 1.
- [ ] I added or reviewed process fields.
- [ ] I searched for Notepad process activity.
- [ ] I searched for Calculator process activity.
- [ ] I searched for `whoami` activity.
- [ ] I searched for `hostname` activity.
- [ ] I searched for `ipconfig` activity.
- [ ] I opened one Sysmon event and reviewed details.
- [ ] I created `Lab05-Sysmon-Process-Timeline.txt`.
- [ ] I created `Lab05-Sysmon-Notes.txt`.
- [ ] I captured the required screenshots.

---

# Troubleshooting

## Problem: Sysmon files are missing

Check the Tools folder.

### Copy

```powershell
Get-ChildItem "C:\LabFiles\Tools"
```

### Type

Type this into PowerShell:

```powershell
Get-ChildItem "C:\LabFiles\Tools"
```

Look for:

```text
Sysmon64.exe
sysmon-bluewave.xml
```

If the files are missing, notify your instructor.

Do not download Sysmon from the internet.

---

## Problem: Sysmon service is not found

Check both possible service names.

### Copy

```powershell
Get-Service Sysmon64 -ErrorAction SilentlyContinue
```

### Type

Type this into PowerShell:

```powershell
Get-Service Sysmon64 -ErrorAction SilentlyContinue
```

### Copy

```powershell
Get-Service Sysmon -ErrorAction SilentlyContinue
```

### Type

Type this into PowerShell:

```powershell
Get-Service Sysmon -ErrorAction SilentlyContinue
```

If Sysmon is not installed, complete Task 6 only if your instructor allows installation.

---

## Problem: Sysmon installation fails

Check that PowerShell is running as Administrator.

Search for:

### Copy

```text
PowerShell
```

### Type

Type this into Windows search:

```text
PowerShell
```

Right-click **Windows PowerShell** and select **Run as administrator**.

Then run:

### Copy

```powershell
Set-Location "C:\LabFiles\Tools"
```

### Type

Type this into Administrator PowerShell:

```powershell
Set-Location "C:\LabFiles\Tools"
```

Then run:

### Copy

```powershell
.\Sysmon64.exe -accepteula -i .\sysmon-bluewave.xml
```

### Type

Type this into Administrator PowerShell:

```powershell
.\Sysmon64.exe -accepteula -i .\sysmon-bluewave.xml
```

---

## Problem: Sysmon Operational log is not found

Try checking the log again.

### Copy

```powershell
Get-WinEvent -ListLog "Microsoft-Windows-Sysmon/Operational"
```

### Type

Type this into PowerShell:

```powershell
Get-WinEvent -ListLog "Microsoft-Windows-Sysmon/Operational"
```

If the log does not exist, Sysmon may not be installed correctly.

Ask your instructor before making changes.

---

## Problem: Kibana does not open

Check that the Kibana URL is correct.

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

## Problem: No Sysmon events appear in Kibana

Try these queries.

### Copy

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon"
```

### Copy

```text
host.name : "WIN11-CLIENT" and event.dataset : "windows.sysmon_operational"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.dataset : "windows.sysmon_operational"
```

### Copy

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Microsoft-Windows-Sysmon/Operational"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Microsoft-Windows-Sysmon/Operational"
```

Also check the time range.

Set it to:

### Copy

```text
Last 24 hours
```

### Type

Select or type:

```text
Last 24 hours
```

If still no events appear, try:

### Copy

```text
Last 7 days
```

### Type

Select or type:

```text
Last 7 days
```

---

## Problem: Sysmon Event ID 1 does not appear

Try alternate event ID fields.

### Copy

```text
event.code : "1"
```

### Type

Type this into the Kibana query bar:

```text
event.code : "1"
```

### Copy

```text
winlog.event_id : 1
```

### Type

Type this into the Kibana query bar:

```text
winlog.event_id : 1
```

Try combining with the Sysmon channel:

### Copy

```text
winlog.channel : "Microsoft-Windows-Sysmon/Operational" and winlog.event_id : 1
```

### Type

Type this into the Kibana query bar:

```text
winlog.channel : "Microsoft-Windows-Sysmon/Operational" and winlog.event_id : 1
```

---

## Problem: Notepad or Calculator events do not appear

Try broader message searches.

### Copy

```text
message : *notepad*
```

### Type

Type this into the Kibana query bar:

```text
message : *notepad*
```

### Copy

```text
message : *calc*
```

### Type

Type this into the Kibana query bar:

```text
message : *calc*
```

Calculator may appear as:

```text
CalculatorApp.exe
```

or another application host process.

---

## Problem: Command-line fields are missing

Try opening the full event details and look for related fields.

### Copy

```text
process.command_line
process.executable
process.name
message
winlog.event_data.CommandLine
```

### Type

Look for these fields:

```text
process.command_line
process.executable
process.name
message
winlog.event_data.CommandLine
```

If the normalized field is missing, the command line may still appear in the raw message.

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
user.name
message
```

### Type

Look for these fields in the event details:

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
user.name
message
```

---

## Problem: Evidence files are missing

Check the evidence folder.

### Copy

```powershell
Get-ChildItem "C:\BlueWave\Evidence"
```

### Type

Type this into PowerShell:

```powershell
Get-ChildItem "C:\BlueWave\Evidence"
```

Confirm these files exist:

```text
Lab05-Sysmon-Notes.txt
Lab05-Sysmon-Process-Timeline.txt
```

If missing, recreate them using Task 29 and Task 30.

---

# Knowledge Check

Answer the following questions.

1. What is Sysmon used for?
2. What Windows log stores Sysmon events?
3. What does Sysmon Event ID 1 represent?
4. Why is the parent process useful during investigation?
5. What field often shows the command used to start a process?
6. What Kibana query can search for Sysmon Event ID 1?
7. What process name did you search for when reviewing Notepad activity?
8. Why might Calculator not appear exactly as `calc.exe`?
9. What should you do if a normalized field such as `process.command_line` is missing?
10. Why is it useful to build a process activity timeline?

---

# Summary

In this lab, you completed the following tasks:

- Confirmed Elastic Agent is running.
- Located Sysmon files on WIN11-CLIENT.
- Confirmed whether Sysmon is installed.
- Installed Sysmon only if required.
- Confirmed the Sysmon Operational log exists.
- Opened the Sysmon Operational log in Event Viewer.
- Generated safe process activity.
- Searched for Sysmon events in Kibana.
- Searched for Sysmon Event ID 1 process creation events.
- Reviewed process name, parent process, command line, username, and timestamp.
- Created a process activity timeline.
- Created Lab 05 notes.

You are now ready for Lab 06, where you will analyse safe simulated endpoint activity using the BlueWave Activity Simulator.

---

# Deliverables

Submit or retain the following items as directed by your instructor.

| Deliverable | Location |
|---|---|
| Lab 05 Sysmon notes | `C:\BlueWave\Evidence\Lab05-Sysmon-Notes.txt` |
| Lab 05 Sysmon process timeline | `C:\BlueWave\Evidence\Lab05-Sysmon-Process-Timeline.txt` |
| Screenshot of Sysmon files in `C:\LabFiles\Tools` | Skillable submission area |
| Screenshot of Sysmon service status | Skillable submission area |
| Screenshot of Sysmon Operational log | Skillable submission area |
| Screenshot of safe process activity commands | Skillable submission area |
| Screenshot of Sysmon events in Kibana | Skillable submission area |
| Screenshot of Sysmon Event ID 1 query | Skillable submission area |
| Screenshot of Notepad or Calculator event | Skillable submission area |
| Screenshot of `whoami`, `hostname`, or `ipconfig` event | Skillable submission area |
| Screenshot of open Sysmon event details | Skillable submission area |
| Screenshot of completed process timeline | Skillable submission area |
| Screenshot of completed Lab 05 notes file | Skillable submission area |

---

# Instructor Notes

## Expected Knowledge Check Answers

1. Sysmon records detailed Windows system activity for monitoring and investigation.
2. Sysmon events are stored in `Microsoft-Windows-Sysmon/Operational`.
3. Sysmon Event ID 1 represents Process Create.
4. The parent process helps explain how another process started.
5. `process.command_line` often shows the command used to start a process.
6. A useful query is:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon" and event.code : "1"
```

7. Students should search for `notepad.exe`.
8. Calculator may launch as a modern Windows app or through another application host process.
9. Open the raw event details and check fields such as `message` or `winlog.event_data.CommandLine`.
10. A timeline helps analysts understand what happened and in what order.

---

## Expected Evidence Files

Students should create:

```text
C:\BlueWave\Evidence\Lab05-Sysmon-Notes.txt
```

and:

```text
C:\BlueWave\Evidence\Lab05-Sysmon-Process-Timeline.txt
```

---

## Expected Windows Commands

Sysmon service checks:

```powershell
Get-Service Sysmon64 -ErrorAction SilentlyContinue
```

```powershell
Get-Service Sysmon -ErrorAction SilentlyContinue
```

Sysmon log check:

```powershell
Get-WinEvent -ListLog "Microsoft-Windows-Sysmon/Operational"
```

Optional Sysmon install command:

```powershell
.\Sysmon64.exe -accepteula -i .\sysmon-bluewave.xml
```

Safe process activity:

```powershell
Start-Process notepad.exe
```

```powershell
Start-Process calc.exe
```

```powershell
whoami
```

```powershell
hostname
```

```powershell
ipconfig
```

Recent Sysmon local event check:

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 10 | Select-Object TimeCreated, Id, ProviderName, Message
```

---

## Expected Elastic Queries

Host query:

```text
host.name : "WIN11-CLIENT"
```

Sysmon provider query:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon"
```

Sysmon dataset query:

```text
host.name : "WIN11-CLIENT" and event.dataset : "windows.sysmon_operational"
```

Sysmon channel query:

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Microsoft-Windows-Sysmon/Operational"
```

Sysmon Event ID 1 query:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon" and event.code : "1"
```

Alternate Event ID 1 query:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Sysmon" and winlog.event_id : 1
```

Notepad query:

```text
host.name : "WIN11-CLIENT" and process.name : "notepad.exe"
```

Calculator query:

```text
host.name : "WIN11-CLIENT" and process.name : "calc.exe"
```

whoami query:

```text
host.name : "WIN11-CLIENT" and process.command_line : *whoami*
```

hostname query:

```text
host.name : "WIN11-CLIENT" and process.command_line : *hostname*
```

ipconfig query:

```text
host.name : "WIN11-CLIENT" and process.command_line : *ipconfig*
```

---

## Expected Visible Results

Students should be able to show:

- Sysmon files are present.
- Sysmon service is running.
- Sysmon Operational log exists.
- Safe process activity was generated.
- Sysmon events are visible in Event Viewer or PowerShell.
- Sysmon events are visible in Kibana.
- Sysmon Event ID 1 events are visible.
- Process fields are reviewed.
- Timeline file is completed.
- Notes file is completed.

---

## Common Student Mistakes

| Mistake | Instructor Guidance |
|---|---|
| Student installs Sysmon even though it is already installed | Remind students to verify first and install only if required |
| Student does not run PowerShell as Administrator for install | Have them reopen PowerShell as Administrator |
| Student searches for Sysmon before logs are ingested | Have them wait, refresh, and check time range |
| Student uses only one field name | Have them try alternate fields such as `winlog.event_id` and `message` |
| Student cannot find Calculator as `calc.exe` | Explain that Calculator may appear as a modern app process |
| Student forgets to record parent process | Remind them parent process helps explain process relationships |
| Student ignores timestamp | Remind them timeline order depends on timestamps |
| Student leaves evidence file incomplete | Have them complete notes and timeline before submitting |

---

## Grading Guidance

Suggested grading allocation:

| Criteria | Points |
|---|---:|
| Sysmon files located | 10 |
| Sysmon installation or verification completed | 15 |
| Sysmon Operational log confirmed | 10 |
| Safe process activity generated | 15 |
| Sysmon events found in Kibana | 15 |
| Sysmon Event ID 1 searched and reviewed | 15 |
| Process fields identified | 10 |
| Timeline completed | 5 |
| Notes and screenshots completed | 5 |
| Total | 100 |

Do not heavily penalise students if a specific process, such as Calculator, appears under a different name, as long as the student documents what they observed.

---

## Free Elastic Basic License Reminder

This lab must use:

- Self-managed Elastic.
- Free Elastic Basic license.
- Kibana Discover.
- Windows and Sysmon event collection.
- No Elastic Cloud.
- No paid subscriptions.
- No external internet access.

---

## Fallback Option if Live Sysmon Logs Are Unavailable

If live Sysmon logs are unavailable, students may use the optional sample file if provided:

```text
C:\LabFiles\Logs\sample-sysmon-events.csv
```

Suggested fallback evidence line:

```text
Live Sysmon events unavailable. Fallback sample file checked: C:\LabFiles\Logs\sample-sysmon-events.csv
```

The preferred method is always live Sysmon evidence in Kibana when available.

---

End of Lab 05.
