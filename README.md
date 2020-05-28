# Troubleshooting Alfa adapters on Kali 2020.2 via VMWare

There are so many different Alfa Adapters out there, some are plug and play and some require some additional tweaking to work. When I come across issues and fix them, I try my best to keep what I did in a notebook. Overtime I would delete notes I thought I didn't need, or lose them. But overtime I continue to see trends where some folks are having similar conflicts or asking repeitive questions without getting assistance. With that being said, my goal is to help share notes/resolutions for anyone that might have been in the same situation I was in, or trying to fix a new one and are looking for troubleshooting ideas. 

As I come across different challenges with these or new adapters, I’ll keep this page updated with resolution steps that worked for me to help prevent any future headaches and perhaps will help you gain confidence in troubleshooting some common issues. Wireless penetration testing can be a very fun and information security awareness exercise and the last place you want to be in is trying to figure out why your adapter isn’t working. 

### Devices Covered

1. Alfa AWUS036NHA
2. Alfa AWUS036ACH

### AWUS036NHA

#### *Driver Install*

Wireless interface registered out of the box

#### *Monitor Mode*

Airmon-ng sets the interface to monitor mode without issue

### AWUS036ACH

#### *Driver Install*

The wireless interface does not register out of the box and I needed to install the driver from GitHub.

1. sudo apt update
2. sudo apt install build-essential bc libelf-dev linux-headers-`uname -r`
3. sudo apt install dkms
4. git clone -b v5.6.4.2 https://github.com/aircrack-ng/rtl8812au.git
5. cd into the cloned folder
6. sudo ./dkms-install.sh
7. Watch dmesg (sudo dmesg -w) and after a few seconds, usbcore should respond with registering the new interface driver rtl88XXau and your wlan0 interface should be registered

#### *Monitor Mode*

I'm assuming it's a driver conflict, but airmon-ng errors when you attempt to enable monitor mode on the wireless interface with this adapter. However, we can still set it manually.

1. sudo ifconfig wlan0 down
2. sudo iwconfig wlan0 mode monitor / manage
3. sudo ifconfig wlan0 up

### Not getting a prompt to connect to your host?
When you connect your USB device to your host, you should get a prompt from VMWare if you want to connect the device to the host or to a virtual machine. If this happens, then chances are the 'VMware USB Arbitration Service' service is stopped (or the device is dead). You can troubleshoot this by seeing if your host system can see the device and/or try restarting the above service.

![Alt text](https://github.com/gh0x0st/alfa_troubleshooting/blob/master/Screenshots/sc_query.png?raw=true "sc query")
