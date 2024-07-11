# On-Prem AD Setup
## Description
This project outlines the essential setup steps for managing an on-premises Active Directory before exploring Microsoft Intune features. It serves as a step-by-step guide on how to install and configure the Azure AD Connect tool to sync on-premises Active Directory users with Azure Active Directory. I will be configuring Azure AD Connect with a .com domain UPN suffix, using a non-routable .com domain for directory synchronization. The non-routable domain used was created in a previous project, making this a continuation from that project.

Be aware that in many companies, entry-level employees or beginners are not expected to set up all these components from scratch. Typically, everything is already configured. This project is designed to help you understand how these systems are set up, as someone else in the company usually manages the Active Directory

While these tasks are typically handled by systems administrators at the hiring company, starting from scratch will give you a comprehensive understanding of the entire setup process. I set up a Microsoft Server to run Active Directory and configured a Domain Controller to manage a domain. The synchronization of the on-premises Active Directory users with Azure Active Directory was based on the 1000+ users created in a previous project. This lab replicates a business environment setup. The required tools include Microsoft Server 2019 ISO, Windows 10 Enterprise ISO, VirtualBox, Azure AD Connect, and the Azure portal. Note that the installation of VirtualBox and the client machine won't be displayed here, but I will provide the download links below as usual.
## Languages and Utilities Used
  + Active Directory
  + Azure AD Connect
  + Powershell
  + CMD
## Environments Used
  + VirtualBox
  + Microsoft Server 2019
  + Windows 10 Enterprise
  + Azure portal
## Links
  + [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
  + [Windows 10 ISO](https://www.microsoft.com/de-de/evalcenter/download-windows-10-enterprise/)
  + [Windows Sever 2019](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019)
## Program walk-through

As mentioned earlier, I will be using the Active Directory domain named DC, which contains 1000 users and will be synced to the Azure cloud. This AD has already been installed, and the server has been promoted, as shown below. The domain name associated with the on-premises AD is mydomain.com, which will be used to identify users during synchronization with Azure. Please note that this is not a verified domain; it is only being used for testing purposes. In a real company, a verified domain, typically one designated for company activities, would be used

[![Screenshot-2024-07-06-155528.png](https://i.postimg.cc/GtTH3kPR/Screenshot-2024-07-06-155528.png)](https://postimg.cc/T5GRC5HN)

I will be using a previously created user admin that I set up earlier

[![Screenshot-2024-07-06-145924.png](https://i.postimg.cc/GhynKMhr/Screenshot-2024-07-06-145924.png)](https://postimg.cc/dLwftjyS)

Next, I will download AD Connect, but first, I need to install a separate domain server designed specifically for the syncing process. This is because you don't want to use the Active Directory (DC) server for syncing, as it is always a busy server. I will install a new server and name it Adsync. This installation process won't be shown here because it is the same as the DC installation, although Active Directory wasn't configured on this server since it is intended mainly for syncing.

After the installation, I need to give it internet access using Adapter 2, which provides internal network access from the DC domain server. This was previously configured in the Active Directory project when I created the bulk users. The Adsync server will connect to this specific domain controller and will be assigned an internal IP address of 172.16.0.101, automatically provided by the DHCP server I previously configured


[![Screenshot-2024-07-11-175128.png](https://i.postimg.cc/NGHGgkmL/Screenshot-2024-07-11-175128.png)](https://postimg.cc/3W3Q1pLH)


[![Screenshot-2024-07-10-125411.png](https://i.postimg.cc/Hsqz0zjq/Screenshot-2024-07-10-125411.png)](https://postimg.cc/LnVj2ztD)

as shown below, it joined the domain network.

[![Screenshot-2024-07-10-125530.png](https://i.postimg.cc/JzC3Hh2m/Screenshot-2024-07-10-125530.png)](https://postimg.cc/SJVYwm4t)

Next, I joined the Adsync server to the DC domain controller. You can find it listed in the DC domain Active Directory under the "Computers" section


[![Screenshot-2024-07-10-133551.png](https://i.postimg.cc/RFxqX3wV/Screenshot-2024-07-10-133551.png)](https://postimg.cc/67z9QpHD)

I also verified it on the DHCP server to ensure it is listed among the address leases, and it shows up correctly

[![Screenshot-2024-07-10-133707.png](https://i.postimg.cc/NfFG88Zk/Screenshot-2024-07-10-133707.png)](https://postimg.cc/SJ0pk9Sn)

After joining the Adsync server to the DC domain controller, I need to turn off the Internet Explorer Enhanced Security Configuration. This will allow me to log in to the Azure portal and download AD Connect for synchronization

[![Screenshot-2024-07-10-133857.png](https://i.postimg.cc/HL9C1jfL/Screenshot-2024-07-10-133857.png)](https://postimg.cc/7ChRgPPv)

I encountered challenges downloading it due to the limited size of my CPU RAM, which slowed down the process. To resolve this, I set the VM to bidirectional, allowing me to download it directly on my host machine and then move it back to the VM after downloading

[![Screenshot-2024-07-10-140216.png](https://i.postimg.cc/k4gLrsQj/Screenshot-2024-07-10-140216.png)](https://postimg.cc/Y4TdY1yg)

Next, I logged into my Azure portal to download AD Connect. Be aware of the difference between the AD Connect options; as shown below, I will be downloading the "Download Connect Sync Agent" because I want to sync from on-premises to the Azure cloud. Note that you must have an Azure Active Directory Premium P2 trial to enable the download of AD Connect

[![Screenshot-2024-07-10-135624.png](https://i.postimg.cc/G3NVNkHQ/Screenshot-2024-07-10-135624.png)](https://postimg.cc/14GvVV0n)


During the installation of AD Connect, I agreed to the license terms and clicked on Continue

[![Screenshot-2024-07-10-140436.png](https://i.postimg.cc/BnyCZgfY/Screenshot-2024-07-10-140436.png)](https://postimg.cc/68rZYC3n)

Next, when synchronizing your on-premises Active Directory with Azure Active Directory, you must have a verified domain in Azure Active Directory. Only UPNs associated with the Azure Active Directory domain are synchronized. However, any UPN containing a non-routable domain such as ".com" or ".local" will be synchronized to an onmicrosoft.com domain

In our case, I see a warning that mydomain.com is not a routable domain. It is recommended to use custom settings to configure user sign-in options. I want to remind you that I do not have a verified domain in my Azure Active Directory, and we are using the non-routable domain mydomain.com for Microsoft Azure Active Directory

Now, I'm going to click on "Customize" for a custom installation. This will give me all the options to choose from

[![Screenshot-2024-07-10-140656.png](https://i.postimg.cc/rwdSHrRS/Screenshot-2024-07-10-140656.png)](https://postimg.cc/PCjppN4x)

On the "Install Required Components" screen, you can customize settings for Azure Active Directory Connect

Selecting the first option allows you to specify a custom installation location. By default, Azure AD Connect uses SQL Express Edition for storage. For larger environments or high availability needs, choose the second option, "Use an existing SQL Server," which requires SQL to be installed on one of your servers. In this demo, we won't select any additional options. After making your selections, click "Install" to proceed

[![Screenshot-2024-07-10-140928.png](https://i.postimg.cc/BvH2Sfgb/Screenshot-2024-07-10-140928.png)](https://postimg.cc/FfFYpqwX)


Next, I'll choose the sign-in options. There are various options available, and you need to select the one that best suits your organization. Most organizations choose Password Hash Synchronization. You can also enable Single Sign-On. For this demo, we will use Password Hash Synchronization

[![Screenshot-2024-07-10-141430.png](https://i.postimg.cc/RF0CPcNm/Screenshot-2024-07-10-141430.png)](https://postimg.cc/Mch8Hf73)

Next On the "Connect to Azure AD" screen, enter your Azure Active Directory account credentials obtained from your Microsoft Azure account. Ensure the user has global administrator privileges. it is connecting to microsoft online to verify the username and password. Then, click "Next.

[![Screenshot-2024-07-10-143726.png](https://i.postimg.cc/XqSjntsj/Screenshot-2024-07-10-143726.png)](https://postimg.cc/r0jXJZ63)

On the "Connect Your Directory" screen, you will need to add a local Active Directory. Under the forest selector, choose your directory and then click "Add." In my case, I select mydomain.com and click "Add Directory" to include the local Active Directory.

A pop-up window will prompt me to either create a new account or use an existing account. This account will be used for directory synchronization

[![Screenshot-2024-07-10-145223.png](https://i.postimg.cc/jSQJm4dn/Screenshot-2024-07-10-145223.png)](https://postimg.cc/w1MvsJN9)

I added the previously created administrator as an admin to the enterprise account, as shown below

[![Screenshot-2024-07-10-145614.png](https://i.postimg.cc/DwdC2Nc2/Screenshot-2024-07-10-145614.png)](https://postimg.cc/NKF8DpVV)

The domain administrator and it's password were entered as seen below

[![Screenshot-2024-07-10-145844.png](https://i.postimg.cc/DfXPvD2t/Screenshot-2024-07-10-145844.png)](https://postimg.cc/nMnDT39k)

After clicking ok click ok the wizard will create a synchronization account with the required permissions I can confirm that mydomain.com is added

[![Screenshot-2024-07-10-150004.png](https://i.postimg.cc/vTZyz9Hn/Screenshot-2024-07-10-150004.png)](https://postimg.cc/sQqbfMWf)

Click "Next" to proceed. I noticed the following warning due to the UPN suffix "mydomain.com" not being routable and therefore unable to be verified in the tenant. To continue, select "Continue without matching all UPN suffixes to verify domains" by checking this checkbox

Once selected, click "Next." Any UPN containing a non-routable domain, such as "mydomain," will synchronize with the "on.microsoft.com" domain name. Please note that I do not have a verified domain in my Azure Active Directory tenant

[![Screenshot-2024-07-10-150624.png](https://i.postimg.cc/9f0NJpGC/Screenshot-2024-07-10-150624.png)](https://postimg.cc/SX0rKcTv)

Click "Next" to continue. On the "Domain and OU Filtering" screen, I have the option to sync the entire Active Directory data by leaving everything as default. Alternatively, I can filter this data by selecting specific domains and OUs. In this case I will only synchronize the "Admin" and "Users" OU. Choose the "Sync selected domains and OUs" option. Clear the checkbox in front of "mydomain.com", expand it, and then select the OUs you wish to sync. For example, I will only select "Admin" and "Users"

[![Screenshot-2024-07-10-151219.png](https://i.postimg.cc/Qxq4DPmj/Screenshot-2024-07-10-151219.png)](https://postimg.cc/qzzXX1WF)

Click "Next." For this step, I recommend sticking with the default settings for basic setup. For more complex configurations, you might consider other options where you need to match users using specific attributes across all directories. Here, I will proceed with the default selection

[![Screenshot-2024-07-10-151349.png](https://i.postimg.cc/nL9GXw4j/Screenshot-2024-07-10-151349.png)](https://postimg.cc/cKZnjh50)


Click "Next." On the "Filter Users and Devices" tab, I have the option to sync all users and devices or specify a specific group. I will proceed with the default setting to sync all users and devices.

Click "Next." On the "Optional Features" tab, choose any additional features you would like to activate. Each feature has an information icon for more details. Note that the first two options are grayed out since I do not have Exchange in our forest. You can also enable "Password Writeback" and then enable "Self-Service Password Reset" in your Azure Active Directory.

[![Screenshot-2024-07-10-152633.png](https://i.postimg.cc/LXH5cJ89/Screenshot-2024-07-10-152633.png)](https://postimg.cc/PLRdZrD9)

Click "Next." Ensure that you have selected the checkbox to start the synchronization process when the configuration completes. Then, click "Install." The installation process will take some time to complete the setup of Microsoft Azure Active Directory Connect on THE Windows Server 2019 domain controller

[![Screenshot-2024-07-10-152849.png](https://i.postimg.cc/X7Nhr4pr/Screenshot-2024-07-10-152849.png)](https://postimg.cc/YjV8VcN7)

The Azure AD Connect configuration has been completed, and the synchronization process has been initiated as seen below

[![Screenshot-2024-07-10-153513.png](https://i.postimg.cc/x8ZKjvXy/Screenshot-2024-07-10-153513.png)](https://postimg.cc/dZ874TmL)

I logged into the Azure account, and under "Azure AD Connect," I can now see that it is enabled. The last synchronization occurred less than one hour ago this indicates the 1001 users as been sync in less than an hour


[![Screenshot-2024-07-10-154248.png](https://i.postimg.cc/d00rCNGm/Screenshot-2024-07-10-154248.png)](https://postimg.cc/Mnhn2DPv)

Under the global admin portal one can see the 1000 users been sync

[![Screenshot-2024-07-10-154439.png](https://i.postimg.cc/SQ15YH8V/Screenshot-2024-07-10-154439.png)](https://postimg.cc/yW9nwQLS)

I decides to click on users and can see the users from our Admin and Users oganisation unit (OU) under on premise sync enabled column I can see yes for our user with their names this means it was really integrated from the on premises environment

[![Screenshot-2024-07-10-154605.png](https://i.postimg.cc/NM43nMKw/Screenshot-2024-07-10-154605.png)](https://postimg.cc/qhNDhpcm)

Let's navigate to the Windows Server 2019 virtual machine. Click on the Start menu, then select "Synchronization Service. The synchronization service has already started

[![Screenshot-2024-07-10-154850.png](https://i.postimg.cc/x8gbqKqF/Screenshot-2024-07-10-154850.png)](https://postimg.cc/9wRfnwdG)

Navigate to "emburgvicmic.microsoft.com" and view the profile name.

Under "Export," click on "Add." Here, we have a total of 1001 objects that have been synced with our Azure Active Directory

[![Screenshot-2024-07-10-155121.png](https://i.postimg.cc/3NpYc7Qj/Screenshot-2024-07-10-155121.png)](https://postimg.cc/fSwGVGLk)

when you double click on the user it shows the users properties and full details of the synchronisation

[![Screenshot-2024-07-10-155238.png](https://i.postimg.cc/s2tL0Wj8/Screenshot-2024-07-10-155238.png)](https://postimg.cc/yJynDkSX)

Let's perform a final verification on the Microsoft 365 Admin Center to ensure integration and synchronization. Hurray!

[![Screenshot-2024-07-10-160530.png](https://i.postimg.cc/WzfgXGjq/Screenshot-2024-07-10-160530.png)](https://postimg.cc/wRL1Bs19)

The account and Active Directory Services are well configured and integrated. Now, let's test what I have done so far to ensure it works correctly. I'll go to the DC domain and create a new user to see if it will sync to Azure and the Microsoft Admin Center

[![Screenshot-2024-07-11-175554.png](https://i.postimg.cc/X75L3qV9/Screenshot-2024-07-11-175554.png)](https://postimg.cc/TpRgJR5P)

[![Screenshot-2024-07-11-175847.png](https://i.postimg.cc/0ydCM4KQ/Screenshot-2024-07-11-175847.png)](https://postimg.cc/NKFXZp4q)

Right now, the user "aaada" might start appearing in the cloud after 30 minutes through AD Connect. During this synchronization period, AD Connect checks for any changes in the Active Directory and transfers them to the cloud. Once you log into your Microsoft Admin Center portal and click on "Admin," you'll see that the last AD sync was completed 6 minutes ago. This indicates that everything is working correctly, and it shows the synchronization status within the company

[![Screenshot-2024-07-11-180240.png](https://i.postimg.cc/Wb0GCynn/Screenshot-2024-07-11-180240.png)](https://postimg.cc/0b2KJchM)

But if you navigate to "Active Users," you won't see the new user called "aaada" immediately because it takes approximately 30 minutes for AD Connect to sync the user to the cloud

[![Screenshot-2024-07-11-180409.png](https://i.postimg.cc/tTHySYCD/Screenshot-2024-07-11-180409.png)](https://postimg.cc/kVwkDMSt)

To expedite the sync process, I use PowerShell to configure it and import the sync module from the Microsoft community. The link will be displayed here

[![Screenshot-2024-07-11-181119.png](https://i.postimg.cc/RhkTK5T3/Screenshot-2024-07-11-181119.png)](https://postimg.cc/vxv9wKkM)

Now, within a few seconds, you can verify that the new user called "aaada" has been synchronized in both Azure and the Microsoft Admin Center

[![Screenshot-2024-07-11-181658.png](https://i.postimg.cc/mgGvW0G2/Screenshot-2024-07-11-181658.png)](https://postimg.cc/TpC7rNwZ)

Same in Azure Portal

[![Screenshot-2024-07-11-181755.png](https://i.postimg.cc/HLYFVXxq/Screenshot-2024-07-11-181755.png)](https://postimg.cc/75RKRCS9)

One more tip incase your Password Hash Synchronization is skipped or not connected with Microsoft Entra ID. As a result passwords will not be synchronized with Microsoft Entra ID.

Recommended action
Restart Microsoft Entra Sync Services:
Please note that any synchronization operations that are currently running will be interrupted. You can choose to perform below steps when no synchronization operation is in progress.
1. Click Start, click Run, type Services.msc, and then click OK.
2. Locate Microsoft Entra Sync, right-click it, and then click Restart.
