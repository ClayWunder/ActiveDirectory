<p align="center">
<img src=https://logowik.com/content/uploads/images/microsoft-active-directory5035.jpg
</p>

<h1>Active Directory deployment on VM in Microsoft Azure.</h1>

This tutorial outlines the deployment of Microsoft Active Directory in Microsoft Azure. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Windows Powershell

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Windows Server 2022

<h2>High-Level Deployment and Configuration Steps</h2>

- Create Resources
- Step 1 - Ensure Connectivity Between Client and Domain Controller.
- Step 2 - 
- Step 3 - 
- Step 4 - 
- Step 5 - 
- Step 6 - 
- Cleanup

<h2>Create Resources</h2>

![Screenshot 2024-01-31 at 3 28 55 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/1771c987-2730-4ff5-a84f-c06e97c9cfd2)

Create Domain Controller VM running Windows Server 2022. Name machine "DC-1"

![Screenshot 2024-01-31 at 3 30 49 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/47bda364-2e10-483f-8d21-76bf34228f94)

In Network Settings, set DC's NIC Private IP address to be static.

![Screenshot 2024-01-31 at 3 51 03 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/27ab1cbd-536c-485f-92de-c7e03a53e7d4)

Create the Client VM in running Windows 10. Machine should be in same resource group and Vnet as DC-1 Name machine "Client-1"

<h2>Ensure Connectivity Between Client and DC</h2>

![Screenshot 2024-01-31 at 3 59 10 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/9c478fd2-7f16-4a48-b91e-c6da803ee1f4)

Login to Client-1 with Microsoft Remote Desktop and ping DC-1's private IP address (perpetual ping).

![Screenshot 2024-01-31 at 4 01 53 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/7848a6ea-dc6d-4ef3-916e-b11d30e83f03)

Login to DC-1 and use Windows Firewall settings to enable ICMPv4 inbound.

![Screenshot 2024-01-31 at 4 03 29 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/3e18791e-d2c1-4764-9d5a-22718ac329f3)

Return to Client-1 to see the ping succeed.

<h2>Install Active Directory</h2>

![Screenshot 2024-01-31 at 4 08 43 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/01b880d7-03a2-4b13-907d-07323c2d440d)

- Login to DC-1 and install Active Directory Domain Services. 
- Server Manager Dashboard
- Manage
- Add Roles and Features
- Active Directory Domain Services

![Screenshot 2024-01-31 at 4 11 59 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/28d20517-7c7e-4663-8814-9663664a356f)

- Click yellow flag in Server Manager Dashboard.
- Click Promote This Server to Domain Controller.
- Add New Forest
- Choose Root Domain Name (mydomain.com)
- Choose Password

![Screenshot 2024-01-31 at 4 17 34 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/06fa5fc9-b9e3-44ca-9f5f-a6981cc13f94)

- Continue Through Install Wizard
- You will be automatically disconnected from DC-1 once complete.
- Re-log back into DC-1 as user mydomain.com/labuser

<h2>Create Admin and Normal User Account in AD</h2>

![Screenshot 2024-02-01 at 1 46 10 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/dc036758-34e5-4cb5-a6d9-2eb0972ab2c7)
![Screenshot 2024-02-01 at 1 49 17 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/d8c7a083-d289-48e6-af06-ededb71103fa)

- In AD Users and Computers (ADUC), create an Organizational Unit (OU) called "_EMPLOYEES"
- Create an Organizational Unit (OU) called "_ADMINS"

![Screenshot 2024-02-01 at 1 50 31 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/071e4efa-74b2-4d6f-9dc5-e33b45f91d30)

Create a new employee named "Jane Doe" (same password) with the username of "jane_admin"

![Screenshot 2024-02-01 at 1 51 15 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/a6d8807a-f44e-48c7-b25f-5d44565b45b2)

Add jane_admin to the "Domain Admins" Security Group

Logout / Close the remote desktop connection to DC-1 and log back in as mydomain.com/jane_admin

<h2>Join Client-1 to your domain (mydomain.com)</h2>

![Screenshot 2024-02-01 at 1 55 46 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/b8ab0704-9775-4415-acde-53ccfda3a798)

From Azure Portal, set Client-1's DMS settings to DC-1's private IP address.
From Azure Portal, restart Client-1
Login to Client-1 as the original local admin (labuser) and join it to the domain. (Computer will restart)

![Screenshot 2024-02-01 at 2 08 42 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/79af385e-8d60-4d5e-8ef0-d01ed193b03e)

Login to DC-1 and verify Client-1 shows up in Active Directory Users and Computers inside the "Computers" container on the root of the domain.

![Screenshot 2024-02-01 at 2 09 37 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/9733552e-ea9e-4214-a5e0-79d9b4c792f7)
![Screenshot 2024-02-01 at 2 10 55 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/54325a4d-82b8-435b-ae3a-0bb72ae392a7)

Create new OU named "_CLIENTS" and drag Client-1 into it.

![Screenshot 2024-02-01 at 2 14 56 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/b48a3687-2a09-4e6a-9f49-3be461f2a2c1)

<h2>Setup Remote Desktop for non-admin users on Client-1</h2>

![Screenshot 2024-02-01 at 2 13 05 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/3c281e4e-1b4c-4902-83c8-b101eae8d33e)

Login to Client-1 as jane_admin and open system properties.

![Screenshot 2024-02-01 at 2 13 55 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/445288b0-385e-4431-abcf-d85571768819)

Select "Remote Desktop"

![Screenshot 2024-02-01 at 2 14 56 PM](https://github.com/ClayWunder/ActiveDirectory/assets/157168474/faca5e2c-73c3-46db-95e5-89404b1c2803)

Allow "domain users" access to remote desktop.

<h2>Create Users and Attempt To Use Them</h2>

Login to DC-1 as jane_admin.
Open Microsoft Poweshell_ISE as admin.
Create new file and paste the contents of the script into it.
Run the script and observe accounts being created
When finished, open ADUC and observe the accounts in the appropriate OU.
