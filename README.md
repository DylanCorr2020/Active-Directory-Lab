
<img height="75%" width="100%" alt="Image" src="https://github.com/user-attachments/assets/c74ee0a4-fdb1-48c7-a78f-ecf69c1261d9" />

# Configuring Active Directory (On-Premises) Within Azure
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.


## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

## Operating Systems Used 

- Windows Server 2022
- Windows 10

 ## High-Level Deployment and Configuration Steps

- Create Resources
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create additional users using powershell and attempt to log into client-1 with one of the users

  
# Part One Preparing AD Infastructure in Azure 

## Step 1: Create a Resource Group

- Select the appropiate Azure subscription.
- Select the appropiate Azure region.


## Step 2: Create our own Virtual Network 

<img  height="75%" width="100%" alt="Image" src="https://github.com/user-attachments/assets/748e4324-54da-4ba7-bc5d-fb12e5a11601" />

## Step 3: Create Domain Controller VM (DC-1)

- Resource Group: Select the previously created Resource Group.
- Networking: Select your own Virtual Network thar was created.
- Operating System: Choose Windows Server 2022
- Note: Be sure to agree to the license agreement when selecting the Windows OS.
- Size: Select a VM size that includes 2 vCPUs.
- Authentication Type: Set this to Username/Password.

<img height="75%" width="100%" alt="Image" src="https://github.com/user-attachments/assets/405889e3-a31a-4b80-84ad-22ea4ce3840c" />


## Step 4: Create the Client Virtual Machine (Client-1)

- Resource Group: Select the previously created Resource Group.
- Networking: Select your own Virtual Network thar was created.
- Operating System: Windows 10 Pro Version 22H2
- Note: Be sure to agree to the license agreement when selecting the Windows OS.
- Size: Select a VM size that includes 2 vCPUs.
- Authentication Type: Set this to Username/Password.

 <img width="100%" height = "75%"  alt="Image" src="https://github.com/user-attachments/assets/0be1793a-208d-4db8-a0bb-517b365ab4a3" />

 ## Step 5: Change DC-1 IP address from dynamic to static

- Home > Virtual  Machines > Network Settings > click on the virtual network interface card > edit Ip configuration > change from dynamic to static

  <img width="100%" height = "75%" alt="Image" src="https://github.com/user-attachments/assets/f0513bc4-526c-438b-b264-de31524e9a50" />

  ## Step 6: Test connectivity between DC-1 and Client-1

  - Log into DC-1 via remote desk top connection
  - Type in the search bar Run and type mf.msc to open windows firewall
  - disable the domain, private, and public profile
 
   <img width="100%" height = "75%" alt="Image" src="https://github.com/user-attachments/assets/077f7cef-53e5-41a9-b47c-23f07e3a9ae0" />


   ## Step 7: Set Client-1's DNS settings to DC-1's private IP address

   - Go into Network settings > click on Client 1's network interface card  > DNS servers
   - Click Custom and add DC-1's private id address
 
 <img width="505" alt="Image" src="https://github.com/user-attachments/assets/c084ab6a-8b55-47ce-b15b-410d88768039" />

  
  - Doing this will allow us to join the domain 

  - After Restart the Client-1 VM 

  - Log into Client-1 VM and attempt to ping DC-1 private IP address for connectivity 

  - Ping should be successful

  - Also check to see if the Client-1 DNS settings is set to DC-1 by typing ipconfig/all. It should show DC-1's private IP address.
 
  <img width="100%" height = "75%" alt="Image" src="https://github.com/user-attachments/assets/65ef83ee-a708-42cb-8dab-416ecaec04f7" />


 # Part Two Deploying Active Directory 

 ## Step 1: Install Active Directory 
     
- Log into DC-1 via remote desktop connection and install active directory domain services.
- Click start and open Server Manager
- Click on Add Roles and Features
- Server selection should only be one DC-1
- Server Roles add Active Directory Domain Services and click Next
  
<img width="100%" height = "75%" alt="Image" src="https://github.com/user-attachments/assets/0c235469-dccd-47c9-8126-cdeab5313058" />

- Confirmattion tick then check "Restart the destination server automatically if required" click yes then install

<img width="100%" height = "75%" alt="Image" src="https://github.com/user-attachments/assets/de2ef90d-e707-45cc-a291-7dc8bfad08eb" />

- Once installation complete you can close 



 
## Step 2: Promote DC-1 as an active controller by setting up a new forest as mydomain.com

- Go back into the server manager and click on "Promote this server to a domain controller"

<img width="100%" height = "75%" alt="Image" src="https://github.com/user-attachments/assets/04420e98-f4b0-41a1-8bc1-548bb392179c" /> 


- Click the add new forest radio button and the domain name "mydomain.com"

  
<img width="100%" height = "75%"  alt="Image" src="https://github.com/user-attachments/assets/b0922233-bf65-4a1f-a00d-fd65777316cf" />

- In Directory Controller Options  create a password

 <img width="100%" height = "75%" alt="Image" src="https://github.com/user-attachments/assets/06428cf8-1e5a-4fbf-9e83-b5663c1ea435" /> 

- In DNS Options uncheck the DNS delegation check box

<img width="100%" height = "75%" alt="Image" src="https://github.com/user-attachments/assets/276cc967-2a29-481f-b5a5-09f981cff1d5" />

- After these steps DC-1 will turn into an actual domain controller should automatically restart 



















