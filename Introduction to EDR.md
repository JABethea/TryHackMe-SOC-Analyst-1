# SOC Alerts Triage— Writeup 

## 1) Introduction
Endpoint Detection and Response (EDR) is a security solution designed to monitor, detect, and respond to advanced threats at the endpoint level. As a SOC analyst, it is essential for you to understand how the EDR works since it is a widely adopted solution in organizations to protect their endpoints. In this room, we will see how an EDR differs from a traditional antivirus and what data it collects from the endpoints. We will also discuss the detection and response capabilities it provides.

**Objectives:**

* Understand the basics of EDR and how it works

* Differentiate EDR from traditional Antivirus solutions

* Examine the architecture of an EDR solution

* Analyze the types of telemetry it collects from endpoints

* Understand the detection and response capabilities of an EDR

* Investigate a realistic alert in the EDR



---

## 2) What is an EDR? — description

**Description:**

A brief overview of the Endpoint Detection and Response (EDR) solutions and their provision of continuous monitoring and protection for endpoint devices. EDRs detect, investigate, and respond to threats in real time. 

Popular EDR Solutions: CrowdStrike Falcon, SentinelOne ActiveEDR, Microsoft Defender for Endpoint, OpenEDR, Symantec EDR

3 Pillars of EDR:
1. Visibility
2. Detection
3. Response

**Q:** Which feature of EDR provides a complete context for all the detections?
**A:** `Visibility`

**Q:**Which process spawned sc.exe?
**A:** `cmd.exe`

---

## 3) Beyond the Antivirus — description

**Description:**
Further explanation is provided on the functions and details of how an alert woudl appear through the practical example of the THM built in SIEM tool. 


**Q:**In the given analogy, what presents an AV?
**A:** `immigration check`

**Q:**Which legitimate process was hijacked by the attacker in the scenario?
**A:** `svchost.exe`

**Q:**Which security solution might mark this activity as clean?
**A:** `antivirus`

---

## 4) Beyond the Antivirus — description

**Description:**
**How EDR Works Behind the Scenes**
EDR Agents
* Deployed on endpoints as sensors.
* Monitor all activities and send detailed data to the central console in real time.
* Perform basic signature and behavior-based threat detection.
EDR Console
* Centralized dashboard that receives and analyzes data from agents.
* Uses machine learning and threat intelligence to correlate events and generate alerts.
* Provides a holistic view of endpoint security status.
What Happens After Detection
* SOC analysts review alerts and prioritize based on severity (Critical to Informational).
* Analysts investigate alerts using detailed logs: file executions, processes, network activity, registry changes.
* If confirmed as a true threat, analysts can take direct action from the console to contain or remediate.
EDR in the Broader Security Ecosystem
* EDR works alongside other tools like Firewalls, DLPs, Email Security Gateways, and IAMs.
* All tools feed into a SIEM (Security Information and Event Management) system for centralized analysis and response.

**Q:** Which component of the EDR is responsible for collecting telemetry from the endpoints?
**A:** `Agent`

**Q:** An EDR agent is also known as a?
**A:** `sensor`


---

## 5) EDR Telemetry — description

**Description:**
* Telemetry: is the detailed data collected by EDR agents from endpoints, functioning like a blackbox: capturing everything needed for threat detection and investigation.
* Types of Collected Telemetry: Process Executions and Terminations, Network Connections, Command Line Activity, Files and Folders Modifications, Registry Modifications
* Why Telemetry Matters: Helps to distinguish between legitimate and malicious activity, enables machine learning and threat intelligence, and reconstructing attack timelines.

**Q:** C2 communications to a C2 server have to go through the network, so network connections are essential in identifying this.
**A:** `Network Connections`

**Q:** Configuration settings on Windows systems are stored in the registry. Changes in Windows registry are often tied to malicious activity.
**A:** `registry`

---

## 6) Detection and Response Capabilities — description

**Description:**
**Detection Techniques in EDR**
* Behavioral Detection: Flags suspicious behavior patterns, like Word spawning PowerShell, even if files appear clean.
* Anomaly Detection: Identifies deviations from normal endpoint behavior, helping spot stealthy threats.
* IOC Matching: Uses threat intelligence feeds to detect known malicious indicators like file hashes.
* MITRE ATT&CK Mapping: Aligns flagged activities with tactics and techniques for better threat context.
* Machine Learning Algorithms: Detects complex, multi-stage attacks by analyzing patterns across large datasets.

**Response Capabilities in EDR**
* Isolate Host: Disconnects infected endpoints from the network to prevent lateral movement.
* Terminate Process: Stops malicious processes without isolating the host, preserving business operations.
* Quarantine: Moves suspicious files to a safe location for review or removal.
* Remote Access: Enables analysts to access endpoint shells for deeper investigation and custom actions.
* Artefacts Collection: Gathers forensic data like memory dumps, event logs, and registry hives remotely.

**Q:** Which feature of the EDR helps you identify threats based on known malicious behaviours? 
**A:** `IOC Matching`

---

## 7) Investigate an alert on EDR — description
Another practical example is provided in the SOC simulator, this time operating in the EDR dashboard. This hands on practical exercise allows you to get hands on experience working in a simulated EDR and identify/handle threats.

<img width="1751" height="1773" alt="image" src="https://github.com/user-attachments/assets/53091fab-6ddb-40e5-9691-592ccb40b7b1" />

1. From this dashboard we can select the first High Severity threat, currently affecting the Host DESKTOP-HR01
<img width="1746" height="1558" alt="image" src="https://github.com/user-attachments/assets/fd5210c3-21e9-469f-ac36-4031318a5d8e" />
The malicious process can be identified in the Process info tab, determining that CURL.exe was the tool launched by CMD.exe to download the payload.
<img width="1732" height="1132" alt="image" src="https://github.com/user-attachments/assets/f5716609-cf9d-47f4-8afe-5753edaf6bf0" />
The payload being downloaded by cURL can also be determined with the absolute path from the commandline command
<img width="1218" height="123" alt="image" src="https://github.com/user-attachments/assets/048a5c6c-bfaa-4609-9bdf-bc7494881b28" />

2. The next incident is concerning Credential Dumping via LSASS Memory Access to WIN-ENG-LAPTOP03
<img width="1757" height="1279" alt="image" src="https://github.com/user-attachments/assets/2cfc9f86-0f7c-4a29-820d-bdfb0e922067" />
Once again in the process info tab, the answer is found under the path when looking at the syncsc.exe process
<img width="1734" height="1480" alt="image" src="https://github.com/user-attachments/assets/325979b4-7915-4614-b042-9063ce71264a" />
The URL that the exfiltration attempt being made on WIN-ENG-LAPTOP03
The answer is in the Network Activity portion of the EDR dashbaord:
<img width="1222" height="134" alt="image" src="https://github.com/user-attachments/assets/0b2863d5-4b2d-4ee2-ab39-24400ecf4373" />

3. The next incident is regarding the UpdateAgent.exe labelled by Threat Intel on DESKTOP-DEV01. Changing the detection Execution from AppData Directory on DESKTOP-DEV01. Enter the same Process Info tab and find the UpdateAgent.exe
<img width="1731" height="1170" alt="image" src="https://github.com/user-attachments/assets/efcccea7-1250-4890-8e09-1543af0451b6" />
In this section we find that the Threat Intel section mentions: Known internal IT utility Tool

---
## 7) Conclusion — description
This section provides a great first look at EDRs and a valuable hands on exercise in using a simulated EDR to investigate alerts and navigate information on these potential threat alerts
