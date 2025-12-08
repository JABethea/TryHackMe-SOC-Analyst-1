# Introduction to SIEM— Writeup 

## 1) Introduction
SIEM stands for Security Information and Event Management system. It is a tool that collects data from various endpoints/network devices across the network, stores them at a centralized place, and performs correlation on them.In this room, we will learn how different devices in a network generate logs and why it’s essential to have a centralized solution to collect, normalize, and correlate these logs.  
**Objectives:**

* Understand the different types of log sources

* Identify the limitations of working with isolated logs
  
* Recognize the importance of a SIEM solution

* Explore the features of a SIEM solution

* Learn various types of log sources and their ingestion in the SIEM
  
* Understand the process behind alerting and alert analysis

**Q:** What does SIEM stand for?
**A:** `Security Information and Event Management system`

---

## 2) Logs Everywhere,Answers Nowhere — description

**Description:**

1. Host-Centric Logs:
   * Capture events on individual devices (e.g., user login, file access, PowerShell execution).
   * Examples: Windows Event Logs, Sysmon, Osquery.
2. Network-Centric Logs:
   * Capture communication between devices or with the internet (e.g., SSH sessions, web traffic, VPN access).

**Q:** Is Registry-related activity host-centric or network-centric?
**A:** `host-centric`

**Q:** Is VPN-related activity host-centric or network-centric?
**A:** `network-centric`

---

## 3) Why SIEM? — description

**Description:**
**Key Features of SIEM**
1. Centralized Log Collection: collects logs from endpoints, servers, firewalls, etc. and centraliz3es them in one place via APIs or agents. Eliminates the need to manually access each machine.
2. Normalization of Logs: raw logs difffer in format, SIEM parse logs into fields and normalizes them into a consistent format, simplifies analysis across diverse sources.
3. Correlation of Logs: Links related events across multiple sources, helps detect malicious patterns, provides context that individual logs cannot
4. Real-time Alerting: Use built-in and custom detection rules, Triggers alerts when suspicious activity matches rule conditions, notifies analysts for immediate investigation
5. Dashboards and Reporting: Alert highlights, system notifications, health alerts, failed login attempts, events ingested count, rules triggered, top domains visited
6. Additional Features: Threat intelligence integration, Advanced search capabilities, other enhancements for mature security options

---

## 4) Log Sources and Ingestion — description

**Description:**
Every device on a network generates logs when activities occur—like website visits, SSH connections, or logins. These logs are essential for monitoring and detecting potential security threats.
**Common Devices and Their Logs:**
1. Windows Machines: Uses Event Viewer to store and view logs. Each event has a unique ID. Logs are forwarded to the SIEM for visibility and analysis.
2. Linux Workstations: Logs are stored in specific files, such as:
* /var/log/httpd: Web requests/responses and errors
* /var/log/cron: Cron job events
* /var/log/auth.log or /var/log/secure: Authentication logs
* /var/log/kern: Kernel-related events
4. Web Servers: Monitor incoming/outgoing traffic for attacks. Apache logs typically found in /var/log/apache or /var/log/httpd.

**Log Ingestion into SIEM**
1. Agent/Forwarder: Lightweight tool installed on endpoints to send logs to SIEM (e.g., Splunk Forwarder).
2. Syslog: Protocol used to send real-time logs from devices like servers and firewalls to SIEM.
3. Manual Upload: Some SIEMs support uploading offline data for analysis (e.g., Splunk, ELK).
4. Post Forwarding:SIEM listens on a port where devices send their log data.

**Q:** Which Event ID is generated when event logs are removed?
**A:** `104`

**Q:** What type of alert may require tuning?
**A:** `False Positive`

---

## 5) Alerting Process and Analysis — description

**Description:**
Correlation rules are logical expressions set by analysts to detect threats.
* 5 failed logins in 10 seconds → Multiple Failed Logins Alert
* Successful login after failed attempts → Brute Force Alert
* USB plugged in → USB Access Alert
* Outbound traffic > 25 MB → Potential Data Exfiltration

**Alert Investigation Workflow**
SOC Analysts use dashboards to monitor and investigate alerts:
1. Alert Triggered: Review associated events/flows. Check which rule condition was met.
2. Investigation Outcome: False Positive: Tune the rule. True Positive: Deepen investigation, Contact asset owner, Isolate affected host, Block suspicious IP


**Q:** C2 communications to a C2 server have to go through the network, so network connections are essential in identifying this.
**A:** `Network Connections`

**Q:** Configuration settings on Windows systems are stored in the registry. Changes in Windows registry are often tied to malicious activity.
**A:** `registry`

---

## 6) Lab Work — description
In the static lab attached, a sample dashboard and events are displayed. When a suspicious activity happens, an Alert is triggered, which means some events match the condition of some rule already configured. 

1. After clicking on the Start Suspicious Activity button, which process caused the alert?
   * View the static site. Start by pressing the Start Suspicious Activity button.
<img width="804" height="797" alt="image" src="https://github.com/user-attachments/assets/138a94cb-1ade-4415-8a1c-93ef78c05726" />
   * A process will start blinking in red:
<img width="782" height="710" alt="image" src="https://github.com/user-attachments/assets/f6b0891e-d8b6-49fd-b044-8bcd8b1894cd" />
   * The process is called cudominer.exe.

2. After clicking on the process, you will enter an event log and need to determine which user was responsible for executing the cudominer process. On the 4th row is the cudominer process, in the UserName column, it will see that the user who executed the process is Chris.
<img width="688" height="309" alt="image" src="https://github.com/user-attachments/assets/b0ea7b94-0785-4e55-9704-69e76e07a052" />
   * This user's HostName is HR_02.
   * In the event row, the Rule that triggered the event can be seen.
<img width="953" height="258" alt="image" src="https://github.com/user-attachments/assets/40b40195-39a8-4956-a5ce-d876395b1803" />
   * You can see that the rule checks is the process name includes miner or crypt. In this case, with the process called cudominer, the term that matched the rule is miner.
3. Go to Analysis / Action button and navigate to the action window. Then confirm if the rule is a true positive or a false positive. The process is most likely some kind of crypto mining process, so I will mark it to be a true posistive. 
<img width="981" height="301" alt="image" src="https://github.com/user-attachments/assets/a5cf9a0b-f0bf-49a7-b38f-614d786f1db9" />



## 7) Conclusion — description
The room covers SIEMs, their capabilities, and what visibility they provide. 
