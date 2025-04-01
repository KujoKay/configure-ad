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

<h2>High-Level Deployment and Configuration Steps</h2>

- Create a Domain Controller VM for DC-1 and Client VM for client-1
- Set the Domain Controller's NIC Private IP address to static. Log into the VM and disable Windows Firewall.
- Set Client 1's DNS settings to DC-1's Private settings.
- Attempt to ping DC-1's private IP address. 

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://github.com/user-attachments/assets/829acfeb-f5fc-4070-a37c-b7911b10d183"/>
<img src="https://github.com/user-attachments/assets/13f6ae21-cd2b-4ecc-af76-f4911a22e1ac"/>
</p>
<p>
1. Create a resource group and name it "Active-Directory-Lab". Next, create a virtual network and name it "Active-Directory-VNet". Make sure they are both in the region.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/650763ee-a7f9-4449-b751-ee9b2b6933a3"/>
<img src="https://github.com/user-attachments/assets/e3f9a7bc-86ee-49cc-9803-74e3856c57a5"/>
<img src="https://github.com/user-attachments/assets/eb4f338e-22af-4a8a-ae45-787095952e88"/>
</p>
<p>
2. Create a virtual machine and put it in "Active-Directory-Lab", name the VM "DC-1" and make sure it is in the same region as the resource group and virtual network. For image, select "Windows Server 2022 Datacenter". For size, select "Standard_D2s_v3 - 2 vcpus, 8 GiB memory". For the username and password, use anything that will be easy to remember. In the "Networking" tab, ensure the VM is connected to the "Active-Directory-VNet". Once that's done, click "Review + Create" then create the VM. 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/744b9b2e-02e9-4193-98bf-2bfa5e84d6a4"/>
</p>
<p>
3. Create another virutal machine but name this one "cilent-1". Make sure it's in the same region as the previous VM. For image, select "Windows 10 Pro, version 22H2". For size, select "Standard_D2s_v3 - 2vcpus, 8 GiB memory". Make sure to check the box under "Licensing" at the bottom of the page. Ensure the VM is connected to the "Active-Directory-VNet". 
</p>
<br />

<p>
4. Now we have to set DC-1's NIC Private IP address to static. Click on the "DC-1" virtual machine, click "Networking" then select "Network settings". Click on the "Network Interface / IP configuration", then select "ipconfig1". From there, an edit menu should appear. Select "static" then save the changes.
</p>
<p>
<img src="https://github.com/user-attachments/assets/91fb6f1b-4c3c-45b1-b08f-f54501e73b0c"/>
<img src="https://github.com/user-attachments/assets/fdd7509e-dc89-4474-bf4b-fa0aa8b84e73"/>
<img src="https://github.com/user-attachments/assets/fde43b77-dfbe-432c-9e16-8a1541871090"/>
</p>

<p>
5. Now we have to disable Windows firewall. Log into the DC-1 VM, right click the Start menu and select "Run". Type "wf.msc" in the prompt. Next, select "Windows Defender Firewall Properties" and turn off the firewall in Domain Profile, Private Profile, and Public Profile.
</p>
<p>
<img src="https://github.com/user-attachments/assets/a8f8d41c-fc0c-4249-a6b7-608b33f5b35f"/>
<img src="https://github.com/user-attachments/assets/c57039c5-0a41-43da-9fbd-0515543cd5ae"/>
</p>

<p>
6. Now we have to set cilent-1's DNS settings to DC-1's Private IP address. Select the cilent-1 VM, click on "Networking" then "Network settings", then select the "Network Interface / IP configuration". After that's done, select "DNS servers", click on "Custom" and insert DC-1's private IP address into the prompt. When that's done, restart cilent-1. 
</p>
<p>
<img src="https://github.com/user-attachments/assets/80de0d3c-ca20-46f9-aaee-292b58a3911a"/>
<img src="https://github.com/user-attachments/assets/2a782765-8dea-4f7d-aaa0-56da7d905276"/>
</p>

<p>
7. Once the cilent-1 VM is done restarting, log into it with the public IP address. When you've logged in, go into Powershell and ping DC-1 private IP address. If it was successful, it should look similiar to the images below. Using ipconfig /all, the private IP address should appear next to "DNS servers". 
</p>
<p>
<img src="https://github.com/user-attachments/assets/7aa43227-31e7-48ce-8ffe-7188a999ea07"/>
<img src="https://github.com/user-attachments/assets/f8f26259-6f06-4b83-98e3-676cb392c9f7"/>
</p>
<br />

<h3 align="center">Deploying Active Directory</h3>
<p>
1. Now we have to install Active Directoy. Log in the DC-1 VM and when the Server Manager application opens up, select "Add roles and features". Click next until you're on the "Server Roles" column. Select the "Active Directory Domain Services" box. Once that's done, click Next unti you're on the "Confirmation" column. Check the "Restart the destination server automatically if required" box and install. 
</p>
<p>
<img src="https://github.com/user-attachments/assets/63e37c55-ce96-4f51-8290-979a028bcfa7"/>
<img src="https://github.com/user-attachments/assets/06486060-b650-4134-a8ff-3ff1170496d9"/>
<img src="https://github.com/user-attachments/assets/ac1ed104-3842-41b9-9da9-e5d6cb0037c9"/>
</p>

<p>
2. Now we have to set up a new forest. On the top right in the "Server Manager" application, click on the flag icon. In the "Deployment Configuration" column, type in a domain name in the "Root domain name" box (it can be anything, just make sure you remember it). For the sake of simplicity, use "mydomain.com". In the "Domain Controller Options", type a new password in the boxes. In "DNS Options", make sure the "Create DNS delegation" box is NOT checked. Once that's done, click Next until you're in "Prerequisites Check", then click install. When it's done installing, the VM will restart.
</p>
<p>
<img src="https://github.com/user-attachments/assets/5e7cbbb5-d3ab-4456-a484-51472d52138a"/>
<img src="https://github.com/user-attachments/assets/e084c47a-e401-4e3d-b484-a456ae030538"/>
</p>

<p>
3. When the VM is done restarting, log back in (NOTE: You must use the domain you chose along with the user name to log in. For example, instead of just the username "labuser", it is now "mydomain.com\labuser". Password remains the same). Open Active Directory Users and Computers, right click your domain, hover over "New" then select "Organizational Unit". Name the new OU "_EMPLOYEES" (make sure its typed out EXACTLY like that). Make another new OU and name it "_ADMINS". 
</p>
<p>
<img src="https://github.com/user-attachments/assets/d32ecc2a-8e2a-47c4-ab43-c4d138c519c2"/>
</p>

<p>
4. Right click "_ADMINS", hover over "New" then select "User". Name the new employee "Jane Doe" with the username "jane_admin" with the same password used when you created the virtual machine (for the sake of the demonstration, check the "password never expires" box). Once the user is finished, right click on it and select "Properties". Select "Member Of", click "Add" and type "domain admins" in the prompt. Click "Check Names" then apply it.  
</p>
<p>
<img src="https://github.com/user-attachments/assets/0e6e9c77-394a-4c9f-85a9-6f2b8f431bda"/>
<img src="https://github.com/user-attachments/assets/3db6d07a-8ae0-4a5c-b654-6d5dba4a324c"/>
<img src="https://github.com/user-attachments/assets/54582bee-9c78-4a41-bb5f-0dc55c2de9f6"/>
</p>

<p>
5. Log out of the DC-1 VM and log back in as "mydomain.com\jane_admin". Password remains the same.
</p>

<p>
6. Log into cilent-1 as the original local admin and connect it to the domain. Right click the Start menu and select System. Click "Rename this PC (advanced)" then select "Change". From there, select "Domain" and enter "mydomain.com". A pop-up will appear to enter a username and password. For username, enter "mydomain.com\jane_admin". Password remains the same. Once you do that, the VM must restart.
</p>
<p>
<img src="https://github.com/user-attachments/assets/d107dc7b-8532-4eb1-a67c-4a3085d1526f"/>
<img src="https://github.com/user-attachments/assets/7759571e-e95e-4185-bab2-4eb9b80a5665"/>
</p>

<p>
7. Return to the DC-1 VM and verify that cilent-1 shows up in the ADUC. To verify, go into Active Directory Users and Computers and click on "Computers". Cilent-1 should appear. Create a new OU, name it "_CLIENTS", and drag cilent-1 into there.
</p>
<br />

<h3 align="center">Creating Users with Powershell</h3>
<p>
1. Log into cilent-1 as mydomain.com\jane_admin. Once that's done, right click the Start menu and open System. Click on "Remote Desktop" and enable it. Next, click "Select users that can remotely access this PC". Click "Add" then type "domain users" in the box. 
</p>
<p>
<img src="https://github.com/user-attachments/assets/5b2b54e4-dd19-4eec-bdb7-70a236b6d3c5"/>
<img src="https://github.com/user-attachments/assets/9b5c2cf2-5a23-4478-a836-34cb4abab948"/>
</p>

<p>
2. Now we will creating users using PowerShell. Log into DC-1 as jane_admin. Open PowerShell ISE and run it as administrator. Create a new file and paste the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1). Run the script and notice the accounts being created (you can stop the script whenever you feel enough accounts have been created). 
</p>
<p>
<img src="https://github.com/user-attachments/assets/909a3ea5-6453-4cfb-ba28-4fb986802d7b"/>
</p>

<p>
Open ADUC and notice the accounts being added into the OU _EMPLOYEES. Choose any random account and attempt to log into cilent-1 with that account. The password by default is "Password1".
</p>
<br />

<h3 align="center">Group Policy and Managing Accounts</h3>
<p>

</p>
