# On-Prem AD Setup, and Intune
## Description
This project outlines the essential setup steps for managing an on-premises Active Directory before exploring Microsoft Intune features. It serves as a step-by-step guide on how to install and configure the Azure AD Connect tool to sync on-premises Active Directory users with Azure Active Directory. I will be configuring Azure AD Connect with a .com domain UPN suffix, using a non-routable .com domain for directory synchronization. The non-routable domain used was created in a previous project, making this a continuation from that project.

Be aware that in many companies, entry-level employees or beginners are not expected to set up all these components from scratch. Typically, everything is already configured. This project is designed to help you understand how these systems are set up, as someone else in the company usually manages the Active Directory

While these tasks are typically handled by systems administrators at the hiring company, starting from scratch will give you a comprehensive understanding of the entire setup process. I set up a Microsoft Server to run Active Directory and configured a Domain Controller to manage a domain. The synchronization of the on-premises Active Directory users with Azure Active Directory was based on the 1000+ users created in a previous project. This lab replicates a business environment setup. The required tools include Microsoft Server 2019 ISO, Windows 10 Enterprise ISO, VirtualBox, Azure AD Connect, and the Azure portal. Note that the installation of VirtualBox and the client machine won't be displayed here, but I will provide the download links below as usual.
## Languages and Utilities Used
  + Active Directory
  + Azure AD Connect
  + Azure portal
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

As previously mentioned, I will be using the Active Directory domain called DC, which contains 1000 users and will be synced to the Azure cloud. This AD has already been installed and the server promoted, as shown below. The domain name associated with the on-premises AD is mydomain.com, which will be used for identifying users during synchronization with Azure. Note that this is not a verified domain; it is only being used for testing purposes. In a real company, a verified domain would be used, typically a specific domain designated for company activities.

[![Screenshot-2024-07-06-155528.png](https://i.postimg.cc/GtTH3kPR/Screenshot-2024-07-06-155528.png)](https://postimg.cc/T5GRC5HN)

I'll be using a created user which will an administrator who is assume to be in charge of this task as showned below

[![Screenshot-2024-07-06-145924.png](https://i.postimg.cc/GhynKMhr/Screenshot-2024-07-06-145924.png)](https://postimg.cc/dLwftjyS)

Next I'm going to go ahead and download AD Connect but first I need to install another seperate domain server that is designed for syncing process because you don't want to use active directory (DC) for syncing because it is always a busy server. so I go ahead and install a new server and call it Adsync. this process won't be showned here because is the same process of installation for the DC although the Active direcory wasn't configure on this server because it is mainly syncing process. 

After the installation i have to give it an ineternet access and this will be using the Adapter 2 internal network access from the DC domain server which was previously configured in the Active directory project which i did when creating the bulk users because I want to connect this adsync to this specific domain controller this adsync server will be using an internal ip address 172.16.0.101 automatically given by the DHCP server which it's been lease by the dhcp server i previously configured.


[![Screenshot-2024-07-11-175128.png](https://i.postimg.cc/NGHGgkmL/Screenshot-2024-07-11-175128.png)](https://postimg.cc/3W3Q1pLH)


[![Screenshot-2024-07-10-125411.png](https://i.postimg.cc/Hsqz0zjq/Screenshot-2024-07-10-125411.png)](https://postimg.cc/LnVj2ztD)

seen as it is joined to the domain network

[![Screenshot-2024-07-10-125530.png](https://i.postimg.cc/JzC3Hh2m/Screenshot-2024-07-10-125530.png)](https://postimg.cc/SJVYwm4t)

Next was to join the Adsyn server to the DC domain controller which i did and you find it in the DC domain active directory under the computer section

[![Screenshot-2024-07-10-133551.png](https://i.postimg.cc/RFxqX3wV/Screenshot-2024-07-10-133551.png)](https://postimg.cc/67z9QpHD)

i also verify it on the Dhcp server to see if is been listed among the address leases and it shows

[![Screenshot-2024-07-10-133707.png](https://i.postimg.cc/NfFG88Zk/Screenshot-2024-07-10-133707.png)](https://postimg.cc/SJ0pk9Sn)

After joining the Adsyn server to DC domain controller i need to put of the internet exploer ehanced security configuration off to enable me login to assure portal so i can download the AD Connect for the sychronisation

[![Screenshot-2024-07-10-133857.png](https://i.postimg.cc/HL9C1jfL/Screenshot-2024-07-10-133857.png)](https://postimg.cc/7ChRgPPv)

I had chalenges downloading it due to the size of my CPU ram which slow down the process so i set the VM to be bidirectional which made it easier for me to download it directly on my host machine amd move it back the VM after downloading.

[![Screenshot-2024-07-10-140216.png](https://i.postimg.cc/k4gLrsQj/Screenshot-2024-07-10-140216.png)](https://postimg.cc/Y4TdY1yg)

Next i login into my Aure portal so i can download the AD connect, beware of the difference between the AD connect as seen below i will be downloading the Download Connect Sync Agent because i want to sync from on premesis to azure cloud. Note you must have azure active directory premium p2 trial to enable you download the AD Connect

[![Screenshot-2024-07-10-135624.png](https://i.postimg.cc/G3NVNkHQ/Screenshot-2024-07-10-135624.png)](https://postimg.cc/14GvVV0n)


During installation of AD connect I agree to the license terms and click on continue

[![Screenshot-2024-07-10-140436.png](https://i.postimg.cc/BnyCZgfY/Screenshot-2024-07-10-140436.png)](https://postimg.cc/68rZYC3n)

Next when you synchronize your on premises active directory with azure active directory you have to have a verified domain in azure active directory only the upns that are associated with the anti-masses active directory domain are synchronized however any upn that contains a non-routable domain such as dot com or local will be synchronized to on domain in our case I can see warning mydomain.com is not routable domain it is recommended to use custom settings to configure user sign-in options. i want to remind you that i don't have verified domain in my azure active directory and we are using mydomain.com domain which is non-routable domain for microsoft azure active directory so now i'm going to click on customize for custom installation it will gives us all the options which we can decide for ourselves

[![Screenshot-2024-07-10-140656.png](https://i.postimg.cc/rwdSHrRS/Screenshot-2024-07-10-140656.png)](https://postimg.cc/PCjppN4x)

On the "Install Required Components" screen, you can customize settings for Azure Active Directory Connect.

Selecting the first option allows you to specify a custom installation location. By default, Azure AD Connect uses SQL Express Edition for storage. For larger environments or high availability needs, choose the second option, "Use an existing SQL Server," which requires SQL to be installed on one of your servers. In this demo, we won't select any additional options. After making your selections, click "Install" to proceed.

[![Screenshot-2024-07-10-140928.png](https://i.postimg.cc/BvH2Sfgb/Screenshot-2024-07-10-140928.png)](https://postimg.cc/FfFYpqwX)


Next, we will choose the sign-in options. There are various options available, and you need to select the one that best suits your organization. Most organizations choose Password Hash Synchronization. You can also enable Single Sign-On. For this demo, we will use Password Hash Synchronization.

[![Screenshot-2024-07-10-141430.png](https://i.postimg.cc/RF0CPcNm/Screenshot-2024-07-10-141430.png)](https://postimg.cc/Mch8Hf73)

Next On the "Connect to Azure AD" screen, enter your Azure Active Directory account credentials obtained from your Microsoft Azure account. Ensure the user has global administrator privileges. it is connecting to microsoft online to verify the username and password. Then, click "Next.

[![Screenshot-2024-07-10-143726.png](https://i.postimg.cc/XqSjntsj/Screenshot-2024-07-10-143726.png)](https://postimg.cc/r0jXJZ63)

On the "Connect Your Directory" screen, you will need to add a local Active Directory. Under the forest selector, choose your directory and then click "Add." In our case, select mydomain.com and click "Add Directory" to include the local Active Directory.

A pop-up window will prompt you to either create a new account or use an existing account. This account will be used for directory synchronization.

[![Screenshot-2024-07-10-145223.png](https://i.postimg.cc/jSQJm4dn/Screenshot-2024-07-10-145223.png)](https://postimg.cc/w1MvsJN9)

I added the administrator which i creates to the enterprise account as showned below

[![Screenshot-2024-07-10-145614.png](https://i.postimg.cc/DwdC2Nc2/Screenshot-2024-07-10-145614.png)](https://postimg.cc/NKF8DpVV)

The domain administrator and it's password were entered as seen below

[![Screenshot-2024-07-10-145844.png](https://i.postimg.cc/DfXPvD2t/Screenshot-2024-07-10-145844.png)](https://postimg.cc/nMnDT39k)

After clicking ok click ok the wizard will create a synchronization account with the required permissions I can confirm that mydomain.com is added

[![Screenshot-2024-07-10-150004.png](https://i.postimg.cc/vTZyz9Hn/Screenshot-2024-07-10-150004.png)](https://postimg.cc/sQqbfMWf)

Click "Next" to proceed.

I noticed the following warning due to the UPN suffix "mydomain.com" not being routable and therefore unable to be verified in the tenant. To continue, select "Continue without matching all UPN suffixes to verify domains" by checking this checkbox.

Once selected, click "Next."

Any UPN containing a non-routable domain, such as "mydomain," will synchronize with the "on.microsoft.com" domain name.

Please note that I do not have a verified domain in my Azure Active Directory tenant

[![Screenshot-2024-07-10-150624.png](https://i.postimg.cc/9f0NJpGC/Screenshot-2024-07-10-150624.png)](https://postimg.cc/SX0rKcTv)

Click "Next" to continue.

On the "Domain and OU Filtering" screen, we have the option to sync the entire Active Directory data by leaving everything as default. Alternatively, we can filter this data by selecting specific domains and OUs.

In our example, we will only synchronize the "Admin" and "Users" OU. Choose the "Sync selected domains and OUs" option.

Clear the checkbox in front of "mydomain.com", expand it, and then select the OUs you wish to sync. For example, I will only select "Admin" and "Users".

[![Screenshot-2024-07-10-151219.png](https://i.postimg.cc/Qxq4DPmj/Screenshot-2024-07-10-151219.png)](https://postimg.cc/qzzXX1WF)

Click "Next."

For this step, I recommend sticking with the default settings for basic setup. For more complex configurations, you might consider other options where you need to match users using specific attributes across all directories.

Here, we will proceed with the default selection.

[![Screenshot-2024-07-10-151349.png](https://i.postimg.cc/nL9GXw4j/Screenshot-2024-07-10-151349.png)](https://postimg.cc/cKZnjh50)


Click "Next."

On the "Filter Users and Devices" tab, we have the option to sync all users and devices or specify a specific group. We will proceed with the default setting to sync all users and devices.

Click "Next."

On the "Optional Features" tab, choose any additional features you would like to activate.

Each feature has an information icon for more details. Note that the first two options are grayed out since we do not have Exchange in our forest.

You can also enable "Password Writeback" and then enable "Self-Service Password Reset" in your Azure Active Directory.

[![Screenshot-2024-07-10-152633.png](https://i.postimg.cc/LXH5cJ89/Screenshot-2024-07-10-152633.png)](https://postimg.cc/PLRdZrD9)

Click "Next."

Ensure that you have selected the checkbox to start the synchronization process when the configuration completes. Then, click "Install."

The installation process will take some time to complete the setup of Microsoft Azure Active Directory Connect on our Windows Server 2019 domain controller.

[![Screenshot-2024-07-10-152849.png](https://i.postimg.cc/X7Nhr4pr/Screenshot-2024-07-10-152849.png)](https://postimg.cc/YjV8VcN7)

The Azure AD Connect configuration has been completed, and the synchronization process has been initiated as seen below

[![Screenshot-2024-07-10-153513.png](https://i.postimg.cc/x8ZKjvXy/Screenshot-2024-07-10-153513.png)](https://postimg.cc/dZ874TmL)

I logged into the Azure account, and under "Azure AD Connect," we can now see that it is enabled. The last synchronization occurred less than one hour ago this indicates the 1000 users as been sync in less than an hour


[![Screenshot-2024-07-10-154248.png](https://i.postimg.cc/d00rCNGm/Screenshot-2024-07-10-154248.png)](https://postimg.cc/Mnhn2DPv)

Under the global admin portal one can see the 1000 users been sync

[![Screenshot-2024-07-10-154439.png](https://i.postimg.cc/SQ15YH8V/Screenshot-2024-07-10-154439.png)](https://postimg.cc/yW9nwQLS)

I decides to click on users and can see the users from our Admin and Users oganisation unit (OU) under on premise sync enabled column I can see yes for our user with their names this means it was really integrated from the on premises environment.

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

The account and an active directory Services account are well configureed and integrated so now let's test few things what we have
done so far and does it really work so what I want you to do is to go to the DC domain and create a new user to see if it will sync to Azure and microsoft admin centre

[![Screenshot-2024-07-11-175554.png](https://i.postimg.cc/X75L3qV9/Screenshot-2024-07-11-175554.png)](https://postimg.cc/TpRgJR5P)

[![Screenshot-2024-07-11-175847.png](https://i.postimg.cc/0ydCM4KQ/Screenshot-2024-07-11-175847.png)](https://postimg.cc/NKFXZp4q)

Right now the user aaada might start showing up after 30 minutes inside the cloud from the Ad connect what this sync is doing after 30 minutes it's going to go out and see any changes in AC directory and it's going to move them to our cloud and so once you log into your microsoft admin centre portal office.com and you click on admin you see that as the ad last sync was done here 6 minutes ago so everything looks good and this shows your synchronization status in the company

[![Screenshot-2024-07-11-180240.png](https://i.postimg.cc/Wb0GCynn/Screenshot-2024-07-11-180240.png)](https://postimg.cc/0b2KJchM)

but if you go to active users we don't see the new user called aaada because it takes about 30 minutes for Ad connect to sync to cloud

[![Screenshot-2024-07-11-180409.png](https://i.postimg.cc/tTHySYCD/Screenshot-2024-07-11-180409.png)](https://postimg.cc/kVwkDMSt)

To make this process faster I use Powershell to configure the process and import the sync module from the microsoft coummunity. the link will be display on here

[![Screenshot-2024-07-11-181119.png](https://i.postimg.cc/RhkTK5T3/Screenshot-2024-07-11-181119.png)](https://postimg.cc/vxv9wKkM)

now few seconds you can verify that the new user called aaada has been snychronise in both azure and microsoft admin center

[![Screenshot-2024-07-11-181658.png](https://i.postimg.cc/mgGvW0G2/Screenshot-2024-07-11-181658.png)](https://postimg.cc/TpC7rNwZ)

In Azure Portal

[![Screenshot-2024-07-11-181755.png](https://i.postimg.cc/HLYFVXxq/Screenshot-2024-07-11-181755.png)](https://postimg.cc/75RKRCS9)
