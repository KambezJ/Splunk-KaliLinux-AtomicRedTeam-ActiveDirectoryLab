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
<img src="https://i.imgur.com/h9bqJqm.png" height="85%" width="85%" alt="Step 11"/>
</p>


<p align="center">
<b>Step 12: Next, head to the settings of each VM and ensure that they are all attached to our NAT Network that we just created in the previous step.</b> <br/>
<img src="https://i.imgur.com/HRQUUpC.png" height="85%" width="85%" alt="Step 12"/>
</p>


<p align="center">
<b>Step 13: Open Ubuntu VM, type "ip a". This shows IP, we need to modify this to reflect static IP on diagram. To fix this type "sudo nano etc netplan" and press tab to autofill. Copy the settings from the image below and apply.</b> <br/>
<img src="https://i.imgur.com/aWTWtLw.png" height="85%" width="85%" alt="Step 13"/>
</p>


<p align="center">
<b>Step 14: After the IP address is fixed, type "ip a" again and check that the changes have been applied successfully. To make sure I have internet connection I will ping google.com (you can press ctrl c to cancel).</b> <br/>
<img src="https://i.imgur.com/VN3rge5.png" height="85%" width="85%" alt="Step 14"/>
</p>


<p align="center">
<b>Step 15: Now we will need to download Splunk on this VM. On your host PC go to Splunk.com - products- free trials and downloads - install Splunk Enterprise for Linux, ensure you download the .deb file.</b> <br/>
<img src="https://i.imgur.com/LUapMGZ.png" height="85%" width="85%" alt="Step 15"/>
</p>


<p align="center">
<b>Step 16: Head back to the Ubuntu/Splunk VM and download the virtualbox guest additions using the following code:</b> <br/>
<img src="https://i.imgur.com/eTOBv1N.png" height="85%" width="85%" alt="Step 16"/>
</p>


<p align="center">
<b>Step 17: Go to top toolbar of the Ubuntu VM- devices- shared folders settings - add a new folder - and select the folder where Splunk was downloaded. Turn on the following options - readonly, automount, make permanent.</b> <br/>
<img src="https://i.imgur.com/EsplxUk.png" height="85%" width="85%" alt="Step 17"/>
</p>


<p align="center">
<b>Step 18: Type sudo reboot to restart the VM. Then type sudo adduser (your username) vboxsf. This is to add our user to the vbox sf group.</b> <br/>
<img src="https://i.imgur.com/a7OMQyY.png" height="85%" width="85%" alt="Step 18"/>
</p>


<p align="center">
<b>Step 19: Create a new directory called Share, then mount the folder from step 17 onto our directory called Share, using the provided code. Next, change to the newly created share directory.</b> <br/>
<img src="https://i.imgur.com/t7LarIq.png" height="85%" width="85%" alt="Step 19"/>
</p>


<p align="center">
<b>Step 20: Use the highlighted code to download and install Splunk. Then change into the directory where Splunk is located on the server using the second line of highlighted code.The third line of code (ls -la) displays  all of our files, including hidden ones, in the current directory.</b> <br/>
<img src="https://i.imgur.com/Oyq3eOZ.png" height="85%" width="85%" alt="Step 20"/>
</p>


<p align="center">
<b>Step 21: Change to the user named Splunk by inputing the first line of highlighted code, then change to the bin directory because thats where all the binaries are located for Splunk to use. Then continue with the installation.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/X2ZRyk1mWX9X/tMx7LqtmDCK5Uc41EQ8A7gAFLJW6Vp7SCdYG9zXE.png" height="85%" width="85%" alt="Step 21"/>
</p>


<p align="center">
<b>Step 22: Next, we will enter the following code that is highlighted to ensure that every time the VM is rebooted, Splunk will run with the correct user named Splunk.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/NaMLDYhf4gbV/L2SaSOkhXFsbFZHsowvPa6cnaU4COlcXhnon0MgE.png" height="85%" width="85%" alt="Step 22"/>
</p>


<p align="center">
<b>Step 23: Reboot the target VM, open command prompt, type "ipconfig" to check the IP address. You'll notice that the IP configuration does not match what is listed on our network architecture diagram. To change this, right click network on the bottom right corner of the toolbar- open network settings- change adapter options- right click on the adapter to open properties-double click IPV4 and configure the IP settings.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/JWsKlYB0kemm/kOuYxIfiTyq3QnWEABVbhX4TrKH0DW2TKMaLcYxG.png" height="85%" width="85%" alt="Step 23"/>
</p>


<p align="center">
<b>Step 24: On the same VM, open the web browser and we will try to access our Splunk server. Type in our IP 192.168.10.10:8000. We enter 8000 because that is the port that Splunk listens on. Once inside, log in using your account.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/7YEo9cuvwIPe/Ywp0gM2y2gd0o0qiNpMDHzQMKh2WbR0HRVvT5l34.png" height="85%" width="85%" alt="Step 24"/>
</p>


<p align="center">
<b>Step 25: In a new tab, open and sign into Splunk.com. Then go to products - free trials and downloads - and download Splunk Universal Forwarder. Continue with default settings, skip the deployment server step, and use 192.168.10.10 port 9997 for the receiving Indexer, and finish install.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/gi79bQV4K4O9/yYrQJ0odnBPK8np96CqtVLMUAD2obNoOAuGtXNcy.png" height="85%" width="85%" alt="Step 25"/>
</p>


<p align="center">
<b>Step 26: Next, we will download Sysmon. System Monitor (Sysmon) is a Windows system service and device driver that, once installed on a system, remains resident across system reboots to monitor and log system activity to the Windows event log. It provides detailed information about process creations, network connections, and changes to file creation time.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/1PAY3yvzd0BS/Gpn48yg31izUlEirjoKrW7c12HzGgrJorIvPfRxi.png" height="85%" width="85%" alt="Step 26"/>
</p>


<p align="center">
<b>Step 27: Now we will need to download the correct Sysmon configuration. Luckily this is provided to us on Github by a creator named OlafHartong. Download the raw code for the repository named sysmonconfig.xml and save it in your downloads folder.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/Zoo2CczmfXfb/x9EUjfkEAhqcgwKQxhsNaxw2FCZR4QV7CDXdnj5s.png" height="85%" width="85%" alt="Step 27"/>
</p>


<p align="center">
<b>Step 28: Open/run Powershell as an administrator, change directories to where Sysmon is downloaded, and copy the following code to access the correct file and install it</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/FRNYzrTzHYyM/KmbN2MDUqe7hHakpkk9WdIZhy4EjuenLussZa73N.png" height="85%" width="85%" alt="Step 28"/>
</p>


<p align="center">
<b>Step 29: Now we will need to create a inputs.conf file in the local directory of SplunkUniversalForwarder. This will instruct our Splunk Forwarder to push events related to Application, Security, System and Sysmon onto our Splunk Server. To achieve this, open notepad as an admin and copy the following code.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/mUePosd1ci88/G6UlKSechnOD2RU028NAGZ6oaWLIoF3jxL3igySI.png" height="85%" width="85%" alt="Step 29"/>
</p>


<p align="center">
<b>Step 29 (continued): Save it under the highlighted directory as inputs.conf and change the save as type to "All Files".</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/Tsujjvizwo6j/IpwVt1uZUmz89bxveHAF9KXBLx5fBuXGiR857SLW.png" height="85%" width="85%" alt="Step 29 continued"/>
</p>


<p align="center">
<b>Step 30: Open the services application as an administrator, find SplunkForwarder, double click to open, go to the Log On tab, and change the Log on setting to Local System Account. We will need to restart SplunkForwarder to apply the changes we made. Apply changes, then restart the service as instructed by the pop-up.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/CluoUCRaHgfB/RAgiFkrjrBcV1Lk7TzDEoY9BffS7bMHYQ2DjFyxU.png" height="85%" width="85%" alt="Step 30"/>
</p>


<p align="center">
<b></b> <br/>
<img src="" height="85%" width="85%" alt="Step 31"/>
</p>


<p align="center">
<b></b> <br/>
<img src="" height="85%" width="85%" alt="Step 32"/>
</p>


<p align="center">
<b></b> <br/>
<img src="" height="85%" width="85%" alt="Step 33"/>
</p>


<p align="center">
<b></b> <br/>
<img src="" height="85%" width="85%" alt="Step 34"/>
</p>


<p align="center">
<b></b> <br/>
<img src="" height="85%" width="85%" alt="Step 35"/>
</p>


<p align="center">
<b></b> <br/>
<img src="" height="85%" width="85%" alt="Step 36"/>
</p>


<p align="center">
<b></b> <br/>
<img src="" height="85%" width="85%" alt="Step 37"/>
</p>


<p align="center">
<b></b> <br/>
<img src="" height="85%" width="85%" alt="Step 38"/>
</p>


<p align="center">
<b></b> <br/>
<img src="" height="85%" width="85%" alt="Step 39"/>
</p>


<p align="center">
<b></b> <br/>
<img src="" height="85%" width="85%" alt="Step 40"/>
</p>
