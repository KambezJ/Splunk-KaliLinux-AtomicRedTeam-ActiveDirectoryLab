<h1>Using Splunk SIEM tool to analyze attacks (by a Kali Linux VM) and vulnerabilities on a custom Active Directory Network</h1>


<h2>Description</h2>
In this lab we will set up an Active Directory Network with 4 VM's, using VirtualBox. The network will include 2 servers; a Splunk Enterprise Server running on Ubuntu, and a Windows Server housing the Active Directory. The Windows Server will act as our Domain Controller and have Splunk Universal Forwarder and Sysmon installed to generate telemetry. We will also create a Target VM running on Windows 10, join it to our domain, create domain users; and install Splunk Universal Forwarder, Sysmon and Atomic Red Team. The Target PC and Active Directory will then be attacked by a VM with Kali Linux installed, which will be pushing brute force attacks using crowbar and a password list. Atomic Red Team (ART) will be used in conjuction with the MITRE ATT&CK Framework to better understand attacks/vulnerabilities/threats and discover blind spots in detection by invoking various types of attacks with ART. The Splunk Enterprise dashboard will be used to query the telemtry generated across the network, and to learn how to identify and detect those attacks. By the end of this lab, you will gain a better understanding of building a logical diagram, basic networking, active directory configuration, virtual machines, Splunk, and vulnerablity/threat/attack detection. To better visualize how the network will function, you may refer to the diagram below.

<br />
<p align="center">
<img src="https://i.imgur.com/OkmLpyH.png?1" height="85%" width="85%" alt="Diagram of what this lab will achieve"/>
</p>


<h2>Tasks that will be performed:</h2>

- <b>Active Directory</b> Creation, Configuration and Administration.
    <br />
- Threat, Attack, Vulnerability detection and analysis using <b>Splunk Enterprise</b> and <b>MITRE ATT&CK Framework</b>.
    <br />
- Basic <b>Network Configuration</b>: IPv4, DNS, DHCP.
    <br />
- Creation of <b>Virtual Machines</b> using <b>Ubuntu, Kali Linux, Windows Server 2022, and Windows 10.</b>
    <br />
- Generation of telemetry using <b>Splunk Universal Forwarder</b> and <b>Sysmon</b>.
    <br />
- Automation of tasks using <b>Powershell, Command Prompt and BASH</b>.
    <br />
- Security Audits and Penetration tests using <b>Kali Linux and Atomic Red Team</b>.
    <br />
- File hash validation (SHA 256)


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b>
- <b>Command Prompt (Batch)</b>
- <b>Kali Linux Terminal (Bash)</b>
- <b>Oracle VirtualBox</b>
- <b>Server Manager</b>
- <b>Splunk Enterprise and Universal Forwarder</b>
- <b>Atomic Red Team</b>
- <b>Sysmon</b>


<h2>Environments Used</h2>

- <b>Windows 10 ISO:</b>
- <b>Windows Server 2022 ISO</b>
- <b>Kali Linux</b>
- <b>Ubuntu</b>
- <b>Oracle VirtualBox</b>


<h2>Program Walk-through</h2>

<p align="center">
<b>Step 1: Download Oracle VirtualBox and its associated Extension Pack. Before we download, open the SHA256 checksums link so we can ensure that the file has not be changed in transit</b> <br/>
<img src="https://i.imgur.com/n7taX3i.png" height="85%" width="85%" alt="Step 1"/>
</p>


<p align="center">
<b>Step 2: Open Powershell and validate the file hash to make sure it matches with the associated hash on the SHA256 page from step one. If it does not match, it most likely means the file was modified in transit and may be a threat.</b> <br/>
<img src="https://i.imgur.com/xLL5JmI.jpeg" height="85%" width="85%" alt="Step 2"/>
</p>


<p align="center">
<b>Step 3: Download Windows 10 ISO. When downloading, make sure you select Windows 10 pro, and continue with custom install. </b> <br/>
<img src="https://i.imgur.com/6sBPoq3.png" height="85%" width="85%" alt="Step 3"/>
</p>


<p align="center">
<b>Step 4: Create the first VM on Oracle VirtualBox and load the Windows 10 ISO .</b> <br/>
<img src="https://i.imgur.com/fOA2FpU.png" height="85%" width="85%" alt="Step 4"/>
</p>


<p align="center">
<b>Step 5: Download Kali Linux. Since we are using VirtualBox, we can download the pre built VM version specifically made for VirtualBox.</b> <br/>
<img src="https://i.imgur.com/8zba7Zg.png" height="85%" width="85%" alt="Step 5"/>
</p>


<p align="center">
<b>Step 6: Load Kali Linux onto VirtualBox as a seperate VM.</b> <br/>
<img src="https://i.imgur.com/xUYuVVg.png" height="85%" width="85%" alt="Step 6"/>
</p>


<p align="center">
<b>Step 7: Download the Windows Server 2022 ISO.</b> <br/>
<img src="https://i.imgur.com/TOaFQdN.png" height="85%" width="85%" alt="Step 7"/>
</p>


<p align="center">
<b>Step 8: Create a new VM and select Windows Server 2022 as the ISO. Make sure to select the Desktop Experience version, otherwise it will give us a CLI instead of a GUI, then continue with Custom Install.</b> <br/>
<img src="https://i.imgur.com/HjWpQIn.png" height="85%" width="85%" alt="Step 8"/>
</p>


<p align="center">
<b>Step 9: Go to ubuntu.com and download the latest version of Ubuntu Server. Load this ISO onto VirtualBox as the fourth VM. This is the VM that will run the Splunk SIEM. Continue with all default options.</b> <br/>
<img src="https://i.imgur.com/Bv2lQjN.png" height="85%" width="85%" alt="Step 9"/>
</p>


<p align="center">
<b>Step 10: On the Splunk/Ubuntu server we just created, enter the command "sudo apt-get update && sudo apt-get upgrade -y". This will update and upgrade all of our repositories.</b> <br/>
<img src="https://i.imgur.com/t0njHdL.png" height="85%" width="85%" alt="Step 10"/>
</p>


<p align="center">
<b>Step 11: Go to the VirtualBox manager, select tools, and create new NAT network. By doing this, our virtual machines will be on the same network but will still have internet access.</b> <br/>
<img src="" height="85%" width="85%" alt="Step 11"/>
</p>


<p align="center">
<b>Step 12: After reboot, navigate to Start - Windows Administrative Tools, and create a new Organizational Tool under the active directory dropdown menu called _ADMINS. Inside _ADMINS create a user for yourself and make it an admin.</b> <br/>
<img src="https://i.imgur.com/FEBcwIc.png" height="85%" width="85%" alt="Step 12"/>
</p>


<p align="center">
<b>Step 13: Go to Server Manager Dashboard - Add roles and features. In Server Roles tab enable Remote Access, continue to Role Services tab (under Web Server Role) and enable Routing. Continue to install.</b> <br/>
<img src="https://i.imgur.com/4S5agjD.png" height="85%" width="85%" alt="Step 13"/>
</p>


<p align="center">
<b>Step 14: Go to Server Manager Dashboard - Tools, and select Routing and Remote Access. Right click on DC - Configure and Enable Routing and Remote Access, select NAT option, then use the network interface that is the actual Internet, not internal(refer to step 9). Continue to finish set up.</b> <br/>
<img src="https://i.imgur.com/kf6xBhm.jpg" height="85%" width="85%" alt="Step 14"/>
</p>


<p align="center">
<b>Step 15: To begin setup of RDACP, first we set up DHCP by going to Server Manager Dashboard - Add roles and features - enable DHCP Server under Server Roles tab, and finish install.</b> <br/>
<img src="https://i.imgur.com/lmsvcs8.png" height="85%" width="85%" alt="Step 15"/>
</p>


<p align="center">
<b>Step 16: Next we will set up the Scope. On the Server Manager Dashboard, go to Tools - DHCP - right click IPv4 and select New Scope.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/v5MnQRghwXQg/dSGAwPAsPgGq3cCvUXuzBQtP5K7oIIWqGELVMYwV.png" height="85%" width="85%" alt="Step 16"/>
</p>


<p align="center">
<b>Step 17: Configure settings based on example info here and on the diagram in the description of this lab. For the next steps in the configuration - add no exclusions - use default settings for next 3 steps - enter DC IP address for next 2 steps, then continue to finish.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/e1N4KwfaU7Ek/eLTlWyAeYIp7m5hkitnezQRb3BimGpu7FUKLNDvO.png" height="85%" width="85%" alt="Step 17"/>
</p>


<p align="center">
<b>Step 18: Go to the notepad application and create 1: a list of random names (I generated 1000 random names); and 2: PowerShell script to create new users using the generated names.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/LZf9bTJnvN4D/R9t9ERdMuIbpqEZTh6wJbn6QBH4UVPeMQ0pg6JcQ.png" height="85%" width="85%" alt="Step 18"/>
</p>


<p align="center">
<b>Step 19: Open PowerShell ISE as admin, import and run the code you wrote to generate new users using the list of names from the previous step.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/NjqmatZ6wuxX/l99oaW1zvBTKo95aIu4H8IPOWlV4gKg6VTeZAL0a.png" height="85%" width="85%" alt="Step 19"/>
</p>


<p align="center">
<b>These are the results after running the script from the last step. Search for users in the Active Directory Users and Computers as a test, so we can ensure the script executed correctly.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/kp5sUuu7Wgzj/YBIYQT8aG8Ex5jH8QCLR9yRhLX5XHJ9ieE48WzzY.png" height="85%" width="85%" alt="Check Progress"/>
</p>


<p align="center">
<b>Now, lets review the diagram and see what is left in the Lab. The only thing left to do is create the CLIENT1 VM and connect it to the internal network.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/JPgtrFIjesv4/ufif9RuLlHwg5WHgObd9gAqJ5aZPIIAcxPV6Jqkj.jpg" height="85%" width="85%" alt="Check Progress"/>
</p>


<p align="center">
<b>Step 20: Navigate to VirtualBox, create new virtual machine and name it CLIENT1. In the network settings, make sure it is attached to Internal Network. This will act as a client computer and allow us to simulate a corporate network like used in a hospital, school, or office.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/9DO3GQHVeU1N/5idNK3twNlLDT2I1RHo9mvUkPCWEsZwDiaiQV8Bh.png" height="85%" width="85%" alt="Step 20"/>
</p>


<p align="center">
<b>Step 21: Run the new CLIENT1 VM, load up Windows 10 ISO, select Windows 10pro as OS, and finish configuration.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/Puar8HwgapxB/WW9d5XDW5SZZQK37DkLyo4mOObF59q8GQnpat1se.png" height="85%" width="85%" alt="Check Progress"/>
</p>


<p align="center">
<b>Step 22: Login to Client VM (using one of the usernames we generated previously on the DC VM) to ensure everything works correctly.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/QAbPqPjxVP21/hwbP8SGVoZ6BCAd65Cd3yXPeIDtR2xAONWhPTHhJ.jpg" height="85%" width="85%" alt="Check Progress"/>
</p>


<p align="center">
<b>Step 23: Run whoami command in Command Prompt as a final check to ensure the network functions as intended.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/z84sCx2gDuf3/h33p2rcQJuIihy6QrVPZsZrxyaOYws0ehIdyU274.jpg" height="85%" width="85%" alt="Check Progress"/>
</p>
