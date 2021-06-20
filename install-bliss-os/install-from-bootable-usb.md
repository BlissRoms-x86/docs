---
description: Install Bliss OS from bootable USB Installer
---

# Install From Bootable USB

## Introduction

**This is the current recommended method for beginners!**

We recommend beginners to use this method as it is the least error-prone and non-destructive. The following instructions were adapted from the Android-x86 project, so some of the images are from them. To look at Android-x86's original installation guide, [click here.](https://www.android-x86.org/installhowto.html)

## Download Bliss OS

You can download a stable Bliss OS build by clicking on the link [here](https://sourceforge.net/projects/blissos-x86/), non-stable builds can be found [here.](https://sourceforge.net/projects/blissos-x86/)

## Install Bliss OS

{% hint style="info" %}
If you are looking for a GUI based installer for Windows, we do include one in some of the .ISO's we produce, and we also support the [Supreme-Gamers Advanced Android-x86 Installer](https://supreme-gamers.com/r/advanced-android-x86-installer-for-windows.61/).
{% endhint %}

When booting into the installer, choose "Installation - Install Android-x86 to harddisk":

![Booting into the installer](../.gitbook/assets/booting-into-installer.png)

Once the installer boots, you will be asked to select the target drive. Choose the NTFS drive that houses your current Windows install. You do **not** need a separate partition, as the installer will create an image on your Windows partition.

![Choose partition](../.gitbook/assets/choose-partition.png)

Choose "Do not re-format" on the next screen. It is important that you choose "Do not re-format", as any other option will cause the installer to continue with the ["Bootable installation method"](install-from-bootable-usb.md#bootable-installation-method-mbruefiesp-3264-bit), which **will** result in **permanent data loss**, including your Windows partition!

![Do not reformat](../.gitbook/assets/do-not-reformat.png)

Choose "Yes" when prompted about the `GRUB` bootloader:

![Install GRUB](../.gitbook/assets/install-grub.png)

The installer will ask whether or not you want to make the system partition read/write-able. If you want to root your installation, you will choose "Yes" here. Otherwise, choose "No."

![Root installation](../.gitbook/assets/root-installation.png)

The installer will begin to write the changes to the disk. This will take some time. Go grab a coffee!

![Grab a coffee](../.gitbook/assets/grab-a-coffee.png)

Then the installer will ask you how much space you want to allocate for the data image. Most users choose 8 GB, 16 GB, or 32 GB.

Congratulations! You should now have a functional dual-boot with Bliss OS!

