# Active-Directory-Home-Lab-Setup-Guide
In this lab, I configure a Windows Server 2022 and deploy it as a Domain Controller (DC) in VMware Workstation. I set up a Windows 10 machine and connect it to the DC. I create a user and give admin rights. I install Remote Server Administration Tools (RSAT) to enable remote Active Directory (AD) management from the client system, then test it.

# Step 1 Setting up Windows Server 2022
Download VMware Workstation Pro 17, Windows Server 2022 ISO, and Windows 10 ISO. To simplify the installation process, make sure to download the ISO versions of both Windows Server and Windows 10. Using ISO files eliminates the need to create bootable USB drives or burn installation media to a CD/DVD. We will use VMware Workstation to directly mount the ISO file to the Virtual Machine (VM) and access the installation files in this step. After installing VMware, click on "Create a New Virtual Machine". 

![Image Alt](https://github.com/AyboFrankOz/Active-Directory-Home-Lab-Setup-Guide/blob/d05083a1da2787d92125eb61b32e27c394ac91f1/images/1.%20VMware%20Workstation%20Pro%20user%20interface.PNG)

Proceed with the wizard: "Typical" > "I will install the operating system later" > Guest operating system: "Microsoft Windows"; Version: "Windows Server 2022" > Virtual machine name: "Windows Server 2022" (You can type anything you like) > Maximum disk size: "60 GB" & "Split virtual disk into multiple files" > Before clicking on "Finish" click on "Customize Hardware".

Increase the Memory to "4 GB (4096 MB)" from the Memory tab.

![Image Alt](https://github.com/AyboFrankOz/Active-Directory-Home-Lab-Setup-Guide/blob/e5dd2d621f5b7b06c27e95ab065a92b3fddac30f/images/2.%20Customizing%20Hardware.PNG)

Increase the number of cores  per processor to 2 from the Processors tab. All of these configurations can be adjusted based on your system’s capabilities. You may increase CPU, RAM, and storage allocations if your machine has sufficient resources to ensure better performance. Alternatively, you can leave them at the minimum or recommended system requirements. One of the key advantages of using virtualization tools is flexibility, which means we can easily modify CPU, RAM, and storage allocations even after creating the virtual machines.   

From the CD/DVD (SATA) tab under Connection, click on "Use ISO image file" then Browse. Select the Windows Server 2022 (ISO) that was downloaded. From the Network Adapter tab, uncheck "Connect at power on" option. This prevents Windows from attempting to download updates during installation, which will speed up the installation. Of course, updates are essential for security, but since this is a home lab environment, Windows can update itself later when we turn on the Network. Click on "Close" then "Finish". 

Once the environment is created, click on "Power on this Virtual Machine" to begin the Operating System (OS) installation.

![Image Alt](https://github.com/AyboFrankOz/Active-Directory-Home-Lab-Setup-Guide/blob/c8dedffdac8aa03d97aae8de94d67e3bc382a35d/images/3.%20VM%20is%20ready%20to%20run.PNG)

Quickly press "any key" when prompted to boot from the CD/ISO. If you are late to press any key to initiate the setup, you need to restart the Virtual Machine. You can do this action via VMware Workstation's menu bar: VM > Power > Restart Guest

![Image Alt](https://github.com/AyboFrankOz/Active-Directory-Home-Lab-Setup-Guide/blob/a907e39874a440d614d389a47763528283a8d49c/images/4.%20%20Initial%20Boot%20-%20press%20any%20key%20prompt.PNG)
![Image Alt](https://github.com/AyboFrankOz/Active-Directory-Home-Lab-Setup-Guide/blob/a907e39874a440d614d389a47763528283a8d49c/images/5.%20Setup%20starts.PNG)

Select "Language, Time and currency format, Keyboard" then click Next. Click on "Install Now". Select an operating system with the Desktop Experience option, "Windows Server 2022 Standard Evaluation (Desktop Experience)" in our case. Next. Accept the Software License Agreement and click Next. Click on "Custom: Install Microsoft Server Operating System only (advanced)" as we are installing Windows for the first time. Click Next, as it will automatically find the disk location that we created before. Once the installation finishes, it will ask us to type a password for the Administrator Account.  

Now, it is ready! We have to press Ctrl+Alt+Del to enter our password for the Administrator Account. However, pressing Ctrl+Alt+Del as usual does not work in a virtual environment. To progress, click VM on the menu bar, then "Send Ctrl+Alt+Del". Alternatively, you can click on the icon next to the "Orange Pause Button", shown below.   
![Image Alt](https://github.com/AyboFrankOz/Active-Directory-Home-Lab-Setup-Guide/blob/69fc563794a7d67ed8fbef996467a7b936dd7263/images/6.%20Ctrl%20%2B%20Alt%20%2B%20Del.PNG)

Enter your password to log in. Every time you log in to a Windows server, the server manager will run by default. You can prevent the server manager from starting automatically via: On the Server Manager Dashboard menu bar, Manage > Server Manager Properties, then click on "Do not start Server Manager automatically at logon".

# Step 2 Setting up VMware Tools
Installing VMware tools enables features such as copy and paste between the host and the VM, provides better graphics, and improves performance.  
On the VMware Workstation menu bar, click VM > Install VMware Tools. 
![Image Alt](https://github.com/AyboFrankOz/Active-Directory-Home-Lab-Setup-Guide/blob/4132c64030ec43f4465c234872de1e036ebe29b5/images/7.%20Before%20VMware%20tools.PNG)

The setup will start automatically. If it does not, open File Explorer, click on the "DVD Drive (D): VMware Tools" under the "This PC" tab, which is located on the left side of the screen. Double-click on "setup" and complete the setup wizard. The screen may flicker during installation, but when it is done, you will feel the difference.

![Image Alt](https://github.com/AyboFrankOz/Active-Directory-Home-Lab-Setup-Guide/blob/4132c64030ec43f4465c234872de1e036ebe29b5/images/8.%20After%20VMware%20tools.PNG)

After exiting the setup wizard, a system restart prompt will appear. Don't restart it yet.

# Step 3 Changing the Computer's Name
Right-click on Start > System. 
![Image Alt](https://github.com/AyboFrankOz/Active-Directory-Home-Lab-Setup-Guide/blob/f411f0ff485ce956fd4c9720de9c36642ea4b56d/images/9.%20Right%20click%20Start.PNG)

The "About" page will open. Scroll down and find "Rename this PC (Advanced).
![Image Alt](https://github.com/AyboFrankOz/Active-Directory-Home-Lab-Setup-Guide/blob/f411f0ff485ce956fd4c9720de9c36642ea4b56d/images/10.%20About%20Page.PNG)

"System Properties" window will open. Click on "Change".
![Image Alt](https://github.com/AyboFrankOz/Active-Directory-Home-Lab-Setup-Guide/blob/f411f0ff485ce956fd4c9720de9c36642ea4b56d/images/11.%20System%20Properties.PNG)

Type a computer name. In our case, "DC01". 
![Image Alt](https://github.com/AyboFrankOz/Active-Directory-Home-Lab-Setup-Guide/blob/f411f0ff485ce956fd4c9720de9c36642ea4b56d/images/12.%20Computer%20Name%20Change.PNG)

When you click "Ok", a system restart prompt will appear again. Click OK to restart the server.
![Image Alt](https://github.com/AyboFrankOz/Active-Directory-Home-Lab-Setup-Guide/blob/f411f0ff485ce956fd4c9720de9c36642ea4b56d/images/13.%20Restart.PNG)

# Step 4 Taking a Fresh Windows Server 2022 Snapshot - Cloning (Optional)
Cloning is the process of creating an exact copy of a VM. This is useful for testing changes on a duplicate before applying them to the primary machine. On the VMware Workstation menu bar, click on VM > Snapshot > "Take Snapshot". Give a name and a description to differentiate other clones you might have. Click on "Take Snapshot" to clone.
![Image Alt](https://github.com/AyboFrankOz/Active-Directory-Home-Lab-Setup-Guide/blob/6ef06c7e1d6a3da1e550808a3dad68133540c2b8/images/14.%20Clone%20Name%20and%20Description.PNG)

# Step 5 Enabling Network
Now, we can reconnect the network in the VMware settings to allow the OS to perform updates. Before logging in to the server, from the VMware Workstation menu bar, click on "VM" then "Settings". Virtual Machine Settings will open. Click on "Network Adapter" and check "Connected" and "Connect at power on" options under Devices Status if they are unchecked.
![Image Alt](https://github.com/AyboFrankOz/Active-Directory-Home-Lab-Setup-Guide/blob/a2d49e9c0c59f6b31973388a1cd0b0113eef75f4/images/15.%20Network%20Adapter%20Settings.PNG)

When you log in to the server, a blue banner will pop up asking if you want to allow this PC to be discoverable by other PC's and devices on this network. Click yes.

# Step 6 Deploying the Server as a Domain Controller
