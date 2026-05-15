Lab 1: BlueWave Clinic Lab 1: Environment Orientation and Case Study Setup
Estimated Duration
```text
60–90 minutes
```
---
Difficulty Level
Beginner
---
Lab Scenario
BlueWave Clinic is a small healthcare organisation that is starting to build a cyber operations capability.
You are a junior cyber operations analyst. Your first job is to understand the lab environment before any monitoring or investigation work begins.
In this lab, you will explore two virtual machines:
A Windows 11 endpoint named WIN11-CLIENT.
An Ubuntu SOC server named UBUNTU-SOC.
You will identify hostnames, IP addresses, basic connectivity, and evidence storage locations. These details will be reused in later labs when Windows logs are sent to Elastic and analysed in Kibana.
---
Learning Objectives
By the end of this lab, you will be able to:
Identify the purpose of each virtual machine in the BlueWave Clinic lab.
Open PowerShell on WIN11-CLIENT.
Open Terminal on UBUNTU-SOC.
Collect hostname, username, and IP address information.
Test basic network connectivity between the two virtual machines.
Create evidence folders for future investigation work.
Document environment information in a simple evidence file.
---
Required Machines
Machine	Purpose
WIN11-CLIENT	Windows 11 endpoint and analyst workstation
UBUNTU-SOC	Ubuntu SOC server that will later run Elastic and Kibana
---
Required Files
File	Location	Purpose
None required	N/A	This lab focuses on environment orientation and evidence setup
---
Before You Begin
Before starting this lab, confirm the following:
You can access the Skillable lab environment.
You can sign in to WIN11-CLIENT.
You can sign in to UBUNTU-SOC.
You can open PowerShell on WIN11-CLIENT.
You can open Terminal on UBUNTU-SOC.
You do not need internet access for this lab.
You do not need to install Elastic in this lab.
You do not need to run the simulator in this lab.
---
Key Terms
Term	Meaning
Virtual machine	A computer running inside the lab environment
Endpoint	A user workstation or device that can generate logs
SOC	Security Operations Centre
Hostname	The name of a computer on a network
IP address	A network address used to communicate with another computer
PowerShell	A Windows command-line tool
Terminal	A Linux command-line tool
Evidence	Information saved during an investigation or lab
Connectivity	The ability of two machines to communicate
Case study	A fictional scenario used to guide the labs
---
Task 1: Review the BlueWave Clinic Lab Environment
Purpose
This task helps you understand the two machines used throughout the course.
You will identify which machine is the Windows endpoint and which machine is the SOC server.
---
Steps
On WIN11-CLIENT
Sign in to the Windows 11 virtual machine.
Look at the desktop.
Confirm that you are working on the Windows machine.
Open the Start menu.
Type:
```text
About your PC
```
Open About your PC.
Review the device information.
Close the Settings window.
---
On UBUNTU-SOC
Sign in to the Ubuntu virtual machine.
Look at the desktop or terminal environment.
Confirm that you are working on the Ubuntu machine.
Open Terminal.
Type the following command:
```bash
hostname
```
Press Enter.
---
Expected Result
On WIN11-CLIENT, you should see that the system is running Windows 11.
On UBUNTU-SOC, the hostname should display something similar to:
```text
UBUNTU-SOC
```
If the hostname is slightly different, record the exact value shown.
---
Screenshot Checkpoint
Capture one screenshot showing that you are signed in to WIN11-CLIENT.
Capture one screenshot showing the Ubuntu Terminal with the hostname command result.
---
Student Note
In your notes, write:
```text
Lab environment contains two virtual machines:
1. WIN11-CLIENT - Windows 11 endpoint and analyst workstation
2. UBUNTU-SOC - Ubuntu SOC server
```
Also write the actual hostname shown on the Ubuntu machine.
---
Task 2: Identify the Windows Hostname and Current User
Purpose
The hostname helps you identify which computer generated logs.
The username helps you identify which account performed an action.
In later labs, you will search for this information in Kibana.
---
Steps
On WIN11-CLIENT
Click the Start button.
Type:
```text
PowerShell
```
Click Windows PowerShell.
In PowerShell, type:
```powershell
hostname
```
Press Enter.
Record the result.
In the same PowerShell window, type:
```powershell
whoami
```
Press Enter.
Record the result.
Type the following command:
```powershell
$env:COMPUTERNAME
```
Press Enter.
Compare the result with the earlier `hostname` result.
---
Expected Result
The hostname should be similar to:
```text
WIN11-CLIENT
```
The current user should be shown in this format:
```text
computername\username
```
Example:
```text
win11-client\student
```
The command:
```powershell
$env:COMPUTERNAME
```
should return the Windows computer name.
---
Screenshot Checkpoint
Capture a screenshot showing the PowerShell results for:
```powershell
hostname
whoami
$env:COMPUTERNAME
```
---
Student Note
Write the following in your evidence notes:
```text
Windows hostname:
Windows logged-in user:
Windows COMPUTERNAME value:
```
Example:
```text
Windows hostname: WIN11-CLIENT
Windows logged-in user: win11-client\student
Windows COMPUTERNAME value: WIN11-CLIENT
```
---
Task 3: Identify the Windows IP Address
Purpose
The Windows IP address is needed to confirm network communication with the Ubuntu SOC server.
In later labs, IP addresses may also appear in Elastic events, network logs, or investigation notes.
---
Steps
On WIN11-CLIENT
Keep Windows PowerShell open.
Type:
```powershell
ipconfig
```
Press Enter.
Look for the active network adapter.
Find the line named:
```text
IPv4 Address
```
Record the IPv4 address.
Run this command for a cleaner IPv4 address view:
```powershell
Get-NetIPAddress -AddressFamily IPv4 | Where-Object {$_.IPAddress -notlike "127.*"} | Select-Object InterfaceAlias, IPAddress, PrefixLength
```
Press Enter.
Identify the IP address for the active lab network adapter.
---
Expected Result
You should see an IPv4 address.
It may look similar to one of these examples:
```text
10.x.x.x
172.16.x.x
192.168.x.x
```
The exact address depends on the Skillable lab network.
Example output:
```text
InterfaceAlias        IPAddress       PrefixLength
--------------        ---------       ------------
Ethernet              10.1.1.20       24
```
---
Screenshot Checkpoint
Capture a screenshot showing the Windows IPv4 address.
The screenshot may show either:
```powershell
ipconfig
```
or:
```powershell
Get-NetIPAddress -AddressFamily IPv4
```
---
Student Note
Write the following in your evidence notes:
```text
Windows IPv4 address:
Windows network adapter name:
```
Example:
```text
Windows IPv4 address: 10.1.1.20
Windows network adapter name: Ethernet
```
---
Task 4: Identify the Ubuntu Hostname and Current User
Purpose
The Ubuntu server will later host Elasticsearch, Kibana, Fleet, and evidence files.
You need to identify the Ubuntu hostname and current user so that later investigation notes are accurate.
---
Steps
On UBUNTU-SOC
Open Terminal.
Type:
```bash
hostname
```
Press Enter.
Record the result.
Type:
```bash
whoami
```
Press Enter.
Record the result.
Type:
```bash
pwd
```
Press Enter.
Record the current working directory.
---
Expected Result
The hostname should be similar to:
```text
UBUNTU-SOC
```
The username should be similar to:
```text
student
```
The working directory may be:
```text
/home/student
```
---
Screenshot Checkpoint
Capture a screenshot showing the Ubuntu Terminal results for:
```bash
hostname
whoami
pwd
```
---
Student Note
Write the following in your evidence notes:
```text
Ubuntu hostname:
Ubuntu logged-in user:
Ubuntu home directory:
```
Example:
```text
Ubuntu hostname: UBUNTU-SOC
Ubuntu logged-in user: student
Ubuntu home directory: /home/student
```
---
Task 5: Identify the Ubuntu IP Address
Purpose
The Ubuntu IP address is important because WIN11-CLIENT will later connect to Kibana on UBUNTU-SOC.
You will also use the Ubuntu IP address when testing network connectivity between the two machines.
---
Steps
On UBUNTU-SOC
In Terminal, type:
```bash
hostname -I
```
Press Enter.
Record the IP address shown.
Type:
```bash
ip addr show
```
Press Enter.
Look for the active network interface.
Find the IP address listed after:
```text
inet
```
Ignore the loopback address:
```text
127.0.0.1
```
Record the Ubuntu IPv4 address.
---
Expected Result
You should see an IPv4 address for UBUNTU-SOC.
It may look similar to:
```text
10.1.1.10
```
or:
```text
192.168.1.10
```
Example output from `hostname -I`:
```text
10.1.1.10
```
Example output from `ip addr show`:
```text
inet 10.1.1.10/24
```
---
Screenshot Checkpoint
Capture a screenshot showing the Ubuntu IP address.
The screenshot may show either:
```bash
hostname -I
```
or:
```bash
ip addr show
```
---
Student Note
Write the following in your evidence notes:
```text
Ubuntu IPv4 address:
Ubuntu network interface name:
```
Example:
```text
Ubuntu IPv4 address: 10.1.1.10
Ubuntu network interface name: ens33
```
---
Task 6: Confirm Connectivity from Windows to Ubuntu
Purpose
WIN11-CLIENT must be able to reach UBUNTU-SOC.
In later labs, Windows will open Kibana in a browser and send logs to Elastic.
This task confirms that basic communication is working.
---
Steps
On WIN11-CLIENT
Go back to Windows PowerShell.
Identify the Ubuntu IP address that you recorded earlier.
Type the following command.
Replace `<UBUNTU-SOC-IP>` with the actual Ubuntu IP address.
```powershell
ping <UBUNTU-SOC-IP>
```
Example:
```powershell
ping 10.1.1.10
```
Press Enter.
Review the result.
Run this additional connectivity command:
```powershell
Test-NetConnection <UBUNTU-SOC-IP>
```
Example:
```powershell
Test-NetConnection 10.1.1.10
```
Press Enter.
---
Expected Result
A successful ping should show replies similar to:
```text
Reply from 10.1.1.10: bytes=32 time<1ms TTL=64
```
A successful `Test-NetConnection` result may show:
```text
PingSucceeded : True
```
If ping is blocked, you may see:
```text
Request timed out.
```
If ping fails, do not panic. Some lab environments block ICMP traffic.
Record the result exactly as shown.
---
Screenshot Checkpoint
Capture a screenshot showing the Windows connectivity test to UBUNTU-SOC.
The screenshot should show either:
```powershell
ping <UBUNTU-SOC-IP>
```
or:
```powershell
Test-NetConnection <UBUNTU-SOC-IP>
```
---
Student Note
Write the following in your evidence notes:
```text
Windows-to-Ubuntu connectivity test:
Command used:
Result:
```
Example:
```text
Windows-to-Ubuntu connectivity test:
Command used: ping 10.1.1.10
Result: Successful replies received
```
Or:
```text
Windows-to-Ubuntu connectivity test:
Command used: ping 10.1.1.10
Result: Request timed out. ICMP may be blocked.
```
---
Task 7: Confirm Connectivity from Ubuntu to Windows
Purpose
Testing connectivity in both directions helps confirm the lab network is working.
This is useful when troubleshooting log forwarding, browser access, and future Elastic Agent communication.
---
Steps
On UBUNTU-SOC
Go back to Terminal.
Identify the Windows IP address that you recorded earlier.
Type the following command.
Replace `<WIN11-CLIENT-IP>` with the actual Windows IP address.
```bash
ping -c 4 <WIN11-CLIENT-IP>
```
Example:
```bash
ping -c 4 10.1.1.20
```
Press Enter.
Review the result.
---
Expected Result
A successful ping should show replies similar to:
```text
64 bytes from 10.1.1.20: icmp_seq=1 ttl=128 time=1.10 ms
64 bytes from 10.1.1.20: icmp_seq=2 ttl=128 time=1.02 ms
64 bytes from 10.1.1.20: icmp_seq=3 ttl=128 time=0.98 ms
64 bytes from 10.1.1.20: icmp_seq=4 ttl=128 time=1.05 ms
```
A summary may show:
```text
4 packets transmitted, 4 received, 0% packet loss
```
If ping is blocked by the Windows firewall, you may see packet loss.
Record the result exactly as shown.
---
Screenshot Checkpoint
Capture a screenshot showing the Ubuntu connectivity test to WIN11-CLIENT.
---
Student Note
Write the following in your evidence notes:
```text
Ubuntu-to-Windows connectivity test:
Command used:
Result:
```
Example:
```text
Ubuntu-to-Windows connectivity test:
Command used: ping -c 4 10.1.1.20
Result: 4 packets transmitted, 4 received, 0% packet loss
```
---
Task 8: Create the Windows Evidence Folder
Purpose
Every lab requires evidence.
The Windows evidence folder will store investigation notes, timelines, screenshots, and reports.
You will use this folder throughout the course.
---
Steps
On WIN11-CLIENT
Open Windows PowerShell.
Type the following command:
```powershell
New-Item -Path "C:\BlueWave\Evidence" -ItemType Directory -Force
```
Press Enter.
Confirm the folder exists by typing:
```powershell
Test-Path "C:\BlueWave\Evidence"
```
Press Enter.
Open File Explorer.
Go to:
```text
C:\BlueWave\Evidence
```
Confirm the folder opens.
---
Expected Result
PowerShell should create the folder or confirm it already exists.
The command:
```powershell
Test-Path "C:\BlueWave\Evidence"
```
should return:
```text
True
```
File Explorer should show the folder:
```text
C:\BlueWave\Evidence
```
---
Screenshot Checkpoint
Capture a screenshot showing either:
PowerShell with `Test-Path` returning `True`, or
File Explorer open at `C:\BlueWave\Evidence`.
---
Student Note
Write the following in your evidence notes:
```text
Windows evidence folder created:
C:\BlueWave\Evidence
```
---
Task 9: Create the Ubuntu Evidence Folder
Purpose
Some evidence may also be stored on UBUNTU-SOC.
This folder will be used when working with Elastic, Kibana, templates, and final reports.
---
Steps
On UBUNTU-SOC
Open Terminal.
Type the following command:
```bash
mkdir -p /home/student/bluewave/evidence
```
Press Enter.
Confirm the folder exists by typing:
```bash
ls -ld /home/student/bluewave/evidence
```
Press Enter.
Type:
```bash
pwd
```
Press Enter.
---
Expected Result
You should see output similar to:
```text
drwxr-xr-x 2 student student 4096 May 15 10:00 /home/student/bluewave/evidence
```
The folder should exist at:
```text
/home/student/bluewave/evidence
```
---
Screenshot Checkpoint
Capture a screenshot showing the Ubuntu evidence folder exists.
---
Student Note
Write the following in your evidence notes:
```text
Ubuntu evidence folder created:
/home/student/bluewave/evidence
```
---
Task 10: Review the Preloaded Lab File Locations
Purpose
The course assumes that required lab tools, templates, installers, logs, and simulator files are already preloaded.
In this task, you will confirm the main lab folders exist.
You will not run any simulator or install any tool in this lab.
---
Steps
On WIN11-CLIENT
Open Windows PowerShell.
Type:
```powershell
Test-Path "C:\LabFiles"
```
Press Enter.
Type:
```powershell
Get-ChildItem "C:\LabFiles"
```
Press Enter.
Look for folders such as:
```text
Tools
Simulators
Logs
Templates
```
---
On UBUNTU-SOC
Open Terminal.
Type:
```bash
ls -la /home/student/labfiles
```
Press Enter.
Look for folders such as:
```text
logs
scripts
templates
capstone
```
---
Expected Result
On WIN11-CLIENT, you should see:
```text
C:\LabFiles
C:\LabFiles\Tools
C:\LabFiles\Simulators
C:\LabFiles\Logs
C:\LabFiles\Templates
```
On UBUNTU-SOC, you should see:
```text
/home/student/labfiles
/home/student/labfiles/logs
/home/student/labfiles/scripts
/home/student/labfiles/templates
/home/student/labfiles/capstone
```
If one or more folders are missing, record what is missing.
Do not create missing course content yourself unless instructed by your instructor.
---
Screenshot Checkpoint
Capture one screenshot showing the Windows `C:\LabFiles` folder listing.
Capture one screenshot showing the Ubuntu `/home/student/labfiles` folder listing.
---
Student Note
Write the following in your evidence notes:
```text
Windows lab files folder checked:
C:\LabFiles

Ubuntu lab files folder checked:
/home/student/labfiles

Missing folders, if any:
```
---
Task 11: Create the Lab 1 Evidence File on Windows
Purpose
Cyber operations work must be documented.
In this task, you will create your first evidence file for BlueWave Clinic.
This file records the environment information collected during the lab.
---
Steps
On WIN11-CLIENT
Open Notepad.
Copy the following template into Notepad:
```text
BlueWave Clinic Cyber Operations with Elastic
Lab 1: Environment Orientation and Case Study Setup

Student Name:
Date:
Lab Environment:

1. Windows Endpoint
Hostname:
Logged-in user:
IPv4 address:
Network adapter:
Evidence folder:

2. Ubuntu SOC Server
Hostname:
Logged-in user:
IPv4 address:
Network interface:
Evidence folder:

3. Connectivity Tests
Windows-to-Ubuntu command:
Windows-to-Ubuntu result:

Ubuntu-to-Windows command:
Ubuntu-to-Windows result:

4. Lab File Locations Checked
Windows lab files path:
Windows lab folders seen:

Ubuntu lab files path:
Ubuntu lab folders seen:

5. Notes
Write 3 to 5 sentences explaining what you learned about the BlueWave Clinic lab environment.
```
Fill in the missing information.
Click File.
Click Save As.
Browse to:
```text
C:\BlueWave\Evidence
```
Save the file as:
```text
Lab1-Environment-Notes.txt
```
In PowerShell, confirm the file exists:
```powershell
Test-Path "C:\BlueWave\Evidence\Lab1-Environment-Notes.txt"
```
Press Enter.
---
Expected Result
The file should exist at:
```text
C:\BlueWave\Evidence\Lab1-Environment-Notes.txt
```
PowerShell should return:
```text
True
```
---
Screenshot Checkpoint
Capture a screenshot showing:
The completed Notepad evidence file, or
PowerShell showing the evidence file exists.
---
Student Note
Your evidence file must include:
```text
Windows hostname
Windows user
Windows IP address
Ubuntu hostname
Ubuntu user
Ubuntu IP address
Connectivity test results
Evidence folder locations
Lab file folder check results
3 to 5 sentence summary
```
---
Task 12: Create a Copy of the Lab 1 Evidence File on Ubuntu
Purpose
Keeping a copy of evidence on UBUNTU-SOC helps prepare for later labs where the Ubuntu system acts as the SOC server and evidence repository.
This task creates a simple Ubuntu-side notes file.
---
Steps
On UBUNTU-SOC
Open Terminal.
Create the evidence file by typing:
```bash
nano /home/student/bluewave/evidence/Lab1-Environment-Notes.txt
```
Press Enter.
Type a short summary using this structure:
```text
BlueWave Clinic Cyber Operations with Elastic
Lab 1 Ubuntu Evidence Copy

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
Fill in the information.
Press Ctrl + O to save.
Press Enter to confirm the filename.
Press Ctrl + X to exit nano.
Confirm the file exists:
```bash
ls -l /home/student/bluewave/evidence/Lab1-Environment-Notes.txt
```
Display the file contents:
```bash
cat /home/student/bluewave/evidence/Lab1-Environment-Notes.txt
```
---
Expected Result
The file should exist at:
```text
/home/student/bluewave/evidence/Lab1-Environment-Notes.txt
```
The `cat` command should display your notes.
---
Screenshot Checkpoint
Capture a screenshot showing the Ubuntu evidence file contents.
---
Student Note
Write in your Windows evidence file that an Ubuntu copy was also created:
```text
Ubuntu evidence copy created:
/home/student/bluewave/evidence/Lab1-Environment-Notes.txt
```
---
Task 13: Final Lab 1 Review
Purpose
This task confirms that you have completed the setup work needed for future labs.
You will review your evidence and make sure the visible results are clear.
---
Steps
On WIN11-CLIENT
Open File Explorer.
Go to:
```text
C:\BlueWave\Evidence
```
Confirm this file exists:
```text
Lab1-Environment-Notes.txt
```
Open the file.
Review the contents.
Confirm that no required section is blank.
---
On UBUNTU-SOC
Open Terminal.
Type:
```bash
ls -l /home/student/bluewave/evidence
```
Press Enter.
Confirm this file exists:
```text
Lab1-Environment-Notes.txt
```
---
Expected Result
You should have a completed Lab 1 evidence file on Windows.
You should also have a Lab 1 evidence copy on Ubuntu.
Your evidence should include hostnames, IP addresses, connectivity results, and evidence folder paths.
---
Screenshot Checkpoint
Capture a final screenshot showing the completed Windows evidence folder.
Capture a final screenshot showing the completed Ubuntu evidence folder.
---
Student Note
At the bottom of your Windows evidence file, add:
```text
Lab 1 completion statement:
I confirmed the BlueWave Clinic lab environment, identified both virtual machines, recorded network information, tested connectivity, and created evidence folders for future cyber operations labs.
```
---
Validation Checklist
Before finishing the lab, confirm each item is complete.
[ ] I signed in to WIN11-CLIENT.
[ ] I signed in to UBUNTU-SOC.
[ ] I opened PowerShell on WIN11-CLIENT.
[ ] I opened Terminal on UBUNTU-SOC.
[ ] I identified the Windows hostname.
[ ] I identified the Windows logged-in user.
[ ] I identified the Windows IPv4 address.
[ ] I identified the Ubuntu hostname.
[ ] I identified the Ubuntu logged-in user.
[ ] I identified the Ubuntu IPv4 address.
[ ] I tested connectivity from Windows to Ubuntu.
[ ] I tested connectivity from Ubuntu to Windows.
[ ] I created the Windows evidence folder.
[ ] I created the Ubuntu evidence folder.
[ ] I checked the Windows lab files folder.
[ ] I checked the Ubuntu lab files folder.
[ ] I created `Lab1-Environment-Notes.txt` on Windows.
[ ] I created a Lab 1 evidence copy on Ubuntu.
[ ] I captured all required screenshots.
[ ] I reviewed my evidence file for completeness.
---
Troubleshooting
Problem: I cannot find PowerShell on Windows
Possible fix:
Click Start, type:
```text
PowerShell
```
Then open Windows PowerShell.
If Windows Terminal opens instead, that is also acceptable if it gives you a PowerShell prompt.
---
Problem: I typed a command and received an error
Possible fix:
Check the spelling of the command.
For example, this is correct:
```powershell
hostname
```
This is incorrect:
```powershell
host name
```
---
Problem: The Windows hostname is not WIN11-CLIENT
Possible fix:
Record the actual hostname shown in PowerShell.
Tell your instructor if the hostname is very different from the lab instructions.
---
Problem: The Ubuntu hostname is not UBUNTU-SOC
Possible fix:
Record the actual hostname shown in Terminal.
Tell your instructor if the hostname is very different from the lab instructions.
---
Problem: `ipconfig` shows many adapters
Possible fix:
Look for the active Ethernet adapter.
Ignore adapters that show:
```text
Media disconnected
```
Also ignore loopback addresses such as:
```text
127.0.0.1
```
---
Problem: `hostname -I` shows more than one IP address
Possible fix:
Record the first non-loopback IPv4 address.
If you are unsure, also check:
```bash
ip addr show
```
Ask your instructor which interface is used for the lab network.
---
Problem: Ping from Windows to Ubuntu fails
Possible fix:
Check that you typed the correct Ubuntu IP address.
Try the command again:
```powershell
ping <UBUNTU-SOC-IP>
```
If it still fails, ICMP may be blocked.
Record the failed result in your evidence file.
---
Problem: Ping from Ubuntu to Windows fails
Possible fix:
Check that you typed the correct Windows IP address.
Try:
```bash
ping -c 4 <WIN11-CLIENT-IP>
```
If it still fails, the Windows firewall may be blocking ping.
Record the failed result in your evidence file.
---
Problem: I cannot create `C:\BlueWave\Evidence`
Possible fix:
Make sure you typed the command exactly:
```powershell
New-Item -Path "C:\BlueWave\Evidence" -ItemType Directory -Force
```
If the folder already exists, that is fine.
---
Problem: I cannot create `/home/student/bluewave/evidence`
Possible fix:
Make sure you typed the command exactly:
```bash
mkdir -p /home/student/bluewave/evidence
```
Make sure you are signed in as the `student` user.
---
Problem: I cannot save the Notepad file
Possible fix:
Make sure the folder exists first:
```text
C:\BlueWave\Evidence
```
Then save the file as:
```text
Lab1-Environment-Notes.txt
```
---
Problem: The lab files folder is missing
Possible fix:
Check the path carefully.
On Windows, check:
```text
C:\LabFiles
```
On Ubuntu, check:
```text
/home/student/labfiles
```
If the folders are missing, record the issue and notify your instructor.
---
Knowledge Check
Answer the following questions in your own words.
Do not include answers from another student.
Question 1: Multiple Choice
Which machine is the Windows endpoint in the BlueWave Clinic lab?
A. UBUNTU-SOC  
B. WIN11-CLIENT  
C. Elastic Cloud  
D. Internet Gateway
---
Question 2: Multiple Choice
Which machine will later host Elastic and Kibana?
A. WIN11-CLIENT  
B. UBUNTU-SOC  
C. Windows Defender  
D. Notepad
---
Question 3: Fill in the Blank
The Windows evidence folder used throughout the course is:
```text
____________________________
```
---
Question 4: Short Answer
Why is it important to record hostnames during a cyber operations investigation?
---
Question 5: Short Answer
What does a connectivity test help confirm?
---
What You Learned
In this lab, you learned how to:
Identify the two virtual machines used in the BlueWave Clinic case study.
Use PowerShell to collect Windows hostname, username, and IP address information.
Use Ubuntu Terminal to collect Linux hostname, username, and IP address information.
Test basic network connectivity between Windows and Ubuntu.
Create evidence folders on both machines.
Create your first investigation notes file.
---
Deliverables
Submit or retain the following items as directed by your instructor.
Deliverable	Location
Lab 1 Windows evidence notes	`C:\BlueWave\Evidence\Lab1-Environment-Notes.txt`
Lab 1 Ubuntu evidence copy	`/home/student/bluewave/evidence/Lab1-Environment-Notes.txt`
Screenshot of Windows hostname and user	Submitted through Skillable instructions
Screenshot of Windows IP address	Submitted through Skillable instructions
Screenshot of Ubuntu hostname and user	Submitted through Skillable instructions
Screenshot of Ubuntu IP address	Submitted through Skillable instructions
Screenshot of Windows-to-Ubuntu connectivity test	Submitted through Skillable instructions
Screenshot of Ubuntu-to-Windows connectivity test	Submitted through Skillable instructions
Screenshot of Windows evidence folder	Submitted through Skillable instructions
Screenshot of Ubuntu evidence folder	Submitted through Skillable instructions
Screenshot of Windows lab files folder check	Submitted through Skillable instructions
Screenshot of Ubuntu lab files folder check	Submitted through Skillable instructions
---
Instructor Notes
Instructor Overview
This lab is designed to orient students to the two-machine Skillable environment.
Students should not install Elastic, configure Elastic Agent, run Sysmon, or execute the simulator in this lab.
The goal is environment awareness, basic command usage, connectivity testing, and evidence setup.
---
Expected Machines
Machine	Expected Role
WIN11-CLIENT	Windows 11 endpoint and analyst workstation
UBUNTU-SOC	Ubuntu SOC server for Elastic and Kibana
---
Expected Windows Commands
Students should run:
```powershell
hostname
```
```powershell
whoami
```
```powershell
$env:COMPUTERNAME
```
```powershell
ipconfig
```
```powershell
Get-NetIPAddress -AddressFamily IPv4 | Where-Object {$_.IPAddress -notlike "127.*"} | Select-Object InterfaceAlias, IPAddress, PrefixLength
```
```powershell
ping <UBUNTU-SOC-IP>
```
```powershell
Test-NetConnection <UBUNTU-SOC-IP>
```
```powershell
New-Item -Path "C:\BlueWave\Evidence" -ItemType Directory -Force
```
```powershell
Test-Path "C:\BlueWave\Evidence"
```
```powershell
Test-Path "C:\BlueWave\Evidence\Lab1-Environment-Notes.txt"
```
---
Expected Ubuntu Commands
Students should run:
```bash
hostname
```
```bash
whoami
```
```bash
pwd
```
```bash
hostname -I
```
```bash
ip addr show
```
```bash
ping -c 4 <WIN11-CLIENT-IP>
```
```bash
mkdir -p /home/student/bluewave/evidence
```
```bash
ls -ld /home/student/bluewave/evidence
```
```bash
nano /home/student/bluewave/evidence/Lab1-Environment-Notes.txt
```
```bash
cat /home/student/bluewave/evidence/Lab1-Environment-Notes.txt
```
---
Expected Evidence Files
Students should create:
```text
C:\BlueWave\Evidence\Lab1-Environment-Notes.txt
```
And:
```text
/home/student/bluewave/evidence/Lab1-Environment-Notes.txt
```
---
Expected Evidence File Content
The Windows evidence file should contain:
Student name.
Date.
Windows hostname.
Windows logged-in user.
Windows IPv4 address.
Windows evidence folder path.
Ubuntu hostname.
Ubuntu logged-in user.
Ubuntu IPv4 address.
Ubuntu evidence folder path.
Windows-to-Ubuntu connectivity result.
Ubuntu-to-Windows connectivity result.
Confirmation of lab file locations.
Short summary of what the student learned.
---
Expected Visible Results
Students should be able to visibly show:
PowerShell command results.
Ubuntu Terminal command results.
Windows IPv4 address.
Ubuntu IPv4 address.
Connectivity test output.
Windows evidence folder.
Ubuntu evidence folder.
Completed Lab 1 notes file.
---
Common Student Mistakes
Mistake	Guidance
Student records `127.0.0.1` as the IP address	Explain that `127.0.0.1` is the loopback address and not the lab network address
Student mixes up Windows and Ubuntu commands	Remind students that PowerShell commands run on Windows and Bash commands run on Ubuntu
Student saves evidence in the wrong folder	Direct them to `C:\BlueWave\Evidence`
Student forgets screenshots	Have them repeat the relevant command and capture the result
Student worries about failed ping	Explain that some lab environments block ICMP and they should record the result
Student tries to install Elastic	Remind them Elastic work starts in later labs
Student tries to run the simulator	Remind them the simulator is introduced later
---
Grading Guidance
Suggested grading allocation:
Criteria	Points
Windows hostname, user, and IP recorded correctly	15
Ubuntu hostname, user, and IP recorded correctly	15
Connectivity tests attempted and documented	15
Windows evidence folder created	10
Ubuntu evidence folder created	10
Lab file locations checked	10
Evidence file completed clearly	15
Screenshots captured	10
Total	100
Do not heavily penalise students if ping fails due to firewall or lab restrictions, as long as they attempted the correct commands and documented the result.
---
Knowledge Check Answer Key
Question 1
Correct answer:
```text
B. WIN11-CLIENT
```
Question 2
Correct answer:
```text
B. UBUNTU-SOC
```
Question 3
Correct answer:
```text
C:\BlueWave\Evidence
```
Question 4
Expected answer:
Hostnames help analysts identify which computer generated an event or was involved in an investigation.
Question 5
Expected answer:
A connectivity test helps confirm that two machines can communicate across the lab network.
---
Free Elastic Basic License Reminder
Elastic is not actively used in this lab.
The course must use self-managed Elastic with the free Basic license.
Students should not use Elastic Cloud.
Students should not create Elastic Cloud accounts.
Students should not download tools from the internet.
---
Fallback Option if Fleet Is Not Available
Fleet is not used in Lab 1.
If Fleet is unavailable at this stage, continue with the lab.
Fleet will be reviewed in later labs.
---
Instructor Preparation Check
Before students begin Lab 1, confirm:
WIN11-CLIENT is accessible.
UBUNTU-SOC is accessible.
Students can open PowerShell.
Students can open Ubuntu Terminal.
`C:\LabFiles` exists if required by the lab image.
`/home/student/labfiles` exists if required by the lab image.
Students have permission to create `C:\BlueWave\Evidence`.
Students have permission to create `/home/student/bluewave/evidence`.
---
Optional Extension Task: Compare Hostname and IP Address Documentation
Purpose
This optional task helps advanced students practise careful documentation.
They will compare system identity information from both machines and explain why accurate host documentation matters in SOC investigations.
---
Steps
On WIN11-CLIENT
Open your evidence file:
```text
C:\BlueWave\Evidence\Lab1-Environment-Notes.txt
```
Add a new section at the bottom:
```text
Optional Extension: Host Identity Comparison

Windows hostname:
Windows IP address:
Ubuntu hostname:
Ubuntu IP address:

Why accurate host identity matters:
```
Write 3 to 5 sentences explaining why hostnames and IP addresses are useful during log analysis.
---
Expected Result
Your evidence file should include an optional extension section explaining the value of accurate host identity.
---
Screenshot Checkpoint
Capture a screenshot showing the optional extension section in your evidence file.
---
Student Note
Add this sentence to your notes:
```text
Accurate hostnames and IP addresses help analysts connect log events to the correct machine during an investigation.
```
---
End of Lab 1.
