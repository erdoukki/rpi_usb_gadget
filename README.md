# rpi_usb_gadget
Interface with Raspberry Pi Zero to function as a mass storage device
### Step 1 - Downloading Raspbian Jessie
1. Download Raspbian Jessie [here](google.com). 
2. Once downloaded, flash the Raspbian Jessie full or Lite onto the SD card. 
3. Since we are using a recent release of Jessie, create a new file called ```ssh``` in the SD card, as it is disabled by default. Creating this file will enable SSH.
4. Open ```cmdline.txt```.  Insert ```modules-load-dwc2,g_ether``` after ```rootwait```. NOTE: This file separates commands by single spaces!
### Step 2 - Enabling the USB Driver
To enable the USB driver, we must edit two files. First, we edit the config.txt
<br>```sudo nano boot/config.txt```
<br>Next, append the line below:
<br>```dtoverlay=dwc2```
<br>Press Ctrl + O, then Enter to save, and Ctrl + X to quit
<br>Second, edit the modules file
<br>```sudo nano /etc/modules```
<br>Then append the line below
<br>```dwc2```
<br>Save and quit (same as with config.txt)
