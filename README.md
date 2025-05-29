
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

<img width="100%" height="400" alt="Image" src="https://github.com/user-attachments/assets/771dbc6b-1ab4-475c-9d09-55ef62c268e6" />


