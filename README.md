# Fortigate-80E-Firmware-upgrade

<h2>Background</h2>
I received this old firewall with a previous configuration on it. The device is not yet registered to my FortiCloud account nor is it licenesed. Fortinet makes it a point to do both of those steps but for my purposes I will only be using this firewall for labs. During this beginner walkthrough, I will explain and show how to factory reset the firewall and upgrade the firmware using TFTPd64. If you have no devices currently registered in your FortiCloud, you will need to find someone who does in order to download the firmware for your device. In this tutorial, we will be upgrading from version 7.2.0 to 7.2.7. Don't forget to backup config files before this process starts.
<br />
<h2>Pre-requisites</h2>

- <b>Console cable</b>
- <b>Ethernet cable</b>
- <b>.out file. [Check here to find out what version firmware Fortinet reccommends](https://community.fortinet.com/t5/FortiGate/Technical-Tip-Recommended-Release-for-FortiOS/ta-p/227178)</b>
- <b>[Register your device in FortiCloud to access firmware](www.forticloud.com) (Optional)</b>
<br />

<h2>Walkthrough</h2>
<h3>Pre-staging</h3>

- <b>Connect console cable to the firewall and host</b>

- <b>[Download and install PuTTY based on your architecture](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)</b>
  - <b>Launch Putty and copy settings. We are using serial for this lab. Click open and wait for power to be connected. </b>
 <p align="center">
<img src="https://i.imgur.com/ZZk2P2r.png" height="40%" width="40%" alt="Putty Serial Connection settings"/>

  <p align="center">
<img src="https://i.imgur.com/lKJuEeo.png" height="40%" width="40%" alt="Putty Serial Connection settings"/>
  
- <b>[Download and install TFTPd64](https://bitbucket.org/phjounin/tftpd64/wiki/Download%20Tftpd64.md)</b>

<h3>How to restore device to factory settings using pinhole or CLI</h3>

- <b>Plug in power and grab something thin like a toothpick and hold the reset button next to the power for ~10 seconds. If you miss it within the first 60 seconds you must power cycle device and try again. </b>

- <b>If you have access to the device then sign and the first option does not work then run the following command</b>
<p align="center">
<img src="https://i.imgur.com/qGAlgKY.png" height="40%" width="40%" alt="exec factoryreset"/>

Allow the device to reboot and confirm that device has been reset. Login with default credentials and create your new password.


<h3>TFTP server configuration</h3>

<b>Now that we have console access and the firewall has been reset, we can now reboot the device and configure the TFTP server. </b>

- <b>Connect the RJ45 cable into your host and WAN 1 of the firewall. </b>

  - <b>Change IPv4 Ethernet adapter settings to the following settings</b>
<p align="center">
<img src="https://i.imgur.com/KzAeNg4.png" height="60%" width="60%" alt="IPv4 Adapter Settings"/> 

- <b>Launch TFTPd64 and put firmware file (.out) in a common place with no other files (It may work either way). Rename the file to something basic and keep the extension. </b>
<p align="center">
<img src="https://i.imgur.com/PA58RmX.png" height="60%" width="60%" alt="File location of firmware"/> 

- <b>Configure the TFTP settings to match your ethernet adapter and the directory where the firmware lies. It might be good to check if your ethernet adapter settings were validated upon exit at this point.</b>
<p align="center">
<img src="https://i.imgur.com/Iall2YU.png" height="80%" width="80%" alt="TFTP configuration and ipconfig cmd command to ensure adapter is set correctly"/> 

<h3>Rebooting firewall and using pre-boot menu to upgrade firmware</h3>

The TFTP server is not configured and ready to go. Log back into firewall and enter the following command. Once the device reboots, quickly press and key to stay in the pre-boot menu. Otherwise you will have to repeat this step.
<p align="center">
<img src="https://i.imgur.com/62o2jaq.png" height="50%" width="50%" alt="Enter Boot menu"/> 

- <b>Execute command and click "any" button. You should be met with a screen that looks like this. Press "C" to configure the TFTP parameters</b>
<p align="center">
<img src="https://i.imgur.com/pPURCmD.png" height="50%" width="50%" alt="Configure TFTP settings on firewall"/> 

- <b>Configure each setting as follows. Don't forget replace your file name.</b>
<p align="center">
<img src="https://i.imgur.com/vxpxsi1.png" height="50%" width="50%" alt="Configure TFTP settings on firewall"/> 
  
- <b>All configs are completed. Enter "Q" for quit and "T" to initiate the file transfer. You should be met with a screen that looks like this.</b>
<p align="center">
<img src="https://i.imgur.com/v4vypop.png" height="80%" width="80%" alt="Initiating the transfer"/> 

- <b>You will be asked to set firmware as default, backup, or to Run image without saving. I chose to use as backup to prevent any risk of losing my config. In the following steps we will boot from the upgraded firmware to make sure it is stable. </b>
<p align="center">
<img src="https://i.imgur.com/Pl5YBW1.png" height="80%" width="80%" alt="Set new firmware as backup first"/> 

  -<b>As you see, we are still running the old version<b>
  <p align="center">
<img src="https://i.imgur.com/MLXxhbX.png" height="80%" width="80%" alt="Old version"/> 


<h3>Booting from new firmware and setting as default</h3>
Reboot the device once again and make sure to enter the pre-boot menu. Enter "B" to boot from newly uploaded firmware and set as default. Create new password and enter the "show" command once you login to confirm it was uploaded correctly.
<p align="center">
<img src="https://i.imgur.com/UqdKyUs.png" height="80%" width="80%" alt="New firmware confirmed"/> 



