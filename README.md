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

<h3>Step 1: Setup the Domain Controller Machine</h3>

Once Oracle Virtual Box is installed, we begin by creating a new virtual machine.

Click on the New button at the top.

![Creating a new VM](images/new-vm.png)

We will name this VM as "Domain Controller" which will be acting as our server machine.

Note: We will not be mounting the ISO file at this step, as it can lead to issues during the initial boot

![Naming Comain Controller Machine](images/naming-domain-controller.png)

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

Once the setup is complete and you are signed into Windows, we will first want to install the VM Windows Guest Additions features for more ease of access.

At the top of the VM application, go to "Devices", and "Mount Guest Additions CD Image"

![Mount Guest Additions CD Image](images/win-guest-additions.png)

Open the Windows Explorer, navigate to "This PC". Under the Devices and drives, double click on "VirtualBox Guest Additions".

![VirtualBox Guest Additions](images/win-guest-additions-2.png)

Inside the disc, run the application called "VBoxWindowsAdditions-amd64"

![VirtualBox Windows Additions](images/win-guest-additions-3.png)

Follow the instructions on the setup wizard to complete the installation. You will be prompted to restart the machine after completing the setup. We recommend doing so to enable its new features.

![VirtualBox Windows Additions Setup](images/win-guest-additions-4.png)

<h3>Step 2: Configure the Network Interfaces Settings</h3>

Once logged back into Windows, we now want to configure the network interfaces on our domain controller machine. Following the diagram, We want one interface for internet connection, while the second interface is for the internal network. Navigate to your network connections through the network icon at the bottom right. In my case, "Ethernet" is my interface for internet connection, and "Ethernet 2" is my interface for the internal network.

![Network Connections](images/network-connections.png)

To avoid any confusion in later steps, we can rename these connections to distinctly represent what they do. I renamed "Ethernet" to "\_INTERNET_" and "Ethernet 2" to "\_INTERNAL|"

![Network Connections](images/network-connections-2.png)

We will want to give it a static IP address to the domain controller within the internal network. Right click on internal network connection, and open its properties. Open the properties of IPv4 protocol. Select the option to use a static IP address, rather than being assigned one automatically. Following the diagram, we will use the IP address of 172.16.0.1 with a subnet mask of 255.255.255.0. For the DNS server, we will it the loopback address of 127.0.0.1.

![NAssign Static IP to Domain Controller](images/network-connections-3.png)

Before continuing, let's rename our machine from its given name to something that clearly defines that it is the domain controller. Right click the windows icon at the bottom left and select "System". Click on "Rename this PC". I will use "DC" as its new name. Allow the machine to restart.

![Rename PC](images/rename-pc.png)

<h3>Step 3: Set up Active Directory</h3>

Open the Server Manager. In the Dashboard, go to "Manage" at the top right and select "Add Roles and Features".

![Add Roles and Features](images/add-roles-and-features.png)

In the wizard slect "Role-based or feature-based installation"

![Add Roles and Features](images/add-roles-and-features-2.png)

Select the current machine named "DC"

![Add Roles and Features](images/add-roles-and-features-3.png)

Select "Active Directory Domain Services" and wait for the installation to complete.

![Add Roles and Features](images/add-roles-and-features-4.png)

In the Server Manager, go to the flag icon at the top right. Select "Promote this server to a domain controller"

![Promote to domain controller](images/add-roles-and-features-5.png)

In the popup menu, select "Add a new forest", and we'll give the domain name a generic name of "mydomain.com". We will use the default setting for the other options. Continue to click "Next" until given the option to "Install". The machine will automatically restart after the installation is complete.

![Promote to domain controller](images/add-roles-and-features-6.png)

After the restart, you should notice our given domain name of "mydomain" coming before your the username on the sign in screen. Sign in normally with your previous password.

![Added to domain](images/added-to-domain.png)

<h3>Step 4: Set up Remote Access</h3>

Open up the "Add Roles and Features Wizard" as done in the previous step. This time, we will be installing the "Remote Access" role.

![Install Remote Access](images/remote-access.png)

We will include the "Routing" feature to allow clients on our private network to be able to access the internet. Continue to click "Next" until given the option to "Install". After it is complete, close the window.

![Install Remote Access](images/remote-access-2.png)

In the Server Manager, under "Tools" at the top right, select "Routing and Remote Access"

![Install Remote Access](images/remote-access-3.png)

Right click on the server "DC and select "Configure and Enable Routing and Remote Access"

![Install Remote Access](images/remote-access-4.png)

Select the NAT option to allow our internal network to connect to the internet through a single IP.

![Install Remote Access](images/remote-access-5.png)

In the next step, select the "\_INTERNET_" interface for the public interface to connect to the internet. Continue with the wizard until you reach "Finish".

![Install Remote Access](images/remote-access-6.png)

With NAT nad RAS set up, our clients to the domain will be able to access the internet once DHCP is set up.

<h3>Step 5: Set up DHCP</h3>

Open up the "Add Roles and Features Wizard" as done in the previous step. This time, we will be installing the "DHCP" role. Continue to click "Next" until given the option to "Install". After it is complete, close the window.

![DHCP Setup](images/dhcp.png)

In the Server Manager, under "Tools" at the top right, select "DHCP"

![DHCP Setup](images/dhcp-2.png)

In the DHCP window, we want to set the scope of addresses as defined in our network diagram. Expand the drop down on the server we created (dc.mydomain.com). Right click on IPv4, and select "New Scope".

![DHCP Setup](images/dhcp-3.png)

In the "New Scope Wizard", I will give this scope the name of "172.16.0.100-200" to describe the range we are using.

![DHCP Setup](images/dhcp-4.png)

As defined in our network diagram, the starting IP address will be "172.16.0.100" and the ending IP address will be "172.16.0.200", with a subnet length of /24.

![DHCP Setup](images/dhcp-5.png)

For the Default Gateway, we will give it the IP address of our domain controller. Enter "172.16.0.1", and click "Add". Continue to click "Next" until given the "Finish" option.

![DHCP Setup](images/dhcp-6.png)

In the DHCP window, right click on the server, and select "Authorize". Refresh the server afterwards.

![DHCP Setup](images/dhcp-7.png)

<h3>Step 6: Creating a client user in Active Directory</h3>

Before creating our client machine to join the domain, we need to create a user account we can use in Active Directory.

Return to the Windows Administrative Tools and open the "Active Directory Users and Computers" once again.

I'm going to create a new Organizational Unit (OU) under our domain called "_Users".

![Add User to AD](images/add-user-to-ad.png)

![Add User to AD](images/add-user-to-ad-2.png)

Next, right click on our newly created "_Users" OU and add a new User to it.

![Add User to AD](images/add-user-to-ad-3.png)

I'm going to use the generic name of "John Doe" for the test user. I will follow the common convention of using the first intial of first name + last for the logon username.

![Add User to AD](images/add-user-to-ad-4.png)

<h3>Step 7: Create the Client VM Machine</h3>

We head back to the Oracle VirtualBox application to create a new virtual machine.

Just like the machine for our domain controller, we start by clicking "New" at the top.

![Add Client Machine](images/client-machine.png)

Name the machine "Client" and double click it to turn it on. When prompted for a bootable medium, navigate to and select the Windows 10 ISO file downloaded.

![Add Client Machine](images/client-machine-2.png)

Follow the on screen installation instructions to complete the installation. I selected "Windows 10 Pro" when asked for the OS version.

While Windows is installing, return to the Oracle VirtualBox app. Go into the Settings for the Client VM. In the Network tab, set Adapter 1 to be Attached to the "Internal Network". We want this machine to emulate a client machine inside our private network.

![Add Client Machine](images/client-machine-3.png)

Once signed into Windows on the client machine, open up the Command Prompt and type in "ipconfig" to ensure that all the network configurations are consistent with our settings.

Here we see that this device is connected to our internal network, the default gateway address is our domain controller, and IP address assigned to the machine is within the scope that we previously set.

![Add Client Machine](images/client-machine-4.png)

With everything working, we can rename this pc and join it to the domain. Right click on the Windows icon at the bottom left, select System.

Scroll to the bottom of the page and select "Rename this PC (advanced)"

In the System Properties, Select the option to change the domain.

![Rename PC advanced](images/client-machine-5.png)

I'm going to rename it to "Client-01"

Enter in the domain "mydomain.com" and click "OK".

![Rename PC advanced](images/client-machine-6.png)

When asked about an account needed to join the domain, use the account named "John Doe" with the username "jdoe" we previously created in AD. The machine will need to be restarted afterwards to apply the changes.

![Rename PC advanced](images/client-machine-7.png)

Once restarted and back to the sign in screen, instead of signing into the local account, we can now sign in using an account on the domain. Select the "Other users".

![Rename PC advanced](images/client-machine-8.png)

Use the username and password for the "John Doe" user we created previously.

![Rename PC advanced](images/client-machine-9.png)

Congratulations! You are now signed into an account on the domain from this machine.

![Rename PC advanced](images/client-machine-11.png)

