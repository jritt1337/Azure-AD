# Deploying and Managing Active Directory in Microsoft Azure
This lab demonstrates the deployment and management of a Windows Active Directory environment hosted on Azure Virtual Machines, including domain configuration, user management, firewall configuration, and client domain joining.

## Environments and Technologies Used
- Microsoft Azure
- Remote Desktop
- Active Directory Domain Services
- Group Policy Management
- PowerShell

## Operating Systems Used
- Windows Server 2025 Datacenter: Azure Edition
- Windows 11 Pro

## Deployment and Configuration Steps

For this project, we will create two different virtual machines (VMs). The first will be the Domain Controller, which will use Windows Server 2025 Datacenter: Azure Edition. The second will be a client using Windows 11 Pro.

### Step 1: Creating the Resource Group and Virtual Network.
Before creating the VMs, we need to create a Resource Group. In Azure, navigate to the **Resource Groups** menu.

<img width="649" height="741" alt="Screenshot 2026-04-28 at 8 42 34 PM" src="https://github.com/user-attachments/assets/d320d953-b24f-4b57-b280-5dbfa4f9f11c" />

From here, click **Create** and configure the Resource Group. You may name the virutal network whatever you wish, but make note of the selected **Region** you choose as you will need to create the virtual network and the VMs within the same region.

<img width="1608" height="844" alt="image" src="https://github.com/user-attachments/assets/95eca0f9-0dcf-42da-9db8-7f5b87fed503" />

Once the Resource Group is created, navigate to the **Virtual Networks** menu.

<img width="848" height="310" alt="Screenshot 2026-05-11 at 1 52 17 PM" src="https://github.com/user-attachments/assets/6f80097f-a11a-441e-9f03-96bf2e199aa1" />

Create and configure the Virtual Network. ***Make sure this is within the Resource Group that was just created, as well as the same region!***

<img width="804" height="656" alt="Screenshot 2026-04-28 at 8 49 48 PM" src="https://github.com/user-attachments/assets/8968fca0-6898-4443-a2b2-34c95706a3df" />

After the Resource Group and Virtual Network have been created, we're ready to begin creating the Virtual Machines.

### Step 2: Creating the VMs.
Now we can begin creating the virtual machines. We will start with the Domain Controller.

Navigate to the **Virtual Machines** menu and then click **Create**.

<img width="830" height="342" alt="Screenshot 2026-05-11 at 2 00 26 PM" src="https://github.com/user-attachments/assets/925c87c3-1f90-4d9f-8e80-e72a26676f08" />

Configure the Domain Controller VM. This machine should be running **Windows Server 2025 Datacenter: Azure Edition**. Double-check that the VM is being created in the correct Resource Group and Region.

<img width="804" height="689" alt="Screenshot 2026-04-28 at 8 55 27 PM" src="https://github.com/user-attachments/assets/bce4ef59-9794-4aa4-9a7f-9d5f51af5e28" />

Underneath **Administrator Account**, be sure to set a secure admin username and password. These credentials will be used later when connecting to the VM through Remote Desktop.

<img width="830" height="334" alt="Screenshot 2026-05-11 at 2 04 22 PM" src="https://github.com/user-attachments/assets/0421fe4f-98a6-48c6-bf45-50d67a3d497f" />

Navigate to the **Networking** tab and *ensure that the virtual network is set to the virtual network that was created in Step 1.*.

<img width="804" height="689" alt="Screenshot 2026-04-28 at 8 59 07 PM" src="https://github.com/user-attachments/assets/0ad37629-8bf0-4820-b96f-804dc8112d19" />


Once everything is in order, click **Review + Create**.

<img width="729" height="800" alt="Screenshot 2026-05-11 at 2 07 53 PM" src="https://github.com/user-attachments/assets/7c9e15aa-332d-4623-a770-10f4d1f3afaa" />


While this VM is spinning up, you can go ahead and create the Client VM. This VM will simulate a workstation on the network. Go back through the process of creating a VM, but this time choose **Windows 11 Pro** as the image. ***Once again, double check that this is within the same resource group/region/virtual network as everything else.***.

<img width="817" height="544" alt="Screenshot 2026-05-11 at 2 09 36 PM" src="https://github.com/user-attachments/assets/54dffdd5-ad68-4173-ac08-7d051f224ad9" />

Once both VMs are created, we're ready to start configuring the Domain Controller.

### Step 3: Configuring the Domain Controller

Once the Domain Controller VM is created, we will need to set its private IP address to be static. This will allow us to use the Domain Controller as the DNS server for the Client. To do this, you will navigate to the Virtual Machines menu and click on the Domain Controller VM. Click **Networking > Network Settings** on the sidebar, and then click the **NIC at the top of the screen**. 

<img width="804" height="520" alt="Screenshot 2026-04-28 at 9 17 02 PM" src="https://github.com/user-attachments/assets/34b3aae7-7eef-4eed-ad82-64214f86b9ba" />

From here, click **ipconfig1**, and then change the Private IP address settings to **Static** and save. ***Take note of the Private IP address, as later we will need it when setting up the Domain Controller as the Client's DNS server.***

<img width="1442" height="652" alt="Screenshot 2026-04-28 at 9 20 23 PM" src="https://github.com/user-attachments/assets/39d6a4a7-5f9a-4fdc-bd0a-8e1bd89d99b1" />

Now we need to connect to the Domain Controller using Remote Desktop Protocol (RDP). Return to the Domain Controller VM overview page and locate the **Public IP Address**

<img width="926" height="628" alt="Screenshot 2026-05-11 at 2 21 27 PM" src="https://github.com/user-attachments/assets/df4677be-970b-4bb9-b1f8-ee90729b29c1" />

Open your preferred RDP Client. If you don't have one, here are some suggestions.

#### Windows
- mstsc.exe (Built into Windows 11)
- Windows App

#### MacOS
- Windows App

#### Linux
- Remmina
- Rustdesk

I'm using Windows App on MacOS, so the following directions may vary depending on your preferred client. In Windows App, you can add the Domain Controller to the Devices menu by clicking the + symbol at the top right, and selecting **Add a PC**.

<img width="1382" height="433" alt="Screenshot 2026-05-11 at 2 25 58 PM" src="https://github.com/user-attachments/assets/7c3a8300-fbf2-44bb-aca8-8e6f92f6c34a" />

In the Add a PC window, input the Domain Controller's Public IP into the **PC Name** field. You can create a label for this VM by filling out the **Friendly Name** field.

<img width="497" height="550" alt="Screenshot 2026-05-11 at 2 27 29 PM" src="https://github.com/user-attachments/assets/f24ebe76-68aa-4c83-aec7-139fe2d638b6" />

Once the Domain Controller is added to your Devices menu, you can double-click it to initialize the connection. You will be asked to provide credentials to log in. Use the Administrator Account credentials you assigned when you created the Domain Controller.

<img width="1442" height="652" alt="Screenshot 2026-04-28 at 9 33 38 PM" src="https://github.com/user-attachments/assets/115a945b-24f0-4888-a989-4e6027a63ecb" />

Once connected, you should now be logged into the Windows Server desktop for the Domain Controller. 

### Step 4: Installing Active Directory on the Domain Controller
On the Domain Controller, open up the Server Manager. It should be opened by default, but if not, you can find it within the Start Menu.

In the Server Manager, click **"Add Roles and Features"**.

<img width="931" height="395" alt="Screenshot 2026-04-30 at 10 22 10 PM" src="https://github.com/user-attachments/assets/3566b7de-153d-433c-87ff-3c5d617a7ac0" />

On the installation type screen, leave **Role-based or feature-based installation** selected and click **Next**.

<img width="819" height="668" alt="Screenshot 2026-05-12 at 5 50 02 PM" src="https://github.com/user-attachments/assets/ea7fdd28-09ba-4351-9ce2-61b2889734bc" />

On the **Server Selection** screen, ensure your Domain Controller is selected, then click **Next**.

<img width="784" height="560" alt="Screenshot 2026-05-13 at 12 20 39 AM" src="https://github.com/user-attachments/assets/37fc3c9b-43f6-4319-b4ec-a891b0b6bb7b" />


On the **Server Roles** screen, check **Active Directory Domain Services**.

<img width="493" height="517" alt="Screenshot 2026-05-12 at 5 57 10 PM" src="https://github.com/user-attachments/assets/c0727364-290d-4a86-a1e5-eda85c979565" />

When prompted, click Add **Features**.

<img width="412" height="422" alt="Screenshot 2026-05-12 at 5 55 02 PM" src="https://github.com/user-attachments/assets/d3145c1a-82fb-439f-8921-d5f1ca8b870a" />


Feel free to click through the next few steps, until the Confirmation page. There you will check the box next to **"Restart the destination server automatically if required"**, then click **"Install"**

<img width="783" height="556" alt="Screenshot 2026-04-30 at 10 24 09 PM" src="https://github.com/user-attachments/assets/d2dc03ee-07ab-4ce0-a7a2-f38c501c66b4" />

While this is setting up, on the main Server Manager menu, you should have a yellow notification flag at the top-right corner. Click it and then click the **"Promote this server to a domain controller"** option. 

<img width="783" height="556" alt="Screenshot 2026-04-30 at 10 31 57 PM" src="https://github.com/user-attachments/assets/06ae9bb9-0213-47aa-a10c-1dd316c2ad9a" />

Under the Deployment Configuration step, you can add the DC to an existing domain, add a new domain to an existing forest, or create a new forest altogether. For the purposes of this tutorial, we're going with the third option: **Add a new forest.** You can name it whatever you wish.

<img width="783" height="556" alt="Screenshot 2026-04-30 at 10 40 55 PM" src="https://github.com/user-attachments/assets/0d3ac47d-c4d0-4da2-b965-79b85bdc40f4" />

In the next step, you will be required to set the DSRM password. **Be sure to set it to something secure.**. 

After that, just click through and install. After Active Directory installation and the setup of the Domain Controller, a reboot will be required. Now we will configure the Windows Firewall.

### Step 5: Windows Firewall Configuration
Before moving forward, we are going to check the Windows Firewall and ensure the proper inbound rules are allowed so that clients can communicate with the Domain Controller. 

Right-click the Start Menu and select **Run**.

<img width="209" height="618" alt="Screenshot 2026-05-11 at 2 37 45 PM" src="https://github.com/user-attachments/assets/a0b010d2-c913-4b05-ae15-b06589081976" />

Run **wf.msc** to bring up the Windows Firewall menu.

<img width="418" height="236" alt="Screenshot 2026-05-11 at 2 38 07 PM" src="https://github.com/user-attachments/assets/eabd67f3-f633-4540-bb35-f172619241c8" />

Ensure the firewall is enabled for all profiles (Domain, Private, and Public).

Then, in the Windows Firewall menu, click **Inbound Rules**.

<img width="1108" height="731" alt="Screenshot 2026-05-04 at 12 41 20 PM" src="https://github.com/user-attachments/assets/022e2483-e768-49c5-b711-a57eccd1ac16" />

You will now see a long list of firewall rules.

<img width="1132" height="783" alt="Screenshot 2026-05-11 at 2 51 05 PM" src="https://github.com/user-attachments/assets/fea36073-e4c3-41e6-910a-5c0468193773" />

From here, ensure the following inbound rule groups and rules are enabled. Some of these rules may already be enabled depending on Windows Server configuration:

* Active Directory Domain Services (rule group)
* DNS Server (UDP-In and TCP-In)
* File and Printer Sharing (Echo Request - ICMPv4-In)
* Kerberos Key Distribution Center (TCP/UDP-In)
* Remote Desktop (TCP-In)

To enable a rule, right-click it and select **Enable Rule**.

<img width="557" height="258" alt="Screenshot 2026-05-11 at 2 50 38 PM" src="https://github.com/user-attachments/assets/793e419c-b951-4cf0-8a79-9c3ac4f218d8" />

In a production environment, these rules would typically be restricted to trusted network ranges rather than be left open to all sources.


### Step 6: Adding and Managing Users in Active Directory

Now that Active Directory is deployed, we want to add and manage users for our domain. Open up **Active Directory Users and Computers** via the Start Menu or by running "**dsa.msc**". 

<img width="1220" height="727" alt="Screenshot 2026-05-12 at 6 03 35 PM" src="https://github.com/user-attachments/assets/1912456e-66bb-4e7c-adb3-e5848d9f2d00" />

From here, find the domain you created in the left-hand sidebar. Right-click the domain to access the available options. From the right-click menu, click **New > Organizational Unit**.

<img width="602" height="473" alt="Screenshot 2026-05-04 at 2 06 38 PM" src="https://github.com/user-attachments/assets/a6f24f23-a5fa-4fb1-b17a-fbf98b080892" />

Create an Organizational Unit named **Employees**.

<img width="438" height="413" alt="Screenshot 2026-05-12 at 6 07 31 PM" src="https://github.com/user-attachments/assets/315db874-5bfc-4590-b7f1-838ac51ecac5" />


Then repeat this process for an Organizational Unit named **Admins**.

<img width="438" height="413" alt="Screenshot 2026-05-12 at 6 13 07 PM" src="https://github.com/user-attachments/assets/a4490088-7019-4da1-ae10-75c78d249107" />

These Organizational Units help separate users by role and make it easier to apply group policies and permissions later.

From here, let's make an administrative user account. Right-click the new **Admins** organizational unit, then **New > User**. 

<img width="594" height="444" alt="Screenshot 2026-05-12 at 6 14 20 PM" src="https://github.com/user-attachments/assets/f5c87efc-bad9-4c28-981b-a121f7a3a0a6" />

Fill out the information, then click **Next**.

<img width="441" height="385" alt="Screenshot 2026-05-12 at 6 17 01 PM" src="https://github.com/user-attachments/assets/a10a015e-6709-496f-974e-c7f291d6dcd9" />

On the password configuration screen, assign a secure password for the account. Configure any password options that fit your environment or lab requirements, then click **Next** and **Finish**.

<img width="441" height="385" alt="Screenshot 2026-05-12 at 6 18 32 PM" src="https://github.com/user-attachments/assets/82fceb4d-f673-4f24-9f1f-4afcca01de22" />

Organizational Units (OUs) are used for organization and policy management, but they do not grant permissions. To give this user administrative privileges, we must add them to the built-in **Domain Admins** group. 

Right-click the new user within the Admins Organizational Unit and click **Properties**. 

<img width="441" height="385" alt="Screenshot 2026-05-12 at 6 20 46 PM" src="https://github.com/user-attachments/assets/39d0db5e-e338-4095-9203-d3bccdaff6ad" />


Click the **Member Of** tab and click **Add**. 

<img width="405" height="540" alt="Screenshot 2026-05-12 at 6 21 38 PM" src="https://github.com/user-attachments/assets/6462ea68-482e-4314-a940-ddc5d032340f" />

We're going to add this user to the built-in **Domain Admins** group, so type that in the field and hit **OK**.

<img width="464" height="282" alt="Screenshot 2026-05-12 at 6 23 45 PM" src="https://github.com/user-attachments/assets/2931d34f-14b3-4110-836e-92a8aae3c3b9" />


Now click **Apply** to add the user to the Domain Admins group.

<img width="410" height="538" alt="Screenshot 2026-05-12 at 6 24 47 PM" src="https://github.com/user-attachments/assets/23e8b38e-637a-45cd-9ce0-b8a64747f99a" />

This account can now be used to log into any domain-joined machine with administrative privileges. We will use this account to log into the client machine after it has been joined to the domain.

**NOTE: In production environments, Domain Admin accounts should be limited and used only when necessary due to their elevated privileges.**

Now create a few standard employee accounts within the Employees Organizational Unit. However, we will need to give these users RDP permissions to be able to login to the client, which we will do later. 

### Step 7: Configuring the Client 
Now that we have the Domain Controller configured, it's time to configure the Client to use the Domain Controller as its DNS server and join it to the domain.

Navigate to the Virtual Machines menu, click your client VM, then **Network Settings** and then the NIC. 

<img width="815" height="633" alt="Screenshot 2026-05-12 at 6 33 08 PM" src="https://github.com/user-attachments/assets/03edf64d-344c-40c5-ba3c-60e0ab74518d" />

In the sidebar, click **DNS Settings** and then **Custom**. Fill out one of the fields with the Private IP of the Domain Controller, and click **Save**.

<img width="569" height="605" alt="Screenshot 2026-04-28 at 10 00 53 PM" src="https://github.com/user-attachments/assets/9469af6b-4d95-4a46-92dc-c6edf4acd979" />

After this, we will need to restart the Client VM so that the settings apply. Go back to the Virtual Machines menu, click the box next to the Client VM, and select the **Restart** option along the top bar.

<img width="1418" height="195" alt="Screenshot 2026-04-28 at 10 07 00 PM" src="https://github.com/user-attachments/assets/c2ed7c8f-880e-43c9-b537-37e79c137c2f" />

After the restart, connect to it using your preferred RDP client.


Once logged into the client, open **Windows PowerShell** and type "**ping (*insert DC private IP address here*)**" and check to make sure the DC is pinging back. 
**Ping Success**

<img width="645" height="623" alt="Screenshot 2026-04-30 at 8 59 51 AM" src="https://github.com/user-attachments/assets/2c61ff8a-0303-4d3a-b8d0-b5466499ac2e" />


**Ping Failure**

<img width="645" height="623" alt="Screenshot 2026-04-30 at 9 08 59 AM" src="https://github.com/user-attachments/assets/4a7ff49a-6e43-4bd8-ab32-d5d41ccdc587" />

If the ping is timing out, go back and review all the steps. The most common mistakes are:

- The Domain Controller firewall rules are configured incorrectly.
- The DNS server IP was not updated
- The Client VM and Domain Controller are not on the same virtual network.


Next, verify that the client is using the Domain Controller as its DNS server by running:"**ipconfig /all**" 

<img width="780" height="623" alt="Screenshot 2026-04-30 at 9 15 03 AM" src="https://github.com/user-attachments/assets/3c37c052-0334-4eda-bbda-482e0b90e9f4" />


Once you've ensured the Client is communicating with the DC properly, it's time to join the client to the domain. Right-click the Start menu, select **System**.

<img width="515" height="980" alt="Screenshot 2026-05-12 at 6 48 25 PM" src="https://github.com/user-attachments/assets/7233ba28-199e-45d5-88e2-b5b01d7a40c9" />


Then click **Domain or workgroup**.

<img width="1197" height="932" alt="Screenshot 2026-05-12 at 6 48 50 PM" src="https://github.com/user-attachments/assets/a87e9e12-1ffb-4511-854c-f0331c1a3b33" />

In the Computer Name tab, click **Change**.

<img width="414" height="471" alt="Screenshot 2026-05-12 at 6 50 41 PM" src="https://github.com/user-attachments/assets/1b1937a2-bb25-4dac-832a-740b1e0ea701" />

Check the **Domain** option and fill in the field with the name of your domain. You will require authorized admin credentials to join the client to the domain. After joining, reboot the client. 

<img width="414" height="471" alt="Screenshot 2026-05-12 at 6 51 39 PM" src="https://github.com/user-attachments/assets/29891e59-2c2d-4637-a71f-0dfb6be1ebc9" />

After the restart, log in with the domain admin account created earlier. **The format for domain logins will now be either DOMAIN\Username, or Username@domain.com**. Employee accounts will be granted Remote Desktop access in the next step.

### Step 8: Basic Group Policy Management
Finally, we'll cover some group policy basics.
If you have not already done so, create a few employee accounts within the Employees Organizational Unit. 

Also, create a **Workstations** OU and drag the client machine from the **Computers** container to this new OU. Now we're going to complete a simple task to show how to do some basic Group Policy management. Firstly, we're going to need to allow our Employees to be able to connect to the client remotely.

Open the Active Directory Users and Computers utility, find and right-click the Employees OU, select **New > Group**. Name the group however you wish. For the purposes of this lab, I used "Remote Users". Make sure that the group scope is **Global** and the group type is **Security**.

<img width="739" height="494" alt="Screenshot 2026-05-05 at 8 39 08 PM" src="https://github.com/user-attachments/assets/79e5c10e-c547-4f71-b37b-615c7f731503" />

After creating the group, double-click it and add the employee users under the **Members** tab.

Now we will need to manage the permissions for the client machine. Open **Group Policy Management**, find the Workstations OU, right click and **Create a GPO in this domain, and Link it here...**. Name it something like "Remote Access". Right click the policy and select **Edit**. Afterwards, navigate to **Computer Configuration > Windows Settings > Security Settings > Restricted Groups**. Right-click and select **Add Group**. 

<img width="1131" height="696" alt="Screenshot 2026-05-05 at 9 07 53 PM" src="https://github.com/user-attachments/assets/e64e334a-45bd-4533-aa68-13ef0044eb5f" />

When the Add Group window appears, click **Browse**, enter **Remote Desktop Users** and click OK.

<img width="597" height="462" alt="Screenshot 2026-05-05 at 9 11 10 PM" src="https://github.com/user-attachments/assets/db0add00-40d7-4c76-a4fc-60e80b83c406" />

Afterwards, double-click the group, and when the Properties windows appears, add the **"Remote Users" (or whatever you named the group)** group. This should allow any users in the Remote Users group to log on to the client remotely.

**NOTE: Restricted Groups can replace the membership of the targeted local group. In production, administrators should carefully test this before applying it broadly.**

Now, on the client while logged on as an administrator, open PowerShell and run "**gpupdate /force**".

<img width="1114" height="627" alt="Screenshot 2026-05-05 at 9 39 39 PM" src="https://github.com/user-attachments/assets/606df195-3c64-4840-9280-f2c88c1c56f3" />

You can verify the applied Group Policy settings by running **gpresult /r** in PowerShell or Command Prompt.

After the Group Policy is updated, you must restart the client. Afterwards, any of the Employees within the Remote Users group should be able to log on to the client.


<img width="1114" height="589" alt="Screenshot 2026-05-05 at 9 33 15 PM" src="https://github.com/user-attachments/assets/be907a1b-0cd0-4ecc-b191-1133452ad9b5" />

































  











