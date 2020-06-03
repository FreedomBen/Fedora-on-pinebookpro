# Fedora-on-pinebookpro

This image contains a base install of Fedora with the GNOME desktop environment. 

Note: Compared to other desktop environements, GNOME 3 seems to run the fastest with Fedora.
Selinux is enabled and set to enforcing mode.

The image can be downloaded using this link: 

https://sourceforge.net/projects/opensuse-on-pinebookpro/files/Rel_2/

The default root password is `linux`.

## Instructions

You must have a SD card with at least 16 GB of storage; preferably 32 GB.

### To flash the image to your SD card
```bash
xzcat fedora-pinebookpro-gnome-0.8.img.xz \
  | dd bs=4M of=/dev/mmcblkX iflag=fullblock oflag=direct status=progress \
 && sync
```
This will allow you to boot Fedora from the SD card. 

### To flash the image on the internal storage of the Pinebook Pro 

To install the Fedora image on the internal storage (eMMC or, if installed, NVME SSD) of the Pinebook Pro, follow these instructions instead. 

You should have an SD card with a linux distribution installed on it. 


Because of the level of compression used, writing this image to the internal storage from pinebookpro via an os running on a sdcard requires you

```bash
unxz --threads=$(nproc) -6e fedora-pinebookpro-gnome-0.8.img.xz
```

from a linux pc, then you can copy the disk image to the os running on pinebookpro via sdcard and enter:

```bash
dd \
  if=fedora-pinebookpro-gnome-0.8.img \
  of=/dev/mmcblkX \
  oflag=sync \
  status=progress \
  bs=32M
```


## Installation and configuration

This image boots up slowly; on the first boot, give it at least 4 to 5 minutes. 

Once the image has booted, the Fedora installation. Simply follow the Fedora Installation instructions. I
https://docs.fedoraproject.org/en-US/fedora/rawhide/install-guide/

After the installation is complete, it is recommended that you change the default root password `Linux` to something more secure. 
To do so, open a terminal and type:

```bash
sudo passwd root
```
You will then be prompted to change it. 


You need to resize the root partition to fill the entirety of your disk. 
To do so, type:
 
```bash
sudo cfdisk /dev/mmcblkX
```

X stands for your disk number. You can find your disk number using the `lsblk` command.

Resize the 6th partition to the end. Scroll over to write, hit enter, type yes when it asks for "yes or no", and then quit. 

After that, type:

```bash
sudo resize2fs /dev/mmcblkXp6
```

### Animations 
To make your experience smoother, it is recommended to you turn the animations off. 
To turn them off, open `gnome-tweaks` -> General Tab -> switch Animations off.

### remove boot variable
Once you know that your system is stable, you can remove the boot variable "maxcpus=4" from /boot/extlinux/extlinux.conf.
This will give you another cpu thread and a considerable speed boost. Disabling it seems to add stability as well.

To get the audio to work, you'll need to unmute 'Left Headphone Mixer Left DAC' and 'Right Headphone Mixer Right DAC'. 
Unfortunately, `gnome-alsamixer`isn't available in the official repository but it can be grabbed here:  https://rpmfind.net/linux/rpm2html/search.php?query=gnome-alsamixer

Thanks to the Manjaro team, suspend and resume is now working correctly. 



*NOTE: This repository was forked from the original one with the intention of making improvements and sending them upstream. Unfortunately, the upstream repository appears to be abandoned.  If you would like to make improvements and want to collaborate, you can send a pull request to this repository.
