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
Preparing Active Directory Infrastructure in Azure
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
 
Be sure to create a username and password for Client-1. Similar to DC-1, the username will also be “labuser”.
 
After configuring settings for Client-1, its time to click “Review+Create”.
 
Once validation passes for Client-1, click “Create”
 
The deployment of Client-1 VM has been successful
 
After creating the two VMs, return to the Virtual Machine list on Azure and go to DC-1. From there, go to Network Settings.
 
From there, we will set the Domain Controller’s Network Interface Controller (NIC) private IP address from Dynamic to Static. 
Within Network Settings, go to Settings  IP Configurations and then click “ipconfig1”. From there, that is where we change the IP to Static. Click Save after completion
 
Go to Remote Desktop Connection and login to DC-1 VM. Click Yes to proceed if it asks questions shown in this screenshot below:
 
Within the DC-1 VM, right click the Start Menu. Go to “Run” and type in “wf.msc” (without the quotations).
 
Within Windows Defender, click “Properties”
 
Be sure to turn off the Domain, Private, and Public profiles and then click “Apply”
 
After applying these settings, its now time to set Client-1’s DNS settings to match DC-1’s Private IP address.
Return to Azure on your main computer. Go to settings of DC-1 and obtain the private IP address as highlighted below:
 
After copying the private IP address from DC-1, go to the settings of Client-1 in Azure. Go to Networking  Network Settings  Network Interface
 
After clicking on Network Interface, click on DNS Servers
 
After clicking on DNS Servers, click Custom and paste the private IP address from DC-1
 
Click Save and you have saved the DNS servers
 
Return to Virtual Machines on Azure and click on the Client-1 checkbox. Click Restart and say yes
 
Return to Remote Desktop Connection and click to connect to Client-1
 
Within Client-1, go to Powershell and Run as Administrator
 
Within Powershell, type ping and the private IP address of VM-1
 
Ensure the ping was successful in Powershell of Client-1
 
Continuing with Powershell, run the ipconfig/all command and you shall see the settings of the private IP address
 
Deploying Active Directory
After preparing the Active Directory infrastructure in Azure, the next part of the process is to deploy Active Directory. To get started, login to DC-1 and install Active Directory Domain Services.
Within DC-1, go to the Start Menu and click Server Manager. From there, click Add roles and features
 
Within “Add roles and features”, go to Server Roles and click Active Directory Domain Services
 
After keeping the default settings in Features and AD DS, go to Confirmation and click Restart
 
Click Install and let the Active Directory install itself
 
Within Server Manager, click on the flag with the yellow symbol. From there, click “promote this server”
 
Within Deployment Configuration, click Add a new forest and enter mydomain.com as your root domain
 
In the next step, you will have to create a password under “Domain Controller options”. However, that is merely a temporary thing as we want to be sure to uncheck Create DNS Delegation before proceeding forward
 
After unchecking the DNS delegation, leave the default settings as is. Once that is complete, click Install after the Prerequisite Check
 
After the installation has been completed, you are automatically signed out from the DC-1 virtual machine. From there, try to log back in via the host computer. As you can see below, using the generic “labuser” does not work. This is due to the fact that its now part of mydomain.com. 
Therefore, you must log back in to the DC-1 VM under the username mydomain.com\labuser
 
After logging back into the VM, go to Start Menu - Windows Administrative Tools - Active Directory Users and Computers
 
Within Active Directory Users and Comps, right click mydomain.com and go to new for Organizational Units
 
When creating a new Organizational Unit, type in _EMPLOYEES without errors
 
When creating another Organizational Unit, type in _ADMINS without errors
 
After creating the Organizational Units for _ADMINS and _EMPLOYEES, the next step is to create a New User within Admins. Type in Jane Doe and jane_admin as username
 
Create a Password for Jane Doe of your own choosing. Only check “password never expires” for lab exercise only. In real life, passwords must be changed every 90-120 days depending on your organization.
 
After creating the Jane Doe admin user, its now time to officially make her a domain admin user. 
Right click Jane Doe's username and go to Properties - Member Of. This is where you click Add
 
Type in Domain Admins and click Check Names. Once its there, click OK
 
Be sure to click Apply for the changes to take effect
 
Once this has been completed, log out of DC-1 and log back in as mydomain.com/jane_admin
 
Simultaneously, log in to the Client VM from the host computer
 
Within Client-1 VM, right click the Start Menu and go to System
 
Within Change, click on Domain and enter mydomain.com
 
Login to the mydomain.com with the jane_admin credentials
 
Click Rename this PC. After that, click on Change
 
The Client-1 VM will restart itself after configuring these settings. 
Return to DC-1 as jane_admin. Verify that Client-1 is present in Active Directory Users and Computers
 
After verifying that Client-1 is present, create a new Organizational Unit called _CLIENTS. 
 
From there, move Client-1 to _CLIENTS
 
Creating Users with Powershell
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
