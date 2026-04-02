**Active Directory Setup – Drew Blythe – Home Lab**

Documented Process with steps, screenshots, and problem fixes

Host PC Specs

- Windows 11
- AMD Ryzen 5 3600 6-Core Processor
- 32GB of RAM
- 1 TB HDD

Virtualization Software

- VirtualBox 7.2.6 Windows package

Virtual Machines

- Active Directory - Windows Server 2022 (2 CPU, 4 GB RAM)
    - https://www.microsoft.com/en-us/windows-server
- Workstation 1 - Windows 11 (2 CPU, 4 GB RAM)
    - https://www.microsoft.com/en-us/software-download/windows11

Core Objective

The goal of this project is to build a virtualized home lab environment using VirtualBox with Windows Server 2022 and Windows 11 VMs. The lab covers configuring VirtualBox to allow for different VMs, configuring an isolated NAT network to allow for communication between VMs, setting up Windows Server 2022 to use Active Directory, creating and managing user accounts within that domain, configuring DNS settings across virtual machines, and joining client machines to the domain.

Virtual Box Installation

1.  First thing you are going to want to do is download VirtualBox off the website [virtualbox.org](https://www.virtualbox.org/)
2.  Click the “Download” button and then click “windows hosts” It is now going to download for you.

1.  Now click on the download and when the pop up appears on your screen click yes. Continue through the screens of the pop up for installation and when done click “Finish”, you have now downloaded VirtualBox.
2.  Now that you have downloaded VirtualBox you can proceed with the Windows Server 2022 installation.

Windows Server 2022 Installation

Click this link to be sent to the Windows Server 2022 ISO download page: https://www.microsoft.com/en-us/windows-server

1.  There will be a Try Windows Server now button. Once you click that, there will be another “Download free trial of Windows Server 2022”. After clicking that link, it will bring you to a page with the ISO download.

1.  Set up the VM allocating 4GB of RAM, 2 CPU, and 30GB virtual disk space. You can do more if you choose. For me, it kept throwing an error whenever I finished and tried to run the VM.

1.  To fix this problem, first I went into Task Manager then to the Performance tab. In the CPU section I saw that Virtualization was disabled.
2.  To turn it on, I had to restart my PC and mash the F11 key repeatedly to get into the BIOS. Once there, I enabled SVM Mode, which turned on virtualization so I could run my VM. Depending on your motherboard your steps might be different to enable virtualization.

With SVM Mode enabled, my virtual machine now starts up normally without auto-aborting anymore.

1.  Now with Virtualization enabled, once the Virtual Machine boots up you will be met with the Windows Setup page. Click next after inputting desired settings.

1.  Now you will be met with the type of install you want to do. Always click on the custom installation otherwise the setup will fail.

1.  Now it will ask you where you want to install. There should be one option which is where and how much memory you allocated upon setup. Click next and the installation will begin.

1.  You will be met with the “Server Manager Dashboard”. Congrats you just downloaded Windows Server 2022 successfully.

Windows 11 Installation

Once finished you will be met with this homepage. Now you can start setting up your Active Directory. Before we do that let’s add Windows 11 VM. Click the link https://www.microsoft.com/en-us/software-download/windows11 to get started on the ISO download.

1.  Select the Windows 11 ISO, click confirm. You will then be asked to select the product language. My preferred language is English so that is what I will be going with here. Click confirm again and it will start downloading your Windows 11 ISO.

1.  With the ISO downloaded you can do the setup the same way as the Windows Server 2022 by repeating the steps listed above. The difference will be the VM name as Windows 11, the ISO image, the OS Version, and any other little tweaks you make to the virtual hardware and hard disk.

I found myself having trouble trying to load the ISO onto the VM so that it would boot correctly. I found out through a little research that I should leave the ISO image empty in the VM setup and select it through the DVD drop down menu. Then I had to mash the enter button repeatedly on load. This bypassed my problem and brought me to the setup menu.

So, if you’re having the same problem go back to setup, leave the ISO image empty, start up the VM, and select the DVD from the drop down and then press enter on bootup.

1.  With that fixed we can now proceed as normal. Now press next until you reach the screen below.

1.  Select the box shown here that acknowledges that you agree to the terms to proceed.

1.  After that you will be met with a screen like this. You should press the “I don’t have a product key” button that will then allow you to pick your product license like shown below.

1.  We are going to choose Windows 11 Pro as it has Active Directory Domain Services capability.

1.  I came to find out that I needed more cores and system memory in order to run Windows 11. So, then I went back to the VM settings and changed to the required specs for Windows 11 to run. With that now fixed we are met with the terms which we can see below.

1.  We can click accept on that.

1.  After accepting the terms, we can now choose the disk to install Windows 11 on. There should only be one disk so select that one and click next on this screen.

1.  With that now done clicking install will bring us to the installation page this might take a few minutes, but it should look like the image below if everything was done correctly.

1.  After that’s done you will be met with the Windows 11 setup. Before continuing any further we have to set up our isolated network that our VMs will be on. This is important because this is how we will use Active Directory with our other machines.

To do this we will follow these steps:

1.  In the upper left-hand corner of VirtualBox, click File > Tools > Network Manager. Once you are in the Network Manager, head over to “NAT Networks.”

1.  Copy the above settings. If you do not have a NatNetwork already, click create, and then fill in all the details from the image. Make sure DHCP is enabled.
2.  Now that we have our NatNetwork created, we are going to put our VMs on it. The steps will be the same for each VM.
3.  Right click one of your VMs and click “settings.” Once you are in the settings panel, click on “Network” and change “Attached to:” to the NAT Network.

Account Creation

We will now head back over to our VMs and create our accounts. We are going to start with our Windows 11 host.

1.  If you have not already, go through the initial prompts. Choose your region, keyboard

layout, and make your way to the Account screen. When it asks if you would like to use a personal account or a school/work account, select the 2<sup>nd</sup> option and click next.

1.  You will reach a screen that asks you to sign in. On this screen you are going to want to click the “sign-in options”. This will allow you to join a Domain instead.
2.  Enter the name you want to use for the device (I chose Soleil). Then select the “next” option.

1.  Enter any password you would like on the next screen and confirm your choice. Keep going through these Windows steps until you see that the “Step 1 of 3: Downloading” screen.
2.  Now you can repeat the process for any other VMs you may have.

Configuring Our Hosts

After finishing all the installation steps, we can now configure our host to use our Active Directory Servers IP address as their DNS server.

1.  On each host search control panel in the search bar > click Network and Internet > Network and sharing center > Change adapter settings > right click on the adapter and select properties.

1.  Once you’re on the properties page click “Internet Protocol Version 4 (TCP/IPv4) and click “Properties”.

1.  Now click “Use the following DNS server addresses.” And now we are going to head back over to our Windows Server 2022 VM.
2.  Once you navigate back to your server VM, click the search bar and type “cmd.”
3.  In the command prompt type “ipconfig”
4.  Take note of the IPv4 address that is displayed on your server.

1.  Now head back over to your Windows 11 host and enter the IPv4 address from our server and enter it in the “Preferred DNS server” field.
2.  Repeat this process for the other host machines.

Active Directory Setup

Back to the server manager on Windows Server 2022 which is the application that first booted when you first logged into Windows Server 2022. If you are unable to find it, you can search for it in the search bar.

1.  In the application click “add roles and features”.

1.  The add roles and features wizard page should pop up. Click next until we get to the “Select server roles” page. Here is where we will need to select the “Active Directory Domain Services” and click “add features”.

1.  After selecting AD DS click next until you get to the install page and wait until the features are installed.
2.  Now that Active Directory is installed you can click the flag with the yellow triangle and click “promote this server to domain controller”.
3.  With that done a deployment configuration screen will pop up. Now you click the “add a new forest” button and name your domain. I named mine sparky.local but it can be named anything as long as .local is present at the end.

1.  When finished with all those steps click next and enter the password on the “Domain Controller Options” page. Everything else can be kept as the default settings.

1.  Click next again when you get to the DNS options leaving everything as it is. Once you get to the “Additional Options” page the screen will load for a moment and then fill in your root domain name that you input in the “Deployment Configuration” page minus the .local.
2.  Click the next button until you reach the “Prerequisites Check” page. Here it will check and see if your system can install the AD. Once that is complete you can click install and once that is done you will have to reboot the server.

Creating Users in Active Directory

With everything done and installed we can start creating users.

1.  Upon reboot you will be brought back to the “Server Manager” application. In the top right corner go to tools > Active Directory Users and Computers.

1.  Once inside click the arrow on the left of \[name\].local. Mine is sparky.local, and then go to the “users” folder.

1.  Right click the “users” folder and select new > user. A popup will appear to create a new user object. You can name the user whatever you want. I named the user Soleil Senini. Also fill in the initials and the user logon areas and then click next.

1.  Once on the passwords page create a password for the user. The password can be anything as long as it meets policy requirements. Once a password is chosen, click the “Password never expires” box. This will cause a popup, and it will automatically deselect the “user must change password at next logon” box. In a normal environment you would not want to do this, however, to make it easier for us and because this is an isolated environment this can be done. Once done click next.

1.  Once details are reviewed and correct click finish. This is the very first user created. Repeat for however many VMs you plan to use.

Joining to the Domain

Now that we have as many users as we do VMs being used we can now begin to join them to the domain.

1.  Navigate to the Windows 11 VM and in the search bar, search for “Access work or school”.

1.  Click on connect in the popup. Another popup will appear to enter an email address. Instead of doing that we want to click “Join this device to a local Active Directory domain”.
2.  A popup page called “Join a domain” will appear. This is where you want to put in your \[name\].local domain name to connect the client to the domain.
3.  With that done it will ask you to log in. Use the information you put into the user section in the Active Directory earlier.

1.  After finishing with that it will ask if you want to restart now or later to connect to the domain. Select “Restart now”.

1.  Repeat the process for all the VMs you want connected to the domain.

Conclusion

That’s it, Active Directory is now deployed and configured. This project was created to show off my hands-on learning and provide a safe space in order to simulate real world scenarios so that I can keep building my core IT skills.

Thank you for taking the time to review my project!
