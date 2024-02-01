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

