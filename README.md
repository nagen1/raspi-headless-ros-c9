# raspi-headless-ros-c9
The below steps will help you to setup raspberry pi with raspbian-lite headless (no UI) and connects to your WI-FI on initial boot and then update SSH permissions in raspi-config.

## Preparing Raspberry pi headness installation
### Download raspbian lite from below link
http://downloads.raspberrypi.org/raspbian_lite/images/raspbian_lite-2017-07-05/

### Writing an image to the SD card
You will need to use an image writing tool to install the image you have downloaded on your SD card.

https://etcher.io/
Etcher is a graphical SD card writing tool that works on Mac OS, Linux and Windows, and is the easiest option for most users. Etcher also supports writing images directly from the zip file, without any unzipping required. To write your image with Etcher:

Download Etcher and install it.
Connect an SD card reader with the SD card inside.
Open Etcher and select from your hard drive the Raspberry Pi .img or  .zip file you wish to write to the SD card.
Select the SD card you wish to write your image to.
Review your selections and click 'Flash!' to begin writing data to the SD card.

### Prepare Wi-Fi settings (don't boot Pi yet)
Eject the SD card and insert it to PC/Mac again. 
It will show up as Boot external drive
- Double click to open / right click open 
- create a file name wpa_supplicant.conf 
- Copy paste the below code and save file (NOTE: the file has to be place in SD card and when it's boots 1st time it will auto configure you Pi Wi-Fi):
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

network={
    ssid="your wi-fi name"
    psk="wi-fi password"
    }
```
Save the changes and Eject the SD card
Insert the SD card in Raspberry Pi and Boot it ( this should work on all versions of Pi) - I tested with Pi3, Zero
Raspberry Pi should boot up and connects to your network. To see it's connected to your network goto 192.168.1.1 your router network URL.

At this point, need to connect Monitor and keyboard just to enable ssh in raspi-config after then good to go without headless
