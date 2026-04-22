<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com)

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

First, I created two virtual machines - one with Windows 10 and another with Ubuntu. Using Remote Desktop, I connected to the Windows 10 VM to proceed with the lab. Inside the Windows 10 VM, I installed Wireshark and opened it along with Windows PowerShell. I initiated a continuous ping to the Ubuntu VM from the Windows 10 VM, monitoring the ICMP traffic in Wireshark. To observe the impact, I added an inbound rule in the Ubuntu VM's Network Security Group to block ICMP traffic, causing the pings to fail. Afterwards, I re-enabled ICMP traffic in the Ubuntu VM's Network Security Group, which restored the ping functionality. I then ended the ping activity. Next, I SSHed into the Ubuntu VM and observed the SSH traffic in Wireshark. After completing the observation, I exited the SSH session. In the Windows 10 VM, I attempted to obtain a new IP address by running the command "ipconfig /renew" and monitored the DHCP traffic in Wireshark. Using the nslookup command, I examined the DNS traffic in Wireshark. Lastly, I observed the ongoing RDP (Remote Desktop Protocol) traffic in Wireshark. As a final step, I deleted the Resource Groups in Azure.

<h2>Actions and Observations</h2>

![image](https://github.com/JaMyraJones/azure-network-protocols/assets/145633544/0cd23946-50ae-4291-99d1-6899a48b82c2)

To start this lab, I created two virtual machines - one with Windows 10 and the other with Ubuntu. It was essential to ensure that both VMs were in the same virtual network (vnet) and subnet. In this case, VM1 represented the Windows 10 VM, while VM2 represented the Ubuntu VM.
</p>
<br />

![image](https://github.com/JaMyraJones/azure-network-protocols/assets/145633544/f209ca36-36a9-40bd-a87f-9f07497a4b1b)

n the top right corner, there was a public IP address. I copied this address and used Remote Desktop to establish a connection with VM1 for the lab.
</p>
<br />

![image](https://github.com/JaMyraJones/azure-network-protocols/assets/145633544/e0c61448-9d72-4fa1-8d4c-ff2b1e8bb5f0)

Within VM1, I installed Wireshark using Microsoft Edge. After opening Wireshark and Windows PowerShell, I filtered the traffic to focus on ICMP (ping) packets. To perform the lab tasks, I initiated a continuous ping to VM2 using its private IP address. Each ping request and reply between VM1 and VM2 was captured by Wireshark, providing a clear view of the ICMP traffic.
</p>
<br />

![image](https://github.com/JaMyraJones/azure-network-protocols/assets/145633544/783a61a5-3229-4587-9776-dfb7f9b9c541)

Once I confirmed the successful ping, I accessed VM2's Network Security Group and added an inbound rule to block ICMP traffic. This configuration change effectively prevented VM2 from receiving ping requests.
</p>
<br />

![image](https://github.com/JaMyraJones/azure-network-protocols/assets/145633544/e3d73592-5da4-4b95-8212-28fde88a6a89)

As expected, after blocking ICMP traffic, the ping requests from VM1 to VM2 started to time out. Wireshark's "Info" section indicated that while VM1 continued to send requests, there were no responses from VM2, accompanied by a "no response found!" message.
</p>
<br />

![image](https://github.com/JaMyraJones/azure-network-protocols/assets/145633544/71bdf33b-18c6-437a-8b3d-9036b9257130)

I then went back to VM2's Network Security Group and selected the rule I created. By choosing the "Allow" option under "Action," I restored the ability for VM2 to receive incoming ICMP traffic.
</p>
<br />

![image](https://github.com/JaMyraJones/azure-network-protocols/assets/145633544/362fbcd4-1135-4ff0-8c4c-e5ea28b7a141)

Now that ICMP traffic was allowed again, both the PowerShell terminal and Wireshark showed that VM2 was sending reply packets. To conclude the ICMP observation, I ended the ping activity in PowerShell.
</p>
<br />

![image](https://github.com/JaMyraJones/azure-network-protocols/assets/145633544/73e3746f-51e7-416d-8e3f-f34e7ee4b1fc)

To examine DHCP traffic, I filtered Wireshark for DHCP packets and attempted to assign a new IP address to VM1 using the "ipconfig /renew" command. Although the private IP address remained unchanged, Wireshark captured the DHCP request and acknowledgement, indicating the generation of DHCP traffic.
</p>
<br />

![image](https://github.com/JaMyraJones/azure-network-protocols/assets/145633544/b696249b-5b04-4f68-a2a6-8e65b16f9b12)
Next, I filtered Wireshark for DNS traffic and executed the "nslookup" command for google.com. The terminal displayed both IPv4 and IPv6 addresses, while Wireshark's "Info" section revealed the presence of A and AAAA records corresponding to the IP address types.
</p>
<br />

![image](https://github.com/JaMyraJones/azure-network-protocols/assets/145633544/1f8c8f5a-e33a-4cab-ace3-53e0b98b4643)

Lastly, I utilized Wireshark to monitor RDP (Remote Desktop Protocol) traffic. Since RDP traffic was already being generated by my physical computer's connection to the VM, I did not need to rely on the PowerShell terminal. With continuous RDP traffic, Wireshark displayed each packet transmitted in real-time. While I typically filtered protocols by name in Wireshark, I decided to filter RDP traffic using its port number (tcp.port == 3389), ensuring accurate capture of the packets.


And that's a wrap! Capturing and analyzing packets is very interesting!


</p>
<br />
