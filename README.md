<img height="75%" width="100%" alt="Image" src="https://github.com/user-attachments/assets/c74ee0a4-fdb1-48c7-a78f-ecf69c1261d9" />

# Configuring Active Directory (On-Premises) in Azure

This guide outlines how to deploy and configure an on-premises-style Active Directory Domain Services (AD DS) environment using Azure Virtual Machines.

---

## 🖥️ Environments and Technologies Used

- **Microsoft Azure** (Virtual Machines/Compute)
- **Remote Desktop Protocol (RDP)**
- **Active Directory Domain Services (AD DS)**
- **PowerShell**

## 💻 Operating Systems Used

- **Windows Server 2022**
- **Windows 10 Pro**

---

## 📋 High-Level Deployment and Configuration Steps

1. Create Azure resources
2. Ensure connectivity between the client and Domain Controller
3. Install Active Directory Domain Services
4. Create admin and standard user accounts in Active Directory
5. Join `Client-1` to the domain (`mydomain.com`)
6. Configure RDP access for non-administrative users
7. Use PowerShell to create additional users and test logins

---

# Part 1: Preparing the AD Infrastructure in Azure

---

## Step 1: Create a Resource Group

- Select the appropriate Azure **Subscription**.
- Choose the correct **Region** for your deployment.

---

## Step 2: Create a Virtual Network

<img height="75%" width="100%" alt="Image" src="https://github.com/user-attachments/assets/748e4324-54da-4ba7-bc5d-fb12e5a11601" />

---

## Step 3: Create the Domain Controller VM (DC-1)

- **Resource Group:** Use the previously created Resource Group.
- **Networking:** Use your custom Virtual Network.
- **OS:** Windows Server 2022 (accept license agreement).
- **Size:** Choose a VM with at least 2 vCPUs.
- **Authentication Type:** Username/Password.

<img height="75%" width="100%" alt="Image" src="https://github.com/user-attachments/assets/405889e3-a31a-4b80-84ad-22ea4ce3840c" />

---

## Step 4: Create the Client Virtual Machine (Client-1)

- **Resource Group:** Use the same Resource Group.
- **Networking:** Select your custom Virtual Network.
- **OS:** Windows 10 Pro (version 22H2).
- **Size:** Minimum 2 vCPUs.
- **Authentication Type:** Username/Password.

<img width="100%" height="75%" alt="Image" src="https://github.com/user-attachments/assets/0be1793a-208d-4db8-a0bb-517b365ab4a3" />

---

## Step 5: Set a Static IP for DC-1

- In Azure Navigate to:  
  **Home > Virtual Machines > DC-1 > Network Settings > NIC > IP Configuration**  
  Change the setting from **Dynamic** to **Static**.

<img width="505" alt="Image" src="https://github.com/user-attachments/assets/214e621f-c98c-48fc-ad1f-9827d0213c96" />

---

## Step 6: Test Connectivity Between DC-1 and Client-1

- Log in to **DC-1** using Remote Desktop.
- Press `Win + R`, type `wf.msc`, and disable all firewall profiles (Domain, Private, Public).

<img width="100%" height="75%" alt="Image" src="https://github.com/user-attachments/assets/077f7cef-53e5-41a9-b47c-23f07e3a9ae0" />

---

## Step 7: Set DNS on Client-1 to Use DC-1's Private IP

- In Azure go to  **Virtual Machines > Client-1 > Network Settings > NIC > DNS Servers**.
- Select **Custom**, then add **DC-1's private IP address**.
- Restart **Client-1**.
- Log in and run `ping <DC-1 private IP>` to confirm connectivity.
- Verify DNS using `ipconfig /all` — DNS should point to DC-1.

<img width="100%" height="75%" alt="Image" src="https://github.com/user-attachments/assets/4373c2f3-4ba9-451a-a5ac-47e17da07baf" />

<img width="100%" height="75%" alt="Image" src="https://github.com/user-attachments/assets/65ef83ee-a708-42cb-8dab-416ecaec04f7" />

---

# Part 2: Deploying Active Directory

---

## Step 1: Install Active Directory Domain Services

- Log in to **DC-1** via Remote Desktop.
- Open **Server Manager > Add Roles and Features**.
- Select **Active Directory Domain Services** and proceed.

<img width="100%" height="75%" alt="Image" src="https://github.com/user-attachments/assets/0c235469-dccd-47c9-8126-cdeab5313058" />

- On the confirmation screen, check:  
  ✅ *"Restart the destination server automatically if required"*  
  Then click **Install**.

<img width="100%" height="75%" alt="Image" src="https://github.com/user-attachments/assets/de2ef90d-e707-45cc-a291-7dc8bfad08eb" />

---

## Step 2: Promote DC-1 to a Domain Controller

- Back in **Server Manager**, click **Promote this server to a domain controller**.

<img width="100%" height="75%" alt="Image" src="https://github.com/user-attachments/assets/04420e98-f4b0-41a1-8bc1-548bb392179c" />

- Choose **Add a new forest** and name it `mydomain.com`.

<img width="100%" height="75%" alt="Image" src="https://github.com/user-attachments/assets/b0922233-bf65-4a1f-a00d-fd65777316cf" />

- Set a **Directory Services Restore Mode (DSRM)** password.

<img width="100%" height="75%" alt="Image" src="https://github.com/user-attachments/assets/06428cf8-1e5a-4fbf-9e83-b5663c1ea435" />

- In **DNS Options**, uncheck **DNS delegation**.

<img width="100%" height="75%" alt="Image" src="https://github.com/user-attachments/assets/276cc967-2a29-481f-b5a5-09f981cff1d5" />

> 🔄 DC-1 will automatically reboot as a domain controller.

---

# Part 3: Create Admin and Join Client to Domain

---

## Step 1: Create an Organizational Unit (OU) and Admin User

- Open:  
  **Start > Windows Administrative Tools > Active Directory Users and Computers**

<img width="100%" height="75%" alt="Image" src="https://github.com/user-attachments/assets/b696bee6-6ab7-4b77-be61-1b59d2cd1df1" />

- Create two OUs: `_EMPLOYEES` and `_ADMINS`.

<img width="100%" height="75%" alt="Image" src="https://github.com/user-attachments/assets/dc99e3b4-7395-4069-81f9-99ec24f503b0" />

- Create a user named **Jane Doe** with:
  - **Username:** `jane_admin`
  - **Password:** `Cyberlab1234!`
  - Place this user in the `_ADMINS` OU.

<img width="100%" height="75%" alt="Image" src="https://github.com/user-attachments/assets/0529cef5-2eed-4e20-a6bb-908e0e309a82" />

- Make `jane_admin` a Domain Admin:
  - Right-click the user > **Properties > Member Of > Add**  
  - Type `Domain Admins`, click **Check Names**, then **OK**.

<img width="100%" height="75%" alt="Image" src="https://github.com/user-attachments/assets/7f34fadf-be25-4f93-92f4-9f25e7e7aac6" />

> ✅ Log out of DC-1 and back in as `mydomain.com\jane_admin`.

---

## Step 2: Join Client-1 to the Domain

- Log in to **Client-1**.
- Navigate to:  
  **Start > Settings > System > Rename this PC (advanced)**  
  - Click **Change**
  - Select **Domain** and enter `mydomain.com`

<img width="100%" height="75%" alt="Image" src="https://github.com/user-attachments/assets/a06a2670-f971-46fb-a220-f9819bdc3579" />

- Enter domain admin credentials (e.g., `jane_admin`) when prompted.
- After a successful domain join, **Client-1** will reboot.










 



















