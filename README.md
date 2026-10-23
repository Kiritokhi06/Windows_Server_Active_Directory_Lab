# Windows Server Active Directory Lab

Building a Windows Server home lab in VirtualBox to learn how to manage a Windows domain.

## Overview
I started this project because I wanted to gain more hands-on experience with Windows Server and Active Directory managing users, computers, and other resources on a network.

I followed the Active Directory activities in [EastCharmer's Windows Server Home Lab Project Tutorial](https://youtube.com/playlist?list=PLAdEnQWAAbfXMY2D4HVZOe-ChfTKmaJfQ&si=SUB2OV0Y0p_i0wIO), adapting the process to my VirtualBox environment running Windows Server 2025.

I used Oracle VirtualBox to set up a Windows Server 2025 virtual machine and a Windows 11 client virtual machine. This gave me a way to practice configuring a domain controller and managing a Windows domain without needing any additional physical computers.

Throughout this project, I followed a Windows Server home lab tutorial and worked through different tasks involving Active Directory, user and group management, Group Policy, and file permissions. I also ran into some issues with virtualization and network configuration I had to troubleshoot along the way.

The goal of this project was to build a functional Windows domain environment where I could practice common system administration tasks and better understand how these services work together.

> **Additional Documentation:**
> Screenshots documenting the setup, configuration, and troubleshootin process are available throughout the repository and in the [`screenshots`](screenshots/) directory.

## Virtual Machine Setup
- Virtualization software: Oracle VirtualBox
- Server OS: Windows Server 2025
- Client OS: Windows 11 Pro
- Directory service: Active Directory Domain Services (AD DS)

## Objectives
- Set up Windows Server and Windows 11 virtual machines
- Install and configure Active Directory Domain Services
- Create and manage organizational units, users, and groups
- Join a Windows 11 client to the domain
- Configure and test Group Policy Objects (GPOs)
- Configure file sharing and NTFS permissions
- Practice storage quotas and file screening using File Server Resource Manager (FSRM)
- Troubleshoot virtualization and domain connectivity issues

## Lab Environment
I created a virtual machine in Oracle VirtualBox to run Windows Server 2025. This machine served as the main server for the lab and would later be configured as the domain controller.

The server would be responsible for hosting Active Directory Domain Services (AD DS) and managing domain accounts and other resources used throughout the project.

![AD Domain Controller VM Creation](<screenshots/01-LabEnvironment/VM_Setup/AD Domain Controller VM Creation.PNG>)

I also created a separate virtual machine running Windows 11 Pro. This machine acted as the client on the domain. 

The client was important because I wanted to test the environment from the perspective of a regular domain computer. Instead of only checking configurations on the server, I could attempt to join the client to the domain, sign in with domain credentials, and test policies and shared resources.

![Helpdesk VM Creation](<screenshots/01-LabEnvironment/VM_Setup/Helpdesk VM Creation.PNG>)

### Troubleshooting Virtualization
During the initial setup, I ran into an issue where the virtual machines would abort when I attempted to start them. This prevented me from continuing with the Windows installations and the rest of the lab.

After investigating the problem, I found through Task Manager that virtualization was disabled in my PC’s BIOS. I restarted the computer and entered the BIOS settings to locate the virtualization options.

![Helpdesk VM Virtualization Troubleshooting(1)](<screenshots/07-Troubleshooting/VM_Virtualization_BIOS_Troubleshooting/Helpdesk VM Virtualization Troubleshooting(1).PNG>)

I navigated through the BIOS settings until I located the advanced CPU configuration options. I enabled virtualization and saved changes before restarting the computer.

![BIOS Step 4](<screenshots/07-Troubleshooting/VM_Virtualization_BIOS_Troubleshooting/BIOS Step 4.jpeg>)

After enabling virtualization, I checked Task Manager to make sure virtualization was turned on, and returned to VirtualBox to try starting the virtual machines again. I was able to continue with the Windows installation process and proceed with the rest of the lab.

![Helpdesk VM Virtualization Troubleshooting(3)](<screenshots/07-Troubleshooting/VM_Virtualization_BIOS_Troubleshooting/Helpdesk VM Virtualization Troubleshooting(3).PNG>)

### Verifying the Operating System
After getting the environment running, I used Windows’ built-in version information to check the installed operating system.

I did this by using:

```text
winver
```

![Winver(1)](<screenshots/01-LabEnvironment/OperatingSystems/Winver(1).PNG>)

## Active Directory

### Installing Active Directory Domain Services

With the virtual machines running, I moved on to configuring the Windows Server as a domain controller. I started by installing the Active Directory Domain Services (AD DS) role through Server Manager.

AD DS provides a centralized way to manage users, computers, groups, and authentication within a Windows domain. Installing the role was the first step toward creating the domain that I would use throughout the rest of the lab.

![Install_AD(4)](<screenshots/02-ActiveDirectory/AD_DS_&_Domain_Controller/Install_AD(4).PNG>)

I selected Active Directory Domain Services from the available server roles and continued through the installation wizard. Once the installation finished, I went on to configure the server as a domain controller.

![DC_Setup](<screenshots/02-ActiveDirectory/AD_DS_&_Domain_Controller/DC_Setup.PNG>)

### Promoting the Server to a Domain Controller

After installing AD DS, I used the Active Directory Domain Services Configuration Wizard to promote the server to a domain controller. I configured a new forest for the lab and created the domain **KiritoDomain.local**.

![DC_Setup(1)](<screenshots/02-ActiveDirectory/AD_DS_&_Domain_Controller/DC_Setup(1).PNG>)

During this process, I configured the domain controller options and set the Directory Services Restore Mode (DSRM) password. I continued through the prerequisite checks and completed the promotion process.

![DC_Setup(2)](<screenshots/02-ActiveDirectory/AD_DS_&_Domain_Controller/DC_Setup(2).PNG>)

After the configuration finished, the server restarted. I signed back in and verified that the Active Directory tools were available for managing the domain.

![DC_Setup_Complete(1)](<screenshots/02-ActiveDirectory/AD_DS_&_Domain_Controller/DC_Setup_Complete(1).PNG>)

### Creating Organizational Units

Once the domain controller was configured, I began organizing the directory using Organizational Units (OUs). OUs provide a way to group directory objects, like users and computers, into a structure that makes them easier to manage.

I created OUs to organize the lab’s directory objects by region, including the United States, Europe, and Asia. In the regional OUs, I created additional OUs for computers, servers, and users.

![AD_Creating_OUs(5)](<screenshots/02-ActiveDirectory/Organizational_Units/AD_Creating_OUs(5).PNG>)

This structure gave me a more organized directory to work with as I created accounts and groups and continued configuring the domain.

### Creating Users and Groups

After setting up the OU structure, I created user accounts to represent users in the domain. I entered the account information, configured passwords, and placed accounts in the appropriate OU.

![AD_Creating_OUs_User(1)](<screenshots/02-ActiveDirectory/Users_&_Groups/AD_Creating_OUs_User(1).PNG>)

I also created groups to organize accounts for administrative purposes. For example, I created an IT security group with a global scope. Security groups can be used to assign permissions to a collection of users rather than configuring access for every account individually.

![AD_Creating_OUs_Group(1)](<screenshots/02-ActiveDirectory/Users_&_Groups/AD_Creating_OUs_Group(1).PNG>)

I also created a distribution group named **DL-ITAdmins**. Unlike a security group, a distribution group is intended for email distribution rather than assigning access permissions to resources.

![AD_Creating_OUs_Group(3)](<screenshots/02-ActiveDirectory/Users_&_Groups/AD_Creating_OUs_Group(3).PNG>)

After creating the accounts and groups, I reviewed them in Active Directory Users and Computers to verify that the directory objects were present.

![AD_Creating_OUs_User(4)](<screenshots/02-ActiveDirectory/Users_&_Groups/AD_Creating_OUs_User(4).PNG>)

### Joining the Windows 11 Client to the Domain

With the domain controller and directory structure in place, I moved to the Windows 11 client and attempted to join it to **KiritoDomain.local**. Joining the client to the domain would allow it to use domain accounts and receive centrally managed settings.

I opened the client computer’s system properties and used the computer name and domain settings to initiate the domain join. I entered the domain name and supplied credentials with permission to join the computer.

![Adding_Computer_to_Domain(2)](<screenshots/02-ActiveDirectory/Domain_Join_&_Authentication/Adding_Computer_to_Domain(2).PNG>)

The initial domain join attempt led me to investigate the network configuration between the two virtual machines. Although both machines were running, the client wasn’t resolving the domain correctly. I documented the network troubleshooting separately and used those steps to identify and correct the issue.

### Troubleshooting Domain Connectivity and DNS

Both virtual machines initially used NAT network adapters. NAT itself wasn’t the problem, but the original adapter and DNS configuration wasn’t allowing the client to resolve and reach the domain controller correctly.

I began by checking the network adapter settings and using **ipconfig** to review the IP configuration on the machines. This helped me examine the addresses assigned to the adapters and determine which DNS server the client was using.

![Pre_Troubleshoot_Computer_Domain_Config(2)](<screenshots/07-Troubleshooting/Domain_Join_Netwrk_Troubleshooting/Pre_Troubleshoot_Computer_Domain_Config(2).PNG>)

To correct the configuration, I created additional network adapters on both virtual machines and assigned static IP addresses so they could communicate on the lab network. I configured the client to use the domain controller’s IP address as its DNS server.

![Computer_Domain_Config_Troubleshoot(6)](<screenshots/07-Troubleshooting/Domain_Join_Netwrk_Troubleshooting/Computer_Domain_Config_Troubleshoot(6).PNG>)

While checking DNS, I also found that the domain controller’s NAT address had been registered in DNS. I removed the unwanted entry so that the client wouldn’t resolve the domain controller to the incorrect address.

I then used **nslookup** to check whether the domain name was resolving through the domain controller’s DNS service.

```text
nslookup KiritoDomain.local
```

![Computer_Domain_Config_Troubleshoot(8)](<screenshots/07-Troubleshooting/Domain_Join_Netwrk_Troubleshooting/Computer_Domain_Config_Troubleshoot(8).PNG>)

After correcting the network and DNS configuration, the domain name resolved to the domain controller’s lab IP address. I also tested connectivity between the machines to confirm that they could communicate over the lab network, via **ping**.

![Computer_Domain_Config_Troubleshoot(9)](<screenshots/07-Troubleshooting/Domain_Join_Netwrk_Troubleshooting/Computer_Domain_Config_Troubleshoot(9).PNG>)

With DNS and connectivity working, I reattempted the domain join again on the Windows 11 client.

![Adding_Computer_to_Domain(3)](<screenshots/02-ActiveDirectory/Domain_Join_&_Authentication/Adding_Computer_to_Domain(3).PNG>)

The client successfully joined **KiritoDomain.local**. Windows displayed a confirmation message, and I restarted the client to apply the change.

![Adding_Computer_to_Domain(4)](<screenshots/02-ActiveDirectory/Domain_Join_&_Authentication/Adding_Computer_to_Domain(4).PNG>)

### Testing Domain Authentication

After restarting the client, I tested signing in with a domain user account rather than relying only on the local Windows account. This allowed me to verify that the client could authenticate against the domain.

![Testing_Domain_Login(3)](<screenshots/02-ActiveDirectory/Domain_Join_&_Authentication/Testing_Domain_Login(3).PNG>)

I also worked with the user account settings in Active Directory Users and Computers, including resetting a user’s password and requiring a password change at the next sign-in.

![Testing_Domain_Login_Psw_Reset(1)](<screenshots/02-ActiveDirectory/Domain_Join_&_Authentication/Testing_Domain_Login_Psw_Reset(1).PNG>)

These tests helped me verify the relationship between the domain controller, the client machine, and the user accounts managed through Active Directory.

### Configuring Fine-Grained Password Policies

I also explored fine-grained password policies through the Active Directory Administrative Center. These settings allow different password and account-lockout requirements to be applied to selected users or groups instead of relying exclusively on one domain-wide password policy.

I created password settings objects for the lab and configured their settings through the Password Settings Container.

![Fine_Grained_Passwords(4)](<screenshots/02-ActiveDirectory/Password_Policies/Fine_Grained_Passwords(4).PNG>)

I then configured the policy’s application to a selected group and reviewed the resulting password settings objects.

![Fine_Grained_Passwords(8)](<screenshots/02-ActiveDirectory/Password_Policies/Fine_Grained_Passwords(8).PNG>)

## Group Policy

After setting up Active Directory and joining the Windows 11 client to the domain, I moved on to Group Policy. Group policy allows administrators to manage settings for domain users and computers from a central location. Instead of manually configuring every computer, I could create Group Policy Objects (GPOs) on the domain controller and apply them to the appropriate users or computers.

I used the Group Policy Management console on the domain controller to create and configure the policies for this part of the lab.

![Group_Policy_Mgmt(1)](<screenshots/03-GroupPolicy/General_GPO_Management/Group_Policy_Mgmt(1).PNG>)

### Creating Group Policy Objects

