# Advanced Installation

This is only for **advanced users**. For regular users, please visit our main [Installation Guide found here.](installation-guide.md)

## Windows-based installer - UEFI/ESP \(64-bit\)

This method is **no longer supported** due to too many people not understanding computer basics and breaking things. **Proceed at your own risk.** This method might be the easiest currently if you understand what you are doing.

For the overall instructions on using this method, please refer to the [tool's original thread](https://forum.xda-developers.com/android/software/winapp-android-x86-installer-uefi-t3222483). The tools have been updated by Team Bliss for easy installation on UEFI/ESP machines. The [builds we produce can be found here.](https://github.com/BlissRoms-x86/Androidx86-Installer-for-Windows/tree/master/bin) And the [source for those builds can be found here.](https://github.com/BlissRoms-x86/Androidx86-Installer-for-Windows) This tool should work on Remix OS as well, but this has not been tested yet.

### Part 1 - Using the Installer

The installer has been updated to accept the `.iso` files for our 8.x/10.x/11.x releases. Just follow the prompts the installer gives. Refer to the original thread for any questions, and please search before asking.

If you plan on using root, the process will require you to manually extract the system.img from within the system.sfs file. Then you must delete the system.sfs file after extracting.

**Warning** - for Pie, you will need to add `androidboot.hardware=android_x86_64` to the grub entry in order to boot!

### Part 2 - Switching the UEFI/EFI boot entry

Option one is to use the EasyUEFI tool, then switch the UEFI/EFI entry it created to boot first. Close and reboot. Option two is to use your BIOS to select the added UEFI boot entry.

## Use syslinux EFI to run Bliss OS 7.x/10.x/11.x

Thanks to @IcedCube for the original post! This method is **NOT recommended** as it is fairly bleeding-edge and experimental, but it should help booting on Chinese tablets that do not want to run `grub`.

Use a Linux installation for the following procedure.

### Part 1 - Grab the required tools

Install `unsquashfs` \(part of `squashfs-tools`\).

### Part 2 - Get Bliss OS

Grab the latest build of Bliss OS 7.x/10.x/11.x.

### Part 3 - Get the syslinux EFI bootstrap

[Grab the `.zip` file from @IcedCube's original post](https://forum.xda-developers.com/showpost.php?p=74977694&postcount=1237), and extract it to the root of the USB drive. This will bootstrap syslinux EFI onto it.

Then, make a folder called `android`.

Now, open up the `.iso` in an archive program. Extract the following files form the root directory of the `.iso` image to the USB drive's `android` folder:

* `initrd.img`
* `ramdisk.img`
* `kernel`

Extract `system.sfs` to a folder somewhere, such as `/tmp`.

Open a terminal and change directory \(using `cd`\) to `/tmp`. Run `ls` and confirm that `system.sfs` is shown in the file list. If there is no output, start over as the file is misplaced.

Run the following:

`unsquashfs ./system.sfs`

This will make a new directory called `squashfs_root`.

### Part 4 - Version specific

#### If you are using Bliss 7.x

Change directory to `squashfs_root` and run `ls`. There should only be one file - a `system.img` inside the directory. Copy the file to the USB's `android` folder.

### If you are using Bliss 10.x

Change directory to `squashfs_root`. The structure is a complete Android root filesystem. To install Bliss OS, the files will need to be in a system image. The following steps will guide you through creating a 2 GB `system.img` file, formatting it, mounting it, and copying the contents of `squashfs_root` into the new disk image.

Execute:

```text
mkdir /mnt/tempMount
truncate /tmp/system.img --size=2G
mkfs.ext4 -m0 /tmp/system.img
sudo mount -o loop /tmp/system.img /mnt/tempMount
sudo cp -prv /tmp/squashfs_root/* /mnt/tempMount/
sync
sudo umount /mnt/tempMount
```

The `sync` command might take some time.

Now copy the `/tmp/system.img` file to your USB's Android folder.

### Part 5 - Creating the data image

First, find where your USB drive is mounted. It is usually in `/mnt` or `/media` \(ex. `/media/USB`\).

`cd` into the `android` folder.

We will create a 3 GB data image file. You can attempt to create a 4 GB image but FAT32 maxes out at 4 GB per file. If your system supports exFAT or NTFS, you may try and use it.

```text
truncate data.img --size=3G
mkfs.ext4 -m0 data.img
sync
```

This will be an completely empty `ext4` disk image, but will be enough to run Bliss.

Finally, check to ensure everything is in structured like so:

```text
<ROOT>
- syslinux.cfg
- android/
-- kernel
-- system.img
-- data.img
-- ramdisk.img
-- initrd.img
- EFI/
-- BOOT/
--- bootia32.efi
--- bootx64.efi
--- ldlinux.e32
--- ldlinux.e64
```

Need to add some kernel parameters? Open `syslinux.cfg` and add them before the `initrd=/android/initrd.img` statement.

Unmount the USB from your computer. Plug it into your device and use the BIOS to boot from your UEFI USB Drive, partition 1. If all goes well, you will get a black screen with small white text saying "Booting Android..." followed by loading files. You should get the Linux kernel text, then see the Bliss boot animation play after a couple minutes depending on your USB drive read/write speed.

## Custom Install - Bliss OS 8.x/10.x/11.x UEFI/ESP \(64-bit\)

Just as a reminder, Team Bliss is **NOT** responsible for any damage caused by this guide. By continuing, you automatically agree to these terms.

### Part 1 - Mounting Your UEFI/ESP Partition

You will want to make sure you can view hidden and system files in Explorer options. Once you do that, hit the start menu, and type in `cmd`. Once "Command Prompt" shows up, right click on it and choose "Open as administrator".

#### `cmd` is not showing up, what should I do?

Press the Windows key and the R key to bring up the "Run..." dialog. Type in `cmd`, and then press Ctrl-Shift-Enter. Press "Yes" on the UAC popup.

Run the following:

```text
mountvol X: /S
```

Then check to see if it is mounted already. Run "Task Manager" by either

* Pressing Ctrl-Alt-Del and then clicking on "Task Manager", or
* Pressing Ctrl-Shift-Esc

Click on "File", "Run new task", "Browse", "This computer", and SYSTEM \(X or type in `X:` in the filepath bar. If you cannot access `X:`, then that could mean one of three things.

* You have an ESP setup \(follow the installation method below\)
* You have a legacy MBR setup
* Your setup has a custom boot sequence

### Part 1 \(alternate\) - ESP setup

Windows 10 sometimes has an EFI partition already mounted under drive letter `Z:`, hidden. A very quick and easy way to access the ESP \(EFI System Partition\) in Windows 10 without using the command line is to start "Task Manager" \(check above if you forgot the steps\), and then click on "File", "Run new task", "Browse", "This computer", and SYSTEM \(Z or type in `Z:` in the filepath bar\).

Now go to `boot/grub/grub.cfg` and edit it accordingly with Notepad++ or another text editor. Save the file and your're ready to go!

### Part 1 \(alternate\) - Killing the `explorer.exe`

Run `cmd` as admin and enter the following command:

```text
taskkill /im explorer.exe /f
```

This will kill the `explorer.exe` process - don't be surprised if it shows a warning. This step is sometimes required, because by default `explorer.exe` is ran by the currently logged in user, and it has to be run by the "Administrator" in order to view the mounted system drive. **The "Administrator" account is not the same as an account with administrative privileges.**

```text
mountvol X: /s
```

This will mount the system partition that usually consists of UEFI related files. `X:` is the letter of the drive - you can use whatever letter you want, but it has to be free for assignment. Then type:

```text
explorer
```

This will run `explorer` as "Administrator" and will allow you to browse the mounted system partition.

The above may not work for all devices, as some handle UEFI differently.

## Part 2 - UEFI installation

Let's start by downloading the required files. [Here is a customized UEFI boot for 32/64-bit machines.](https://www.androidfilehost.com/?w=files&flid=143191)

Please note that if you came from our Nougat builds to our Bliss OS 8.x builds, you will have to edit the `grub.cfga`.

If you are using Bliss OS 8.x/10.x, please use the `grub` entry below as a guide:

```text
menuentry 'Bliss-x86' --class android {
    search --file --no-floppy --set=root /AndroidOS/system.sfs
    linux /AndroidOS/kernel root=/dev/ram0 SRC=/AndroidOS androidboot.selinux=permissive androidboot.hardware=android_x86_64 quiet DATA=
    initrd /AndroidOS/initrd.img
}
```

If you are installing on `ext3`/`ext4`, due to a bug in the install you will have to use the following `grub` entry setup:

```text
menuentry 'Bliss-x86' --class android {
    search --file --no-floppy --set=root /AndroidOS/system.sfs
    linux /AndroidOS/kernel root=/dev/ram0 SRC=/AndroidOS  androidboot.selinux=permissive androidboot.hardware=android_x86_64 quiet DATA=
    initrd /AndroidOS/initrd.img
}
```

Now that we have the partition mounted, we can copy that `BOOT` directory to your UEFI partition using `explorer` as the Administrator or by using the "New Task" dialog from Task Manager. \(See above if you forgot the steps!\) Once it is copied, go back to the Administrator `cmd` prompt and type:

```text
mountvol X: /D
```

or if you used `Z:`, type:

```text
mountvol Z: /D
```

This will dismount the UEFI/ESP volume for safe reboot. We then suggest you use EasyUEFI here to create the UEFI boot entry. Open the app, and create a new entry. Select your UEFI partition, and in the "File" Path, click "Browse" and use the file manager window to browse to your `BOOT/grub/grubx64.efi` file. Click OK, and then choose the new `grub` entry and move it to the top. Make sure Secure Boot is turned off or else it likely will just boot back to Windows.

### Part 4 - The Manual Blissification of Your PC

To do a manual "Wubi like" install of Bliss OS after you install the UEFI entry, you will need to open the Bliss OS `.iso`/`.img` with 7zip, and then drag all the `.img` & `.sfs` files to `C:/android-x86` or whatever your target drive is \(make sure your `grub` entries match where you are putting these\). Then create your `data.img`. We suggest using a tool like RMXtools \(use version 1.7\) from XDA to create it. Check the tool's thread for detailed instructions. You will want to create your `data.img` inside that `android-x86` folder.

You can now reboot if you have installed the custom UEFI entry right and selected it using EasyUEFI. You should boot right to the Android-x86 `grub` theme. There, you can use up and down to select, and return to boot that entry. You can also hit `e` to edit the selected entry. You will want to pay attention to which entry you select, since there will be one for `Bliss-x86(32bit)` and one or `Bliss-x86_64(64bit)`.

