# Windows 11
Try changing the type of your partition in Linux. For MBR partitions, this should be type 7 in fdisk. For GPT partitions, this should be type 6 in fdisk ("Microsoft basic data"), or 0700 in gdisk. We have to do some chicanery to get Linux partitions to appear in the first place, but unfortunately this confuses diskmgmt.msc too much.

List \\.\PHYSICALDRIVE<number>
```
wmic diskdrive list
```
wsl mount
```
wsl --mount \\.\PHYSICALDRIVE0 --bare
wsl lsblk -f # to detect /dev/sd<letter><?partition> or UUID /dev/disks/by-uuid/
wsl sudo mkdir /mnt/sdd && sudo mount /dev/sdd /mnt/sdd
# or even better
wsl sudo mkdir /mnt/uuid-uuid... && sudo mount /dev/disks/by-uuid/uuid-uuid... /mnt/uuid-uuid....

```



How do I change drive letter and mount points without using Disk Management? 
Using the mountvol tool from an elevated command prompt. 
Run mountvol to see a list of all volume ids available along with their drive letters or mount points attached. 
Commands to use with example volume id of \\?\Volume{00000000-0000-0000-0000-000000000000}:

Change drive letter to L: mountvol L: \\?\Volume{00000000-0000-0000-0000-000000000000}
Add mount point C:\emptyfolder: mountvol C:\emptyfolder \\?\Volume{00000000-0000-0000-0000-000000000000}
Remove drive letter L: mountvol L: /D
Remove mount point C:\emptyfolder: mountvol C:\emptyfolder /D
How do I format a partition as Btrfs?

Use the included command line program mkbtrfs.exe. We can't add Btrfs to Windows' own dialog box, unfortunately, as its list of filesystems has been hardcoded. You can also run format /fs:btrfs, if you don't need to set any Btrfs-specific options.

I can't reformat a mounted Btrfs filesystem
If Windows' Format dialog box refuses to appear, try running format.com with the /fs flag, e.g. format /fs:ntfs D:.


## WSL
Most time its the best to mount the drive in WSL 2 and then use the \\Linux ref to access it a minimal linux to archive that would be enough that runs simple a while true loop with 2mb ram
