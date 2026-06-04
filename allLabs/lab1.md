# Lab 01 - BlueWave Clinic Environment Orientation and Case Study Setup

## Estimated Time

60–90 minutes

---

## Lab Purpose

In this lab, you will become familiar with the two virtual machines used in the BlueWave Clinic cyber operations environment. You will identify system names, user accounts, IP addresses, network connectivity, lab file locations, and evidence folders.

This lab prepares you for later labs where the Windows 11 endpoint sends logs to Elastic and Kibana on the Ubuntu SOC server.

---

## How to Use Copy or Type Inputs

To remove unnecessary duplication, this version uses one input block for each command, path, search term, filename, or template.

When you see a block labelled **Student Input - Copy or Type**, either copy it or type it manually.

> [!note]
> Use either Copy or Type. You do not need to do both.

> [!alert]
> Type commands exactly as shown. Small spelling, spacing, quotation mark, slash, or symbol mistakes can cause errors.

---

## Learning Objectives

By the end of this lab, you will be able to:

- Identify the purpose of each virtual machine in the lab.
- Use PowerShell on Windows to collect basic system information.
- Use Terminal on Ubuntu to collect basic system information.
- Identify IPv4 addresses on Windows and Ubuntu.
- Test basic connectivity between the two lab machines.
- Create evidence folders on Windows and Ubuntu.
- Locate the preloaded lab files.
- Create a structured Lab 01 evidence notes file.

---

## Scenario

BlueWave Clinic is a small healthcare organisation that is building a basic Security Operations Centre, also known as a SOC.

You are a junior cyber operations analyst. Before collecting logs, using Kibana, or investigating alerts, you must understand the lab environment.

| Machine | Role |
|---|---|
| WIN11-CLIENT | Windows 11 endpoint and analyst workstation |
| UBUNTU-SOC | Ubuntu SOC server that will later run Elastic, Kibana, and Fleet |

In later labs:

- WIN11-CLIENT will generate Windows logs.
- WIN11-CLIENT will generate Sysmon process activity.
- Elastic Agent will send logs to UBUNTU-SOC.
- Kibana will be used to search and analyse the logs.

> [!note]
> Elastic and Kibana are not used in this lab. They are introduced in Lab 02.

> [!alert]
> Do not run `BlueWaveActivitySimulator.exe` in this lab. The simulator is introduced later.

---

## Required Machines

| Machine | Used For |
|---|---|
| WIN11-CLIENT | Windows commands, evidence folder, lab notes |
| UBUNTU-SOC | Ubuntu commands, evidence folder, network checks |

---

## Evidence You Will Create

| Evidence File | Location |
|---|---|
| Lab01-Environment-Notes.txt | +++C:\BlueWave\Evidence\Lab01-Environment-Notes.txt+++ |
| Lab01-Environment-Notes.txt | +++/home/student/bluewave/evidence/Lab01-Environment-Notes.txt+++ |

---

## Important Paths

### Windows Paths

| Path | Purpose |
|---|---|
| `C:\LabFiles` | Main Windows lab files folder |
| `C:\LabFiles\Tools` | Preloaded tools |
| `C:\LabFiles\Simulators` | Preloaded simulator files |
| `C:\LabFiles\Logs` | Sample logs |
| `C:\LabFiles\Templates` | Evidence and report templates |
| `C:\BlueWave\Evidence` | Windows evidence folder |

### Ubuntu Paths

| Path | Purpose |
|---|---|
| `/home/student/labfiles` | Main Ubuntu lab files folder |
| `/home/student/labfiles/logs` | Sample logs |
| `/home/student/bluewave/evidence` | Ubuntu evidence folder |

---

## Screenshots You Should Capture

Recommended screenshots:

1. Windows hostname and username.
2. Windows IPv4 address.
3. Ubuntu hostname and username.
4. Ubuntu IPv4 address.
5. Windows-to-Ubuntu connectivity test.
6. Ubuntu-to-Windows connectivity test.
7. Windows evidence folder.
8. Ubuntu evidence folder.
9. Windows lab files folder listing.
10. Ubuntu lab files folder listing.
11. Completed Windows evidence notes file.

---

# Task 1 - Sign In to WIN11-CLIENT

## Where to Work

Use **WIN11-CLIENT**.

## Steps

1. Open the **WIN11-CLIENT** virtual machine.
2. Sign in using the credentials provided by your instructor or Skillable environment.
3. Wait for the Windows desktop to load.
4. Confirm you can see the Windows desktop.

## Expected Result

You should be signed in to the Windows 11 endpoint.

## Record in Evidence Notes

```text
Windows VM access confirmed: Yes
```

---

# Task 2 - Confirm the Windows Operating System

## Where to Work

Use **WIN11-CLIENT**.

## Steps

1. Select the **Windows Start** menu.
2. Search for **About your PC**.

### Student Input - Copy or Type

```text
About your PC
```

3. Open **About your PC**.
4. Confirm the operating system is Windows 11.
5. Close the Settings window.

## Expected Result

You should see that the system is running Windows 11.

## Record in Evidence Notes

```text
Windows operating system: Windows 11
```

---

# Task 3 - Open Windows PowerShell

## Where to Work

Use **WIN11-CLIENT**.

## Steps

1. Select the **Windows Start** menu.
2. Search for **PowerShell**.

### Student Input - Copy or Type

```text```  ++++PowerShell++++


3. Open **Windows PowerShell**.

## Expected Result

A PowerShell window should open.

## Record in Evidence Notes

```text
PowerShell opened: Yes
```

---

# Task 4 - Identify Windows System Information

## Where to Work

Use **WIN11-CLIENT** and **Windows PowerShell**.

## Steps

Run each command and record the result.

| Purpose | Student Input - Copy or Type | Record In Evidence Notes |
|---|---|---|
| Windows hostname | +++hostname+++ | `Windows hostname:` |
| Logged-in user | +++whoami+++ | `Windows logged-in user:` |
| Computer name variable | +++$env:COMPUTERNAME+++ | `Windows COMPUTERNAME value:` |

## Expected Result

The hostname and computer name should usually be:

```text
WIN11-CLIENT
```

The logged-in user may look similar to:

```text
win11-client\student
```

## Screenshot Checkpoint

Capture a screenshot showing the commands and results.

---

# Task 5 - Identify the Windows IPv4 Address

## Where to Work

Use **WIN11-CLIENT** and **Windows PowerShell**.

## Steps

1. Run the Windows network configuration command.

### Student Input - Copy or Type

```powershell
ipconfig
```

2. Look for the active network adapter.
3. Find the line named `IPv4 Address`.
4. Run the cleaner IPv4 display command.

### Student Input - Copy or Type

```powershell
Get-NetIPAddress -AddressFamily IPv4 | Where-Object {$_.IPAddress -notlike "127.*"} | Select-Object InterfaceAlias, IPAddress, PrefixLength
```

5. Record the IPv4 address and active network adapter.

## Expected Result

You should see an IPv4 address.

Example:

```text
InterfaceAlias        IPAddress       PrefixLength
--------------        ---------       ------------
Ethernet              10.1.1.20       24
```

> [!alert]
> Do not record `127.0.0.1`. That is the loopback address.

> [!hint]
> Ignore adapters that show `Media disconnected`.

## Record in Evidence Notes

```text
Windows IPv4 address:
Windows network adapter:
```

---

# Task 6 - Sign In to UBUNTU-SOC and Open Terminal

## Where to Work

Use **UBUNTU-SOC**.

## Steps

1. Open the **UBUNTU-SOC** virtual machine.
2. Sign in using the credentials provided by your instructor or Skillable environment.
3. Open **Terminal**.
4. Wait for the Terminal prompt to appear.

## Expected Result

A Terminal window should open.

The prompt may look similar to:

```text
student@UBUNTU-SOC:~$
```

## Record in Evidence Notes

```text
Ubuntu VM access confirmed: Yes
Ubuntu Terminal opened: Yes
```

---

# Task 7 - Identify Ubuntu System Information

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

Run each command and record the result.

| Purpose | Student Input - Copy or Type | Record In Evidence Notes |
|---|---|---|
| Ubuntu hostname | `hostname` | `Ubuntu hostname:` |
| Logged-in user | `whoami` | `Ubuntu logged-in user:` |
| Home directory | `pwd` | `Ubuntu home directory:` |

## Expected Result

The Ubuntu hostname should usually be:

```text
UBUNTU-SOC
```

The logged-in user should usually be:

```text
student
```

The home directory should usually be:

```text
/home/student
```

## Screenshot Checkpoint

Capture a screenshot showing the commands and results.

---

# Task 8 - Identify the Ubuntu IPv4 Address

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Run the quick Ubuntu IP address command.

### Student Input - Copy or Type

```bash
hostname -I
```

2. Record the IP address shown.
3. Run the detailed network command.

### Student Input - Copy or Type

```bash
ip addr show
```

4. Look for the active network interface.
5. Find the IP address after the word `inet`.
6. Record the Ubuntu IPv4 address and network interface name.

## Expected Result

You should see an IPv4 address.

Example:

```text
inet 10.1.1.10/24
```

> [!alert]
> Do not record `127.0.0.1`. That is the loopback address.

## Record in Evidence Notes

```text
Ubuntu IPv4 address:
Ubuntu network interface:
```

---

# Task 9 - Test Connectivity from Windows to Ubuntu

## Where to Work

Use **WIN11-CLIENT** and **Windows PowerShell**.

## Steps

1. Return to **WIN11-CLIENT**.
2. Find the Ubuntu IPv4 address you recorded.
3. Run the ping command, replacing `<UBUNTU-SOC-IP>` with your Ubuntu IP address.

### Student Input - Copy or Type

```powershell
ping <UBUNTU-SOC-IP>
```

Example:

```powershell
ping 10.1.1.10
```

4. Run the PowerShell connectivity command.

### Student Input - Copy or Type

```powershell
Test-NetConnection <UBUNTU-SOC-IP>
```

Example:

```powershell
Test-NetConnection 10.1.1.10
```

## Expected Result

A successful ping may show:

```text
Reply from 10.1.1.10: bytes=32 time<1ms TTL=64
```

A successful `Test-NetConnection` result may show:

```text
PingSucceeded : True
```

> [!note]
> Some lab environments block ping. If ping fails, record the result and continue.

> [!alert]
> Do not change firewall settings in this lab.

## Record in Evidence Notes

```text
Windows-to-Ubuntu command:
Windows-to-Ubuntu result:
```

---

# Task 10 - Test Connectivity from Ubuntu to Windows

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Return to **UBUNTU-SOC**.
2. Find the Windows IPv4 address you recorded.
3. Run the ping command, replacing `<WIN11-CLIENT-IP>` with your Windows IP address.

### Student Input - Copy or Type

```bash
ping -c 4 <WIN11-CLIENT-IP>
```

Example:

```bash
ping -c 4 10.1.1.20
```

## Expected Result

A successful result may show:

```text
4 packets transmitted, 4 received, 0% packet loss
```

> [!note]
> A failed ping does not always mean the lab is broken. The Windows firewall may block ping.

> [!alert]
> Do not change firewall settings in this lab.

## Record in Evidence Notes

```text
Ubuntu-to-Windows command:
Ubuntu-to-Windows result:
```

---

# Task 11 - Create Evidence Folders

## Where to Work

Use **WIN11-CLIENT** PowerShell and **UBUNTU-SOC** Terminal.

## Steps on WIN11-CLIENT

Create and validate the Windows evidence folder.

### Student Input - Copy or Type

```powershell
New-Item -Path "C:\BlueWave\Evidence" -ItemType Directory -Force
Test-Path "C:\BlueWave\Evidence"
```

Open the folder in File Explorer.

### Student Input - Copy or Type

```text
C:\BlueWave\Evidence
```

## Steps on UBUNTU-SOC

Create and validate the Ubuntu evidence folder.

### Student Input - Copy or Type

```bash
mkdir -p /home/student/bluewave/evidence
ls -ld /home/student/bluewave/evidence
```

## Expected Result

The Windows `Test-Path` command should return:

```text
True
```

Ubuntu should show folder details for:

```text
/home/student/bluewave/evidence
```

## Record in Evidence Notes

```text
Windows evidence folder: C:\BlueWave\Evidence
Ubuntu evidence folder: /home/student/bluewave/evidence
```

---

# Task 12 - Check Preloaded Lab Files

## Where to Work

Use both machines.

## Steps on WIN11-CLIENT

Check the Windows lab files folder.

### Student Input - This folder contains information on tools, simulators, logs, and templates used in this course. 

```powershell
Test-Path "C:\LabFiles"
Get-ChildItem "C:\LabFiles"
```

Look for:

```text files
Tools
Simulators
Logs
```

## Steps on UBUNTU-SOC

Check the Ubuntu lab files folder.

### Student Input - Copy or Type

```bash
ls -la /home/student/labfiles
```

Look for:

```text files 
Logs
```

> [!alert]
> Do not run files from the `Simulators` folder in this lab.

## Record in Evidence Notes

```text
Windows lab files path: C:\LabFiles
Windows lab folders seen:
Ubuntu lab files path: /home/student/labfiles
Ubuntu lab folders seen:
```

---

# Task 13 - Create the Lab 01 Evidence Notes File on Windows

## Where to Work

Use **WIN11-CLIENT** and **Notepad**.

## Steps

1. Open **Notepad**.
2. Copy or type the template below.
3. Fill in the missing information using your lab results.
4. Save the file in the required folder.

### Save Location - Copy or Type

```text
C:\BlueWave\Evidence
```

### Filename - Copy or Type

```text
Lab01-Environment-Notes.txt
```

### Evidence Template - Copy or Type

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 01 - Environment Orientation and Case Study Setup

Student Name:
Date:

1. Windows Endpoint

Windows VM access confirmed:
Windows operating system:
PowerShell opened:
Windows hostname:
Windows logged-in user:
Windows COMPUTERNAME value:
Windows IPv4 address:
Windows network adapter:
Windows evidence folder:

2. Ubuntu SOC Server

Ubuntu VM access confirmed:
Ubuntu Terminal opened:
Ubuntu hostname:
Ubuntu logged-in user:
Ubuntu home directory:
Ubuntu IPv4 address:
Ubuntu network interface:
Ubuntu evidence folder:

3. Connectivity Tests

Windows-to-Ubuntu command:
Windows-to-Ubuntu result:

Ubuntu-to-Windows command:
Ubuntu-to-Windows result:

4. Lab File Locations

Windows lab files path:
Windows lab folders seen:

Ubuntu lab files path:
Ubuntu lab folders seen:

5. Lab Summary

Write 3 to 5 sentences explaining what you learned about the BlueWave Clinic lab environment.
```

## Expected Result

The file should be saved as:

```text
C:\BlueWave\Evidence\Lab01-Environment-Notes.txt
```

## Screenshot Checkpoint

Capture a screenshot showing the completed evidence notes file.

---

# Task 14 - Confirm the Windows Evidence Notes File Exists

## Where to Work

Use **WIN11-CLIENT** and **Windows PowerShell**.

## Steps

Run the validation command.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\Evidence\Lab01-Environment-Notes.txt"
```

## Expected Result

PowerShell should return:

```text
True
```

---

# Task 15 - Create the Ubuntu Evidence Copy

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Open a new notes file with nano.

### Student Input - Copy or Type

```bash
nano /home/student/bluewave/evidence/Lab01-Environment-Notes.txt
```

2. Copy or type the Ubuntu evidence copy template below.
3. Fill in the missing information.
4. Press **Ctrl + O**.
5. Press **Enter**.
6. Press **Ctrl + X**.

### Ubuntu Evidence Copy Template - Copy or Type

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 01 - Ubuntu Evidence Copy

Windows hostname:
Windows IPv4 address:

Ubuntu hostname:
Ubuntu IPv4 address:

Windows evidence folder:
C:\BlueWave\Evidence

Ubuntu evidence folder:
/home/student/bluewave/evidence

Connectivity summary:
```

## Expected Result

The Ubuntu evidence copy should be saved at:

```text
/home/student/bluewave/evidence/Lab01-Environment-Notes.txt
```

---

# Task 16 - Confirm the Ubuntu Evidence Notes File Exists

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

List and display the file.

### Student Input - Copy or Type

```bash
ls -l /home/student/bluewave/evidence/Lab01-Environment-Notes.txt
cat /home/student/bluewave/evidence/Lab01-Environment-Notes.txt
```

## Expected Result

You should see the file listed and the contents displayed.

## Screenshot Checkpoint

Capture a screenshot showing the Ubuntu evidence notes file contents.

---

# Task 17 - Final Validation

## Where to Work

Use both machines.

## Steps on WIN11-CLIENT

Validate the Windows evidence file.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\Evidence\Lab01-Environment-Notes.txt"
```

Expected result:

```text
True
```

## Steps on UBUNTU-SOC

Validate the Ubuntu evidence file.

### Student Input - Copy or Type

```bash
test -f /home/student/bluewave/evidence/Lab01-Environment-Notes.txt && echo "Ubuntu evidence file exists"
```

Expected result:

```text
Ubuntu evidence file exists
```

## Expected Result

Both evidence files should exist. Your notes should include hostnames, usernames, IP addresses, connectivity results, and lab file folder checks.

---

# Validation Checklist

Before you finish the lab, confirm each item is complete.

- [ ] I signed in to WIN11-CLIENT.
- [ ] I confirmed WIN11-CLIENT is running Windows 11.
- [ ] I opened Windows PowerShell.
- [ ] I recorded the Windows hostname.
- [ ] I recorded the Windows logged-in user.
- [ ] I recorded the Windows COMPUTERNAME value.
- [ ] I recorded the Windows IPv4 address.
- [ ] I recorded the Windows network adapter.
- [ ] I signed in to UBUNTU-SOC.
- [ ] I opened Ubuntu Terminal.
- [ ] I recorded the Ubuntu hostname.
- [ ] I recorded the Ubuntu logged-in user.
- [ ] I recorded the Ubuntu home directory.
- [ ] I recorded the Ubuntu IPv4 address.
- [ ] I recorded the Ubuntu network interface.
- [ ] I tested connectivity from Windows to Ubuntu.
- [ ] I tested connectivity from Ubuntu to Windows.
- [ ] I created `C:\BlueWave\Evidence`.
- [ ] I created `/home/student/bluewave/evidence`.
- [ ] I checked `C:\LabFiles`.
- [ ] I checked `/home/student/labfiles`.
- [ ] I created `C:\BlueWave\Evidence\Lab01-Environment-Notes.txt`.
- [ ] I created `/home/student/bluewave/evidence/Lab01-Environment-Notes.txt`.
- [ ] I captured the required screenshots.
- [ ] I reviewed my evidence notes for completeness.

---

# Troubleshooting

## PowerShell or Terminal does not open

Search for the required tool.

### Windows Search - Copy or Type

```text
PowerShell
```

### Ubuntu Search - Copy or Type

```text
Terminal
```

---

## Hostname is different from the lab guide

Run the hostname command again and record the exact result.

### Windows - Copy or Type

```powershell
hostname
```

### Ubuntu - Copy or Type

```bash
hostname
```

Tell your instructor if the hostname is very different from the lab instructions.

---

## IP address output shows many adapters

Do not record the loopback address.

```text
127.0.0.1
```

On Windows, run the cleaner IPv4 command again.

```powershell
Get-NetIPAddress -AddressFamily IPv4 | Where-Object {$_.IPAddress -notlike "127.*"} | Select-Object InterfaceAlias, IPAddress, PrefixLength
```

On Ubuntu, review detailed IP output.

```bash
ip addr show
```

Look for an active interface and an `inet` address that is not `127.0.0.1`.

---

## Ping fails

First confirm the correct IP address was used.

### Windows to Ubuntu - Copy or Type

```powershell
ping <UBUNTU-SOC-IP>
```

### Ubuntu to Windows - Copy or Type

```bash
ping -c 4 <WIN11-CLIENT-IP>
```

If ping still fails, record the failed result and continue.

> [!alert]
> Do not change firewall settings in this lab.

---

## Evidence folder is missing

Create and validate the missing folder.

### Windows - Copy or Type

```powershell
New-Item -Path "C:\BlueWave\Evidence" -ItemType Directory -Force
Test-Path "C:\BlueWave\Evidence"
```

### Ubuntu - Copy or Type

```bash
mkdir -p /home/student/bluewave/evidence
ls -ld /home/student/bluewave/evidence
```

---

## Evidence file is missing

Confirm the required filename and location.

### Windows Filename

```text
Lab01-Environment-Notes.txt
```

### Windows Path

```text
C:\BlueWave\Evidence
```

### Windows Validation - Copy or Type

```powershell
Test-Path "C:\BlueWave\Evidence\Lab01-Environment-Notes.txt"
```

### Ubuntu Validation - Copy or Type

```bash
ls -l /home/student/bluewave/evidence/Lab01-Environment-Notes.txt
```

---

## Lab files folder is missing

Check the required path.

### Windows - Copy or Type

```powershell
Test-Path "C:\LabFiles"
Get-ChildItem "C:\LabFiles"
```

### Ubuntu - Copy or Type

```bash
ls -la /home/student/labfiles
```

If the path is still missing, record the issue and notify your instructor.

> [!alert]
> Do not download files from the internet.

---

# Knowledge Check

Answer the following questions.

1. What is the role of WIN11-CLIENT in the BlueWave Clinic lab?
2. What is the role of UBUNTU-SOC in the BlueWave Clinic lab?
3. What PowerShell command shows the Windows hostname?
4. What PowerShell command shows the currently logged-in Windows user?
5. What Ubuntu command shows the Ubuntu hostname?
6. What Ubuntu command shows the currently logged-in Ubuntu user?
7. Why should you avoid recording `127.0.0.1` as the lab IP address?
8. What folder is used to store Windows evidence?
9. What folder is used to store Ubuntu evidence?
10. Why is it important to record hostnames during a cyber operations investigation?

---

# Summary

In this lab, you completed the following tasks:

- Signed in to WIN11-CLIENT.
- Confirmed the Windows 11 endpoint role.
- Opened Windows PowerShell.
- Collected Windows hostname, user, computer name, and IP address information.
- Signed in to UBUNTU-SOC.
- Opened Ubuntu Terminal.
- Collected Ubuntu hostname, user, home directory, and IP address information.
- Tested basic connectivity between the two virtual machines.
- Created Windows and Ubuntu evidence folders.
- Checked the preloaded lab file locations.
- Created Lab 01 evidence notes.

You are now ready for Lab 02, where you will access Elastic and Kibana.

---

# Deliverables

Submit or retain the following items as directed by your instructor.

| Deliverable | Location |
|---|---|
| Windows Lab 01 evidence notes | `C:\BlueWave\Evidence\Lab01-Environment-Notes.txt` |
| Ubuntu Lab 01 evidence copy | `/home/student/bluewave/evidence/Lab01-Environment-Notes.txt` |
| Screenshot of Windows hostname and user | Skillable submission area |
| Screenshot of Windows IPv4 address | Skillable submission area |
| Screenshot of Ubuntu hostname and user | Skillable submission area |
| Screenshot of Ubuntu IPv4 address | Skillable submission area |
| Screenshot of Windows-to-Ubuntu connectivity test | Skillable submission area |
| Screenshot of Ubuntu-to-Windows connectivity test | Skillable submission area |
| Screenshot of Windows evidence folder | Skillable submission area |
| Screenshot of Ubuntu evidence folder | Skillable submission area |
| Screenshot of Windows lab files folder | Skillable submission area |
| Screenshot of Ubuntu lab files folder | Skillable submission area |
| Screenshot of completed Windows evidence notes file | Skillable submission area |

---

# Instructor Notes

## Expected Knowledge Check Answers

1. WIN11-CLIENT is the Windows 11 endpoint and analyst workstation.
2. UBUNTU-SOC is the Ubuntu SOC server that will later host Elastic, Kibana, and Fleet.
3. The Windows hostname command is `hostname`.
4. The Windows logged-in user command is `whoami`.
5. The Ubuntu hostname command is `hostname`.
6. The Ubuntu logged-in user command is `whoami`.
7. `127.0.0.1` is a loopback address and does not identify the machine on the lab network.
8. The Windows evidence folder is `C:\BlueWave\Evidence`.
9. The Ubuntu evidence folder is `/home/student/bluewave/evidence`.
10. Hostnames help analysts connect events to the correct system during an investigation.

## Expected Evidence Files

Students should create:

```text
C:\BlueWave\Evidence\Lab01-Environment-Notes.txt
```

and:

```text
/home/student/bluewave/evidence/Lab01-Environment-Notes.txt
```

## Grading Guidance

| Criteria | Points |
|---|---:|
| Windows hostname, user, computer name, and IP recorded | 15 |
| Ubuntu hostname, user, home directory, and IP recorded | 15 |
| Connectivity tests attempted and documented | 15 |
| Windows evidence folder created | 10 |
| Ubuntu evidence folder created | 10 |
| Lab file locations checked | 10 |
| Evidence notes completed clearly | 15 |
| Screenshots captured | 10 |
| Total | 100 |

Do not heavily penalise failed ping results if the student used the correct commands and documented the result.

## Elastic Basic License Reminder

Elastic is not used in this lab.

For the course:

- Use self-managed Elastic.
- Use the free Elastic Basic license.
- Do not use Elastic Cloud.
- Do not require paid subscriptions.
- Do not require external internet access.

---

End of Lab 01.
