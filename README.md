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
