<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>Deployment and Configuration Steps</h2>

<!--- Step 1 -->
<p>
In this lab, we are going to create two Azure Virtual Machines in the same Virtual Network. One will be a Domain Controller, and the other will be a Client Virtual Machine. We are going to change the Domain Controller's Virtual NIC to static so that it doesn't change, because by default the Domain Controller's IP address is dynamic and you don't want it to get it's IP address assigned from the DHCP server. The Client Machine will be joined to the Domain. We will then control the DNS settings on the Client Machine so that the Client Machine is using the Domain Controller as it's DNS server.
</p>
<p>
<img src="https://i.imgur.com/MHG5iF0.png" height="80%" width="80%" alt="Azure Virtual Network"/>
</p>
<br />

<!--- Step 2 -->
<p>
The Domain Controller has to have a static private IP address. The Client Machine will connect to the Domain Controller to ensure connectivity, and we will try to ping the Domain Controller from the Client Machine. The first ping will not work correctly, because we have to enable ICMPv4 on the firewall on the Domain Controller. Now we are able to ping the Domain Controller successfully from the Client Machine.
</p>
<p>
<img src="https://i.imgur.com/9VBRsHX.png" height="80%" width="80%" alt="Window Defender"/>
</p>
<p>
<img src="https://i.imgur.com/BcU5Ktn.png" height="80%" width="80%" alt="Administrator Command Prompt"/>
</p>
<br />

<!--- Step 3 -->
<p>
Now we will log back into the Domain Controller to install Active Directory Users & Computers. Promote the Virtual Machine to Domain Controller, and setup a new forest as "mydomain.com". After you setup a new forest, restart and then log back into the Domain Controller as:
</p>

- User: mydomain\labuser

<p>
If you completed the steps correctly, you should be able to run Active Directory Users & Computers as shown below:
</p>

<p>
<img src="https://i.imgur.com/utdfSRo.png" height="80%" width="80%" alt="Active Directory Users and Computers"/>
</p>
<br />

<!--- Step 4 -->
<p>
Next, we are going to start creating Organizational Units. Let's first createone name _EMPLOYEES, and then another named _ADMINS. In order to do this, right click Domain Area -> Select New -> Organizational Unit and then fill out the fields required. Then, click inside of your Organization Unit and right click -> Select New -> Select User -> and then fill out the information for a new user.
</p>

- Name: Jane Doe

<p>
Jane Doe is going to be an Admin, so her username will be jane_admin. Lastly add Jane to the domain admins security group.
</p>

<p>
<img src="https://i.imgur.com/xM96tLh.png" height="80%" width="80%" alt="Domain Users"/>
</p>
<p>
<img src="https://i.imgur.com/MIcf77B.png" height="80%" width="80%" alt="Activity Directy Admins"/>
</p>
<br />

<!--- Step 5 -->
<p>
Next, you can use jane_admin as the administrator account. We will now join the Client Machine to the Domain (mydomain.com) from the Azure Portal. We will change the Client Machine's DNS settings to the Domain Controller's private IP address. After you do this, restart the Client Machine from within the Azure Portal. The picture below shows verification that the Client Machine is on the Domain Controller's DNS.
</p>
<p>
<img src="https://i.imgur.com/i2MCqk1.png" height="80%" width="80%" alt="DNS Servers"/>
</p>
<p>
<img src="https://i.imgur.com/Y9cbfWT.jpg" height="80%" width="80%" alt="DNS Servers Command Line"/>
</p>
<br />

<!--- Step 6 -->
<p>
Now we have to join the Client Machine to the Domain. In order to do this, navigate to your system settings and go to About. Select Rename This PC (Advanced) -> Select Change The Domain -> Enter "mydomain.com" and after that enter in the credentials from "mydomain.com\labuser".
</p>

<p><strong> Your computer will restart </strong> and then the Client Machine will be a part of mydomain.com. </p>

<p>
<img src="https://i.imgur.com/HtCiS3H.png" height="80%" width="80%" alt="System Properties"/>
</p>
<br />

<!--- Step 7 -->
<p>
The Client Machine is now a part of the Domain. Next, we will set up Remote Desktop for Non-Administrative users on the Client Machine. To do this, we have to log into the Client Machine as an Admin and open System Properties. Click Remote Desktop -> Allow "Domain Users" access to Remote Desktop. After this is done, you should be able to log into the Client Machine as a normal User.
</p>
<p>
<img src="https://i.imgur.com/n6ai6qx.png" height="80%" width="80%" alt="Remote Desktop"/>
</p>
<br />

<!--- Step 8 -->
<p>
Now to verify that normal Users can Remote Desktop Protocol into the Client Machine, we will use a powershell script that will generate thousands of dummy Users for us to use. Select one of the Users created and then Remote Desktop Protocol into the Client Machine using:
</p>

- mydomain\enterusernamehere

<p>
<img src="https://i.imgur.com/WD8Nu8u.png" height="80%" width="80%" alt="Powershell Script"/>
</p>
<p>
<img src="https://i.imgur.com/KjYCphy.png" height="80%" width="80%" alt="Domain User Properties"/>
</p>
<p>
<img src="https://i.imgur.com/vfHkfSl.png" height="80%" width="80%" alt="Command Line"/>
</p>
<p>
Once inside the Client Machine using the random User you selected, open up the command line and type "whoami". You should then see the username of the User you created. We were successfully able to sign into the Client Machine using the credentials of a User we created from a Powershell Script in the Domain.
</p>
<br />

<p>
If you want more practice with:
</p>

- <a href="https://github.com/galexwade/azure-dns">DNS</a>
- <a ="https://github.com/galexwade/network-file-shares">Network File Shares and Permissions</a>

<p>
<strong>Keep your Domain Controller and Client Machine up and running.</strong>
</p>
