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

<p>
</p>
<p>
In this lab we will be creating two VMs in the same VNET. One will be the Domain Controller, the other will be the Client machine. We will change the Domain Controller to a static IP because it will offering Active Directory services to the client machine. The Client machine will be joined to the domain. We will control the DNS settings on the client machine, the client machine will use the Domain Controller as its DNS server. 
</p>
<br />

<p>
<img src="https://i.imgur.com/d22FHIm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
DC-1 has to have a static Private IP Address. Client one will connect to DC-1 to ensure connectivity we will try to ping DC-1 from Client-1. At first the ping will not work correctly. We have to enable ICMPv4 on the firewall on DC-1. Now we can ping DC-1 successfully from Client-1
</p>
<br />

<p>
<img src="https://user-images.githubusercontent.com/121250918/210021925-f7a3a842-3b2c-405b-bbbb-5cf1bcf10fb0.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://user-images.githubusercontent.com/121250918/210021934-9b316556-069e-4d0c-8c64-055433f2a99c.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
Now we will log back into DC-1 to install AD Users & Computers. Promote the VM to DC, setup a new forest as "TristanDomain.com" afterwards restart then log back into DC-1 as user: "TristanDomain.com\labuser". If you performed the steps properly you should be able to run AD Users & Computers as shown below.
</p>
<img src="https://user-images.githubusercontent.com/121250918/210021936-e56ec104-63d7-4455-94e0-279863a6b4f2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
Excellent! We can start creating Organizational Units (OU). Let's first create an OU named __Employees. Create another OU named __Admins. In order to do that right click on the domain area. Select new->Organizational Unit and fill out the field. Then click inside of your OU and right click, select new and select user and fill out the information for your new user. The user should be named John Doe, she is going to be an Admin so her username will be John_admin. Lastly add John to the domain admins security group. 
</p>
<img src="https://user-images.githubusercontent.com/121250918/210021942-453eb69a-959e-47c0-9598-49601b97917c.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
<img src="https://user-images.githubusercontent.com/121250918/210021946-6a657eec-ea90-4b71-bd3a-6c428f5e8eb0.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
From now on you can use John_admin as the administrator account. Now we will join Client-1 to the domain (TristanDomain.com) from the azure portal we will change client-1's DNS settings to the DC's Private IP address. After you do that restart Client-1 from within the Azure portal. Our picture below shows verification that client-1 is on the DC-1 DNS. 
</p>
<img src="https://user-images.githubusercontent.com/121250918/210021952-b2e57436-7461-43c2-af5f-071f723a1fbe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
</p>
</p>
<p>
</p>
<p>
We have to join Client-1 to the domain in order to do so navigate to your system settings and go to about. Off to the right select rename this pc (advanced). From there select to change the domain. Enter "TristanDomain.com" after that enter your credentials from TristanDomain.com\labuser. Your computer will restart and then client-1 will be a part of TristanDomain.com
</p>
<br />
<p>
  <p>
<img src="https://user-images.githubusercontent.com/121250918/210021956-b4e3b17b-7116-4d59-836d-fdadbcaacebb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Awesome!!! Client-1 is now a part of the domain. Now we will set up remote desktop for non-administrative users on Client-1. We have to log into Client-1 as an admin and open system properties. Click on "Remote Desktop", allow "domain users" access to remote desktop. After completing those steps you should be able to log into Client-1 as a normal user.
</p>
<br />

<p>
  <p>
<img src="https://user-images.githubusercontent.com/121250918/210021960-df0eb0a9-7362-4902-b555-feb0c228b2af.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly to verify that noraml users can RDP into Client-1 we will use a script to generate thousands of users into the domain. We will input the script in powershell, after the users are created we will select one and RDP into Client-1.
</p>
<br />
<img src="https://user-images.githubusercontent.com/121250918/210021964-49bcca1b-f063-4301-a60f-72ed85051c70.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<p>
  <p>
<img src="https://user-images.githubusercontent.com/121250918/210021968-e8916f41-98db-49db-890d-ec31575aad2a.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<img src="https://user-images.githubusercontent.com/121250918/210021975-6b636992-44cf-48a5-834e-3ac068bc849e.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
As you can see the Powershell script created a user with the username "baba.semo" We were able to login to Client-1 with his credentials as a normal user. 
</p>
