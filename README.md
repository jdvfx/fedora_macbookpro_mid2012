# guide: installing Fedora Linux on Macbook Pro 9,2 (mid 2012)

>>> you'll need a ethernet cable <<<

> How to smoothly and safely transistion from MacOS to Fedora Linux.
Instead of re-formating, we'll install Linux on a new drive
and copy over the data from the MacOS drive while using Linux.
That way, MacOS and all the data is kept (as a backup)
To boot into MacOS, all we need is to plug the drive back via USB
and press the Option key to boot into MacOS.

> what you'll need
- 1 usb stick : under 5$ for 8GB stick
- 1 internal SSD drive : under $50 for a basic 500GB
- 1 SATA to USB Cable  : under $10
- a ethernet cable. the Wifi don't work out of the box unfortunately

> create a bootable USB stick with Fedora Linux on it

1) shutdown computer
2) remove the screws at the back of the laptop
3) replace the existing drive with the new SSD drive
4) put the back cover and plug the USB stick
4) start up with the option Key pressed
5) select the USB stick name, that should boot up into a Fedora install
6) update the system, in a terminal: <code>sudo dnf update</code>
7) install the wifi driver:
   - enable RPM fusion free and non-free:
    <code>sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm</code>
    <code>sudo dnf install https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm</code>







