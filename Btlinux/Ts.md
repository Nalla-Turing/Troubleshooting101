# Bluetooth not working in PoPOS!
### Date:- 16 Feb 2025

## Problem Statement
Bluetooth not working at all. Even when I go to settings, to turn on the the toggle, nothing happens.
Turning off the toggle shows the bluettoh icon in the system menu but turning in bluetooth from there also just turns it off on it's own.


## Solution Attempts
I search up my problem to find the troubleshooting page of System76 themselves.
(I hope it works 🙂 )
## Attempt-1
Reinstalling the official Linux Bluetooth stack `bluez` and the `gnome-bluetooth` (GNOME tool for managing bt devices)

    sudo apt reinstall --purge bluez gnome-bluetooth

After reinstallation, shutdown your machine and power it back on, so that your hardware can completely reset.

### Result-Failure
<br>

## Attempt-2
Using `TLP` (a free, open-source, command-line utility designed to optimize battery life on Linux laptops by tweaking kernel settings that impact power consumption, offering both default and customizable power-saving options)
to change the settings interferring with Bluetooth functionality

1:- edit the `tlp.conf` file 
    sudo nano /etc/tlp.conf

2:- Change the settings as follows
    WIFI_PWR_ON_AC=off
    WIFI_PWR_ON_BAT=off
    USB_AUTOSUSPEND=0
    USB_EXCLUDE_BTUSB=1
    USB_DENYLIST="your bluettoh device id"

3:- Save the settings and restart tlp
    sudo systemctl restart tlp

4:- Check the autosuspend setting
    tlp-stat --usb

If the BLuetooth is in `control = auto` change it to `on`
    echo 'options btusb enable_autosuspend=n' | sudo tee /etc/modprobe.d/btusb.conf

reload the Bluetooth module Using
    sudo modprobe -r btusb
    sudo modprobe btusb

And after check the autosuspend setting again reboot the system
    sudo reboot 
Or just shut down and restart

### Result - Failure
<br>

## Attempt -3
Install blueman and use it to turn on Bluetooth.

### Result -  blueman shows the bluetooth icon and then vanishes in a few seconds
<br>

## Attempt - 4
1- Checking if my bluetooth devices/ interfaces is showing
    hciconfig -a

It seems that OS is recognizing my adapter but not being able to initialize it properly
as `BD Address: 00:00:00:00:00:00` 

2:- Trying to restart the adapter and then checking the config
<!-- cobaltcheese@pop-os:~$ sudo hciconfig hci0 reset
[sudo] password for cobaltcheese: 
Can't get device info: No such device -->

Hmm it's strange, it means the Bluetooth adapter (hci0) is not properly recognized at the system level.
while `lsusb` shows the device `(IMC Networks Wireless_Device)` used for bluetooth.
Checking if I have the required firmware file
    ls /lib/firmware | grep -i bt

It seems to have some no missing firmware files but then why the fuck is it not working :(
