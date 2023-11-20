# nexbox-a95x-armbian

A guide for installing [Armbian](https://www.armbian.com)
to [Nexbox A95X](https://androidtvbox.eu/nexbox-a95x-review-android-tv-box-powered-amlogic-s905/)

Based upon the
original [post](https://forum.armbian.com/topic/17106-installation-instructions-for-tv-boxes-with-amlogic-cpus/) of
SteeMan on the Armbian forums.

## Introduction

The Nexbox A95X is a small form factor TV box designed to run a modified Android operating system.
Its performance is quite good thanks to the small four core [Amlogic](https://en.wikipedia.org/wiki/Amlogic) SoC.
The video playback capabilities of the box are also good, it handles 1080p like a charm and only struggles a little bit
with 4K content.
Considering Android is based upon the Linux operating system, one can wonder how to install a full-fetched Linux system
onto the A95X. This guide aims to help you out with that.

## The hardware

* **CPU:** Amlogic S905 quad core ARM Cortex-A53 @ up to 2.0GHz
* **GPU:** penta-core Mali-450MP GPU
* **Memory:** 1 or 2 GB DDR3
* **Storage:** 8 or 16 GB NAND flash
* **Network:** 10/100M Ethernet, 802.11 b/g/n Wi-Fi
* **Ports:** 2xUSB 2.0, micro SD card slot, AV port, SPDIF port and an RJ45 slot

## So what will you need?

The first question is whether you want to use your A95X box as a server or as a more user-friendly computer with desktop
environment.

The most important tool of the project is a **toothpick**, so remember to grab one of that as well.

You will also need the power supply of the TV box, an HDMI cable, a TV or a monitor and a USB keyboard.

After you have decided which system is optimal for your needs, you need to grab an 8GB SD card (4GB is plenty if you
choose the server).
It is important that you select a **GOOD** and **RELIABLE** SD card.
Next, you have to select the
desired image from
this [link](https://armbian.systemonachip.net/dl/aml-s9xx-box/archive/).

There are two sets of images listed there. One
of the sets include the latest [Debian](https://www.debian.org/releases/) release-based armbian versions and the others
are based upon the latest [Ubuntu LTE](https://ubuntu.com/download/server)
version. Both of the sets of these images will have just server versions and other versions with some desktop
environment included. Like [Cinnamon](https://projects.linuxmint.com/cinnamon/) or [Xfce](https://www.xfce.org).

## Boot preparation

1. Select the desired image and download it. (Likely the image file will be zipped
   with xz. I recommend unzipping it.)
2. Next, you need to download an image flasher tool like [balenaEtcher](https://etcher.balena.io)
   or [Rufus](https://rufus.ie)
3. Grab your SD card of choice and flash the selected image onto it.
4. After the flashing has completed, you need to edit some files on the **BOOT** partition. (The partition name should
   be named BOOT, but it can be something similar.)
5. In the aforementioned partition there is a file under `extlinux/extlinux.conf`. Create a backup of
   the `extlinux.conf` file and replace the first few lines of `extlinux.conf` file to these lines:

   (Explanation: leave the last line starting with `append` in the file and replace everything else.)
   ```
   LABEL Armbian
   LINUX /Image
   INITRD /uInitrd

   FDT /dtb/amlogic/meson-gxl-s905x-p212.dtb
   
   append ...   
   ```
6. Now you need to go back to the **BOOT** partition and **COPY** the file, named `u-boot-s905x2-s912` to the same place
   with the
   name: `u-boot.ext`
7. That should cover the software part of the project.

## Booting up the box

* Plug the power supply jack out of the TV box.
* Plug in the HDMI and the keyboard.
* Put your SD card into the TF card slot of the TV box.
* This part will be tricky at first:
    1. Push the toothpick into the **AV** port of the box until you hear a click and hold it there. (Yep, the reset
       switch is located inside the AV port.)
    2. Plug in the power jack and let the box boot for a little.
    3. Count to seven and release the reset switch.
    4. Now the box should be booted into Armbian.

       (If it did not work, you can try holding the reset switch for different intervals.)

## Troubleshooting

I have only tested this on the A95X with 4 cores, 2GB ram and 16GB NAND memory. I have also included
the `extlinux.conf`, the `u-boot` and the `dtb` files that worked for me. You can download the exact image I used for my
TV
box [here](https://armbian.systemonachip.net/dl/aml-s9xx-box/archive/Armbian_23.8.1_Aml-s9xx-box_jammy_current_6.1.50.img.xz).
But here are some helpful tips:

* If the device just boots into the Android system and nothing happens, the most likely thing is that the SD card is
  broken **OR** the reset switch was not held for long enough, or it was held for too long, try finding the sweet spot.

* If the device keeps boot-looping or tries to boot one time and shuts off immediately, it is like due to a
  wrong `u-boot` file. Try out with different ones.

* If the device boots into the linux system, and you find it stuck at the `Loading Kernel...` prompt, or it is stuck
  whilst loading the kernel, you can try out different dtb files.

  Remember back to that `extlinux.conf` file. There is a line in that starting with the `FDT` keyword. After that, you
  will find a relative path to the dtb files on the partition.
  So here comes the tricky part. Try out different ones. Given enough trying, you will find the correct dtb file.

* If none of the above worked, try searching for solutions on
  the [Armbian Forums](https://forum.armbian.com/forum/88-amlogic-meson/).

## After booted

After the TV box booted you need to log in with these credentials:\
login name: `root`\
login password: `1234`

After that, the box asks you to create a new root password.
And followed by that comes the new user creation sequence.
Create your user there, or you can skip it by pressing `Ctrl`+`C`.

If you have installed a desktop environment, it should load after the creation of your user account.
Now you can use your box as a linux system.
All your work should be saved onto your SD card.

## How to install _truly_ install Armbian?

> **_WARNING:_** This operation cannot be undone. You will lose the old Android system on your box.

Log into your box with the root account and run the `install-aml.sh` script by issuing the following command:
`./install-aml.sh`.

This should install Armbian onto your TV box. After the script has finished, you can poweroff the system by
issuing: `poweroff`. Remove the SD card from the box and replug the powerjack into the box.

## Congratulations

Now you have a useful piece of hardware for literally anything.