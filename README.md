# Guide: Installing Fedora Linux on Macbook Pro 9,2 (13" mid 2012)

> you will need an ethernet cable

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
- Download the Fedora ISO & Fedora Media Writer:</br>
https://fedoraproject.org/workstation/download
- Alternatively, flash the ISO to the USB stick with Etcher:</br>
https://etcher.balena.io/

remove SSD or HardDrive:
https://www.ifixit.com/Guide/MacBook+Pro+13-Inch+Unibody+Mid+2012+Hard+Drive+Replacement/10378</br>

1) shutdown computer
2) remove the screws at the back of the laptop
3) replace the existing drive with the new blank SSD drive

4) put the back cover and plug the USB stick with the Fedora installer
4) start up with the option Key pressed
5) select the USB stick name, that should boot up into a Fedora install
6) update the system, in a terminal: <code>sudo dnf update</code>
7) install the wifi driver:
   - enable RPM fusion free and non-free:</br>
    <code>sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm</code></br>
    <code>sudo dnf install https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm</code>
    - install wifi driver</br>
    <sudo dnf broadcom-wl akmod-wl>

   - reboot. if the wifi works, lock the linux kernel to the current version:
   <code>gnome-text-editor /etc/dnf/dnf.conf</code></br>
   under [main] add: exclude=kernel*

   > the wifi adpater is a Broadcom BCM4331</br>
   you can double check what kernel version works with it:
   https://linux-hardware.org/?id=pci:14e4-4331-106b-00f5&page=5#status

   > if all fails, an easy fix is to use a wifi dongle, eg: TP-Link N150


8) reboot and connect to wifi
9) enable the Apple file system</br>
<code>sudo dnf install apfs-fuse</code></br>

9) plug the MacOS drive with the Sata to USB cable
10) mount the MacOS drive</br>
- first check the drive letter with:
<code>gnome-disks</code></br>
it should be the last drive, with the largest partition</br>
<em>Contents: APFS - Not Mounted</em> eg: /dev/sdb2</br>
<code>sudo apfs-fuse -o uid=1000,gid=1000,allow_other /dev/sdb2 /run/media/mac</code></br>
Enter your MacOS password, the disk is most likely encrypted.

to unmount the drive later on, use:</br>
<code>sudo umount /run/media/mac</code>

11) copy all your personal files from MacOS to Fedora:</br>
replace MAC_USERNAME and LINUX_USERNAME with your user names

<code>rsync -a --progress /run/media/mac/root/Users/MAC_USERNAME/Desktop/ /home/LINUX_USERNAME/Desktop/</code>

repeat for other folders like Pictures, Music, Documents, Downloads, ...

13) setup sleep when lid is close</br>
<code>sudo gnome-text-editor /usr/lib/systemd/logind.conf</code></br>
un-comment (remove #) on this line: #HandleLidSwitch=suspend

14) control fans (optional)
<code>sudo dnf install mbpfan lm_sensors</code></br>
<code>sudo systemctl enable mbpfan</code></br>
<code>sudo systemctl start mbpfan</code></br>
- check mbpfan is running: </br>
<code>sudo systemctl status mbpfan</code></br>

- the fan config file is located in: /etc/mbpfan.conf

15) enable dark mode by default (optional)</br>
<code>gsettings set org.gnome.desktop.interface gtk-theme Adwaita-dark && gsettings set org.gnome.desktop.interface color-scheme prefer-dark</code>

------------------------------------------------------

> Fedora cheat sheet / how to re-install the wifi driver on an older kernel</br>
boot into an older kernel: press F8 on boot</br>
after login, set the default kernel to that version:</br>
list available kernels:</br>
<code>sudo grubby --info=ALL | grep -E "^kernel|^index"</code></br>
change the default kernel version on boot, eg: index=1</br>
<code>sudo grubby --set-default-index=1</code>
then re-install the driver: <code>sudo dnf reinstall broadcom-wl</code></br>
if kernel-devel is missing, let's install it</br>
get the current kernel version:
<code>uname -r</code></br>
6.11.11.200.fc40.x86_64</br>
to rebuild the wifi driver for another kernel, we'll need kernel-devel
with the corresponding kernel version</br>
download the correct version:<br>
https://koji.fedoraproject.org/koji/packageinfo?buildStart=0&packageID=8&buildOrder=-completion_time&tagOrder=name&tagStart=0#buildlist</br>
then install with: <code>sudo rpm -i ~/Downloads/kernel-devel.6.11.11-200.fc40.x86_64.rpm</code></br>
then re-install the driver: <code>sudo dnf reinstall broadcom-wl</code></br>
reboot






