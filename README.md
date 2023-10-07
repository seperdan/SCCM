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
<br>Microsoft Server 2019: https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019</br>
<br>Microsoft Windows 10 ISO: https://www.microsoft.com/en-us/software-download/windows10</br>
<br>Microsoft Endpoint Configuration Manager: https://archive.org/details/mu_microsoft_endpoint_configuration_manager_current_branch_version_2103_x86_x64_dvd_77e1425b</br>
<br>SQL Server 2016: https://www.microsoft.com/en-us/evalcenter/download-sql-server-2016</b>

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

<b>When prompted with the installation configurations, ensure to select the Server 2019 ISO AND select "Windows Server Standard Core" under version to install like so:</b>

![5](https://github.com/seperdan/SCCM/assets/54723844/6a0d0c52-bbff-40d7-9590-f6c5b6a59e80)
![6](https://github.com/seperdan/SCCM/assets/54723844/c028abee-bee6-4731-8636-54136263303d)

<b>Once you get to this prompt, click on "Customize Hardware"<b>
![7](https://github.com/seperdan/SCCM/assets/54723844/54008368-e8f3-4020-8026-edf9cabfeeab)

<b>and ensure that the Network Adaptor is correctly configured to bridged VMnet19 that we just setup! </b>




