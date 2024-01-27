---
title: "So, You Bricked PmOS"
date: 2024-01-26T23:15:27-08:00
draft: false
---

Oh no! You broke the GUI while doing an upgrade with pmOS. It might be possible to fix it with chroot, and this short tutorial will show you how.

The following was tested using an x86 laptop, and a oneplus6t-fajita running postmarketOS.

## First things, first.

This is a real good opportunity to back up your data. Try to keep that in mind while you are doing this tutorial.

## Download jumpdrive

Download jumpdrive from [here](https://github.com/dreemurrs-embedded/Jumpdrive/releases).

## Boot into fastboot.

Boot the phone into fastboot. On the Oneplus 6 hold the volume up and the power button at the same time. On the Oneplus6t hold both volume buttons and the power button at the same time. Once the phone has rebooted into fastboot, release the power button first, and then release the volume keys.

## Boot jumpdrive.

In the directory that you downloaded jumpdrive type in `sudo fastboot boot boot-oneplus-enchilada.img ` for oneplus6 enchilada, or `sudo fastboot boot  boot-oneplus-fajita.img` for oneplus6t fajita.

## Download qemu-aarch64-static from your package manager.

sudo apt install qemu-aarch64-static

sudo dnf install qemu-aarch-static

ETC

## The rest is easy...

Connect the phone and computer together with USB. The root partition will be the only partition available through USB. Unlock and mount it to your computer. 

**This is the perfect time to copy all the files that are important to you from your phone to your computer**

Run the following commands, replacing <PATH> with the full path to the pmOS mount. In my case `sudo chroot /mnt/pmOS_root qemu-aarch64-static <COMMAND>`

`sudo chroot <PATH> qemu-aarch64-static apk update`

`sudo chroot <PATH> qemu-aarch64-static apk upgrade`

`sudo chroot <PATH> qemu-aarch64-static apk fix`

## And that's it!

After doing some testing I found that when APK was doing the upgrade, installing a new kernel would fail, which is a good thing since /boot isn't mounted. I tried to figure out how to mount boot using jumpdrive, but I couldn't find a way to do so.