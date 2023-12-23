<p align="center">
<img src="https://imgur.com/Hi8FH7s.png" alt="Azure Sentinel"/>
</p>

<h1>Utilizing Azure Sentinel to create a HoneyPot</h1>
In this tutorial, we set up an Azure Virtual Machine with a weak security posture to watch Brute Force Attacks in real time <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure Virual Machine
- Azure Network Security Groups (NSG)
- Azure Sentinel
- Azure Log Analytics
- Remote Desktop
- Powershell
- Kusto Query Language

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)

<h2>High-Level Steps</h2>

- We created a Resource Group with an Azure Virtual Machine and set up an incredibly weak Security Posture via the Azure Firewall and internal Windows Defender
- We set up an Azure Log Analytics Workspace connected to the Azure Virtual Machine to record logging data
- We set up Azure Sentinel to work in tandem with the Azure Log Analytics to map out real time attacks

<h2>Actions and Observations</h2>

<p>
<strong>Network Security Group</strong> </p>
<img src="https://imgur.com/F1jX8qw.png" height="80%" width="80%" alt="NSG Policy"/>
<p><strong>Windows Defender Firewall</strong></p>
<img src="https://imgur.com/GONGDa6.png" height="80%" width="80%" alt="Windows Defender Policy"/>
</p>
<p>
The first thing we did once we got the Azure Virtual Machine up and running with a Windows 10 Operating System was force it to be as vulnerable as possible. To do this we set up the attached Network Security Group to allow traffic to any port and have this NSG completely open to the entire internet. The Second step was to log into the actual VM itself and completely shut off the Windows Defender Firewall Policy. You would still need a password to log into Remote Desktop Protocol, but the assumption would be that if the IP Address can reply to an ICMP echo request, people would try to Brute Force Remote Desktop Protocol.
</p>
<br />

<p>
<img src="https://imgur.com/ZRBZNQN.png" height="80%" width="80%" alt="siem API"/>
</p>
<p>
The next step was to utilize a Powershell Script within Powershell ISE that I found on the internet to document failed RDP login attempts. I utilized an API Key from https://ipgeolocation.io/ to use the IP Address to find the latitude and longitude of the IP Addresses that were attacking my vulnerable VM. This script takes the IP Address of the attacker, finds their Geolocation and creates a log file within the VM called failed_rdp.logs I believe in full transparency and I did find this script from Youtube/Github. I do not know Powershell.

<img src="https://imgur.com/QVGl5oR.png" height="80%" width="80%" alt="logs"/>

</p>
<br />
<p>
We then utilized Kusto Query Language to parse the data after setting it up within the Azure Log Analytics to check for country, timestamp, longitude, latitude, and IP Address. I do not know Kusto Query Language, this was something that I had to research in order to make sure that the data was parsed properly for later use. 
<img src="https://imgur.com/uI73K7v.png" height="80%" width="80%" alt="KQL"/>
</p>
<p>
The final step was to utilize this parsed data and set up a Microsoft Azure Sentinel Workbook. We utilized the geolocation data from ealier to create a map of these real time attacks that were happenning around the world. We only tested the failed RDP attempts with the assumption that there are attackers all around the world who are sending ping to IP Address ranges in order to see waht is open to the internet. From the attacker's point of view, if they are receiving ICMP Echo Replies, they are most likely running an NMAP scan on the IP Address, and seeing that port 3389 (among the rest of the ports) are currently open. They seem to be utilizing some sort of Brute Force attacking script to try any available passwords to gain access to the Honey Pot that I have set up within Azure. I would like to continue this project to take in various other types of logs to pay attention to other potential attack methods at a later time. 
<img src="https://imgur.com/BTKPFGV.png" height="80%" width="80%" alt="Map"/>
</p>
<br />
