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

- Create Two Virtual Machines (Linux and Windows)
- Install Wireshark on Windows VM (Virtual Machine)
- Filter Traffic to Observe Different Protocols
- Delete or Pause Resource Groups to Save Money

<h2>Actions and Observations</h2>

<p align="center">
<img src="https://imgur.com/VGnkLWa.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create a Windows 10 virtual machine in Azure (2 vcpus) following the same steps in my previous presentations. This process is simple, you just set the image to Windows 10, agree to the agreement at the bottom of the Basics tab, then let the other settings default. Then, create another virtual machine with the image of Ubuntu 20.04. Once the virtual machines are created, log into the Windows VM and navigate to Wireshark(.)org and select 'Download.'
</p>
<br />

<p align="center">
<img src="https://imgur.com/83KE2He.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Select the 'Windows x64 Installer.'
</p>
<br />

<p align="center">
<img src="https://imgur.com/CewgKE2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
During the set up wizard, do not install the USBPcap but proceed with the installation.
</p>
<br />

<p align="center">
<img src="https://imgur.com/MzlS2eH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open Wireshark via the start menu (type this in if it does not automatically appear) and with 'Ethernet' highlighted, select start capture by pressing the shark fin under 'File' in the top left.
</p>
<br />

<p align="center">
<img src="https://imgur.com/jX8bKvA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
This looks overwhelming, but this shows all the traffic happening on the Windows' server/system
</p>
<br />

<p align="center">
<img src="https://imgur.com/lcrzluF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Enter 'icmp' in the prompt to filter the traffic. You can also enter things like DNS to filter for DNS traffic, or SSH, RDH, DNS, HTTP/S, etc
</p>
<br />

<p align="center">
<img src="https://imgur.com/J32J4tH.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Return to the Azure portal outside of the virtual machine. Expand the Linux/Ubuntu virtual machine and record the VM's Private IP Address
</p>
<br />

<p align="center">
<img src="https://imgur.com/AqzFCQX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Return to the Windows VM. On the start menu, open Windows PowerShell. With Wireshark open in the background to observe traffic, enter the prompt 'ping ' followed by Ubuntu's Private IP address. For me, and typically for you as well, this was 10.0.0.5, though this may vary so do not copy this as shown unless your VM's Private IP is the same as mine. Behind you while this prompt is being ran, you can see this traffic being processed
</p>
<br />

<p align="center">
<img src="https://imgur.com/Cl3Ig5q.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now, enter the prompt 'ipconfig /all'
</p>
<br />

<p align="center">
<img src="https://imgur.com/DyAWsWe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
You can see how the information between both virtual machines are being shown. Every process ran over a network has information like this being communicated behind the scenes. Wireshark basically gives you the ability to visualize this 
</p>
<br />

<p align="center">
<img src="https://imgur.com/jdE78YJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now on PowerShell, enter 'ping (enter your Ubuntu's Private IP address) -t' to initiate a continuous ping
</p>
<br />

<p align="center">
<img src="https://imgur.com/1ZUzq0b.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Return to Azure on your regular system and expand your Ubuntu's Network settings as shown above (Networking --> Network Settings). Click the 'Network security group' 
</p>
<br />

<p align="center">
<img src="https://imgur.com/z1G5p4p.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Under settings, expand 'Inbound security rules,' then select 'Add.' Copy the settings I configured: Source = Any, Source port ranges = * (this means all or any), Destination = Any, Service = Custom, Destination port ranges = *, Protocol = the last option or 'ICMPv4, Action = Deny, and set the Priority to any number less than the current lowest number priority. Now add this
</p>
<br />

<p align="center">
<img src="https://imgur.com/PZ3EY8s.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
When you return to the Windows virtual machine and observe PowerShell and Wireshark, you can see the process begins to 'Time out.' This is because the inbound security rule was set to block out all ICMP traffic (internet control message protocol, this shows information on how data is being transferred across a network).
</p>
<br />
