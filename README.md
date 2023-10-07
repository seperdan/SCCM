<h1>Basic Virtual Machine (VM), Active Directory (AD) and Microsoft Systen Center Configuration Manager (SCCM) Home Lab Set up</h1>

<h1>Description:</h1>
<b>Hi! In this Github, I will demonstrate how to set up your own virtual environment with Active Directory and Microsoft System Center Configuration Manager in a simulated Enterprise setting!</b>

<h2>Utilities & Services Used and Created</h2>
<br>- Powershell</br> 
<br>- Server Manager</br>
<br>- Active Directory</br>
<br>- DNS</br>
<br>- DHCP</br>
<br>- NAT</br>
<br>- RAS</br>
<br>- SQL Server 2016</br>
<br>- SCCM</br>

<h2>Environments and Tools Used</h2>
<br>VMware Work Station Pro (Free Trial Version): https://www.vmware.com/ca/products/workstation-pro.html</br>
<br>Microsoft Server 2016: https://www.microsoft.com/en-us/evalcenter/download-windows-server-2016</br>
<br>Microsoft Windows 10 ISO: https://www.microsoft.com/en-us/software-download/windows10</br>
<br>Microsoft Endpoint Configuration Manager: https://archive.org/details/mu_microsoft_endpoint_configuration_manager_current_branch_version_2103_x86_x64_dvd_77e1425b</br>
<br>SQL Server 2016: https://www.microsoft.com/en-us/evalcenter/download-sql-server-2016</br>
<br>Windows ADK: https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install</br>

<h2 align="center">Virtual Machine Setup Walkthrough</h2>

<b>For this setup, we will be using this network diagram:</b>

![1](https://github.com/seperdan/SCCM/assets/54723844/82bf8701-5219-42e3-833d-1d37ad0b3311)

<b>Our first virtual machine will the the Server. It will house two network adapters, VMnet 18 and VMnet19, one for connecting to the outside interent and the other for conenction to the VM's private network that the clients will connect to. Note that the external network will gain automatic addressing from our home network but we will need to assign an IP address to the internal network which we'll take care of shortly. Once having done so, we will install services such as Active Directory, DNS, DHCP, etc. and then configure the Windows 10 client image to connect to the server image! Finally, we will create an SCCM-based virtual environment to connect it all!</b>

<b>To start, download the VMware Work Station Pro, Microsoft Server 2019 ISO, and Microsoft Windows 10 ISO from the links under "Environments and Tools Used</b>.
<b>Open VMware and click on "Edit" and then "Virtual Network Editor"</b>

![2](https://github.com/seperdan/SCCM/assets/54723844/4ffe55d6-95ff-4de4-8177-46cbbb0bd700)

<b>Now you will be presented with network settings. Configure them as such in accordance to your network diagram, ensuring to create VMnet19 and setting it to be "Bridged" to your local ethernet adaptor which in my case is "Realtek Gaming GbE Family Controller."</b>

![3](https://github.com/seperdan/SCCM/assets/54723844/d7cda194-b42d-4c83-a2aa-172b8dd0a4e9)

<b>Now click on "create a new virtual machine"</b>

![4](https://github.com/seperdan/SCCM/assets/54723844/ad0603a1-6b41-4dbb-bfb0-fdcecd198c28)

<b>When prompted with the installation configurations, ensure to select the Server 2016 ISO AND select "Windows Server 2016 Standard" under version to install like so:</b>

![5](https://github.com/seperdan/SCCM/assets/54723844/6a0d0c52-bbff-40d7-9590-f6c5b6a59e80)
![6](https://github.com/seperdan/SCCM/assets/54723844/41ddc7ef-dd56-4f67-a4d0-cb29709656c8)

<b>Once you get to this prompt, click on "Customize Hardware"<b>
![7](https://github.com/seperdan/SCCM/assets/54723844/a91f3983-a1ab-4d03-81e3-f720d951315e)

<b>and ensure that the Network Adaptor is correctly configured to bridged VMnet19 that we just setup!</b>

![8](https://github.com/seperdan/SCCM/assets/54723844/90a7a484-70f0-4891-b569-292e134e6df7)

<b>Once that's done, OK everything and your first virtual environment should boot up after a brief installation!</b>
![9](https://github.com/seperdan/SCCM/assets/54723844/27299c31-2cc8-4ff4-a554-6ef77a49d87c)

<b>Now, we will need to set a static IP for out network. Nagivate to network connections:</b>
![10](https://github.com/seperdan/SCCM/assets/54723844/f8e542cc-0831-422d-8e34-edaf24f38c1e)

<b>Right click on your network, go on properties, and select the "Inter Protocol Version 4 (TCP/IPv4)" option</b>
![11](https://github.com/seperdan/SCCM/assets/54723844/abe8d61d-3f08-4e24-84fc-2c00a077c779)

<b>Set the following IP address and DNS Mask address per our network diagram above and select OK:</b>
![12](https://github.com/seperdan/SCCM/assets/54723844/1be3929b-da02-4ac4-b142-b55966b3f16f)

<h1>Active Directory and General Server Configuration Walkthrough:</h1>
<b>Now, go on "Add roles and features" on Server Manager (SM) to configure our services:</b>

![13](https://github.com/seperdan/SCCM/assets/54723844/753909ac-809d-47e9-a436-686c83f6ab36)

<b>and by following this short clip, install the "Active Directory Domain Services", "DHCP", "DNS", and "Remote Access" services!</b>

https://github.com/seperdan/SCCM/assets/54723844/d5795b5f-b069-4a94-b3a5-c90e545eb0a4

![16](https://github.com/seperdan/SCCM/assets/54723844/8f41f22d-f70f-4650-b8cf-78ff77d53749)

<b>These installations will create their respective services in our server environment! Once this is done for the Active Directory Domain Services, click on "Promote this server to a domain controller" to promote your AD to be under a forest which you can create for ourself. A forest can be comprised of multiple AD's.

![14](https://github.com/seperdan/SCCM/assets/54723844/dade62d4-93d0-4b43-9f34-7ef2d22332d7)

![15](https://github.com/seperdan/SCCM/assets/54723844/928a58e1-8ecc-4536-81c0-90014f682654)

<b>After restarting, you might notice that there's now another user on our Server VM setup.

![17](https://github.com/seperdan/SCCM/assets/54723844/4a9d3daf-dae1-4863-8191-800955acf584)

<b>You can sign into the newly created user like with credentials "Administrator" and the password you just set like so:

![18](https://github.com/seperdan/SCCM/assets/54723844/57cd5bf5-e225-4cdf-a342-671255cfb488)

<b>Next, navigate to Active Directory Users and Computers through using the Windows search. Then right click on your domain, create a new Organizationl Unit (OU), name it something like "_ADMIN", right click on the new OU to create a new user with your credentials. This user will serve as the domain administrator!</b>

https://github.com/seperdan/SCCM/assets/54723844/4706a504-d6c7-40ad-b7c5-7b95e3ab5c1e

<b>Note: You can similarly find Active Directory Users and Computers by clicking on "Tools" and then selecting "Active Directory Users and Computers" on the Server Manager tool like so:</b>

![18](https://github.com/seperdan/SCCM/assets/54723844/e9476d14-9ffd-43c6-90bf-1525a242a52a)

<b>Even though we've created our user as the domain administrator, we haven't given it admin premissions yet. We can do this by following these steps:</b>

https://github.com/seperdan/SCCM/assets/54723844/63a56875-ccf4-45bb-9def-1b466df112a6

![19](https://github.com/seperdan/SCCM/assets/54723844/5e561356-5bf1-4a62-8fc8-ef6a5c9ba8b9)

<b>*Make sure to also add "Schema Admins" as seen in the image above</b>
<br>After having done so, we can sign out of the built-in admin account and sign into the newly-created domain admin account:</br>

https://github.com/seperdan/SCCM/assets/54723844/b94608d8-48e1-49f5-9736-2ae82a574ac5

<b>Now we are logged in as the IT administrator!</b>

<b>Now in your newly-created local admin account, we use "Tools->Routing and Remote Access" to configure the RAS and NAT to be able to connect to the internet.</b>

https://github.com/seperdan/SCCM/assets/54723844/aaff4d02-88f9-4ce8-993a-95fb8129a346

<b>*Note: Sometimes the network interface that you need, in this case "Main_Internet" will be greyed out when setting up the Rotuing and Remote Access server. The solution for this is to exit the installation wizard and try again. Make sure to select the main internet network interace instead of the internal internet!</b>

![19](https://github.com/seperdan/SCCM/assets/54723844/e8a787c0-5536-43ce-a9e8-e6098092efb2)
![20](https://github.com/seperdan/SCCM/assets/54723844/3f982e76-6956-4320-832c-b00fff48255c)

<b>Now you will have this when you navigate to Routing and Remote Access!</b>

![21](https://github.com/seperdan/SCCM/assets/54723844/bf529fb1-a47f-4ee9-ba7f-08243764d31b)

<b>Now it's time to setup the DHCP and its scope!</b>

<b>The scope I will be creating will give assign IP addresses in the range of 192.168.1.1 to 192.168.1.100. This means that the DHCP will assign 100 different IP addresses. I also set the amount of time the IP addresses can be leased out to 100 days just for convenience as this is only a VM and the lease duration doesn't matter.

https://github.com/seperdan/SCCM/assets/54723844/ee23f4cf-e987-4571-ab0a-e5c9d51481c2

<b>And here is our scope now:</b>

![20](https://github.com/seperdan/SCCM/assets/54723844/3db6e9d6-4915-4b7d-a17e-e42ccfb132d7)

<b>Now when we right click on the domain and select "Add/remove bind Bindings" we can see that bounded by default to our ethernet adapter.</b>

![21](https://github.com/seperdan/SCCM/assets/54723844/b87f0bfa-4550-484d-b34b-17f1437cb4fd)

<b>So at this point DHCP is functional and will hand out IP addresses on VMnet 19!</b>

<b>Now we need to set up the external wireless connections, VMnet18 which will be bridged to wireless!</b>

<b>Follow these steps to add our new connection, VMnet18, to our workstation:</b>

https://github.com/seperdan/SCCM/assets/54723844/8f2378da-9970-4511-98ee-f26fa7732cd5

<b>Next, add that we've bridged it to our wireless adaptor, we can set VMnet18 up as the network adaptor by following these steps:</b>

https://github.com/seperdan/SCCM/assets/54723844/2553066a-42cc-4225-b2bc-2572dc8f4536

<b>Now, if we run ipconfig in cmd, we can see that we have an IP address we can use to connect to the internet with DHCP!</b>

![22](https://github.com/seperdan/SCCM/assets/54723844/f41f80b2-43aa-45af-a394-fbf79b9516ad)

<b>But before we can go to the internet, we should configure the DNS as well.</b>

https://github.com/seperdan/SCCM/assets/54723844/8ff72f7c-891e-46b5-9258-bec3f63c0702

<b>*Note: I added 8.8.8.8 and 8.8.4.4 as forwarders because they're Google's forwarders. So now when our machine is asking for external DNS, these DNS forwarders will handle that!</b>

<b>Next, we must create a server with routing enabled by configuring "scope options." The scope options allow your machines to be able to learn some information about your infrastructure when they recieve an IP address from the DHCP server.</b>

![23](https://github.com/seperdan/SCCM/assets/54723844/59802354-4c97-4da4-b46e-43a8b7654739)

<b>To do this, follow the following video:</b>

https://github.com/seperdan/SCCM/assets/54723844/4d1aaa44-7b5d-4730-a241-1ad6cfd8a41d

<b>Now we've successfully set up our Server with AD services up! Now we must configure the SCCM server on another virtual machine!</b>

<h1>SCCM Machine Setup Walkthrough:</h1>

<b>
