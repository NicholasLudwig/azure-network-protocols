<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
This repository explains how to observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create Virtual Machines in Microsoft Azure
- Observe traffic across various network protocols
- ICMP, SSH
- DHCP, DNS, RDP 

<h2>Actions and Observations</h2>

<p>
Begin by creating two virtual machines in Azure portal, one running Windows and one running Linux Ubuntu. Ensure they are running on the same Vnet by using Network Watcher.
</p>
<br />

![net-watch-top](https://github.com/NicholasLudwig/azure-network-protocols/assets/104456331/299a4955-c083-44dc-a69d-90dbf2f0eb7d)

<br />
<p>
Connect to the Windows virtual machine and download Wireshark which will allow us to observe traffic across various protocols. In Wireshark, filter traffic by ICMP by typing it in the bar at the top. Ping the Linux virtual machine's <b>private</b> IP address and observe the traffic in Wireshark. Initiate a perpetual ping to the Linux virtual machine in either Powershell or the Command Prompt by using "ping -t". Back in the Azure Portal, navigate to Network Security Groups > select the Linux security group > Inbound security rules > Add. Create a new rule with a high priority to deny any incoming traffic across ICMP. 
</p>
<br />
<p>
To do this, simply make sure the number for priority is lower than any existing rule. For example, a rule with a priority number of 200 will be executed before a rule with a priority number of 300. Back on the Windows virtual machine, observe in Wireshark that traffic is no longer getting through to our Linux virtual machine due to this new rule. Re-enable ICMP traffic by going back into the Linux network security group and either deleting the rule we just created or changing deny to allow. Observe that ICMP traffic is re-enabled in Wireshark. End the perpetual ping by pressing "ctrl+c" in either Powershell or Command Prompt.
</p>
<br />

![icmp-ping](https://github.com/NicholasLudwig/azure-network-protocols/assets/104456331/e1fceff5-9f32-4ddf-9d84-ee27cae13c29)![nsg-sec-rules](https://github.com/NicholasLudwig/azure-network-protocols/assets/104456331/0816f60b-a919-486a-b92e-5398208da7dc)



<br />
<p>
Observe SSH traffic by opening Powershell from the Windows virtual machine and entering "ssh [username]@[private IP address]". Continue connecting, enter the appropriate password and hit enter to connect. As a security feature, Linux will not display the password as you are typing it so make sure to type it correctly. Observe the traffic flow in Wireshark. Close the connection by typing "exit" and pressing enter.
</p>
<br />

![ssh-linux](https://github.com/NicholasLudwig/azure-network-protocols/assets/104456331/d57c3b65-df82-4aab-b9b4-736c6e9fc52a)

<br />
<p>
Observe DHCP traffic by filtering for DHCP in Wireshark then typing "ipconfig \renew" into the command prompt. Observe the traffic flow in Wireshark.
</p>
<p>
We can observe DNS traffic in a similar way. Filter for DNS traffic in Wireshark by typing it in the bar at the top. From the command prompt, enter "nslookup www.google.com". Do the same for Netflix. Observe DNS traffic flow in Wireshark.
</p>
<p>
Filter for RDP traffic by entering "tcp.port == 3389" (the port that uses remote desktop) and notice the nonstop spam of traffic. This is because we are currently using the Remote Desktop Protocol by being connected to our Windows virtual machine.
</p>
<br />

![dhcp-dns-traffic](https://github.com/NicholasLudwig/azure-network-protocols/assets/104456331/bbf70b99-9494-4b80-9081-9f22ce2ec0ca)

<br />
<p>
Having a foundational understanding of how traffic flows across various protocols can improve our ability to troubleshoot problems. Anything which improves our efficiency directly benefits customers and builds upon our own understanding and knowledge enabling us to provide better service.
</p>
