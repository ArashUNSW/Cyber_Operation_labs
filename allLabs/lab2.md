# Lab 02 - BlueWave Clinic Elastic and Kibana Installation and Orientation

## Estimated Time

120–180 minutes

---

## Lab Purpose

In this lab, you will learn where Elastic software comes from, how the Skillable lab provides the required installers, and how to install or verify Elasticsearch and Kibana on the Ubuntu SOC server.

You will then open Kibana from the Windows 11 workstation and explore the main Kibana areas that will be used throughout the course.

This lab introduces the Elastic platform before Windows logs are collected in later labs.

---

## How to Use Copy or Type Inputs

This lab keeps the Skillable **Copy or Type** requirement without repeating the same command twice.

For each command, URL, path, search term, filename, or template, you will see one block named:

```text
Student Input - Copy or Type
```

You may either:

- Copy the text directly into the lab environment.
- Type the same text manually.

> [!note]
> Copy or type the text exactly as shown. You only need to use one method.

> [!alert]
> Commands, URLs, paths, quotation marks, slashes, backslashes, and spaces must match the instructions exactly.

---

## Learning Objectives

By the end of this lab, you will be able to:

- Identify where Elasticsearch, Kibana, and Elastic Agent can be downloaded from in a real environment.
- Explain why this Skillable lab uses preloaded offline installers.
- Locate preloaded Elastic installation files on UBUNTU-SOC.
- Install or verify Elasticsearch on Ubuntu.
- Configure Elasticsearch for a single-node training lab.
- Start and test the Elasticsearch service.
- Install or verify Kibana on Ubuntu.
- Configure Kibana for browser access from WIN11-CLIENT.
- Start and test the Kibana service.
- Open Kibana from the Windows browser.
- Sign in to Kibana using lab credentials.
- Verify that the environment is using self-managed Elastic.
- Navigate to Discover, Fleet, Dashboards, and Data Views.
- Create a Lab 02 Elastic installation and orientation evidence file.

---

## Scenario

BlueWave Clinic has deployed a small self-managed Elastic environment on the Ubuntu SOC server.

You are acting as a junior cyber operations analyst. Before connecting the Windows endpoint to Elastic, you need to understand how the core Elastic services are installed, verify that they are running, and learn where key Kibana features are located.

| Machine | Role |
|---|---|
| WIN11-CLIENT | Analyst workstation used to open Kibana in a browser |
| UBUNTU-SOC | Server running Elasticsearch, Kibana, and later Fleet |

You will not collect Windows logs yet. That begins in Lab 03 and Lab 04.

> [!note]
> This course uses self-managed Elastic with the free Basic license.

> [!alert]
> Do not use Elastic Cloud in this lab. Do not create an Elastic Cloud account.

> [!alert]
> Do not download files from the internet during the Skillable lab unless your instructor specifically tells you to do so.

---

## Software Download and Installation Model

Students should understand where the software comes from, but the Skillable lab is designed to work without internet access.

In a real preparation environment, Elastic software is downloaded from Elastic's official download pages or installed from Elastic package repositories.

In this Skillable lab, the software should already be downloaded and placed in the lab files folder.

| Software | Real-World Source | Skillable Lab Method |
|---|---|---|
| Elasticsearch | Official Elastic download page or Elastic package repository | Use preloaded `.deb` installer |
| Kibana | Official Elastic download page or Elastic package repository | Use preloaded `.deb` installer |
| Elastic Agent | Official Elastic Agent download page or Kibana Fleet instructions | Use preloaded archive or package |
| Fleet Server | Installed using Elastic Agent and Kibana Fleet instructions | Use Kibana-generated command if required |

### Official Download References for Instructor Preparation

These URLs are for awareness and instructor preparation. Students should not download files during the Skillable lab unless instructed.

### Student Input - Copy or Type

```text
Elasticsearch official download source: https://www.elastic.co/downloads/elasticsearch
Kibana official download source: https://www.elastic.co/downloads/kibana
Elastic Agent official download source: https://www.elastic.co/downloads/elastic-agent
Elastic downloads landing page: https://www.elastic.co/downloads
```

> [!alert]
> In this lab, use the preloaded files. Do not download replacement software during the lab.

---

## Required Machines

| Machine | Used For |
|---|---|
| WIN11-CLIENT | Browser access to Kibana and evidence notes |
| UBUNTU-SOC | Elasticsearch and Kibana installation and service verification |

---

## Required Files

The exact version number may vary. Use the files provided in your lab image.

| File Pattern | Expected Location | Purpose |
|---|---|---|
| `elasticsearch-*.deb` | `/home/student/labfiles/tools/elastic` | Elasticsearch installer |
| `kibana-*.deb` | `/home/student/labfiles/tools/elastic` | Kibana installer |
| `elastic-agent-*-linux-x86_64.tar.gz` | `/home/student/labfiles/tools/elastic` | Elastic Agent archive for Fleet Server or later labs |
| `Lab02-Elastic-Installation-Orientation.txt` | `C:\BlueWave\Evidence` | Student-created Windows evidence file |
| `Lab02-Elastic-Installation-Orientation.txt` | `/home/student/bluewave/evidence` | Optional Ubuntu evidence copy |

> [!note]
> If your lab uses a different installer folder, use the path provided by your instructor.

---

## Important Paths and URLs

| Item | Value |
|---|---|
| Expected Elastic installer folder | `/home/student/labfiles/tools/elastic` |
| Windows evidence path | `C:\BlueWave\Evidence` |
| Ubuntu evidence path | `/home/student/bluewave/evidence` |
| Elasticsearch configuration | `/etc/elasticsearch/elasticsearch.yml` |
| Kibana configuration | `/etc/kibana/kibana.yml` |
| Kibana URL format | `http://<UBUNTU-SOC-IP>:5601` |
| Example Kibana URL | `http://10.1.1.10:5601` |
| Elasticsearch service | `elasticsearch` |
| Kibana service | `kibana` |
| Elasticsearch port | `9200` |
| Kibana port | `5601` |
| Fleet Server port | `8220` |

> [!note]
> Replace `<UBUNTU-SOC-IP>` with the actual Ubuntu IP address from your lab environment.

---

## Screenshots You Should Capture

Recommended screenshots:

1. Preloaded Elastic installer folder.
2. Elasticsearch installation or package verification.
3. Elasticsearch service status on UBUNTU-SOC.
4. Elasticsearch local response check.
5. Kibana installation or package verification.
6. Kibana service status on UBUNTU-SOC.
7. Kibana local response check.
8. Ubuntu IP address used for Kibana access.
9. Kibana login page or Kibana home page.
10. Kibana Discover page.
11. Kibana Fleet page.
12. Kibana Dashboards page.
13. Kibana Data Views page.
14. Elastic license page, if available.
15. Completed Lab 02 evidence notes file.

---

## Key Terms

| Term | Meaning |
|---|---|
| Elastic | Platform used to store, search, and analyse event data |
| Elasticsearch | Service that stores and indexes events |
| Kibana | Web interface used to search and visualise Elastic data |
| Discover | Kibana area used to search and inspect events |
| Fleet | Kibana area used to manage Elastic Agents |
| Fleet Server | Service that connects Elastic Agents to Fleet |
| Elastic Agent | Tool that sends logs from a machine to Elastic |
| Data View | Kibana object that tells Kibana which data to search |
| Dashboard | Kibana page with charts, tables, and visualisations |
| Basic license | Free Elastic license used in this course |
| Self-managed Elastic | Elastic installed and managed in the lab, not Elastic Cloud |
| Offline installer | Installer file already downloaded into the lab environment |

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

## Record in Evidence Notes

```text
WIN11-CLIENT access confirmed:
UBUNTU-SOC access confirmed:
```

---

# Task 2 - Confirm the Ubuntu Hostname and User

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Open **Terminal**.
2. Run the hostname command.

### Student Input - Copy or Type

```bash
hostname
```

3. Run the current user command.

### Student Input - Copy or Type

```bash
whoami
```

4. Record the results.

## Expected Result

The hostname should usually be:

```text
UBUNTU-SOC
```

The user should usually be:

```text
student
```

## Record in Evidence Notes

```text
Ubuntu hostname:
Ubuntu logged-in user:
```

---

# Task 3 - Identify the Ubuntu IP Address

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Run the quick IP address command.

### Student Input - Copy or Type

```bash
hostname -I
```

2. Run the network interface command for more detail.

### Student Input - Copy or Type

```bash
ip -br addr
```

3. Identify the non-loopback IPv4 address.

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

```text
Ubuntu IPv4 address:
Ubuntu active interface:
```

---

# Task 4 - Create the Ubuntu Evidence Folder

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Create the Ubuntu evidence folder.

### Student Input - Copy or Type

```bash
mkdir -p /home/student/bluewave/evidence
```

2. Confirm the folder exists.

### Student Input - Copy or Type

```bash
ls -ld /home/student/bluewave/evidence
```

## Expected Result

The folder should exist:

```text
/home/student/bluewave/evidence
```

## Record in Evidence Notes

```text
Ubuntu evidence folder confirmed:
```

---

# Task 5 - Review Where the Elastic Software Comes From

## Where to Work

Use this lab document.

## Steps

1. Review the official download source list in the Software Download and Installation Model section.
2. Confirm that students should use the preloaded installers in the Skillable lab.
3. Record the result.

## Expected Result

You should understand that Elastic software is downloaded from official Elastic sources during lab preparation, but students use preloaded installers inside Skillable.

## Record in Evidence Notes

```text
Official Elastic download sources reviewed:
Skillable uses preloaded installers:
Internet download required during lab:
```

Expected values:

```text
Official Elastic download sources reviewed: Yes
Skillable uses preloaded installers: Yes
Internet download required during lab: No
```

---

# Task 6 - Locate the Preloaded Elastic Installer Folder

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Check whether the expected Elastic installer folder exists.

### Student Input - Copy or Type

```bash
ls -ld /home/student/labfiles/tools/elastic
```

2. List the installer folder.

### Student Input - Copy or Type

```bash
ls -lah /home/student/labfiles/tools/elastic
```

3. Search for Elastic installers if the folder is missing or unclear.

### Student Input - Copy or Type

```bash
find /home/student/labfiles -maxdepth 5 -type f \( -name "elasticsearch*.deb" -o -name "kibana*.deb" -o -name "elastic-agent*.tar.gz" -o -name "elastic-agent*.deb" \) 2>/dev/null
```

## Expected Result

You should find files similar to:

```text
elasticsearch-<version>-amd64.deb
kibana-<version>-amd64.deb
elastic-agent-<version>-linux-x86_64.tar.gz
```

## Screenshot Checkpoint

Capture a screenshot showing the installer folder and files.

## Record in Evidence Notes

```text
Elastic installer folder found:
Installer folder path:
Elasticsearch installer found:
Kibana installer found:
Elastic Agent installer found:
```

---

# Task 7 - Check Whether Elasticsearch Is Already Installed

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Check whether the Elasticsearch package is installed.

### Student Input - Copy or Type

```bash
dpkg -l | grep elasticsearch
```

2. Check whether the Elasticsearch service exists.

### Student Input - Copy or Type

```bash
systemctl status elasticsearch --no-pager
```

## Expected Result

If Elasticsearch is already installed, you should see package or service information.

If it is not installed, continue to Task 8.

> [!note]
> Some Skillable environments may already have Elasticsearch installed. If it is installed and running, verify it instead of reinstalling it unless your instructor tells you to reinstall.

## Record in Evidence Notes

```text
Elasticsearch already installed:
Elasticsearch service status before install:
```

---

# Task 8 - Install Elasticsearch from the Preloaded Package

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

Complete this task only if Elasticsearch is not already installed or your instructor tells you to reinstall it.

1. Change to the Elastic installer folder.

### Student Input - Copy or Type

```bash
cd /home/student/labfiles/tools/elastic
```

2. Install the Elasticsearch Debian package.

### Student Input - Copy or Type

```bash
sudo dpkg -i ./elasticsearch-*.deb
```

3. If the system reports missing dependencies and the lab image has cached dependencies, run the fix command.

### Student Input - Copy or Type

```bash
sudo apt-get install -f -y
```

4. Confirm the package is installed.

### Student Input - Copy or Type

```bash
dpkg -l | grep elasticsearch
```

## Expected Result

Elasticsearch should be installed as a system package.

The output should show an Elasticsearch package name and version.

> [!alert]
> Do not download Elasticsearch during the Skillable lab unless your instructor specifically allows it.

## Screenshot Checkpoint

Capture a screenshot showing Elasticsearch installation or package verification.

## Record in Evidence Notes

```text
Elasticsearch installation performed:
Elasticsearch package version:
```

---

# Task 9 - Configure Elasticsearch for the Training Lab

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

These settings support a simple single-node training lab.

1. Back up the Elasticsearch configuration file.

### Student Input - Copy or Type

```bash
sudo cp /etc/elasticsearch/elasticsearch.yml /etc/elasticsearch/elasticsearch.yml.bak.$(date +%Y%m%d%H%M%S)
```

2. Add single-node discovery if it is not already configured.

### Student Input - Copy or Type

```bash
grep -q '^discovery.type:' /etc/elasticsearch/elasticsearch.yml || echo 'discovery.type: single-node' | sudo tee -a /etc/elasticsearch/elasticsearch.yml
```

3. Add the Elasticsearch network binding if it is not already configured.

### Student Input - Copy or Type

```bash
grep -q '^network.host:' /etc/elasticsearch/elasticsearch.yml || echo 'network.host: 0.0.0.0' | sudo tee -a /etc/elasticsearch/elasticsearch.yml
```

4. Add the Elasticsearch HTTP port if it is not already configured.

### Student Input - Copy or Type

```bash
grep -q '^http.port:' /etc/elasticsearch/elasticsearch.yml || echo 'http.port: 9200' | sudo tee -a /etc/elasticsearch/elasticsearch.yml
```

5. Review active Elasticsearch configuration lines.

### Student Input - Copy or Type

```bash
sudo grep -v '^#' /etc/elasticsearch/elasticsearch.yml | sed '/^$/d'
```

## Expected Result

The configuration should include values similar to:

```text
discovery.type: single-node
network.host: 0.0.0.0
http.port: 9200
```

> [!alert]
> These settings are for an isolated training lab. Do not copy this configuration directly into production.

## Record in Evidence Notes

```text
Elasticsearch configuration backed up:
Elasticsearch single-node configured:
Elasticsearch network host configured:
Elasticsearch port configured:
```

---

# Task 10 - Start and Enable Elasticsearch

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Reload systemd.

### Student Input - Copy or Type

```bash
sudo systemctl daemon-reload
```

2. Enable and start Elasticsearch.

### Student Input - Copy or Type

```bash
sudo systemctl enable --now elasticsearch
```

3. Check Elasticsearch service status.

### Student Input - Copy or Type

```bash
systemctl status elasticsearch --no-pager
```

4. Check whether port 9200 is listening.

### Student Input - Copy or Type

```bash
sudo ss -tulpn | grep 9200
```

## Expected Result

You should see a status line similar to:

```text
Active: active (running)
```

Port 9200 should be listening.

## Screenshot Checkpoint

Capture a screenshot showing Elasticsearch status.

## Record in Evidence Notes

```text
Elasticsearch service enabled:
Elasticsearch service status:
Elasticsearch port 9200 listening:
```

---

# Task 11 - Check Whether Elasticsearch Responds Locally

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Test Elasticsearch with HTTP.

### Student Input - Copy or Type

```bash
curl http://localhost:9200
```

2. If your lab uses HTTPS or security is enabled, test with HTTPS and allow the self-signed certificate.

### Student Input - Copy or Type

```bash
curl -k https://localhost:9200
```

3. If credentials are required and your instructor provided the password, test with the `elastic` user.

### Student Input - Copy or Type

```bash
curl -k -u elastic https://localhost:9200
```

## Expected Result

You may see JSON-style output that includes information such as:

```text
cluster_name
version
tagline
```

Some lab environments may show an authentication message.

> [!note]
> If Elasticsearch asks for authentication, that still confirms the service is reachable.

## Screenshot Checkpoint

Capture a screenshot showing the local Elasticsearch response.

## Record in Evidence Notes

```text
Elasticsearch local response check:
Elasticsearch authentication required:
```

---

# Task 12 - Generate a Kibana Enrollment Token if Required

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

Complete this task only if the Kibana first-time setup page asks for an enrollment token.

1. Generate a Kibana enrollment token.

### Student Input - Copy or Type

```bash
sudo /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
```

2. Copy the token into the Kibana setup screen only if required.
3. Do not paste the full token into public notes or screenshots.

## Expected Result

A long enrollment token should be displayed.

> [!alert]
> The enrollment token is sensitive. Do not share it outside the lab.

## Record in Evidence Notes

```text
Kibana enrollment token required:
Kibana enrollment token generated:
```

---

# Task 13 - Check Whether Kibana Is Already Installed

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Check whether the Kibana package is installed.

### Student Input - Copy or Type

```bash
dpkg -l | grep kibana
```

2. Check whether the Kibana service exists.

### Student Input - Copy or Type

```bash
systemctl status kibana --no-pager
```

## Expected Result

If Kibana is already installed, you should see package or service information.

If it is not installed, continue to Task 14.

## Record in Evidence Notes

```text
Kibana already installed:
Kibana service status before install:
```

---

# Task 14 - Install Kibana from the Preloaded Package

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

Complete this task only if Kibana is not already installed or your instructor tells you to reinstall it.

1. Change to the Elastic installer folder.

### Student Input - Copy or Type

```bash
cd /home/student/labfiles/tools/elastic
```

2. Install the Kibana Debian package.

### Student Input - Copy or Type

```bash
sudo dpkg -i ./kibana-*.deb
```

3. If the system reports missing dependencies and the lab image has cached dependencies, run the fix command.

### Student Input - Copy or Type

```bash
sudo apt-get install -f -y
```

4. Confirm the package is installed.

### Student Input - Copy or Type

```bash
dpkg -l | grep kibana
```

## Expected Result

Kibana should be installed as a system package.

The output should show a Kibana package name and version.

> [!alert]
> Do not download Kibana during the Skillable lab unless your instructor specifically allows it.

## Screenshot Checkpoint

Capture a screenshot showing Kibana installation or package verification.

## Record in Evidence Notes

```text
Kibana installation performed:
Kibana package version:
```

---

# Task 15 - Configure Kibana for Browser Access

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Back up the Kibana configuration file.

### Student Input - Copy or Type

```bash
sudo cp /etc/kibana/kibana.yml /etc/kibana/kibana.yml.bak.$(date +%Y%m%d%H%M%S)
```

2. Configure Kibana to listen on the lab network interface if it is not already configured.

### Student Input - Copy or Type

```bash
grep -q '^server.host:' /etc/kibana/kibana.yml || echo 'server.host: "0.0.0.0"' | sudo tee -a /etc/kibana/kibana.yml
```

3. Configure the Kibana server port if it is not already configured.

### Student Input - Copy or Type

```bash
grep -q '^server.port:' /etc/kibana/kibana.yml || echo 'server.port: 5601' | sudo tee -a /etc/kibana/kibana.yml
```

4. Review active Kibana configuration lines.

### Student Input - Copy or Type

```bash
sudo grep -v '^#' /etc/kibana/kibana.yml | sed '/^$/d'
```

## Expected Result

The configuration should include values similar to:

```text
server.host: "0.0.0.0"
server.port: 5601
```

## Record in Evidence Notes

```text
Kibana configuration backed up:
Kibana server host configured:
Kibana server port configured:
```

---

# Task 16 - Start and Enable Kibana

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Enable and start Kibana.

### Student Input - Copy or Type

```bash
sudo systemctl enable --now kibana
```

2. Check Kibana service status.

### Student Input - Copy or Type

```bash
systemctl status kibana --no-pager
```

3. Check whether port 5601 is listening.

### Student Input - Copy or Type

```bash
sudo ss -tulpn | grep 5601
```

4. Review recent Kibana logs if the service is not ready.

### Student Input - Copy or Type

```bash
sudo journalctl -u kibana --no-pager -n 50
```

## Expected Result

You should see a status line similar to:

```text
Active: active (running)
```

Port 5601 should be listening after Kibana finishes starting.

> [!note]
> Kibana may take a few minutes to become available.

## Screenshot Checkpoint

Capture a screenshot showing Kibana status.

## Record in Evidence Notes

```text
Kibana service enabled:
Kibana service status:
Kibana port 5601 listening:
```

---

# Task 17 - Check Whether Kibana Responds Locally

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Run a local Kibana check.

### Student Input - Copy or Type

```bash
curl -I http://localhost:5601
```

2. If Kibana is configured with HTTPS, use this alternate check.

### Student Input - Copy or Type

```bash
curl -k -I https://localhost:5601
```

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

Capture a screenshot showing the Kibana local response check.

## Record in Evidence Notes

```text
Kibana local response check:
```

---

# Task 18 - Generate a Kibana Verification Code if Required

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

Complete this task only if the Kibana browser setup asks for a verification code.

1. Generate the Kibana verification code.

### Student Input - Copy or Type

```bash
sudo /usr/share/kibana/bin/kibana-verification-code
```

2. Enter the code into the Kibana setup screen.

## Expected Result

A short verification code should be displayed.

> [!note]
> The verification code is only used during first-time Kibana setup.

## Record in Evidence Notes

```text
Kibana verification code required:
Kibana verification code generated:
```

---

# Task 19 - Build the Kibana URL

## Where to Work

Use **WIN11-CLIENT**.

Use the Ubuntu IP address from Task 3.

## Steps

1. Find the Ubuntu IP address you recorded.
2. Build the Kibana URL using this format.

### Student Input - Copy or Type

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

## Record in Evidence Notes

```text
Kibana URL:
```

---

# Task 20 - Open Kibana from WIN11-CLIENT

## Where to Work

Use **WIN11-CLIENT** and a web browser.

## Steps

1. Open the browser on WIN11-CLIENT.
2. Select the address bar.
3. Enter the Kibana URL you built in Task 19.

### Student Input - Copy or Type

```text
http://<UBUNTU-SOC-IP>:5601
```

Example:

```text
http://10.1.1.10:5601
```

4. Press **Enter**.
5. Wait for Kibana to load.
6. If first-time setup appears, use the enrollment token and verification code from earlier tasks.

## Expected Result

You should see one of the following:

```text
Kibana login page
```

or:

```text
Kibana home page
```

## Screenshot Checkpoint

Capture a screenshot showing the Kibana login page or home page.

## Record in Evidence Notes

```text
Kibana opened from Windows browser:
Kibana first-time setup required:
```

---

# Task 21 - Sign In to Kibana

## Where to Work

Use **WIN11-CLIENT** and the browser with Kibana open.

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

```text
Kibana sign-in successful:
```

---

# Task 22 - Confirm This Is Self-Managed Elastic

## Where to Work

Use **WIN11-CLIENT** and Kibana in the browser.

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

## Record in Evidence Notes

```text
Elastic deployment type:
Elastic Cloud used:
```

Expected values:

```text
Elastic deployment type: Self-managed lab Elastic
Elastic Cloud used: No
```

---

# Task 23 - Locate the Main Kibana Navigation Menu

## Where to Work

Use **WIN11-CLIENT** and Kibana in the browser.

## Steps

1. Look for the navigation menu on the left side of Kibana.
2. If the menu is collapsed, select the menu icon.
3. Review the visible Kibana sections.
4. Look for these areas.

### Student Input - Copy or Type

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

```text
Kibana navigation menu located:
```

---

# Task 24 - Open Kibana Discover

## Where to Work

Use **WIN11-CLIENT** and Kibana in the browser.

## Steps

1. In Kibana, open the navigation menu.
2. Select **Discover**.
3. If you cannot find Discover, use the Kibana search bar.

### Student Input - Copy or Type

```text
Discover
```

4. Open **Discover**.

## Expected Result

The Discover page should open.

You may see a message asking for a data view, or you may see event data if data already exists.

> [!note]
> It is acceptable if no Windows logs are visible yet. Windows log collection begins in later labs.

## Screenshot Checkpoint

Capture a screenshot showing the Discover page.

## Record in Evidence Notes

```text
Discover page opened:
```

---

# Task 25 - Review the Discover Time Range Control

## Where to Work

Use **WIN11-CLIENT** and Kibana Discover.

## Steps

1. Locate the time range control in Discover.
2. It is usually near the top right of the page.
3. Select the time range control.
4. Set the time range to **Last 24 hours** if available.

### Student Input - Copy or Type

```text
Last 24 hours
```

5. Select **Update** or **Refresh** if required.

## Expected Result

Discover should now use a recent time range.

> [!note]
> Time range selection is important in later labs. If the time range is too narrow, you may not see expected events.

## Record in Evidence Notes

```text
Discover time range reviewed:
```

---

# Task 26 - Locate the Data View Selector in Discover

## Where to Work

Use **WIN11-CLIENT** and Kibana Discover.

## Steps

1. Stay on the Discover page.
2. Look for the Data View selector.
3. It may appear near the top left of the Discover page.
4. Select the Data View selector.
5. Review the available data views.

## Expected Result

You may see one or more data views, such as:

```text
logs-*
metrics-*
filebeat-*
winlogbeat-*
logs-windows.*
```

It is acceptable if your environment has no Windows data view yet.

## Screenshot Checkpoint

Capture a screenshot showing the Data View selector or available data views.

## Record in Evidence Notes

```text
Data View selector located:
Data Views observed:
```

---

# Task 27 - Open Data Views from Stack Management

## Where to Work

Use **WIN11-CLIENT** and Kibana in the browser.

## Steps

1. Open the Kibana navigation menu.
2. Search for or open **Stack Management**.

### Student Input - Copy or Type

```text
Stack Management
```

3. Look for **Data Views**.

### Student Input - Copy or Type

```text
Data Views
```

4. Open **Data Views**.

## Expected Result

The Data Views page should open.

You should be able to see existing data views or a page where data views can be created.

> [!note]
> Do not create a new data view unless your instructor tells you to. This lab is orientation only.

## Screenshot Checkpoint

Capture a screenshot showing the Data Views page.

## Record in Evidence Notes

```text
Data Views page opened:
```

---

# Task 28 - Open Fleet

## Where to Work

Use **WIN11-CLIENT** and Kibana in the browser.

## Steps

1. Open the Kibana navigation menu.
2. Search for or open **Fleet**.

### Student Input - Copy or Type

```text
Fleet
```

3. Review the page.
4. Look for tabs or sections such as these.

### Student Input - Copy or Type

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

```text
Fleet page opened:
Agents visible in Fleet:
```

---

# Task 29 - Review Fleet Server Readiness

## Where to Work

Use **WIN11-CLIENT**, **Kibana Fleet**, and **UBUNTU-SOC Terminal**.

## Steps

1. In Fleet, check whether a Fleet Server is already visible.
2. On UBUNTU-SOC, check the Elastic Agent service.

### Student Input - Copy or Type

```bash
systemctl status elastic-agent --no-pager
```

3. Check whether Fleet Server port 8220 is listening.

### Student Input - Copy or Type

```bash
sudo ss -tulpn | grep 8220
```

## Expected Result

One of the following should be true:

```text
Fleet Server is already configured.
```

or:

```text
Fleet Server is not configured yet and will need to be added before Windows enrollment.
```

> [!note]
> If Fleet Server is not ready, Lab 03 may include the enrollment preparation steps, or your instructor may provide a preconfigured Fleet Server.

## Record in Evidence Notes

```text
Fleet Server visible in Kibana:
Elastic Agent service on Ubuntu:
Fleet Server port 8220 listening:
```

---

# Task 30 - Optional: Prepare Fleet Server Only If Instructed

## Where to Work

Use **WIN11-CLIENT**, **Kibana Fleet**, and **UBUNTU-SOC Terminal**.

## Steps

Complete this task only if Fleet Server is not already configured and your instructor tells you to set it up.

1. In Kibana, open **Fleet**.
2. Select **Add Fleet Server**.
3. Use the Quick Start option unless your instructor provides advanced settings.
4. Follow the Kibana-generated instructions.
5. Do not invent a service token or enrollment token.
6. On UBUNTU-SOC, locate the preloaded Elastic Agent archive.

### Student Input - Copy or Type

```bash
find /home/student/labfiles -maxdepth 5 -type f -name "elastic-agent-*-linux-x86_64.tar.gz" 2>/dev/null
```

7. Create a temporary extraction folder.

### Student Input - Copy or Type

```bash
mkdir -p /home/student/elastic-agent-install
```

8. Extract the preloaded Elastic Agent archive.

### Student Input - Copy or Type

```bash
tar -xzf /home/student/labfiles/tools/elastic/elastic-agent-*-linux-x86_64.tar.gz -C /home/student/elastic-agent-install --strip-components=1
```

9. Change to the extracted Elastic Agent folder.

### Student Input - Copy or Type

```bash
cd /home/student/elastic-agent-install
```

10. Confirm the Elastic Agent version.

### Student Input - Copy or Type

```bash
./elastic-agent version
```

11. Run the exact Fleet Server install command shown by Kibana.

### Example Only - Do Not Copy Unless It Matches Your Kibana Screen

```text
sudo ./elastic-agent install <KIBANA-GENERATED-FLEET-SERVER-OPTIONS>
```

12. Confirm the Elastic Agent service.

### Student Input - Copy or Type

```bash
systemctl status elastic-agent --no-pager
```

13. Confirm Fleet Server port 8220 if configured.

### Student Input - Copy or Type

```bash
sudo ss -tulpn | grep 8220
```

## Expected Result

Fleet Server should appear in Kibana Fleet and Elastic Agent should run as a service.

> [!alert]
> The Fleet Server command contains sensitive generated values. Use only the command shown in your Kibana page.

## Record in Evidence Notes

```text
Fleet Server setup performed:
Elastic Agent archive found:
Elastic Agent version:
Fleet Server install command obtained from Kibana:
Fleet Server connected:
```

---

# Task 31 - Review Agent Policies in Fleet

## Where to Work

Use **WIN11-CLIENT** and Kibana Fleet.

## Steps

1. In Fleet, select **Agent policies** if visible.

### Student Input - Copy or Type

```text
Agent policies
```

2. Review the list of available policies.
3. Look for a Windows-related policy.

### Student Input - Copy or Type

```text
Windows
```

4. Do not change any policy settings in this lab.

## Expected Result

You may see an agent policy prepared for Windows endpoints.

Examples:

```text
Windows Endpoint Policy
Default policy
```

It is acceptable if the exact policy name is different.

## Screenshot Checkpoint

Capture a screenshot showing the Agent policies page if available.

## Record in Evidence Notes

```text
Agent policies reviewed:
Windows policy observed:
```

---

# Task 32 - Open Dashboards

## Where to Work

Use **WIN11-CLIENT** and Kibana in the browser.

## Steps

1. Open the Kibana navigation menu.
2. Search for or open **Dashboards**.

### Student Input - Copy or Type

```text
Dashboards
```

3. Review the page.

## Expected Result

The Dashboards page should open.

You may see existing dashboards or an empty dashboard list.

> [!note]
> Dashboards may be used later to visualise log data, but this lab only asks you to locate the page.

## Screenshot Checkpoint

Capture a screenshot showing the Dashboards page.

## Record in Evidence Notes

```text
Dashboards page opened:
Dashboards observed:
```

---

# Task 33 - Locate License Management

## Where to Work

Use **WIN11-CLIENT** and Kibana in the browser.

## Steps

1. Open the Kibana navigation menu.
2. Search for or open **Stack Management**.

### Student Input - Copy or Type

```text
Stack Management
```

3. Search for or open **License Management**.

### Student Input - Copy or Type

```text
License Management
```

4. Review the license type if the page is available.
5. Do not start a trial.
6. Do not change the license.

## Expected Result

If License Management is available, the expected course license is:

```text
Basic
```

If you cannot access License Management, record that it was not available to your account.

> [!alert]
> Do not start a free trial. This course is designed for free self-managed Elastic Basic features.

## Screenshot Checkpoint

Capture a screenshot showing the license page if available.

## Record in Evidence Notes

```text
License Management page available:
Elastic license observed:
Trial started:
```

---

# Task 34 - Locate Kibana Help or Version Information

## Where to Work

Use **WIN11-CLIENT** and Kibana in the browser.

## Steps

1. Look for the help menu, user menu, or space menu in Kibana.
2. Look for version, help, or about information.
3. If visible, record the Kibana version.
4. If you cannot find the version, record that it was not located.

## Expected Result

You may find Kibana version information.

If not, that is acceptable for this lab.

## Record in Evidence Notes

```text
Kibana version observed:
```

---

# Task 35 - Create the Lab 02 Evidence Notes File on Windows

## Where to Work

Use **WIN11-CLIENT** and **Notepad**.

## Steps

1. Return to **WIN11-CLIENT**.
2. Open **Notepad**.
3. Copy or type the evidence template below into Notepad.
4. Fill in the missing information using your lab results.
5. Select **File**.
6. Select **Save As**.
7. Save the file in the required folder with the required filename.

### Save Location - Copy or Type

```text
C:\BlueWave\Evidence
```

### Filename - Copy or Type

```text
Lab02-Elastic-Installation-Orientation.txt
```

### Evidence Template - Copy or Type

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 02 - Elastic and Kibana Installation and Orientation

Student Name:
Date:

1. Lab Access

WIN11-CLIENT access confirmed:
UBUNTU-SOC access confirmed:
Ubuntu hostname:
Ubuntu logged-in user:
Ubuntu IPv4 address:
Ubuntu active interface:
Ubuntu evidence folder confirmed:

2. Software Sources and Installers

Official Elastic download sources reviewed:
Skillable uses preloaded installers:
Internet download required during lab:
Elastic installer folder found:
Installer folder path:
Elasticsearch installer found:
Kibana installer found:
Elastic Agent installer found:

3. Elasticsearch Installation and Verification

Elasticsearch already installed:
Elasticsearch service status before install:
Elasticsearch installation performed:
Elasticsearch package version:
Elasticsearch configuration backed up:
Elasticsearch single-node configured:
Elasticsearch network host configured:
Elasticsearch port configured:
Elasticsearch service enabled:
Elasticsearch service status:
Elasticsearch port 9200 listening:
Elasticsearch local response check:
Elasticsearch authentication required:
Kibana enrollment token required:
Kibana enrollment token generated:

4. Kibana Installation and Verification

Kibana already installed:
Kibana service status before install:
Kibana installation performed:
Kibana package version:
Kibana configuration backed up:
Kibana server host configured:
Kibana server port configured:
Kibana service enabled:
Kibana service status:
Kibana port 5601 listening:
Kibana local response check:
Kibana verification code required:
Kibana verification code generated:

5. Kibana Access

Kibana URL:
Kibana opened from Windows browser:
Kibana first-time setup required:
Kibana sign-in successful:
Elastic deployment type:
Elastic Cloud used:

6. Kibana Orientation

Kibana navigation menu located:
Discover page opened:
Discover time range reviewed:
Data View selector located:
Data Views observed:
Data Views page opened:
Fleet page opened:
Agents visible in Fleet:
Fleet Server visible in Kibana:
Elastic Agent service on Ubuntu:
Fleet Server port 8220 listening:
Fleet Server setup performed:
Fleet Server connected:
Agent policies reviewed:
Windows policy observed:
Dashboards page opened:
Dashboards observed:

7. License Review

License Management page available:
Elastic license observed:
Trial started:
Kibana version observed:

8. Lab Summary

Write 3 to 5 sentences explaining what you learned about installing or verifying Elasticsearch and Kibana, opening Kibana from Windows, and locating Discover, Fleet, Dashboards, and Data Views.
```

## Expected Result

The file should be saved as:

```text
C:\BlueWave\Evidence\Lab02-Elastic-Installation-Orientation.txt
```

## Screenshot Checkpoint

Capture a screenshot showing the completed evidence notes file.

---

# Task 36 - Confirm the Windows Evidence Notes File Exists

## Where to Work

Use **WIN11-CLIENT** and **Windows PowerShell**.

## Steps

1. Run the file validation command.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\Evidence\Lab02-Elastic-Installation-Orientation.txt"
```

2. Press **Enter**.

## Expected Result

PowerShell should return:

```text
True
```

If the result is `False`, check the folder path and filename.

## Screenshot Checkpoint

Capture a screenshot showing the validation result if required.

---

# Task 37 - Create an Optional Ubuntu Evidence Copy

## Where to Work

Use **UBUNTU-SOC** and **Terminal**.

## Steps

1. Create or open the Ubuntu evidence notes file.

### Student Input - Copy or Type

```bash
nano /home/student/bluewave/evidence/Lab02-Elastic-Installation-Orientation.txt
```

2. Press **Enter**.
3. Copy or type the Ubuntu evidence copy template below.
4. Fill in the missing information.
5. Press **Ctrl + O**.
6. Press **Enter** to save.
7. Press **Ctrl + X** to exit nano.

### Ubuntu Evidence Copy Template - Copy or Type

```text
BlueWave Clinic Cyber Operations with Elastic
Lab 02 - Ubuntu Evidence Copy

Ubuntu hostname:
Ubuntu IPv4 address:
Installer folder path:
Elasticsearch installer found:
Kibana installer found:
Elasticsearch service status:
Kibana service status:
Elasticsearch local response check:
Kibana local response check:
Kibana URL:
Kibana opened from Windows browser:
Fleet Server visible:
License observed:
```

## Expected Result

The optional Ubuntu evidence copy should be saved at:

```text
/home/student/bluewave/evidence/Lab02-Elastic-Installation-Orientation.txt
```

## Record in Windows Evidence Notes

```text
Ubuntu Lab 02 evidence copy created:
```

---

# Task 38 - Final Validation

## Where to Work

Use both machines.

## Steps on WIN11-CLIENT

1. Open **Windows PowerShell**.
2. Validate the Windows Lab 02 evidence file.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\Evidence\Lab02-Elastic-Installation-Orientation.txt"
```

3. Open **File Explorer**.
4. Browse to the evidence folder.

### Student Input - Copy or Type

```text
C:\BlueWave\Evidence
```

5. Confirm that `Lab02-Elastic-Installation-Orientation.txt` exists.

## Steps on UBUNTU-SOC

1. Open **Terminal**.
2. Confirm Elasticsearch status one final time.

### Student Input - Copy or Type

```bash
systemctl is-active elasticsearch
```

3. Confirm Kibana status one final time.

### Student Input - Copy or Type

```bash
systemctl is-active kibana
```

4. Confirm Kibana is listening on port 5601.

### Student Input - Copy or Type

```bash
sudo ss -tulpn | grep 5601
```

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

Kibana should be reachable from WIN11-CLIENT.

## Screenshot Checkpoint

Capture a final screenshot showing the Windows evidence file.

Capture a final screenshot showing the Elasticsearch and Kibana active checks if required.

---

# Validation Checklist

Before you finish the lab, confirm each item is complete.

- [ ] I signed in to WIN11-CLIENT.
- [ ] I signed in to UBUNTU-SOC.
- [ ] I confirmed the Ubuntu hostname and user.
- [ ] I identified the Ubuntu IP address.
- [ ] I reviewed official Elastic download sources.
- [ ] I confirmed that Skillable uses preloaded installers.
- [ ] I located the preloaded Elastic installer folder.
- [ ] I checked whether Elasticsearch was already installed.
- [ ] I installed Elasticsearch if required.
- [ ] I configured Elasticsearch for the training lab.
- [ ] I started and enabled Elasticsearch.
- [ ] I checked the local Elasticsearch response.
- [ ] I generated a Kibana enrollment token if required.
- [ ] I checked whether Kibana was already installed.
- [ ] I installed Kibana if required.
- [ ] I configured Kibana for browser access.
- [ ] I started and enabled Kibana.
- [ ] I checked the local Kibana response.
- [ ] I generated a Kibana verification code if required.
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
- [ ] I reviewed Fleet Server readiness.
- [ ] I reviewed Agent policies if available.
- [ ] I opened Dashboards.
- [ ] I checked License Management if available.
- [ ] I did not start a trial.
- [ ] I created `C:\BlueWave\Evidence\Lab02-Elastic-Installation-Orientation.txt`.
- [ ] I captured the required screenshots.
- [ ] I reviewed my evidence notes for completeness.

---

# Troubleshooting

## Elasticsearch installer is missing

Search for the installer.

### Student Input - Copy or Type

```bash
find /home/student/labfiles -maxdepth 5 -type f -name "elasticsearch*.deb" 2>/dev/null
```

If no installer is found, notify your instructor.

> [!alert]
> Do not download replacement installers during the Skillable lab unless instructed.

---

## Elasticsearch package does not install

Confirm the package exists.

### Student Input - Copy or Type

```bash
ls -lah /home/student/labfiles/tools/elastic/elasticsearch-*.deb
```

Try the package installation again.

### Student Input - Copy or Type

```bash
cd /home/student/labfiles/tools/elastic
sudo dpkg -i ./elasticsearch-*.deb
```

If dependency errors appear and cached packages are available, run:

### Student Input - Copy or Type

```bash
sudo apt-get install -f -y
```

If the issue continues, ask your instructor.

---

## Elasticsearch does not start

Check the service status and logs.

### Student Input - Copy or Type

```bash
systemctl status elasticsearch --no-pager
```

### Student Input - Copy or Type

```bash
sudo journalctl -u elasticsearch --no-pager -n 80
```

Check active configuration.

### Student Input - Copy or Type

```bash
sudo grep -v '^#' /etc/elasticsearch/elasticsearch.yml | sed '/^$/d'
```

---

## Elasticsearch curl test shows an authentication message

This can be normal.

It means Elasticsearch is reachable but requires credentials.

Record the result and continue.

---

## Kibana installer is missing

Search for the installer.

### Student Input - Copy or Type

```bash
find /home/student/labfiles -maxdepth 5 -type f -name "kibana*.deb" 2>/dev/null
```

If no installer is found, notify your instructor.

> [!alert]
> Do not download replacement installers during the Skillable lab unless instructed.

---

## Kibana package does not install

Confirm the package exists.

### Student Input - Copy or Type

```bash
ls -lah /home/student/labfiles/tools/elastic/kibana-*.deb
```

Try the package installation again.

### Student Input - Copy or Type

```bash
cd /home/student/labfiles/tools/elastic
sudo dpkg -i ./kibana-*.deb
```

If dependency errors appear and cached packages are available, run:

### Student Input - Copy or Type

```bash
sudo apt-get install -f -y
```

If the issue continues, ask your instructor.

---

## Kibana does not start

Check the service status and logs.

### Student Input - Copy or Type

```bash
systemctl status kibana --no-pager
```

### Student Input - Copy or Type

```bash
sudo journalctl -u kibana --no-pager -n 80
```

Confirm port 5601.

### Student Input - Copy or Type

```bash
sudo ss -tulpn | grep 5601
```

---

## Kibana does not open from Windows

Confirm the Ubuntu IP address.

### Student Input - Copy or Type

```bash
hostname -I
```

Confirm Kibana is listening.

### Student Input - Copy or Type

```bash
sudo ss -tulpn | grep 5601
```

Use this URL format from the Windows browser.

### Student Input - Copy or Type

```text
http://<UBUNTU-SOC-IP>:5601
```

Example:

```text
http://10.1.1.10:5601
```

Do not use `127.0.0.1` from the Windows browser.

---

## Kibana asks for an enrollment token

Generate a Kibana enrollment token on UBUNTU-SOC.

### Student Input - Copy or Type

```bash
sudo /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
```

Paste the token into the Kibana setup screen only.

> [!alert]
> Do not include the token in public screenshots.

---

## Kibana asks for a verification code

Generate the verification code on UBUNTU-SOC.

### Student Input - Copy or Type

```bash
sudo /usr/share/kibana/bin/kibana-verification-code
```

Enter the code into the Kibana setup screen.

---

## Discover shows no data

This may be normal in Lab 02 because Windows logs may not be connected yet.

Set the time range to:

### Student Input - Copy or Type

```text
Last 24 hours
```

Then continue.

---

## Fleet, Data Views, or License Management is not visible

Use the Kibana search bar.

### Student Input - Copy or Type

```text
Fleet
Data Views
License Management
```

If a page is still not visible, record that it was not available to your account and continue.

> [!alert]
> Do not start a trial.

---

## Fleet Server is not configured

Open Fleet in Kibana and check whether Fleet Server exists.

### Student Input - Copy or Type

```text
Fleet
```

If Fleet Server must be added, use **Add Fleet Server** and follow the Kibana-generated command.

> [!alert]
> Do not invent Fleet Server tokens or commands.

---

## Windows evidence folder or file is missing

Create the folder if needed.

### Student Input - Copy or Type

```powershell
New-Item -Path "C:\BlueWave\Evidence" -ItemType Directory -Force
```

Confirm the evidence file.

### Student Input - Copy or Type

```powershell
Test-Path "C:\BlueWave\Evidence\Lab02-Elastic-Installation-Orientation.txt"
```

Expected result:

```text
True
```

If the result is `False`, confirm the filename is exactly:

```text
Lab02-Elastic-Installation-Orientation.txt
```

---

# Knowledge Check

Answer the following questions.

1. Why does the Skillable lab use preloaded Elastic installers?
2. What service stores and indexes Elastic data?
3. What service provides the Elastic web interface?
4. What Ubuntu command installs a `.deb` package?
5. What service name is used for Elasticsearch?
6. What service name is used for Kibana?
7. What port does Kibana usually use in this lab?
8. Why should you not use `127.0.0.1` from the Windows browser to access Kibana on Ubuntu?
9. What Kibana area is used to manage Elastic Agents?
10. What should you do if Kibana asks for an enrollment token?

---

# Summary

In this lab, you completed the following tasks:

- Reviewed where Elastic software comes from.
- Located preloaded Elastic installers.
- Installed or verified Elasticsearch.
- Configured and started Elasticsearch.
- Tested Elasticsearch locally.
- Installed or verified Kibana.
- Configured and started Kibana.
- Opened Kibana from WIN11-CLIENT.
- Signed in to Kibana.
- Confirmed the lab uses self-managed Elastic rather than Elastic Cloud.
- Opened Discover.
- Reviewed the Discover time range control.
- Located Data Views.
- Opened Fleet.
- Reviewed Fleet Server readiness.
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
| Windows Lab 02 evidence notes | `C:\BlueWave\Evidence\Lab02-Elastic-Installation-Orientation.txt` |
| Optional Ubuntu Lab 02 evidence copy | `/home/student/bluewave/evidence/Lab02-Elastic-Installation-Orientation.txt` |
| Screenshot of preloaded Elastic installer folder | Skillable submission area |
| Screenshot of Elasticsearch package verification | Skillable submission area |
| Screenshot of Elasticsearch service status | Skillable submission area |
| Screenshot of Elasticsearch local response check | Skillable submission area |
| Screenshot of Kibana package verification | Skillable submission area |
| Screenshot of Kibana service status | Skillable submission area |
| Screenshot of Kibana local response check | Skillable submission area |
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

1. The Skillable lab uses preloaded installers so the lab works without internet access and uses known approved versions.
2. Elasticsearch stores and indexes Elastic data.
3. Kibana provides the Elastic web interface.
4. `sudo dpkg -i ./package-name.deb` installs a Debian package.
5. The Elasticsearch service name is `elasticsearch`.
6. The Kibana service name is `kibana`.
7. Kibana usually uses port `5601`.
8. `127.0.0.1` points to the local machine. From Windows, it would point to WIN11-CLIENT, not UBUNTU-SOC.
9. Fleet is used to manage Elastic Agents.
10. Generate a Kibana enrollment token on UBUNTU-SOC and enter it into the Kibana setup screen.

## Expected Evidence Files

Students should create:

```text
C:\BlueWave\Evidence\Lab02-Elastic-Installation-Orientation.txt
```

Optional Ubuntu copy:

```text
/home/student/bluewave/evidence/Lab02-Elastic-Installation-Orientation.txt
```

## Expected Installer Files

Expected installer files may include:

```text
/home/student/labfiles/tools/elastic/elasticsearch-<version>-amd64.deb
/home/student/labfiles/tools/elastic/kibana-<version>-amd64.deb
/home/student/labfiles/tools/elastic/elastic-agent-<version>-linux-x86_64.tar.gz
```

The exact version number may vary.

## Expected Ubuntu Commands

Install Elasticsearch:

```bash
cd /home/student/labfiles/tools/elastic
sudo dpkg -i ./elasticsearch-*.deb
```

Install Kibana:

```bash
cd /home/student/labfiles/tools/elastic
sudo dpkg -i ./kibana-*.deb
```

Check services:

```bash
systemctl status elasticsearch --no-pager
systemctl status kibana --no-pager
```

Check ports:

```bash
sudo ss -tulpn | grep 9200
sudo ss -tulpn | grep 5601
```

Open Kibana from Windows:

```text
http://<UBUNTU-SOC-IP>:5601
```

## Expected Results

Students should be able to show:

- Official software source model reviewed.
- Preloaded installer files located.
- Elasticsearch installed or already installed.
- Elasticsearch running.
- Elasticsearch reachable locally.
- Kibana installed or already installed.
- Kibana running.
- Kibana reachable locally.
- Kibana reachable from WIN11-CLIENT.
- Self-managed Elastic confirmed.
- Discover opened.
- Fleet page opened.
- Data Views located.
- Dashboards opened.
- License reviewed or access limitation documented.
- Evidence notes completed.

## Common Student Mistakes

| Mistake | Instructor Guidance |
|---|---|
| Student downloads from the internet | Remind them to use preloaded installers |
| Student uses Elastic Cloud | Remind them this is self-managed Elastic only |
| Student records `127.0.0.1` as the Ubuntu IP | Have them use the lab network IP |
| Student forgets to start services | Have them run `systemctl enable --now` commands |
| Student opens Kibana with localhost from Windows | Have them use the UBUNTU-SOC IP address |
| Student exposes enrollment token in screenshots | Remind them tokens are sensitive |
| Student panics because Discover has no data | Explain that Windows logs are not collected until later labs |
| Student cannot find Fleet | Have them use the Kibana search bar |
| Student cannot find License Management | Record it as unavailable if permissions do not allow access |
| Student changes Fleet or Data View settings | Remind them Lab 02 is orientation and installation only |
| Student starts an Elastic trial | Remind them to stay on the free Basic license |

## Grading Guidance

| Criteria | Points |
|---|---:|
| Official sources and preloaded installer model understood | 10 |
| Installer files located | 10 |
| Elasticsearch installed or verified | 15 |
| Elasticsearch configured, started, and tested | 15 |
| Kibana installed or verified | 15 |
| Kibana configured, started, and opened from Windows | 15 |
| Discover, Fleet, Dashboards, and Data Views reviewed | 10 |
| License reviewed or access limitation documented | 5 |
| Evidence notes and screenshots completed | 5 |
| Total | 100 |

## Free Elastic Basic License Reminder

This course must use:

- Self-managed Elastic.
- Free Elastic Basic license.
- Preloaded offline installers.
- Kibana on UBUNTU-SOC.
- Fleet if available.
- No Elastic Cloud.
- No paid subscriptions.
- No external internet dependency during the lab.

If a feature appears to require a paid license, use the free alternative:

- Kibana Discover.
- Saved queries.
- Manual analysis.
- Evidence screenshots.
- Markdown or text incident notes.

## Fallback if Fleet Is Not Available

If Fleet is not visible or not configured:

1. Have students record that Fleet was not available.
2. Continue with Discover, Dashboards, Data Views, and service verification.
3. In Lab 03, the instructor may provide an alternate Elastic Agent enrolment method or preconfigured Fleet access.

---

End of Lab 02.
