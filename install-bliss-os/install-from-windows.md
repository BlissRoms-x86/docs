---
description: Install Bliss OS from Windows operating system
---

## Install Bliss OS from Windows

**This is the current recommended method for beginners!**

We recommend beginners to use this method as it is the least error-prone and non-destructive. The following instructions were adapted from the Android-x86 project, so some of the images are from them. To look at Android-x86's original installation guide, [click here.](https://www.android-x86.org/installhowto.html)

When booting into the installer, choose "Installation - Install Android-x86 to harddisk":

![beginner-installation-install-1](https://i.imgur.com/gEnnGFp.png)

Once the installer boots, you will be asked to select the target drive. Choose the NTFS drive that houses your current Windows install. You do **not** need a separate partition, as the installer will create an image on your Windows partition.

![beginner-installation-install-2](https://i.imgur.com/fpMo5GS.png)

Choose "Do not re-format" on the next screen. It is important that you choose "Do not re-format", as any other option will cause the installer to continue with the ["Bootable installation method"](#bootable-installation-method-mbruefiesp-3264-bit), which **will** result in **permanent data loss**, including your Windows partition!

![beginner-installation-install-3](https://i.imgur.com/QSDt8ia.png)

Choose "Yes" when prompted about the `grub` bootloader:

![beginner-installation-install-4](https://i.imgur.com/PfmjHHi.png)

The installer will ask whether or not you want to make the system partition read/write-able. If you want to root your installation, you will choose "Yes" here. Otherwise, choose "No."

![beginner-installation-install-5](https://i.imgur.com/SXEeevy.png)

The installer will begin to write the changes to the disk. This will take some time. Go grab a coffee!

![beginner-installation-install-6](https://i.imgur.com/iQ4tEAD.png)

Then the installer will ask you how much space you want to allocate for the data image. Most users choose 8 GB, 16 GB, or 32 GB.

Congratulations! You should now have a functional dual-boot with Bliss OS!
