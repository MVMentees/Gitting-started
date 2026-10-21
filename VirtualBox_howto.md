# Download packages needed

[Download Rocky Linux 9.8](https://download.rockylinux.org/pub/rocky/9/isos/x86_64/Rocky-9.8-x86_64-dvd.iso)

[Download VirtualBox for Windows](https://download.virtualbox.org/virtualbox/7.2.20/VirtualBox-7.2.20-175154-Win.exe)

[Download putty.exe for Windows](https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe)

# Load Virtual Box by running the exe that downloaded for it

*If VirtualBox will not load, this is the C++ redistributable you need*

[Download C++ redistributable](https://download.visualstudio.microsoft.com/download/pr/b43c7beb-2917-4ee5-bbad-947ae7c7ed04/6AFAE68A783F11292149175844AED0E2CE3F247BC0250F6CB18C931295B3F399/VC_redist.x64.exe)

After the install is complete, you have to answer it installed correctly

# Creat Virtual Machine for Docker

## Add New virtual Machine

Run Virtual Box
* Click Machine >> New
```
VM Name   MyRocketSoftwareDockers
        VM Folder <take default>
        ISO Image c:...\Downloads\Rocky9....iso
[Next]
	User Name   dockeradmin
	Password    Rocket123!
[Next]
	Base Memory    4096
  	Number of CPUs    1
	Disk Size        60 GB
[Next] [Finish]
```

## Mount Rocky9 iso and restart

It will fail to boot, until you load the ISO

* Devices>>Optical drive
```
click on the Rocky9 iso
click Machine>>Reset
Up arrow to Install Rocket Linux 9.8 and press Enter
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
	
* Settings >> Network >> Adapter 2
```
Attached to    Host-only Adapter

## Start virtual machine and configure second network adapter

* Login as dockeradmin
* Click on 9 dot square
* Settings >> Network and change second adapter
```
IPv6    Disabled

IPv4    Manual      Address   192.168.56.2    Netmask 255.255.255.0      Gateway   <leave blank>

## Putty to new machine and setup for docker images

* Copy putty.exe from Downloads to your desktop

* Connect to new machine with putty

	Host Name \(or IP address\)
	
		dockeradmin@192.168.56.2 [Open]

## Stage the new Docker VM

1. Turn off Selinux

	`sudo sed -i 's/^SELINUX=enforcing/SELINUX=disabled/' /etc/sysconfig/selinux`

2. Disable and stop firewall

	`sudo systemctl disable firewalld.service`
	`sudo systemctl stop firewalld.service`

3. Load docker, git and telnet

	`sudo dnf -y install docker git telnet`

4. Create dockers directory

	`mkdir -m777 /home/dockeradmin/dockers`
	
	Put each docker build in a separate sub-folder
        Leave the zips for them in the dockers directory

5. Create a directory for the docker files on the host
	
	`mkdir -m777 /home/dockeradmin/PBD`  \(for Pickleball Docker\)

6. Add ssh key to be able to communicate with github later

	`ssh-keygen -t rsa` Just press return through all the prompts.

6. Reboot system for changes to take affect

	`sudo shutdown -r now`
	
## Setup GitHub

Connect to the new VM GUI and run Firefox [fire Icon on the bottom left]  Click activities on the top left if nothing shows on the bottom

	Login to github in Firefox on the Docker VM https://github.com/login

	You may need to register with your email the first time

## Download the Pickleball Docker build zip

`https://github.com/MVMentees/MV-Dockers/blob/main/build_universe-pickleball.zip`

Click the download icon on the right side of the grey highlighted bar

This download the file to the /home/dockeradmin/Downloads directory

## Add the VM Docker ssh key to your github

2. On a second tab on FireFox, call up your public ssh key

	`file:///home/dockeradmin/.ssh/id_rsa.pub`

	Select all with \<Ctrl\>A, then copy with \<Ctrl\>C

3. Back in the github site tab, add ssh key to github
	
	```
	Click on profile on the right top of the screen
		Settings >> SSH and GPG keys [New SSH key]
			Title    My Docker VM
                        Paste key from copy above
			[Add SSH key]
	```

## Load Pickleball Repo

Add Pickleball repo to your home directory

Login to the Docker VM with putty or use terminal from the GUI desktop

`git clone git@github.com:MVMentees/Pickleball.git`

## Create the docker for universe-pickleball

Copy the docker zip to the dockers directory

	`cp /home/dockeradmin/Downloads/build_universe-pickleball.zip /home/dockersadmin/dockers/.`

Make the directory for the universe-pickleball docker build

	`mkdir -m777 /home/dockeradmin/dockers/universe-pickleball`

Unzip the zip file

	`cd /home/dockeradmin/dockers/universe-pickleball`
	`unzip ../build_universe-pickleball.zip`

Add the UniVerse TE release

    This has to be requested here and they will email it to you.   Either using scp or winscp or using a email client using Firefox, you need to get the UVTE...zip file into this same directory.

Build the docker image

	`docker build -t universe-pickleball .`

	See the readme for details on how to run the new docker image
	








	


