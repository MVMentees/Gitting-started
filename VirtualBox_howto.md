# Download packages needed

[Download Rocky Linux 9.9](https://download.rockylinux.org/pub/rocky/9/isos/x86_64/Rocky-9.8-x86_64-dvd.iso)

[Download VirtualBox for Windows](https://download.virtualbox.org/virtualbox/7.2.20/VirtualBox-7.2.20-175154-Win.exe)

[Download putty.exe for Windows](https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe) used to stage the box later

# Load Virtual Box by running the exe that downloaded for it

*If VirtualBox will not load, this is the C++ redistributable you need*

[Download C++ redistributable](https://download.visualstudio.microsoft.com/download/pr/b43c7beb-2917-4ee5-bbad-947ae7c7ed04/6AFAE68A783F11292149175844AED0E2CE3F247BC0250F6CB18C931295B3F399/VC_redist.x64.exe)

After the install is complete, you have to answer it installed correctly

# Create Virtual Machine for Docker

## Add New virtual Machine

Run Virtual Box
* Click Machine >> New
```
VM Name   MyRocketSoftwareDockers
        VM Folder <take default>
        ISO Image c:...\Downloads\Rocky9....iso
[Next] or could be just next section
	User Name   dockeradmin
	Password    Rocket123!
[Next] or could be just next section
	Base Memory    4096
  	Number of CPUs    1
	Disk Size        60 GB
[Next] or could be just next section
[Finish]
```

## Mount Rocky9 iso and restart

It will fail to boot, until you load the ISO

* Devices>>Optical drive
```
click on the Rocky9 iso
click Machine>>Reset
Up arrow to Install Rocket Linux 9.9 and press Enter
Highlight English and English(United States) [Continue]
Installation Destination [Done]
User Creation
	Full Name  Docker Admin
	User Name  dockeradmin
	check both Make this user administrator and Require a password to use this account
	Password   Rocket123!
	Confirm    Rocket123!
[Done]
[Begin installation][Reboot]
```

## Turn new machine off

* Machine >> Shutdown

## Check Host-Only Network adapter address

* File >> Tools >> Network

See what address the VirtualBox Host-Only Ethernet Adapter has, mine was 192.168.56.1

## Add a network adapter to the MyRocketSoftwareDockers VM
	
* Click on the second picture on the left side which will redisplay the machines
* For the **MyRocketSoftwareDockers*  Settings >> Network >> Adapter 2
```
Check Enable Network Adapter
Attached to    Host-only Adapter
Leave other defaults and click [OK]

## Start virtual machine and configure second network adapter

* Login as dockeradmin
* Click on 9 dot square
* Settings >> Network and change second adapter
```
IPv6    Disabled

IPv4    Manual      Address   192.168.56.2    Netmask 255.255.255.0      Gateway   <leave blank>

## Reboot machine and stage based on Stage_Box document
