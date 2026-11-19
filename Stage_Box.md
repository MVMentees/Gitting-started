## Putty to new machine and setup for docker images

* Download putty if you haven't already got it

    [Download putty.exe for Windows](https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe) 

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

3. Load packages not in Rocky 9 default

   ```
   sudo dnf install -y epel-release
   sudo dnf install -y podman-compose
   sudo dnf -y install docker git telnet
   sudo dnf -y install httpd
   sudo systemctl enable --now httpd
   ```

5. Add ssh key to be able to communicate with github later

	`ssh-keygen -t rsa` Just press return through all the prompts.

6. Reboot system for changes to take affect

	`sudo shutdown -r now`

7. Assure selinux is disabled

   `cat /etc/sysconfig/selinux`

   Assure it is now SELINUX=disabled

   If not, vi the item and change it to that
	
## Setup GitHub

Connect to the new VM GUI and run Firefox [fire Icon on the bottom left]  Click activities on the top left if nothing shows on the bottom

	Login to github in Firefox on the Docker VM https://github.com/login

	You may need to register with your email the first time

## Add the VM Docker ssh key to your github

2. On a second tab on FireFox, call up your public ssh key

	`file:///home/dockeradmin/.ssh/id_rsa.pub`

    It may ask "What should Firefox do with this file?".  If so, pick Text editor in the pull down box. 

	Select all with \<Ctrl\>A, then copy with \<Ctrl\>C

3. Back in the github site tab, add ssh key to github
	
	```
	Click on profile on the right top of the screen
		Settings >> SSH and GPG keys [New SSH key]
			Title    My Docker VM
                        Paste key from copy above
			[Add SSH key]
	```
