# Fedora on the Pinebook Pro
This repository serves as an installation guide for Fedora on the Pinebook Pro. 

The Pinebook Pro is a Free and Open Source ARM 64-Bit laptop made by PINE64, a community driven company focused on creating ARM devices.

You can learn more about PINE64 <a href="/https://www.pine64.org/">here</a>. 


## Download 
This image contains a base installation of Fedora with the GNOME desktop environment. 
SELinux is enabled and set to enforcing mode.

The Fedora image can be downloaded using this link: 

https://sourceforge.net/projects/opensuse-on-pinebookpro/files/Rel_2/

The default root password is `linux`.

## Instructions
You must have a SD card with at least 16 GB of storage; preferably 32 GB.

### To flash the image to the SD card
- Plug your SD Card in your computer.
- Download the Fedora image.
- Open the terminal and run:
```bash
xzcat fedora-pinebookpro-gnome-0.8.img.xz | dd bs=4M of=/dev/mmcblkX iflag=fullblock oflag=direct status=progress && sync
```
This will install Fedora on the SD card and allow you to boot from it. 

### To flash the image on the internal storage of the Pinebook Pro 

To install the Fedora image on the internal storage (eMMC or, if installed, NVME SSD) of the Pinebook Pro, follow these instructions instead. 
You should have a bootable SD card for your Pinebook Pro with a linux distribution installed on it. 
- Boot from the SD card.
- Download the Fedora image or copy it to the Pinebook Pro.

Because of the level of compression used, writing the image to the interal storage requires you to unlock the Pinebook Pro threads.
- Decompress the image and unlock the threads:
```bash
unxz --threads=$(nproc) -6e fedora-pinebookpro-gnome-0.8.img.xz
```
- To install on the internal storage:
```bash
dd if=fedora-pinebookpro-gnome-0.8.img of=/dev/mmcblkX oflag=sync status=progress bs=32M
```

## Installation and configuration
This image boots up slowly. On the first boot, give it at least 4 to 5 minutes. 

Once the image has booted, Simply follow the Fedora Installation instructions. 
- If needed, the official Fedora Installation Guide is avalaible <a href="https://docs.fedoraproject.org/en-US/fedora/rawhide/install-guide/">here</a>. 

## Post installation
### Root password
After the installation is complete, it is recommended that you change the default root password `linux` to something more secure.
- Open a terminal and run:
```bash
sudo passwd root
```
You will then be prompted to change it. 

## Resizing root partition
- Resize the 6th partition to fill the entirety of your disk:
```bash
sudo cfdisk /dev/mmcblkX

sudo resize2fs /dev/mmcblkXp6
```
Note: X stands for your disk number. You can find your disk number using the `lsblk` command.
### Animations 
To make your experience smoother, it is recommended to turn the animations off. 
- Open `gnome-tweaks` -> General Tab -> switch Animations off.

### Audio

The easiest way to get the audio to work properly is by using the `gnome-alsamixer`. 

However,`gnome-alsamixer`isn't available in the official repository. You can download it <a href="/https://rpmfind.net/linux/rpm2html/search.php?query=gnome-alsamixer/">here</a>.
- unmute both 'Left Headphone Mixer Left DAC' and 'Right Headphone Mixer Right DAC'.

### Remove boot variable
Once your system is configured and stable:
- remove the boot variable `maxcpus=4` from `/boot/extlinux/extlinux.conf`.

This will give you another CPU thread and a considerable speed boost. Disabling it seems to add stability as well.

## Getting help
Official PINE64 Wiki:https://wiki.pine64.org/index.php/Main_Page/

PINE64 Forum: https://forum.pine64.org/

If you're having difficulties installing an OS image on your PINE64 device, you can use the <a href="/https://github.com/pine64dev/PINE64-Installer/blob/master/README.md/">official PINE64 Installer</a>.

Note: Fedora is not avalaible using the PINE64 Installer. 

## About
*This repository was forked from the original one with the intention of making improvements and sending them upstream. Unfortunately, the upstream repository appears to be abandoned. If you would like to make improvements or would like to collaborate, feel free to send a pull request to this repository instead.*
