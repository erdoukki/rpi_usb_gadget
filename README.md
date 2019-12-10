# rpi_usb_gadget
Interface with Raspberry Pi Zero to function as a mass storage device
### Step 1 - Downloading Raspbian Jessie
Download Raspbian Jessie [here](google.com). Once downloaded,
### Step 2 - Enabling the USB Driver
To enable the USB driver, we must edit two files. First, we edit the config.txt
```sudo nano boot/config.txt```
Next, append the line below:
```dtoverlay=dwc2```
Press Ctrl + O, then Enter to save, and Ctrl + X to quit
Second, edit the modules file
```sudo nano /etc/modules```
Then append the line below
```dwc2```
Save and quit (same as with config.txt)
