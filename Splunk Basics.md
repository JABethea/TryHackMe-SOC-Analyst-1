# Splunk Basics— Writeup 

## 1) Introduction
Splunk is one of the leading SIEM solutions in the market. It allows users to collect, analyze, and correlate network and machine logs in real time. In this room, we will explore the basics of Splunk and its functionalities, and how it provides better visibility of network activities and helps speed up detection.
**Learning Objectives:**
* Understanding the components of Splunk
* Exploring some available options in Splunk
* Understanding log ingestion in Splunk
* Practically ingesting some Logs in Splunk and analyzing them


---

## 2) Connect with the Lab — description

**Description:**

Before proceeding with the following tasks, start the attached virtual machine by clicking the Start Machine below. 

The machine may take up to 3-5 minutes to start. After the machine starts, the Splunk Instance can be accessed at http:/MACHINE_IP either directly on the AttackBox or via the TryHackMe VPN. 

---

## 3) Why SIEM? — description

**Description:**
Splunk has three main components: Forwarder, Indexer, and Search Head

1.Splunk Forwarder: Splunk Forwarder is a lightweight agent installed on the endpoint intended to be monitored, and its main task is to collect the data and send it to the Splunk instance. It does not affect the endpoint’s performance as it takes a few resources to process. 
2. Splunk Indexer: Splunk Indexer plays the main role in processing the data it receives from forwarders. It parses and normalizes the data into field-value pairs, categorizes it, and stores the results as events, making the processed data easy to search and analyze.
3. Search Head: Splunk Search Head is the place within the Search & Reporting App where users can search the indexed logs, as shown below. The searches are done using the SPL (Search Processing Language), a powerful query language for searching indexed data. When the user performs a search, the request is sent to the indexer, and the relevant events are returned as field-value pairs.

**Q:** Which component is used to collect and send data over the Splunk instance?
**A:** `Forwarder`

---

## 4) Navigating Splunk — description

**Description:**
When you access Splunk, you will see the default home screen as shown below:
<img width="2102" height="786" alt="image" src="https://github.com/user-attachments/assets/99dc2064-8ca3-43bc-a996-80e672acfc50" />
**Splunk Bar**
<img width="1542" height="37" alt="image" src="https://github.com/user-attachments/assets/3bbf9633-d209-4a90-9f48-a5ae9ffd3fbc" />
**Apps Panel**
<img width="272" height="213" alt="image" src="https://github.com/user-attachments/assets/7093974c-fd8a-46b4-a8a4-d3d12dcdc690" />
**Explore Splunk**
<img width="1018" height="244" alt="image" src="https://github.com/user-attachments/assets/04643725-6906-4c1a-868c-47b99fedff5e" />
**Splunk Dashboard**
<img width="1548" height="660" alt="image" src="https://github.com/user-attachments/assets/e37d0fe2-c9ce-48a4-a644-b81fac18a32a" />

**Q:** In the Add Data tab, which option is used to collect data from files and ports?
**A:** `Monitor`

---

## 5) Adding Data - description

Splunk can ingest any data. According to the Splunk documentation, when data is added to Splunk, the data is processed and transformed into a series of individual events. The data sources can be event logs, website logs, firewall logs, etc. The data sources are grouped into categories.
For the practical exercise for this section on learning splunk the following instructions have been provided:
Download the log file VPN_logs from the Download Task Files button in the room and upload it to the Splunk instance we started in Task #2. If you are using the AttackBox, the log file is available in the /root/Rooms/SplunkBasic/ directory.

Press the upload method button:
<img width="1041" height="305" alt="image" src="https://github.com/user-attachments/assets/b346c891-d454-4171-9d66-5889d6096283" />
Navigate to the files you downloaded, or find them at /root/Rooms/SplunkBasic/ on the AttackBox. Then press next:
<img width="931" height="532" alt="image" src="https://github.com/user-attachments/assets/0cf3b336-d519-4ce4-9520-61fc98ccb7f5" />
Now you have to select the source type, which is JSON. Press next again.
<img width="1195" height="369" alt="image" src="https://github.com/user-attachments/assets/30454bbe-6567-43bb-8150-2f0bc83b84e1" />
Now we have to enter a host field value, which should be similar to the name of the machine (host). Let’s enter VPN_Connections here.
<img width="954" height="385" alt="image" src="https://github.com/user-attachments/assets/b3254eef-e562-4f73-beec-aab0b7ae6461" />
Now, create a new index called VPN_Logs. Press next, check everything and press Submit. Finally, press Start Searching.
Now you come in the search screen. To see the correct number of events I have to press the search button once.
<img width="1234" height="311" alt="image" src="https://github.com/user-attachments/assets/e0623a08-4099-4af7-bf86-175e6c75dfc3" />
The number of events is 2862.

How many log events are captured by the user Maleena? Find the Username field on the left menu, and press Maleena.
<img width="938" height="739" alt="image" src="https://github.com/user-attachments/assets/98137218-e92e-4184-94de-ab290f132328" />
Alternatively, you can enter the following in the search input field: source=”VPNlogs.json” host=”VPN_Connections” index=”vpn_logs” sourcetype=”_json” UserName=Maleena
The result is 60.

What is the username associated with IP 107.14.182.38?
Make sure you removed the filter on Maleena, and press the source ip field. Select any of the IPs to see how to edit the input field. Now you can edit the IP address to sort on.
source=”VPNlogs.json” host=”VPN_Connections” index=”vpn_logs” sourcetype=”_json” Source_ip=”107.14.182.38″
<img width="1084" height="640" alt="image" src="https://github.com/user-attachments/assets/0e827613-3692-441f-83c5-4eb575982711" />
Now you should see that the username associated to this IP address is Smith.

What is the number of events that originated from all countries except France?

The field we need to use here is called Source_Country.
Normally, if we were only interested in France we would use something like “Source_Country=France”, but to do the opposite we !=. The search field should look like this:
source=”VPNlogs.json” host=”VPN_Connections” index=”vpn_logs” sourcetype=”_json” Source_Country!=France

<img width="1217" height="333" alt="image" src="https://github.com/user-attachments/assets/b94f79b4-b191-4ebf-bce6-5f9d3648ec47" />

The answer is that there are 2814 events remaining

How many VPN events were associated with the IP 107.3.206.58?
Then we would just enter the specific SPL search: 
source=”VPNlogs.json” host=”VPN_Connections” index=”vpn_logs” sourcetype=”_json” Source_ip=”107.3.206.58″

<img width="1218" height="288" alt="image" src="https://github.com/user-attachments/assets/7d391582-aa01-4cd1-835f-44e48e995d7b" />

---

## 6) Conclusion - description
Some experience was gained practicing fundamentals and getting hands on with the Splunk SIEM. 
