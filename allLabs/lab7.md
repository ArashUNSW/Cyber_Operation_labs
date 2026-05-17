# Lab 07 - Creating Simple Elastic Detection Logic

## Estimated Time

90–120 minutes

---

## Lab Purpose

In this lab, you will create simple detection logic in Elastic for safe endpoint activity.

You will use Kibana to build a basic detection based on activity from previous labs, such as the BlueWave Activity Simulator, `whoami`, and `hostname`.

You will then trigger safe activity, search for matching events, and document why the logic matched.

This lab introduces the difference between events, alerts, detections, and incidents.

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

- Explain the difference between an event, detection, alert, and incident.
- Search for simulator-related process events in Kibana.
- Search for `whoami` and `hostname` command activity.
- Create simple Elastic detection logic.
- Create a custom detection rule if the Security rules feature is available.
- Use a saved query or manual detection workflow if detection rules are unavailable.
- Trigger safe activity to test detection logic.
- Review matching events or alerts.
- Explain why the detection logic matched.
- Document detection notes and evidence.

---

## Scenario

BlueWave Clinic has collected Windows and Sysmon events from WIN11-CLIENT.

The SOC team wants to start building simple detection logic for investigation-worthy endpoint activity.

In earlier labs, you reviewed process activity such as:

- `BlueWaveActivitySimulator.exe`
- `whoami`
- `hostname`
- `ipconfig`
- `notepad.exe`
- `calc.exe`

In this lab, you will create simple detection logic for safe simulated activity.

Your detection logic will focus on activity that is useful for training and triage:

```text
BlueWaveActivitySimulator.exe
```

and command-line activity containing:

```text
whoami
hostname
```

> [!note]
> This lab uses safe educational activity only.

> [!note]
> The simulator is not malware.

> [!alert]
> Do not create detections for real malware or offensive activity in this lab.

> [!alert]
> Do not use Elastic Cloud. This course uses self-managed Elastic with the free Basic license only.

---

## Required Machines

| Machine | Used For |
|---|---|
| WIN11-CLIENT | Browser access to Kibana, safe activity generation, evidence notes |
| UBUNTU-SOC | Elasticsearch and Kibana services |
| Kibana browser session on WIN11-CLIENT | Detection logic, Discover searches, alerts or saved queries |

---

## Required Files

| File | Location | Purpose |
|---|---|---|
| BlueWaveActivitySimulator.exe | `C:\LabFiles\Simulators` | Safe educational activity generator |
| Lab07-Detection-Rule-Notes.txt | `C:\BlueWave\Evidence` | Student-created detection notes |

---

## Important Paths

### Windows Paths

| Path | Purpose |
|---|---|
| `C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe` | Safe simulator executable |
| `C:\BlueWave\Evidence` | Evidence folder |
| `C:\BlueWave\Evidence\Lab07-Detection-Rule-Notes.txt` | Lab 07 evidence notes |
| `C:\BlueWave\SimulatorOutput` | Simulator output folder |
| `C:\BlueWave\SimulatorOutput\activity-note.txt` | Simulator output file |

### Kibana Areas

| Kibana Area | Purpose |
|---|---|
| Discover | Search and validate events |
| Security | Detection rules and alerts, if available |
| Rules | Create or review custom detection rules |
| Alerts | Review generated alerts, if available |
| Saved query | Save a reusable KQL query if detection rules are unavailable |
| Data view selector | Select logs data |
| Time picker | Set event search range |
| KQL query bar | Enter detection logic |

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
| Elastic Agent status | Running |
| Sysmon status | Running |
| Simulator path | `C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe` |

> [!note]
> Your Ubuntu SOC IP address may be different. Use the actual IP address from your lab environment.

---

## Detection Logic Used in This Lab

You will use one primary detection query and several supporting queries.

### Primary Detection Query

This query searches for the BlueWave Activity Simulator process.

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

### Supporting Query 1

This query searches for `whoami` command-line activity.

```text
host.name : "WIN11-CLIENT" and process.command_line : *whoami*
```

### Supporting Query 2

This query searches for `hostname` command-line activity.

```text
host.name : "WIN11-CLIENT" and process.command_line : *hostname*
```

### Optional Combined Query

This query matches simulator activity or either command.

```text
host.name : "WIN11-CLIENT" and (process.name : "BlueWaveActivitySimulator.exe" or process.command_line : *whoami* or process.command_line : *hostname*)
```

> [!note]
> The primary query is simpler and is recommended for beginner students.

---

## Screenshots You Should Capture

Capture screenshots as instructed by your trainer or Skillable platform.

Recommended screenshots:

1. Elastic Agent service status.
2. Sysmon service status.
3. Discover showing the primary detection query results.
4. Discover showing `whoami` or `hostname` query results.
5. Detection rule creation screen, if available.
6. Saved query screen, if detection rules are unavailable.
7. Safe activity generation command.
8. Matching alert, if alerting is available.
9. Matching Discover event, if using fallback detection.
10. Completed Lab 07 evidence notes file.

---

## Key Terms

| Term | Meaning |
|---|---|
| Event | A recorded action or observation, such as a process starting |
| Detection logic | A query or rule designed to find activity of interest |
| Detection rule | A configured rule that searches for matching events |
| Alert | A notification or record created when a detection rule matches |
| Incident | A larger investigation that may include one or more alerts and evidence |
| KQL | Kibana Query Language |
| Saved query | A reusable search query saved in Kibana |
| False positive | An alert that looks suspicious but is not harmful |
| Triage | Initial review of an event or alert |
| Severity | A label describing how important an alert may be |
| Risk score | A number used to help prioritise alerts |

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

# Task 5 - Confirm the Simulator File Exists

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Check the simulator file path.

### Student Input - Copy or Type

```powershell
Test-Path "C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe"
```

2. Press **Enter**.
3. If the result is `True`, continue.

## Expected Result

PowerShell should return:

```text
True
```

> [!note]
> The simulator is safe educational activity. It is not malware.

## Screenshot Checkpoint

Capture a screenshot showing the simulator file path check if required.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Simulator file exists:
Simulator file path:
```

---

# Task 6 - Open Kibana from Windows

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

# Task 7 - Open Kibana Discover

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

# Task 8 - Select the Logs Data View and Time Range

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

# Task 9 - Review the Primary Detection Query in Discover

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Click the Kibana query bar.
2. Enter the primary detection query.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

3. Press **Enter** or select **Update**.
4. Review the results.

If no results appear, try this alternate query:

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

If simulator activity exists from Lab 06, matching events may appear.

If no events appear, you will generate safe activity later in this lab.

## Screenshot Checkpoint

Capture a screenshot showing the query and results or no-result state.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Primary detection query:
Primary query matched existing events:
```

---

# Task 10 - Review the whoami Supporting Query

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Enter the `whoami` detection query.

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

You may see events related to `whoami`.

If no events appear, you will generate safe activity later.

## Screenshot Checkpoint

Capture a screenshot showing the query and result.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
whoami detection query:
whoami query matched existing events:
```

---

# Task 11 - Review the hostname Supporting Query

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Enter the `hostname` detection query.

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

You may see events related to `hostname`.

If no events appear, you will generate safe activity later.

## Screenshot Checkpoint

Capture a screenshot showing the query and result.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
hostname detection query:
hostname query matched existing events:
```

---

# Task 12 - Decide Which Detection Workflow Is Available

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana**.

## Steps

1. Open the Kibana main navigation menu.
2. Search for **Rules**.

### Student Input - Copy or Type

```text
Rules
```

3. Also search for **Alerts** if needed.

### Student Input - Copy or Type

```text
Alerts
```

4. If Security detection rules are available, continue to Task 13.
5. If Security detection rules are not available, use the saved query fallback starting at Task 18.

## Expected Result

One of the following should be true:

```text
Detection rules are available.
```

or:

```text
Detection rules are unavailable. Saved query fallback will be used.
```

> [!note]
> Some lab images may not expose detection rule creation. The fallback workflow is acceptable.

## Screenshot Checkpoint

Capture a screenshot showing the Rules area or the unavailable feature state.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Detection rules available:
Fallback workflow required:
```

---

# Task 13 - Open the Detection Rules Page

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Security**, if available.

## Steps

Complete this task only if detection rules are available.

1. Open the Kibana main navigation menu.
2. Open **Security** or **Rules**.
3. Navigate to the detection rules page.

Possible navigation names include:

### Student Input - Copy or Type

```text
Security
Rules
Detection rules
```

4. Select **Create rule** or **Create new rule**.

### Student Input - Copy or Type

```text
Create rule
```

## Expected Result

The rule creation workflow should open.

## Screenshot Checkpoint

Capture a screenshot showing the rule creation workflow.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Detection rule creation page opened:
```

---

# Task 14 - Create a Custom Query Detection Rule

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Security detection rule creation**, if available.

## Steps

Complete this task only if detection rules are available.

1. Select the custom query rule type if prompted.

### Student Input - Copy or Type

```text
Custom query
```

2. Enter the rule name.

### Student Input - Copy or Type

```text
BlueWave - Simulator Process Activity
```

3. Enter the rule description.

### Student Input - Copy or Type

```text
Detects safe BlueWave Activity Simulator process execution on WIN11-CLIENT for training and triage practice.
```

4. Enter the index pattern or data view if required.

### Student Input - Copy or Type

```text
logs-*
```

5. Enter the KQL query.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

6. Set the severity.

### Student Input - Copy or Type

```text
Low
```

7. Set the risk score if required.

### Student Input - Copy or Type

```text
21
```

8. Set the rule interval if required.

### Student Input - Copy or Type

```text
5 minutes
```

9. Set the look-back time if required.

### Student Input - Copy or Type

```text
10 minutes
```

10. Save the rule.
11. Enable the rule if prompted.

## Expected Result

A custom detection rule should be created.

The rule should use the simulator process query.

> [!note]
> Exact fields and screens may vary by Elastic version.

## Screenshot Checkpoint

Capture a screenshot showing the completed rule settings or saved rule.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Detection rule name:
Detection rule query:
Detection rule severity:
Detection rule risk score:
Detection rule interval:
Detection rule look-back:
Detection rule enabled:
```

---

# Task 15 - Add Rule Tags or Investigation Notes

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Security detection rule creation**, if available.

## Steps

Complete this task only if detection rules are available and the screen allows tags or notes.

1. Add a tag if the rule supports tags.

### Student Input - Copy or Type

```text
BlueWave
```

2. Add another tag if supported.

### Student Input - Copy or Type

```text
Training
```

3. Add investigation guidance if supported.

### Student Input - Copy or Type

```text
Review the process name, parent process, command line, user, timestamp, and related child processes. Confirm whether the activity was generated by the approved BlueWave lab simulator.
```

## Expected Result

The detection rule should include simple tags or investigation guidance if the fields are available.

> [!note]
> If tags or investigation notes are not available, record that the option was not visible and continue.

## Screenshot Checkpoint

Capture a screenshot if tags or investigation guidance were added.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Detection rule tags added:
Investigation guidance added:
```

---

# Task 16 - Save and Enable the Detection Rule

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Security detection rule creation**, if available.

## Steps

Complete this task only if detection rules are available.

1. Review the rule settings.
2. Confirm the query is correct.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

3. Save the rule.
4. Enable the rule if it is not already enabled.

## Expected Result

The rule should be saved and enabled.

## Screenshot Checkpoint

Capture a screenshot showing the saved and enabled rule.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Detection rule saved:
Detection rule enabled:
```

---

# Task 17 - Run the Detection Rule Manually if Available

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Security**, if available.

## Steps

Complete this task only if detection rules are available and manual run is available.

1. Open the saved detection rule.
2. Look for **Run rule** or **Manual run**.

### Student Input - Copy or Type

```text
Run rule
Manual run
```

3. If available, run the rule over the last 24 hours.

### Student Input - Copy or Type

```text
Last 24 hours
```

4. Wait for the rule run to complete.
5. Review whether alerts were generated.

## Expected Result

If matching simulator activity exists, alerts may be generated.

If no matching activity exists yet, no alerts may appear.

> [!note]
> If manual run is unavailable, continue to the next task and generate safe activity.

## Screenshot Checkpoint

Capture a screenshot showing manual run results if available.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Manual rule run available:
Manual rule run completed:
Alerts generated during manual run:
```

---

# Task 18 - Fallback: Save the Primary Query in Discover

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

Complete this task if detection rules are unavailable.

1. Open **Discover**.
2. Enter the primary detection query.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

3. Set the time range to **Last 24 hours**.

### Student Input - Copy or Type

```text
Last 24 hours
```

4. Save the query if the saved query option is available.
5. Name the saved query.

### Student Input - Copy or Type

```text
BlueWave Simulator Process Detection Query
```

6. If saved queries are not available, record the query as a manual detection query in your evidence notes.

## Expected Result

One of the following should be true:

```text
Saved query created.
```

or:

```text
Manual detection query documented.
```

> [!note]
> This fallback is acceptable if detection rule creation is unavailable.

## Screenshot Checkpoint

Capture a screenshot showing the saved query or the manual query in Discover.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Saved query fallback used:
Saved query name:
Manual detection query documented:
```

---

# Task 19 - Fallback: Save or Document a whoami Query

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

Complete this task if detection rules are unavailable.

1. Enter the `whoami` query in Discover.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.command_line : *whoami*
```

2. Save the query if available.
3. Name the saved query.

### Student Input - Copy or Type

```text
BlueWave whoami Activity Query
```

4. If saved queries are unavailable, document the query manually.

## Expected Result

The `whoami` query should be saved or documented.

## Screenshot Checkpoint

Capture a screenshot showing the query.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
whoami saved query created:
whoami saved query name:
whoami manual query documented:
```

---

# Task 20 - Fallback: Save or Document a hostname Query

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

Complete this task if detection rules are unavailable.

1. Enter the `hostname` query in Discover.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.command_line : *hostname*
```

2. Save the query if available.
3. Name the saved query.

### Student Input - Copy or Type

```text
BlueWave hostname Activity Query
```

4. If saved queries are unavailable, document the query manually.

## Expected Result

The `hostname` query should be saved or documented.

## Screenshot Checkpoint

Capture a screenshot showing the query.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
hostname saved query created:
hostname saved query name:
hostname manual query documented:
```

---

# Task 21 - Generate Safe Test Activity

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Return to Windows PowerShell.
2. Record the current time.

### Student Input - Copy or Type

```powershell
Get-Date
```

3. Press **Enter**.
4. Run `whoami`.

### Student Input - Copy or Type

```powershell
whoami
```

5. Press **Enter**.
6. Run `hostname`.

### Student Input - Copy or Type

```powershell
hostname
```

7. Press **Enter**.
8. Run the BlueWave Activity Simulator.

### Student Input - Copy or Type

```powershell
Start-Process -FilePath "C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe"
```

9. Press **Enter**.
10. Wait for the simulator to complete.
11. Record the current time again.

### Student Input - Copy or Type

```powershell
Get-Date
```

12. Press **Enter**.

## Expected Result

The commands should run safely.

The simulator should run normally.

You should have a time window for the generated test activity.

> [!note]
> The simulator is safe educational activity and is not malware.

## Screenshot Checkpoint

Capture a screenshot showing the safe activity commands.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Safe test activity generated:
Test activity start time:
Test activity commands:
Test activity end time:
```

---

# Task 22 - Wait for Events to Arrive in Elastic

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana**.

## Steps

1. Return to Kibana Discover.
2. Set the time range to **Last 15 minutes**.

### Student Input - Copy or Type

```text
Last 15 minutes
```

3. Refresh Discover.
4. If no events appear, set the time range to **Last 1 hour**.

### Student Input - Copy or Type

```text
Last 1 hour
```

5. Refresh again.

## Expected Result

Recent activity should appear after a short ingestion delay.

> [!note]
> Elastic Agent may take a short time to send new events.

## Screenshot Checkpoint

Capture a screenshot if required.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Post-test time range used:
Events refreshed after test activity:
```

---

# Task 23 - Verify the Simulator Detection Match in Discover

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Run the primary detection query again.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

2. Press **Enter** or select **Update**.
3. Review whether a recent simulator event appears.
4. Open one matching event.
5. Review the timestamp, user, process, parent process, and command line.

## Expected Result

A recent event should match the detection query.

If no event appears, try the alternate queries from Task 9.

## Screenshot Checkpoint

Capture a screenshot showing the simulator query match.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Simulator detection matched after test:
Matching simulator event timestamp:
Matching simulator event user:
Matching simulator parent process:
Why the simulator query matched:
```

---

# Task 24 - Verify the whoami Detection Match in Discover

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Run the `whoami` query again.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.command_line : *whoami*
```

2. Press **Enter** or select **Update**.
3. Review whether a recent `whoami` event appears.
4. Open one matching event if available.
5. Review the timestamp, user, process, parent process, and command line.

If no event appears, try:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *whoami*
```

## Expected Result

A recent event should match the `whoami` query.

## Screenshot Checkpoint

Capture a screenshot showing the `whoami` query match.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
whoami detection matched after test:
Matching whoami event timestamp:
Matching whoami event user:
Matching whoami parent process:
Why the whoami query matched:
```

---

# Task 25 - Verify the hostname Detection Match in Discover

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Run the `hostname` query again.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.command_line : *hostname*
```

2. Press **Enter** or select **Update**.
3. Review whether a recent `hostname` event appears.
4. Open one matching event if available.
5. Review the timestamp, user, process, parent process, and command line.

If no event appears, try:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *hostname*
```

## Expected Result

A recent event should match the `hostname` query.

## Screenshot Checkpoint

Capture a screenshot showing the `hostname` query match.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
hostname detection matched after test:
Matching hostname event timestamp:
Matching hostname event user:
Matching hostname parent process:
Why the hostname query matched:
```

---

# Task 26 - Review Alerts if Detection Rules Are Available

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Security**, if available.

## Steps

Complete this task only if detection rules are available and the rule was enabled.

1. Open the Kibana main navigation menu.
2. Search for **Alerts**.

### Student Input - Copy or Type

```text
Alerts
```

3. Open **Alerts**.
4. Set the time range to **Last 24 hours**.

### Student Input - Copy or Type

```text
Last 24 hours
```

5. Search or filter for the rule name.

### Student Input - Copy or Type

```text
BlueWave - Simulator Process Activity
```

6. Review any generated alerts.
7. Open one alert if available.

## Expected Result

If the detection rule ran successfully and matched recent events, an alert may appear.

If no alert appears, continue using Discover evidence.

> [!note]
> Alert generation can depend on rule schedule, permissions, and Elastic configuration.

## Screenshot Checkpoint

Capture a screenshot showing the alert or no-alert result.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Alerts page reviewed:
Alert generated:
Alert rule name:
Alert timestamp:
Alert host:
Alert user:
```

---

# Task 27 - Explain the Detection Match

## Where to Work

Use **WIN11-CLIENT**.

Use your evidence notes.

## Steps

1. Review the detection query.
2. Review the matching event.
3. Identify the reason the query matched.
4. Write a short explanation.

Use this explanation structure:

### Student Input - Copy or Type

```text
The detection matched because the event was generated on WIN11-CLIENT and the process or command line matched the detection query. The activity is safe lab activity, but it is useful for SOC training because it shows how endpoint process events can be detected and reviewed.
```

## Expected Result

Your evidence notes should explain why the detection matched.

## Screenshot Checkpoint

No screenshot is required unless instructed.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Detection match explanation:
```

---

# Task 28 - Classify the Detection Result

## Where to Work

Use **WIN11-CLIENT**.

Use your evidence notes.

## Steps

1. Review the matching event or alert.
2. Decide how the result should be classified.

Possible classifications:

### Student Input - Copy or Type

```text
Expected training activity
Suspicious-looking but authorised
Needs triage
False positive
```

3. For this lab, choose the best classification.
4. Explain your choice.

## Expected Result

For simulator activity, the expected classification is usually:

```text
Suspicious-looking but authorised
```

or:

```text
Expected training activity
```

> [!note]
> The simulator is approved lab activity. It is not malware.

## Screenshot Checkpoint

No screenshot is required unless instructed.

## Record in Evidence Notes

### Student Input - Copy or Type

```text
Detection result classification:
Classification reason:
```

---

# Task 29 - Create the Lab 07 Detection Notes File

## Where to Work

Use **WIN11-CLIENT**.

Use **Notepad**.

## Steps

1. Open **Notepad**.
2. Copy or type the detection notes template below.
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
Lab07-Detection-Rule-Notes.txt
```

## Detection Notes Template

### Student Input - Copy or Type

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 07 - Creating Simple Elastic Detection Logic

Student Name:
Date:

1. Environment Verification

Windows hostname:
Windows evidence folder confirmed:
Elastic Agent service status:
Sysmon service status:
Simulator file exists:
Simulator file path:

2. Kibana Setup

Kibana URL used:
Kibana opened successfully:
Discover opened:
Data view selected:
Time range used:

3. Query Review

Primary detection query:
Primary query matched existing events:
whoami detection query:
whoami query matched existing events:
hostname detection query:
hostname query matched existing events:

4. Detection Workflow

Detection rules available:
Fallback workflow required:
Detection rule creation page opened:
Saved query fallback used:
Saved query name:
Manual detection query documented:

5. Detection Rule Details

Detection rule name:
Detection rule query:
Detection rule severity:
Detection rule risk score:
Detection rule interval:
Detection rule look-back:
Detection rule tags added:
Investigation guidance added:
Detection rule saved:
Detection rule enabled:
Manual rule run available:
Manual rule run completed:
Alerts generated during manual run:

6. Fallback Saved Queries

whoami saved query created:
whoami saved query name:
whoami manual query documented:
hostname saved query created:
hostname saved query name:
hostname manual query documented:

7. Safe Test Activity

Safe test activity generated:
Test activity start time:
Test activity commands:
Test activity end time:
Post-test time range used:
Events refreshed after test activity:

8. Detection Validation

Simulator detection matched after test:
Matching simulator event timestamp:
Matching simulator event user:
Matching simulator parent process:
Why the simulator query matched:

whoami detection matched after test:
Matching whoami event timestamp:
Matching whoami event user:
Matching whoami parent process:
Why the whoami query matched:

hostname detection matched after test:
Matching hostname event timestamp:
Matching hostname event user:
Matching hostname parent process:
Why the hostname query matched:

9. Alert Review

Alerts page reviewed:
Alert generated:
Alert rule name:
Alert timestamp:
Alert host:
Alert user:

10. Detection Assessment

Detection match explanation:
Detection result classification:
Classification reason:

11. Lab Summary

Write 3 to 5 sentences explaining what detection logic you created, how you tested it, and what matched.
```

## Expected Result

The detection notes file should be saved at:

```text
C:\BlueWave\Evidence\Lab07-Detection-Rule-Notes.txt
```

## Screenshot Checkpoint

Capture a screenshot showing the completed Lab 07 evidence notes file.

---

# Task 30 - Confirm the Lab 07 Evidence File Exists

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Confirm the Lab 07 notes file exists.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\Evidence\Lab07-Detection-Rule-Notes.txt"
```

2. Press **Enter**.

## Expected Result

PowerShell should return:

```text
True
```

## Screenshot Checkpoint

Capture a screenshot showing the validation command returning `True`.

---

# Task 31 - Final Validation

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana**, **PowerShell**, and **File Explorer**.

## Steps

1. Confirm the primary query was reviewed.
2. Confirm supporting queries were reviewed.
3. Confirm a detection rule was created or a saved query/manual fallback was documented.
4. Confirm safe test activity was generated.
5. Confirm at least one matching event was found or query attempts were documented.
6. Confirm the detection match explanation was written.
7. Confirm the activity classification was written.
8. Confirm the Lab 07 notes file exists.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\Evidence\Lab07-Detection-Rule-Notes.txt"
```

## Expected Result

You should have:

```text
Detection logic reviewed
Detection rule or fallback saved query documented
Safe test activity generated
Matching events reviewed
Detection match explained
Activity classified
Lab07-Detection-Rule-Notes.txt saved
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
- [ ] I confirmed the simulator file exists.
- [ ] I opened Kibana.
- [ ] I opened Discover.
- [ ] I selected a logs data view.
- [ ] I set the time range to Last 24 hours.
- [ ] I reviewed the primary simulator detection query.
- [ ] I reviewed the `whoami` supporting query.
- [ ] I reviewed the `hostname` supporting query.
- [ ] I checked whether detection rules are available.
- [ ] I created a detection rule if rules are available.
- [ ] I used the saved query or manual fallback if rules are unavailable.
- [ ] I generated safe test activity.
- [ ] I refreshed events after test activity.
- [ ] I verified the simulator detection match.
- [ ] I verified the `whoami` detection match.
- [ ] I verified the `hostname` detection match.
- [ ] I reviewed alerts if available.
- [ ] I explained why the detection matched.
- [ ] I classified the result.
- [ ] I created `Lab07-Detection-Rule-Notes.txt`.
- [ ] I captured the required screenshots.

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

## Problem: Discover is not visible

Use the Kibana navigation search.

### Student Input - Copy or Type

```text
Discover
```

If Discover is still unavailable, ask your instructor.

---

## Problem: Detection rules are not available

Use the saved query or manual query fallback.

Primary fallback query:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

Record:

```text
Detection rules unavailable. Saved query or manual query fallback used.
```

---

## Problem: The rule creation page looks different

Elastic screens may vary by version.

Look for similar options such as:

### Student Input - Copy or Type

```text
Rules
Detection rules
Create rule
Custom query
```

If the options are not available, use the fallback workflow.

---

## Problem: No alerts appear after creating the rule

Possible reasons:

- Rule has not run yet.
- Rule schedule has not elapsed.
- Rule is disabled.
- Query did not match recent events.
- Time range does not include the activity.
- Alerts feature is unavailable.

Use Discover to validate the query manually:

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

---

## Problem: Simulator events do not appear

Try alternate simulator queries.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and process.executable : *BlueWaveActivitySimulator*
```

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *BlueWaveActivitySimulator*
```

Also check the time range:

### Student Input - Copy or Type

```text
Last 1 hour
```

---

## Problem: whoami or hostname events do not appear

Try broader message queries.

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *whoami*
```

### Student Input - Copy or Type

```text
host.name : "WIN11-CLIENT" and message : *hostname*
```

If the events still do not appear, record that no matching event was found.

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

## Problem: The simulator file is missing

Check the simulator file.

### Student Input - Copy or Type

```powershell
Test-Path "C:\LabFiles\Simulators\BlueWaveActivitySimulator.exe"
```

If the result is `False`, notify your instructor.

Do not download or create a replacement simulator.

---

## Problem: The Lab 07 evidence file is missing

Check that you saved it as:

### Student Input - Copy or Type

```text
Lab07-Detection-Rule-Notes.txt
```

Check that you saved it in:

### Student Input - Copy or Type

```text
C:\BlueWave\Evidence
```

Confirm with:

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\Evidence\Lab07-Detection-Rule-Notes.txt"
```

Expected result:

```text
True
```

---

# Knowledge Check

Answer the following questions.

1. What is the difference between an event and an alert?
2. What is detection logic?
3. What is the purpose of a detection rule?
4. What is a saved query?
5. What primary query did you use to detect simulator activity?
6. Why is the simulator detection considered safe for this lab?
7. Why might a detection rule not immediately generate an alert?
8. What fallback workflow should you use if detection rules are unavailable?
9. Why is it important to explain why a detection matched?
10. How would you classify approved simulator activity in this lab?

---

# Summary

In this lab, you completed the following tasks:

- Reviewed simple detection concepts.
- Opened Kibana Discover.
- Tested detection queries for simulator, `whoami`, and `hostname` activity.
- Checked whether detection rules were available.
- Created a custom detection rule if available.
- Used saved query or manual query fallback if rules were unavailable.
- Generated safe test activity.
- Validated matching events in Discover.
- Reviewed alerts if available.
- Explained why the detection matched.
- Classified the detection result.
- Created Lab 07 detection notes.

You are now ready for Lab 08, where you will use Elastic to triage and respond to a simulated incident.

---

# Deliverables

Submit or retain the following items as directed by your instructor.

| Deliverable | Location |
|---|---|
| Lab 07 detection notes | `C:\BlueWave\Evidence\Lab07-Detection-Rule-Notes.txt` |
| Screenshot of primary detection query in Discover | Skillable submission area |
| Screenshot of `whoami` or `hostname` query | Skillable submission area |
| Screenshot of detection rule creation, if available | Skillable submission area |
| Screenshot of saved query fallback, if used | Skillable submission area |
| Screenshot of safe test activity generation | Skillable submission area |
| Screenshot of matching simulator event | Skillable submission area |
| Screenshot of generated alert, if available | Skillable submission area |
| Screenshot of completed Lab 07 notes file | Skillable submission area |

---

# Instructor Notes

## Expected Knowledge Check Answers

1. An event is a recorded action or observation. An alert is created when detection logic matches activity of interest.
2. Detection logic is a query or rule used to find activity that should be reviewed.
3. A detection rule automatically searches for matching events and may create alerts.
4. A saved query is a reusable query that can be run manually in Kibana.
5. The primary query is:

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

6. The simulator is approved lab activity and is not malware.
7. The rule may not have run yet, may be disabled, may not match the time range, or alerts may not be available.
8. Use a saved query or document a manual detection query in Discover.
9. Explaining the match helps analysts understand why the event or alert matters.
10. Approved simulator activity should be classified as expected training activity or suspicious-looking but authorised.

---

## Expected Evidence File

Students should create:

```text
C:\BlueWave\Evidence\Lab07-Detection-Rule-Notes.txt
```

---

## Expected Detection Rule Values

Rule name:

```text
BlueWave - Simulator Process Activity
```

Rule type:

```text
Custom query
```

Index pattern:

```text
logs-*
```

Primary query:

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

Severity:

```text
Low
```

Risk score:

```text
21
```

Interval:

```text
5 minutes
```

Look-back:

```text
10 minutes
```

Tags:

```text
BlueWave
Training
```

---

## Expected Elastic Queries

Primary simulator query:

```text
host.name : "WIN11-CLIENT" and process.name : "BlueWaveActivitySimulator.exe"
```

Alternate simulator queries:

```text
host.name : "WIN11-CLIENT" and process.executable : *BlueWaveActivitySimulator*
```

```text
host.name : "WIN11-CLIENT" and message : *BlueWaveActivitySimulator*
```

whoami query:

```text
host.name : "WIN11-CLIENT" and process.command_line : *whoami*
```

hostname query:

```text
host.name : "WIN11-CLIENT" and process.command_line : *hostname*
```

Optional combined query:

```text
host.name : "WIN11-CLIENT" and (process.name : "BlueWaveActivitySimulator.exe" or process.command_line : *whoami* or process.command_line : *hostname*)
```

---

## Expected Visible Results

Students should be able to show:

- Discover query for simulator activity.
- Discover query for `whoami` or `hostname`.
- Detection rule created or fallback saved query documented.
- Safe test activity generated.
- Matching event in Discover.
- Alert result if alerting is available.
- Evidence notes completed.

---

## Common Student Mistakes

| Mistake | Instructor Guidance |
|---|---|
| Student cannot find Detection Rules | Use the saved query/manual query fallback |
| Student uses Elastic Cloud | Remind them this course uses self-managed Elastic only |
| Student expects alerts immediately | Explain rule intervals and ingestion delay |
| Student forgets to generate test activity | Have them run safe activity in Task 21 |
| Student uses the wrong hostname | Have them confirm with `hostname` in PowerShell |
| Student copies a malformed query | Have them use the exact Copy option |
| Student classifies simulator activity as malware | Remind them it is approved safe lab activity |
| Student does not explain why the query matched | Have them describe process name, host, user, and command line |

---

## Grading Guidance

Suggested grading allocation:

| Criteria | Points |
|---|---:|
| Environment and services verified | 10 |
| Primary and supporting queries tested | 20 |
| Detection rule created or fallback documented | 25 |
| Safe activity generated | 10 |
| Matching events reviewed | 15 |
| Alerts reviewed if available | 5 |
| Detection explanation and classification completed | 10 |
| Evidence notes and screenshots completed | 5 |
| Total | 100 |

Do not penalise students for using the fallback workflow if detection rules are unavailable in the lab image.

---

## Free Elastic Basic License Reminder

This lab must use:

- Self-managed Elastic.
- Free Elastic Basic license.
- Kibana Discover.
- Detection rules if available in the lab image.
- Saved query or manual query fallback if detection rules are unavailable.
- Safe simulated endpoint activity only.
- No Elastic Cloud.
- No paid subscriptions.
- No external internet access.

---

## Fallback Option if Detection Rules Are Not Available

If detection rules are not available, students should:

1. Use Discover.
2. Save the simulator query if saved queries are available.
3. Save or document the `whoami` and `hostname` queries.
4. Generate safe activity.
5. Rerun the queries.
6. Document matching events manually.

Suggested fallback evidence line:

```text
Detection rules were unavailable. A Discover saved query/manual detection workflow was used instead.
```

---

End of Lab 07.
