# Manual Install on Linux

Create a directory at / as /blissos

1. Extract initrd.img, ramdisk.img, kernel and system.\* from your desired blissOS ISO into the /blissos directory.
2. Make a dir called /blissos/data \(Only valid for android-pie, for higher versions you need data.img, can be created with make\_ext4fs\)
3. Create a new grub entry with this the following code: 

`menuentry "BlissOS" { set SOURCE_NAME="blissos" search --set=root --file /$SOURCE_NAME/kernel   
     linux /$SOURCE_NAME/kernel quiet root=/dev/ram0 androidboot.hardware=android_x86_64androidboot.selinux=permissive acpi_sleep=s3_bios,s3_mode SRC=/$SOURCE_NAME   
     initrd /$SOURCE_NAME/initrd.img  
}`

