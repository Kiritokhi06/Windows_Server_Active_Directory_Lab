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

I started by creating GPOs in Group Policy Management. Each GPO was given a name based on its purpose, making it easier to identify and manage the different settings throughout the lab.

Some of the policies I created included password and account-lockout settings, drive mapping, desktop wallpaper, Control Panel restrictions, and USB storage restrictions.

![Creating_GPOs(1)](<screenshots/03-GroupPolicy/General_GPO_Management/Creating_GPOs(1).PNG>)

After creating the GPOs, I configured their individual settings through the Group Policy Management Editor. I also linked policies to the appropriate locations in the domain so that they could be applied to the intended users or computers.

![Creating_GPOs(2)](<screenshots/03-GroupPolicy/General_GPO_Management/Creating_GPOs(2).PNG>)

### Password and Account Lockout Policies

One of the first areas I worked on was password security. Password policies allow admins to define requirements for domain passwords, including minimum length, complexity, and password age. These settings help establish consistent password requirements for domain accounts.

I configured a Password Policy GPO and adjusted several settings, including a minimum password length of 8 characters, password complexity requirements, a maximum password age of 90 days, and a minimum password age of 30 days.

![Password_Policy_GPO(8)](<screenshots/03-GroupPolicy/Password_&_Account_Policies/Password_Policy_GPO(8).PNG>)

I also worked with the Default Domain Policy to review and configure additional password requirements. The settings shown in my screenshots included password history, password age, minimum length, and password complexity.

![Password_Req_Policy(2)](<screenshots/03-GroupPolicy/Password_&_Account_Policies/Password_Req_Policy(2).PNG>)

Next, I created an Account Lockout GPO to limit repeated unsuccessful sign-in attempts. I configured the account lockout threshold to 5 invalid logon attempts, the account lockout duration to 10 minutes, and the counter reset interval to 10 minutes.

These settings allowed me to practice configuring account-lockout behavior to help protect domain accounts against repeated password guesses.

1[Account_Lockout_GPO(3)](<screenshots/03-GroupPolicy/Password_&_Account_Policies/Account_Lockout_GPO(3).PNG>)

I then tested the account-lockout configuration by attempting to sign in with invalid credentials. The client displayed a message indicating that the account was locked and couldn’t be used to sign in.

![Acc_Lockout_Test(2)](<screenshots/03-GroupPolicy/Password_&_Account_Policies/Acc_Lockout_Test(2).PNG>)

### Mapping Network Drives with Group Policy

I also created a GPO to map a shared network folder as a drive for domain users. Drive mapping provides a convenient way for users to access shared folders without having to enter the network path every time.

I created a GPO named **Drive Mapping** and configured the drive mapping under User Configuration using Group Policy Preferences. I specified the network share location and assigned it the drive letter **E**.

![Drive_Mapping_GPO(2)](<screenshots/03-GroupPolicy/Drive_Mapping/Drive_Mapping_GPO(2).PNG>)

The network location used for the mapping was the **Shared Files** folder hosted on the server. I configured the drive mapping to use the shared folder’s network path.

![Network_Drive_GPO_Setup(2)](<screenshots/03-GroupPolicy/Drive_Mapping/Network_Drive_GPO_Setup(2).PNG>)

After configuring the drive mapping, I linked the GPO to the appropriate user OU. This allowed the policy to be applied to users in that OU when they signed in.

![Network_Drive_GPO_Setup(4)](<screenshots/03-GroupPolicy/Drive_Mapping/Network_Drive_GPO_Setup(4).PNG>)

I then used **gpupdate /force** on the client to force a Group Policy update. This command refreshes the applicable computer and user policies rather than waiting for the normal background refresh.

```text
gpupdate /force
```

![Network_Drive_GPO_Setup(5)](<screenshots/03-GroupPolicy/Drive_Mapping/Network_Drive_GPO_Setup(5).PNG>)

After the policy update, I checked File Explorer on the client to verify that the shared network device was available.

![Network_Drive_GPO_Setup(6)](<screenshots/03-GroupPolicy/Drive_Mapping/Network_Drive_GPO_Setup(6).PNG>)

### Configuring a Desktop Wallpaper Policy

Another policy I worked on was desktop wallpaper. The purpose of this GPO was to configure a consistent desktop background for domain users.

I created a GPO named **Desktop Wallpaper** and edited the setting under User Configuration in the Administrative Templates section. I enabled the Desktop Wallpaper policy and reviewed the options for specifying the wallpaper path and display style.

![Desktop_Wallpaper_GPO(1)](<screenshots/03-GroupPolicy/User_Interface_Restrictions/Desktop_Wallpaper_GPO(1).PNG>)

This gave me experience with using Administrative Templates to manage user interface settings centrally. The policy’s configuration also demonstrates how an admin can control a desktop setting through Group Policy rather than requiring each user to configure it manually.

### Restricting Control Panel Access

I created another GPO named **Restrict Control Panel** to restrict access to the Control Panel and PC settings. This type of policy can be useful in environments where admins want to limit the settings users can change on their computers.

I enabled the policy through User Configuration under Administrative Templates and the Control Panel settings.

![Restrict_Control_Panel_GPO(1)](<screenshots/03-GroupPolicy/User_Interface_Restrictions/Restrict_Control_Panel_GPO(1).PNG>)

After configuring the policy, I updated Group Policy on the Windows 11 client and attempted to open the Control Panel. The client displayed a restriction message stating that the operation had been cancelled because of restrictions in effect on the computer.

![Testin_GPOs(6)](<screenshots/03-GroupPolicy/GPO_Testing_&_Validation/Testin_GPOs(6).PNG>)

This confirmed that the restriction was being enforced on the client.

### Disabling USB Storage Access

I also created a GPO named **Disable USB Storage** to restrict access to removable storage devices. This setting is relevant to security because removable drives can be used to copy files onto or off of a computer.

I configured the policy under Computer Configuration in the Administrative Templates section, navigating to System and then Removable Storage Access. I enabled the setting named **All Removable Storage classes: Deny all access**.

![Disable_USB_Storage_GPO(2)](<screenshots/03-GroupPolicy/Device_&_Security_Restrictions/Disable_USB_Storage_GPO(2).PNG>)

Enabling this setting configures Windows to deny access to removable storage classes covered by the policy. 

### Configuring User Rights

In addition to the other restrictions, I created a GPO named **User Rights** to work with user-rights assignments. These settings control which users and groups are allowed or denied certain actions on a computer, including signing in locally or through Remote Desktop Services.

I configured the relevant user-rights settings in the Group Policy Management Editor and specified the accounts or groups to which the restrictions applied.

![User_Rights_GPO(2)](<screenshots/03-GroupPolicy/Device_&_Security_Restrictions/User_Rights_GPO(2).PNG>)

I then tested access from the client. The screenshots document sign-in and Remote Desktop attempts, including an access-denied message indicating that the account wasn’t authorized for remote login.

![User_Rights_GPO(9)](<screenshots/03-GroupPolicy/Device_&_Security_Restrictions/User_Rights_GPO(9).PNG>)

This helped me to better understand how user-rights assignments can affect access independently of whether an account exists in Active Directory.

### Testing and Verifying Group Policy

After configuring the policies, I used the Windows 11 client to check whether the intended settings were taking effect. I also used **gpupdate /force** to refresh policy settings during testing.

![Testin_GPOs(4)](<screenshots/03-GroupPolicy/GPO_Testing_&_Validation/Testin_GPOs(4).PNG>)

One example was the Control Panel restriction, where the client displayed the expected message when I attempted to open a restricted setting. I also tested account-lockout behavior and reviewed the results of the user-rights restrictions.

These tests helped me connect the settings configured on the domain controller with the behavior observed on the client. They also gave me practical experience creating GPOs, linking them to domain objects, refreshing policy, and verifying their effects.

## File Service Permissions

### Setting Up File Sharing

For this part of the lab, I configured file sharing on the Windows Server so that users on the domain could access shared folders over the network. I created a folder named **Shared Files** on the server and opened its properties to configure the sharing settings. I enabled sharing and used the Advanced Sharing options to configure the share permissions.

![File_Sharing_Setup(2)](<screenshots/04-FileServicesPermissions/File_Sharing/File_Sharing_Setup(2).PNG>)

I reviewed the share permissions and added domain users so that access could be managed through the domain. I also reviewed the Security tab, which is where NTFS permissions are configured. Share permissions control access through the network share, while NTFS permissions control access to the folder and its contents on the file system. When a user accesses a folder over the network, both sets of permissions apply.

![File_Sharing_Setup(7)](<screenshots/04-FileServicesPermissions/File_Sharing/File_Sharing_Setup(7).PNG>)

### Testing File Sharing

After configuring the share, I tested it from the Windows 11 client by mapping the network location as a drive. I opened File Explorer, selected the option to map a network drive, and entered the shared folder's network path. This allowed me to access the server's shared folder from the client instead of only accessing it directly on the server.

![Testing_File_Sharing(1)](<screenshots/04-FileServicesPermissions/File_Sharing/Testing_File_Sharing(1).PNG>)

After mapping the drive, I verified that the shared folder appeared in File Explorer on the client. This confirmed that I could reach the share over the network using the configured path.

![Testing_File_Sharing(2)](<screenshots/04-FileServicesPermissions/File_Sharing/Testing_File_Sharing(2).PNG>)

### Configuring NTFS Permissions and Inheritance

I then practiced configuring NTFS permissions on folders and their contents. In one activity, I created a **Common** folder with subfolders named **Events** and **Project**. I also created a domain security group named **Project** to practice assigning permissions to a group rather than managing access only through individual user accounts.

![Perm_and_Inher_Lab1, or (2)](<screenshots/04-FileServicesPermissions/NTFS_Permissions_&_Inheritance/Perm_and_Inher_Lab1(2).PNG>)

I opened the folder's Security settings and reviewed the existing permissions. From Advanced Security Settings, I examined how permissions were inherited from the parent folder. I then practiced changing inheritance so I could manage permissions directly on the folder, including converting inherited permissions into explicit permissions and removing inherited permissions where needed.

![Perm_and_Inher_Lab1(10)](<screenshots/04-FileServicesPermissions/NTFS_Permissions_&_Inheritance/Perm_and_Inher_Lab1(10).PNG>)

I also used the permissions editor to review which users and groups had access and which permissions were allowed. This helped me understand how folder access can be assigned to specific groups and how changes to inheritance affect the permissions applied to subfolders and files.

### Managing Access to Shared Department Folders

In another activity, I worked with a **HRGroup** folder and configured its sharing and security settings. I reviewed the share permissions separately from the NTFS permissions to practice controlling access at both levels.

![2nd_File_Sharing_Activity(1)](<screenshots/04-FileServicesPermissions/File_Sharing/2nd_File_Sharing_Activity(1).PNG>)

I continued working with folder permissions in additional file-sharing activities, including reviewing the permissions assigned to groups and users. These exercises gave me more practice with setting access on shared folders and understanding how permissions are applied to the files and folders inside them.

![3rd_File_Sharing_Activity(4)](<screenshots/04-FileServicesPermissions/File_Sharing/3rd_File_Sharing_Activity(4).PNG>)

### Configuring Access-Based Enumeration

I also configured Access-Based Enumeration (ABE) for a department share. The purpose of ABE is to hide files and folders from users when they don’t have permission to access those items. This helps keep a shared directory easier to navigate and prevents users from seeing folder entries that aren’t intended for them, although ABE doesn’t replace the need to configure the correct permissions.

For this activity, I worked with a **Dept_Shares** folder containing **HR** and **IT** subfolders. I reviewed the domain users and groups involved, including the **IT_Dept** security group, and configured the folder permissions to control access to the department folders.

![1st_ABE_Activity(8)](<screenshots/04-FileServicesPermissions/Access_Based_Enumeration/1st_ABE_Activity(8).PNG>)

I then opened the share settings in Server Manager and enabled access-based enumeration for the **Dept_Shares** share. This setting allows the share to display only the folders and files that the connected user has permission to access.

![1st_ABE_Activity(19)](<screenshots/04-FileServicesPermissions/Access_Based_Enumeration/1st_ABE_Activity(19).PNG>)

Finally, I took the ABE test screenshots to document the client-side checks performed for this activity.

![ABE_Activity_Test(4)](<screenshots/04-FileServicesPermissions/Access_Based_Enumeration/ABE_Activity_Test(4).PNG>)

## File Server Resource Manager (FSRM)

### Installing File Server Resource Manager

For this part of the lab, I installed File Server Resource Manager (FSRM) on the Windows Server VM. FSRM is a Windows Server tool that helps admins manage storage by setting folder quotas and controlling which types of files can be stored in specific locations.

I opened Server Manager, selected Add Roles and Features, and navigated to the File and Storage Services options. From there, I selected File Server Resource Manager and completed the installation. After the installation finished, I opened FSRM through the Windows administrative tools to access its configuration options.

![Installing_FSRM(3)](<screenshots/05-FSRM/General_Setup/Installing_FSRM(3).PNG>)

### Configuring Storage Quotas

After installing FSRM, I opened Quota Management and reviewed the options for creating quotas and quota templates. A quota sets a storage limit for a folder, helping prevent a shared folder from using more disk space than intended. This is useful in a domain environment where multiple users may save files to the same server.

I created a quota for the shared folder by selecting its folder path and opening the custom quota properties. This allowed me to define a storage limit for the folder and configure the quota settings instead of relying only on the default templates.

![Quota_Mgmnt_Setup(1)](<screenshots/05-FSRM/Storage_Quotas/Quota_Mgmnt_Setup(1).PNG>)

I also reviewed the notification threshold settings. These can be configured to notify administrators when a folder reaches a certain percentage of its storage limit. For example, an admin could set a threshold at 80% so there is time to review storage usage before the quota is reached.

![Quota_Mgmnt_Setup(3)](<screenshots/05-FSRM/Storage_Quotas/Quota_Mgmnt_Setup(3).PNG>)

The quota configuration provides a way to manage storage at the folder level rather than allowing shared folders to grow without a defined limit.

![Quota_Mgmnt_Setup(4)](<screenshots/05-FSRM/Storage_Quotas/Quota_Mgmnt_Setup(4).PNG>)

### Configuring File Screening

I then worked with File Screening Management in FSRM. File screening allows administrators to control which file types can be saved to a particular folder. This can help prevent shared folders from filling up with files that aren’t appropriate for their intended use, such as large video files or executable files.

I opened File Screening Management and reviewed the available file screen templates. For this lab, I configured a regular file screen using an existing template rather than creating a custom one. The template applies predefined rules to restrict the file types that can be stored in the selected folder.

![File_Screening_Setup](<screenshots/05-FSRM/File_Screening/File_Screening_Setup.PNG>)

I selected the appropriate template and applied the file screen to the shared folder. This allowed me to practice using FSRM's built-in file screening options to manage the types of files users can save to a network share.

![File_Screening_Setup(2)](<screenshots/05-FSRM/File_Screening/File_Screening_Setup(2).PNG>)

### Reviewing the FSRM Configuration

After configuring quotas and file screening, I reviewed the available settings in the FSRM console. The quota settings provide a way to manage how much storage a folder can use, while file screening provides a way to restrict the types of files stored there. Together, these features give administrators more control over the use of shared storage on a Windows Server.

## Service Account & Sysinternals

### Creating a Service Account

For this part of the lab, I created a service account in Active Directory. Service accounts are used to run specific applications or tasks instead of relying on a regular user account. In this activity, the goal was to configure a dedicated account for a computer that automatically signs in and opens a specific website when it starts.

I created the account through Active Directory Users and Computers and organized it under a separate **Service Accounts** OU to keep it separate from regular user accounts.

![Creating_Service_Acc(1)](<screenshots/06-ServiceAcc&Sysinternals/Service_Account_Management/Creating_Service_Acc(1).PNG>)

### Downloading and Using Sysinternals

I then downloaded Microsoft Sysinternals on the Windows client that was joined to the domain. Sysinternals is a collection of utilities that can help admins manage and troubleshoot Windows systems.

For this activity, I used the **Autologon** utility to configure automatic sign-in with the service account. I entered the account credentials and domain, then enabled the setting.

![Sysinternals(4)](<screenshots/06-ServiceAcc&Sysinternals/Sysinternals_Tools/Sysinternals(4).PNG>)

### Configuring Automatic Startup

After configuring Autologon, I set up the browser to open a specific webpage at startup. I chose to display **google.com**.

![Sysinternals(8)](<screenshots/06-ServiceAcc&Sysinternals/Sysinternals_Tools/Sysinternals(8).PNG>)

I adjusted the computer's power settings so it wouldn’t automatically go to sleep. These settings help the computer remain available without requiring someone to sign in and open the webpage manually each time it starts.

![Sysinternals(14)](<screenshots/06-ServiceAcc&Sysinternals/Sysinternals_Tools/Sysinternals(14).PNG>)

I then tested the configuration by restarting the client computer and checking whether it signed in automatically and opened the webpage as expected.

### Restricting Local Logons with Group Policy

The final part of this activity involved using Group Policy to restrict who could sign in locally to the dedicated computer. I created a GPO and configured the **Deny log on locally** user-right assignment under Computer Configuration, Windows Settings, Security Settings, and Local Policies.

Rather than adding individual users one at a time, I used the existing group containing the regular employees. I then linked the policy to the computer-related OU so that the restriction would apply to the intended computer.

![Sysinternals(16)](<screenshots/06-ServiceAcc&Sysinternals/Sysinternals_Tools/Sysinternals(16).PNG>)

After applying the policy, I tested the configuration to check that the restrictions worked as intended.

## What I Learned
This project gave me hands-on experience with: 

- Windows Server installation and configuration
- Setting up an Active Directory domain
- Creating and organizing Organizational Units (OUs)
- Creating and managing domain users and security groups
- Joining a Windows 11 client to a domain
- Configuring DNS and network adapters for domain connectivity
- Creating and testing Group Policy Objects (GPOs)
- Configuring file shares and NTFS permissions
- Managing permission inheritance and Access-Based Enumeration (ABE)
- Using File Server Resource Manager to configure storage quotas and file screening
- Creating and organizing service accounts
- Using Microsoft Sysinternals Autologon to configure automatic sign-in
- Troubleshooting DNS resolution and domain join issues
- Managing and testing a Windows domain environment in VirtualBox

One of the most useful parts of the project was troubleshooting domain connectivity. I had to adjust the network adapters and DNS settings and remove an unwanted NAT address from the domain’s DNS records before I could successfully join the Windows 11 client to the domain. This was definitely the most rewarding part of the project for me.

This project helped me better understand how Active Directory, DNS, Group Policy, and file permissions work together in a Windows domain environment. It also gave me practical experience configuring and troubleshooting services that are commonly used to manage users, computers, and shared resources on a network. 
