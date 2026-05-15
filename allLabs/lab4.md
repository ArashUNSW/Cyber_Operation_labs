# Lab 04 - Collecting and Reviewing Windows Event Logs

## Estimated Time

90–120 minutes

---

## Lab Purpose

In this lab, you will use Kibana Discover to collect, search, filter, and review Windows Event Logs from WIN11-CLIENT.

You will confirm that Windows logs are arriving in Elastic, filter events by hostname and event source, review common Windows log types, compare one Windows Event Viewer event with one Kibana event, and create a Windows log collection summary.

This lab continues the BlueWave Clinic SOC build from Lab 03.

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
> Type commands and queries exactly as shown. Queries are sensitive to spaces, punctuation, quotation marks, and field names.

---

## Learning Objectives

By the end of this lab, you will be able to:

- Open Kibana Discover.
- Select a logs data view.
- Set the Kibana time range.
- Search for events from WIN11-CLIENT.
- Filter Windows Event Logs by host name.
- Filter events by Windows event provider.
- Review Security, System, and Application events.
- Open Windows Event Viewer.
- Compare one Windows Event Viewer event with one Kibana event.
- Capture evidence screenshots.
- Create a Windows log collection summary.

---

## Scenario

BlueWave Clinic has now enrolled the Windows 11 endpoint into Elastic using Elastic Agent.

The next task is to confirm that Windows Event Logs are being collected and that analysts can search them in Kibana.

As a junior cyber operations analyst, you need to prove that logs from WIN11-CLIENT are visible in Elastic.

You will review Windows logs in two places:

1. **Windows Event Viewer** on WIN11-CLIENT.
2. **Kibana Discover** in the browser.

You will compare local Windows evidence with Elastic evidence to understand how endpoint activity appears after collection.

> [!note]
> This lab focuses on Windows Event Logs. Sysmon process activity is reviewed in Lab 05.

> [!note]
> This lab uses self-managed Elastic on UBUNTU-SOC.

> [!alert]
> Do not use Elastic Cloud. Do not create an Elastic Cloud account. Do not download tools from the internet.

---

## Required Machines

| Machine | Used For |
|---|---|
| WIN11-CLIENT | Browser access to Kibana, Event Viewer, PowerShell, evidence notes |
| UBUNTU-SOC | Elasticsearch and Kibana services |

---

## Required Files

| File | Location | Purpose |
|---|---|---|
| sample-windows-events.csv | `C:\LabFiles\Logs` | Optional fallback sample data if live logs are unavailable |
| timeline-template.md | `C:\LabFiles\Templates` | Optional event comparison structure |
| Lab04-Windows-Log-Summary.txt | `C:\BlueWave\Evidence` | Student evidence file created during this lab |

---

## Important Paths

### Windows Paths

| Path | Purpose |
|---|---|
| `C:\BlueWave\Evidence` | Windows evidence folder |
| `C:\BlueWave\Evidence\Lab04-Windows-Log-Summary.txt` | Lab 04 evidence notes |
| `C:\LabFiles\Logs\sample-windows-events.csv` | Optional fallback sample Windows logs |
| `C:\LabFiles\Templates\timeline-template.md` | Optional timeline template |

### Kibana Areas

| Kibana Area | Purpose |
|---|---|
| Discover | Search and review events |
| Data view selector | Choose the data source to search |
| Time picker | Select the event time range |
| KQL query bar | Enter search and filter queries |
| Event details panel | Review individual event fields |

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

> [!note]
> Your Ubuntu SOC IP address may be different. Use the actual IP address from your lab environment.

---

## Screenshots You Should Capture

Capture screenshots as instructed by your trainer or Skillable platform.

Recommended screenshots:

1. Kibana Discover open with the selected data view.
2. Kibana time range set to Last 24 hours.
3. Discover showing events from WIN11-CLIENT.
4. Discover showing Windows Security events.
5. Discover showing Windows System or Application events.
6. Windows Event Viewer showing a local Windows event.
7. Kibana showing a matching or similar event.
8. Completed Lab 04 evidence notes file.

---

## Key Terms

| Term | Meaning |
|---|---|
| Windows Event Log | A Windows record of system, security, application, and operational activity |
| Event Viewer | Windows tool used to view local event logs |
| Security log | Windows log containing security-related events |
| System log | Windows log containing operating system and service events |
| Application log | Windows log containing application-related events |
| Provider | The source that generated an event |
| Event ID | A number identifying the type of Windows event |
| Kibana Discover | Kibana page used to search and inspect events |
| Data view | Kibana object that defines which indexes can be searched |
| KQL | Kibana Query Language |
| Timestamp | The date and time when an event occurred |

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
> If Elastic Agent is not installed or not running, Lab 03 may need to be reviewed.

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

# Task 4 - Open Kibana from Windows

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

Capture a screenshot showing that Kibana is open.

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

# Task 5 - Open Kibana Discover

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

# Task 6 - Select the Logs Data View

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Locate the data view selector in Discover.
2. Select a logs-related data view.

Common data view names may include:

### Copy

```text
logs-*
```

### Type

Look for or select this data view:

```text
logs-*
```

Alternative possible names:

### Copy

```text
winlogbeat-*
metrics-*
```

### Type

Look for one of these if `logs-*` is not available:

```text
winlogbeat-*
metrics-*
```

3. Record the data view selected.

## Expected Result

A logs-related data view should be selected.

> [!note]
> In Elastic Agent environments, `logs-*` is commonly used. Some lab images may use a different data view.

## Screenshot Checkpoint

Capture a screenshot showing the selected data view.

## Record in Evidence Notes

### Copy

```text
Data view selected:
```

### Type

Type this into your evidence notes, then add the selected data view:

```text
Data view selected:
```

---

# Task 7 - Set the Time Range to Last 24 Hours

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Find the time picker in the top-right area of Discover.
2. Select the time range.
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

The time range should show:

```text
Last 24 hours
```

> [!hint]
> If no events appear, try expanding the time range later to `Last 7 days`.

## Screenshot Checkpoint

Capture a screenshot showing the time range set to Last 24 hours.

## Record in Evidence Notes

### Copy

```text
Time range used:
```

### Type

Type this into your evidence notes:

```text
Time range used:
```

Example:

```text
Time range used: Last 24 hours
```

---

# Task 8 - Search for Events from WIN11-CLIENT

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Click the Kibana query bar.
2. Enter the host name query.

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

If still no events appear, try this broader query:

### Copy

```text
message : *WIN11-CLIENT*
```

### Type

Type this alternate query into the Kibana query bar:

```text
message : *WIN11-CLIENT*
```

## Expected Result

Events from WIN11-CLIENT should appear in Discover.

You may see fields such as:

```text
@timestamp
host.name
agent.name
event.dataset
event.provider
event.code
message
```

## Screenshot Checkpoint

Capture a screenshot showing events from WIN11-CLIENT in Discover.

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

# Task 9 - Add Useful Fields to the Discover Table

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. In Discover, locate the field list.
2. Search for the timestamp field.

### Copy

```text
@timestamp
```

### Type

Type or search for this field:

```text
@timestamp
```

3. Add the field to the table if it is not already visible.
4. Search for the host name field.

### Copy

```text
host.name
```

### Type

Type or search for this field:

```text
host.name
```

5. Add the field to the table.
6. Search for the event provider field.

### Copy

```text
event.provider
```

### Type

Type or search for this field:

```text
event.provider
```

7. Add the field to the table.
8. Search for the event code field.

### Copy

```text
event.code
```

### Type

Type or search for this field:

```text
event.code
```

9. Add the field to the table.
10. Search for the message field.

### Copy

```text
message
```

### Type

Type or search for this field:

```text
message
```

11. Add the field to the table if available.

## Expected Result

The Discover table should show useful event fields.

Recommended fields:

```text
@timestamp
host.name
event.provider
event.code
message
```

> [!note]
> Some fields may not be present in every event. If a field is unavailable, record that it was not visible.

## Screenshot Checkpoint

Capture a screenshot showing the Discover table with useful fields.

## Record in Evidence Notes

### Copy

```text
Fields added to Discover table:
```

### Type

Type this into your evidence notes, then list the fields:

```text
Fields added to Discover table:
```

---

# Task 10 - Search for Windows Security Events

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Keep the time range set to **Last 24 hours**.
2. Enter a query for Windows Security events.

### Copy

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Security-Auditing"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Security-Auditing"
```

3. Press **Enter** or select **Update**.
4. Review the results.

If no results appear, try this alternate query:

### Copy

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Security"
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Security"
```

If still no results appear, try this broader query:

### Copy

```text
host.name : "WIN11-CLIENT" and message : *Security*
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and message : *Security*
```

## Expected Result

You should see Windows Security-related events if they are being collected.

Common Security event IDs may include logon, audit, or policy-related events.

> [!note]
> The exact events depend on the lab image and recent activity.

## Screenshot Checkpoint

Capture a screenshot showing Security events or the query attempt.

## Record in Evidence Notes

### Copy

```text
Security event query used:
Security events found:
Example Security event ID:
Example Security provider:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Security event query used:
Security events found:
Example Security event ID:
Example Security provider:
```

---

# Task 11 - Search for Windows System Events

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Enter a query for Windows System events.

### Copy

```text
host.name : "WIN11-CLIENT" and winlog.channel : "System"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and winlog.channel : "System"
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try this alternate query:

### Copy

```text
host.name : "WIN11-CLIENT" and event.dataset : "windows.system"
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.dataset : "windows.system"
```

If still no results appear, try this broader query:

### Copy

```text
host.name : "WIN11-CLIENT" and message : *service*
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and message : *service*
```

## Expected Result

You should see Windows System events if they are being collected.

System events may include service, driver, startup, shutdown, or system component messages.

## Screenshot Checkpoint

Capture a screenshot showing System events or the query attempt.

## Record in Evidence Notes

### Copy

```text
System event query used:
System events found:
Example System event ID:
Example System provider:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
System event query used:
System events found:
Example System event ID:
Example System provider:
```

---

# Task 12 - Search for Windows Application Events

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Enter a query for Windows Application events.

### Copy

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Application"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Application"
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try this alternate query:

### Copy

```text
host.name : "WIN11-CLIENT" and event.dataset : "windows.application"
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.dataset : "windows.application"
```

If still no results appear, try this broader query:

### Copy

```text
host.name : "WIN11-CLIENT" and message : *Application*
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and message : *Application*
```

## Expected Result

You should see Windows Application events if they are being collected.

Application events may include software, service, or application messages.

## Screenshot Checkpoint

Capture a screenshot showing Application events or the query attempt.

## Record in Evidence Notes

### Copy

```text
Application event query used:
Application events found:
Example Application event ID:
Example Application provider:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Application event query used:
Application events found:
Example Application event ID:
Example Application provider:
```

---

# Task 13 - Search for Windows PowerShell Events

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Enter a query for PowerShell-related Windows events.

### Copy

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-PowerShell"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-PowerShell"
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try this alternate query:

### Copy

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Windows PowerShell"
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Windows PowerShell"
```

If still no results appear, try this query:

### Copy

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Microsoft-Windows-PowerShell/Operational"
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Microsoft-Windows-PowerShell/Operational"
```

## Expected Result

PowerShell events may appear if PowerShell logging is configured and recent activity exists.

> [!note]
> It is acceptable if no PowerShell events appear yet. Record the result.

## Screenshot Checkpoint

Capture a screenshot showing PowerShell events or the query attempt.

## Record in Evidence Notes

### Copy

```text
PowerShell event query used:
PowerShell events found:
Example PowerShell event ID:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
PowerShell event query used:
PowerShell events found:
Example PowerShell event ID:
```

---

# Task 14 - Open a Single Event in Kibana

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. In Discover, use a query that returns events from WIN11-CLIENT.

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
3. Select one event from the results table.
4. Open the event details.
5. Review the available fields.

Look for these fields if available:

### Copy

```text
@timestamp
host.name
event.provider
event.code
winlog.channel
message
agent.name
```

### Type

Look for these fields in the event details:

```text
@timestamp
host.name
event.provider
event.code
winlog.channel
message
agent.name
```

## Expected Result

You should be able to open a single event and view its field details.

## Screenshot Checkpoint

Capture a screenshot showing the open event details panel.

## Record in Evidence Notes

### Copy

```text
Selected Kibana event timestamp:
Selected Kibana event provider:
Selected Kibana event ID:
Selected Kibana event channel:
Selected Kibana event message summary:
```

### Type

Type these lines into your evidence notes, then add values from the event:

```text
Selected Kibana event timestamp:
Selected Kibana event provider:
Selected Kibana event ID:
Selected Kibana event channel:
Selected Kibana event message summary:
```

---

# Task 15 - Open Windows Event Viewer

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
4. Wait for Event Viewer to load.

## Expected Result

Windows Event Viewer should open.

You should see the left navigation tree.

## Screenshot Checkpoint

Capture a screenshot showing Event Viewer open.

## Record in Evidence Notes

### Copy

```text
Event Viewer opened:
```

### Type

Type this into your evidence notes:

```text
Event Viewer opened:
```

---

# Task 16 - Review the Local Windows Security Log

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows Event Viewer**.

## Steps

1. In Event Viewer, expand **Windows Logs**.
2. Select **Security**.

### Copy

```text
Windows Logs > Security
```

### Type

Navigate to this path in Event Viewer:

```text
Windows Logs > Security
```

3. Select one recent event.
4. Review the event details.
5. Record the event ID and time.

## Expected Result

You should see Security events in Event Viewer.

A selected event should show:

```text
Log Name
Source
Event ID
Level
Logged
Computer
```

## Screenshot Checkpoint

Capture a screenshot showing a selected Security event in Event Viewer.

## Record in Evidence Notes

### Copy

```text
Event Viewer Security event ID:
Event Viewer Security event time:
Event Viewer Security source:
```

### Type

Type these lines into your evidence notes, then add values:

```text
Event Viewer Security event ID:
Event Viewer Security event time:
Event Viewer Security source:
```

---

# Task 17 - Review the Local Windows System Log

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows Event Viewer**.

## Steps

1. In Event Viewer, expand **Windows Logs**.
2. Select **System**.

### Copy

```text
Windows Logs > System
```

### Type

Navigate to this path in Event Viewer:

```text
Windows Logs > System
```

3. Select one recent event.
4. Review the event details.
5. Record the event ID and time.

## Expected Result

You should see System events in Event Viewer.

## Screenshot Checkpoint

Capture a screenshot showing a selected System event in Event Viewer.

## Record in Evidence Notes

### Copy

```text
Event Viewer System event ID:
Event Viewer System event time:
Event Viewer System source:
```

### Type

Type these lines into your evidence notes, then add values:

```text
Event Viewer System event ID:
Event Viewer System event time:
Event Viewer System source:
```

---

# Task 18 - Compare Event Viewer and Kibana

## Where to Work

Use **WIN11-CLIENT**.

Use **Event Viewer** and **Kibana Discover**.

## Steps

1. Choose one event from Event Viewer.
2. Record the event log name.
3. Record the event ID.
4. Record the event time.
5. Return to Kibana Discover.
6. Search for the same or similar event ID.

Replace `<EVENT_ID>` with the event ID from Event Viewer.

### Copy

```text
host.name : "WIN11-CLIENT" and event.code : "<EVENT_ID>"
```

### Type

Type this into the Kibana query bar, replacing `<EVENT_ID>` with your event ID:

```text
host.name : "WIN11-CLIENT" and event.code : "<EVENT_ID>"
```

Example:

```text
host.name : "WIN11-CLIENT" and event.code : "7036"
```

7. Press **Enter** or select **Update**.
8. Review the results.
9. Compare the Kibana event timestamp with the Event Viewer timestamp.
10. Compare the event provider or source.
11. Compare the event message.

If no results appear, try the alternate field:

### Copy

```text
host.name : "WIN11-CLIENT" and winlog.event_id : <EVENT_ID>
```

### Type

Type this into the Kibana query bar, replacing `<EVENT_ID>` with your event ID:

```text
host.name : "WIN11-CLIENT" and winlog.event_id : <EVENT_ID>
```

Example:

```text
host.name : "WIN11-CLIENT" and winlog.event_id : 7036
```

## Expected Result

You should find either:

```text
A matching event
```

or:

```text
A similar event from the same log source or provider
```

> [!note]
> Timestamps may not look exactly the same because Kibana may display times in a different time zone or format.

## Screenshot Checkpoint

Capture a screenshot showing the Event Viewer event.

Capture a screenshot showing the Kibana search result.

## Record in Evidence Notes

### Copy

```text
Compared Event Viewer event ID:
Compared Kibana query:
Matching or similar Kibana event found:
Timestamp comparison:
Provider/source comparison:
Message comparison:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Compared Event Viewer event ID:
Compared Kibana query:
Matching or similar Kibana event found:
Timestamp comparison:
Provider/source comparison:
Message comparison:
```

---

# Task 19 - Use a Broader Windows Log Query

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. In Discover, run a broader query for Windows logs.

### Copy

```text
host.name : "WIN11-CLIENT" and event.module : "windows"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.module : "windows"
```

2. Press **Enter** or select **Update**.
3. Review the results.

If no results appear, try:

### Copy

```text
host.name : "WIN11-CLIENT" and event.dataset : windows.*
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.dataset : windows.*
```

If still no results appear, return to:

### Copy

```text
host.name : "WIN11-CLIENT"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT"
```

## Expected Result

You should see Windows-related events from WIN11-CLIENT.

## Screenshot Checkpoint

Capture a screenshot of the broader Windows log query results.

## Record in Evidence Notes

### Copy

```text
Broad Windows log query used:
Broad Windows log query result:
```

### Type

Type these lines into your evidence notes, then add your values:

```text
Broad Windows log query used:
Broad Windows log query result:
```

---

# Task 20 - Check the Optional Sample Windows Events File

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Check whether the optional sample Windows events file exists.

### Copy

```powershell
Test-Path "C:\LabFiles\Logs\sample-windows-events.csv"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\LabFiles\Logs\sample-windows-events.csv"
```

2. Press **Enter**.
3. If the result is `True`, list the file details.

### Copy

```powershell
Get-Item "C:\LabFiles\Logs\sample-windows-events.csv"
```

### Type

Type this into PowerShell:

```powershell
Get-Item "C:\LabFiles\Logs\sample-windows-events.csv"
```

4. Press **Enter**.

## Expected Result

The file may exist as a fallback evidence file.

The `Test-Path` command may return:

```text
True
```

or:

```text
False
```

> [!note]
> The sample file is optional. Live Kibana events are preferred.

## Screenshot Checkpoint

Capture a screenshot only if your instructor asks you to verify fallback files.

## Record in Evidence Notes

### Copy

```text
Sample Windows events file checked:
Sample Windows events file exists:
```

### Type

Type these lines into your evidence notes, then add the result:

```text
Sample Windows events file checked:
Sample Windows events file exists:
```

---

# Task 21 - Create the Lab 04 Evidence Notes File

## Where to Work

Use **WIN11-CLIENT**.

Use **Notepad**.

## Steps

1. Open **Notepad**.
2. Copy or type the evidence template below.
3. Fill in the missing information using your lab results.
4. Select **File**.
5. Select **Save As**.
6. Browse to the Windows evidence folder.

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
Lab04-Windows-Log-Summary.txt
```

### Type

Type this filename exactly:

```text
Lab04-Windows-Log-Summary.txt
```

## Evidence Template

### Copy

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 04 - Collecting and Reviewing Windows Event Logs

Student Name:
Date:

1. Environment Verification

Windows hostname:
Elastic Agent service status:
Kibana URL used:
Discover opened:
Data view selected:
Time range used:

2. Host Event Search

Host query used:
Events from WIN11-CLIENT found:
Fields added to Discover table:

3. Windows Security Events

Security event query used:
Security events found:
Example Security event ID:
Example Security provider:

4. Windows System Events

System event query used:
System events found:
Example System event ID:
Example System provider:

5. Windows Application Events

Application event query used:
Application events found:
Example Application event ID:
Example Application provider:

6. Windows PowerShell Events

PowerShell event query used:
PowerShell events found:
Example PowerShell event ID:

7. Selected Kibana Event

Selected Kibana event timestamp:
Selected Kibana event provider:
Selected Kibana event ID:
Selected Kibana event channel:
Selected Kibana event message summary:

8. Event Viewer Comparison

Event Viewer opened:
Event Viewer Security event ID:
Event Viewer Security event time:
Event Viewer Security source:
Event Viewer System event ID:
Event Viewer System event time:
Event Viewer System source:

Compared Event Viewer event ID:
Compared Kibana query:
Matching or similar Kibana event found:
Timestamp comparison:
Provider/source comparison:
Message comparison:

9. Optional Fallback File

Sample Windows events file checked:
Sample Windows events file exists:

10. Lab Summary

Write 3 to 5 sentences explaining what you learned about Windows Event Logs and how they appear in Kibana.
```

### Type

Type this template into Notepad manually:

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 04 - Collecting and Reviewing Windows Event Logs

Student Name:
Date:

1. Environment Verification

Windows hostname:
Elastic Agent service status:
Kibana URL used:
Discover opened:
Data view selected:
Time range used:

2. Host Event Search

Host query used:
Events from WIN11-CLIENT found:
Fields added to Discover table:

3. Windows Security Events

Security event query used:
Security events found:
Example Security event ID:
Example Security provider:

4. Windows System Events

System event query used:
System events found:
Example System event ID:
Example System provider:

5. Windows Application Events

Application event query used:
Application events found:
Example Application event ID:
Example Application provider:

6. Windows PowerShell Events

PowerShell event query used:
PowerShell events found:
Example PowerShell event ID:

7. Selected Kibana Event

Selected Kibana event timestamp:
Selected Kibana event provider:
Selected Kibana event ID:
Selected Kibana event channel:
Selected Kibana event message summary:

8. Event Viewer Comparison

Event Viewer opened:
Event Viewer Security event ID:
Event Viewer Security event time:
Event Viewer Security source:
Event Viewer System event ID:
Event Viewer System event time:
Event Viewer System source:

Compared Event Viewer event ID:
Compared Kibana query:
Matching or similar Kibana event found:
Timestamp comparison:
Provider/source comparison:
Message comparison:

9. Optional Fallback File

Sample Windows events file checked:
Sample Windows events file exists:

10. Lab Summary

Write 3 to 5 sentences explaining what you learned about Windows Event Logs and how they appear in Kibana.
```

## Expected Result

The evidence notes file should be saved at:

```text
C:\BlueWave\Evidence\Lab04-Windows-Log-Summary.txt
```

## Screenshot Checkpoint

Capture a screenshot showing the completed Lab 04 evidence notes file.

---

# Task 22 - Confirm the Lab 04 Evidence File Exists

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Open **Windows PowerShell**.
2. Run the validation command.

### Copy

```powershell
Test-Path "C:\BlueWave\Evidence\Lab04-Windows-Log-Summary.txt"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab04-Windows-Log-Summary.txt"
```

3. Press **Enter**.

## Expected Result

PowerShell should return:

```text
True
```

## Screenshot Checkpoint

Capture a screenshot showing the validation result if required.

---

# Task 23 - Final Validation

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana**, **Event Viewer**, **PowerShell**, and **File Explorer**.

## Steps

1. Confirm Discover shows events from WIN11-CLIENT.
2. Confirm you attempted Security log searches.
3. Confirm you attempted System log searches.
4. Confirm you attempted Application log searches.
5. Confirm you opened at least one event in Kibana.
6. Confirm you opened Event Viewer.
7. Confirm you compared one Event Viewer event with Kibana.
8. Confirm your Lab 04 evidence file exists.

### Copy

```powershell
Test-Path "C:\BlueWave\Evidence\Lab04-Windows-Log-Summary.txt"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab04-Windows-Log-Summary.txt"
```

## Expected Result

You should have:

```text
Events from WIN11-CLIENT visible in Discover
Windows log queries attempted
Event Viewer opened
One event compared between Event Viewer and Kibana
Lab04-Windows-Log-Summary.txt saved
```

## Screenshot Checkpoint

Capture any final screenshots required by your instructor.

---

# Validation Checklist

Before finishing the lab, confirm each item is complete.

- [ ] I confirmed WIN11-CLIENT is available.
- [ ] I confirmed the Windows evidence folder exists.
- [ ] I confirmed Elastic Agent is running.
- [ ] I opened Kibana from WIN11-CLIENT.
- [ ] I opened Discover.
- [ ] I selected a logs data view.
- [ ] I set the time range to Last 24 hours.
- [ ] I searched for events from WIN11-CLIENT.
- [ ] I added useful fields to the Discover table.
- [ ] I searched for Security events.
- [ ] I searched for System events.
- [ ] I searched for Application events.
- [ ] I searched for PowerShell events.
- [ ] I opened a single event in Kibana.
- [ ] I opened Windows Event Viewer.
- [ ] I reviewed a local Security event.
- [ ] I reviewed a local System event.
- [ ] I compared one Event Viewer event with Kibana.
- [ ] I checked the optional sample Windows events file.
- [ ] I created `Lab04-Windows-Log-Summary.txt`.
- [ ] I captured the required screenshots.

---

# Troubleshooting

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

If Kibana still does not open, confirm Kibana is running on UBUNTU-SOC or ask your instructor.

---

## Problem: Discover is not visible

Use the Kibana navigation search.

### Copy

```text
Discover
```

### Type

Type this into the Kibana navigation search:

```text
Discover
```

If Discover is still unavailable, ask your instructor to confirm your Kibana permissions.

---

## Problem: No data view is available

Look for logs-related data views.

### Copy

```text
logs-*
winlogbeat-*
```

### Type

Look for these data views:

```text
logs-*
winlogbeat-*
```

If no data view exists, ask your instructor whether a data view must be created or refreshed.

---

## Problem: No events appear from WIN11-CLIENT

Check the time range.

Set the time range to:

### Copy

```text
Last 24 hours
```

### Type

Select or type:

```text
Last 24 hours
```

Try expanding to:

### Copy

```text
Last 7 days
```

### Type

Select or type:

```text
Last 7 days
```

Try alternate queries:

### Copy

```text
host.name : "WIN11-CLIENT"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT"
```

### Copy

```text
agent.name : "WIN11-CLIENT"
```

### Type

Type this into the Kibana query bar:

```text
agent.name : "WIN11-CLIENT"
```

### Copy

```text
message : *WIN11-CLIENT*
```

### Type

Type this into the Kibana query bar:

```text
message : *WIN11-CLIENT*
```

---

## Problem: Elastic Agent is not running

Check the service.

### Copy

```powershell
Get-Service elastic-agent
```

### Type

Type this into PowerShell:

```powershell
Get-Service elastic-agent
```

If the service is not found or not running, review Lab 03 or ask your instructor.

---

## Problem: Security events do not appear

Try alternate Security queries.

### Copy

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Security"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Security"
```

### Copy

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Security-Auditing"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Security-Auditing"
```

If no results appear, record the issue.

---

## Problem: System or Application events do not appear

Try alternate queries.

### Copy

```text
host.name : "WIN11-CLIENT" and winlog.channel : "System"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and winlog.channel : "System"
```

### Copy

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Application"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Application"
```

If no results appear, record the issue.

---

## Problem: Field names are different

Elastic field names may vary by configuration.

Try alternate fields such as:

### Copy

```text
event.code
winlog.event_id
event.provider
winlog.provider_name
winlog.channel
event.dataset
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
message
```

---

## Problem: Event Viewer does not open

Search for Event Viewer from the Windows Start menu.

### Copy

```text
Event Viewer
```

### Type

Type this into the Windows search box:

```text
Event Viewer
```

Open **Event Viewer**.

---

## Problem: Event Viewer and Kibana timestamps do not match exactly

Possible reasons:

- Kibana may display timestamps in a different time zone.
- Event ingestion may add extra timestamps.
- The event may be similar but not identical.
- The query time range may exclude the event.

Record what you observe.

---

## Problem: The Lab 04 evidence file is missing

Check that you saved it as:

### Copy

```text
Lab04-Windows-Log-Summary.txt
```

### Type

Type this filename exactly:

```text
Lab04-Windows-Log-Summary.txt
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
Test-Path "C:\BlueWave\Evidence\Lab04-Windows-Log-Summary.txt"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab04-Windows-Log-Summary.txt"
```

Expected result:

```text
True
```

---

# Knowledge Check

Answer the following questions.

1. What Kibana page is used to search Windows Event Logs?
2. What Windows tool is used to view local Windows events?
3. What query can search for events from WIN11-CLIENT?
4. What does `event.provider` usually describe?
5. What does `event.code` usually represent?
6. Name three common Windows log categories reviewed in this lab.
7. Why should you set the time range before searching in Discover?
8. Why might Event Viewer and Kibana timestamps look different?
9. What should you do if a Kibana query returns no results?
10. Why is it useful to compare Event Viewer and Kibana events?

---

# Summary

In this lab, you completed the following tasks:

- Confirmed Elastic Agent is running.
- Opened Kibana Discover.
- Selected a logs data view.
- Set the time range.
- Searched for events from WIN11-CLIENT.
- Reviewed Windows Security events.
- Reviewed Windows System events.
- Reviewed Windows Application events.
- Reviewed PowerShell-related events.
- Opened Event Viewer.
- Compared one local Windows event with one Kibana event.
- Created a Windows log collection summary.

You are now ready for Lab 05, where you will collect and review Sysmon process activity.

---

# Deliverables

Submit or retain the following items as directed by your instructor.

| Deliverable | Location |
|---|---|
| Lab 04 Windows log summary | `C:\BlueWave\Evidence\Lab04-Windows-Log-Summary.txt` |
| Screenshot of Discover with selected data view | Skillable submission area |
| Screenshot of Discover showing WIN11-CLIENT events | Skillable submission area |
| Screenshot of Security event query | Skillable submission area |
| Screenshot of System or Application event query | Skillable submission area |
| Screenshot of opened Kibana event details | Skillable submission area |
| Screenshot of Event Viewer Security or System event | Skillable submission area |
| Screenshot of Event Viewer and Kibana comparison | Skillable submission area |
| Screenshot of completed Lab 04 notes file | Skillable submission area |

---

# Instructor Notes

## Expected Knowledge Check Answers

1. Kibana Discover is used to search Windows Event Logs.
2. Windows Event Viewer is used to view local Windows events.
3. A useful query is:

```text
host.name : "WIN11-CLIENT"
```

4. `event.provider` describes the event source or provider that generated the event.
5. `event.code` usually represents the Windows Event ID or normalized event identifier.
6. Security, System, and Application are three common Windows log categories.
7. The time range controls which events are searched and displayed.
8. Kibana may display a different time zone or the event may have a different ingestion timestamp.
9. Check the time range, use alternate field names, broaden the query, and confirm logs are arriving.
10. Comparing Event Viewer and Kibana helps confirm local events are being collected and searchable in Elastic.

---

## Expected Evidence File

Students should create:

```text
C:\BlueWave\Evidence\Lab04-Windows-Log-Summary.txt
```

---

## Expected Elastic Queries

Primary host query:

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

Security queries:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-Security-Auditing"
```

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Security"
```

System queries:

```text
host.name : "WIN11-CLIENT" and winlog.channel : "System"
```

```text
host.name : "WIN11-CLIENT" and event.dataset : "windows.system"
```

Application queries:

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Application"
```

```text
host.name : "WIN11-CLIENT" and event.dataset : "windows.application"
```

PowerShell queries:

```text
host.name : "WIN11-CLIENT" and event.provider : "Microsoft-Windows-PowerShell"
```

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Windows PowerShell"
```

```text
host.name : "WIN11-CLIENT" and winlog.channel : "Microsoft-Windows-PowerShell/Operational"
```

Event ID comparison query:

```text
host.name : "WIN11-CLIENT" and event.code : "<EVENT_ID>"
```

Alternate Event ID comparison query:

```text
host.name : "WIN11-CLIENT" and winlog.event_id : <EVENT_ID>
```

---

## Expected Visible Results

Students should be able to show:

- Kibana Discover open.
- A logs data view selected.
- Time range set to Last 24 hours.
- Events from WIN11-CLIENT.
- At least one Windows event category query attempted.
- Event Viewer open.
- One Windows event reviewed locally.
- One Kibana event opened and reviewed.
- Lab 04 evidence file saved.

---

## Common Student Mistakes

| Mistake | Instructor Guidance |
|---|---|
| Student forgets to set the time range | Have them set Last 24 hours or Last 7 days |
| Student uses the wrong hostname | Have them run `hostname` in PowerShell |
| Student uses the wrong data view | Have them try `logs-*` or a logs-related data view |
| Student expects every event source to have results | Explain that some sources may depend on configuration and recent activity |
| Student confuses Event Viewer source with Kibana provider | Explain field normalization and naming differences |
| Student records `message` only and misses event ID | Ask them to record timestamp, provider, event ID, and channel |
| Student cannot match exact Event Viewer event | Allow comparison with a similar event if the exact event is not available |
| Student does not capture screenshots | Have them repeat the query and capture evidence |

---

## Grading Guidance

Suggested grading allocation:

| Criteria | Points |
|---|---:|
| Kibana Discover opened and data view selected | 10 |
| Time range set correctly | 10 |
| Host events found or search attempts documented | 15 |
| Security/System/Application searches completed | 20 |
| Event details reviewed in Kibana | 10 |
| Event Viewer opened and local event reviewed | 10 |
| Event Viewer and Kibana comparison completed | 15 |
| Evidence notes completed | 5 |
| Screenshots captured | 5 |
| Total | 100 |

Do not heavily penalise students if one event source returns no results, as long as they use the correct query, try alternates, and document the result.

---

## Free Elastic Basic License Reminder

This lab must use:

- Self-managed Elastic.
- Free Elastic Basic license.
- Kibana Discover.
- Windows Event Log collection.
- No Elastic Cloud.
- No paid subscriptions.
- No external internet access.

---

## Fallback Option if Live Windows Logs Are Unavailable

If live logs are unavailable, students may use the optional sample file if provided:

```text
C:\LabFiles\Logs\sample-windows-events.csv
```

Suggested fallback evidence line:

```text
Live Windows events unavailable. Fallback sample file checked: C:\LabFiles\Logs\sample-windows-events.csv
```

The preferred method is always live Kibana Discover evidence when available.

---

End of Lab 04.
