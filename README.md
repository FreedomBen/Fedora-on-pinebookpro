# Fedora-on-pinebookpro

*NOTE:  I originally forked this repo with the intention of making improvements and sending them upstream, but the upstream appears to be abandoned.  If you make improvements and want to collaborate, either send a PR to this repo or let me know where yours is and I'll send them to you.  I don't particularly need another project but I don't want this to languish either.*

This image contains a base install of Fedora with the Gnome desktop environment. To my surprise, Gnome runs the fastest of any desktop environment I tried with Fedora! Selinux is enabled and set to enforcing mode.

grab the image here:

https://sourceforge.net/projects/opensuse-on-pinebookpro/files/Rel_2/

Instructions: You must have more than 16G of space to write this image! Preferably 32G or larger

To write image to sdcard from a linux pc:

```bash
xzcat fedora-pinebookpro-gnome-0.8.img.xz \
  | dd bs=4M of=/dev/mmcblkX iflag=fullblock oflag=direct status=progress \
 && sync
```

Because of the level of compression I used, writing this image to internal disk from pinebookpro via an os running on a sdcard to internal memory requires you

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

The root password is "linux".

## Caveats

This image boots up slowly. Especially on the first boot, give it 3-4 minutes. I'm going to play around with the kernel configuration to speed this up. I've had the same issue with my opensuse image as well. Once you've booted up and logged in, It's smooth sailing. Also, thanks to the Manjaro team, suspend and resume is now working, so you shouldn't have to powerdown and poweron all that much! Thanks again guys!

____________________________________________________________________________________________________________________________

Once you boot, give it a couple of minutes and the Fedora setup screen will appear, just as it would after a desktop install. Create your username, password, etc. I'd also recommend changing the root password to something other than "linux". After you're set up, open a terminal and type:

```bash
sudo passwd root
```

Then change it.

After that, you'll need to resize the root partition to fill the entirety of your disk. So type:
 
```bash
sudo cfdisk /dev/mmcblkX
```

(the X stands for your disk number. You can get this with the `lsblk` command)

Then resize the 6th partition resize it to the end. Scroll over to write, hit enter, type yes when it asks for "yes or no", and then quit. After that, type:

```bash
sudo resize2fs /dev/mmcblkXp6
```

Then, open gnome-tweaks and turn animations off. It's just going to slow things down, and it performs much better without them.
Also, Chromium runs a lot faster than Firefox on this setup, so if you want it, just run:

```bash
sudo dnf install chromium -y
```

Then you're good to go.

Once you know that your system is stable, you can remove the boot variable "maxcpus=4" from /boot/extlinux/extlinux.conf, and that will give you another cpu thread, and a surprisingly huge speed boost. Disabling seems to add stability, but removing it on the image I'm currently on hasn't caused any problems.

To get audio working, you'll need to unmute 'Left Headphone Mixer Left DAC' and 'Right Headphone Mixer Right DAC'. Gnome-alsamixer isn't available in the official repos but it can be grabbed here:  https://rpmfind.net/linux/rpm2html/search.php?query=gnome-alsamixer

I'll be creating and adding kernel rpm's up here somewhat frequently. This image contains the latest kernel from the pinebook pro gitlab page. I'll probably start creating two kernels for both my Fedora images and openSUSE images. One with an selinux label and the other with an apparmor label. Both could be used on either os, given your preferences.

Anyway, hope you enjoy, and let me know if there are any issues!
