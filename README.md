
<img width="100%" height="400" alt="Image" src="https://github.com/user-attachments/assets/c74ee0a4-fdb1-48c7-a78f-ecf69c1261d9" />

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

<img height="505" alt="Image" src="https://github.com/user-attachments/assets/771dbc6b-1ab4-475c-9d09-55ef62c268e6" />

## Step 2: Create our own Virtual Network 

<img  height="505" alt="Image" src="https://github.com/user-attachments/assets/748e4324-54da-4ba7-bc5d-fb12e5a11601" />

## Step 3: Create Domain Controller VM (DC-1)

- Resource Group: Select the previously created Resource Group.
- Networking: Select your own Virtual Network thar was created.
- Operating System: Choose Windows Server 2022
- Note: Be sure to agree to the license agreement when selecting the Windows OS.
- Size: Select a VM size that includes 2 vCPUs.
- Authentication Type: Set this to Username/Password.

<img height= "505" alt="Image" src="https://github.com/user-attachments/assets/405889e3-a31a-4b80-84ad-22ea4ce3840c" />  



