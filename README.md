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
- Automation of tasks using <b>Powershell, Batch and BASH</b>.
    <br />
- Security Auditing and Penetration testing using <b>Kali Linux and Atomic Red Team</b>.
    <br />
- File hash validation <b>(SHA 256)</b>


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

- <b>Windows 10 ISO:</b> Target PC/VM
- <b>Windows Server 2022 ISO:</b> Active Directory Domain Controller
- <b>Kali Linux:</b> Attacking PC/VM
- <b>Ubuntu:</b> Splunk Server
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
<b>Step 31: In step 29 we inputed code that will save all events under an index named endpoint. Currently, no such index exists on our Splunk Server, so we will have to add it. To do so, open the server on the Windows 10 VM and log in. Head to settings, Indexes ( under data) create a new index named endpoint with default settings, and save. Now our SplunkForwarder will be able to send all event information to this index.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/OcWaHnWzpyeZ/6V056Mm9Pfdxrd7F5loGDpL0QRuMg9DHHjfWjhK0.png" height="85%" width="85%" alt="Step 31"/>
</p>


<p align="center">
<b>Step 32: Now we have to make sure that the Server is correctly configured to receive data. To do so go to settings, Forwarding and Receiving (under data), Configure receiving, and add the port 9997. Port 9997 is the port that Splunk forwarder and Splunk Indexer use to communicate and transmit data/information.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/bwXcTz7Zqcfy/njGxj83RqFlAcGd0oQjcxZtmzaiU8SwlpmMwXk3S.png" height="85%" width="85%" alt="Step 32"/>
</p>


<p align="center">
<b>Step 33: If everything has been configured correctly so far, you should now see data come in from the Target-PC. On the Splunk Server, go to Apps, Search & Reporting, and type in index=endpoint. If you see events populate the screen and you can see all 4 event types when you click "a source" (under selected fields, on the left side of the window), then you have successfully completed all steps up to this point. If something seems off, backtrack and find out where the error has occured. It can be due to a simple syntax error in the code, or because you did not set up the service account to "local" (refer to step 30).</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/J7cAy7PnfVeZ/x79P77BdUocp1haQqUn2dCajbm2fGz0R97VvckH5.png" height="85%" width="85%" alt="Step 33"/>
</p>


<p align="center">
<b>Step 34: Now we will repeat steps 23-33 to install and activate SplunkForwarder, but this time on the Windows Server VM. I will leave this part to you so you can gain some hands on experience.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/omSYEpUXDoVo/nn3WcxqEk5HdChSBiQWe2VMhfzuDHt4cQkk4oUy5.png" height="85%" width="85%" alt="Step 34"/>
</p>


<p align="center">
<b>Step 35: After you have installed and conifugred SplunkForwarder on the ADDC VM (windows server VM), the next step is to add Active Directory Domain Services and make this VM our Domain Controller. To do this, open Server Manager, Manage (top right toolbar), add Roles and Features, select Active Directory Domain Services and continue to install. </b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/mXS7KiGNpOx5/xK2BljongUw1lYDMS7Mo3dMyIjluext4b222rZ1l.png" height="85%" width="85%" alt="Step 35"/>
</p>


<p align="center">
<b>Step 36: Once ADDS is installed, head to the flag icon on the top toolbar of the Server Manager Dashboard, and click "Promote this server to a domain controller. For the deployment operation select add a new forest, add a password, continue with default settings, and install.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/JqSp2xB14iCz/wsdM5VyDWvYgevNROEKCYpsQnKeoKsckCP136a5Z.png" height="85%" width="85%" alt="Step 36"/>
</p>


<p align="center">
<b>Step 37: Next we will add some users to our domain. Go to Tools on the Server Manager Dashboard and select Active Directory Users and Computers. Right click the domain (test.local in my case), go to new, and add a new organizational unit named IT for the IT department. Then, right click on the new IT unit and select add a new user (I will be adding Jsmith). For the Password I will be using "P@ssw0rd!"; make sure to remember what password you use for the users, as we will need these later in the lab.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/hXhNvPnXA7b4/Lrkg1eQ2LbPxFdWN0iIqgnhuaOUMboXkxcFL2xwD.png" height="85%" width="85%" alt="Step 37"/>
</p>


<p align="center">
<b>Step 38: Repeat the instructions from step 37 to create an organizational unit for an "HR" department, and add another user within that department (I will be adding Tmitchell).</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/DlxfCyJBd2bo/BVRvipmSYByPyxZLCo0Aeeg6mXgU9S9GMflrT2tY.png" height="85%" width="85%" alt="Step 38"/>
</p>


<p align="center">
<b>Step 39: Now that the Active Directory is set up and our server is the Domain Controller, we need to head to the Windows 10 VM and join it with the Domain. Once inside, search for PC in the search bar, right click and open properties, open advanced system settings, and select the change button under the Computer Name tab. Under "member of", select Domain, and type in your domains name.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/tofwpQ63N18Z/Ghdq21AFDGV1KGDY9VumQncbjMbXGCAzAvWLbGEG.png" height="85%" width="85%" alt="Step 39"/>
</p>


<p align="center">
<b>Step 40: You will get an error because our target PC does not know how to resolve the domain name, and this is goes back to DNS settings. To fix this go to the bottom right toolbar, right click on network, and Open Network and Internet settings. Select Change adapter options, open the properties of the adapter, and double click the IPv4 option. As the default, we entered 8.8.8.8 (google DNS IP) as the DNS server, but now we will change it to our Domain IP address.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/8bFytuGAEQSW/fmVu4tTmod80ca0ZEiHY5QFmJm5kFZdX2Uv1Ugcl.png" height="85%" width="85%" alt="Step 40"/>
</p>


<p align="center">
<b>Step 41: To check that the DNS server has been configured successfully, open Command Prompt and type "ipconfig /all", and make sure that the information is correct. </b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/PXK624WbXa0o/6mpK5qpqQ71LAn2ixauhE7I0AlcgMbH6ceowEb81.png" height="85%" width="85%" alt="Step 41"/>
</p>


<p align="center">
<b>Step 42: Now that we have resolved the DNS error, repeat step 39. Log into the domain using the username/password you created (to avoid any errors, make sure that the Domain Controller/Windows Server VM is running). The system will prompt you to restart, so proceed with restarting but do not sign in once the system reboots.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/ZzrfkOafirG6/t1TxDJr8DafAnvrSncdgCvza2Y2hU8a5uHDdAp4b.png" height="85%" width="85%" alt="Step 42"/>
</p>


<p align="center">
<b>Step 43: Now we will confirm that everything is set up correctly by signing into one of the users we created in step 37-38. To do so, sign in using "other user" and sign in. Finally, open all of the VM's we created on VirtualBox and take snapshots so we have saved states to revert to in case something breaks in the next steps.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/3voDHXZKsy7I/ueOhVWLfJ4A1d1UeKdEUM1UOh2fkPtrJvDtNTI6D.png" height="85%" width="85%" alt="Step 43"/>
</p>


<p align="center">
<b>Step 44: We will now configure the Kali Linux VM (attacking machine). Open the VM, and login using username kali and password kali. First we will configure the IP address; to do this right click the ethernet icon on the top toolbar, and select edit connections. Select the default adapter and click on the settings icon on the bottom left of the window. Go to IPv4 settings, change the method to manual, add an address, copy the following information (based on our diagram), and save changes.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/YIeubNI33FVC/hl7WYosEGoTneau57bjqDsDVQjRHKDbYH0f8qi2f.png" height="85%" width="85%" alt="Step 44"/>
</p>


<p align="center">
<b>Step 45: To verify the changes, open the Kali Linux terminal by right clicking on the home screen and selecting the "open terminal here" option. Once inside, type "ip a" and verify the IP address. If it still has not changed, right click the ethernet icon on the top toolbar, disconnect, and reconnect to the adapter. Verify the changes again on the terminal.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/1AqD5FuN4Vc0/89FNDdbHCXhECP56AKyhMtkNnd70wjIDN9oqadC4.png" height="85%" width="85%" alt="Step 45"/>
</p>


<p align="center">
<b>Step 46: In terminal, we will verify our connectivity by pinging google.com and our Splunk Server IP to ensure transmission. Additionally, we will upgrade/update our repositories by typing the highlighted code.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/9XNobkyKIwe9/mUtx90CxVzF9buaIS8oNKDpIJnOWJCdNMorvjJ6z.png" height="85%" width="85%" alt="Step 46"/>
</p>


<p align="center">
<b>Step 47: After the upgrade/update, we will begin to set up our attack. Use the mkdir command to create a directory called AD-project. After that, we will download a tool called crowbar which we can use to perform targeted brute force attacks. WARNING: Be very careful and use this tool responsibly, do not target any other assets, and only use it for lab environments.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/7DJ3TF7BB5Ey/PGOv1YYBTNST3uvRYk4xq1tXDXPhyPbuzhtrdCRv.png" height="85%" width="85%" alt="Step 47"/>
</p>


<p align="center">
<b>Step 48: We will use a popular wordlist that comes with Kali Linux called rockyou.txt. Use the following lines of code to unzip the file and copy it into our ad-project folder. We will only use the first 20 lines of the file and save it as a file named passwords.txt.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/EDpcqH3ABkmm/Uo5jDtTGZyegbUVnW29sk4zoe5eCxAhjFN5tmrhj.png" height="85%" width="85%" alt="Step 48"/>
</p>


<p align="center">
<b>Step 49: Lets say that as an attacker, we want to target a certain password since we have already exposed a certain user's password. Add the password we used for the users in our domain (P@ssw0rd!) to our password.txt list.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/aZ41h1VTKZu2/cNov5BIuEoQIzo79OdQYjWTQKLOJiSb6Ou6Qj8NR.png" height="85%" width="85%" alt="Step 49"/>
</p>


<p align="center">
<b>Step 50: Open the Target VM (Windows 10 VM) so we can enable Remote Desktop Protocol (RDP). Type in PC in the windows search bar, go to properties, advanced system settings, login with the administrator account, go to the Remote tab, and select "Allow remote connections to this computer". Select users, and add the two users we created for our domain (Tmitchell and Jsmith in my case).</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/WVyq1J2mYSUR/hxQOYdVB1QqKvWTNOeIg7P6aggS11lNMjypC3tBi.png" height="85%" width="85%" alt="Step 50"/>
</p>


<p align="center">
<b>Step 51: Go to the Kali Linux VM and open terminal. Type in crowbar -h. Crowbar is the tool we will be using for the brute force attacks, and -h opens the help menu.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/I1M4q7IgMv9U/0HbezXXeyhWb80is12so47BhZHrwBeaGuEOvMhuA.png" height="85%" width="85%" alt="Step 51"/>
</p>


<p align="center">
<b>Step 52: We will now run this following code: "crowbar -b rdp -u jsmith -C passwords.txt -s 192.168.10.100/32". -b is a flag that specifies the service (remote desktop), -u specifies the targeted user, -C specifies the password list, -s specifies the targets IP address. We use the /32 CIDR notation to specify that we only want to target that single IP.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/ZzzKoZUdF425/6wzGNLqEGU5LNOy8mnvj780BKOuksOfVIq5m6bar.png" height="85%" width="85%" alt="Step 52"/>
</p>


<p align="center">
<b>Step 53: Now that we have a RDP Success, lets go to the Splunk Enterprise Server and see what telemetry we've generated. Go to Search and Reporting, type "index=endpoint Jsmith", change the time to the last 15 or 60 minutes. Once the events populate the page, go to the left toolbar and select the # EventCode option under Interesting Fields. You will see an EventID of 4625 (failed logon attempt) with a count of 26, which means that there have been 26 failed attempts to log into the Jsmith account. There is also a EventID 4624 (successful logon attempt) around the same exact time which, for future reference, is a good sign of a brute force attack. You can also see the worstation name (kali) and its source network address (192.168.10.250).</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/uWNAOaKFxx4I/oIaftv7TWuxqxoYh1InU3thDrM6qgKWnZL0npPkA.png" height="85%" width="85%" alt="Step 53"/>
</p>


<p align="center">
<b>Helpful Tip: If you are curious about what each of these event ID's mean, you can go to www.ultimatewindowssecurity.com/securitylog/encyclopedia/ and search for each EventID.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/ILndboEbgsbM/cycygTO3qTpPA9fVLOiaTSHXPGySyP8NDtLpw2vh.png" height="85%" width="85%" alt="Helpful tip"/>
</p>


<p align="center">
<b>Step 54: Next, we will install Atomic Red Team (ART) on the target VM to run some tests. To do so, open Powershell as an administrator, type in the following code, and type Y to confirm. </b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/oeOEA2BJDxc4/QyEvYRwkV70xhqow9lfvSSxmbp0GI8DLjWn5mC9i.png" height="85%" width="85%" alt="Step 54"/>
</p>


<p align="center">
<b>Step 55: Before we can download ART, we will need to set an exclusion for the C drive otherwise Microsoft Defender will detect and remove certain files from ART. Navigate to Windows Security, Virus and Threat Protection, manage settings, scroll down to exclusions, add exclusion, select folder, and select the C drive under This PC. The result should look like this.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/r02yBF32dbKz/mx7qgkdysQcBhsfut8DdWTOTt6QEpsbyNsLCeYYJ.png" height="85%" width="85%" alt="Step 55"/>
</p>


<p align="center">
<b>Step 56: Now we can head back to Powershell and input the following code to download ART from the internet. When finished, press Y and enter to install the dependencies.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/7qgX9IEsKv7Y/KFK1qYfjIYeWkQzpLLVann6YNka8NrEz2MaMVEbV.png" height="85%" width="85%" alt="Step 56"/>
</p>


<p align="center">
<b>Step 57: Open the C drive and look inside the atomics folder inside of AtomicRedTeam, you will see files that start with T, these are technique ID's that map back to the MITRE ATT@CK framework.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/pbQd989agcJX/f0ZJqdoxrmNwJazXgaxOvqiI6O07vqk1e3gk0QKc.png" height="85%" width="85%" alt="Step 57"/>
</p>


<p align="center">
<b>Helpful tip: The MITRE ATT@CK framework provides guidance on what an attacker might do once they have compromised an environment. By going to https://attack.mitre.org, you can see what all of these different Technique ID's mean. By hovering over the attacks with your cursor, you will see the associated Technique ID's. (For this example we will look at a "create local account" tactic with the ID of T1136.001). </b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/V2EhCMke6Qpr/UE6n81MMjzGOhMz8HPp3QViQ9Xpv8rF8VKk5jqrk.png" height="85%" width="85%" alt="Helpful tip 2"/>
</p>


<p align="center">
<b>Step 58: To use the command to invoke the T1136.001 attack, type in the following code. This will automatically generate telemtry on the Splunk server, based on creating a local account (username is NewLocalUser).</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/4jktvcWn2YMe/1H4asZ6T4vGHC3rHRQrve3G9kJpV77MQkgzHCZUN.png" height="85%" width="85%" alt="Step 58"/>
</p>


<p align="center">
<b>Step 59: If we go into our Splunk Enterprise server page and try to search for this event, it will say no results found. This tells us that if an attacker were to try creating new local accounts under the current system settings, we would not be able to detect it; we are essentially blind to this activity/attack. This is how ATR is helpful in a vulnerability detection role; it will identify the gaps in visibility and generate the telemtry to see if we can detect the activity.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/QAvsfsLBf5GA/4zgcxP2h4Vr7yyj2vRJrhlv5MV9yzqjIY1IQ4zgP.png" height="85%" width="85%" alt="Step 59"/>
</p>


<p align="center">
<b>Step 60: Lets try the same thing with a different attack technique. Going back to the MITRE page, we will select Command and Scripting Interpreter, specifically for Powershell (T1059.001). </b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/Zhs26K7WvlOI/xpBLa9QsxSyhNnXPrbJB3kBfS13R5mrZ3twSbksq.png" height="85%" width="85%" alt="Step 60"/>
</p>


<p align="center">
<b>Step 61: Now go back to Powershell and type in this code to invoke the attack.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/5IJq6OGzRNBU/0Hm2GH5mOQP127rnMUuWQdxfcN9pfbPqZhcWHe9x.png" height="85%" width="85%" alt="Step 61"/>
</p>


<p align="center">
<b>Step 62: Once it runs, you will receive notifications that Defender has found threats, and you will see the highlighted code.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/QL45PQmi2F2V/TV29Ng860MjPNkDljjEAXxqkijRRGCZrW6M3uEBK.png" height="85%" width="85%" alt="Step 62"/>
</p>


<p align="center">
<b>Step 63: Go back to the Splunk Enterprise Server page search for "index=endpoint powershell" to see if we can find that code from the previous step.</b> <br/>
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/YPBk82UgyR6U/QlEulUHZ3U3p1r6HeHzvKiHSY5ayYxzot70G1iKi.png" height="85%" width="85%" alt="Step 63"/>
</p>


<p align="center">
<img src="https://pixelfed.de/storage/m/_v2/676491643439523429/062ac74bd-fb82c6/nnGsZlFGZb4w/uhvwiT13pV7L3Vn3ccnbjj5VLtE014UFICHhJzTV.png" height="85%" width="85%" alt="Step 63"/>
</p>


<p align="center">
<b>Using the information and strategies from this lab, we can identify and strenghten our threat, vulnerability, and attack detection by finding our security blind spots and building alerts to detect them.</b> <br/>
</p>
