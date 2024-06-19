Creating a full-blown Active Directory lab on your personal computer using VirtualBox is a valuable exercise that can greatly enhance your understanding of Windows networking. To start, you'll need to download and install Oracle VirtualBox. Next, download both a Windows 10 ISO and a Server 2019 ISO. Our first task will be to create a virtual machine (VM) that will serve as our domain controller. This VM will house Active Directory, and it should be equipped with two network adapters: one for connecting to the outside internet and the other for connecting to the VirtualBox private network, which our clients will use.

After creating the VM, install Server 2019 on it. We'll need to assign IP addressing for the internal network while the external network will automatically receive IP addressing from your home network or router, eliminating the need for manual configuration. Once the IP addressing is set up, we’ll proceed to name the server, install Active Directory, and create our domain. Additionally, we'll configure routing to ensure that clients on the private network can access the internet through the domain controller. Setting up DHCP on the domain controller is also crucial as it will enable the Windows 10 machine to automatically obtain an IP address.

Before creating our client virtual machine, we'll run a PowerShell script on the domain controller to automatically create a thousand users in Active Directory. This step will showcase the utility of PowerShell and demonstrate the various tasks it can perform. Following this, we'll create another VM and install Windows 10 on it. This VM, named "Client1," will be connected to the private VirtualBox network. We'll then join it to the domain and log in using one of our domain accounts.

By the end of this tutorial, you will have built a basic Windows networking environment with Active Directory and a few networking services. This setup not only provides a practical learning experience but also lays a solid foundation for more advanced networking concepts.


<p align="center">
1. review diagram: 
<br/>
<img src="https://i.imgur.com/N8XxLVQ.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
2. download and install oracle virtual box: 
<br/>
<img src="https://i.imgur.com/FDaJu29.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
3. also download and isntall the virtual box extension pack as well: 
<br/>
<img src="https://i.imgur.com/mmBiAx9.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
4. download windows 10 iso, it will download as MediaCreationTool_22H2: 
<br/>
<img src="https://i.imgur.com/7ieZF9O.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
5. download windows server 2019: 
<br/>
<img src="https://i.imgur.com/ozCzxqI.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
6. configure Server 2019 virtual machine, we'll call it "DC" for Domain Controller -- nice and easy to remember: 
<br/>
<img src="https://i.imgur.com/gkwfHMJ.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/29eLAkK.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/CuLt7p9.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/P9mTqTa.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
7. configure additional settings 
	-remember when configuring network settings, we're creating our domain controller so we want two nics. 1 that's dedicated to the internet that runs NAT, and the second is dedicated for the internal vmware network: 
<br/>
<img src="https://i.imgur.com/HVov5hn.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/WdxxwMW.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/uOVzfv4.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
8. start the "DC" vm, and this is where we'll select the Server 2019 iso. for me I had to select it and reboot: 
<br/>
<img src="https://i.imgur.com/8yQzQnJ.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
9. it may take a while to reach this screen due to loading time, but once you've reached it we'll just install by clicking next, and then Install Now:
<br/>
<img src="https://i.imgur.com/Mx6Fkio.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/w6PxqyZ.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
10. select 'Windows Server 2019 Standard Evaluation (Desktop Experience):
<br/>
<img src="https://i.imgur.com/5wVf2s4.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
11. after accepting license terms, select which type of installation you want
	-we're going to format the hard drive and install it from scratch so select "Custom: Install Windows only (advanced)":
<br/>
<img src="https://i.imgur.com/AxtXViW.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
12. select default location it should read something like "Drive 0 Unallocated Space" and click next:
<br/>
<img src="https://i.imgur.com/OhiiiG8.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
13. the next part will take a while, the server is going to restart several times as well:
<br/>
<img src="https://i.imgur.com/ZYxMDs8.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
14. once the Server 2019 installation commences you'll be met with the prompt for setting up the default admin account 
	-we will just use Password1 as the pw. it will be the pw for everything in this lab:
<br/>
<img src="https://i.imgur.com/UPXUNmN.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
15. we now have our Server 2019 vm fully installed!:
<br/>
<img src="https://i.imgur.com/21KMc5w.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
16. let's log in and install vm guest editions.
	-vm guest additions kind of just gives a better experience when using the vm:
<br/>
<img src="https://i.imgur.com/WYK8bYp.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
17. it will appear in File Explorer as a drive, we're just going to click into it and run the amd64 version
	-select 'Next', 'Next', 'Install', and then let it run until it's finished:
<br/>
<img src="https://i.imgur.com/TmHSV8d.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
18. Once that's finished installing, don't select 'Reboot now' go ahead and select 'I want to manually reboot later':
<br/>
<img src="https://i.imgur.com/joeqQeT.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
19. We're just going to shut down the vm ourselves:
<br/>
<img src="https://i.imgur.com/sdztpQF.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
20. once it's been shut down, open it back up and log back in 
	-you'll notice that you can expand the entire vm window now and the mouse should move much smoother:
<br/>
<img src="https://i.imgur.com/gUH1RW1.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
21. now that we have the guest additions installed we can set up our ip addressing 
	-settings 
	-Network & Internet 
	-Ethernet 
	-Change adapter options:
<br/>
<img src="https://i.imgur.com/UAGeDP9.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
22. we've established that there are going to need to be two networks, one is like your home internet that you use and the other is the internal one so we have to establish which one is which:
<br/>
<img src="https://i.imgur.com/pUL77WC.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
23. right click the first one and click status, this is like the typical IPv4 address so looks like it's my home IP:
<br/>
<img src="https://i.imgur.com/dAllflq.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
24. we're gonna rename it _INTERNET_ and when we do status > details for the second Ethernet we see that it has an APIPA address. we will also rename it X_Internal_X:
<br/>
<img src="https://i.imgur.com/9eeP1kP.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
25. we need to give our internal nic an IP address. so that would be 
	-settings 
	-Network & Internet 
	-Ethernet 
	-Change adapter options 
	-right click X_Internal_X
	-properties 
	-select Internet Protocol Version 4
	-properties 
	-use the following 
		IP address: 172.16.0.1
		Subnet mask: 255.255.255.0	
		Preferred DNS Server: 127.0.0.1:
<br/>
<img src="https://i.imgur.com/3MDaQSN.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
26. next set up some active directory domain services and create a domain 
	-in AD click Add roles and features, 'Next', 'Next', and you'll be met with the Server Selection pane where you can pick the server where you want to install ADDS. select the only server there is, and click 'Next':
<br/>
<img src="https://i.imgur.com/vkCkaVl.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/b6bTry6.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
27. select 'Active Directory Domain Services' and then 'Add Features', it will then be checked and then we'll click 'Next':
<br/>
<img src="https://i.imgur.com/kPELUGg.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
28. click 'Next' all the way until the Confirmation pane and then you'll need to click 'Install'. It will take a while:
<br/>
<img src="https://i.imgur.com/mhUrnTt.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
29. now that role has been installed we can close the Add Roles And Features Wizard window. You can see up in the top right corner there is now a little flag with a yellow triangle and an exclamation point inside it.  
	-this is letting us know that we need to do our post deployment configuration. 
hover your mouse over that icon and click Promote this server to a domain controller:
<br/>
<img src="https://i.imgur.com/xATrXy8.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
30. This will open up a Deployment Configuration window, and we'll need to select 'Add a new forest' and create a Root domain name: mydomain.com:
<br/>
<img src="https://i.imgur.com/IOfS2Xr.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
31. you'll also have to put in a password for the Domain Controller Options pane but we don't use it all. just use Password1. After that, hit 'Next' all the way until the 'Install' button lights up. then click that. :
<br/>
<img src="https://i.imgur.com/RQGQ2fu.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
32. when it's finished it will automatically reboot the vm:
<br/>
<img src="https://i.imgur.com/3oKgXuk.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
33. upon logging in you'll notice we have a MYDOOMAIN\Administrator profile name now, so that's cool:
<br/>
<img src="https://i.imgur.com/09vDkc5.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
34. we're going to create our own dedicated admin account instead of using the built in administrator account. so we do that by clicking:
	-Start
	-Windows Administrative Tools
	-Active Directory Users and Computers:
<br/>
<img src="https://i.imgur.com/N0bwmfd.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
35. you can see we have our newly created domain mydomain.com. we can now create an Organizational Unit:
<br/>
<img src="https://i.imgur.com/SMxC3sh.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
36. we will name it _ADMINS:
<br/>
<img src="https://i.imgur.com/pKRPb0L.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
37. now we can create a new user:
<br/>
<img src="https://i.imgur.com/w0xQ9LA.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
38. Obviously use your own name but I tried to model it after what I believe a typical company would structure their employees usernames. Also, we're using Password1 for password if you haven't caught on by now:
<br/>
<img src="https://i.imgur.com/6XsOp6b.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/KU9MWdK.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/bqeBCiU.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
39. now we need to make the user an admin account so we'll right click the username and click: 
	-Properties
	-Member Of
	-Add
	-Under Enter the object names to select type:
		-domain admins
	-Click Check Names and you should get Domain Admins. we see it resolves so click OK:
<br/>
<img src="https://i.imgur.com/1S0Ak5a.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/1UqyqkN.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
40. we now have our own domain account and we're going to log out so we can use it:
<br/>
<img src="https://i.imgur.com/Tl3SGhn.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
41. instad of logging into the Administrator account, click Other user and use the domain admin account you created:
<br/>
<img src="https://i.imgur.com/kQ1PeZP.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
42. now that we've logged in the next thing we're going to do is install RAS/NAT
	-the purpose of this is to allow the windows 10 client to be on the private virtual network but still be able to access the internet through the domain controller. so now we'll go to:
		-Add roles and features 
		-Next
		-Next 
		-In the Server Roles pane select the Remote Access box and click Next
		-Next
		-For the Role Services pane we click the Routing box (DirectAccess and VPN (RAS) will be selected automatically) and then we'll click next 
		-Next
		-Next
		-Install, and this will...you guessed it take a while:
<br/>
<img src="https://i.imgur.com/5WwrwjR.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/zIisU1i.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/jR2Afd6.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/p8ekkhp.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/Hiraqjp.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/XuCt6ab.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/Sh89q2Y.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/hYqivzw.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
43. Once it's done installing click close and navigate to:
	-Tools 
	-Routing and Remote Access: 
<br/>
<img src="https://i.imgur.com/OXpb6PP.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
44. Right click on the Domain Controller and select Configure and Enable Routing and Remote Access:
<br/>
<img src="https://i.imgur.com/JlEpFFD.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
45. Hit Next, then select NAT, and then Next:
<br/>
<img src="https://i.imgur.com/iP2P8lR.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
46. Select the _INTERNET_ interface, then Next, and then Finish. It may take a second:
<br/>
<img src="https://i.imgur.com/XDgkgEp.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
47. Now we see green, and that everything is totally configured! 
	-now our windows 10 clients will be able to get to the internet, assuming we set up dhcp for them which is the next step:
<br/>
<img src="https://i.imgur.com/UZ9DFHH.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
48. go back to:
	-Add roles and features 
	-Next
	-Next 
	-Select DHCP Server 
	-Next
	-Next
	-Next 
	-Install & Close :
<br/>
<img src="https://i.imgur.com/ZYeFRNI.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
<img src="https://i.imgur.com/2jeXDNS.jpeg" height="80%" width="80%" alt="AD"/>
<br />
<br />
