<p align="center">
<img src="https://i.imgur.com/cUPgsW9.png" height="60%" width="60%" alt="AD"/>
</p>

# How to Set Up a Basic Home Lab Running Active Directory & PowerShell

Welcome to the "How to Setup a Basic Home Lab Running Active Directory" repository. This guide provides step-by-step instructions for creating a functional home lab environment that includes Active Directory (AD). This setup is perfect for those looking to enhance their IT skills, prepare for certifications, or create a testing environment.

## Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Hardware Requirements](#hardware-requirements)
- [Software Requirements](#software-requirements)
- [Step-by-Step Guide](#step-by-step-guide)
  - [Step 1: Preparing the Environment](#step-1-preparing-the-environment)
  - [Step 2: Installing the Operating System](#step-2-installing-the-operating-system)
  - [Step 3: Setting Up Active Directory](#step-3-setting-up-active-directory)
  - [Step 4: Configuring Active Directory](#step-4-configuring-active-directory)
  - [Step 5: Additional Configurations](#step-5-additional-configurations)
- [Walkthrough](#walkthrough)
- [Troubleshooting](#troubleshooting)
- [Resources](#resources)
- [Contributing](#contributing)
- [License](#license)

## Introduction

Active Directory (AD) is a directory service developed by Microsoft for Windows domain networks. It is widely used for managing permissions and access to networked resources. This guide will help you set up a basic home lab to practice and learn about Active Directory.

## Prerequisites

Before starting, ensure you have:

- Basic knowledge of networking and Windows Server
- Administrative privileges on your system
- A stable internet connection

## Hardware Requirements

- A physical or virtual machine with at least:
  - 4 GB of RAM
  - 40 GB of disk space
  - Dual-core processor

## Software Requirements

- Windows Server (2016, 2019, or later)
- Hyper-V, VMware, or another virtualization software (if using virtual machines)

## Step-by-Step Guide

### Step 1: Preparing the Environment

1. **Set up your physical or virtual machine.**
   - Ensure your hardware meets the requirements.
   - Install your preferred virtualization software if using VMs.

2. **Download Windows Server ISO.**
   - Obtain the Windows Server ISO from the official Microsoft website or another legitimate source.

### Step 2: Installing the Operating System

1. **Install Windows Server.**
   - Boot your machine from the Windows Server ISO.
   - Follow the installation prompts and complete the setup.

2. **Configure initial settings.**
   - Set up your server with a static IP address.
   - Join the server to a workgroup.

### Step 3: Setting Up Active Directory

1. **Install AD DS (Active Directory Domain Services).**
   - Open Server Manager.
   - Add roles and features.
   - Select AD DS and complete the installation.

2. **Promote the server to a domain controller.**
   - Open Server Manager.
   - Promote this server to a domain controller.
   - Follow the wizard to create a new forest and domain.

### Step 4: Configuring Active Directory

1. **Create Organizational Units (OUs).**
   - Open Active Directory Users and Computers.
   - Create OUs for better management and organization.

2. **Add users and groups.**
   - Create user accounts and security groups as needed.

3. **Configure Group Policies.**
   - Open Group Policy Management.
   - Create and link GPOs to manage settings across your domain.

### Step 5: Additional Configurations

1. **Set up DNS.**
   - Ensure your DNS server is correctly configured.
   - Verify DNS settings to avoid connectivity issues.

2. **Install additional roles and features as needed.**
   - Consider installing DHCP, File Services, or other roles based on your needs.

## Walkthrough

Creating a full-blown Active Directory lab on your personal computer using VirtualBox is a valuable exercise that can greatly enhance your understanding of Windows networking. To start, you'll need to download and install Oracle VirtualBox. Next, download both a Windows 10 ISO and a Server 2019 ISO. Our first task will be to create a virtual machine (VM) that will serve as our domain controller. This VM will house Active Directory, and it should be equipped with two network adapters: one for connecting to the outside internet and the other for connecting to the VirtualBox private network, which our clients will use.

After creating the VM, install Server 2019 on it. We'll need to assign IP addressing for the internal network while the external network will automatically receive IP addressing from your home network or router, eliminating the need for manual configuration. Once the IP addressing is set up, we’ll proceed to name the server, install Active Directory, and create our domain. Additionally, we'll configure routing to ensure that clients on the private network can access the internet through the domain controller. Setting up DHCP on the domain controller is also crucial as it will enable the Windows 10 machine to automatically obtain an IP address.

Before creating our client virtual machine, we'll run a PowerShell script on the domain controller to automatically create a thousand users in Active Directory. This step will showcase the utility of PowerShell and demonstrate the various tasks it can perform. Following this, we'll create another VM and install Windows 10 on it. This VM, named "Client1," will be connected to the private VirtualBox network. We'll then join it to the domain and log in using one of our domain accounts.

By the end of this tutorial, you will have built a basic Windows networking environment with Active Directory and a few networking services. This setup not only provides a practical learning experience but also lays a solid foundation for more advanced networking concepts.



    Review the diagram:
<p align="center">
<br/>
<img src="https://i.imgur.com/N8XxLVQ.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Download and install Oracle VirtualBox:
<p align="center">
<br/>
<img src="https://i.imgur.com/FDaJu29.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Also download and install the VirtualBox Extension Pack as well:
<p align="center">
<br/>
<img src="https://i.imgur.com/mmBiAx9.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Download Windows 10 ISO, it will download as MediaCreationTool_22H2:
<p align="center">
<br/>
<img src="https://i.imgur.com/7ieZF9O.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Download Windows Server 2019:
<p align="center">
<br/>
<img src="https://i.imgur.com/ozCzxqI.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Configure Server 2019 virtual machine, we'll call it "DC" for Domain Controller -- nice and easy to remember:
<p align="center">
<br/>
<img src="https://i.imgur.com/gkwfHMJ.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/29eLAkK.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/CuLt7p9.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/P9mTqTa.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Configure additional settings:
    - Remember when configuring network settings, we're creating our domain controller so we want two NICs. One that's dedicated to the internet that runs NAT, and the second is dedicated to the internal VirtualBox network:
<p align="center">
<br/>
<img src="https://i.imgur.com/HVov5hn.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/WdxxwMW.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/uOVzfv4.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Start the "DC" VM, and this is where we'll select the Server 2019 ISO. For me, I had to select it and reboot:
<p align="center">
<br/>
<img src="https://i.imgur.com/8yQzQnJ.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    It may take a while to reach this screen due to loading time, but once you've reached it, we'll just install by clicking next, and then Install Now:
<p align="center">
<br/>
<img src="https://i.imgur.com/Mx6Fkio.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/w6PxqyZ.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Select 'Windows Server 2019 Standard Evaluation (Desktop Experience)':
<p align="center">
<br/>
<img src="https://i.imgur.com/5wVf2s4.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>


    After accepting license terms, select which type of installation you want:
    - We're going to format the hard drive and install it from scratch so select "Custom: Install Windows only (advanced)":
<p align="center">
<br/>
<img src="https://i.imgur.com/AxtXViW.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Select default location it should read something like "Drive 0 Unallocated Space" and click next:
<p align="center">
<br/>
<img src="https://i.imgur.com/OhiiiG8.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    The next part will take a while, the server is going to restart several times as well:
<p align="center">
<br/>
<img src="https://i.imgur.com/ZYxMDs8.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Once the Server 2019 installation commences you'll be met with the prompt for setting up the default admin account:
    - We will just use Password1 as the password. It will be the password for everything in this lab:
<p align="center">
<br/>
<img src="https://i.imgur.com/UPXUNmN.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    We now have our Server 2019 VM fully installed!:
<p align="center">
<br/>
<img src="https://i.imgur.com/21KMc5w.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Let's log in and install VM guest editions:
    - VM guest additions kind of just gives a better experience when using the VM:
<p align="center">
<br/>
<img src="https://i.imgur.com/WYK8bYp.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    It will appear in File Explorer as a drive, we're just going to click into it and run the amd64 version:
    - Select 'Next', 'Next', 'Install', and then let it run until it's finished:
<p align="center">
<br/>
<img src="https://i.imgur.com/TmHSV8d.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>


    Once that's finished installing, don't select 'Reboot now' go ahead and select 'I want to manually reboot later':
<p align="center">
<br/>
<img src="https://i.imgur.com/joeqQeT.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    We're just going to shut down the VM ourselves:
<p align="center">
<br/>
<img src="https://i.imgur.com/sdztpQF.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Once it's been shut down, open it back up and log back in:
    - You'll notice that you can expand the entire VM window now and the mouse should move much smoother:
<p align="center">
<br/>
<img src="https://i.imgur.com/gUH1RW1.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Now that we have the guest additions installed we can set up our IP addressing:
    - Settings 
    - Network & Internet 
    - Ethernet 
    - Change adapter options:
<p align="center">
<br/>
<img src="https://i.imgur.com/UAGeDP9.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    We've established that there are going to need to be two networks, one is like your home internet that you use and the other is the internal one so we have to establish which one is which:
<p align="center">
<br/>
<img src="https://i.imgur.com/pUL77WC.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Right-click the first one and click status, this is like the typical IPv4 address so looks like it's my home IP:
<p align="center">
<br/>
<img src="https://i.imgur.com/dAllflq.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    We're gonna rename it _INTERNET_ and when we do status > details for the second Ethernet we see that it has an APIPA address. We will also rename it X_Internal_X:
<p align="center">
<br/>
<img src="https://i.imgur.com/9eeP1kP.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    We need to give our internal NIC an IP address. So that would be:
    - Settings 
    - Network & Internet

 
    - Ethernet 
    - Change adapter options 
    - Right-click X_Internal_X
    - Properties 
    - Select Internet Protocol Version 4
    - Properties 
    - Use the following:
        - IP address: 172.16.0.1
        - Subnet mask: 255.255.255.0	
        - Preferred DNS Server: 127.0.0.1:
<p align="center">
<br/>
<img src="https://i.imgur.com/3MDaQSN.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Next, set up some active directory domain services and create a domain:
    - In AD click Add roles and features, 'Next', 'Next', and you'll be met with the Server Selection pane where you can pick the server where you want to install ADDS. Select the only server there is, and click 'Next':
<p align="center">
<br/>
<img src="https://i.imgur.com/vkCkaVl.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/b6bTry6.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Select 'Active Directory Domain Services' and then 'Add Features', it will then be checked and then we'll click 'Next':
<p align="center">
<br/>
<img src="https://i.imgur.com/kPELUGg.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Click 'Next' all the way until the Confirmation pane and then you'll need to click 'Install'. It will take a while:
<p align="center">
<br/>
<img src="https://i.imgur.com/mhUrnTt.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Now that role has been installed we can close the Add Roles And Features Wizard window. You can see up in the top right corner there is now a little flag with a yellow triangle and an exclamation point inside it:
    - This is letting us know that we need to do our post-deployment configuration.
    - Hover your mouse over that icon and click Promote this server to a domain controller:
<p align="center">
<br/>
<img src="https://i.imgur.com/xATrXy8.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    This will open up a Deployment Configuration window, and we'll need to select 'Add a new forest' and create a Root domain name: mydomain.com:
<p align="center">
<br/>
<img src="https://i.imgur.com/IOfS2Xr.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    You'll also have to put in a password for the Domain Controller Options pane but we don't use it all. Just use Password1. After that, hit 'Next' all the way until the 'Install' button lights up. Then click that:
<p align="center">
<br/>
<img src="https://i.imgur.com/RQGQ2fu.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    When it's finished it will automatically reboot the VM:
<p align="center">
<br/>
<img src="https://i.imgur.com/3oKgXuk.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>


    Upon logging in you'll notice we have a MYDOMAIN\Administrator profile name now, so that's cool:
<p align="center">
<br/>
<img src="https://i.imgur.com/09vDkc5.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    We're going to create our own dedicated admin account instead of using the built-in administrator account. So we do that by clicking:
    - Start
    - Windows Administrative Tools
    - Active Directory Users and Computers:
<p align="center">
<br/>
<img src="https://i.imgur.com/N0bwmfd.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    You can see we have our newly created domain mydomain.com. We can now create an Organizational Unit:
<p align="center">
<br/>
<img src="https://i.imgur.com/SMxC3sh.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    We will name it _ADMINS:
<p align="center">    
<br/>
<img src="https://i.imgur.com/pKRPb0L.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>


    Now we can create a new user:
<p align="center">
<br/>
<img src="https://i.imgur.com/w0xQ9LA.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Obviously use your own name but I tried to model it after what I believe a typical company would structure their employees' usernames. Also, we're using Password1 for password if you haven't caught on by now:
<p align="center">
<br/>
<img src="https://i.imgur.com/6XsOp6b.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/KU9MWdK.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/bqeBCiU.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Now we need to make the user an admin account so we'll right-click the username and click:
    - Properties
    - Member Of
    - Add
    - Under Enter the object names to select type:
        - Domain admins
    - Click Check Names and you should get Domain Admins. We see it resolves so click OK:
<p align="center">
<br/>
<img src="https://i.imgur.com/1S0Ak5a.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/1UqyqkN.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    We now have our own domain account and we're going to log out so we can use it:
<p align="center">
<br/>
<img src="https://i.imgur.com/Tl3SGhn.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Instead of logging into the Administrator account, click Other user and use the domain admin account you created:
<p align="center">
<br/>
<img src="https://i.imgur.com/kQ1PeZP.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Now that we've logged in the next thing we're going to do is install RAS/NAT:
    - The purpose of this is to allow the Windows 10 client to be on the private virtual network but still be able to access the internet through the domain controller. So now we'll go to:
        - Add roles and features 
        - Next
        - Next 
        - In the Server Roles pane select the Remote Access box and click Next
        - Next
        - For the Role Services pane we click the Routing box (DirectAccess and VPN (RAS) will be selected automatically) and then we'll click next 
        - Next
        - Next
        - Install, and this will...you guessed it take a while:
<p align="center">
<br/>
<img src="https://i.imgur.com/5WwrwjR.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/zIisU1i.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/jR2Afd6.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/p8ekkhp.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/Hiraqjp.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/XuCt6ab.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/Sh89q2Y.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/hYqivzw.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Once it's done installing click close and navigate to:
    - Tools 
    - Routing and Remote Access:
<p align="center">
<br/>
<img src="https://i.imgur.com/OXpb6PP.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Right-click on the Domain Controller and select Configure and Enable Routing and Remote Access:
<p align="center">
<br/>
<img src="https://i.imgur.com/JlEpFFD.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Hit Next, then select NAT, and then Next:
<p align="center">
<br/>
<img src="https://i.imgur.com/iP2P8lR.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Select the _INTERNET_ interface, then Next, and then Finish. It may take a second:
<p align="center">
<br/>
<img src="https://i.imgur.com/XDgkgEp.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Now we see green, and that everything is totally configured!
    - Now our Windows 10 clients will be able to get to the internet, assuming we set up DHCP for them which is the next step:
<p align="center">
<br/>
<img src="https://i.imgur.com/UZ9DFHH.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>



    Go back to Add roles and features 
    - Next
    - Next 
    - Select DHCP Server 
    - Next
    - Next
    - Next 
    - Install & Close:
<p align="center">
<br/>
<img src="https://i.imgur.com/ZYeFRNI.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>
<img src="https://i.imgur.com/2jeXDNS.jpeg" height="80%" width="80%" alt="AD"/>
<br/>
<br/>

## Troubleshooting

- **Common issues:**
  - Ensure your server has a static IP address.
  - Verify DNS settings and ensure the DNS server is reachable.
  - Check event logs for errors related to AD DS.

- **Helpful commands:**
  - `dcdiag`: Diagnose domain controller issues.
  - `nslookup`: Verify DNS configuration.

## Resources

- [Microsoft Learn: Active Directory](https://docs.microsoft.com/en-us/learn/browse/?products=windows-server)
- [Official Windows Server Documentation](https://docs.microsoft.com/en-us/windows-server/)

## Contributing

Contributions are welcome! Please submit a pull request or open an issue to discuss any changes or suggestions.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
