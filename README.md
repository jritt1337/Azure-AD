# On-premises Active Directory Deployed using Microsoft Azure
This is a tutorial outlining the setup and configuration of Active Directory using Azure Virtual Machines.

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

Once connected, our first order of business is to disable the Windows Firewall. Right-click the start menu and then click **Run**. Run **wf.msc** to bring up the Windows Firewall menu. Click **Windows Defender Firewall Properties** and then under **the Domain, Public, and Private Profile tabs** change **Firewall State** to **"Off"**.

<img width="1491" height="813" alt="Screenshot 2026-04-28 at 9 42 37 PM" src="https://github.com/user-attachments/assets/d7e576ee-04b5-487b-93d5-e3a3273ffcf6" />.

### Step 4: Configuring the Client's DNS Settings
Now that we have the Domain Controller configured, it's time to configure the Client to use the Domain Controller as its DNS server.

Navigate to the Virtual Machines menu, click your client VM, then **Network Settings** and then the NIC. In the sidebar, click **DNS Settings** and then **Custom**. Fill out one of the fields with the Private IP of the Domain Controller, and click save.

<img width="569" height="605" alt="Screenshot 2026-04-28 at 10 00 53 PM" src="https://github.com/user-attachments/assets/9469af6b-4d95-4a46-92dc-c6edf4acd979" />

After this, we will need to restart the Client VM so that the settings apply. Go back to the Virtual Machines menu, click the box next to the Client VM, and select the **Restart** option along the top bar.

<img width="1418" height="195" alt="Screenshot 2026-04-28 at 10 07 00 PM" src="https://github.com/user-attachments/assets/c2ed7c8f-880e-43c9-b537-37e79c137c2f" />







  











