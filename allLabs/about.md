# BlueWave Clinic Cyber Operations with Elastic - Lab Series Summary

## Course Overview

This 9-lab Skillable series builds a realistic defensive cyber operations environment for **BlueWave Clinic**, a fictional healthcare organisation developing its first small Security Operations Centre.

Students work with two virtual machines:

| Machine | Purpose |
|---|---|
| WIN11-CLIENT | Windows 11 endpoint and analyst workstation |
| UBUNTU-SOC | Ubuntu SOC server running self-managed Elastic, Kibana, and Fleet |

The course uses **self-managed Elastic with the free Basic license**.  
It does not use Elastic Cloud, paid features, internet downloads, offensive tooling, malware, exploits, or credential theft.

The labs are designed for beginner-to-intermediate learners and use safe simulated endpoint activity for SOC investigation practice.

---

## Case Study Summary

BlueWave Clinic wants to improve its visibility into endpoint activity and build a basic SOC workflow.

Students act as junior SOC analysts responsible for:

- Understanding the lab environment.
- Verifying Windows and Ubuntu systems.
- Accessing Elastic and Kibana.
- Enrolling a Windows endpoint into Elastic.
- Collecting Windows Event Logs.
- Collecting Sysmon process activity.
- Running a safe educational simulator.
- Searching endpoint telemetry in Kibana.
- Creating simple detection logic.
- Performing incident triage.
- Completing a final capstone investigation.

The main safe activity generator used in the series is:

`BlueWaveActivitySimulator.exe`

It is a safe educational activity generator used to create harmless Windows events for SOC analysis.

---

## What We Created

We created a full 9-lab Skillable course:

| Lab | Title | Focus |
|---|---|---|
| Lab 01 | Environment Orientation and Case Study Setup | VM orientation, hostnames, IPs, connectivity, evidence folders |
| Lab 02 | Elastic and Kibana Orientation | Elastic services, Kibana access, Basic license, Discover, Fleet |
| Lab 03 | Enrolling Windows 11 into Elastic | Elastic Agent, Fleet policy, endpoint enrolment |
| Lab 04 | Collecting and Reviewing Windows Event Logs | Windows Security, System, Application, PowerShell logs |
| Lab 05 | Collecting Sysmon Process Activity | Sysmon verification, Event ID 1, process activity |
| Lab 06 | Analysing Simulated Endpoint Activity | Safe simulator investigation, child processes, timeline |
| Lab 07 | Creating Simple Elastic Detection Logic | Detection queries, rules, saved query fallback |
| Lab 08 | Incident Triage and Response Using Elastic | Alert triage, indicators, timeline, incident ticket |
| Lab 09 | Final Elastic Cyber Operations Capstone | Full investigation, executive summary, technical report |

Each lab includes:

- Clear scenario
- Learning objectives
- Required machines and files
- Step-by-step instructions
- Copy and Type options for commands and queries
- Screenshot checkpoints
- Evidence notes
- Validation checklist
- Troubleshooting
- Knowledge check
- Deliverables
- Instructor notes
- Grading guidance

---

## What Students Will Learn

By completing the full series, students will learn how to:

- Work safely in a two-VM cyber lab environment.
- Use Windows PowerShell and Ubuntu Terminal for basic system checks.
- Identify hostnames, usernames, IP addresses, and evidence paths.
- Access Kibana in a self-managed Elastic environment.
- Understand Elastic, Kibana, Fleet, Elastic Agent, and data views.
- Enrol a Windows endpoint into Elastic using Fleet.
- Search Windows Event Logs in Kibana Discover.
- Compare Event Viewer events with Kibana events.
- Understand the value of Sysmon for endpoint monitoring.
- Investigate Sysmon Event ID 1 process creation events.
- Identify process names, parent processes, command lines, users, and timestamps.
- Analyse safe simulated endpoint activity.
- Build simple KQL-based detection logic.
- Use saved queries or detection rules depending on the lab environment.
- Triage a simulated alert.
- Identify investigation indicators.
- Build an incident timeline.
- Write an incident ticket.
- Write a final executive and technical incident report.
- Recommend containment, recovery, and escalation actions.

---

## Student Outcome

At the end of the series, students will have completed a realistic SOC workflow from environment setup to final reporting.

They will understand how endpoint telemetry moves from Windows into Elastic, how analysts search that telemetry in Kibana, and how to turn raw events into investigation evidence, timelines, detections, and reports.

The final capstone confirms that students can perform a complete beginner-level Elastic cyber operations investigation in a safe, defensive lab environment.
