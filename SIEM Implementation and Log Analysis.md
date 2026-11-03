# SIEM Implementation and Log Analysis

<a href="https://linkedin.com/in/william-a-dean"><img src="https://img.shields.io/badge/-LinkedIn-0072b1?&style=for-the-badge&logo=linkedin&logoColor=white" /></a>
<a href="https://github.com/WillD92/WillD92/blob/main/resume.md"><img src="https://img.shields.io/badge/Resume-7B2CBF?style=for-the-badge&logo=linkedin&logoColor=white" /></a>

## Objective

In this project I built a small home lab to learn how security teams spot attacks.

I set up a **SIEM** (Security Information and Event Management) tool. Think of it as a security camera system for computers: it collects activity records (called **logs**) from different machines and puts them in one place, so you can search them and spot anything suspicious.

I then ran safe, simulated attacks against my own lab computer and used the SIEM to find evidence of them in the logs.

## Key Terms (Quick Glossary)

| Term | What it means |
|------|---------------|
| **Log** | A record of something that happened on a computer, like a login or a program starting |
| **SIEM** | A tool that collects logs from many computers so you can search and analyze them |
| **Endpoint** | Any computer being monitored (here, a Windows virtual machine) |
| **Forwarder** | A small program that sends logs from the endpoint to the SIEM |
| **Sysmon** | A free Microsoft tool that records detailed activity on Windows |
| **Virtual Machine (VM)** | A computer running inside your computer, so you can test safely |
| **Event ID** | A number Windows gives each type of event (for example, 4625 = failed login) |

## Skills Learned

- Setting up a SIEM from scratch
- Sending logs from a computer to the SIEM
- Searching logs to find specific events
- Recognizing signs of an attack, such as many failed logins
- Building a simple dashboard and alert
- Documenting what I found, step by step

## Tools Used

- **Splunk Enterprise (free trial):** the SIEM
- **Splunk Universal Forwarder:** sends logs to Splunk
- **Sysmon:** records detailed Windows activity
- **Windows 10 VM:** the computer being monitored
- **Kali Linux VM:** the "attacker" computer used for testing
- **VirtualBox:** free software for running the virtual machines

> ⚠️ **Safety note:** All attacks were run only inside my own isolated lab. Never test against computers you don't own or have permission to test.

## Steps

### 1. Plan the Lab
![Network Diagram](https://placehold.co/900x450/7B2CBF/white?text=Network+Diagram)

*Ref 1: A simple map of the lab. The Windows VM sends logs to the Splunk server, and the Kali VM acts as the attacker. Everything runs on a private network inside VirtualBox.*

**Why:** Drawing the setup first helps you understand how the pieces connect.

### 2. Install Splunk
![Splunk Installation](https://placehold.co/900x450/0072b1/white?text=Splunk+Installation)

*Ref 2: Splunk installed and opened in a web browser at `http://localhost:8000`.*

**Why:** Splunk is where all the logs will end up, so it has to be set up first.

### 3. Tell Splunk to Receive Logs
![Data Inputs](https://placehold.co/900x450/7B2CBF/white?text=Receiving+Port+%26+Index+Setup)

*Ref 3: In Splunk, I turned on "receiving" on port 9997 and created an **index** (a storage folder for logs) named `endpoint`.*

**Why:** Splunk needs a "door" (port) to receive logs and a place (index) to store them.

### 4. Install the Forwarder and Sysmon on Windows
![Forwarder and Sysmon](https://placehold.co/900x450/0072b1/white?text=Forwarder+%2B+Sysmon+Setup)

*Ref 4: The Universal Forwarder and Sysmon installed on the Windows VM. I edited a settings file (`inputs.conf`) to choose which logs to send.*

**Why:** Sysmon records the activity, and the forwarder ships it to Splunk.

### 5. Check That Logs Are Arriving
![Log Ingestion](https://placehold.co/900x450/7B2CBF/white?text=Logs+Arriving+in+Splunk)

*Ref 5: Searching `index=endpoint` in Splunk shows events coming in from the Windows VM.*

**Why:** Always confirm data is flowing before you move on. If nothing shows up, check the IP address, the port and the firewall.

### 6. Simulate an Attack
![Attack Simulation](https://placehold.co/900x450/0072b1/white?text=Attack+Simulation)

*Ref 6: From Kali, I tried logging in to the Windows VM many times with wrong passwords. This is called a **brute-force attack**.*

**Why:** This creates realistic "bad" activity that we can practice finding.

### 7. Find the Attack in the Logs
![Log Analysis](https://placehold.co/900x450/7B2CBF/white?text=Log+Analysis+%E2%80%93+Event+ID+4625)

*Ref 7: Splunk shows many **Event ID 4625** (failed login) entries coming from the Kali machine's IP address.*

**Why:** Lots of failed logins in a short time from one source is a classic warning sign.

### 8. Build a Dashboard and Alert
![Dashboard](https://placehold.co/900x450/0072b1/white?text=Detection+Dashboard)

*Ref 8: A dashboard showing failed logins over time, plus an alert that triggers if there are more than 5 failed logins in 5 minutes.*

**Why:** Dashboards make problems easy to see, and alerts mean you don't have to watch the screen all day.

## Example Searches (Explained)

```spl
index=endpoint EventCode=4625
```
Shows every failed login.

```spl
index=endpoint EventCode=4625 | stats count by src_ip
```
Counts failed logins by the IP address they came from, so the attacker stands out.

## What I Learned

Before this project, "SIEM" was just a buzzword to me. Now I understand how logs travel from a computer to a SIEM, how to search them, and how a real attack shows up in the data. This gave me a strong foundation in log analysis and threat detection.

## Next Steps

- Try a different SIEM, such as Wazuh or Elastic
- Simulate more attack types with Atomic Red Team
- Map my detections to the MITRE ATT&CK framework
