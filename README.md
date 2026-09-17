<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure) - Part 1</h1>
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

- Step 1 – Create Domain Control and Client VMs on Azure
- Step 2 – Deploying Active Directory and Creating Users with Powershell
- Step 3 – Understanding Group Policy and Managing Accounts
- Step 4 – Understanding DNS and Network File Shares and Permissions (Parts 2 and 3)

<h2>Deployment and Configuration Steps</h2>

<p>
 
***Preparing Active Directory Infrastructure in Azure***

 
The first step is to create a Resource Group in Azure

<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/e55a4235-7ab6-48dd-b59d-c1dd2d81276f" />

 
Then, name the Resource Group. In this exercise, it will be known as “Active-Directory-Lab”.


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/8a3ecf0c-d210-462f-8417-01aeb4afc05d" />

 
After creating the Resource Group, create a new Virtual Network. From Azure, go to Virtual Networks and click “Create Virtual Network”


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/1d7ea1f6-6f05-43ad-89d0-b54beabbd664" />

 
When creating the new Virtual Network, assign it to the resource group created above and name the Virtual Network. For this, it will be named “Active-Directory-Vnet”


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/a960187a-f033-4ae3-a213-cf6dfb68d036" />

 
After creating the Resource Group and Virtual Network, begin creating the Virtual Machine. Go to Virtual Machines in Azure and click “Create”.


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/4a2d4b56-54e3-4521-a9ac-6727e08bfb48" />

 
From there, create the Domain Controller VM named “DC-1”. Assign the VM to the resource group you created earlier.


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/fbefcc8c-2be3-482f-a8e5-8b1a936eab6f" />

 
Assign DC-1 to the Windows Server 2022 OS virtual machine


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/ac1ae5ca-96ce-4b9c-8244-65eb94bcf06a" />

 
After assigning the OS to the virtual machine, create a username and password for the VM. The username for this VM will be “labuser”.


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/b9bb71a2-5caf-44c2-96fe-40135500897d" />

 
After configuring the settings for DC-1, click “Review+Create”.


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/8abf6584-8602-40e9-8651-bd90017c7839" />

 
Once Validation passes, click “Create” to finalize the VM for DC-1.


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/1c5bbb0a-3ba5-43ea-b6c4-14a8745fad17" />

 
The deployment of the DC-1 Virtual Machine is now successful


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/5cf8a519-8a9f-42c1-9189-109fa45f04dd" />

 
After creating DC-1, return to Virtual Machines in Azure and create another Virtual Machine


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/1d491df1-bbb2-48b5-8a01-b82d1a8f3794" />

 
Be sure to name the virtual machine “Client-1” and have it under the same resource group as DC-1


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/6c17ba9a-b62c-43c5-aa90-c1a62d2ae774" />

 
Assign the Client-1 VM to the Windows 10 OS


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/9065b945-c3d1-47a3-affb-206d9ca6e402" />

 
Be sure to create a username and password for Client-1. Similar to DC-1, the username will also be “labuser”.


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/e58cb8cd-0d4c-4712-9a1e-16505cfc30fb" />

 
After configuring settings for Client-1, its time to click “Review+Create”.


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/cd32414a-d60f-4e5f-8157-4a6037c007ab" />

 
Once validation passes for Client-1, click “Create”


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/f64c83c8-b665-4c67-8011-f84ec3bf066a" />

 
The deployment of Client-1 VM has been successful


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/bd6bc7c5-5d3f-42e8-8e20-ef3c67b4337d" />

 
After creating the two VMs, return to the Virtual Machine list on Azure and go to DC-1. From there, go to Network Settings.


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/5eec4757-15f6-430d-9059-9991068ad087" />

 
From there, we will set the Domain Controller’s Network Interface Controller (NIC) private IP address from Dynamic to Static. 

Within Network Settings, go to Settings --> IP Configurations and then click “ipconfig1”. From there, that is where we change the IP to Static. Click Save after completion


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/4630ade6-8896-4d3c-91bf-f41ac3435173" />

 
Go to Remote Desktop Connection and login to DC-1 VM. Click Yes to proceed if it asks questions shown in this screenshot below:


<img width="816" height="771" alt="image" src="https://github.com/user-attachments/assets/fa3039ac-7231-433f-8166-fe1d8ca7f2f3" />


 
Within the DC-1 VM, right click the Start Menu. Go to “Run” and type in “wf.msc” (without the quotations).


<img width="738" height="472" alt="image" src="https://github.com/user-attachments/assets/abe57f39-9d01-4a72-a203-0d15cc3d17c5" />

 
Within Windows Defender, click “Properties”


<img width="975" height="731" alt="image" src="https://github.com/user-attachments/assets/145c7096-1e07-47b3-b5d3-390f2e4a72a4" />

 
Be sure to turn off the Domain, Private, and Public profiles and then click “Apply”


<img width="975" height="371" alt="image" src="https://github.com/user-attachments/assets/3b8f4b36-1ec8-4a04-a939-16463b359179" />

 
After applying these settings, its now time to set Client-1’s DNS settings to match DC-1’s Private IP address.

Return to Azure on your main computer. Go to settings of DC-1 and obtain the private IP address as highlighted below:


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/577752cd-e40c-412c-a104-c8e8f96f8b5f" />

 
After copying the private IP address from DC-1, go to the settings of Client-1 in Azure. Go to Networking --> Network Settings --> Network Interface


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/c023c7b3-6010-482e-b161-8fd9acdd9115" />

 
After clicking on Network Interface, click on DNS Servers


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/3f359418-44bb-45e5-aee0-983fa3873927" />

 
After clicking on DNS Servers, click Custom and paste the private IP address from DC-1


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/b8e88c9e-c63f-4b6d-a5a0-953a79b973e7" />

 
Click Save and you have saved the DNS servers


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/3b853b57-830e-4447-9993-aa4a4cb001eb" />

 
Return to Virtual Machines on Azure and click on the Client-1 checkbox. Click Restart and say yes


<img width="975" height="468" alt="image" src="https://github.com/user-attachments/assets/bcdf067e-9f60-4f0a-8aa4-386ab42f0eee" />

 
Return to Remote Desktop Connection and click to connect to Client-1


<img width="847" height="486" alt="image" src="https://github.com/user-attachments/assets/efed7d73-64ca-45ff-8e6b-61d7f24eeed2" />

 
Within Client-1, go to Powershell and Run as Administrator


<img width="975" height="846" alt="image" src="https://github.com/user-attachments/assets/494fc1a0-e977-4190-9f61-312137178f2d" />
 
Within Powershell, type ping and the private IP address of VM-1


<img width="975" height="728" alt="image" src="https://github.com/user-attachments/assets/dade7e29-9b12-48d6-8a5d-57204859c5d8" />

 
Ensure the ping was successful in Powershell of Client-1


<img width="975" height="647" alt="image" src="https://github.com/user-attachments/assets/33798580-823d-434a-bd95-50884a911152" />


Continuing with Powershell, run the ipconfig/all command and you shall see the settings of the private IP address


<img width="975" height="523" alt="image" src="https://github.com/user-attachments/assets/18a7e043-cf07-46b0-92af-91faf5a23751" />
 
***Deploying Active Directory***


After preparing the Active Directory infrastructure in Azure, the next part of the process is to deploy Active Directory. To get started, login to DC-1 and install Active Directory Domain Services.

Within DC-1, go to the Start Menu and click Server Manager. From there, click Add roles and features


<img width="975" height="523" alt="image" src="https://github.com/user-attachments/assets/4058b055-3f85-4280-a868-4d80d4fba6a6" />

 
Within “Add roles and features”, go to Server Roles and click Active Directory Domain Services


<img width="975" height="694" alt="image" src="https://github.com/user-attachments/assets/8be80cb7-14bb-4888-90e7-0d8c242f8772" />

 
After keeping the default settings in Features and AD DS, go to Confirmation and click Restart


<img width="975" height="694" alt="image" src="https://github.com/user-attachments/assets/23cda85d-30ac-49b2-9f26-a7d0efa4f6a2" />

 
Click Install and let the Active Directory install itself


<img width="975" height="694" alt="image" src="https://github.com/user-attachments/assets/0402d611-35c8-472c-b06f-7f4e2f34bd97" />

 
Within Server Manager, click on the flag with the yellow symbol. From there, click “promote this server”


<img width="975" height="523" alt="image" src="https://github.com/user-attachments/assets/cb8d2fbc-22b8-4f15-a05d-c064c01f4bdd" />

 
Within Deployment Configuration, click Add a new forest and enter mydomain.com as your root domain


<img width="975" height="717" alt="image" src="https://github.com/user-attachments/assets/f1e4e2b6-fb4b-4300-ac6f-4dcab8102da2" />

 
In the next step, you will have to create a password under “Domain Controller options”. However, that is merely a temporary thing as we want to be sure to uncheck Create DNS Delegation before proceeding forward


<img width="975" height="717" alt="image" src="https://github.com/user-attachments/assets/7ce29580-d493-4bb2-bdda-ae789c7430bf" />

 
After unchecking the DNS delegation, leave the default settings as is. Once that is complete, click Install after the Prerequisite Check


<img width="975" height="717" alt="image" src="https://github.com/user-attachments/assets/904a1eb7-d64e-45f4-b44f-28b5e0bb0082" />

 
After the installation has been completed, you are automatically signed out from the DC-1 virtual machine. From there, try to log back in via the host computer. As you can see below, using the generic “labuser” does not work. This is due to the fact that its now part of mydomain.com. 

Therefore, you must log back in to the DC-1 VM under the username mydomain.com\labuser


<img width="891" height="1022" alt="image" src="https://github.com/user-attachments/assets/de6cac7f-c5f6-4c05-976a-0d641a44d61e" />

 
After logging back into the VM, go to Start Menu - Windows Administrative Tools - Active Directory Users and Computers


<img width="975" height="846" alt="image" src="https://github.com/user-attachments/assets/93aa67f2-133a-4589-a9a6-6e9a3314eab3" />

 
Within Active Directory Users and Comps, right click mydomain.com and go to new for Organizational Units


<img width="975" height="523" alt="image" src="https://github.com/user-attachments/assets/57564986-1aed-49a0-b50d-cc9dd1880ebc" />

 
When creating a new Organizational Unit, type in _EMPLOYEES without errors


<img width="853" height="738" alt="image" src="https://github.com/user-attachments/assets/16c93c18-40cb-459b-9bf6-e5a06fe4b3a3" />

 
When creating another Organizational Unit, type in _ADMINS without errors


<img width="853" height="738" alt="image" src="https://github.com/user-attachments/assets/66617a37-de10-4473-b06d-936b01500376" />

 
After creating the Organizational Units for _ADMINS and _EMPLOYEES, the next step is to create a New User within Admins. Type in Jane Doe and jane_admin as username


<img width="853" height="738" alt="image" src="https://github.com/user-attachments/assets/56ec20d7-dc27-4b79-814a-c2d924a386f2" />

 
Create a Password for Jane Doe of your own choosing. Only check “password never expires” for lab exercise only. In real life, passwords must be changed every 90-120 days depending on your organization.


<img width="853" height="738" alt="image" src="https://github.com/user-attachments/assets/920492f5-550c-4f25-8c52-0a9f472592c2" />

 
After creating the Jane Doe admin user, its now time to officially make her a domain admin user.

Right click Jane Doe's username and go to Properties - Member Of. This is where you click Add


<img width="801" height="1050" alt="image" src="https://github.com/user-attachments/assets/0fcba9af-ca83-4fc6-ab7f-4ed619b24957" /> 


Type in Domain Admins and click Check Names. Once its there, click OK


<img width="893" height="491" alt="image" src="https://github.com/user-attachments/assets/8d6c5c90-153e-4007-a13a-fc32d641499c" />

 
Be sure to click Apply for the changes to take effect


<img width="801" height="1050" alt="image" src="https://github.com/user-attachments/assets/cfb0e0b1-ba7c-44ca-93f5-624513efb9b7" />

 
Once this has been completed, log out of DC-1 and log back in as mydomain.com/jane_admin


<img width="847" height="949" alt="image" src="https://github.com/user-attachments/assets/b14571fb-3595-42c0-aa10-d5b4ce5716ff" />


Simultaneously, log in to the Client VM from the host computer


<img width="891" height="722" alt="image" src="https://github.com/user-attachments/assets/a05c1316-69da-45a1-a10c-90a56e479533" />

 
Within Client-1 VM, right click the Start Menu and go to System


<img width="975" height="762" alt="image" src="https://github.com/user-attachments/assets/0aec7636-bd05-4a8f-bc4d-a4a785997b22" />

 
Within Change, click on Domain and enter mydomain.com


<img width="672" height="751" alt="image" src="https://github.com/user-attachments/assets/64ae7915-43b3-45d1-835b-2cc90a717221" />

 
Login to the mydomain.com with the jane_admin credentials


<img width="891" height="585" alt="image" src="https://github.com/user-attachments/assets/699ffbe2-621c-4d5c-8e8e-09ab479f9081" />

 
Click Rename this PC. After that, click on Change


<img width="853" height="894" alt="image" src="https://github.com/user-attachments/assets/699ef116-e275-4186-9377-661f2b14f8d9" />


The Client-1 VM will restart itself after configuring these settings. 

Return to DC-1 as jane_admin. Verify that Client-1 is present in Active Directory Users and Computers


<img width="975" height="685" alt="image" src="https://github.com/user-attachments/assets/34d2d922-11c5-4e51-8f3c-e214fde280ac" />

 
After verifying that Client-1 is present, create a new Organizational Unit called _CLIENTS. 


<img width="853" height="738" alt="image" src="https://github.com/user-attachments/assets/17294de4-fb44-4307-9207-8e5a8065da81" />

 
From there, move Client-1 to _CLIENTS


<img width="909" height="345" alt="image" src="https://github.com/user-attachments/assets/33892572-75e2-4d13-a744-5623fb6aede6" />


 
***Creating Users with Powershell***


After configuring Active Directory on DC-1 and Client-1, its now time to create users with Powershell and set up Remote Desktop for non-admin users on Client-1. 
To start, we must log into Client-1 AS mydomain.com\jane_admin
 
Once you are logged in to Client-1 VM as jane_admin, go to System Properties and click Remote Desktop.
 
After clicking Remote Desktop, go to “select users that can remotely access this PC”. From there, allow “domain users” access to remote desktop.
 
Once that is complete, you can now log into Client-1 as a normal, non-admin user. Normally, this would be done with the Group Policy that would allow you to change MANY systems at once.
Now, its time to create additional users using Powershell. To begin with, log in to DC-1 as jane_admin.
 
Once you are logged in to DC-1 as jane_admin, go to Powershell ISE and “Run as Administrator”
 
After turning on Powershell ISE, return to the Host Computer and log in to GitHub. From there, you want to create a new file. For this exercise, a script was created from the CourseCareers instructor on creating new users. Click “copy raw file”
 
After copying the raw file, return to DC-1 and create a new file using Notepad to paste the script from GitHub
 
After pasting the script from GitHub, its time to click “Run Script”
 
Run the script and observe how many accounts are being created
 
When finished, open Active Directory Users and Computers and observe the accounts in the appropriate Organizational Units:   _EMPLOYEES
 
After this is complete, attempt to log in to Client-1 as one of the users created from the Powershell script. In this case, it’s “deg.rowe”. Remember the password generated from the Github script.
 
Once you are successful in logging in as the user created by Powershell, you can choose to play around with it or log out. Sometimes, the script will continue to run to create more users, but you can always stop the script at your discretion.
Group Policy and Managing Accounts
Now that users were created by Powershell have been wrapped up, the final step in the Active Directory process is creating a Group Policy and managing accounts in the event a user is locked out.
To get started, go to DC-1 as jane_admin. Pick a random user created from the previous script within Active Directory Users
 
In this scenario, the user selected was base.case. The purpose of this exercise is to observe how many times base.case can log in with the wrong password before being locked out.
Return to the host computer and open up Remote Desktop. Attempt to log in to Client-1 using the base.case account and password. In this scenario, base.case will be logging in with the wrong password.
 
 
After 10 attempts, the account base.case is officially locked out in the 11th attempt
 
Configure Account Lockout in Group Policy
To ensure that rules are enforced for user accounts in the domain, it must be enforced by the domain controller.
Before proceeding, return to DC-1 as Jane Admin and follow these steps below:
	Click Start, and type gpmc.msc in the search box, then press Enter. This opens the Group Policy Management Console.
 
	In the GPMC, navigate to the Group Policy Objects section.
 
	Right-click Group Policy Objects and select New to create a new GPO, or right-click an existing GPO and select Edit to modify it.


 
Be sure to give the new GPO a descriptive name if you're creating a new one, like "Account Lockout Policy".
	In the Group Policy Management Editor, expand the following:
○	Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy.
 
You will then see three primary settings that needs to be configured:
1.	Account Lockout Duration:
○	Definition: The time in minutes that an account remains locked before it is automatically unlocked.
○	Configuration: Double-click on this setting, select Define this policy setting, and then set the duration (e.g., 30 minutes).
 
2.	Account Lockout Threshold:
○	Definition: The number of failed logon attempts that will trigger an account lockout.
○	Configuration: Double-click on this setting, select Define this policy setting, and then set the threshold (e.g., 3 invalid attempts).
 
3.	Reset Account Lockout Counter After:
○	Definition: The time in minutes after which the failed logon attempts counter is reset to 0, assuming there are no additional failed logon attempts.
 
After setting the Group Policy in DC-1 as jane_admin, return to Client-1 as jane_admin. This step is important to fully implement the Group Policy set by DC-1
On Client-1, open Command Prompt and type gpupdate /force, then press Enter. This will force an update to happen
 
Once the Group Policy has taken effect, return to DC-1 to restore base.case account within Active Directory Users and go to Properties – Account
 
Click the checkbox to unlock the account as it’s currently locked by the Active Directory Domain Controller as shown above.
If necessary, reset the password of the client account base.case to unlock it. This is the case for the majority of users
 
Once the password has been reset and the account unlocked by the DC-1 admin, re log back into base.case account within Client-1 with the correct password after admin restores the account in DC-1
 
This is a sign that the base.case password for Client-1 was successful and you can click Yes to proceed
 
After dealing with account lockouts, we will now simulate a scenario where accounts have to be enabled/disabled. This would occur if a member of the organization left or the account was compromised in a leak. 
In this case, DC-1’s jane_admin will disable base.case’s account in Active Directory. From there, this will be the confirmation
 
After the account was disabled, base.case will attempt to log back in, but to no avail
 
 
After base.case attempted to log in, jane_admin of DC-1 can restore the account by enabling it again. From there, this will be the prompt.
 
After DC-1 reinstated base.case, attempt to log in again. This was a successful attempt
 
After the account was reinstated, we will now conclude Part 1 of Active Directory by observing the logs
In both DC-1 and Client-1, go to the Start Menu and type eventvwr.msc. After that, click on Security and observe the logs with Event Viewer
(For Client-1, this will be a little tricky as they’re a non-admin, but you could run eventvwr.msc from Client-1 under the “Run as Administrator” prompt. From there, DC-1’s jane admin would log in to allow Client-1 to see the logs)
 
From DC-1, you could find the name base.case in the Event Viewer and observe the login attempts
 
This is a precursor for people who enter Cybersecurity as they are constantly observing login attempts for multiple users.
This officially concludes Part 1 of the Active Directory lab. Part 2 will discuss configuring DNS servers within the same Active Directory.

</p>
<br />
