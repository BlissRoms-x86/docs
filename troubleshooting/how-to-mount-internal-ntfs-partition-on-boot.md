# How to mount internal NTFS partition on boot

It is possible to automatically mount disks at boot. There may be other ways to do this, but one way to do it is:

1\) Write the commands for creating a folder and mounting the disk in a text file which will be a bash script file. 

`#!/system/bin/sh  
  
mkdir /mnt/folder_name mount.ntfs /dev/block/partition_name /mnt/folder_name`

Here 'partition\_name' could be any of the disk you want to mount like for example sda1 devb1 etc. and 'folder\_name' is the folder where you want to mount the disk.

2\) Place the script file under /system/etc/init.d. You can name the file anything you want, but it should not have any extension i.e. .txt or any other extension must be removed.

3\) Reboot and enjoy ! Use a file manager like Mixplorer which can bookmark the mounted locations for easy access. The disks should now be mounted automatically at every boot.

