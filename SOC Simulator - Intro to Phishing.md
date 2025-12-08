# SOC Simulator - Introduction to Phisihing — Writeup 

## Overview

**Goal:** 
This SOC simulation will be using TryHackMe's SOC Simulator to introducer users to Phishing Scenarios. The user selects a SIEM to simulate, I chose to use the Splunk simulation, since I already have experience with Elastic and would like to try out Splunk, as it is an industry standard. 
<img width="1133" height="938" alt="image" src="https://github.com/user-attachments/assets/306b4620-4b95-4a32-829d-277f5183e740" />

---

## Find all of the true positives

**Description:**

The scenarios are accessed trhough a menu screen with alerts that need addressing, there will be one alert pending at the beginning of the scenario. 
I will be addressing these alerts by #, although in a real life scenario these are to be addressed based on their Severity level instead of this organizational ease that I am taking in this repository. 
<img width="1536" height="955" alt="image" src="https://github.com/user-attachments/assets/09d9322b-3555-4bf6-9d8a-56d5bb4c6436" />
<img width="1536" height="542" alt="image" src="https://github.com/user-attachments/assets/0f2b289f-1b43-4c82-b779-2980ced37dcd" />


---

## 8814 - Inbound Email Containing a Suspicious External Link

**Description:**
The alert was triggered by an inbound email containing 1+ external link, possibly with suspicious characteristics. The firewall and proxy logs are needing to be checked to determine if any endpoints have been attempted to access the URLs in the email and whether or not the connections were allowed or blocked. 
<img width="1536" height="638" alt="image" src="https://github.com/user-attachments/assets/8b4a75b7-01dc-4e76-9ee6-f2c9c4b4a1c1" />

**datasource**: email
**timestamp**: 
**subject**: Action Required: Finalize Your Onboarding Profile
**sender**: onboarding@hrconnex.thm
**recipient**: j.garcia@thetrydaily.thm
**attachment**: none

**content**: Hi Ms. Garcia,\n\nWelcome to TheTryDaily!\n\nAs part of your onboarding, please complete your final profile setup so we can configure your access.\n\nKindly click the link below:\n\n<a href=”https://hrconnex.thm/onboarding/15400654060/j.garcia”>Set Up My Profile</a>.\n\nIf you have questions, please reach out to the HR Onboarding Team.

**direction**: In bound


I assigned the ticket to myself, then I wrote a case report, I checked the domain https://hrconnex.thm in the SIEM to see if the user clicked the link. This provides additional information of HR commmunications, with the following:
Hi IT Team,\n\nHope you’re doing well! I’m reaching out because one of our new hires, J. Garcia, hasn’t received her onboarding email from hrconnex.thm, which is our new third-party HR partner for handling account setup and paperwork.\n\nHe was supposed to receive a link to complete her profile and sign some documents, but it hasn’t come through — not in her inbox or spam. We’ve double-checked the email address on our end, and it’s correct.\n\nLet me know if you need any more info from my side.\n\n
<img width="1536" height="927" alt="image" src="https://github.com/user-attachments/assets/0fe9bd25-876e-4cfb-9605-35c1f1fe56c0" />

This indicates that this issue was because there is a 3rd party HR partner involved and that the alert is a False Positive. 


---

