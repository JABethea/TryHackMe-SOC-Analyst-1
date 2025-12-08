SOC L1 Workbooks and Lookups — Writeup Room overview Goal: Become familiar with SOC investigation workbooks, SOC asset inventory usage, understanding corporate network diagrams, and practice workflow building.

1) Introduction — description Description: This section provides a brief introduction ot the concepts of alert funneling, reporting, escalation, and the fundamentals of SOC Communications.

2) Assests & Identities — Description: Key concepts in IAM (Identity & Access Management) are covered, with different examples of Identity and Asset Invtories provided.

Q: Looking at the identity inventory, what is the role of R.Lund at the company? A: US Financial Adviser
Q: Checking the asset inventory, what data does the HQ-FINFS-02 server store? A: Financial Records
Q: Finally, does the file sharing from the scenario look legitimate and expected? (Yea/Nay) A: Yea

3) Network Diagrams - Description: Further discussion on identity and asset inventory topics are provided, with further detail on firewall logs and IP adresses. Network diagrams and the role they play in visualizing the security permiter around network assets and how this is handled in a SOC are the core features of this section.
Example Setup:
Firewall exposes VPN service on port 10443 and web services on HTTP.
Firewall protects three subnets:
VPN subnet → 10.10.0.0/16
Database subnet → 172.16.15.0/24
Office subnet → 172.16.23.0/24
Network diagrams allow SOC analysts to: Identify exposed services, understand subnet roles, reconstruct attack paths and see how threat actors traverse the network. 

Q: According to the network diagram, which service is exposed on the TCP/10443 port? A: VPN
Q: Now, which subnet would the server behind 172.16.15.99 IP belong to? A: Database Subnet
Q: Finally, does the scenario look like a True Positive (TP) or False Positive (FP)? A: TP

4) Workbooks Theory - Description: SOC Workbooks AKA Playbooks are defined as structured documents that outline exact steps analysts must follow to investigate and remediate threats. Their purpose is to ensure consistency, reduce mistakes, and streamline analysis.
   It is the role of the senior analysts and engineers to prepare workbooks so that Lower level analysts can triage alerts correctly without missing vital details.
   Practical example of a workbook is provided for an Unusual Login Location Scenario:
   Flow: From receiving a login alert → identity enrichment → threat intelligence checks → SIEM investigation → escalation if needed.
Divided into three logical groups:
Enrichment → Gather context using identity inventory and threat intel.
Investigation → Analyze SIEM logs, user behavior, and IP details to decide if login is expected.
Escalation → Escalate to L2 analysts or confirm with the user if suspicious.
   The workbook's role is to ensure that analysts don't skip critical steps and deliver consistent, high-quality triage and avoid premature or incorrect verdicts.
   
Q: Which SOC role would use workbooks the most (e.g. SOC Manager)? A: SOC L1 Analyst
Q: What is the process of gathering user, host, or IP context using TI and lookups? A: Enrichment
Q: Looking at the workbook example, what platform is used as an identity inventory source? A: BambooHR

5) Workbooks Practice - Description: A practical scenario is presented where the user matches the correct information in the boxes to properly itereate through the workbook. 
The 3 Workbooks are for Email Analysis, PowerShell Analysis, and Network Analysis
1. <img width="1107" height="859" alt="image" src="https://github.com/user-attachments/assets/5324965e-9935-41f3-8d4c-42b0503ca49d" />
2. <img width="2048" height="1628" alt="image" src="https://github.com/user-attachments/assets/4e70c4c7-8913-41c5-aced-ac140e93430d" />
3. <img width="2038" height="1670" alt="image" src="https://github.com/user-attachments/assets/65c7ae04-5443-4946-b547-8da3ed8a2280" />
The Flags awarded for completing the workbooks correctly are: THM{the_most_common_soc_workbook} THM{be_vigilant_with_powershell} and THM{asset_inventory_is_essential}

7) Conclusion - Description: Building workbooks through a series of guided process completion for given scenarios was a very straightforward application of the information covered in the lesson. 
