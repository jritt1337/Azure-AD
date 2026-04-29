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







