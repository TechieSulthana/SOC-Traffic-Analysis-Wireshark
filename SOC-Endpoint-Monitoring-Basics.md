# SOC Analyst Reference: Endpoint Monitoring & Log Analysis

## Executive Summary
This repository serves as a practical blueprint for entry-level Tier 1 SOC Analyst functions, focusing on endpoint security tracking across Windows and Linux operating systems. It documents critical Windows Event IDs and essential Linux command-line operations required to investigate system anomalies and unauthorized access attempts.

---

## 1. Windows Event Log Triage (Cheat Sheet)
When monitoring Windows enterprise endpoints through a SIEM, Tier 1 analysts must monitor these core Event IDs to track initial access, brute-forcing, and anti-forensics behaviors:

| Event ID | Event Name | SOC Triage & Analytical Context |
| :--- | :--- | :--- |
| **4624** | Successful Logon | Look closely at **Logon Type 10** (Remote Desktop / RDP). Unsuccessful or off-hours Type 10 logons flag potential compromised credentials. |
| **4625** | Failed Logon | A sudden spike (thousands of events within minutes) points to an automated **Brute-Force** or password-spraying attack. |
| **4688** | New Process Created | Monitors application execution. High alert if office applications (`winword.exe`) spawn command line binaries (`cmd.exe` or `powershell.exe`). |
| **1102** | Audit Log Cleared | A critical indicator of compromise (IOC). Threat actors clear logs to destroy forensic evidence and cover their tracks. |

---

## 2. Linux Log Analysis via Command Line
Many enterprise servers run on Linux. Below are the essential commands utilized to investigate systems and hunt for threats inside the `/var/log/` directory:

### Core Investigation Commands:
* `pwd` : Displays the exact directory path currently being audited.
* `ls -la` : Lists all contents in the directory. The `-la` flag is mandatory to uncover **hidden files** (files starting with `.`) which malware scripts frequently use to evade basic visibility.
* `cd /var/log` : Changes the directory to move directly into the central storage location for Linux system and authentication logs.
* `cat /etc/passwd` : Reads the system password file to audit all existing user accounts and detect unauthorized backdoor accounts created by attackers.
* `grep "Failed password" auth.log` : Parses mass text logs instantly to isolate malicious SSH brute-force attempts on Port 22 without manual scrolling.
