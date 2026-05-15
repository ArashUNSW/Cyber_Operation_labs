# Lab 03 - Enrolling Windows 11 into Elastic

## Estimated Time

90–120 minutes

---

## Lab Purpose

In this lab, you will connect the Windows 11 endpoint to Elastic using Elastic Agent and Fleet.

You will verify the Elastic Agent policy in Kibana, install or confirm Elastic Agent on WIN11-CLIENT, enrol the endpoint into Fleet, and confirm that Windows logs begin arriving in Elastic.

This lab is an important step in the BlueWave Clinic SOC build. After this lab, the Windows endpoint should be visible to Elastic.

---

## How to Use Copy and Type Options

This lab uses **Copy** and **Type** options for every command, URL, path, filename, query, and template that students may need to enter.

### Copy Option

Use the **Copy** option when you want to copy and paste the text directly into the lab environment.

### Type Option

Use the **Type** option when you need to manually type the text.

> [!note]
> The Copy and Type options contain the same command or text. Use one option unless your instructor tells you otherwise.

> [!alert]
> Type commands exactly as shown. Commands are sensitive to spaces, punctuation, slashes, quotation marks, and backslashes.

---

## Learning Objectives

By the end of this lab, you will be able to:

- Open Kibana Fleet.
- Review an Elastic Agent policy.
- Identify the Windows integration in Fleet.
- Identify whether Sysmon collection is included or documented.
- Locate the offline Elastic Agent installer on Windows.
- Install or verify Elastic Agent on WIN11-CLIENT.
- Enrol WIN11-CLIENT into Fleet.
- Confirm that the Elastic Agent status is Healthy.
- Search Kibana Discover for initial Windows events.
- Create an agent enrolment evidence file.

---

## Scenario

BlueWave Clinic has prepared a small two-machine cyber operations lab.

The organisation wants logs from the Windows 11 workstation to be collected in Elastic so that junior analysts can investigate endpoint activity in Kibana.

You are now ready to connect the Windows endpoint to Elastic.

In this lab:

- You will use **Kibana Fleet** to review the endpoint policy.
- You will use **Elastic Agent** on WIN11-CLIENT.
- You will enrol WIN11-CLIENT into the Fleet-managed Elastic environment.
- You will verify that Windows logs start to appear in Kibana.

> [!note]
> This lab uses self-managed Elastic on UBUNTU-SOC.

> [!alert]
> Do not use Elastic Cloud. Do not create an Elastic Cloud account. Do not download Elastic Agent from the internet.

> [!note]
> The Elastic Agent installer is assumed to be preloaded in the Skillable environment.

---

## Required Machines

| Machine | Used For |
|---|---|
| WIN11-CLIENT | Browser access to Kibana, Elastic Agent installation, endpoint enrolment |
| UBUNTU-SOC | Elasticsearch, Kibana, Fleet, and Elastic services |

---

## Required Files

| File | Location | Purpose |
|---|---|---|
| elastic-agent-windows.zip | `C:\LabFiles\Tools` | Offline Elastic Agent installer |
| Lab03-Agent-Enrolment-Notes.txt | `C:\BlueWave\Evidence` | Student evidence file created during this lab |

---

## Important Paths

### Windows Paths

| Path | Purpose |
|---|---|
| `C:\LabFiles\Tools` | Location of the Elastic Agent installer |
| `C:\BlueWave\Evidence` | Windows evidence folder |
| `C:\BlueWave\Evidence\Lab03-Agent-Enrolment-Notes.txt` | Lab 03 evidence notes |
| `C:\Program Files\Elastic\Agent` | Common Elastic Agent installation path |

### Ubuntu Paths

| Path | Purpose |
|---|---|
| `/home/student/bluewave/evidence` | Ubuntu evidence folder |
| `/home/student/labfiles` | Preloaded Ubuntu lab files |

---

## Information You Need Before Starting

You need the following from Lab 01 and Lab 02:

| Item | Example |
|---|---|
| Ubuntu SOC IP address | `10.1.1.10` |
| Kibana URL | `http://10.1.1.10:5601` |
| Kibana username | Provided by instructor |
| Kibana password | Provided by instructor |
| Windows hostname | `WIN11-CLIENT` |

> [!note]
> Your IP address may be different. Use the actual UBUNTU-SOC IP address from your lab environment.

---

## Screenshots You Should Capture

Capture screenshots as instructed by your trainer or Skillable platform.

Recommended screenshots:

1. Kibana Fleet page.
2. Elastic Agent policy showing Windows integration.
3. Elastic Agent installation folder or installation command output.
4. Fleet page showing WIN11-CLIENT enrolled.
5. Fleet page showing Elastic Agent status as Healthy.
6. Kibana Discover showing events from WIN11-CLIENT.
7. Completed Lab 03 evidence notes file.

---

## Key Terms

| Term | Meaning |
|---|---|
| Elastic Agent | A tool installed on an endpoint to collect and send data to Elastic |
| Fleet | Kibana area used to manage Elastic Agents and policies |
| Agent policy | A configuration assigned to Elastic Agents |
| Integration | A prebuilt Elastic configuration for collecting a specific type of data |
| Windows integration | Elastic integration used to collect Windows Event Logs |
| Enrolment | The process of connecting Elastic Agent to Fleet |
| Healthy | Fleet status showing the agent is connected and working |
| Data view | A Kibana object used to search indexed event data |
| Discover | Kibana page used to search and inspect events |
| Endpoint telemetry | Activity data collected from an endpoint |

---

# Task 1 - Confirm WIN11-CLIENT Is Ready

## Where to Work

Use **WIN11-CLIENT**.

## Steps

1. Open the **WIN11-CLIENT** virtual machine.
2. Sign in using the lab credentials.
3. Open **Windows PowerShell**.
4. Confirm the hostname.

### Copy

```powershell
hostname
```

### Type

Type this into PowerShell:

```powershell
hostname
```

5. Press **Enter**.
6. Confirm the result is the Windows endpoint name.

## Expected Result

The hostname should usually be:

```text
WIN11-CLIENT
```

If your hostname is different, record the exact hostname shown.

## Screenshot Checkpoint

Capture a screenshot showing the hostname result if required.

## Record in Evidence Notes

### Copy

```text
Windows hostname:
```

### Type

Type this into your evidence notes, then add the value you saw:

```text
Windows hostname:
```

---

# Task 2 - Confirm UBUNTU-SOC Is Reachable

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Identify the UBUNTU-SOC IP address from Lab 01.
2. Test basic connectivity to UBUNTU-SOC.
3. Replace `<UBUNTU-SOC-IP>` with the actual Ubuntu IP address.

### Copy

```powershell
Test-NetConnection <UBUNTU-SOC-IP>
```

### Type

Type this into PowerShell, replacing `<UBUNTU-SOC-IP>` with the actual Ubuntu IP address:

```powershell
Test-NetConnection <UBUNTU-SOC-IP>
```

Example:

```powershell
Test-NetConnection 10.1.1.10
```

4. Press **Enter**.
5. Review the result.

## Expected Result

If connectivity is working, you may see:

```text
PingSucceeded : True
```

If ping is blocked, the command may show a failed ping. Continue if Kibana opens in the browser later.

> [!note]
> Some lab environments block ping. The important check in this lab is whether WIN11-CLIENT can open Kibana and whether Elastic Agent can enrol.

## Screenshot Checkpoint

Capture a screenshot of the connectivity result if required.

## Record in Evidence Notes

### Copy

```text
Windows-to-Ubuntu connectivity result:
```

### Type

Type this into your evidence notes, then add the result:

```text
Windows-to-Ubuntu connectivity result:
```

---

# Task 3 - Open Kibana from Windows

## Where to Work

Use **WIN11-CLIENT**.

Use a web browser.

## Steps

1. Open the browser on WIN11-CLIENT.
2. Enter the Kibana URL.
3. Replace `<UBUNTU-SOC-IP>` with your Ubuntu SOC IP address.

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
5. Wait for Kibana to load.
6. Sign in using the lab credentials provided by your instructor.

## Expected Result

Kibana should open in the browser.

You should be signed in to Kibana.

## Screenshot Checkpoint

Capture a screenshot showing the Kibana home page.

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

# Task 4 - Open Fleet in Kibana

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana** in the browser.

## Steps

1. In Kibana, open the main navigation menu.
2. Search for **Fleet**.

### Copy

```text
Fleet
```

### Type

Type this into the Kibana search or navigation search box:

```text
Fleet
```

3. Select **Fleet**.
4. Wait for the Fleet page to open.

## Expected Result

The Fleet page should open.

You may see tabs such as:

```text
Agents
Agent policies
Enrollment tokens
Settings
```

## Screenshot Checkpoint

Capture a screenshot showing the Fleet page.

## Record in Evidence Notes

### Copy

```text
Fleet opened successfully:
```

### Type

Type this into your evidence notes:

```text
Fleet opened successfully:
```

---

# Task 5 - Review the Agent Policies Page

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Fleet**.

## Steps

1. In Fleet, select **Agent policies**.
2. Look for a policy prepared for Windows endpoints.

The policy may be named something similar to:

### Copy

```text
Windows Endpoint Policy
```

### Type

Look for this or a similar policy name:

```text
Windows Endpoint Policy
```

Alternative possible names:

### Copy

```text
BlueWave Windows Policy
Default policy
Windows policy
```

### Type

Look for one of these possible policy names:

```text
BlueWave Windows Policy
Default policy
Windows policy
```

3. Open the Windows-related policy.
4. Review the integrations included in the policy.

## Expected Result

You should see an agent policy that can be used by WIN11-CLIENT.

The policy should include a Windows-related integration or log collection configuration.

## Screenshot Checkpoint

Capture a screenshot showing the agent policy list.

## Record in Evidence Notes

### Copy

```text
Agent policy name:
```

### Type

Type this into your evidence notes, then add the policy name:

```text
Agent policy name:
```

---

# Task 6 - Confirm the Windows Integration Is Included

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Fleet**.

## Steps

1. Open the Windows-related agent policy.
2. Review the integrations listed in the policy.
3. Look for a Windows integration.

### Copy

```text
Windows
```

### Type

Look for this integration name:

```text
Windows
```

4. Select the Windows integration if needed.
5. Review the logs or data streams configured.

Look for log sources such as:

### Copy

```text
Security
System
Application
Windows PowerShell
Microsoft-Windows-PowerShell/Operational
```

### Type

Look for these log source names:

```text
Security
System
Application
Windows PowerShell
Microsoft-Windows-PowerShell/Operational
```

## Expected Result

The policy should include Windows log collection.

Expected Windows log sources may include:

```text
Windows Security
Windows System
Windows Application
Windows PowerShell
Microsoft-Windows-PowerShell/Operational
```

> [!note]
> The exact naming may vary depending on the lab image and Elastic version.

## Screenshot Checkpoint

Capture a screenshot showing the Windows integration in the policy.

## Record in Evidence Notes

### Copy

```text
Windows integration found:
Windows log sources observed:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Windows integration found:
Windows log sources observed:
```

---

# Task 7 - Check Whether Sysmon Collection Is Included or Documented

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Fleet**.

## Steps

1. Stay in the Windows-related agent policy.
2. Look for Sysmon-related collection.
3. Search visually for terms such as:

### Copy

```text
Sysmon
```

### Type

Look for this term:

```text
Sysmon
```

4. Also look for the Sysmon log channel name.

### Copy

```text
Microsoft-Windows-Sysmon/Operational
```

### Type

Look for this log channel:

```text
Microsoft-Windows-Sysmon/Operational
```

5. Record whether Sysmon collection is already included.

## Expected Result

One of the following should be true:

```text
Sysmon collection is included in the policy.
```

or:

```text
Sysmon collection is not visible yet and will be confirmed in a later lab.
```

> [!note]
> If Sysmon collection is not visible in Fleet, continue with the lab. Sysmon is investigated in detail in Lab 05.

## Screenshot Checkpoint

Capture a screenshot showing the policy area where Sysmon is included or where the Windows integration is shown.

## Record in Evidence Notes

### Copy

```text
Sysmon collection visible in policy:
Sysmon log channel observed:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Sysmon collection visible in policy:
Sysmon log channel observed:
```

---

# Task 8 - Locate the Elastic Agent Installer on Windows

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Open **Windows PowerShell**.
2. Check that the Tools folder exists.

### Copy

```powershell
Test-Path "C:\LabFiles\Tools"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\LabFiles\Tools"
```

3. Press **Enter**.
4. List the files in the Tools folder.

### Copy

```powershell
Get-ChildItem "C:\LabFiles\Tools"
```

### Type

Type this into PowerShell:

```powershell
Get-ChildItem "C:\LabFiles\Tools"
```

5. Press **Enter**.
6. Look for the Elastic Agent zip file.

### Copy

```text
elastic-agent-windows.zip
```

### Type

Look for this filename in the output:

```text
elastic-agent-windows.zip
```

## Expected Result

The Tools folder should exist.

The Elastic Agent installer should be available at:

```text
C:\LabFiles\Tools\elastic-agent-windows.zip
```

> [!alert]
> Do not download Elastic Agent from the internet. Use the preloaded file.

## Screenshot Checkpoint

Capture a screenshot showing the Elastic Agent zip file in `C:\LabFiles\Tools`.

## Record in Evidence Notes

### Copy

```text
Elastic Agent installer found:
Elastic Agent installer path:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Elastic Agent installer found:
Elastic Agent installer path:
```

Example:

```text
Elastic Agent installer found: Yes
Elastic Agent installer path: C:\LabFiles\Tools\elastic-agent-windows.zip
```

---

# Task 9 - Check Whether Elastic Agent Is Already Installed

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. In PowerShell, check the common Elastic Agent installation folder.

### Copy

```powershell
Test-Path "C:\Program Files\Elastic\Agent"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\Program Files\Elastic\Agent"
```

2. Press **Enter**.
3. Check whether the Elastic Agent service exists.

### Copy

```powershell
Get-Service elastic-agent -ErrorAction SilentlyContinue
```

### Type

Type this into PowerShell:

```powershell
Get-Service elastic-agent -ErrorAction SilentlyContinue
```

4. Press **Enter**.
5. Record whether Elastic Agent is already installed.

## Expected Result

If Elastic Agent is installed, you may see service output similar to:

```text
Status   Name            DisplayName
------   ----            -----------
Running  elastic-agent   Elastic Agent
```

If nothing appears, Elastic Agent may not be installed yet.

> [!note]
> Some Skillable environments may already have Elastic Agent installed. If it is already installed and enrolled, your instructor may ask you to verify instead of reinstalling.

## Screenshot Checkpoint

Capture a screenshot showing whether the Elastic Agent service exists.

## Record in Evidence Notes

### Copy

```text
Elastic Agent already installed:
Elastic Agent service status:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Elastic Agent already installed:
Elastic Agent service status:
```

---

# Task 10 - Extract the Elastic Agent Installer if Needed

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Complete this task only if Elastic Agent is not already installed or your instructor tells you to reinstall it.
2. Create a working folder for the extracted installer.

### Copy

```powershell
New-Item -Path "C:\LabFiles\Tools\elastic-agent" -ItemType Directory -Force
```

### Type

Type this into PowerShell:

```powershell
New-Item -Path "C:\LabFiles\Tools\elastic-agent" -ItemType Directory -Force
```

3. Press **Enter**.
4. Extract the Elastic Agent zip file.

### Copy

```powershell
Expand-Archive -Path "C:\LabFiles\Tools\elastic-agent-windows.zip" -DestinationPath "C:\LabFiles\Tools\elastic-agent" -Force
```

### Type

Type this into PowerShell:

```powershell
Expand-Archive -Path "C:\LabFiles\Tools\elastic-agent-windows.zip" -DestinationPath "C:\LabFiles\Tools\elastic-agent" -Force
```

5. Press **Enter**.
6. List the extracted files.

### Copy

```powershell
Get-ChildItem "C:\LabFiles\Tools\elastic-agent" -Recurse | Select-Object FullName
```

### Type

Type this into PowerShell:

```powershell
Get-ChildItem "C:\LabFiles\Tools\elastic-agent" -Recurse | Select-Object FullName
```

7. Press **Enter**.
8. Look for:

### Copy

```text
elastic-agent.exe
```

### Type

Look for this file:

```text
elastic-agent.exe
```

## Expected Result

The extracted folder should contain `elastic-agent.exe`.

> [!note]
> The exact extracted folder name may include the Elastic Agent version number.

## Screenshot Checkpoint

Capture a screenshot showing `elastic-agent.exe` in the extracted folder.

## Record in Evidence Notes

### Copy

```text
Elastic Agent extracted:
Extracted folder location:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Elastic Agent extracted:
Extracted folder location:
```

---

# Task 11 - Get the Fleet Enrolment Command

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Fleet** in the browser.

## Steps

1. In Kibana, go to **Fleet**.
2. Select **Agents**.
3. Select **Add agent** or **Add Elastic Agent**.
4. Select the Windows-related agent policy.
5. Choose the Windows installation option.
6. Locate the enrolment command.
7. Copy the enrolment command shown by Kibana.

The command may look similar to this:

### Example Only - Do Not Copy Unless It Matches Your Lab

```powershell
.\elastic-agent.exe install --url=https://<FLEET-SERVER>:8220 --enrollment-token=<TOKEN>
```

> [!alert]
> Your actual enrolment token will be different. Use the command shown in your Kibana Fleet page.

## Expected Result

You should have an enrolment command from Kibana Fleet.

The command should include:

```text
elastic-agent.exe install
```

and:

```text
--url
```

and:

```text
--enrollment-token
```

## Screenshot Checkpoint

Capture a screenshot showing the Add Agent page. Do not expose sensitive tokens outside your lab submission requirements.

## Record in Evidence Notes

### Copy

```text
Fleet enrolment command obtained:
Agent policy selected:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Fleet enrolment command obtained:
Agent policy selected:
```

> [!alert]
> Do not paste the full enrolment token into your evidence notes unless your instructor specifically asks for it.

---

# Task 12 - Open PowerShell as Administrator

## Where to Work

Use **WIN11-CLIENT**.

## Steps

1. Select the **Windows Start** menu.
2. Search for PowerShell.

### Copy

```text
PowerShell
```

### Type

Type this into the Windows search box:

```text
PowerShell
```

3. Right-click **Windows PowerShell**.
4. Select **Run as administrator**.
5. If a User Account Control prompt appears, select **Yes**.

## Expected Result

An elevated PowerShell window should open.

The window title may include:

```text
Administrator: Windows PowerShell
```

> [!note]
> Elastic Agent installation usually requires administrator permissions.

## Screenshot Checkpoint

Capture a screenshot showing Administrator PowerShell if required.

## Record in Evidence Notes

### Copy

```text
Administrator PowerShell opened:
```

### Type

Type this into your evidence notes:

```text
Administrator PowerShell opened:
```

---

# Task 13 - Change to the Elastic Agent Folder

## Where to Work

Use **WIN11-CLIENT**.

Use **Administrator PowerShell**.

## Steps

1. Find the folder that contains `elastic-agent.exe`.
2. Change directory to the extracted Elastic Agent folder.

If your extracted folder contains `elastic-agent.exe` directly, use:

### Copy

```powershell
Set-Location "C:\LabFiles\Tools\elastic-agent"
```

### Type

Type this into Administrator PowerShell:

```powershell
Set-Location "C:\LabFiles\Tools\elastic-agent"
```

If the extracted files are inside a versioned folder, use the path that contains `elastic-agent.exe`.

Example:

### Copy

```powershell
Set-Location "C:\LabFiles\Tools\elastic-agent\elastic-agent-8.x.x-windows-x86_64"
```

### Type

Type the path that matches your extracted folder:

```powershell
Set-Location "C:\LabFiles\Tools\elastic-agent\elastic-agent-8.x.x-windows-x86_64"
```

3. Confirm `elastic-agent.exe` exists.

### Copy

```powershell
Test-Path ".\elastic-agent.exe"
```

### Type

Type this into Administrator PowerShell:

```powershell
Test-Path ".\elastic-agent.exe"
```

4. Press **Enter**.

## Expected Result

The validation command should return:

```text
True
```

If it returns `False`, you are not in the correct folder.

## Screenshot Checkpoint

Capture a screenshot showing `Test-Path ".\elastic-agent.exe"` returning `True`.

## Record in Evidence Notes

### Copy

```text
Elastic Agent working folder:
elastic-agent.exe found:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Elastic Agent working folder:
elastic-agent.exe found:
```

---

# Task 14 - Install and Enrol Elastic Agent

## Where to Work

Use **WIN11-CLIENT**.

Use **Administrator PowerShell**.

## Steps

1. Make sure you are in the folder that contains `elastic-agent.exe`.
2. Paste the enrolment command copied from Kibana Fleet.
3. Review the command before pressing Enter.
4. Confirm it includes the correct Fleet Server URL and enrolment token.
5. Press **Enter**.
6. If prompted to continue, type `Y`.

### Copy

```text
Y
```

### Type

Type this if PowerShell asks whether you want to continue:

```text
Y
```

7. Wait for the installation and enrolment process to complete.

## Expected Result

You may see output similar to:

```text
Elastic Agent will be installed at C:\Program Files\Elastic\Agent and will run as a service.
```

You may also see:

```text
Elastic Agent has been successfully installed.
```

or:

```text
Successfully enrolled the Elastic Agent.
```

> [!alert]
> The enrolment command contains a token. Use the command generated by your lab Fleet page. Do not invent a token.

> [!note]
> The exact output may vary depending on the Elastic Agent version.

## Screenshot Checkpoint

Capture a screenshot showing successful Elastic Agent installation or enrolment.

## Record in Evidence Notes

### Copy

```text
Elastic Agent install/enrolment completed:
Installation result:
```

### Type

Type these lines into your evidence notes, then add your result:

```text
Elastic Agent install/enrolment completed:
Installation result:
```

---

# Task 15 - Verify the Elastic Agent Service on Windows

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Run the service check command.

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

The service should exist.

The status should usually be:

```text
Running
```

Example:

```text
Status   Name            DisplayName
------   ----            -----------
Running  elastic-agent   Elastic Agent
```

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

# Task 16 - Confirm WIN11-CLIENT Appears in Fleet

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Fleet** in the browser.

## Steps

1. Return to Kibana.
2. Open **Fleet**.
3. Select **Agents**.
4. Look for the Windows endpoint hostname.

### Copy

```text
WIN11-CLIENT
```

### Type

Look for this hostname or your actual Windows hostname:

```text
WIN11-CLIENT
```

5. Review the agent status.

## Expected Result

WIN11-CLIENT should appear in the Fleet Agents list.

The status should ideally show:

```text
Healthy
```

It may take a few minutes for the status to update.

> [!note]
> If the status is Updating or Offline immediately after installation, wait briefly and refresh the page.

## Screenshot Checkpoint

Capture a screenshot showing WIN11-CLIENT in Fleet.

## Record in Evidence Notes

### Copy

```text
WIN11-CLIENT visible in Fleet:
Fleet agent status:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
WIN11-CLIENT visible in Fleet:
Fleet agent status:
```

---

# Task 17 - Review Agent Details in Fleet

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Fleet**.

## Steps

1. In the Fleet Agents list, select the WIN11-CLIENT agent.
2. Review the agent details page.
3. Locate the following information if visible:

### Copy

```text
Agent status
Host name
Agent policy
Last check-in
Agent version
```

### Type

Look for these fields on the agent details page:

```text
Agent status
Host name
Agent policy
Last check-in
Agent version
```

4. Record the information.

## Expected Result

You should be able to identify:

```text
Host name: WIN11-CLIENT
Agent status: Healthy
Agent policy: Windows-related policy
```

## Screenshot Checkpoint

Capture a screenshot showing the agent details page.

## Record in Evidence Notes

### Copy

```text
Agent hostname:
Agent policy:
Agent version:
Last check-in:
```

### Type

Type these lines into your evidence notes, then add the values:

```text
Agent hostname:
Agent policy:
Agent version:
Last check-in:
```

---

# Task 18 - Open Kibana Discover

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana**.

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

3. Open **Discover**.
4. Select the available logs data view.

Possible data view names may include:

### Copy

```text
logs-*
```

### Type

Look for a data view similar to:

```text
logs-*
```

Alternative possible data views:

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

5. Set the time range to **Last 24 hours**.

### Copy

```text
Last 24 hours
```

### Type

Select or type this time range:

```text
Last 24 hours
```

## Expected Result

Discover should open.

A logs-related data view should be selected.

The time range should be set to:

```text
Last 24 hours
```

## Screenshot Checkpoint

Capture a screenshot showing Discover with the selected data view and time range.

## Record in Evidence Notes

### Copy

```text
Discover opened:
Data view used:
Time range used:
```

### Type

Type these lines into your evidence notes, then add your values:

```text
Discover opened:
Data view used:
Time range used:
```

---

# Task 19 - Search for Events from WIN11-CLIENT

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. In Discover, click the search query bar.
2. Search for the Windows hostname.

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
4. Review the results.

If no results appear, try this alternate query:

### Copy

```text
agent.name : "WIN11-CLIENT"
```

### Type

Type this alternate query into the Kibana query bar:

```text
agent.name : "WIN11-CLIENT"
```

If still no results appear, try searching for the hostname in the message field:

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
host.name
agent.name
event.dataset
event.provider
event.code
message
```

> [!note]
> It may take a few minutes for new logs to arrive after enrolment.

## Screenshot Checkpoint

Capture a screenshot showing events from WIN11-CLIENT in Discover.

## Record in Evidence Notes

### Copy

```text
Kibana query used:
Events from WIN11-CLIENT found:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Kibana query used:
Events from WIN11-CLIENT found:
```

---

# Task 20 - Search for Windows Event Logs

## Where to Work

Use **WIN11-CLIENT**.

Use **Kibana Discover**.

## Steps

1. Keep the time range set to **Last 24 hours**.
2. Search for Windows events.

### Copy

```text
host.name : "WIN11-CLIENT" and event.module : "windows"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.module : "windows"
```

3. Press **Enter** or select **Update**.
4. Review the results.

If no results appear, try this alternate query:

### Copy

```text
host.name : "WIN11-CLIENT" and event.dataset : windows.*
```

### Type

Type this alternate query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT" and event.dataset : windows.*
```

If no results appear, try this broader query:

### Copy

```text
host.name : "WIN11-CLIENT"
```

### Type

Type this broader query into the Kibana query bar:

```text
host.name : "WIN11-CLIENT"
```

## Expected Result

You should see Windows-related log events from WIN11-CLIENT.

Common event sources may include:

```text
Security
System
Application
PowerShell
```

> [!note]
> Exact field names can vary. Use the alternate queries if needed.

## Screenshot Checkpoint

Capture a screenshot showing Windows-related events in Discover.

## Record in Evidence Notes

### Copy

```text
Windows events found in Discover:
Windows event query used:
```

### Type

Type these lines into your evidence notes, then add your findings:

```text
Windows events found in Discover:
Windows event query used:
```

---

# Task 21 - Create the Lab 03 Evidence Notes File

## Where to Work

Use **WIN11-CLIENT**.

Use **Notepad**.

## Steps

1. Open **Notepad**.
2. Copy or type the evidence template below.
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

7. Save the file as:

### Copy

```text
Lab03-Agent-Enrolment-Notes.txt
```

### Type

Type this filename exactly:

```text
Lab03-Agent-Enrolment-Notes.txt
```

## Evidence Template

### Copy

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 03 - Enrolling Windows 11 into Elastic

Student Name:
Date:

1. Environment Information

Windows hostname:
Ubuntu SOC IP address:
Kibana URL used:

2. Fleet Review

Fleet opened successfully:
Agent policy name:
Windows integration found:
Windows log sources observed:
Sysmon collection visible in policy:
Sysmon log channel observed:

3. Elastic Agent Installer

Elastic Agent installer found:
Elastic Agent installer path:
Elastic Agent already installed before this lab:
Elastic Agent extracted:
Elastic Agent working folder:
elastic-agent.exe found:

4. Agent Enrolment

Fleet enrolment command obtained:
Agent policy selected:
Administrator PowerShell opened:
Elastic Agent install/enrolment completed:
Installation result:
Elastic Agent service status:

5. Fleet Verification

WIN11-CLIENT visible in Fleet:
Fleet agent status:
Agent hostname:
Agent policy:
Agent version:
Last check-in:

6. Discover Verification

Discover opened:
Data view used:
Time range used:
Kibana query used:
Events from WIN11-CLIENT found:
Windows events found in Discover:
Windows event query used:

7. Lab Summary

Write 3 to 5 sentences explaining what you completed in this lab and why Elastic Agent enrolment matters.
```

### Type

Type the same template into Notepad manually:

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 03 - Enrolling Windows 11 into Elastic

Student Name:
Date:

1. Environment Information

Windows hostname:
Ubuntu SOC IP address:
Kibana URL used:

2. Fleet Review

Fleet opened successfully:
Agent policy name:
Windows integration found:
Windows log sources observed:
Sysmon collection visible in policy:
Sysmon log channel observed:

3. Elastic Agent Installer

Elastic Agent installer found:
Elastic Agent installer path:
Elastic Agent already installed before this lab:
Elastic Agent extracted:
Elastic Agent working folder:
elastic-agent.exe found:

4. Agent Enrolment

Fleet enrolment command obtained:
Agent policy selected:
Administrator PowerShell opened:
Elastic Agent install/enrolment completed:
Installation result:
Elastic Agent service status:

5. Fleet Verification

WIN11-CLIENT visible in Fleet:
Fleet agent status:
Agent hostname:
Agent policy:
Agent version:
Last check-in:

6. Discover Verification

Discover opened:
Data view used:
Time range used:
Kibana query used:
Events from WIN11-CLIENT found:
Windows events found in Discover:
Windows event query used:

7. Lab Summary

Write 3 to 5 sentences explaining what you completed in this lab and why Elastic Agent enrolment matters.
```

## Expected Result

The evidence notes file should be saved at:

```text
C:\BlueWave\Evidence\Lab03-Agent-Enrolment-Notes.txt
```

## Screenshot Checkpoint

Capture a screenshot showing the completed Lab 03 evidence notes file.

## Record in Evidence Notes

This task creates the evidence notes file.

---

# Task 22 - Confirm the Lab 03 Evidence File Exists

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Open **Windows PowerShell**.
2. Run the file validation command.

### Copy

```powershell
Test-Path "C:\BlueWave\Evidence\Lab03-Agent-Enrolment-Notes.txt"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab03-Agent-Enrolment-Notes.txt"
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

Use **Kibana**, **PowerShell**, and **File Explorer**.

## Steps

1. Confirm Elastic Agent service is running.

### Copy

```powershell
Get-Service elastic-agent
```

### Type

Type this into PowerShell:

```powershell
Get-Service elastic-agent
```

2. Confirm WIN11-CLIENT is visible in Fleet.
3. Confirm the Fleet status is Healthy or recently connected.
4. Confirm Discover shows events from WIN11-CLIENT.
5. Confirm the Lab 03 notes file exists.

### Copy

```powershell
Test-Path "C:\BlueWave\Evidence\Lab03-Agent-Enrolment-Notes.txt"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab03-Agent-Enrolment-Notes.txt"
```

## Expected Result

You should have:

```text
Elastic Agent service running
WIN11-CLIENT visible in Fleet
Fleet status Healthy or connected
Events from WIN11-CLIENT visible in Discover
Lab03-Agent-Enrolment-Notes.txt saved
```

## Screenshot Checkpoint

Capture any final screenshots required by your instructor.

---

# Validation Checklist

Before finishing the lab, confirm each item is complete.

- [ ] I confirmed WIN11-CLIENT is available.
- [ ] I confirmed UBUNTU-SOC is reachable or Kibana opens.
- [ ] I opened Kibana from WIN11-CLIENT.
- [ ] I opened Fleet.
- [ ] I reviewed the Agent policies page.
- [ ] I found or identified the Windows-related agent policy.
- [ ] I confirmed the Windows integration is included.
- [ ] I checked whether Sysmon collection is included or documented.
- [ ] I located `elastic-agent-windows.zip`.
- [ ] I checked whether Elastic Agent was already installed.
- [ ] I extracted Elastic Agent if required.
- [ ] I obtained the Fleet enrolment command.
- [ ] I opened PowerShell as Administrator.
- [ ] I installed or verified Elastic Agent.
- [ ] I confirmed the Elastic Agent service exists.
- [ ] I confirmed WIN11-CLIENT appears in Fleet.
- [ ] I confirmed the agent status is Healthy or connected.
- [ ] I opened Kibana Discover.
- [ ] I searched for events from WIN11-CLIENT.
- [ ] I confirmed Windows events are appearing or documented the issue.
- [ ] I created `Lab03-Agent-Enrolment-Notes.txt`.
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

If Kibana still does not open, confirm Kibana is running on UBUNTU-SOC in Lab 02 or ask your instructor.

---

## Problem: Fleet is not visible in Kibana

Use the Kibana navigation search.

### Copy

```text
Fleet
```

### Type

Type this into the Kibana navigation search:

```text
Fleet
```

If Fleet is unavailable, ask your instructor whether the lab uses a preconfigured saved search fallback.

---

## Problem: No Windows agent policy is visible

Look for similar policy names.

### Copy

```text
Windows Endpoint Policy
BlueWave Windows Policy
Default policy
Windows policy
```

### Type

Look for these names in Fleet:

```text
Windows Endpoint Policy
BlueWave Windows Policy
Default policy
Windows policy
```

If no policy is available, ask your instructor to confirm Fleet preparation.

---

## Problem: Elastic Agent installer is missing

Check the expected path.

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
elastic-agent-windows.zip
```

Do not download Elastic Agent from the internet.

---

## Problem: `elastic-agent.exe` is not found after extraction

List the extracted files recursively.

### Copy

```powershell
Get-ChildItem "C:\LabFiles\Tools\elastic-agent" -Recurse | Select-Object FullName
```

### Type

Type this into PowerShell:

```powershell
Get-ChildItem "C:\LabFiles\Tools\elastic-agent" -Recurse | Select-Object FullName
```

Find the folder that contains:

```text
elastic-agent.exe
```

Then change to that folder before running the enrolment command.

---

## Problem: PowerShell says access is denied

Close PowerShell.

Open PowerShell as Administrator.

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

---

## Problem: The enrolment command fails

Check the following:

- You are running PowerShell as Administrator.
- You are in the folder containing `elastic-agent.exe`.
- You copied the enrolment command from your own Fleet page.
- The enrolment token has not expired.
- WIN11-CLIENT can reach the Fleet Server URL.
- The Elastic services on UBUNTU-SOC are running.

Do not invent a new token.

Ask your instructor if the enrolment token needs to be regenerated.

---

## Problem: Elastic Agent service is not running

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

If the service exists but is stopped, ask your instructor before changing service state.

---

## Problem: WIN11-CLIENT does not appear in Fleet

Try these steps:

1. Wait a few minutes.
2. Refresh the Fleet Agents page.
3. Confirm the Elastic Agent service is running.
4. Confirm WIN11-CLIENT can reach Kibana and Fleet.
5. Check whether enrolment completed successfully.

---

## Problem: Agent status is Offline or Updating

Possible fix:

Wait a few minutes and refresh the Fleet page.

The agent may need time to check in.

If the status remains Offline, ask your instructor to check Fleet Server connectivity.

---

## Problem: No events appear in Discover

Try these steps:

1. Set the time range to **Last 24 hours**.
2. Try this query:

### Copy

```text
host.name : "WIN11-CLIENT"
```

### Type

Type this into the Kibana query bar:

```text
host.name : "WIN11-CLIENT"
```

3. Try this alternate query:

### Copy

```text
agent.name : "WIN11-CLIENT"
```

### Type

Type this into the Kibana query bar:

```text
agent.name : "WIN11-CLIENT"
```

4. Try this broader query:

### Copy

```text
*
```

### Type

Type this into the Kibana query bar:

```text
*
```

If events still do not appear, record the issue and ask your instructor.

---

## Problem: The field name is different

Elastic field names may vary.

Try alternate fields such as:

### Copy

```text
host.name
agent.name
winlog.computer_name
event.dataset
message
```

### Type

Look for these fields in event details:

```text
host.name
agent.name
winlog.computer_name
event.dataset
message
```

---

# Knowledge Check

Answer the following questions.

1. What is the purpose of Elastic Agent?
2. What is the purpose of Fleet?
3. What is an Agent policy?
4. Why should you use the preloaded Elastic Agent installer instead of downloading one?
5. What status should the Elastic Agent ideally show in Fleet?
6. Which Kibana page is used to search events?
7. What query can you use to search for events from WIN11-CLIENT?
8. Why is it useful to confirm the Windows integration in the policy?
9. Why should you avoid copying the full enrolment token into your report?
10. What should you do if no events appear immediately after enrolment?

---

# Summary

In this lab, you completed the following tasks:

- Opened Kibana from WIN11-CLIENT.
- Opened Fleet.
- Reviewed the Windows endpoint policy.
- Confirmed the Windows integration.
- Checked whether Sysmon collection was visible or documented.
- Located the offline Elastic Agent installer.
- Installed or verified Elastic Agent.
- Enrolled WIN11-CLIENT into Fleet.
- Confirmed the Elastic Agent service.
- Confirmed WIN11-CLIENT appears in Fleet.
- Opened Discover.
- Searched for Windows events from WIN11-CLIENT.
- Created Lab 03 evidence notes.

You are now ready for Lab 04, where you will collect and review Windows Event Logs in Kibana.

---

# Deliverables

Submit or retain the following items as directed by your instructor.

| Deliverable | Location |
|---|---|
| Lab 03 agent enrolment notes | `C:\BlueWave\Evidence\Lab03-Agent-Enrolment-Notes.txt` |
| Screenshot of Fleet page | Skillable submission area |
| Screenshot of Windows agent policy | Skillable submission area |
| Screenshot of Windows integration | Skillable submission area |
| Screenshot of Elastic Agent installer location | Skillable submission area |
| Screenshot of Elastic Agent service status | Skillable submission area |
| Screenshot showing WIN11-CLIENT in Fleet | Skillable submission area |
| Screenshot showing Healthy agent status | Skillable submission area |
| Screenshot showing events from WIN11-CLIENT in Discover | Skillable submission area |
| Screenshot of completed Lab 03 notes file | Skillable submission area |

---

# Instructor Notes

## Expected Knowledge Check Answers

1. Elastic Agent collects logs and telemetry from endpoints and sends them to Elastic.
2. Fleet manages Elastic Agents, policies, integrations, and enrolment.
3. An Agent policy is a configuration assigned to one or more Elastic Agents.
4. The lab is offline and uses preloaded files. Students should not download tools from the internet.
5. The agent should ideally show `Healthy`.
6. Kibana Discover is used to search events.
7. A useful query is:

```text
host.name : "WIN11-CLIENT"
```

8. The Windows integration confirms that Windows logs are configured for collection.
9. Enrolment tokens are sensitive lab values and should not be exposed unnecessarily.
10. Wait a few minutes, refresh, check the time range, try alternate queries, and confirm the agent is healthy.

---

## Expected Evidence File

Students should create:

```text
C:\BlueWave\Evidence\Lab03-Agent-Enrolment-Notes.txt
```

---

## Expected Command Outputs

Elastic Agent service should show:

```text
Running
```

Fleet should show the agent as:

```text
Healthy
```

or recently connected.

Discover should show events from:

```text
WIN11-CLIENT
```

---

## Expected Elastic Queries

Primary query:

```text
host.name : "WIN11-CLIENT"
```

Alternate queries:

```text
agent.name : "WIN11-CLIENT"
```

```text
message : *WIN11-CLIENT*
```

```text
host.name : "WIN11-CLIENT" and event.module : "windows"
```

```text
host.name : "WIN11-CLIENT" and event.dataset : windows.*
```

---

## Common Student Mistakes

| Mistake | Instructor Guidance |
|---|---|
| Student uses Elastic Cloud | Remind them this course uses self-managed Elastic only |
| Student downloads Elastic Agent | Remind them the installer is preloaded |
| Student runs PowerShell without Administrator rights | Have them reopen PowerShell as Administrator |
| Student copies an old or wrong enrolment token | Have them generate or copy the current command from Fleet |
| Student runs the enrolment command from the wrong folder | Have them locate the folder containing `elastic-agent.exe` |
| Student expects events instantly | Explain that logs may take a few minutes to appear |
| Student uses the wrong data view | Have them try `logs-*` or another logs-related data view |
| Student searches with the wrong hostname | Have them verify hostname with `hostname` |

---

## Grading Guidance

Suggested grading allocation:

| Criteria | Points |
|---|---:|
| Fleet opened and policy reviewed | 15 |
| Windows integration identified | 10 |
| Sysmon collection checked or documented | 10 |
| Elastic Agent installer located | 10 |
| Elastic Agent installed or verified | 15 |
| WIN11-CLIENT visible in Fleet | 15 |
| Agent status checked | 10 |
| Events searched in Discover | 10 |
| Evidence notes completed | 5 |
| Total | 100 |

Do not heavily penalise students if Sysmon collection is not visible yet, as long as they document the result correctly.

---

## Free Elastic Basic License Reminder

This lab must use:

- Self-managed Elastic.
- Free Elastic Basic license.
- No Elastic Cloud.
- No paid subscriptions.
- No external internet access.

---

## Fallback Option if Fleet Is Not Available

If Fleet is not available in the lab image, students should:

1. Verify whether Elastic Agent is already installed.
2. Confirm whether logs are arriving in Discover.
3. Use a saved search or data view to locate Windows events.
4. Document that Fleet was not available.
5. Continue with instructor-provided fallback instructions.

Suggested fallback evidence line:

```text
Fleet unavailable. Fallback method used: Discover verification of Windows logs.
```

---

End of Lab 03.
