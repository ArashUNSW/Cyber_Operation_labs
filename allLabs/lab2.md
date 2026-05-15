# Lab 02 - BlueWave Clinic Elastic and Kibana Orientation

## Estimated Time

60–120 minutes

---

## Lab Purpose

In this lab, you will verify that Elasticsearch and Kibana are running on the Ubuntu SOC server.

You will then open Kibana from the Windows 11 workstation and explore the main areas that will be used throughout the course.

This lab introduces the Elastic interface before Windows logs are collected in later labs.

---

## How to Use Copy and Type Options

This lab uses **Copy** and **Type** options for every command, search term, URL, path, filename, and template that students may need to enter.

### Copy Option

Use the **Copy** option when you want to copy and paste the text directly into the lab environment.

### Type Option

Use the **Type** option when you need to manually type the text.

> [!note]
> The Copy and Type options contain the same command or text. Use one option unless your instructor tells you otherwise.

> [!alert]
> Type commands exactly as shown. Commands are sensitive to spaces, punctuation, slashes, and quotation marks.

---

## Learning Objectives

By the end of this lab, you will be able to:

- Verify that Elasticsearch is running on UBUNTU-SOC.
- Verify that Kibana is running on UBUNTU-SOC.
- Identify the Kibana web address used by WIN11-CLIENT.
- Open Kibana from the Windows browser.
- Sign in to Kibana using lab credentials.
- Verify that the environment is using self-managed Elastic.
- Locate the Elastic license information if available.
- Navigate to Discover.
- Navigate to Fleet.
- Navigate to Dashboards.
- Locate Data Views.
- Create a Lab 02 Elastic orientation evidence file.

---

## Scenario

BlueWave Clinic has deployed a small self-managed Elastic environment on the Ubuntu SOC server.

You are still acting as a junior cyber operations analyst.

Before connecting the Windows endpoint to Elastic, you need to verify that the Elastic services are running and learn where key Kibana features are located.

In this lab, you will use:

| Machine | Role |
|---|---|
| WIN11-CLIENT | Analyst workstation used to open Kibana in a browser |
| UBUNTU-SOC | Server running Elasticsearch, Kibana, and later Fleet |

You will not collect Windows logs yet. That begins in Lab 03 and Lab 04.

> [!note]
> This course uses self-managed Elastic with the free Basic license.

> [!alert]
> Do not use Elastic Cloud in this lab. Do not create an Elastic Cloud account.

> [!note]
> This lab assumes that Elasticsearch and Kibana are already installed by the instructor or Skillable lab builder.

---

## Required Machines

| Machine | Used For |
|---|---|
| WIN11-CLIENT | Browser access to Kibana and evidence notes |
| UBUNTU-SOC | Elasticsearch and Kibana service verification |

---

## Required Files

No installer files are required in this lab.

You will create the following evidence file:

| Evidence File | Location |
|---|---|
| Lab02-Elastic-Orientation.txt | `C:\BlueWave\Evidence\Lab02-Elastic-Orientation.txt` |

Optional Ubuntu evidence copy:

| Evidence File | Location |
|---|---|
| Lab02-Elastic-Orientation.txt | `/home/student/bluewave/evidence/Lab02-Elastic-Orientation.txt` |

---

## Important Paths

### Windows Evidence Path

```text
C:\BlueWave\Evidence
```

### Ubuntu Evidence Path

```text
/home/student/bluewave/evidence
```

### Common Kibana URL Format

```text
http://<UBUNTU-SOC-IP>:5601
```

Example:

```text
http://10.1.1.10:5601
```

> [!note]
> Replace `<UBUNTU-SOC-IP>` with the actual Ubuntu IP address from your lab environment.

---

## Screenshots You Should Capture

Capture screenshots as instructed by your trainer or Skillable platform.

Recommended screenshots:

1. Elasticsearch service status on UBUNTU-SOC.
2. Kibana service status on UBUNTU-SOC.
3. Ubuntu IP address used for Kibana access.
4. Kibana login page or Kibana home page.
5. Kibana Discover page.
6. Kibana Fleet page.
7. Kibana Dashboards page.
8. Kibana Data Views page.
9. Elastic license page, if available.
10. Completed Lab 02 evidence notes file.

---

## Key Terms

| Term | Meaning |
|---|---|
| Elastic | A platform used to store, search, and analyse event data |
| Elasticsearch | The service that stores and indexes events |
| Kibana | The web interface used to search and visualise Elastic data |
| Discover | Kibana area used to search and inspect events |
| Fleet | Kibana area used to manage Elastic Agents |
| Elastic Agent | A tool that sends logs from a machine to Elastic |
| Data View | A Kibana object that tells Kibana which data to search |
| Dashboard | A Kibana page with charts, tables, and visualisations |
| Basic license | The free Elastic license used in this course |
| Self-managed Elastic | Elastic installed and managed in the lab, not Elastic Cloud |

---

# Task 1 - Sign In to Both Lab Machines

## Where to Work

Use both **WIN11-CLIENT** and **UBUNTU-SOC**.

## Steps

1. Open **WIN11-CLIENT**.
2. Sign in using the lab credentials.
3. Confirm that the Windows desktop loads.
4. Open **UBUNTU-SOC**.
5. Sign in using the lab credentials.
6. Confirm that the Ubuntu desktop or terminal environment loads.

## Expected Result

You should be signed in to both virtual machines.

## Screenshot Checkpoint

No screenshot is required unless instructed by your trainer.

## Record in Evidence Notes

### Copy

```text
WIN11-CLIENT access confirmed: Yes
UBUNTU-SOC access confirmed: Yes
```

### Type

Type these lines into your evidence notes later:

```text
WIN11-CLIENT access confirmed: Yes
UBUNTU-SOC access confirmed: Yes
```

---

# Task 2 - Confirm the Ubuntu Hostname

## Where to Work

Use **UBUNTU-SOC**.

Use **Terminal**.

## Steps

1. Open **Terminal**.
2. Run the hostname command.

### Copy

```bash
hostname
```

### Type

Type this into Terminal:

```bash
hostname
```

3. Press **Enter**.
4. Record the result.

## Expected Result

The hostname should usually be:

```text
UBUNTU-SOC
```

If the hostname is different, record the exact value shown.

## Screenshot Checkpoint

Capture a screenshot if required by your trainer.

## Record in Evidence Notes

### Copy

```text
Ubuntu hostname:
```

### Type

Type this into your evidence notes, then add the value you saw:

```text
Ubuntu hostname:
```

---

# Task 3 - Identify the Ubuntu IP Address

## Where to Work

Use **UBUNTU-SOC**.

Use **Terminal**.

## Steps

1. Run the quick IP address command.

### Copy

```bash
hostname -I
```

### Type

Type this into Terminal:

```bash
hostname -I
```

2. Press **Enter**.
3. Record the IP address shown.
4. If more detail is needed, run the network interface command.

### Copy

```bash
ip addr show
```

### Type

Type this into Terminal:

```bash
ip addr show
```

5. Press **Enter**.
6. Identify the non-loopback IPv4 address.

## Expected Result

The Ubuntu IP address may look similar to:

```text
10.1.1.10
```

or:

```text
192.168.1.10
```

> [!alert]
> Do not use `127.0.0.1` as the Kibana address from Windows. That address only points to the local machine.

## Screenshot Checkpoint

Capture a screenshot showing the Ubuntu IP address.

## Record in Evidence Notes

### Copy

```text
Ubuntu IPv4 address:
```

### Type

Type this into your evidence notes, then add the value you saw:

```text
Ubuntu IPv4 address:
```

---

# Task 4 - Check Elasticsearch Service Status

## Where to Work

Use **UBUNTU-SOC**.

Use **Terminal**.

## Steps

1. Run the Elasticsearch service status command.

### Copy

```bash
systemctl status elasticsearch
```

### Type

Type this into Terminal:

```bash
systemctl status elasticsearch
```

2. Press **Enter**.
3. Look for the service status.
4. If the status screen stays open, press **q** to exit.

## Expected Result

You should see a status line similar to:

```text
Active: active (running)
```

If the service is not running, record the message shown and see the troubleshooting section.

## Screenshot Checkpoint

Capture a screenshot showing Elasticsearch status.

## Record in Evidence Notes

### Copy

```text
Elasticsearch service status:
```

### Type

Type this into your evidence notes, then add the value you saw:

```text
Elasticsearch service status:
```

Example:

```text
Elasticsearch service status: active (running)
```

---

# Task 5 - Check Kibana Service Status

## Where to Work

Use **UBUNTU-SOC**.

Use **Terminal**.

## Steps

1. Run the Kibana service status command.

### Copy

```bash
systemctl status kibana
```

### Type

Type this into Terminal:

```bash
systemctl status kibana
```

2. Press **Enter**.
3. Look for the service status.
4. If the status screen stays open, press **q** to exit.

## Expected Result

You should see a status line similar to:

```text
Active: active (running)
```

If the service is not running, record the message shown and see the troubleshooting section.

## Screenshot Checkpoint

Capture a screenshot showing Kibana status.

## Record in Evidence Notes

### Copy

```text
Kibana service status:
```

### Type

Type this into your evidence notes, then add the value you saw:

```text
Kibana service status:
```

Example:

```text
Kibana service status: active (running)
```

---

# Task 6 - Check Whether Elasticsearch Responds Locally

## Where to Work

Use **UBUNTU-SOC**.

Use **Terminal**.

## Steps

1. Run a local Elasticsearch check.

### Copy

```bash
curl http://localhost:9200
```

### Type

Type this into Terminal:

```bash
curl http://localhost:9200
```

2. Press **Enter**.
3. Review the output.

## Expected Result

You may see JSON-style output that includes information such as:

```text
cluster_name
version
You Know, for Search
```

Some lab environments may require authentication or use HTTPS. If so, you may see an authentication or connection message.

> [!note]
> This step is a simple service response check. If the service status in Task 4 was active, continue even if curl requires authentication.

## Screenshot Checkpoint

Capture a screenshot if required by your trainer.

## Record in Evidence Notes

### Copy

```text
Elasticsearch local response check:
```

### Type

Type this into your evidence notes, then add the value you saw:

```text
Elasticsearch local response check:
```

---

# Task 7 - Check Whether Kibana Responds Locally

## Where to Work

Use **UBUNTU-SOC**.

Use **Terminal**.

## Steps

1. Run a local Kibana check.

### Copy

```bash
curl -I http://localhost:5601
```

### Type

Type this into Terminal:

```bash
curl -I http://localhost:5601
```

2. Press **Enter**.
3. Review the output.

## Expected Result

You may see an HTTP response similar to:

```text
HTTP/1.1 302 Found
```

or:

```text
HTTP/1.1 200 OK
```

A redirect or OK response usually means Kibana is responding.

## Screenshot Checkpoint

Capture a screenshot if required by your trainer.

## Record in Evidence Notes

### Copy

```text
Kibana local response check:
```

### Type

Type this into your evidence notes, then add the value you saw:

```text
Kibana local response check:
```

---

# Task 8 - Build the Kibana URL

## Where to Work

Use **WIN11-CLIENT**.

Use the Ubuntu IP address from Task 3.

## Steps

1. Find the Ubuntu IP address you recorded.
2. Build the Kibana URL using this format.

### Copy

```text
http://<UBUNTU-SOC-IP>:5601
```

### Type

Type this URL format, replacing `<UBUNTU-SOC-IP>` with your Ubuntu IP address:

```text
http://<UBUNTU-SOC-IP>:5601
```

Example:

```text
http://10.1.1.10:5601
```

3. Record your actual Kibana URL.

## Expected Result

You should have a Kibana URL that points from WIN11-CLIENT to UBUNTU-SOC.

## Screenshot Checkpoint

No screenshot is required yet.

## Record in Evidence Notes

### Copy

```text
Kibana URL:
```

### Type

Type this into your evidence notes, then add your actual URL:

```text
Kibana URL:
```

Example:

```text
Kibana URL: http://10.1.1.10:5601
```

---

# Task 9 - Open Kibana from WIN11-CLIENT

## Where to Work

Use **WIN11-CLIENT**.

Use a web browser.

## Steps

1. Open the browser on WIN11-CLIENT.
2. Select the address bar.
3. Enter the Kibana URL you built in Task 8.

### Copy

```text
http://<UBUNTU-SOC-IP>:5601
```

### Type

Type your Kibana URL into the browser address bar:

```text
http://<UBUNTU-SOC-IP>:5601
```

Example:

```text
http://10.1.1.10:5601
```

4. Press **Enter**.
5. Wait for Kibana to load.

## Expected Result

You should see one of the following:

```text
Kibana login page
```

or:

```text
Kibana home page
```

If Kibana does not open, check the troubleshooting section.

## Screenshot Checkpoint

Capture a screenshot showing the Kibana login page or home page.

## Record in Evidence Notes

### Copy

```text
Kibana opened from Windows browser: Yes
```

### Type

Type this into your evidence notes:

```text
Kibana opened from Windows browser: Yes
```

---

# Task 10 - Sign In to Kibana

## Where to Work

Use **WIN11-CLIENT**.

Use the browser with Kibana open.

## Steps

1. On the Kibana login page, select the **Username** field.
2. Enter the lab username provided by your instructor.
3. Select the **Password** field.
4. Enter the lab password provided by your instructor.
5. Select **Log in**.

## Expected Result

Kibana should open after successful sign-in.

You may see a home page, welcome page, or setup page depending on the lab image.

> [!note]
> The exact username and password may vary by Skillable environment. Use the lab credentials provided by your instructor or lab instructions.

## Screenshot Checkpoint

Capture a screenshot after signing in to Kibana.

## Record in Evidence Notes

### Copy

```text
Kibana sign-in successful: Yes
```

### Type

Type this into your evidence notes:

```text
Kibana sign-in successful: Yes
```

---

# Task 11 - Confirm This Is Not Elastic Cloud

## Where to Work

Use **WIN11-CLIENT**.

Use Kibana in the browser.

## Steps

1. Look at the browser address bar.
2. Confirm the URL points to the Ubuntu SOC server IP address.
3. Confirm the URL does not point to Elastic Cloud.
4. Do not create any external accounts.

## Expected Result

Your Kibana URL should look similar to:

```text
http://10.1.1.10:5601
```

It should not look like an Elastic Cloud URL.

## Screenshot Checkpoint

Capture a screenshot showing the Kibana URL if required by your trainer.

## Record in Evidence Notes

### Copy

```text
Elastic deployment type: Self-managed lab Elastic
Elastic Cloud used: No
```

### Type

Type these lines into your evidence notes:

```text
Elastic deployment type: Self-managed lab Elastic
Elastic Cloud used: No
```

---

# Task 12 - Locate the Main Kibana Navigation Menu

## Where to Work

Use **WIN11-CLIENT**.

Use Kibana in the browser.

## Steps

1. Look for the navigation menu on the left side of Kibana.
2. If the menu is collapsed, select the menu icon.
3. Review the visible Kibana sections.
4. Look for items such as:

### Copy

```text
Discover
Dashboards
Management
Fleet
```

### Type

Look for these menu names in Kibana:

```text
Discover
Dashboards
Management
Fleet
```

## Expected Result

You should be able to see the main navigation areas in Kibana.

> [!hint]
> Kibana menus can vary slightly by version. If you do not see an item, use the search bar at the top of Kibana.

## Screenshot Checkpoint

Capture a screenshot showing the Kibana navigation menu.

## Record in Evidence Notes

### Copy

```text
Kibana navigation menu located: Yes
```

### Type

Type this into your evidence notes:

```text
Kibana navigation menu located: Yes
```

---

# Task 13 - Open Kibana Discover

## Where to Work

Use **WIN11-CLIENT**.

Use Kibana in the browser.

## Steps

1. In Kibana, open the navigation menu.
2. Select **Discover**.
3. If you cannot find Discover, use the Kibana search bar.
4. Search for Discover.

### Copy

```text
Discover
```

### Type

Type this into the Kibana search bar:

```text
Discover
```

5. Open **Discover**.

## Expected Result

The Discover page should open.

You may see a message asking for a data view, or you may see event data if data already exists.

> [!note]
> It is acceptable if no Windows logs are visible yet. Windows log collection begins in later labs.

## Screenshot Checkpoint

Capture a screenshot showing the Discover page.

## Record in Evidence Notes

### Copy

```text
Discover page opened: Yes
```

### Type

Type this into your evidence notes:

```text
Discover page opened: Yes
```

---

# Task 14 - Review the Discover Time Range Control

## Where to Work

Use **WIN11-CLIENT**.

Use Kibana Discover.

## Steps

1. Locate the time range control in Discover.
2. It is usually near the top right of the page.
3. Select the time range control.
4. Review the available time options.
5. Set the time range to **Last 24 hours** if available.

### Copy

```text
Last 24 hours
```

### Type

Select or type this time range if Kibana allows manual entry:

```text
Last 24 hours
```

6. Select **Update** or **Refresh** if required.

## Expected Result

Discover should now use a recent time range.

> [!note]
> Time range selection is important in later labs. If the time range is too narrow, you may not see expected events.

## Screenshot Checkpoint

Capture a screenshot showing Discover with the time range visible if required.

## Record in Evidence Notes

### Copy

```text
Discover time range reviewed: Last 24 hours
```

### Type

Type this into your evidence notes:

```text
Discover time range reviewed: Last 24 hours
```

---

# Task 15 - Locate the Data View Selector in Discover

## Where to Work

Use **WIN11-CLIENT**.

Use Kibana Discover.

## Steps

1. Stay on the Discover page.
2. Look for the Data View selector.
3. It may appear near the top left of the Discover page.
4. Select the Data View selector.
5. Review the available data views.

## Expected Result

You may see one or more data views.

Examples may include:

```text
logs-*
```

```text
metrics-*
```

```text
filebeat-*
```

```text
winlogbeat-*
```

```text
logs-windows.*
```

It is acceptable if your environment has no Windows data view yet.

## Screenshot Checkpoint

Capture a screenshot showing the Data View selector or available data views.

## Record in Evidence Notes

### Copy

```text
Data View selector located: Yes
Data Views observed:
```

### Type

Type these lines into your evidence notes, then add what you saw:

```text
Data View selector located: Yes
Data Views observed:
```

---

# Task 16 - Open Data Views from Stack Management

## Where to Work

Use **WIN11-CLIENT**.

Use Kibana in the browser.

## Steps

1. Open the Kibana navigation menu.
2. Look for **Stack Management**.
3. If you cannot find it, use the Kibana search bar.

### Copy

```text
Stack Management
```

### Type

Type this into the Kibana search bar:

```text
Stack Management
```

4. Open **Stack Management**.
5. Look for **Data Views**.
6. Open **Data Views**.

### Copy

```text
Data Views
```

### Type

Type this into the Kibana search bar if needed:

```text
Data Views
```

## Expected Result

The Data Views page should open.

You should be able to see existing data views or a page where data views can be created.

> [!note]
> Do not create a new data view unless your instructor tells you to. This lab is orientation only.

## Screenshot Checkpoint

Capture a screenshot showing the Data Views page.

## Record in Evidence Notes

### Copy

```text
Data Views page opened: Yes
```

### Type

Type this into your evidence notes:

```text
Data Views page opened: Yes
```

---

# Task 17 - Open Fleet

## Where to Work

Use **WIN11-CLIENT**.

Use Kibana in the browser.

## Steps

1. Open the Kibana navigation menu.
2. Look for **Fleet**.
3. If you cannot find Fleet, use the Kibana search bar.

### Copy

```text
Fleet
```

### Type

Type this into the Kibana search bar:

```text
Fleet
```

4. Open **Fleet**.
5. Review the page.
6. Look for tabs or sections such as **Agents**, **Agent policies**, or **Settings**.

### Copy

```text
Agents
Agent policies
Settings
```

### Type

Look for these labels in Fleet:

```text
Agents
Agent policies
Settings
```

## Expected Result

Fleet should open.

You may see no enrolled agents yet. That is acceptable.

> [!note]
> WIN11-CLIENT will be enrolled with Elastic Agent in Lab 03.

## Screenshot Checkpoint

Capture a screenshot showing the Fleet page.

## Record in Evidence Notes

### Copy

```text
Fleet page opened: Yes
Agents visible in Fleet:
```

### Type

Type these lines into your evidence notes, then add what you saw:

```text
Fleet page opened: Yes
Agents visible in Fleet:
```

Example:

```text
Agents visible in Fleet: None yet
```

---

# Task 18 - Review Agent Policies in Fleet

## Where to Work

Use **WIN11-CLIENT**.

Use Kibana Fleet.

## Steps

1. In Fleet, select **Agent policies** if visible.

### Copy

```text
Agent policies
```

### Type

Select or search for this label:

```text
Agent policies
```

2. Review the list of available policies.
3. Look for a Windows-related policy if one exists.

### Copy

```text
Windows
```

### Type

Look for the word:

```text
Windows
```

4. Do not change any policy settings in this lab.

## Expected Result

You may see an agent policy prepared for Windows endpoints.

Examples may include:

```text
Windows Endpoint Policy
```

or:

```text
Default policy
```

It is acceptable if the exact policy name is different.

## Screenshot Checkpoint

Capture a screenshot showing the Agent policies page if available.

## Record in Evidence Notes

### Copy

```text
Agent policies reviewed: Yes
Windows policy observed:
```

### Type

Type these lines into your evidence notes, then add what you saw:

```text
Agent policies reviewed: Yes
Windows policy observed:
```

---

# Task 19 - Open Dashboards

## Where to Work

Use **WIN11-CLIENT**.

Use Kibana in the browser.

## Steps

1. Open the Kibana navigation menu.
2. Look for **Dashboards**.
3. If you cannot find Dashboards, use the Kibana search bar.

### Copy

```text
Dashboards
```

### Type

Type this into the Kibana search bar:

```text
Dashboards
```

4. Open **Dashboards**.
5. Review the page.

## Expected Result

The Dashboards page should open.

You may see existing dashboards or an empty dashboard list.

> [!note]
> Dashboards may be used later to visualise log data, but this lab only asks you to locate the page.

## Screenshot Checkpoint

Capture a screenshot showing the Dashboards page.

## Record in Evidence Notes

### Copy

```text
Dashboards page opened: Yes
Dashboards observed:
```

### Type

Type these lines into your evidence notes, then add what you saw:

```text
Dashboards page opened: Yes
Dashboards observed:
```

---

# Task 20 - Locate License Management

## Where to Work

Use **WIN11-CLIENT**.

Use Kibana in the browser.

## Steps

1. Open the Kibana navigation menu.
2. Open **Stack Management**.

### Copy

```text
Stack Management
```

### Type

Type this into the Kibana search bar if needed:

```text
Stack Management
```

3. Look for **License Management**.

### Copy

```text
License Management
```

### Type

Type this into the Kibana search bar if needed:

```text
License Management
```

4. Open **License Management** if it is available.
5. Review the license type.
6. Do not start a trial.
7. Do not change the license.

## Expected Result

If License Management is available, the license should show Basic or a lab-prepared license state.

The expected course license is:

```text
Basic
```

If you cannot access License Management, record that it was not available to your account.

> [!alert]
> Do not start a free trial. This course is designed for free self-managed Elastic Basic features.

## Screenshot Checkpoint

Capture a screenshot showing the license page if available.

## Record in Evidence Notes

### Copy

```text
License Management page available:
Elastic license observed:
Trial started: No
```

### Type

Type these lines into your evidence notes, then add what you saw:

```text
License Management page available:
Elastic license observed:
Trial started: No
```

---

# Task 21 - Locate Kibana Help or Version Information

## Where to Work

Use **WIN11-CLIENT**.

Use Kibana in the browser.

## Steps

1. Look for the help menu, user menu, or space menu in Kibana.
2. Look for information such as version, help, or about information.
3. If visible, record the Kibana version.
4. If you cannot find the version, record that it was not located.

## Expected Result

You may find Kibana version information.

If not, that is acceptable for this lab.

## Screenshot Checkpoint

No screenshot is required unless instructed.

## Record in Evidence Notes

### Copy

```text
Kibana version observed:
```

### Type

Type this into your evidence notes, then add what you saw:

```text
Kibana version observed:
```

Example:

```text
Kibana version observed: Not located
```

---

# Task 22 - Create the Lab 02 Evidence Notes File on Windows

## Where to Work

Use **WIN11-CLIENT**.

Use **Notepad**.

## Steps

1. Return to **WIN11-CLIENT**.
2. Open **Notepad**.
3. Copy or type the evidence template below into Notepad.
4. Fill in the missing information using your lab results.
5. Select **File**.
6. Select **Save As**.
7. Browse to the Windows evidence folder.
8. Save the file with the required filename.

## Evidence Template

### Copy

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 02 - Elastic and Kibana Orientation

Student Name:
Date:

1. Lab Access

WIN11-CLIENT access confirmed:
UBUNTU-SOC access confirmed:
Ubuntu hostname:
Ubuntu IPv4 address:

2. Elastic Services

Elasticsearch service status:
Kibana service status:
Elasticsearch local response check:
Kibana local response check:

3. Kibana Access

Kibana URL:
Kibana opened from Windows browser:
Kibana sign-in successful:
Elastic deployment type:
Elastic Cloud used:

4. Kibana Orientation

Kibana navigation menu located:
Discover page opened:
Discover time range reviewed:
Data View selector located:
Data Views observed:
Data Views page opened:
Fleet page opened:
Agents visible in Fleet:
Agent policies reviewed:
Windows policy observed:
Dashboards page opened:
Dashboards observed:

5. License Review

License Management page available:
Elastic license observed:
Trial started:
Kibana version observed:

6. Lab Summary

Write 3 to 5 sentences explaining what you learned about Elasticsearch, Kibana, Discover, Fleet, Dashboards, and Data Views.
```

### Type

Type these headings and fields into Notepad:

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 02 - Elastic and Kibana Orientation

Student Name:
Date:

1. Lab Access

WIN11-CLIENT access confirmed:
UBUNTU-SOC access confirmed:
Ubuntu hostname:
Ubuntu IPv4 address:

2. Elastic Services

Elasticsearch service status:
Kibana service status:
Elasticsearch local response check:
Kibana local response check:

3. Kibana Access

Kibana URL:
Kibana opened from Windows browser:
Kibana sign-in successful:
Elastic deployment type:
Elastic Cloud used:

4. Kibana Orientation

Kibana navigation menu located:
Discover page opened:
Discover time range reviewed:
Data View selector located:
Data Views observed:
Data Views page opened:
Fleet page opened:
Agents visible in Fleet:
Agent policies reviewed:
Windows policy observed:
Dashboards page opened:
Dashboards observed:

5. License Review

License Management page available:
Elastic license observed:
Trial started:
Kibana version observed:

6. Lab Summary

Write 3 to 5 sentences explaining what you learned about Elasticsearch, Kibana, Discover, Fleet, Dashboards, and Data Views.
```

## Save Location

### Copy

```text
C:\BlueWave\Evidence
```

### Type

Type this path into the Save As location:

```text
C:\BlueWave\Evidence
```

## Filename

### Copy

```text
Lab02-Elastic-Orientation.txt
```

### Type

Type this filename exactly:

```text
Lab02-Elastic-Orientation.txt
```

## Expected Result

The file should be saved as:

```text
C:\BlueWave\Evidence\Lab02-Elastic-Orientation.txt
```

## Screenshot Checkpoint

Capture a screenshot showing the completed evidence notes file.

## Record in Evidence Notes

This task creates the main evidence notes file.

---

# Task 23 - Confirm the Windows Evidence Notes File Exists

## Where to Work

Use **WIN11-CLIENT**.

Use **Windows PowerShell**.

## Steps

1. Open **Windows PowerShell**.
2. Run the file validation command.

### Copy

```powershell
Test-Path "C:\BlueWave\Evidence\Lab02-Elastic-Orientation.txt"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab02-Elastic-Orientation.txt"
```

3. Press **Enter**.

## Expected Result

PowerShell should return:

```text
True
```

If the result is `False`, check the folder path and filename.

## Screenshot Checkpoint

Capture a screenshot showing the validation result if required.

## Record in Evidence Notes

No additional note is required.

---

# Task 24 - Create an Optional Ubuntu Evidence Copy

## Where to Work

Use **UBUNTU-SOC**.

Use **Terminal**.

## Steps

1. Return to **UBUNTU-SOC**.
2. Open **Terminal**.
3. Create or open the Ubuntu evidence notes file.

### Copy

```bash
nano /home/student/bluewave/evidence/Lab02-Elastic-Orientation.txt
```

### Type

Type this into Terminal:

```bash
nano /home/student/bluewave/evidence/Lab02-Elastic-Orientation.txt
```

4. Press **Enter**.
5. Copy or type the Ubuntu evidence copy template below.
6. Fill in the missing information.
7. Press **Ctrl + O**.
8. Press **Enter** to save.
9. Press **Ctrl + X** to exit nano.

## Ubuntu Evidence Copy Template

### Copy

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 02 - Ubuntu Evidence Copy

Ubuntu hostname:
Ubuntu IPv4 address:
Elasticsearch service status:
Kibana service status:
Kibana URL:
Kibana opened from Windows browser:
Discover page opened:
Fleet page opened:
Dashboards page opened:
License observed:
```

### Type

Type these lines into nano:

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 02 - Ubuntu Evidence Copy

Ubuntu hostname:
Ubuntu IPv4 address:
Elasticsearch service status:
Kibana service status:
Kibana URL:
Kibana opened from Windows browser:
Discover page opened:
Fleet page opened:
Dashboards page opened:
License observed:
```

## Expected Result

The optional Ubuntu evidence copy should be saved at:

```text
/home/student/bluewave/evidence/Lab02-Elastic-Orientation.txt
```

## Screenshot Checkpoint

No screenshot is required unless instructed.

## Record in Evidence Notes

### Copy

```text
Ubuntu Lab 02 evidence copy created: Yes
```

### Type

Type this into your Windows evidence notes if applicable:

```text
Ubuntu Lab 02 evidence copy created: Yes
```

---

# Task 25 - Final Validation

## Where to Work

Use both machines.

## Steps on WIN11-CLIENT

1. Open **Windows PowerShell**.
2. Validate the Windows Lab 02 evidence file.

### Copy

```powershell
Test-Path "C:\BlueWave\Evidence\Lab02-Elastic-Orientation.txt"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab02-Elastic-Orientation.txt"
```

3. Confirm the result is:

```text
True
```

4. Open **File Explorer**.
5. Browse to the evidence folder.

### Copy

```text
C:\BlueWave\Evidence
```

### Type

Type this path into the File Explorer address bar:

```text
C:\BlueWave\Evidence
```

6. Confirm that `Lab02-Elastic-Orientation.txt` exists.

## Steps on UBUNTU-SOC

1. Open **Terminal**.
2. Confirm Elasticsearch status one final time.

### Copy

```bash
systemctl is-active elasticsearch
```

### Type

Type this into Terminal:

```bash
systemctl is-active elasticsearch
```

3. Press **Enter**.
4. Confirm Kibana status one final time.

### Copy

```bash
systemctl is-active kibana
```

### Type

Type this into Terminal:

```bash
systemctl is-active kibana
```

5. Press **Enter**.

## Expected Result

The Windows evidence file should exist.

Elasticsearch should return:

```text
active
```

Kibana should return:

```text
active
```

## Screenshot Checkpoint

Capture a final screenshot showing the Windows evidence file.

Capture a final screenshot showing the Elasticsearch and Kibana active checks if required.

---

# Validation Checklist

Before you finish the lab, confirm each item is complete.

- [ ] I signed in to WIN11-CLIENT.
- [ ] I signed in to UBUNTU-SOC.
- [ ] I confirmed the Ubuntu hostname.
- [ ] I identified the Ubuntu IP address.
- [ ] I checked Elasticsearch service status.
- [ ] I checked Kibana service status.
- [ ] I checked the local Elasticsearch response.
- [ ] I checked the local Kibana response.
- [ ] I built the Kibana URL.
- [ ] I opened Kibana from the Windows browser.
- [ ] I signed in to Kibana.
- [ ] I confirmed the lab is using self-managed Elastic, not Elastic Cloud.
- [ ] I located the main Kibana navigation menu.
- [ ] I opened Discover.
- [ ] I reviewed the Discover time range control.
- [ ] I located the Data View selector.
- [ ] I opened Data Views.
- [ ] I opened Fleet.
- [ ] I reviewed Agent policies if available.
- [ ] I opened Dashboards.
- [ ] I checked License Management if available.
- [ ] I did not start a trial.
- [ ] I created `C:\BlueWave\Evidence\Lab02-Elastic-Orientation.txt`.
- [ ] I captured the required screenshots.
- [ ] I reviewed my evidence notes for completeness.

---

# Troubleshooting

## Problem: Elasticsearch service is not running

Check the service status again.

### Copy

```bash
systemctl status elasticsearch
```

### Type

Type this into Terminal:

```bash
systemctl status elasticsearch
```

Possible fix:

Tell your instructor or lab support that Elasticsearch is not active.

> [!note]
> Students should not spend the lab installing Elasticsearch. It should already be installed in the Skillable environment.

---

## Problem: Kibana service is not running

Check the service status again.

### Copy

```bash
systemctl status kibana
```

### Type

Type this into Terminal:

```bash
systemctl status kibana
```

Possible fix:

Tell your instructor or lab support that Kibana is not active.

---

## Problem: The browser cannot open Kibana

Confirm the Ubuntu IP address.

### Copy

```bash
hostname -I
```

### Type

Type this into Terminal:

```bash
hostname -I
```

Confirm the Kibana URL format.

### Copy

```text
http://<UBUNTU-SOC-IP>:5601
```

### Type

Type your Kibana URL using this format:

```text
http://<UBUNTU-SOC-IP>:5601
```

Example:

```text
http://10.1.1.10:5601
```

Possible fix:

Check that you did not use `127.0.0.1` from the Windows browser.

---

## Problem: Kibana login fails

Possible fix:

Check that you used the lab username and password provided by your instructor or Skillable environment.

If the credentials still fail, notify your instructor.

---

## Problem: Discover shows no data

Possible fix:

This may be normal in Lab 02.

Windows logs may not be connected yet.

Set the time range to:

### Copy

```text
Last 24 hours
```

### Type

Select or type this time range:

```text
Last 24 hours
```

Then continue.

---

## Problem: Fleet is not visible

Possible fix:

Use the Kibana search bar.

### Copy

```text
Fleet
```

### Type

Type this into the Kibana search bar:

```text
Fleet
```

If Fleet is still not visible, record that Fleet was not visible to your account and continue.

---

## Problem: License Management is not visible

Possible fix:

Use the Kibana search bar.

### Copy

```text
License Management
```

### Type

Type this into the Kibana search bar:

```text
License Management
```

If it is still not visible, record that the page was not available to your account.

Do not start a trial.

---

## Problem: Data Views page is not visible

Possible fix:

Use the Kibana search bar.

### Copy

```text
Data Views
```

### Type

Type this into the Kibana search bar:

```text
Data Views
```

If it is still not visible, record that the page was not available to your account.

---

## Problem: The evidence folder does not exist on Windows

Create the folder again.

### Copy

```powershell
New-Item -Path "C:\BlueWave\Evidence" -ItemType Directory -Force
```

### Type

Type this into PowerShell:

```powershell
New-Item -Path "C:\BlueWave\Evidence" -ItemType Directory -Force
```

Confirm the folder exists.

### Copy

```powershell
Test-Path "C:\BlueWave\Evidence"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence"
```

Expected result:

```text
True
```

---

## Problem: The Lab 02 evidence file is missing

Check the filename.

### Copy

```text
Lab02-Elastic-Orientation.txt
```

### Type

Type this filename exactly:

```text
Lab02-Elastic-Orientation.txt
```

Confirm with PowerShell.

### Copy

```powershell
Test-Path "C:\BlueWave\Evidence\Lab02-Elastic-Orientation.txt"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab02-Elastic-Orientation.txt"
```

Expected result:

```text
True
```

---

# Knowledge Check

Answer the following questions.

1. What service stores and indexes Elastic data?
2. What service provides the Elastic web interface?
3. What Kibana page is used to search and inspect events?
4. What Kibana area is used to manage Elastic Agents?
5. What Kibana object tells Kibana which data to search?
6. What port does Kibana usually use in this lab?
7. Why should you not use `127.0.0.1` from the Windows browser to access Kibana on Ubuntu?
8. What license type should this course use?
9. Should students create an Elastic Cloud account for this course?
10. Why is the Discover time range important during log analysis?

---

# Summary

In this lab, you completed the following tasks:

- Verified that Elasticsearch is running on UBUNTU-SOC.
- Verified that Kibana is running on UBUNTU-SOC.
- Identified the Ubuntu IP address.
- Built the Kibana URL.
- Opened Kibana from the Windows browser.
- Signed in to Kibana.
- Confirmed the lab uses self-managed Elastic rather than Elastic Cloud.
- Opened Discover.
- Reviewed the Discover time range control.
- Located Data Views.
- Opened Fleet.
- Reviewed Agent policies if available.
- Opened Dashboards.
- Checked the Elastic license page if available.
- Created Lab 02 evidence notes.

You are now ready for Lab 03, where you will enrol WIN11-CLIENT into Elastic using Elastic Agent.

---

# Deliverables

Submit or retain the following items as directed by your instructor.

| Deliverable | Location |
|---|---|
| Windows Lab 02 evidence notes | `C:\BlueWave\Evidence\Lab02-Elastic-Orientation.txt` |
| Optional Ubuntu Lab 02 evidence copy | `/home/student/bluewave/evidence/Lab02-Elastic-Orientation.txt` |
| Screenshot of Elasticsearch service status | Skillable submission area |
| Screenshot of Kibana service status | Skillable submission area |
| Screenshot of Ubuntu IP address | Skillable submission area |
| Screenshot of Kibana login or home page | Skillable submission area |
| Screenshot of Discover page | Skillable submission area |
| Screenshot of Data Views page | Skillable submission area |
| Screenshot of Fleet page | Skillable submission area |
| Screenshot of Dashboards page | Skillable submission area |
| Screenshot of License Management page, if available | Skillable submission area |
| Screenshot of completed Lab 02 evidence notes | Skillable submission area |

---

# Instructor Notes

## Expected Knowledge Check Answers

1. Elasticsearch stores and indexes Elastic data.
2. Kibana provides the Elastic web interface.
3. Discover is used to search and inspect events.
4. Fleet is used to manage Elastic Agents.
5. A Data View tells Kibana which data to search.
6. Kibana usually uses port `5601`.
7. `127.0.0.1` points to the local machine. From Windows, it would point to WIN11-CLIENT, not UBUNTU-SOC.
8. The course should use the free Elastic Basic license.
9. No. Students should not create an Elastic Cloud account.
10. The Discover time range controls which events are visible. If it is too narrow, expected events may not appear.

---

## Expected Evidence Files

Students should create:

```text
C:\BlueWave\Evidence\Lab02-Elastic-Orientation.txt
```

Optional Ubuntu copy:

```text
/home/student/bluewave/evidence/Lab02-Elastic-Orientation.txt
```

---

## Expected Service Checks

Elasticsearch status command:

```bash
systemctl status elasticsearch
```

Expected status:

```text
Active: active (running)
```

Kibana status command:

```bash
systemctl status kibana
```

Expected status:

```text
Active: active (running)
```

Quick final checks:

```bash
systemctl is-active elasticsearch
```

Expected result:

```text
active
```

```bash
systemctl is-active kibana
```

Expected result:

```text
active
```

---

## Expected Kibana URL

The expected URL format is:

```text
http://<UBUNTU-SOC-IP>:5601
```

Example:

```text
http://10.1.1.10:5601
```

The exact IP address depends on the Skillable environment.

---

## Common Student Mistakes

| Mistake | Instructor Guidance |
|---|---|
| Student uses `127.0.0.1:5601` from Windows | Explain that this points to Windows, not Ubuntu |
| Student starts an Elastic trial | Remind them the course uses the free Basic license |
| Student tries to create an Elastic Cloud account | Remind them this is a self-managed lab |
| Student panics because Discover has no data | Explain that Windows logs are not collected until later labs |
| Student cannot find Fleet | Have them use the Kibana search bar |
| Student cannot find License Management | Record it as unavailable if permissions do not allow access |
| Student changes Fleet or Data View settings | Remind them Lab 02 is orientation only |
| Student forgets screenshots | Have them revisit each page and capture the required screenshots |

---

## Grading Guidance

Suggested grading allocation:

| Criteria | Points |
|---|---:|
| Elasticsearch service verified | 15 |
| Kibana service verified | 15 |
| Kibana accessed from Windows browser | 15 |
| Discover opened and documented | 10 |
| Fleet opened and documented | 10 |
| Dashboards opened and documented | 10 |
| Data Views located and documented | 10 |
| License reviewed or access limitation documented | 5 |
| Evidence notes completed clearly | 5 |
| Screenshots captured | 5 |
| Total | 100 |

---

## Free Elastic Basic License Reminder

This course must use:

- Self-managed Elastic.
- Free Elastic Basic license.
- No Elastic Cloud.
- No paid subscriptions.
- No external internet access requirement.

If a feature appears to require a paid license, use the free alternative:

- Kibana Discover.
- Saved queries.
- Manual analysis.
- Evidence screenshots.
- Markdown or text incident notes.

---

## Fallback if Fleet Is Not Available

If Fleet is not visible or not configured:

1. Have students record that Fleet was not available.
2. Continue with Discover, Dashboards, Data Views, and service verification.
3. In Lab 03, the instructor may provide an alternate Elastic Agent enrolment method or preconfigured Fleet access.

---

End of Lab 02.
