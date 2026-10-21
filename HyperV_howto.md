1. Download the latest Rocky9 Linux iso.  Rocky 10 is the newest, but use Rocky 9 for now.
    
    [Download Rocky 9](https://rockylinux.org/download)
    
    Select a version:  **9**

    **\[DVD ISO\]**
    
    This downloads Rocky-9.8-x86_64-dvd.iso

2.	If Hyper-V is installed, skip this step.  If not, or you are not sure, do this:
    1. 	Run powershell as an administrator
    2.  Run `Get-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All`
        
        If you see State:  Enabled then it is installed and skip to the next step

    3. Run `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All` and **reboot**
   
        If it is not **State: Enabled**, then HyperV cannot run on your Windows version

3.	Add an internal Virtual switch to allow a known IP to access from VSCode, Accuterm and other desktop tools, again from powershell as an administrator
    ```
	New-VMSwitch -Name "IPRange172" -SwitchType Internal
	New-NetIPAddress `
	-InterfaceAlias "vEthernet (IPRange172)" `
	-IPAddress 172.31.254.1 `
	-PrefixLength 24
    ```

4.	Create a New Virtual machine called MyRocketSoftwareDockers [Creating MyRocketSoftwareDockers VM](https:\\vimeo.xxx)

    1. Run HyperV
    2. Click on New >> Virtual Machine...
    3. Name: **MyRocketSoftwareDockers**
    4. Select **Generation 2**
    5. **4096** Mb memory
    6. Connection: **Default Switch**
    7. Select **Create a virtual hard disk** and take defaults
    8. Select ** Install an operating system from a bootable image file**
    9. Browse Downloads directory and pick **Rocket-9.8-x86_64-dvd.iso**
    10. Click **Finish**
    11. With ***MyRocketSoftwareDockers*** highlighted, click **Settings**
    12. Highlight ***Network Adapter*** and Click **Add**
    13. Virtual Switch:  Select ***IPRange172*** and **Apply**
    14. Click on **Security** in left panel and uncheck **Enable Secure Boot**
    15. **OK** to save
5.  Select **Connect** to run the machine and install the Rocky 9 Release
    1.  Click **Start**
    2.  Highlight ***Install Rocket Linux 9.8*** and press Enter
    3.  Pick Language *Usually defaults of English - English\(United States\) then **Continue**
    4.  Click ***Installation Destination*** then **\[Done\]**
    5.  Click ***Network & Host Name***
        1. Highlight **Ethernet \(Eth1\)** and click **Configure**
        2. Highlight ***IPv6 Settings*** Method **Disabled**
        3. Highlight ***IPv4 Settings*** Method **Manual**
        4. Click **Add**  Address **172.31.245.2**  Netmask **24** Gateway **172.31.254.1**
        5. Click **Save**
        6. Click slide switch in corner and the interface show show IP address 172.31.254.2/24
        7. **\[Done\]**
     6. Click ***User Creation***
        1. Full name **Docker Admin**
        2. User name **dockeradmin**
        3. Check **Make this user administrator**
        4. Put in password.   I used **Rocket123!**
        5. **[Done\]**
     7. Click ***Begin Installation***  *It will take 5 or so minutes*
     8. Click ***Reboot System***
6. Disable Selinux and turnoff firewall
   1. Login to the new system as *Docker Admin* by clicking on the name
   2. This first time you click **No Thanks** for Welcome tour
   3. Click on \{Terminal Icon\} second from the right on the bottom of the screen
   4. Type this in the screen `sudo nano /etc/sysconfig/selinux` and enter password
   5. Arrow down to SELINUX=enforcing and change it to **SELINUX=disabled**
   6. \<Ctrl\> X then **Y** to save and press **Enter** to accept file name
   7. Disable the firewall with `sudo systemctl disable firewalld.service`
   8. Stop the current firewall with `sudo systemctl stop firewalld.service`
7. Install docker package and add item to disable warning message
   1. Type `sudo dnf -y install docker`
   2. Type `docker -v` to show docker is installed.
   3. Type `sudo touch /etc/containers/nodocker` to eliminate warning message
   4. Type `docker -v` and it should no longer show the warning message
8. You MUST now Reboot so selinux is disabled `sudo shutdown -r now`
9.  Your new VM is complete.  You can **X** from the console
10. Download putty.exe if you don't have it
	https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe
11. Use putty to access the new virtual machine
    1.  Copy and Paste it from Download to your Desktop
    2.  click on the putty.exe icon
    3.  Put Host Name \(or IP address\) as **172.31.254.2**
    4.  Click **Open**
    5.  **Accept** the ssh key 
    6.  Login as **dockeradmin** and your password |(mine was Rocket123!\)
    7.  You now have command line access to the new VM.  It makes it easier to cut/paste.
12. Download your first docker build from learn.rocketsoftware.com
    
    This will be in the Downloads directory for your user.  In mine that is c:\Users\dell\Downloads

13. Move the docker from your windows box to the Linux box
    scp "UniVerse Class Image-20260305.zip" dockeradmin@172.31.254.3:/home/dockeradmin/.

14. Using putty, login to the MyRocketDockers VM and build and run the docker
    1.  Login to 172.31.5254.2 or go to the putty already open and unzip, build and run
    2.  Type `ls` and it should show the zip we just copied in the prior step
    3.  Type `mkdir dockers -m777`
    4.  Copy the zip to the new folder `cp Universe\ Class\ Image-20260305.zip dockers/.`
    5.  Go to the dockers directory `cd dockers`
    6.  make a directory for this class `mkdir universe-essentials`
    7.  Go to that directory `cd universe-essentials`
    8.  Unzip the docker files `unzip ../Universe\ Class\ Image-20240305.zip`
    9.  As in the readme.txt, build the docker with `docker build -t universe-essentials`
    10. You can now run it temporary with `docker run -it --rm universe-essentials:latest`
    11. `quit` from the temporary
    12. Create a directory to store your own docker image files with `sudo mkdir /savedUV -m777`
    13. Now run docker with the saved files and redirected ports
    14. `docker run -it -p2223:23 -p32438:31438 --rm -v /savedUV:/mystuff:z universe-essentials:latest`
    15. You can now use putty to get to that running docker by starting a session to the same address **172.31.254.2**, but this time using telnet on port **2223**
