# Lab 01 - BlueWave Clinic Environment Orientation and Case Study Setup

## Estimated Time

60–90 minutes

---

## Lab Purpose

In this lab, you will become familiar with the two virtual machines used in the BlueWave Clinic cyber operations environment.

You will identify system names, user accounts, IP addresses, network connectivity, lab file locations, and evidence folders.

This lab prepares you for later labs where the Windows 11 endpoint sends logs to Elastic and Kibana on the Ubuntu SOC server.

---

## How to Use Copy and Type Options

This Skillable lab gives you two options for every command, search term, file path, filename, and template that a student may need to enter.

### Copy Option

Use this when you want to copy and paste the command directly into PowerShell, Terminal, File Explorer, or Notepad.

### Type Option

Use this when copy and paste is not available, or when your instructor wants you to practise typing commands manually.

> [!note]
> The Copy and Type options contain the same command or text. Choose one option. Do not run both unless the task tells you to verify a result.

> [!alert]
> Type commands exactly as shown. Small spelling, spacing, quotation mark, or symbol mistakes can cause errors.

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

You are a junior cyber operations analyst.

Before collecting logs, using Kibana, or investigating alerts, you must understand the lab environment.

The lab contains two virtual machines:

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

> [!note]
> This lab does not require internet access.

> [!alert]
> Do not run `BlueWaveActivitySimulator.exe` in this lab. The simulator is introduced in a later lab.

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
| Lab01-Environment-Notes.txt | `C:\BlueWave\Evidence\Lab01-Environment-Notes.txt` |
| Lab01-Environment-Notes.txt | `/home/student/bluewave/evidence/Lab01-Environment-Notes.txt` |

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
| `/home/student/labfiles/scripts` | Helper scripts |
| `/home/student/labfiles/templates` | Evidence and report templates |
| `/home/student/labfiles/capstone` | Capstone files used later |
| `/home/student/bluewave/evidence` | Ubuntu evidence folder |

---

## Screenshots You Should Capture

Capture screenshots as instructed by your trainer or Skillable platform.

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

Use the **WIN11-CLIENT** virtual machine.

## Steps

1. Open the **WIN11-CLIENT** virtual machine.
2. Sign in using the credentials provided by your instructor or Skillable environment.
3. Wait for the Windows desktop to load.
4. Confirm you can see the Windows desktop.

## Expected Result

You should be signed in to the Windows 11 endpoint.

## Screenshot Checkpoint

Capture a screenshot showing that you are signed in to WIN11-CLIENT.

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

### Copy Option

```text
About your PC
```

### Type Option

Type this into the Windows search box:

```text
About your PC
```

3. Open **About your PC**.
4. Confirm the operating system is Windows 11.
5. Close the Settings window.

## Expected Result

You should see that the system is running Windows 11.

## Screenshot Checkpoint

Capture a screenshot if required by your instructor.

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

### Copy Option

```text
PowerShell
```

### Type Option

Type this into the Windows search box:

```text
PowerShell
```

3. Open **Windows PowerShell**.

## Expected Result

A PowerShell window should open.

The prompt may look similar to:

```text
PS C:\Users\student>
```

## Record in Evidence Notes

```text
PowerShell opened: Yes
```

---

# Task 4 - Identify the Windows Hostname

## Where to Work

Use **WIN11-CLIENT** and **Windows PowerShell**.

## Steps

1. Run the hostname command.

### Copy Option

```powershell
hostname
```

### Type Option

Type this command into PowerShell:

```powershell
hostname
```

2. Press **Enter**.
3. Record the hostname.

## Expected Result

The hostname should usually be:

```text
WIN11-CLIENT
```

If your result is different, record the exact hostname shown.

## Screenshot Checkpoint

Capture a screenshot showing the command and result.

## Record in Evidence Notes

```text
Windows hostname:
```

---

# Task 5 - Identify the Windows Logged-In User

## Where to Work

Use **WIN11-CLIENT** and **Windows PowerShell**.

## Steps

1. Run the current user command.

### Copy Option

```powershell
whoami
```

### Type Option

Type this command into PowerShell:

```powershell
whoami
```

2. Press **Enter**.
3. Record the logged-in user.

## Expected Result

The result may look similar to:

```text
win11-client\student
```

## Screenshot Checkpoint

Capture a screenshot showing the command and result.

## Record in Evidence Notes

```text
Windows logged-in user:
```

---

# Task 6 - Confirm the Windows Computer Name Variable

## Where to Work

Use **WIN11-CLIENT** and **Windows PowerShell**.

## Steps

1. Run the computer name variable command.

### Copy Option

```powershell
$env:COMPUTERNAME
```

### Type Option

Type this command into PowerShell:

```powershell
$env:COMPUTERNAME
```

2. Press **Enter**.
3. Compare the result with the hostname from Task 4.
4. Record the value.

## Expected Result

The result should usually be:

```text
WIN11-CLIENT
```

## Screenshot Checkpoint

Capture a screenshot showing the command and result.

## Record in Evidence Notes

```text
Windows COMPUTERNAME value:
```

---

# Task 7 - Identify the Windows IPv4 Address

## Where to Work

Use **WIN11-CLIENT** and **Windows PowerShell**.

## Steps

1. Run the Windows network configuration command.

### Copy Option

```powershell
ipconfig
```

### Type Option

Type this command into PowerShell:

```powershell
ipconfig
```

2. Press **Enter**.
3. Look for the active network adapter.
4. Find the line named `IPv4 Address`.
5. Record the IPv4 address.
6. Run the cleaner IPv4 display command.

### Copy Option

```powershell
Get-NetIPAddress -AddressFamily IPv4 | Where-Object {$_.IPAddress -notlike "127.*"} | Select-Object InterfaceAlias, IPAddress, PrefixLength
```

### Type Option

Type this command into PowerShell:

```powershell
Get-NetIPAddress -AddressFamily IPv4 | Where-Object {$_.IPAddress -notlike "127.*"} | Select-Object InterfaceAlias, IPAddress, PrefixLength
```

7. Press **Enter**.
8. Record the active network adapter name.

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

## Screenshot Checkpoint

Capture a screenshot showing the Windows IPv4 address.

## Record in Evidence Notes

```text
Windows IPv4 address:
Windows network adapter:
```

---

# Task 8 - Sign In to UBUNTU-SOC

## Where to Work

Use the **UBUNTU-SOC** virtual machine.

## Steps

1. Open the **UBUNTU-SOC** virtual machine.
2. Sign in using the credentials provided by your instructor or Skillable environment.
3. Wait for the Ubuntu desktop or terminal environment to load.

## Expected Result

You should be signed in to the Ubuntu SOC server.

## Record in Evidence Notes

```text
Ubuntu VM access confirmed: Yes
```

---

# Task 9 - Open Ubuntu Terminal

## Where to Work

Use **UBUNTU-SOC**.

## Steps

1. Open **Terminal**.
2. Wait for the Terminal prompt to appear.

## Expected Result

A Terminal window should open.

The prompt may look similar to:

```text
student@UBUNTU-SOC:~$
```

## Record in Evidence Notes

```text
Ubuntu Terminal opened: Yes
```

---

# Task 10 - Identify the Ubuntu Hostname

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Run the Ubuntu hostname command.

### Copy Option

```bash
hostname
```

### Type Option

Type this command into Terminal:

```bash
hostname
```

2. Press **Enter**.
3. Record the hostname.

## Expected Result

The hostname should usually be:

```text
UBUNTU-SOC
```

## Screenshot Checkpoint

Capture a screenshot showing the command and result.

## Record in Evidence Notes

```text
Ubuntu hostname:
```

---

# Task 11 - Identify the Ubuntu Logged-In User

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Run the Ubuntu current user command.

### Copy Option

```bash
whoami
```

### Type Option

Type this command into Terminal:

```bash
whoami
```

2. Press **Enter**.
3. Record the logged-in user.

## Expected Result

The result should usually be:

```text
student
```

## Screenshot Checkpoint

Capture a screenshot showing the command and result.

## Record in Evidence Notes

```text
Ubuntu logged-in user:
```

---

# Task 12 - Identify the Ubuntu Home Directory

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Run the print working directory command.

### Copy Option

```bash
pwd
```

### Type Option

Type this command into Terminal:

```bash
pwd
```

2. Press **Enter**.
3. Record the current directory.

## Expected Result

The result should usually be:

```text
/home/student
```

## Record in Evidence Notes

```text
Ubuntu home directory:
```

---

# Task 13 - Identify the Ubuntu IPv4 Address

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Run the quick Ubuntu IP address command.

### Copy Option

```bash
hostname -I
```

### Type Option

Type this command into Terminal:

```bash
hostname -I
```

2. Press **Enter**.
3. Record the IP address shown.
4. Run the detailed network command.

### Copy Option

```bash
ip addr show
```

### Type Option

Type this command into Terminal:

```bash
ip addr show
```

5. Press **Enter**.
6. Look for the active network interface.
7. Find the IP address after the word `inet`.
8. Record the Ubuntu IPv4 address and network interface name.

## Expected Result

You should see an IPv4 address.

Example:

```text
inet 10.1.1.10/24
```

> [!alert]
> Do not record `127.0.0.1`. That is the loopback address.

## Screenshot Checkpoint

Capture a screenshot showing the Ubuntu IPv4 address.

## Record in Evidence Notes

```text
Ubuntu IPv4 address:
Ubuntu network interface:
```

---

# Task 14 - Test Connectivity from Windows to Ubuntu

## Where to Work

Use **WIN11-CLIENT** and **Windows PowerShell**.

## Steps

1. Return to **WIN11-CLIENT**.
2. Open **Windows PowerShell**.
3. Find the Ubuntu IPv4 address you recorded in Task 13.
4. Run the ping command.
5. Replace `<UBUNTU-SOC-IP>` with your Ubuntu IP address.

### Copy Option

```powershell
ping <UBUNTU-SOC-IP>
```

### Type Option

Type this command into PowerShell, replacing `<UBUNTU-SOC-IP>` with your Ubuntu IP address:

```powershell
ping <UBUNTU-SOC-IP>
```

Example:

```powershell
ping 10.1.1.10
```

6. Press **Enter**.
7. Record the result.
8. Run the PowerShell connectivity command.
9. Replace `<UBUNTU-SOC-IP>` with your Ubuntu IP address.

### Copy Option

```powershell
Test-NetConnection <UBUNTU-SOC-IP>
```

### Type Option

Type this command into PowerShell, replacing `<UBUNTU-SOC-IP>` with your Ubuntu IP address:

```powershell
Test-NetConnection <UBUNTU-SOC-IP>
```

Example:

```powershell
Test-NetConnection 10.1.1.10
```

10. Press **Enter**.
11. Record the result.

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

## Screenshot Checkpoint

Capture a screenshot showing the Windows-to-Ubuntu connectivity test.

## Record in Evidence Notes

```text
Windows-to-Ubuntu command:
Windows-to-Ubuntu result:
```

---

# Task 15 - Test Connectivity from Ubuntu to Windows

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Return to **UBUNTU-SOC**.
2. Open **Terminal**.
3. Find the Windows IPv4 address you recorded in Task 7.
4. Run the ping command.
5. Replace `<WIN11-CLIENT-IP>` with your Windows IP address.

### Copy Option

```bash
ping -c 4 <WIN11-CLIENT-IP>
```

### Type Option

Type this command into Terminal, replacing `<WIN11-CLIENT-IP>` with your Windows IP address:

```bash
ping -c 4 <WIN11-CLIENT-IP>
```

Example:

```bash
ping -c 4 10.1.1.20
```

6. Press **Enter**.
7. Record the result.

## Expected Result

A successful result may show:

```text
4 packets transmitted, 4 received, 0% packet loss
```

> [!note]
> A failed ping does not always mean the lab is broken. The Windows firewall may block ping.

> [!alert]
> Do not change firewall settings in this lab.

## Screenshot Checkpoint

Capture a screenshot showing the Ubuntu-to-Windows connectivity test.

## Record in Evidence Notes

```text
Ubuntu-to-Windows command:
Ubuntu-to-Windows result:
```

---

# Task 16 - Create the Windows Evidence Folder

## Where to Work

Use **WIN11-CLIENT** and **Windows PowerShell**.

## Steps

1. Return to **WIN11-CLIENT**.
2. Open **Windows PowerShell**.
3. Run the folder creation command.

### Copy Option

```powershell
New-Item -Path "C:\BlueWave\Evidence" -ItemType Directory -Force
```

### Type Option

Type this command into PowerShell:

```powershell
New-Item -Path "C:\BlueWave\Evidence" -ItemType Directory -Force
```

4. Press **Enter**.
5. Confirm the folder exists.

### Copy Option

```powershell
Test-Path "C:\BlueWave\Evidence"
```

### Type Option

Type this command into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence"
```

6. Press **Enter**.
7. Open **File Explorer**.
8. Browse to the evidence folder.

### Copy Option

```text
C:\BlueWave\Evidence
```

### Type Option

Type this path into the File Explorer address bar:

```text
C:\BlueWave\Evidence
```

## Expected Result

The `Test-Path` command should return:

```text
True
```

## Screenshot Checkpoint

Capture a screenshot showing the Windows evidence folder exists.

## Record in Evidence Notes

```text
Windows evidence folder: C:\BlueWave\Evidence
```

---

# Task 17 - Create the Ubuntu Evidence Folder

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Return to **UBUNTU-SOC**.
2. Open **Terminal**.
3. Run the folder creation command.

### Copy Option

```bash
mkdir -p /home/student/bluewave/evidence
```

### Type Option

Type this command into Terminal:

```bash
mkdir -p /home/student/bluewave/evidence
```

4. Press **Enter**.
5. Confirm the folder exists.

### Copy Option

```bash
ls -ld /home/student/bluewave/evidence
```

### Type Option

Type this command into Terminal:

```bash
ls -ld /home/student/bluewave/evidence
```

6. Press **Enter**.

## Expected Result

You should see folder details similar to:

```text
drwxr-xr-x 2 student student 4096 May 15 10:00 /home/student/bluewave/evidence
```

## Screenshot Checkpoint

Capture a screenshot showing the Ubuntu evidence folder exists.

## Record in Evidence Notes

```text
Ubuntu evidence folder: /home/student/bluewave/evidence
```

---

# Task 18 - Check the Windows Lab Files Folder

## Where to Work

Use **WIN11-CLIENT** and **Windows PowerShell**.

## Steps

1. Return to **WIN11-CLIENT**.
2. Open **Windows PowerShell**.
3. Check whether the main lab files folder exists.

### Copy Option

```powershell
Test-Path "C:\LabFiles"
```

### Type Option

Type this command into PowerShell:

```powershell
Test-Path "C:\LabFiles"
```

4. Press **Enter**.
5. List the folder contents.

### Copy Option

```powershell
Get-ChildItem "C:\LabFiles"
```

### Type Option

Type this command into PowerShell:

```powershell
Get-ChildItem "C:\LabFiles"
```

6. Press **Enter**.
7. Look for these folders:

```text
Tools
Simulators
Logs
Templates
```

## Expected Result

The `Test-Path` command should return:

```text
True
```

The folder listing should include:

```text
Tools
Simulators
Logs
Templates
```

> [!alert]
> Do not run files from the `Simulators` folder in this lab.

## Screenshot Checkpoint

Capture a screenshot showing the `C:\LabFiles` folder listing.

## Record in Evidence Notes

```text
Windows lab files path: C:\LabFiles
Windows lab folders seen:
```

---

# Task 19 - Check the Ubuntu Lab Files Folder

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Return to **UBUNTU-SOC**.
2. Open **Terminal**.
3. List the Ubuntu lab files folder.

### Copy Option

```bash
ls -la /home/student/labfiles
```

### Type Option

Type this command into Terminal:

```bash
ls -la /home/student/labfiles
```

4. Press **Enter**.
5. Look for these folders:

```text
logs
scripts
templates
capstone
```

## Expected Result

The folder listing should include:

```text
logs
scripts
templates
capstone
```

If folders are missing, record what is missing.

## Screenshot Checkpoint

Capture a screenshot showing the `/home/student/labfiles` folder listing.

## Record in Evidence Notes

```text
Ubuntu lab files path: /home/student/labfiles
Ubuntu lab folders seen:
```

---

# Task 20 - Create the Lab 01 Evidence Notes File on Windows

## Where to Work

Use **WIN11-CLIENT** and **Notepad**.

## Steps

1. Return to **WIN11-CLIENT**.
2. Open **Notepad**.
3. Copy or type the evidence template below into Notepad.
4. Fill in the missing information using your lab results.
5. Select **File**.
6. Select **Save As**.
7. Browse to `C:\BlueWave\Evidence`.
8. Save the file as `Lab01-Environment-Notes.txt`.

### Evidence Template - Copy Option

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

### Evidence Template - Type Option

Type the same headings manually into Notepad.

Use these section headings:

```text
1. Windows Endpoint
2. Ubuntu SOC Server
3. Connectivity Tests
4. Lab File Locations
5. Lab Summary
```

### Save Location - Copy Option

```text
C:\BlueWave\Evidence
```

### Save Location - Type Option

Type this folder path into the Save As location:

```text
C:\BlueWave\Evidence
```

### Filename - Copy Option

```text
Lab01-Environment-Notes.txt
```

### Filename - Type Option

Type this filename exactly:

```text
Lab01-Environment-Notes.txt
```

## Expected Result

The file should be saved as:

```text
C:\BlueWave\Evidence\Lab01-Environment-Notes.txt
```

## Screenshot Checkpoint

Capture a screenshot showing the completed evidence notes file.

---

# Task 21 - Confirm the Windows Evidence Notes File Exists

## Where to Work

Use **WIN11-CLIENT** and **Windows PowerShell**.

## Steps

1. Open **Windows PowerShell**.
2. Run the file validation command.

### Copy Option

```powershell
Test-Path "C:\BlueWave\Evidence\Lab01-Environment-Notes.txt"
```

### Type Option

Type this command into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab01-Environment-Notes.txt"
```

3. Press **Enter**.

## Expected Result

PowerShell should return:

```text
True
```

---

# Task 22 - Create the Lab 01 Evidence Copy on Ubuntu

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Return to **UBUNTU-SOC**.
2. Open **Terminal**.
3. Open a new notes file with nano.

### Copy Option

```bash
nano /home/student/bluewave/evidence/Lab01-Environment-Notes.txt
```

### Type Option

Type this command into Terminal:

```bash
nano /home/student/bluewave/evidence/Lab01-Environment-Notes.txt
```

4. Press **Enter**.
5. Copy or type the Ubuntu evidence copy template below into nano.
6. Fill in the missing information.
7. Press **Ctrl + O**.
8. Press **Enter**.
9. Press **Ctrl + X**.

### Ubuntu Evidence Copy Template - Copy Option

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

### Ubuntu Evidence Copy Template - Type Option

Type the same headings manually into nano:

```text
Windows hostname:
Windows IPv4 address:
Ubuntu hostname:
Ubuntu IPv4 address:
Windows evidence folder:
Ubuntu evidence folder:
Connectivity summary:
```

## Expected Result

The Ubuntu evidence copy should be saved at:

```text
/home/student/bluewave/evidence/Lab01-Environment-Notes.txt
```

---

# Task 23 - Confirm the Ubuntu Evidence Notes File Exists

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Run the file listing command.

### Copy Option

```bash
ls -l /home/student/bluewave/evidence/Lab01-Environment-Notes.txt
```

### Type Option

Type this command into Terminal:

```bash
ls -l /home/student/bluewave/evidence/Lab01-Environment-Notes.txt
```

2. Press **Enter**.
3. Display the file contents.

### Copy Option

```bash
cat /home/student/bluewave/evidence/Lab01-Environment-Notes.txt
```

### Type Option

Type this command into Terminal:

```bash
cat /home/student/bluewave/evidence/Lab01-Environment-Notes.txt
```

4. Press **Enter**.

## Expected Result

You should see the file listed and the contents displayed.

## Screenshot Checkpoint

Capture a screenshot showing the Ubuntu evidence notes file contents.

---

# Task 24 - Final Validation

## Where to Work

Use both machines.

## Steps on WIN11-CLIENT

1. Open **Windows PowerShell**.
2. Validate the Windows evidence file.

### Copy Option

```powershell
Test-Path "C:\BlueWave\Evidence\Lab01-Environment-Notes.txt"
```

### Type Option

Type this command into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab01-Environment-Notes.txt"
```

3. Confirm the result is:

```text
True
```

## Steps on UBUNTU-SOC

1. Open **Terminal**.
2. Validate the Ubuntu evidence file.

### Copy Option

```bash
test -f /home/student/bluewave/evidence/Lab01-Environment-Notes.txt && echo "Ubuntu evidence file exists"
```

### Type Option

Type this command into Terminal:

```bash
test -f /home/student/bluewave/evidence/Lab01-Environment-Notes.txt && echo "Ubuntu evidence file exists"
```

3. Confirm the result is:

```text
Ubuntu evidence file exists
```

## Expected Result

Both evidence files should exist.

Both evidence folders should exist.

Your notes should include hostnames, usernames, IP addresses, connectivity results, and lab file folder checks.

## Screenshot Checkpoint

Capture a final screenshot showing the Windows evidence folder.

Capture a final screenshot showing the Ubuntu evidence validation result.

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

## Problem: PowerShell does not open

Open the **Windows Start** menu.

Search for **PowerShell**.

### Copy

```text
PowerShell
```

### Type

Type this into the Windows search box:

```text
PowerShell
```

Open **Windows PowerShell**.

If Windows Terminal opens instead, that is acceptable if it gives you a PowerShell prompt.

---

## Problem: Ubuntu Terminal does not open

Open the Ubuntu application search.

Search for **Terminal**.

### Copy

```text
Terminal
```

### Type

Type this into the Ubuntu application search:

```text
Terminal
```

Open **Terminal**.

---

## Problem: The Windows hostname is not WIN11-CLIENT

Record the exact hostname shown.

Tell your instructor if the hostname is very different from the lab instructions.

To check again, run this command on **WIN11-CLIENT** in **PowerShell**.

### Copy

```powershell
hostname
```

### Type

Type this into PowerShell:

```powershell
hostname
```

---

## Problem: The Ubuntu hostname is not UBUNTU-SOC

Record the exact hostname shown.

Tell your instructor if the hostname is very different from the lab instructions.

To check again, run this command on **UBUNTU-SOC** in **Terminal**.

### Copy

```bash
hostname
```

### Type

Type this into Terminal:

```bash
hostname
```

---

## Problem: `ipconfig` shows many network adapters

Look for the active Ethernet adapter.

Ignore adapters that show:

### Copy

```text
Media disconnected
```

### Type

Look for and ignore this text:

```text
Media disconnected
```

Do not use the loopback address.

### Copy

```text
127.0.0.1
```

### Type

Do not record this address:

```text
127.0.0.1
```

Run the cleaner Windows IPv4 command again if needed.

### Copy

```powershell
Get-NetIPAddress -AddressFamily IPv4 | Where-Object {$_.IPAddress -notlike "127.*"} | Select-Object InterfaceAlias, IPAddress, PrefixLength
```

### Type

Type this into PowerShell exactly as shown:

```powershell
Get-NetIPAddress -AddressFamily IPv4 | Where-Object {$_.IPAddress -notlike "127.*"} | Select-Object InterfaceAlias, IPAddress, PrefixLength
```

---

## Problem: `hostname -I` shows more than one IP address

Record the first non-loopback IPv4 address.

You can also run this command on **UBUNTU-SOC**.

### Copy

```bash
ip addr show
```

### Type

Type this into Terminal:

```bash
ip addr show
```

Look for an address after this word:

### Copy

```text
inet
```

### Type

Look for this word in the output:

```text
inet
```

Do not use the loopback address.

### Copy

```text
127.0.0.1
```

### Type

Do not record this address:

```text
127.0.0.1
```

---

## Problem: Ping from Windows to Ubuntu fails

Check that you typed the correct Ubuntu IP address.

Try the command again on **WIN11-CLIENT** in **PowerShell**.

### Copy

```powershell
ping <UBUNTU-SOC-IP>
```

### Type

Type this into PowerShell, replacing `<UBUNTU-SOC-IP>` with your Ubuntu IP address:

```powershell
ping <UBUNTU-SOC-IP>
```

Example:

### Copy

```powershell
ping 10.1.1.10
```

### Type

Type this example only if your Ubuntu IP address is `10.1.1.10`:

```powershell
ping 10.1.1.10
```

If it still fails, ICMP may be blocked.

Record the failed result in your evidence file.

Do not change firewall settings in this lab.

---

## Problem: Ping from Ubuntu to Windows fails

Check that you typed the correct Windows IP address.

Try the command again on **UBUNTU-SOC** in **Terminal**.

### Copy

```bash
ping -c 4 <WIN11-CLIENT-IP>
```

### Type

Type this into Terminal, replacing `<WIN11-CLIENT-IP>` with your Windows IP address:

```bash
ping -c 4 <WIN11-CLIENT-IP>
```

Example:

### Copy

```bash
ping -c 4 10.1.1.20
```

### Type

Type this example only if your Windows IP address is `10.1.1.20`:

```bash
ping -c 4 10.1.1.20
```

If it still fails, the Windows firewall may be blocking ping.

Record the failed result in your evidence file.

Do not change firewall settings in this lab.

---

## Problem: The Windows evidence folder does not exist

Run this command on **WIN11-CLIENT** in **PowerShell**.

### Copy

```powershell
New-Item -Path "C:\BlueWave\Evidence" -ItemType Directory -Force
```

### Type

Type this into PowerShell:

```powershell
New-Item -Path "C:\BlueWave\Evidence" -ItemType Directory -Force
```

Then confirm the folder exists.

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

## Problem: The Ubuntu evidence folder does not exist

Run this command on **UBUNTU-SOC** in **Terminal**.

### Copy

```bash
mkdir -p /home/student/bluewave/evidence
```

### Type

Type this into Terminal:

```bash
mkdir -p /home/student/bluewave/evidence
```

Then confirm the folder exists.

### Copy

```bash
ls -ld /home/student/bluewave/evidence
```

### Type

Type this into Terminal:

```bash
ls -ld /home/student/bluewave/evidence
```

---

## Problem: The Windows evidence file is missing

Check that you saved the file with this exact filename.

### Copy

```text
Lab01-Environment-Notes.txt
```

### Type

Type this filename exactly:

```text
Lab01-Environment-Notes.txt
```

Check that you saved it in this exact folder.

### Copy

```text
C:\BlueWave\Evidence
```

### Type

Type this path exactly:

```text
C:\BlueWave\Evidence
```

Confirm with this command on **WIN11-CLIENT** in **PowerShell**.

### Copy

```powershell
Test-Path "C:\BlueWave\Evidence\Lab01-Environment-Notes.txt"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\BlueWave\Evidence\Lab01-Environment-Notes.txt"
```

Expected result:

```text
True
```

---

## Problem: The Ubuntu evidence file is missing

Create it again on **UBUNTU-SOC** in **Terminal**.

### Copy

```bash
nano /home/student/bluewave/evidence/Lab01-Environment-Notes.txt
```

### Type

Type this into Terminal:

```bash
nano /home/student/bluewave/evidence/Lab01-Environment-Notes.txt
```

Then confirm it exists.

### Copy

```bash
ls -l /home/student/bluewave/evidence/Lab01-Environment-Notes.txt
```

### Type

Type this into Terminal:

```bash
ls -l /home/student/bluewave/evidence/Lab01-Environment-Notes.txt
```

---

## Problem: `C:\LabFiles` is missing

Check the path carefully on **WIN11-CLIENT**.

### Copy

```text
C:\LabFiles
```

### Type

Type this path exactly:

```text
C:\LabFiles
```

You can also confirm with PowerShell.

### Copy

```powershell
Test-Path "C:\LabFiles"
```

### Type

Type this into PowerShell:

```powershell
Test-Path "C:\LabFiles"
```

If it is still missing, record the issue and notify your instructor.

Do not download files from the internet.

---

## Problem: `/home/student/labfiles` is missing

Check the path carefully on **UBUNTU-SOC**.

### Copy

```text
/home/student/labfiles
```

### Type

Type this path exactly:

```text
/home/student/labfiles
```

You can also confirm with Terminal.

### Copy

```bash
ls -la /home/student/labfiles
```

### Type

Type this into Terminal:

```bash
ls -la /home/student/labfiles
```

If it is still missing, record the issue and notify your instructor.

Do not download files from the internet.

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
