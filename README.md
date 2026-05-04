# On-premises Active Directory Deployed using Microsoft Azure
This is a tutorial outlining the setup and deployment of Active Directory using Azure Virtual Machines.

## Environments and Technologies Used
- Microsoft Azure
- Remote Desktop
- Active Directory Domain Services
- Powershell

## Operating Systems Used
- Windows Server 2025 Datacenter: Azure Edition
- Windows 11 Pro

## Deployment and Configuration Steps

For this project, we will create two different virtual machines (VMs). The first will be the Domain Controller, which will use Windows Server 2025 Datacenter: Azure Edition. The second will be a client using Windows 11 Pro.

### Step 1: Creating the Resource Group and Virtual Network.
Firstly, before creating the VMs, we need to create a Resource Group. Navigate to the "Resource Group" menu.

<img width="649" height="741" alt="Screenshot 2026-04-28 at 8 42 34 PM" src="https://github.com/user-attachments/assets/d320d953-b24f-4b57-b280-5dbfa4f9f11c" />

Then from here, create the Resource Group. Name it whatever you want, but do make note of the Region you choose as you will need to create the virtual network and the VMs within the same region.

<img width="1608" height="844" alt="image" src="https://github.com/user-attachments/assets/95eca0f9-0dcf-42da-9db8-7f5b87fed503" />

Once the Resource Group is created, navigate to the "Virtual Networks" menu to create a virtual network. Yet again, name it whatever you want. ***Make sure this is within the Resource Group that was just created, as well as the same region!***

<img width="804" height="656" alt="Screenshot 2026-04-28 at 8 49 48 PM" src="https://github.com/user-attachments/assets/8968fca0-6898-4443-a2b2-34c95706a3df" />

### Step 2: Creating the VMs.
After the Resource Group and Virtual Network are created, it's time to create the VMs. We will start with the Domain Controller.

Navigate to the "Virtual Machines" menu and then create a virtual machine. This machine should be running **Windows Server 2025 Datacenter: Azure Edition**. Double-check to make sure this is in the correct Resource Group and region.

<img width="804" height="689" alt="Screenshot 2026-04-28 at 8 55 27 PM" src="https://github.com/user-attachments/assets/bce4ef59-9794-4aa4-9a7f-9d5f51af5e28" />

Underneath **Administrator Account**, be sure to set a secure admin username and password.

Navigate to the Network tab and ***ensure that the virtual network is set to the virtual network that was created in Step 1.***.

<img width="804" height="689" alt="Screenshot 2026-04-28 at 8 55 27 PM" src="https://github.com/user-attachments/assets/4bf1abb0-2601-40e2-918e-5cbd4df08815" />

Once everything is in order, click **Review + Create**. While this VM is spinning up, you can go ahead and create the Client VM. This VM will simulate a workstation on the network. Go back through the process of creating a VM, but this time choose **Windows 11 Pro** as the image. ***Once again, double check that this is within the same resource group/region/virtual network as everything else.***.

Once both VMs are created, we're ready to start setting everything up.

### Step 3 - Configuring the Domain Controller

Once the Domain Controller VM is created, we will need to set its private IP address to be static. This will allow us to use the Domain Controller as the DNS server for the Client. To do this, you will navigate to the Virtual Machines menu and click on the Domain Controller VM. Click **Networking > Network Settings** on the sidebar, and then click the **NIC at the top of the screen**. 

<img width="804" height="520" alt="Screenshot 2026-04-28 at 9 17 02 PM" src="https://github.com/user-attachments/assets/34b3aae7-7eef-4eed-ad82-64214f86b9ba" />

From here, click **ipconfig1**, and then change the Private IP address settings to **Static** and save. ***Take note of the Private IP address, as later we will need it when setting up the Domain Controller as the Client's DNS server.***

<img width="1442" height="652" alt="Screenshot 2026-04-28 at 9 20 23 PM" src="https://github.com/user-attachments/assets/39d6a4a7-5f9a-4fdc-bd0a-8e1bd89d99b1" />

Now we need to RDP into the Domain Controller using a RDP client. Here are a few suggestions.

#### Windows
- mstsc.exe
- Windows App

#### MacOS
- Windows App

#### Linux
- Remmina
- Rustdesk

Using your preferred client, RDP into the Domain Controller using the username and password you assigned to it. You can find the IP needed to RDP in by examining the Domain Controller in the Virtual Machines menu.

<img width="1442" height="652" alt="Screenshot 2026-04-28 at 9 33 38 PM" src="https://github.com/user-attachments/assets/115a945b-24f0-4888-a989-4e6027a63ecb" />

For a simple lab setup, one option is to disable Windows Firewall. A more secure alternative is shown in Step 3.5. Right-click the start menu and then click **Run**. Run **wf.msc** to bring up the Windows Firewall menu. Click **Windows Defender Firewall Properties** and then under **the Domain, Public, and Private Profile tabs** change **Firewall State** to **"Off"**. **IMPORTANT NOTE: In a production environment, you would configure the firewall rules instead of disabling it altogether.**

<img width="1491" height="813" alt="Screenshot 2026-04-28 at 9 42 37 PM" src="https://github.com/user-attachments/assets/d7e576ee-04b5-487b-93d5-e3a3273ffcf6" />.

### Step 3.5: Windows Firewall Configuration
As an alternative to disabling Windows Firewall in the previous step, we can configure the required inbound rules to allow domain and network communication. This is a more secure approach that better reflects how firewalls are handled in real-world environments.

Ensure the firewall is enabled for all profiles (Domain, Private, and Public). Then, in the Windows Firewall menu, click **Inbound Rules**.

<img width="1108" height="731" alt="Screenshot 2026-05-04 at 12 41 20 PM" src="https://github.com/user-attachments/assets/022e2483-e768-49c5-b711-a57eccd1ac16" />

From here, ensure the following inbound rule groups and rules are enabled:

* Active Directory Domain Services (rule group)
* DNS Server (UDP-In and TCP-In)
* File and Printer Sharing (Echo Request - ICMPv4-In)
* Kerberos Key Distribution Center (TCP/UDP-In)
* Remote Desktop (TCP-In)

In a production environment, these rules would further be restricted to trusted network ranges rather than be left open to all sources.

### Step 4: Installing Active Directory on the Domain Controller
On the Domain Controller, open up the Server Manager. It should be opened by default, but if not, you can find it within the Start Menu.

In the Server Manager, click **"Add Roles and Features"**.

<img width="931" height="395" alt="Screenshot 2026-04-30 at 10 22 10 PM" src="https://github.com/user-attachments/assets/3566b7de-153d-433c-87ff-3c5d617a7ac0" />

You can click Next on the next few screens, make sure the appropriate server is selected when at the "Server Selection" step, and when you get to "Server Roles" click **Active Directory Domain Services** and click the "Add Features" button on the pop-up that follows.

<img width="905" height="518" alt="Screenshot 2026-04-30 at 10 23 49 PM" src="https://github.com/user-attachments/assets/9b095bc3-97e0-4416-8dd0-f23088b5256c" />

Feel free to click through the next few steps, until the confirmation. There you will check the box next to **"Restart the destination server automatically if required"**, then click **"Install"**

<img width="783" height="556" alt="Screenshot 2026-04-30 at 10 24 09 PM" src="https://github.com/user-attachments/assets/d2dc03ee-07ab-4ce0-a7a2-f38c501c66b4" />

While this is setting up, on the main Server Manager menu, you should have a yellow notification flag at the top right. Click it and then click the **"Promote this server to a domain controller"** option. 

<img width="783" height="556" alt="Screenshot 2026-04-30 at 10 31 57 PM" src="https://github.com/user-attachments/assets/06ae9bb9-0213-47aa-a10c-1dd316c2ad9a" />

Under the Deployment Configuration step, you can add the DC to an existing domain, add a new domain to an existing forest, or create a new forest altogether. For the purposes of this tutorial, we're going with the third option: **Add a new forest.** You can name it whatever you wish.

<img width="783" height="556" alt="Screenshot 2026-04-30 at 10 40 55 PM" src="https://github.com/user-attachments/assets/0d3ac47d-c4d0-4da2-b965-79b85bdc40f4" />

In the next step, you will be required to set the DSRM password. **Be sure to set it to something secure.**. After that, just click through and install. After Active Directory installation and the setup of the Domain Controller, a reboot will be required.

### Step 5: Adding and Managing Users in Active Directory

Now that Active Directory is deployed, we want to add and manage users for our domain. Open up **Active Directory Users and Computers** via the Start Menu or by running "**dsa.msc**". From here, find the domain you created and expand the options. Right-click the domain to access the available options. Let’s create two Organizational Units: Employees and Admins. From the right-click menu, click **New > Organizational Unit**. Create an "Employees" Organizational Unit, then repeat this process for the "Admins" unit. These Organizational Units help separate users by role and make it easier to apply policies and permissions later.

<img width="602" height="473" alt="Screenshot 2026-05-04 at 2 06 38 PM" src="https://github.com/user-attachments/assets/a6f24f23-a5fa-4fb1-b17a-fbf98b080892" />


From here, let's make an admin account. Right-click the new **Admins** organizational unit, then **New > User**. Fill out the information, and on the next menu you will be able to set the password for the user. As this will be an admin account, do make sure it is a secure password. You can use any of the options listed per your or your organizations needs or policies. Afterwards, click Next, then Finish.


<img width="602" height="473" alt="Screenshot 2026-05-04 at 2 12 13 PM" src="https://github.com/user-attachments/assets/ee847b67-5862-41c4-9bf7-e2e6e69bd693" />

Organizational Units (OUs) are used for organization and policy management, but they do not grant permissions. To give this user administrative privileges, we must add them to the built-in **Domain Admins** group. Right-click the new user within the Admins Organizational Unit and click **Properties**. Click the **Member Of** tab and click **Add**. We're going to add this user to the built-in **Domain Admins** group, so type that in the field and hit OK, then apply the settings. This account can now be used to log into any domain-joined machine with administrative privileges. We will use this account to log into the client machine after it has been joined to the domain.

<img width="1103" height="543" alt="Screenshot 2026-05-04 at 2 14 43 PM" src="https://github.com/user-attachments/assets/5a5cc17e-fa18-417a-af54-7f699caf529d" />


**NOTE: In production environments, Domain Admin accounts should be limited and used only when necessary due to their elevated privileges.**



### Step 6: Configuring the Client 
Now that we have the Domain Controller configured, it's time to configure the Client to use the Domain Controller as its DNS server and join it to the domain.

Navigate to the Virtual Machines menu, click your client VM, then **Network Settings** and then the NIC. In the sidebar, click **DNS Settings** and then **Custom**. Fill out one of the fields with the Private IP of the Domain Controller, and click save.

<img width="569" height="605" alt="Screenshot 2026-04-28 at 10 00 53 PM" src="https://github.com/user-attachments/assets/9469af6b-4d95-4a46-92dc-c6edf4acd979" />

After this, we will need to restart the Client VM so that the settings apply. Go back to the Virtual Machines menu, click the box next to the Client VM, and select the **Restart** option along the top bar.

<img width="1418" height="195" alt="Screenshot 2026-04-28 at 10 07 00 PM" src="https://github.com/user-attachments/assets/c2ed7c8f-880e-43c9-b537-37e79c137c2f" />

After the restart, we will RDP into the client VM to ensure everything is setup correctly and that the connection between the Domain Controller and Client is recognized.

Once logged into the client, open **Windows Powershell** and type "**ping (*insert DC private IP address here*)**" and check to make sure the DC is pinging back. If the ping is timing out, go back and review all the steps. Most commonly mistake is forgetting to save the firewall settings on the Domain Controller. If the ping fails, it may indicate a networking or firewall issue, so ensure that the firewall is configured correctly, or confirm it was disabled if you chose the simpler lab method.

**Ping Success**

<img width="645" height="623" alt="Screenshot 2026-04-30 at 8 59 51 AM" src="https://github.com/user-attachments/assets/2c61ff8a-0303-4d3a-b8d0-b5466499ac2e" />


**Ping Failure**

<img width="645" height="623" alt="Screenshot 2026-04-30 at 9 08 59 AM" src="https://github.com/user-attachments/assets/4a7ff49a-6e43-4bd8-ab32-d5d41ccdc587" />

Also, type "**ipconfig /all**" and check the client's DNS server. It should be pointing to your DC's private IP address.

<img width="780" height="623" alt="Screenshot 2026-04-30 at 9 15 03 AM" src="https://github.com/user-attachments/assets/3c37c052-0334-4eda-bbda-482e0b90e9f4" />

Once you've ensured the Client is communicating with the DC properly, it's time to join the client to the domain. Right-click the Start menu, select **System**, then click **Advanced system settings**. In the Computer Name tab, click **Change**. Check the **Domain** option and fill in the field with the name of your domain. You will require authorized admin credentials to join the client to the domain. After joining, reboot the client. 
<img width="1505" height="960" alt="Screenshot 2026-05-04 at 1 26 35 PM" src="https://github.com/user-attachments/assets/2c2d1dc6-63e1-462c-8f07-d284db84368b" />

After the restart, log in to the domain account using the format **DOMAIN\username or user@domain.com** (ex. **DOMAIN\jdoe** or **jdoe@domain.com**). Once logged in, you can verify domain membership by opening System Properties and confirming the computer is joined to the domain, and confirm the logged-in user by running **whoami** in Command Prompt or PowerShell.



















  











