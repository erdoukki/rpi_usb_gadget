# rpi_usb_gadget
Interface with Raspberry Pi Zero to function as a mass storage device
### Step 1 Setting Up Pi Zero
1. Download Raspbian Jessie [here](google.com). 
2. Once downloaded, flash the Raspbian Jessie full or Lite onto the SD card. 
3. Since we are using a recent release of Jessie, create a new file called ```ssh``` in the SD card, as it is disabled by default. Creating this file will enable SSH.
4. Open ```cmdline.txt```.  Insert ```modules-load-dwc2,g_ether``` after ```rootwait```. NOTE: This file separates commands by single spaces!
5. Eject the SD card and and connect it via USB to the computer. Once connected, it will appear as a USB Ethernet device. You can SSH into it by typing ```ssh pi@raspberrypi.local```. You can now access the system files of the Raspberry Pi through your computer.
### Step 2 - Enabling the USB Driver
1. To enable the USB driver, we must edit two files. First, we edit the config.txt by typing```sudo nano boot/config.txt```
2. Next, append the line below:
<br>```dtoverlay=dwc2```
<br>Press Ctrl + O, then Enter to save, and Ctrl + X to quit
3. Second, edit the modules file
<br>```sudo nano /etc/modules```
<br>Then append the line below
<br>```dwc2```
4. Save and quit (same as with config.txt)
### Step 3 - Creating and Mounting a container file using ```dd```
1. This command below will create an empty 1GB file 
<br> ```sudo dd bs=1M if=/dev/zero of/piusb.bin count=1024```
<br> You can alter the size of count to fit your specs
2. Format the file as a FAT32 file system by typing
<br> ```sudo mkdosfs /piusb.bin -F 32 -I```
3. To mount the container file, let us create a folder where we shall mount the file system
<br> ```sudo mkdir /mnt/usb_share```
4. To mount the container file automatically at boot time, we have to edit the file system table
<br> ```sudo nano /etc/fstab```
<br>Append the line below to the end of the file
<br> ```/piusb.bin /mnt/usb_share vfat users,umask=000 0 2```
<br>Ctrl + O to save, then, Ctrl + X to quit
### Step 4 - Replace g_ether with g_mass_storage
1. To make the Pi Zero appear as a mass storage device, we have to edit the cmdline.txt and modules files again.
<br>For the cmdline.txt:
<br>```sudo nano /boot/cmdline.txt```
<br>Replace ```g_ether``` with ```g_mass_storage```
<br> And for modules:
<br>```sudo nano /etc/modules```
<br> Append the line with ```g_mass_storage```, after the ```dwc2``` line we did earlier
### Step 5 - Load the module at boot time
1. We create a script in order to initialize the modeule using ```modprobe```. First, type
<br>```sudo nano /etc/init.d/scriptname```
<br> Put the script below
<br>```case "$1" in```
<br>&nbsp;&nbsp;&nbsp;&nbsp;```start)```
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;```sudo modprobe g_mass_storage file=/piusb.bin stall=0 ro=0```
<br>&nbsp;&nbsp;&nbsp;&nbsp;```stop)```
<br>&nbsp;&nbsp;&nbsp;&nbsp;```*)```
<br>```esac```
```
case "$1" in
  start)
    sudo modprobe g_mass_storage file=/piusb.bin stall=0 ro=0
  stop)
  *)
```
