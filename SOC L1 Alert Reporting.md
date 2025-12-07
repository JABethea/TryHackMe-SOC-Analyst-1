SOC L1 Alert Reporting — Writeup
Room overview
Goal: Learn how to properly report, escalate, and communicate about high-risk SOC alerts.

1) Introduction — description
Description: This section provides a brief introduction ot the concepts of alert funneling, reporting, escalation, and the fundamentals of SOC Communications. 

2)Alert Funneling — 
Description: As a continuation from Alert Triaging, Alert Funneling continues the process of event management in a SOC. This section stresses the importance of proper documentation and escalation of True Positives. 
Q: What is the process of passing suspicious alerts to an L2 analyst for review? A: Alert Escalation

Q: What is the process of formally describing alert details and findings? A: Alert Reporting

3) Reporting Guide — description
Description: A foundational guide for L1 SOC Analysts to provide a short and structured report for documentation and escalation, centring on the 5 W's.

Who — which user, service account, or process was involved?
What — the exact action or sequence of events (commands run, files downloaded, emails clicked).
When — precise start and end timestamps for the suspicious activity.
Where — which host, IP, cloud instance, or URL was involved.
Why — your reasoning for the final verdict (why you classified it as True/False Positive).

This is followed up with a simulated SOC Dashboard. 5 Alerts are presented and a scenario of data exfiltration requires identification and documentation in a report. 
<img width="2048" height="595" alt="image" src="https://github.com/user-attachments/assets/78e86b01-2a17-4a2e-bb50-76aa946e72a7" />
<img width="1536" height="369" alt="image" src="https://github.com/user-attachments/assets/f5c9255d-081f-469f-b852-bd10f4718170" />
<img width="1768" height="939" alt="image" src="https://github.com/user-attachments/assets/b385861c-612e-4bf3-8ec4-891a34f151fa" />


Q: According to the SOC dashboard, which user email leaked the sensitive document? A: m.boslan@tryhackme.thm
Q: Looking at the new alerts, who is the "sender" of the suspicious, likely phishing email? A: support@microsoft.com
Q: Open the phishing alert, read its details, and try to understand the activity.
Using the Five Ws template, what flag did you receive after writing a good report? A: THM{nice_attempt_faking_microsoft_support}

4) Escalation Guide — description
Description: After completing the alert report, the fundamental steps of escalation are given to alert to L2 when working in a SOC. 
When to escalate
Escalate the alert to L2 if any of the following apply:

The alert is an indicator of a major cyberattack requiring deeper investigation or DFIR.
Remediation actions are required (e.g., malware removal, host isolation, password reset).
Communication with customers, partners, management, or law enforcement is required.
You do not fully understand the alert and need help from more senior analysts.
Escalation steps
Reassign the alert to the L2 on shift and ping them in corporate chat or in person.
Note: Some teams require a formal written escalation request with additional fields; others do not.
Example: L1 escalates a phishing alert to L2, and L2 rotates the user’s credentials.
What L2 does next
L2 will receive the ticket from you, read your report, and contact you if they need clarification. Once clear, L2 typically:
Researches the alert details further and validates whether it is a True Positive.
Communicates with other departments if needed.
For major incidents, starts a formal Incident Response process.
Requesting L2 support
It is acceptable for L1 to request senior support when something is unclear. Especially during early months, it’s better to discuss the alert and clarify SOC procedures than to close an alert you don’t understand.

The practical exercise for this section on escalating has you as an L1 Analyst escalate the same alert from earlier to L2. 
<img width="1300" height="504" alt="image" src="https://github.com/user-attachments/assets/769e8291-c2fd-4fb4-9322-2d767ef69b33" />

Q:Who is your current L2 in the SOC dashboard that you can assign (escalate) the alerts to? A: E.Fleming
<img width="1536" height="358" alt="image" src="https://github.com/user-attachments/assets/f702a553-a716-4b3d-aaf4-da8d4fe12ece" />
Then investigate the new alert in the queue, write an alert comment, and then decide whether or not escalation is required. 
<img width="1536" height="358" alt="image" src="https://github.com/user-attachments/assets/72a12f47-a6d4-4852-b2df-d3c874bb8400" />
The invoked commands seem to be related to Active Directory reconaissance: whoami, netuser, Get-ADUser, nltest, net group "Domain Admins" /domain
These are enumeration commands often used by attackers for gaining a foothold. 
The parent process revshell.exe is suspicious and indicates a reverse shell executable. It is also in a public folder, commonly used for attack file drops.
The user: NT AUTHORITY\SYSTEM is running at teh highest privilege level, meaning the attacker has already achieved privilege escalation. 
The host is the mail server, meaning that the attacker has probably gained access through the host as they are usually internet-facing. 

Q: What flag did you receive after correctly escalating the alert from the previous task to L2? A: THM{good_job_escalating_your_first_alert}

The requirement for an alert comment after identifying this alert to be critical and proper escalation to be performed. The following is my write up based on the 5 W's for an alert comment on the escalation:
Who

Account: NT AUTHORITY\SYSTEM (executing commands) — highest privilege.

Processes: w3wp.exe (IIS worker) → revshell.exe (parent, located C:\Users\Public\revshell.exe) → cmd.exe (source process executing commands).

What: Observed commands indicative of AD/domain discovery and privilege enumeration:
whoami, net user, Get-ADUser (PowerShell/AD), nltest /dclist:tryhackme.thm, net group “Domain Admins” /domain, plus dir and hostname.

When: Detection occurred when the SIEM raised this alert (use the SIEM event/ticket timestamp).
Action for L1: capture and record the exact SIEM event timestamp and related log timestamps (IIS, Windows Security, Sysmon if available) as part of evidence collection.

Where: Host DMZ-MSEXCHANGE-2013 (Windows Server 2012 R2) — a mail server in the DMZ (likely internet-facing).
Files/process paths: C:\Users\Public\revshell.exe, C:\Windows\System32\cmd.exe, C:\Windows\System32\inetsrv\w3wp.exe.
Why / How (likely): Likely chain: attacker exploited the mail/Exchange-facing service (IIS) → uploaded or executed a web shell / reverse shell (revshell.exe) → spawned cmd.exe running AD enumeration commands as SYSTEM.

Q: Now, investigate the second new alert in the queue and provide a detailed alert comment.
Then, decide if you need to escalate this alert and move on according to the process.
A: THM{looks_like_webshell_via_old_exchange}

5) SOC Communication — description
Description: Further destail is provided on SOC procedures for communicating, crisis communication procedurs, and communication cases given.
Communication Cases
You need to escalate an urgent, critical alert, but L2 is unavailable and does not respond for 30 minutes.
Ensure you know where to find emergency contacts. First, try to call L2, then L3, and finally your manager.
The alert about Slack/Teams account compromise requires you to validate the login with the affected user.
Do not contact the user through the breached chat - use alternative contact methods like a phone call.
You receive an overwhelming number of alerts during a short period of time, some of which are critical.
Prioritise the alerts according to the workflow, but inform your L2 on shift about the situation.
After a few days, you realise that you misclassified the alert and likely missed a malicious action.
Immediately reach out to your L2 explaining your concerns. Threat actors can be silent for weeks before impact.
You can not complete the alert triage since the SIEM logs are not parsed correctly or are not searchable.
Do not skip the alert - investigate what you can and report the issue to your L2 on shift or SOC engineer.

Q: Should you first try to contact your manager in case of a critical threat (Yea/Nay)?
A: Nay
Q: Should you immediately contact your L2 if you think you missed the attack (Yea/Nay)?
A: Yea

6) Conclusion — description
Description:A brief congratulations on learning the importance of alert reporting, escalation, and communication. 

Reviewing SIEM/EDR alerts and tickets.
Performing initial enrichments (WHOIS, VirusTotal, process/parent checks).
Escalating validated incidents to L2/CIRT.
Updating the ticket with evidence and remediation suggestions.
Attending handover and daily standups.
Pro tip: produce reproducible steps for L2 so they don’t waste time redoing initial work.

