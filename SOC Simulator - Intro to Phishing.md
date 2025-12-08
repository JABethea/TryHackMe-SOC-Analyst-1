# SOC Simulator - Introduction to Phisihing — Writeup 

## Overview

**Goal:** 
This SOC simulation will be using TryHackMe's SOC Simulator to introducer users to Phishing Scenarios. The user selects a SIEM to simulate, I chose to use the Splunk simulation, since I already have experience with Elastic and would like to try out Splunk, as it is an industry standard. 
<img width="1133" height="938" alt="image" src="https://github.com/user-attachments/assets/306b4620-4b95-4a32-829d-277f5183e740" />

---

## Find all of the true positives

**Description:**

The scenarios are accessed trhough a menu screen with alerts that need addressing, there will be one alert pending at the beginning of the scenario. 
In keeping with best practices, I will be addressing these issues in this SIEM by level of Severity, rather than their ID # or time of recipience. 
<img width="1536" height="955" alt="image" src="https://github.com/user-attachments/assets/09d9322b-3555-4bf6-9d8a-56d5bb4c6436" />
<img width="1536" height="542" alt="image" src="https://github.com/user-attachments/assets/0f2b289f-1b43-4c82-b779-2980ced37dcd" />


---

## 8816 - Access to a Blacklisted External URL Blocked by Firewall
**Description:**
This alert was triggered when a user attempted to access an external URL that is listed in the organization’s blacklist or threat intelligence feeds. The firewall or proxy successfully blocked the outbound request, preventing the connection. Note: The blacklist only covers known threats. It does not guarantee protection against new or unknown malicious domains.

**Timestamp**: 11/20/2025 20:04:23.489
**Action**: blocked
**Source IP**: 10.20.2.17
**Source Port**: 34257
**Destination IP**: 67.199.248.11
**Destination Port**: 80
**URL**: http://bit.ly/3sHkX3da12340
**Application**: web-browsing
**Protocol**: TCP
**Rule**: Blocked Websites


---

## 8814 - Inbound Email Containing a Suspicious External Link

**Description:**
The alert was triggered by an inbound email containing 1+ external link, possibly with suspicious characteristics. The firewall and proxy logs are needing to be checked to determine if any endpoints have been attempted to access the URLs in the email and whether or not the connections were allowed or blocked. 
<img width="3333" height="580" alt="image" src="https://github.com/user-attachments/assets/026e3e5d-64ff-48e4-a2a9-0c3f5375ccd3" />


**datasource**: email
**timestamp**: 12/8/25 4:58:03:032 PM
**subject**: Action Required: Finalize Your Onboarding Profile
**sender**: onboarding@hrconnex.thm
**recipient**: j.garcia@thetrydaily.thm
**attachment**: none

**content**: Hi Ms. Garcia,\n\nWelcome to TheTryDaily!\n\nAs part of your onboarding, please complete your final profile setup so we can configure your access.\n\nKindly click the link below:\n\n<a href=”https://hrconnex.thm/onboarding/15400654060/j.garcia”>Set Up My Profile</a>.\n\nIf you have questions, please reach out to the HR Onboarding Team.

**direction**: inbound


I assigned the ticket to myself, then I wrote a case report, I checked the domain https://hrconnex.thm in the SIEM to see if the user clicked the link. This provides additional information of HR commmunications, with the following:
Hi IT Team,\n\nHope you’re doing well! I’m reaching out because one of our new hires, J. Garcia, hasn’t received her onboarding email from hrconnex.thm, which is our new third-party HR partner for handling account setup and paperwork.\n\nHe was supposed to receive a link to complete her profile and sign some documents, but it hasn’t come through — not in her inbox or spam. We’ve double-checked the email address on our end, and it’s correct.\n\nLet me know if you need any more info from my side.\n\n
<img width="1536" height="927" alt="image" src="https://github.com/user-attachments/assets/0fe9bd25-876e-4cfb-9605-35c1f1fe56c0" />

This indicates that this issue was because there is a 3rd party HR partner involved and that the alert is a False Positive. 


---

## 8815 - Inbound Email Containing a Suspicious External Link

**Description**
This alert was triggered by an inbound email contains one or more external links due to potentially suspicious characteristics. As part of the investigation, check firewall or proxy logs to determine whether any endpoints have attempted to access the URLs in the email and whether those connections were allowed or blocked.

**datasource**: email
**timestamp**: 11/20/2025 20:03:09.489
**subject**: Your Amazon Package Couldn’t Be Delivered – Action Required
**sender**: urgents@amazon.biz
**recipient**: h.harris@thetrydaily.thm
**attachment**: none

content:

Dear Customer,\n\nWe were unable to deliver your package due to an incomplete address.\n\nPlease confirm your shipping information by clicking the link below:\n\nhttp://bit.ly/3sHkX3da12340\n\nIf we don’t hear from you within 48 hours, your package will be returned to sender.\n\nThank you,\n\nAmazon Delivery

**direction:** inbound

This event is similar to 8814, however it has a clearly suspicious email and the bit.ly shortlink also seems highly suspicious. Further investigation in the SIEM indicates that the user already clicked the link, but was blocked. 
<img width="819" height="457" alt="image" src="https://github.com/user-attachments/assets/856f9ed2-d0ea-47cc-a40e-6c6e7cd620ab" />
These indicators lead me to believe this incident is a True Positive. This incident does not require escalation. 
**Time of activity:** 11/24/25
**List of Affected Entities:** –-
**Reason for Classifying as True Positive:** Malicious site spoofing Amazon, suspicious external link blocked by firewall
**Reason for Escalating the Alert:** –-
**Recommended Remediation Actions:** Educate user directly about phishing emails and clicking links, possibly suggest further user training for phishing and other social engineering, and/or suggest future phishing campaigns internally to provide incites onto end user safety practices. 
**List of Attack Indicators:** Suspicious and unknown external source. 

---

## 8817 - Inbound Email Containing a Suspicious External Link

**Description**
This alert was triggered by an inbound email contains one or more external links due to potentially suspicious characteristics. As part of the investigation, I checked the firewall or proxy logs to determine whether any endpoints have attempted to access the URLs in the email and whether those connections were allowed or blocked.

**datasource**: email
**timestamp**: 11/20/2025 20:05:27.489
**subject**: Unusual Sign-In Activity on Your Microsoft Account
**sender**: no-reply@m1crosoftsupport.co
**recipient**: c.allen@thetrydaily.thm
**attachment**: none

**content:**
Hi C.Allen,\n\nWe detected an unusual sign-in attempt on your Microsoft account.\n\nLocation: Lagos, Nigeria\n\nIP Address: 102.89.222.143\n\nDate: 2025-01-24 06:42\n\nIf this was not you, please secure your account immediately to avoid unauthorized access.\n\n<a href=”https://m1crosoftsupport.co/login”>Review Activity</a>\n\nThank you,\n\nMicrosoft Account Security Team

**direction:** inbound
**Time of activity:**
Once more a phishing link. This time from a domain that looks a lot like Microsoft: no-reply@m1crosoftsupport.co. In addition, the email tries to panic the reader by writing that their Microsoft account is hacked from someone in Nigeria. Without a doubt a true positive.
**List of Affected Entities:** --
**Reason for Classifying as True Positive:** Definitely phishing. Microsoft pretending email and creating false feeling of urgency.
**Reason for Escalating the Alert:** Yes, domain should get blocked.
**Recommended Remediation Actions:** --
**List of Attack Indicators:** Emphasizes urgency or confidentiality: and contains suspicious attachments or links.

All the True Positives were identified, so the scenario was completed.

---



