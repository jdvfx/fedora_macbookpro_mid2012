# Guide: Installing Fedora Linux on Macbook Pro 9,2 (mid 2012)

> you will need a ethernet cable

How to smoothly and safely transistion from MacOS to Fedora Linux.
Instead of re-formating, we'll install Linux on a new drive
and copy over the data from the MacOS drive while using Linux.
That way, MacOS and all the data is kept (as a backup)
After installing Linux, we can boot into MacOS via usb using the Option Key

> what you need
- 1 usb stick : under 5$ for 8GB stick
- 1 internal SSD drive : under $50 for a basic 500GB
- 1 SATA to USB Cable  : under $10
- 1 ethernet cable. the Wifi doesn't work out of the box unfortunately

> create a bootable USB stick with Fedora Linux on it

1) shutdown computer
2) remove the screws at the back of the laptop
3) replace the existing drive with the new SSD drive
4) put the back cover and plug the USB stick
4) start up with the option Key pressed
5) select the USB stick name, that should boot up into a Fedora install
6) update the system, in a terminal: <code>sudo dnf update</code>
7) install the wifi driver:
   - enable RPM fusion free and non-free:</br>
    <code>sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm</code></br>
    <code>sudo dnf install https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm</code>
8) reboot and connect to wifi
9) enable Apple file system</br>
<code>sudo dnf install apfs-fuse</code></br>

9) plug the MacOS drive with the Sata to USB cable
10) mount the MacOS drive</br>
<code>sudo apfs-fuse -o uid=1000,gid=1000,allow_other /dev/sdb2 /run/media/mac</code>

to unmount the drive later on, use:</br>
<code>sudo umount /run/media/mac</code>

11) copy all your personal files from MacOS to Fedora:</br>
replace MAC_USERNAME and LINUX_USERNAME with your user names

<code>rsync -a --progress /run/media/mac/root/Users/MAC_USERNAME/Desktop/ /home/LINUX_USERNAME/Desktop/</code>

repeat for other folders like Pictures,Documents,Downloads,...

