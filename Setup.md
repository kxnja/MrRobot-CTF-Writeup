# MrRobot Set up 

# What we need

1.  A laptop or desktop with at least 8 GB of RAM( the more the better though)
2. A stable internet connection to download files

# Steps

1. Install VirtualBox (https://www.virtualbox.org/) and the kali Linux .ova file for VirtualBox (https://www.kali.org/get-kali/#kali-virtual-machines).
2. Install the Mr robot.ova file from vulnhub (https://www.vulnhub.com/entry/mr-robot-1,151/).
3. Run the VirtualBox file first to set up the virtual machine 
4. Run the kali Linux.ova file second and name it as you please(do not change other setting if not needed.
5. Run the mr robot .ova file and treat it as we did on the kali file. (They should both open on virtual box)
6. Right click on the kali Linux machine (DO NOT POWER IT ON FIRST) then open settings and go to the network setting and change it to the internal network from NAT (CHANGE THE NETWORK NAME TO INTNET to avoid difficulties later, I'll explain how).Do the same for the mr robot application. (Change it from bridged adapter to internal network and MUST have the same network name as the kali machine that is INTNET)This is just to make sure they are on the same network.
7. Set up a DHCP server - This is essential to assign IP addresses to your devices (your kali linux machine and mr robot)
	a. Open command prompt by typing cmd on the search box on the task bar (on windows though it is fairly similar on mac or linux)
    b. Type in ```cd /Program Files/Oracle/VirtualBox```
   and click enter (I recommend copying and pasting to avoid errors)
    c. type in   ```vboxmanage dhcpserver add --network=intnet --server-ip=10.38.1.1 --lower-ip=10.38.1.110 --upper-ip=10.38.1.120 --netmask=255.255.255.0  --enable```      (I'll explain ,I know it's a lot)
  8. Final step is now going back to start your kali and mrrobot VM (cheers and happy hacking ethically of course )

# Explanation

So essentially what we have done is that we have used the vboxmanage tool to create a dhcp server for our network (that is why I suggested we use the same name to avoid misconfiguring ) we gave the server the IP - 10.38.1.1 and gave it a range of 10.38.1.110 to 10.38.1.120 to assign to connected devices.
    The netmask basically only defines what part is the network and what part is the host (you). We then finish by enabling it . 

# Note

  Also in this procedure the VMs are completely isolated from your PC and hence you might not be able to browse on your kali VM.
  We isolate it because mrrobot in this case being a vulnerable machine hackers might exploit it and get into your system.
