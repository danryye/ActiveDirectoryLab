<h1>Active Directory Home Lab</h1>

<h2>Description</h2>
Project consists of setting up an Active Directory Home Lab environment with Oracle Virtual Box. This labs involves simulating a real world company's network map by setting up a central domain controller machine that directs internal devices to the outside internet through a single IP. New machines inside the internal network joining the domain are automatically assigned an IP within a set range and are able to sign in through an existing account within AD.

<h2>Network Map</h2>

I will be following the network map provided by [Josh Madakor](https://www.youtube.com/@JoshMadakor)

![Network Map](images/josh-madakor-network-map.png)

<h2>Files and Programs Used</h2>

<h3>Oracle Virtual Box - Virtual Machine Software</h3>

Download and install Oracle Virtual Box from the  [official website.](https://www.virtualbox.org/)

<h3>Windows Server 2019 ISO File</h3>

Downloaded from Microsoft's [official website.](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019)

<h3>Windows 10 ISO File</h3>

Downloaded from Microsoft's [official website.](https://www.microsoft.com/en-us/software-download/windows10)

<h2>Insructions</h2>

Once Oracle Virtual Box is installed, we begin by creating a new virtual machine.

Click on the New button at the top.

![Creating a new VM](images/new-vm.png)

We will name this VM as "Domain Controller" which will be acting as our server machine.

Note: We will not be mounting the ISO file at this step, as it can lead to issues during the initial boot

![Naming Comain Controller](images/naming-domain-controller.png)

When assigning the resources for the VM, it is recommended to provide at least 2 GBs of RAM and 1 core for the processor. If you have the available resources, more can added to speed up later steps.

I will be using the default values provided by Virtual Box.

![VM RAM and Processing Cores](images/resource-allocation.png)

For the Virtual Hard Disk, we recommend providing at least 20 GBs. The Windows Server OS can tack up a large amount of storage space, we want to ensure that there is sufficient space to prevent errors.

I will be using the default values provided by Virtual Box.

![Virtual Hard Disk](images/virtual-hard-disk.png)

Review the settings on summary page and click "Finish"

Before continuing, we need to enable a second Newtork Adapter for our machine to emulate an internal network.

Make sure our Domain Controller is highlighted on the left and open the Settings.

![Domain Controller Settings](images/dc-settings.png)

Navigate to the Network Tab, the first newtork adapter is enabled by default. We will be using this one to access the internet. We will want to enable the second network adapter for use as our internal network.

Set the "Attached to" setting to "Internal Network"

![Enable second network adapter for internal network use](images/enable-network-adapter-2.png)

** Optional: Enabling Shared Clipboard and Drag'n'Drop can provide ease of access between the host machine and the VM **

![Shared Clipboard and Drag'n'Drop](images/shared-clipboard.png)

Double click on the Domain Controller Box or hit Start at the top to start up the VM.

When prompted for a boot medium, locate the Windows 2019 Server ISO file downloaded previously and click "Mount and Retry Boot"

![Mount Windows Server 2019 ISO](images/mount-windows-server-2019-iso.png)

Follow the on screen steps to complete the installation for Windows Server 2019

When prompted, select the "Windows Server 2019 Standard Evaluation (Desktop Experience)" for ease of access Windows GUI.

![Install Windows Server 2019](images/install-win-server-2019.png)

