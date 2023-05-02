<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04


<h2>Actions and Observations</h2>

<p>
<img src="https://imgur.com/jW2qnlF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
First things first, we need to create our resource groups. Create a Windows 10 Virtual Machine and a Linux(Ubuntu) Virtual Machine and make sure they are both useing the same resource group. Observe your virtual network within Network Watcher
</p>
<br />

<p>
<img src="https://imgur.com/W6O6rzk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/OJnaEzz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, use Remote Desktop to connect to your Vindows 10 Virtual Machine. Within your Windows 10 Virtual Machine, download and install Wireshark from your web browser. 
</p> 
Go to this link: https://www.wireshark.org/download.html
</p>
Once that is done open Wireshark and click on Ethernet -> Start Capturing Packets. Observe the traffic.
</p>
<br />

<p>
<img src="https://imgur.com/6O3ghs0.png" height="500%" width="500%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/aeY59Ji.png" height="500%" width="500%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Wireshark, click the filter bar on the top and type ICMP. What this is doing is filtering for only ICMP traffic. Now what we are going to do is ping the Ubuntu VM from the Windows 10 VM, to do this open command line in the Windows 10 VM and type "ping" then whatever the Ubuntu VMs private IP address is. Observe the ping requests within wireshark. You can also ping a public website like Google by typing "ping www.google.com".
</p>
<br />

<p>
<img src="https://imgur.com/Z4JPwyY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/GlaP83A.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, we are going to initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM. Do this by typing "ping x.x.x.x -t" in command prompt. Now thats running, we are going to block ICMP traffic by changing the firewall settings on the Ubuntu VM. Go back to Azure portal and go to the Ubuntu VM -> Networking -> Add inbound port. Change the Protocol to ICMP, Action to Deny, and Priority lower than 300, then save. Then go back to the Windows 10 VM and you can now see that it is timing out(it may take a few seconds). Stop ping activity by pressing CTRL + C.
</p>
<br />


<p>
<img src="https://imgur.com/7j0Gndn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/jHaYZbV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Great! Now we are going to observe SSH traffic. Back in Wireshark, filter for SSH traffic only. From your Windows 10 VM, “SSH into” your Ubuntu Virtual Machine (via its private IP address). To do this go to command prompt and type "ssh labuser@x.x.x.x"(username@privateIP). Then type your password and you should be into the Ubuntu VMs command line. Type commands (username, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark
Exit the SSH connection by typing ‘exit’ and pressing [Enter].
</p>
<br />


<p>
<img src="https://imgur.com/WzEoKac.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/opp6oCV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To observe DHCP traffic in Wireshark, you need to filter for DHCP traffic only. Once you have filtered for DHCP traffic, attempt to issue your Windows 10 VM a new IP address from the command line using the "ipconfig /renew" command. As you do this, observe the DHCP traffic appearing in Wireshark. This will allow you to see the DHCP messages that are exchanged between the Windows 10 VM and the DHCP server as they negotiate a new IP address for the VM. 
</p>
<br />


<p>
<img src="https://imgur.com/7rdDMZa.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://imgur.com/KJRDHoF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
To observe DNS traffic, start by filtering for DNS traffic only in Wireshark. Next, use the nslookup command within a command line in your Windows 10 VM to see the IP addresses for both google.com and disney.com. With both of these steps completed, you can then observe the DNS traffic being shown in Wireshark. This will allow you to see how DNS queries and responses are being sent and received, giving you a better understanding of how DNS works and how traffic is flowing across your network.
</p>
<br />


<p>
<img src="https://imgur.com/DmasUGL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly, we are going to observe RDP traffic. In Wireshark type "tcp.port==3389" in the filter bar for RDP traffic only. Observe the immediate non-stop spam of traffic. It is constantly spamming because the RDP (protocol) is constantly showing you a live stream from one computer to another, therfor traffic is always being transmitted. Congratulations, you have now finished the tutorial!
</p>
<br />




