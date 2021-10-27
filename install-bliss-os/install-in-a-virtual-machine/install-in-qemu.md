---
layout: post
title: 'Bliss OS: How to install Bliss OS on Qemu'
date: '2021-10-27'
---
# Install with Qemu Script

This article is a guide to install `Project Sakura` on `qemu`. Bliss at the moment does not work. Gearlock causes resizefs errors that do not occur in the current Project Sakura build. As the current builds do not have Gearlock. There is a version of Bliss 11.13 that does have a working Gearlock in the BLISS OS telegram group.
  


## Background

Qemu while simple in use, can be complicated to understand for beginners. This is because Qemu has a LOT of options for customization, many of which are useful, but more so that are not. 

Qemu on windows as of time of writing this will NOT work on bliss 14. but may work on bliss 11. this is due to lack of software rendering on bliss 14. assuming Gearlock issues get fixed. in the mean time, use Project Sakura OS. 

If you wish to use this guide on windows 10 or above, It may be possible to use WSL, however to get Qemu KVM support on WSL you will need a custom kernel.


## Make the image.

When you have Qemu installed be it on Linux or windows, you will have installed a plethora of Qemu related tools. the tool we need for this is qemu-img, Navigate to the folder you want your image installed in and run the following command.

`qemu-img create -f qcow2 Bliss14.qcow2 20G`

  Breaking this down a little bit running
  `qemu-img create` is used to tell the program we want to create.

  `-f qcow2` is used to tell it we want to create the image using qcow2 format. qemu-img supports many formats so we want to specify this one.

  `Bliss14.qcow2` is the name of the image

  `20G` is the image size.

## Make a script to run the VM.

Below is a sample bash script used to run Bliss14 in a Qemu VM

 ```
 #!/bin/bash
 qemu-system-x86_64 \
 -enable-kvm \
 -M q35 \
 -m 4096 -smp 4 -cpu kvm64 \
 -bios /usr/share/ovmf/x64/OVMF.fd \
 -drive file=disks/bliss14-k54-gapps.qcow2,if=virtio \
 -cdrom images/Bliss-v11.iso \
 -usb \
 -device usb-tablet \
 -device usb-kbd \
 -device qemu-xhci,id=xhci \
 -machine vmport=off \
 -device virtio-vga-gl -display sdl,gl=on \
 -net nic,`model=virtio-net-pci` -net user,hostfwd=tcp::4444-:5555
 ```

While this looks a little complicated, when we break this down we can see that it isn't. we are just telling Qemu what we want our virtual machine to have attached to it. if you don
  
`qemu-system-x86_64` is used to launch the program, there are various other commands that can launch different versions of Qemu, but this is the one we want in the majority of cases.

`-enable-kvm` tells Qemu to use KVM hypervisor, this is how we get near native CPU performance from our VM

`-M q35` This tells Qemu what Machine type to run as, we need to specify this as `i440fx` will not work it is simple too old.

`-m 4096 -smp 4` tells Qemu How much ram to use, and how many cores to add the the VM

`-cpu host-passthrough` This tells Qemu what CPU it should be trying telling the guest it is. Generally it is the preferred option. However in case you start to get instability `-cpu kvm64` may be the option you need

 `-bios /usr/share/ovmf/x64/OVMF.fd \` is needed to tell Qemu to boot using UEFI, which is necessary as right now there is a bug that prevents android-generic based roms from being installed when in legacy mode.

`-drive file=disks/bliss14-k54-gapps.qcow2,if=virtio \` is how we mount the disk, Using this method instead of `-hda` lets us use virtio driver instead of emulated driver, giving us greater performance in the VM.

`-cdrom images/Bliss-v11.iso \` Just mount the iso for bliss, while we could use virtio driver for this, there is no real need to, because it's only use is installing the OS, this can be removed when the VM is installed

`-usb` is used to tell Qemu to add a USB controller, no real need to give it any additional arguments

`-device usb-tablet ` is one of two options for mouse capture the other being `-device usb-mouse` using Tablet will allow you to use Qemu as if it were any other app, using `usb-mouse` on the other hand will capture the mouse, and lock it to the VM. and to free it you will need to click `ctrl+alt` to free it.

`-device usb-kbd`  is for keyboard, no need to change anything here, 

`-device qemu-xhci,id=xhci` tells Qemu what USB version to use, no need to change this

`-machine vmport=off \` this option turns of vmware I/O emulation. this has caused me some bugs in the past, so I leave it off.

`-device virtio-vga-gl` this is needed for graphics acceleration, and while you can add a plethora of arguments, none are really beneficial at this time.

`-display sdl,gl=on` This is used for the display, there are three main options here `-display sdl,gl=on` `display gtk.sdl-on` and `-display spice-app,gl=on` The difference between these is personal preference. be warned that even if you close the VM's window with `-display spice-app,gl=on` the VM will still be running, you will need to kill it from the terminal.

`-net nic,model=virtio-net-pci -net user,hostfwd=tcp::4444-:5555` This really long command is the network command, `-net nic,model=virtio-net-pci` is what is used to add the network device to the guest in which case we are using virtio drivers again for best performance. `-net user` tells qemu how to pass the network through and the argument `hostfwd=tcp::4444-:5555` forwards port 4444 and port 5555 together, meaning if we open another terminal and type `adb connect localhost:4444` we can get an adb connection to the VM. 



## Install
The rest of the installation is the same as real hardware. You should be able to proceed as normal. 