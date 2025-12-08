# SOC Metrics and Objectives — Writeup 

## 1) Introduction

**Goal:*
Discover the concepts of SLA, MTTD, MTTA, and MTTR
Understand the importance of the False Positive rate
Learn why and how to improve the metrics as an L1 analyst
Practice with managing SOC team performance metrics


---

## 2) Core Metrics — Description

**Description:**
Overview of the primary goal of a SOC -  to protect the confidentiality, integrity, and availability of the organisation's digital assets. The SOC team performs its purpose by developing, receiving, and triaging alerts. The L1 analysts' role in this process is to reliably report True Positives to the higher level, to L2. 
Further detail is delivered on four SOC metrics: Alerts Count, False Positive Rate, Alert Escalation Rate, and Threat Detection RAte. Further specifics are delivered for each of the 4 metrics.
1. Alert Counts: 5 to 30 alerts per day as an L1 Analyst is a good metric.
2. False Positive: a rate of 0% is unachievable and 80% or higher is a serious problem.
3. Alert Escalation Rate: usually aimed to be below 50%, below 20% is even better.
4. Threat Detection Rate: The threat detection rate should always be at 100% since every missed threat can have devastating consequences.


**Q:** Is zero alerts for one month a good sign for your SOC team? (Yea/Nay)?
**A:** `Nay`

**Q:** What is the False Positive Rate if only 10 out of 50 alerts appear to be real threats?
**A:** `80%`

---

## 3) Triage Metrics — description

**Description:**
Purpose: Ensure threats are detected, acknowledged, and responded to quickly before attackers achieve their goals.
Definition: SLA is a formal agreement between the SOC team and management (or MSSP and customers) that sets performance expectations.
Key SLA Metrics
1. SOC Team Availability
   * Common SLA: 24/7 coverage (sometimes 8/5 for smaller teams).
   * Ensures analysts are always available to handle alerts.
2. Mean Time to Detect (MTTD)
   * SLA: 5 minutes.
   * Measures how quickly SOC tools detect an attack after it begins.
3. Mean Time to Acknowledge (MTTA)
   * SLA: 10 minutes.
   * Measures how quickly L1 analysts begin triaging a new alert.
4. Mean Time to Respond (MTTR)
   * SLA: 60 minutes.
   * Measures how quickly SOC takes action to stop the breach (e.g., isolate device, secure account).

**Q:**Imagine a scenario where the SOC team receives a critical alert on Saturday.
If the team works 8/5, on which day of the week will they acknowledge the alert?
**A:** `Monday`

**Q:**Imagine a scenario where an employee was lured into running data stealer malware.
  1. The SOC team received the "Connection to Redline Stealer C2" alert after 12 minutes.
  2. One of the L1 analysts on shift moved the alert to In Progress 10 minutes later.
  3. After 6 minutes, the alert was escalated to L2, who spent 35 minutes cleaning the malware.
Provide the MTTD, MTTA, and MTTR via comma as your answer (e.g. 10,20,30).
**A:** `12,10,51`

---

## 4) Improving Metrics — description

**Description:**

**Q:** Which process is aimed at preventing or reducing the chance of an attack?
**A:** `CVE-2025-53770`

**Q:** How would you respond to a detected vulnerability on your system?
**A:** `Patch`

---

## 4) Misconfigurations — description
**Description:**
False Positive Rate over 80%	Your team receives too much noise in the alerts. Try to:
   1. Exclude trusted activities like system updates from your EDR or SIEM detection rules
   2. Consider automating alert triage for most common alerts using SOAR or custom scripts
Mean Time to Detect over 30 min	Your team detects a threat with a high delay. Try to:
   1. Contact SOC engineers to make the detection rules run faster or with a higher rate
   2. Check if SIEM logs are collected in real-time, without a 10-minute delay
Mean Time to Acknowledge over 30 min	L1 analysts start alert triage with a high delay. Try to:
   1. Ensure the analysts are notified in real-time when a new alert appears
   2. Try to evenly distribute alerts in the queue between the analysts on shift
Mean Time to Respond over 4 hours	SOC team can’t stop the breach in time. Try to:
   1. As L1, make everything possible to quickly escalate the threats to L2
   2. Ensure your team has documented what to do during different attack scenarios

**Q:**What is the highest acceptable False Positive Rate for SOC teams?
**A:** `80%`
**Q:**Should all SOC roles work together to keep metrics improving? (Yea/Nay)
**A:** `Yea`

---

## 5) Practice Scenarios — description
**Description:**
A practical scenario of matching the correct information to the Problematic Metric, Improvement Task, and proper Assignee to the Task

*First Scenario: Unhappy Customer*
Problematic Metric: Time to respond was to high, too much time spend to contain the attack. This is the real problem. The alert got solved, but the team took way to much time resolving it.
Improvement Task: Create a workbook explaining credential rotation steps, and present it to the team. Creating a workbook will make future events much quicker to solve.
Assign Task To: Assign the research and workbook creation task to the L2 that handled the incident. The L2 that handled the incident took a lot of time to find out the proper procedure. He should be the one writing the workbook.
Flag: THM{mttr:quick_start_but_slow_response}
*Second Scenario: Delayed Alert*
Problematic Metric: Time to Detect of 20 minutes led to a delayed alert triage. This is a unnecessary problem, since we had the necessary manpower to solve the attack earlier.
Improvement Task: Tune the SIEM and the detection rules to run more often, every 5 minutes. Since the attack got catched by the SIEM there is nothing wrong with its rules. But the rules should run more often.
Assign Task To: Assign the detection rules’ schedule review to the dedicated SOC engineer. We should let a SOC engineer look at the schedules!
Flag: THM{mttd:time_between_attack_and_alert}
*Third Scenario: Tired Analyst*
Problematic Metric: False Positive Rate is the core of the problem. The analysts are getting flooded by false postives, and they have trouble remaining vigilant.
Improvement Task: Schedule a call with the team to implement the False Positive remediation process. It is important to talk to the team to find out what the real challenges are, before talking with engineers.
Assign Task To: Assign the task to SOC engineers to exclude the system and IT noise from the rules. Now that we have talked with the team we can discuss with the engineers how to reduce the number of false positives by looking at reducing system and IT noise from the rules.
Flag: THM{fpr:the_main_cause_of_l1_burnout}

---

## 6) Conclusion — description

**Description:**
An excellent provision of SOC Metrics and Objectives to help new users understand and get hands on practical experience with scenario based metrics. 
